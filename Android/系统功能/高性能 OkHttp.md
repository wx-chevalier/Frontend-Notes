原文地址，[这里](http://omgitsmgp.com/2015/12/02/effective-okhttp/?utm_source=tuicool&utm_medium=referral)

[OkHttp](http://square.github.io/okhttp/) 是笔者在为为 [Khan Academy](https://www.khanacademy.org/)开发这个 [Android app](https://play.google.com/store/apps/details?id=org.khanacademy.android)时候的必用库之一。OkHttp 库本身已经提供了非常优秀的功能配置，但是下面是我们在实践中总结出来的一些能够有效增加资源利用率的步骤：

# 使用文件系统级别的响应缓存

默认情况下，OkHttp 并没有将包含`Cache-Control`头部文件的响应进行缓存。因此你的客户端可能会浪费时间与带宽去重复请求相同的资源，而不是简单地读取初次请求时候的缓存副本。为了保证基于文件系统的响应缓存，应该配置一个`com.squareup.okhttp.Cache`实例并且把它传递给`OkHttpClient`的`setCache`方法。在初始化该`Cache`时，需要指定一个`File`对象来指代文件目录以及最大的缓存容量。响应会被缓存写入指定的文件夹，如果缓存大小已经超过了文件夹的指定大小，会根据 LRU 规则来进行筛选。下面是笔者的具体实践：

```java
// Base directory recommended by http://stackoverflow.com/a/32752861/400717.
// Guard against null, which is possible according to
// https://groups.google.com/d/msg/android-developers/-694j87eXVU/YYs4b6kextwJ and
// http://stackoverflow.com/q/4441849/400717.
final @Nullable File baseDir = context.getCacheDir();
if (baseDir != null) {
  final File cacheDir = new File(baseDir, "HttpResponseCache");
  okHttpClient.setCache(new Cache(cacheDir, HTTP_RESPONSE_DISK_CACHE_MAX_SIZE));
}
```

其中，我们建议的`HTTP_RESPONSE_DISK_CACHE_MAX_SIZE` 值为`10 * 1024 * 1024`, 或者 10 MB。

# 使用 Stetho

[Stetho](http://facebook.github.io/stetho/) 是来自于 Facebook 的一个很有用的辅助库，能够允许开发者使用 [Chrome Developer Tools](https://developers.google.com/web/tools/setup/workspace/setup-devtools) 来监测 Android 应用。除了能够允许开发者监测 SQLite 数据库以及视图层级之外，Stetho 还能辅助监测所有的来自 OkHttp 的网络请求与响应。

![Image of Stetho](http://omgitsmgp.com/assets/images/posts/stetho-inspector-network.png)

这样的自检方法能够保证服务器返回恰当的带有缓存控制的响应，同时还能监测当缓存资源存在时是否确定没有请求被发出。如果要使用 Stetho，只需要简单地来添加一个`StethoInterceptor`实例到网络拦截器中：

```java
okHttpClient.networkInterceptors().add(new StethoInterceptor());
```

接下来，在应用运行之后，打开 Chrome 并且跳转到`chrome://inspect`界面，既可以看到上述画面。

# 与 Picasso 以及 Retrofit 混合使用

笔者是直接使用了 Picasso 进行网络图片的加载，以及使用 Retrofit 来简化请求构造以及响应解码。默认情况下，这两个库是使用了 OkHttpClient 进行底层的网络交互。以 Picasso 的 2.5.2 版本的`OkHttpDownloader`为例：

```java
private static OkHttpClient defaultOkHttpClient() {
  OkHttpClient client = new OkHttpClient();
  client.setConnectTimeout(Utils.DEFAULT_CONNECT_TIMEOUT_MILLIS, TimeUnit.MILLISECONDS);
  client.setReadTimeout(Utils.DEFAULT_READ_TIMEOUT_MILLIS, TimeUnit.MILLISECONDS);
  client.setWriteTimeout(Utils.DEFAULT_WRITE_TIMEOUT_MILLIS, TimeUnit.MILLISECONDS);
  return client;
}
```

Retrofit 有一个很类似的工厂方法。在应用中，图片往往是常用的大型资源之一，Picasso 本身维护了一个基于 LRU 规则的缓存系统，是直接在内存中的缓存。

```java
final Picasso picasso = new Picasso.Builder(context)
    .downloader(new OkHttpDownloader(okHttpClient))
    .build();

// The client should inject this instance whenever it is needed, but replace the singleton
// instance just in case.
Picasso.setSingletonInstance(picasso);
```

# 定制一个 User Agent 拦截器

对于由客户端提供的包含在 User-Agent 头文件中的详细的本机信息是往往用来日志记录与分析的重要来源。默认情况下，OkHttp 中自带的 User-Agent 属性会说明 OkHttp 的版本。如果需要定制自己的 User-Agent 信息，可以通过创建一个拦截器并且替换以下这些值：

```java
public final class UserAgentInterceptor implements Interceptor {
  private static final String USER_AGENT_HEADER_NAME = "User-Agent";
  private final String userAgentHeaderValue;

  public UserAgentInterceptor(String userAgentHeaderValue) {
    this.userAgentHeaderValue = Preconditions.checkNotNull(userAgentHeaderValue);
  }

  @Override
  public Response intercept(Chain chain) throws IOException {
    final Request originalRequest = chain.request();
    final Request requestWithUserAgent = originalRequest.newBuilder()
        .removeHeader(USER_AGENT_HEADER_NAME)
        .addHeader(USER_AGENT_HEADER_NAME, userAgentHeaderValue)
        .build();
    return chain.proceed(requestWithUserAgent);
  }
}
```

在构造传入拦截器的具体的 User-Agent 的值时，我们使用了如下信息：

* Android 的版本信息
* `Build.MODEL`, 或者用户可见的终端产品名
* `Build.BRAND`
* `Build.VERSION.SDK_INT`
* `BuildConfig.APPLICATION_ID`
* `BuildConfig.VERSION_NAME`
* `BuildConfig.VERSION_CODE`

最后三个值是 Gradle 文件中包含的版本信息。如果你是使用的 WebView，则可以直接在 WebView 中进行构造：

```java
WebSettings settings = webView.getSettings();
settings.setUserAgentString(userAgentHeaderValue);
```

# 明确合理的超时时间

Picasso specifies:

* A connect timeout of 15 seconds.
* A read timeout of 20 seconds.
* A write timeout of 20 seconds.

Whereas Retrofit specifies:

* A connect timeout of 15 seconds.
* A read timeout of 20 seconds.
* No write timeout.
