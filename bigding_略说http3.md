# 略说HTTP协议（三：首部字段）
> 本文章是读了《图解http》之后的第二篇记录，文章的内容在此书中都能够找到

前面讲了报文的基本格式和各种状态码，今天就再来继续讲讲HTTP首部字段。

##HTTP首部字段（以下简称首部字段）介绍
###首部字段的作用
首部字段是构成HTTP报文的要素之一，在服务器与客户端之间的HTTP通信过程中都会使用首部字段，他能起到传递额外重要信息的作用。

首部字段提供了报文主体大小，所使用的语言，认证信息等内容。

###首部字段的结构
格式为： 首部字段名：字段值。例如：Content-Type:text/html。并且，一个首部字段名可以对应多个字段值。

###四种首部字段的类型
通用首部字段：请求报文和响应报文都会是使用的首部。

请求首部字段：从客户端向服务器发送请求报文时使用的首部。补充了请求的附加内容，客户端信息，响应内容相关优先级等信息。

响应首部字段：从服务器端向客户端返回响应报文时使用的首部。补充了响应的附加内容，也会要求客户端添加额外的内容信息。

实体首部字段：针对请求报文和响应报文的实体部分使用的首部。补充了资源内容的更新时间等于实体有关的信息。

##HTTP通用首部字段
### Cache-Control:泳衣操作缓存的工作机制，有多个参数可选，指令之间通过逗号分开。

**Cache-Control指令一览表：**
缓存请求指令
<table cellspacing="0">
	<tr>
		<td><b>指令</b></td>
		<td><b>参数</b></td>
		<td><b>说明</b></td>
	</tr>
	<tr>
		<td>no-cache</td>
		<td>无</td>
		<td>强制向原服务器再次请求</td>
	</tr>
	<tr>
		<td>no-store</td>
		<td>无</td>
		<td>不缓存请求或响应的任何内容</td>
	</tr>
	<tr>
		<td>max-age=[秒]</td>
		<td>必需</td>
		<td>响应的最大Age值</td>
	</tr>
	<tr>
		<td>max-stale(=[秒])</td>
		<td>可省略</td>
		<td>接收已过期的响应</td>
	</tr>
	<tr>
		<td>min-fresh=[秒]</td>
		<td>必需</td>
		<td>期望指定时间内的响应仍有效</td>
	</tr>
	<tr>
		<td>no-transform</td>
		<td>无</td>
		<td>代理不可更改媒体类型</td>
	</tr>
	<tr>
		<td>only-if-cached</td>
		<td>无</td>
		<td>从缓存获取资源</td>
	</tr>
	<tr>
		<td>cache-ex-tension</td>
		<td>-</td>
		<td>新指令标记（token）</td>
	</tr>
</table>
缓存响应指令：
<table cellspacing="0">
	<tr>
		<td><b>指令</b></td>
		<td><b>参数</b></td>
		<td><b>说明</b></td>
	</tr>
	<tr>
		<td>public</td>
		<td>无</td>
		<td>可向任意方提供响应的缓存</td>
	</tr>
	<tr>
		<td>private</td>
		<td>可省略</td>
		<td>缓存钱必须先确认其有效性</td>
	</tr>
	<tr>
		<td>no-store</td>
		<td>无</td>
		<td>不缓存请求或响应的任何内容</td>
	</tr>
	<tr>
		<td>no-tramdform</td>
		<td>无</td>
		<td>代理不可更改媒体类型</td>
	</tr>
	<tr>
		<td>must-revalidate</td>
		<td>无</td>
		<td>可缓存但是必须再向原服务器进行确认</td>
	</tr>
	<tr>
		<td>proxy-revalidate</td>
		<td>无</td>
		<td>要求中间缓存服务器对缓存的响应有效性再进行确认</td>
	</tr>
	<tr>
		<td>max-age=[秒]</td>
		<td>必需</td>
		<td>响应的最大Age值</td>
	</tr>
	<tr>
		<td>s-maxage=[秒]</td>
		<td>必需</td>
		<td>g公共缓存服务器响应的最大Age值</td>
	</tr>
	<tr>
		<td>cache-extension</td>
		<td>-</td>
		<td>新指令标记（token）</td>
	</tr>
</table>	
###Connection：控制不在转发给代理的首部字段和管理持久连接
+ 控制不再转发给代理的首部字段 

	Connection：不再转发的首部字段名

+ 管理持久连接

	Connection：close 

"close"表示服务器想要断开持久链接

	Connection：Keep-Alive 

HTTP/1.1之前版本的HTTP协议是默认非持久连接的，所以在旧版本上想要维持持续连接，就要指定本字段(Keep-Alive)

###Date:表明创建HTTP报文的日期和时间
	Date:Tue,03 Jul 2016 04:40:59 GMT
###Pragma:HTTP/1.1之前的遗留字段，仅用于向后兼容，格式唯一
	Pragma:no-cache
通常写法为：

    Cache-Control:no-cache 
	Pragma:no-cache


###Trailer:Trailer会事先说明在报文主体后记录了哪些首部字段。该首部字段可应用于在HTTP/1.1版本分块传输编码时。
	HTTP/1.1 200 OK
	Date:tue,03 Jul 2016 04:40:58 GMT
	Content-Type:text/hrml
	...
	Transfer-Encoding:chunked
	Trailer:Expires
	...(报文主体)...
	0
	Expires:Tue,28 Sep 2014 23:59:58 GMT
可以看到报文首部字段中的Trialer的值为Expires，之后就在报文主体中出现了首部字段Expires

###Transfer-Encoding:规定传输报文主体时使用的编码方式
例如刚才Trailer示例中的

	Content-Type：text/html
###Upgrade：用于检测HTTP协议及其他协议是否可以使用更改版本进行通信，其参数值可以用来指定一个完全不同的通信协议
###Via:追踪客户端与服务器之间的请求和响应报文的传输路径
报文经过代理或网关时，会首先在首部字段Via中附加改服务器的信息，然后进行转发，次字段不仅可以用于追踪报文的转发，还可以避免请求会还的发生，所以必须在经过代理时附加该首部字段内容。、
###Warning：该首部中通常会含有一些告知用户缓存相关问题的警告
	Waring:[警告码][警告的主机：端口号]“[端口号]”([时间日期])
警告码在HTTP/1.1中定义了7种，如下表：
<table cellspacing="0">
	<tr>
		<td><b>警告码</b></td>
		<td><b>警告内容</b></td>
		<td><b>说明</b></td>
	</tr>
	<tr>
		<td>110</td>
		<td>Respionse is stale(响应已过期)</td>
		<td>代理返回已经过期的资源</td>
	</tr>
	<tr>
		<td>111</td>
		<td>Revalidation(再验证失败)</td>
		<td>代理无法再验证资源有效期时失败（服务器无法到达等原因）</td>
	</tr>
	<tr>
		<td>112</td>
		<td>Disconnection operator(断开连接操作)</td>
		<td>代理与互联网连接被故意切断</td>
	</tr>
	<tr>
		<td>113</td>
		<td>Heruristic expiration(试探性过期)</td>
		<td>响应的试用期超过24小时</td>
	</tr>
	<tr>
		<td>199</td>
		<td>Miscellaneous wearing(杂项警告)</td>
		<td>任意的警告内容</td>
	</tr>
	<tr>
		<td>214</td>
		<td><Transformation applied(使用了转换)</td>
		<td>代理对内容编码或媒体类型等执行了某些处理时</td>
	</tr>
	<tr>
		<td>299</td>
		<td>Miscellaneous persistent warning(持久杂项警告)</td>
		<td>任意警告内容</td>
	</tr>
</table>