# interviewSumup

理论题:

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

function clone(Obj) {
    var buf;

    //讨论:数组,对象,两者都不是

    if (Obj instanceof Array) {
        buf = [];

        for (var i = 0; i < Obj.length; i++) {
            buf[i] = clone(Obj[i]);
        }

        return buf;
    } else if (Obj instanceof Object) {
        buf = {};
        for ( var k in Obj){
            buf[k] = clone(Obj[k]);
        }

        return buf;
    } else {
        return Obj;
    }
}


//设计模式

//1)单例 :任意对象都是单例

var obj = {name : "nex" , age : 33};

//2)工厂 :使用原型prototype
    function Person() {
        this.name = "nexl"
    }
    function Animation() {
        this.name = "mortonl"
    }
    
    function Factory() {
    }
    
    Factory.prototype.getInstance = function (className) {
        return eval('new' + className + '()');
    }
    
    var factory = new Factory();
    var obj01 = factory.getInstance('Person');
    var obj02 = factory.getInstance('Animation');


//关于eval:eval() 函数可计算某个字符串，并执行其中的的 JavaScript 代码。


//3)代理 :新建个代理类包一下老类的接口



//4) 观察者

    function Publisher(){
        this.listenrs = [];
    }
    
    Publisher.prototype = {
        'addListener':function(listener){
            this.listenrs.push(listener);
        }
    ,
        'removeListener':function(listener){
                delete this.listenrs[listener];
        }
        ,
        'notify':function(obj){
            for (var i = 0 ; i <this.listenrs.length ; i++){
                var listener = this.listenrs[i];
                if (typeof listener !== 'undefined'){
                    listener.process(obj);
                }
            }
        }
    
    };
    
    
    function Subscriber(){
    }
    
    Subscriber.prototype = {
        'process' : function(obj){
            console.log(obj);
        }
    };
    
    
    var publisher = new Publisher();
    publisher.addListener(new Subscriber());
    publisher.addListener(new Subscriber());
    publisher.notify({name : 'michaelqin',ageo:30});
    publisher.notify('xxx');




JS笔试题:

//自定义回调


    function testCallback(callback) {  
        alert('come in!');  
        callback();  
    }  
      
    /** 
     * 被回调的函数 
     */  
    function a() {  
        alert('a');  
    }  
      
    /** 
     * 开始测试方法 
     */  
    function start() {  
        testCallback(a);  
    }  
    
    
    或者
    function testCallback(callback(a,b)){
        callback("aaa","bbb");
    }
    
    function c(a,b){
        alert(a+b);
    }
    
    function test(){
        testCallback(c(a,b));
    }
    
    
 

考察this

JavaScript

var length = 10;
function fn() {
  console.log(this.length);
}

var obj = {
  length: 5,
  method: function(fn) {
    fn();
    arguments[0]();
  }
};

obj.method(fn, 1);

var length = 10;
function fn() {
  console.log(this.length);
}
 
var obj = {
  length: 5,
  method: function(fn) {
    fn();
    arguments[0]();
  }
};
 
obj.method(fn, 1);
输出：10 2

第一次输出10应该没有问题。我们知道取对象属于除了点操作符还可以用中括号，所以第二次执行时相当于arguments调用方法，this指向arguments，而这里传了两个参数，故输出arguments长度为2。



图片懒加载:

当页面滚动的距离大于图片距顶端的距离与窗口可视高度


    简单的设置下图片的样式:
    <style type="text/css">
        img{
            width: 600px;
            height: 400px;
            background-color: #CCCCCC;
            position: absolute;
            top: 1000px;
        }
    </style>
    注意图片地址要写在 data-src 里
    <body>
            <img src="" data-src = "http://img2.zol.com.cn/product/98_940x705/577/ceDYOL5WHbbM.jpg"/>
    </body>
    js部分:
    <script type="text/javascript">
            var img = document.querySelector("img");
            // 图片距顶端距离
            var t = img.offsetTop;
            // 窗口可视高度
            var  h = document.documentElement.clientHeight;
            // 显示图片所需要的最小自动距离
            var distance = t - h;
            // 滑动触发
            window.onscroll = function () {
                // 滑动时 顶端距离
                var scrollTop = document.body.scrollTop || window.pageYOffset ||document.documentElement.scrollTop;
                // 懒加载
                if (scrollTop >= distance) {
                    var imPath = img.getAttribute("data-src");
                    img.setAttribute("src", imPath);
                }
            }
    </script>
    这里scrollTop是一个比较头疼的问题,又涉及到兼容的问题
    (1)IE6/IE7/IE8：
    对于没有doctype声明的页面里可以使用  document.body.scrollTop来获取 scrollTop高度 ；
    有声明的可以直接使用document.documentElement.scrollTop；
    (2)Firefox:
    直接用document.documentElement.scrollTop ；
    (3)chrome:
    document.body.scrollTop和document.documentElement.scrollTop ；可能都取不到值
    (4)Safari:
    有自己获取scrollTop的函数 ： window.pageYOffset ；
    所以保险的写法就是var scrollTop = document.body.scrollTop || window.pageYOffset ||document.documentElement.scrollTop;, 不管你是啥浏览器我都兼容.yi

