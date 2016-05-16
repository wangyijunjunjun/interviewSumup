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









