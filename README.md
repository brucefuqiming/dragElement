# dragElement
drag some element in html, using mousedown, mousemove, mouseup/在html中拖拽元素


包括：
（1）整体拖动
（2）指定元素拖动

```
<html>  
<head>  
    <meta charset="utf-8" />  
    <title></title>  
    <style>*{margin: 0;padding: 0;}</style>  
</head>  
<body>  
<div id="demo" style="position:absolute;width: 100px;height: 100px;background: #ccc;border:solid 1px #ccc;">
	<div id="demoheader" style="border-bottom: 1px dotted #fff;margin-bottom: 10px">头部</div>
	<div>头部拖拽</div>
	
</div>  

<div id="demo2" style="position:absolute; left: 50% ;top: 50%; width: 100px;height: 100px;background: #ccc;border:solid 1px #ccc;">
	<div style="border-bottom: 1px dotted #fff;margin-bottom: 10px">头部2</div>
	<div>内容2</div>
	<button id="demoheader2"  >点我拖拽</button>
</div>
<div id="demoheader3" style="position:absolute;right:20px;bottom:20px;width: 100px;height: 100px;background: #ccc;border:solid 1px #ccc;">
	<div style="border-bottom: 1px dotted #fff;margin-bottom: 10px">头部3</div>
	<div>整体拖拽</div>
</div>
</body>  
<script>  

/*
	dragId:触发拖拽的元素ID，如面板的头部；
	targetId：可选，拖拽的元素的ID，如整个面板，如果不指定，则未拖拽触发元素本身
*/
var dragElement = function(dragId, targetId) {
  var dragEle = document.getElementById(dragId),
      targetEle = targetId == undefined ? dragEle: document.getElementById(targetId)
  //用于确定是否是拖拽的对象  
  var drag;
  //鼠标位于目标元素上的时候距离目标元素的位置  
  var x, y;
  
  //鼠标按下去
  dragEle.onmousedown = function(evt) {
    //取得事件对象  
    _event = evt || window.event;
    //设置drag元素  
    target = _event.target || _event.srcElement;
    x = _event.clientX - targetEle.offsetLeft;
    y = _event.clientY - targetEle.offsetTop;
    drag = targetEle;

    /*确保鼠标移动和鼠标松开是放在dragEle.onmousedown里的，否则与onmousedown平级只能启用最后一次调用
	另外，通过将onmousemove和onmouseup绑定到document上启用拖拽，而不是绑定到dragEle上，避免鼠标滑动太快导致拖拽卡顿
	*/
    //鼠标移动  
    document.onmousemove = function(evt) {
      //确定鼠标是在目标元素上按下去后才开始移动  
      if (drag) {
	  
        _event = evt || window.event;
		
        //设置边界  
        var left = _event.clientX - x;
        var top = _event.clientY - y;
        body = document.documentElement || document.body;

        if (left < 0) left = 0;
        if (left > body.offsetWidth - drag.offsetWidth) left = body.offsetWidth - drag.offsetWidth;
		
        if (top < 0) top = 0;
        if (top > (body.offsetHeight - drag.offsetHeight)) top = (body.offsetHeight - drag.offsetHeight);
		

        //设置样式  
        drag.style.cursor = 'move';
        drag.style.border = 'dashed 1px red';
        drag.style.left = left + 'px';
        drag.style.top = top + 'px';
      }
    }

    //松开鼠标  
    document.onmouseup = function(evt) {
      if (drag) {
        //卸载样式  
        drag.style.cursor = '';
        drag.style.border = 'dashed 1px #ccc';
      }
      drag = null;
    }
  }
}

	
	dragElement("demoheader", "demo");
	dragElement("demoheader2", "demo2");
	dragElement("demoheader3");
	
	
</script>  
</html>  
```
