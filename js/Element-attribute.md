### Element.target

1. **[clientWidth](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/clientWidth)**：目标元素的width+padding(左右两侧)

2. **[offsetWidth](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetWidth)**：目标元素的width+padding(左右两侧)+border(左右两侧)

3. **[clientLeft](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/clientLeft)**：目标元素左边框border的宽度

4. **offsetLeft**：目标元素左边框离其具有定位的父元素之间的距离

5. **clientX**：鼠标相对于浏览器窗口可视区域的X坐标（横向）

6. **offsetX**：鼠标相对于绑定事件元素的X坐标

7. **pageX**：鼠标相对于文档的X坐标，会计算滚动距离；如果没有滚动距离，值与clientX一样

8. **screenX**：鼠标相对于显示器屏幕左侧的X坐标

9. **getBoundingClientRect().left**：目标元素左边框相对于浏览器可视区域的距离，可能为负值



#### [window.scrollY](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/scrollY)

描述：文档在垂直方向已滚动的像素值。

* **[window.pageYOffset](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/pageYOffset)** 是 **scollY** 属性的别名；

* 浏览器滚动条向下滚动了 100px，则window.scorllY  === 100;
* 浏览器滚动条向右滚动了 100px，则window.scorllX  === 100;



#### [Node.ownerDocument](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/ownerDocument)

描述：只读属性会返回当前节点的顶层的 document 对象。

* 得到当前元素的 HTML 节点，使用 **Node.ownerDocument.documentElement**



#### [Element.getBoundingClientRect()](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getBoundingClientRect)

描述：返回一个 [`DOMRect`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMRect) 对象；对象包含元素的大小和元素相对当前浏览器窗口的位置。



#### [Window.getComputedStyle()](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/getComputedStyle)

描述：获取元素的所有 CSS 属性的值； 返回一个对象，该对象包含元素的所有 CSS 属性的值。

包含参数：

* element —— 当前被获取属性对象的元素

* pseudoElt —— 指定一个要匹配的伪元素的字符串，普通元素可省略或为 **null**

  ```javascript
  const element = docuemnt.querySelector('.box');
  element.style.display = 'none';
  window.getComputedStyle(element)['display'] === 'none' // true
  ```

  

