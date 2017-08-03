# JavaScript

## 事件

### 1.事件流

事件冒泡（IE）：事件开始时由最具体的元素（文档中嵌套层次最深的解节点）接收，然后逐级向上传播到较为不具体的节点（文档）。

事件捕获（Netscape）：不具体的节点应该更早的接收到事件，而最具体的节点应该最后接收到事件。事件捕获的用意在于在事件到达预定目标之前捕获它。

DOM事件流（IE8及更早版本不支持）：
* 事件捕获阶段（为截获事件提供了机会）
* 处于目标阶段（实际目标接收到事件）
* 事件冒泡阶段（对事件做出响应）

### 2.事件处理程序（响应事件的函数）

#### HTML事件处理程序

demo1:

```html
<input type="button" value="click me" onclick="alert('clicked')"/>
```
demo2:

```html
<script type="text/javascript">
       function showMessage() {
           alert('hello world');
       }
</script>
<input type="button" value="click me" onclick="showMessage()"/>
```

#### DOM0级事件处理程序

```javascript
var btn = document.getElementById('mybtn');
btn.onclick = function () {
    alert('clicked');
}
```

#### DOM2级事件处理程序
addEventListener()、removeEventListener()：接收三个参数，要处理的事件名、作为事件处理程序的函数、一个布尔值（true表示在捕获阶段调用事件处理程序，false表示在冒泡阶段调用事件处理程序）。

```javascript
var btn = document.getElementById('mybtn');
btn.addEventListener('click',function () {
    alert('clicked');
})
btn.addEventListener('click',function () {
    alert('hello world');
})
//先输出clicked，再输出hello world
```

#### IE事件处理程序
attachEvent()、detachEvent()：接收两个参数，事件处理程序名称、事件处理程序函数。由于IE8以及更早版本只支持事件冒泡，所以通过attachEvent()添加的事件处理程序都会被添加到冒泡阶段。

```javascript
var btn = document.getElementById('mybtn');
btn.attachEvent('onclick',function () {
    alert('clicked');
})
btn.attachEvent('onclick',function () {
    alert('hello world');
})
//先输出hello world，再输出clicked。
```
#### 跨浏览器的事件处理程序

### 3.事件对象

#### DOM中的事件对象

```html
<input type="button" value="click me" onclick="alert('event.type')"/>
```

| 属性/方法                   | 类型         | 说明   |
| ---------------------------|:------------:|:-----|
| bubbles                    | Boolean      |表明事件是否冒泡 |
| cancelable                 | Boolean      |表明是否可以取消事件的默认行为|
| currentTarget              | Element      |事件处理程序当前正在处理事件的那个元素|
| defaultPrevented           | Boolean      |True表示已经调用了preventDefault()|
| detail                     | Integer      |与事件相关的细节信息|
| eventPhase                 | Integer      |调用事件程序的阶段：1表示捕获阶段，2表示‘处于目标’，3表示冒泡阶段|
| preventDefault()           | Function     |取消事件的默认行为。如果cancelable为true则可以使用|
| stopImmediatePropagation() | Function     |取消事件的进一步捕获或冒泡，同时阻止任何事件处理程序被调用（DOM3新增）|
| stopPropagation()          | Function     |取消事件的进一步捕获或冒泡。如果bubbles为true则可以使用|
| target                     | Element      |事件的目标|
| trusted                    | Boolean      |True表示事件是浏览器生成的。False表示事件是由开发人员通过JavaScript创建的（DOM3新增）|
| type                       | String       |被触发的事件类型|
| view                       | AbstractView |与事件相关的抽象视图。等同于发生事件的window对象|

在事件处理程序内部，对象this始终等于currentTarget的值，而target则只包含事件的实际目标。

#### IE中的事件对象