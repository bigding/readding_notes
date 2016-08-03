# 略说HTTP协议（二：报文格式和状态码）
> 本文章是读了《图解http》之后的第二篇记录，文章的内容在此书中都能够找到

## HTTP报文
### 什么是HTTP报文
报文是指HTTP协议交互的信息。请求端（客户端）的报文称之为请求报文，响应端（服务器端）报文称之为响应报文。

HTTP报文大致可分为报文首部和报文主体两部分，由空行划分，通常，不一定有报文主体。

## 报文的结构
> 为了简便就直接用表格表示了（表格中的例子是请求报文）

<table cellspacing="0">
	<tr>
		<td>报文首部</td>
		<td>
			(请求报文例子)<br>
			POST /search HTTP/1.1<br>
			Accept: image/gif, image/x-xbitmap, image/jpeg, image/pjpeg, application/vnd.ms-excel, application/vnd.ms-powerpoint, 
			application/msword, application/x-silverlight, application/x-shockwave-flash, */*  <br>
			Referer: <a href="http://www.google.cn/">http://www.google.cn/</a>  
			Accept-Language: zh-cn  <br>
			Accept-Encoding: gzip, deflate  
			User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; TheWorld)  <br>
			Host: <a href="http://www.google.cn">www.google.cn</a>  <br>
			Connection: Keep-Alive  
		</td>
		<td>
			(响应报文例子)<br>
			HTTP/1.1 200 OK<br>
			Date: Sat, 31 Dec 2005 23:59:59 GMT<br>
			Content-Type: text/html;charset=ISO-8859-1<br>
			Content-Length: 122
		</td>
	</tr>
	<tr>
		<td>空行(CR+LF)</td>
		<td>空行（CR+LF）</td>
		<td>空行（CR+LF）</td>
	</tr>
	<tr>
		<td>报文主体</td>
		<td>无</td>
		<td>
			＜html＞<br>
			＜head＞<br/>
			＜title＞Wrox Homepage＜/title＞<br>
			＜/head＞<br>
			＜body＞<br>
				...<br>
			＜/body＞<br>
			＜/html＞
		</td>
	</tr>
</table>
**报文首部**

请求报文和响应报文稍有不同之处，就首部而言，对比如下图：

<table cellspacing="0">
	<tr>
		<td><b>请求报文首部</b></td>
		<td><b>响应报文首部 </b></td>	
	</tr>
	<tr>
		<td>请求行</td>
		<td>状态行</td>
	</tr>
	<tr>
		<td>请求首部字段</td>
		<td>响应首部字段</td>
	</tr>
	<tr>
		<td>通用首部字段</td>
		<td>通用首部字段</td>
	</tr>
	<tr>
		<td>实体首部字段</td>
		<td>实体首部字段</td>
	</tr>
	<tr>
		<td>其他</td>
		<td>其他</td>
	</tr>
</table>

##状态行的各种状态码
什么是状态码呢？上面响应报文例子中第一样的200就是状态码，而后面的OK就是原因短语了。下面我们就来细细的说说状态码是怎样的。

大家一定觉得各种状态码很难记，好复杂，什么404,500的难记死了，其实状态码是规律可循的，并在这里列出主要的14个状态码。
<table cellspacing="0">
	<tr>
		<td></td>
		<td><b>类别</b></td>
		<td><b>原因短语</b></td>
	</tr>
	<tr>
		<td>1XX</td>
		<td>Information(状态性状态码)</td>
		<td>接受的请求正在处理</td>
	</tr>
	<tr>
		<td>2XX</td>
		<td>Success(成功状态码)</td>
		<td>请求正常处理完毕</td>
	</tr>
	<tr>
		<td>3XX</td>
		<td>Redirection(重定向状态码)</td>
		<td>需要进行附加操作以完成请求</td>
	</tr>
	<tr>
		<td>4XX</td>
		<td>Client Error(客户端错误状态码)</td>
		<td>服务器无法处理请求</td>
	</tr>
	<tr>
		<td>5XX</td>
		<td>Server error(服务器错误状态码)</td>
		<td>服务器处理请求错误</td>
	</tr>
</table>

###2XX成功
+ 200 OK:表示客户端发来的请求在服务器端被正常处理了。
+ 204 No Content：表示该客户端发来的请求已经正常处理，但是返回的响应报文中不含实体的主体部分
+ 206 Partial Content:该状态码表示客户端进行了范围请求，而服务器成功执行了这部分的GET请求。

###3XX重定向
+ 301 Moved Permanently：永久性重定向。表示请求的资源已经被分配了新的URL，以后应使用现在资源指向的URL
+ 302 Found：临时性重定向。表示请求的资源已被分配了新的URL，希望用户本次能使用新的URL访问。和301的不同是301是永久重定向，而301是临时重定向，只重定向一次。302在协议中是不允许将POST变换成GET的，但是在实际使用中，大家并不会遵守。甚至很多浏览器将302响应视为303响应。
+ 303 See Other：表示由于	请求对应资源存在着另一个URL,应使用GET方法定向获取请求的资源。303和302状态码功能相同，但是303表示客户端应当以GET方式获取资源。
+ 304 Not Modified:表示服务器的资源没有改变，可以直接使用客户端没有过期的缓存。
+ 307 Temporary Redirect：临时重定向，与302 Found含义相同。307禁止将POST变换成GET，这点事302 Found没有做到的。
###4XX客户端错误
+ 400 Bad Request：请求错误，表示请求报文中存在语法错误。当错误发生时，需要修改请求的内容后再次发送请求。
+ 401 Unauthorized：发送的请求需要有通过HTTP认证。当第一次请求时要求在页面中进行认证，而第二次请求时就会返回用户认证失败。
+ 403 Forbidden：表示请求资源的访问被服务器拒绝了。可能原因是未获取文件系统的访问权限，访问权限出现问题等。
+ 404 Not Found：服务器无法找到请求的资源，或者服务器拒绝请求并且不想说明理由。
###5XX服务器错误
+ 500 Internal Server Error:服务执行时发生了错误
+ 503 Service Unavailable：服务器暂时处于超负荷状态或者正在进行停机维护，现在无法处理请求。
###小结
以上的状态码只是最常用的一部分，就已经很难记了，重要的是多用多熟悉。再者，常见到的也就那个几个而已，多用用就记住了。