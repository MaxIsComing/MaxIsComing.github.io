---
layout: post
title: 两天面试下来的感受
categories: [笔记]
tags: [笔记]
description: write the code, change the world!
---
首先，layoutSubviews和drawRect都是异步执行。layoutSubViews方便数据计算,drawRect方便视图重绘。<br/><br/>
layoutSubviews在下面情况下会被调用：<br/>
1、init初始化时候不会触发layoutSubviews。<br/>
2、addSubviews的时候会触发layoutSubviews。<br/>
3、设置view的frame的时候会触发layoutSubviews，前提是frame的值设置前后发生了变化。<br/>
4、滚动一个scrollView的时候会触发layoutSubviews。<br/>
5、旋转屏幕的时候会触发父view上的layoutSubviews。<br/>
6、改变一个view的大小也会触发父view的layoutSubviews。<br/>
7、直接调用setLayoutSubviews。<br/>
<br/>
<br/>
drawRect在以下情况会被调用：<br/>
1、如果初始化的时候没有设置rect大小，将直接导致drawRect：不会被调用。drawRect：调用是在Controller->loadView，Controller->viewDidLoad两个方法之后调用的。<br/>
2、在sizeToFit后将会被调用，比如label就可以事先通过sizeToFit计算出size，然后调用drawRect：方法。<br/>
3、通过设置contentMode属性为UIViewContentModeRedraw，那么每次设置或者更改frame的时候自动调用drawRect：。<br/>
4、直接调用setNeedsDisplay或者setNeedsDisplayInRect:，但是rect不能为0。<br/>
<br/>
<br/>
drawRect:使用注意点<br/>
1、若使用UIView绘图，只能在drawRect：方法中获取contextRef进行绘图。其他地方获取到得Hi一个invalidate ref，是不能用于画图的。drawRect：方法不能手动调用，必须通过setNeedsDisplay或者setNeedsDisplayInRect让系统自动调用。<br/>
2、如果使用Calayer绘图则只能在drawInContext：中绘制或者delegate中绘制，同样也是调用setNeedDispaly等间接调用drawRect：方法。<br/>
3、如果要实时画图，不能用gesture，只能用touchBegan等方法来调用setNeedsDisplay。<br/>

