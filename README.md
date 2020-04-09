# SlidingWindowScorllComponent
SlidingWindowScorll  with VUE
### 15个元素实现无限滚动列表
VUE实现思路如下，在mounted阶段，设置intersectionObserver监听末尾item。当滑动到最后一个Item。从头删除Num个item。末尾添加Num个item。在update阶段，监听top和bottom 的Item。注意设置标志位，否则删除item时，update函数被触发。  
实现无线滚动列表，可以懒加载图片。Item更新后的占位由transform实现。
### npm run dev 开启热更新调试
