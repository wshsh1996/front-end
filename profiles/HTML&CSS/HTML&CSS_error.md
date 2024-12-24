> 此文件主要留存日常代码书写过程中的 HTML&CSS 相关的报错与解决方案，部分方案参考自网络，如有侵权，请联系chinajnzhang@hotmail.com删除

### &lt;input type="number"&gt;maxlength失效
* 移动端页面为了方便用户交互通常将手机号输入框和数字输入框设置type属性，在用户触发fcous（）时手机键盘自动弹出数字键盘。
* 解决方案：修改为&lt;input
type="tel"&gt;可以起到同样的效果

### 解决图形验证码接口返回文件流图片
* 将图片按照相关格式转码即可
* `<img class="image-code" :src="'data:image/png;base64,'+imgCodeUrl"/>`

### 火狐浏览器去掉`type="number"的箭头`

```
input[type="number"] {
    -moz-appearance: textfield;
}
```

### 只能输入数字的正则表达式在火狐的兼容问题解决方法
* 加入了event.which  保证了火狐的兼容问题

```
<input alt="" class="" name='' type="text" onkeypress="return (/[\d.]/.test(String.fromCharCode(event.which||event.KeyCode)))"/>
```

### 设置两个DIV为display:inline-block出现上下错位问题
* 原因：
	* 同一行的行内元素对齐方式默认是底部对齐，即`vertical-align：baseline`
	* 对于内容为空的`inline-block`元素而言，该元素的基线就是它的`margin`底边缘，否则就是元素的内部最后一行内联元素的基线
* 解决方式：
	* `float`（考虑脱离流后后面元素不易控制，故不首选）
	* `vertical-align: top;`

### input textarea 实现placeholder换行
* 当我们在使用`<input type="textarea">`时，`placeholder`属性需要设置换行时可以使用unicode字符`&#10`进行处理
`<input type="textarea" rows="5" placeholder="测试换行符&#10;这是第二行的placeholder">`

### ios webview  flex-wrap兼容性问题
* IOS部分版本内嵌H5页面 `flex-wrap` 无法生效，会造成 `flex` 盒子失效

### input type=tel & type=number

### `pointer-events: none` 鼠标穿透
* 属性：
	* `auto` : 效果和没有定义pointer-events属性相同，鼠标不会穿透当前层。在SVG中，该值和visiblePainted的效果相同。
	* `none` : 元素不再是鼠标事件的目标，鼠标不再监听当前层而去监听下面的层中的元素。
* 应用场景：
	* 不需要鼠标点击的展示元素
* 注意：
	* 部分默认可点击的标签，如 `<a>` 标签，添加样式后不可点击，但是使用 `tab` 键还是可以选中点击的，去掉 `<a>` 标签的 `href` 属性可以解决这一问题

### Safari浏览器 、微信浏览器下拉回弹效果
* 解决方式：

```
// HTML
<body class="box">
    <div class="scroll" style="height:1500px">  
    </div>
</body>

// JS
let overscroll = function(el) {
    el.addEventListener('touchstart', function() {
        var top = el.scrollTop, totalScroll = el.scrollHeight, currentScroll = top + el.offsetHeight;
        if(top === 0) {
            el.scrollTop = 1;
        }else if(currentScroll === totalScroll) {
            el.scrollTop = top - 1;
        }
    });

    el.addEventListener('touchmove', function(evt) {
    if(el.offsetHeight < el.scrollHeight)
        evt._isScroller = true;
    });
}
overscroll(document.querySelector('.scroll'));
document.body.addEventListener('touchmove', function(evt) {
    if(!evt._isScroller) {
        evt.preventDefault();
    }
});

// CSS
.box{
  overflow: auto;
  -webkit-overflow-scrolling: touch;
}
```
* 还有一串神秘代码

```
// 监听滑动事件，如果是非滑动组件就禁止滑动
document.body.addEventListener("touchmove", function(e) {
	if(e._isScroller) return
	e.preventDefault()
}, {
	passive:false
})
```