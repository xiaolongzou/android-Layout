# Android 6大基本布局

### LinearLayout

  常见属性：
  
    orientation: 排列方式
    
    gravity：控制组件所包含的子元素的对齐方式。可多个组合比如center|bottom
    
    layout_gravity: 控制改组件在父容器的对齐方式
    
    background：背景图片/颜色
    
    divider：分割线，showDividers才显示
    
    showDividers：设置分割线所在的位置，none，beginning，middle，end
    
    dividerPadding：设置分割线的padding
    
    layout_weight: 权重，用来等比划划分区域的 本质上是将剩余空间进行分配
    
    layout_weight 栗子：
    
        3个子元素
        
        layout_height为match_parent
        
        layout_weight为2 1 1
        
        剩余空间 1 - 3 = -2
        
        1 - 2 * 2/4 = 0
        
        1 - 2 * 1/4 = 1/2
        
        1 - 2 * 1/4 = 1/2
        
        实际使用中通过layout_weight去等比划分 一般将layout_height设置为0（orientation：vertical）

### RelativeLayout

所有的子元素位置初始都是相对于原点

常见属性：

     根据父容器定位
     
     1.layout_alignParentLeft 左对齐
     
     2.layout_alignParentRight 右对齐
     
     3.layout_alignParentTop 顶部对齐
     
     4.layout_alignParentBottom 底部对齐
     
     5.layout_centerHorizontal 水平居中
     
     6.layout_centerVertical 垂直居中
     
     7.layout_centerInParent 中间位置

     根据兄弟组件定位
     
     1.layout_toLeftOf 放置于参考组件左边
     
     2.layout_toRightOf 放置于参考组件右边
     
     3.layout_above 放置于参考组件上边
     
     4.layout_below 放置于参考组件下边
     
     5.layout_alignTop 对齐参考组件的上边界
     
     6.layout_aligntBottom 对齐参考组件的下边界
     
     7.layout_alignLeft 对齐参考组件的左边界
     
     8.layout_alignRight 对齐参考组件的右边界

     margin：设置组件与父容器的间距

     padding：设置组件内元素与父容器的间距
     
### FrameLayout

