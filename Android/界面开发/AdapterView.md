


# Custom AdapterView
``` java
public class MyAdapterView extends AdapterView<ImageAdapter> implements AdapterView.OnItemClickListener{    
  private ImageAdapter mAdapter;    
  public MyAdapterView(Context context, 
AttributeSet attrs, int defStyle) 
  {        
    super(context, attrs, defStyle);        
    initThings();    
  }    
@Override    
public ImageAdapter getAdapter() 
{        
// TODO Auto-generated method stub        
return mAdapter;    
}    
@Override    
public void setAdapter(ImageAdapter adapter) {        
// TODO Auto-generated method stub        
requestLayout();    
}    
View obtainView(int position) {        
View child = mAdapter.getView(position, null, this);          
return child;    
}    
@Override    
protected void onLayout(boolean changed, int l, int t, int r, int b) 
{     
super.onLayout(changed, l, t, r, b);        
for(int i=0;i<mAdapter.getCount();i++){
            View child = obtainView(i);
            child.layout(10, 70*i, 70, 70);
            addViewInLayout(child, i, null, true);        }        this.invalidate();    }    @Override    public void onItemClick(AdapterView<?> parent, View v, int position, long id) {        Log.d("MYEXAMPLES","Clicked an item!");    }}
```
## Event
### Click
``` java
public boolean performItemClick(View view, 
int position, long id) 
{    
if (mOnItemClickListener != null) 
{        
// ...        
mOnItemClickListener.onItemClick(this, view, position, id);        
return true;    
}    
    return false;
}
```
































