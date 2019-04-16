## 前端解决跨域的几种方法
  ### 1.Jsonp
  **说明**:
  Jsonp的原理是请求服务器返回的是回调函数名称包裹一层字符串的调用代码,字符串内容可以是任意格式，比如JSON内容，字符串等等。
  jsonp的核心则是动态添加script标签来调用服务器提供的js脚本
  客户端不再把远程js文件写死，而是编码实现动态查询;
  服务端检测jsonp和callback,返回JSON字符串或者字符串数据
  ***
  * 注意：只能用在get的请求方式
        客户端请求的url为重点jsonp?callback=xx,xx函数必须写在全局上
        服务端要检测jsonp和callback的存在,并且带数据的字符串返回给客户端
  ### 2.cors
  **说明**:
  CORS原理只需要向响应头header中设置Access-Control-Allow-Origin
          支持CORS浏览器IE浏览器不能低于IE10,其他都支持
  ***        
  * 注意：分为两种请求:简单请求和非简单请求
  简单请求的两种条件(都要符合):
  (1)请求方式:HEAD或GET或POST
  (2)Content-Type:application/x-www-form-u或multipart/form-data或text/plain
  非简单请求:PUT或DELETE或Content-Type-Type:application/json等
  在正式通信前增加预检请求(请求方法是OPTIONS),只有首次会预检,后面的不会再预检,总共2次请求
  * 携带cookie:要同时设置才有效果,设置了Access-Control-Allow-Origin要指定,不能为*
  服务器为Access-Control-Allow-Credentials: true;
  浏览器为xhr.withCredentials = true; 

  ### 3.nginx
  **说明**:nginx反向代理的本质其实就是接口服务的转发与header的处理
  前端利用host结合nginx实现跨域的运行流程：
  Brower》host》nginx》目标地址》服务器数据》nginx》Brower
  ***
  * 绑定本地host
  打开C:\Windows\System32\drivers\etc,加上 127.0.0.1 a.com
  * 安装nginx
   到[官网](https://nginx.org/en/download.html)下载后放到项目目录
   cmd-cd到nginx目录下,`start nginx.exe`, 
   打开nginx输入http://localhost,可以访问就说明成功安装了
   停止的命令:`nginx -s stop`
   重启的命令:`nginx -s reload`
  * 运行代理
   更改nginx的conf文件下的server配置,
   用node启动server.js服务器,`node server.js`
   最后用`live-server ./`  ,运行根目录的index,通过a.com运行访问
   