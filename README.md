#### 前端面试
***
##### 1、浏览内核有哪些？
1. ie-->trident
2. firefix-->gecko
3. safari-->webkit
4.chrome-->blink
##### 2、浏览器是如何渲染页面的？
1.解析生成dom tree
2.解析生成 css rule tree
3.将 dom tree与css rule tree 合成 render tree
4.layout 计算每一个节点的位置
5.painting 开始渲染
6.reflow 当位置发生变化，回流
7.repaint 重绘，只改变内部样式 backgroun-color

##### 3、何优化首屏渲染性能？

##### 4、jsonp 工作原理？
