# TableGrid

## UICollectionView

> 参考资料
>
> * \[UICollectionView详解\]\[9\]
> * [Creating Custom Collection View Cells in iOS7](http://blog.csdn.net/x359981514/article/category/1266042)
> * [An iPhone iOS 6 Storyboard-based Collection View Tutorial](http://www.techotopia.com/index.php/An_iPhone_iOS_6_Storyboard-based_Collection_View_Tutorial)
> * [UICollectionView Tutorial By Swift](http://www.raywenderlich.com/78550/beginning-ios-collection-views-swift-part-1)

### UICollectionViewCell：自定义样式

#### Auto Size

```text
- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout sizeForItemAtIndexPath:(NSIndexPath *)indexPath {
    return CGSizeMake(100, 100);
}
```

## UIStackView

1. UICollectionView is like a grid, UIStackView is only for 1 dimension: vertical or horizontal.

UICollectionView is like UITableView, but it supports more than single-column layouts.

> Collection views provide the same general function as table views except that a collection view is able to support more than just single-column layouts. Collection views support customizable layouts that can be used to implement multi-column grids, tiled layouts, circular layouts, and many more. You can even change the layout of a collection view dynamically if you want.

vs

> The UIStackView class provides a streamlined interface for laying out a collection of views in either a column or a row

1. For me, With StackView, you benefit the "AutoLayout" feature, for example: you put 4 views in the Stack, this component will decide how those views will be presented on the screen, depending on their size.

> * [iOS 9: UIStackView入门](http://www.cocoachina.com/ios/20150623/12233.html)
> * [uistackview-tutorial-introducing-stack-views](http://www.raywenderlich.com/114552/uistackview-tutorial-introducing-stack-views)

## UIStackView-Extension

### [FDStackView](https://github.com/forkingdog/FDStackView)

直接在Podfile中加入FDStackView，即可以无缝使用StackView及其N多的功能。

## CardView

### [Koloda](https://github.com/Yalantis/Koloda)

![Preview](https://github.com/Yalantis/Koloda/raw/master/Koloda_v2_example_animation.gif)

\[9\]: [http://www.cnblogs.com/ios8/p/iOS-UICollectionView.html](http://www.cnblogs.com/ios8/p/iOS-UICollectionView.html)

