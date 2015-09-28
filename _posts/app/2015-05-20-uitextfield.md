---
layout: post
title: UITextField键盘遮挡问题 
categories: [app]
key: 1437804900
---

## UITextField键盘遮挡问题的解决

----------------------------------------
**今天在实现一个页面的时候遇到这个问题，所以就记录下这个知识点。
网上的一些博客的解决方案是将整个view上移，但是我遇到的问题是需要将处于view中部的tableview上移，然后将tableview滚动到特定的位置。**


### 1.如何获得键盘的高度？

获得键盘的高度信息，需要监听键盘弹出事件。

```
- (void)addKeyboardObserver
{
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(onKeyboardWillShow:) name:UIKeyboardWillShowNotification object:nil];
    
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(onKeyboardWillHide:) name:UIKeyboardWillHideNotification object:nil];
    
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(onKeyboardDidShow:) name:UIKeyboardDidShowNotification object:nil];
}
```

当键盘将要显示出来时，触发onKeyboardWillShow方法。在这个方法中我们可以通过

```
NSDictionary* userInfo = [aNotification userInfo];
```
得到键盘的信息。

```
Printing description of userInfo:
{
    UIKeyboardAnimationCurveUserInfoKey = 7;
    UIKeyboardAnimationDurationUserInfoKey = "0.25";
    UIKeyboardBoundsUserInfoKey = "NSRect: {{0, 0}, {320, 216}}";
    UIKeyboardCenterBeginUserInfoKey = "NSPoint: {160, 676}";
    UIKeyboardCenterEndUserInfoKey = "NSPoint: {160, 460}";
    UIKeyboardFrameBeginUserInfoKey = "NSRect: {{0, 568}, {320, 216}}";
    UIKeyboardFrameChangedByUserInteraction = 0;
    UIKeyboardFrameEndUserInfoKey = "NSRect: {{0, 352}, {320, 216}}";
}
```
可以看出，UIKeyboardFrameBeginUserInfoKey表示的是键盘的初始位置，UIKeyboardCenterEndUserInfoKey表示键盘在动画结束时的最终显示位置。**216即为键盘的高度**

### 2.上移tableview

这个就不详述了，也就是改变下frame。上代码吧~

```
CGRect newFrame = _tableView.frame;
newFrame.size.height = height-kHeadViewHeight;
_tableView.frame = newFrame;
```

### 3.设置tableview偏移

```
CGRect frame = _tableView.frame;
CGPoint contentOffsetPoint = _tableView.contentOffset;
if ( (contentOffsetPoint.y == _tableView.contentSize.height - frame.size.height) ||
    (_tableView.contentSize.height < frame.size.height) ) {
    CGPoint newOffset;
    newOffset.x = 0;
    newOffset.y = _tableView.contentSize.height - newFrame.size.height;
    
    if (newOffset.y < 0) {
        newOffset.y = 0;
    }
    
    _tableView.contentOffset = newOffset;
}

```

### 4.如何恢复？

这个也不用多说了。






