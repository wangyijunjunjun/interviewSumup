# interviewSumup



1)跨域问题

ajax或者iframe指向的地址中，二级域名、端口、协议必须与主页面完全相同，否则就算跨域
比如
a.baidu.com访问b.baidu.com  是跨域；
a.baidu.com:8080访问a.baidu.com:80 是跨域；
http://a.baidu.com访问https://a.baidu.com 是跨域


ajax跨域，两种办法：后端写个代理接口，让后端去抓数据；或者与对方合作，用jsonp等方式传送数据


说白了就是XmlHttpRequest太弱,只允许请求当前源（域名、协议、端口）的资源


js代码题:
//深度克隆:

