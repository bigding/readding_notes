#浏览器中各种宽度，高度
##浏览器可点击区域
	var h=window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight;
##html页面高度
	var h = document.body.scrollHeight|document.documentElement.scrollHeight
##滚动条高度
    var Yoffset=window.pageYOffset || document.documentElement.scrollTop;