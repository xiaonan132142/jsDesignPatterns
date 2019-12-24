# commen jsDesignPatterns
### javascript常用设计模式整理关于模式概念说明，模式实例以及应用场景
### 具体包含
#### 1单例模式
#### 2工厂模式
#### 3策略模式
#### 4代理模式
#### 5发布订阅
#### 6装饰模式
#### 7适配器模式
#### 为什么要学习设计模式？
##### 首先设计模式有助于我们提升代码质量，逻辑思维能力，代码的可维护性强以及阅读源码能力。使用得当事半功倍，使用不当事倍功半。每种设计模式都有相应的应用场景，常见如下…
##### 一单例模式
##### 一个类只能有一个实例，提供全局访问，即使多次实例化该类，也只返回第一次实例化的对象。
##### 优势:减少不必要的内存开销，以及避免不必要的全局变量冲突。
##### 简单单例
```
let Tool = {
	getISODate:function(){},
	getUTCDate:function(){}
} 
Es6
class Tool = {
	constructor(name){
 this.name=name
}
}
访问：Tool.func1()

```
##### 惰性单例 适用于比较复杂的单例，存在私有变量和私有方法。
```
let Tool = (function(){
	let _instance = null
	function init(){
		let now = new Date()
		this. getISODate:function(){
			return now.toISOString()
}
this. getUTCDate:function(){
			return now.toUTCString()
}
}
return function(){
	if(!_instance){
		_instance=new init()
}
return _instance
}
})
Es6
class Tool {
	constructor(name){
		if(!Tool._instance){
this.name=name
Tool._instance=this

}
return Tool._instance
}
} 
```
##### 应用场景
##### 模块管理
```
var modle = (function(){
var modle1={}
  var modle2={}
  return { modle1: modle1, modle2: modle2}	
})()

```
##### 登陆弹框-保证一次创建而不是多次创建，创建好只需要display:none或者block就可以
```
class Login {
  constructor(){
    this.init()
  }
  // 初始化init
  init(){
    // 创建dom
    let mask = document.createElement('div')
    mask.classList.add('mask-layer')
    mask.innerHTML='<div class="login-wrapper">\n' +
      '      <div class="login-title">\n' +
      '        <div class="title-text">登录框</div>\n' +
      '        <div class="close-btn">×</div>\n' +
      '      </div>\n' +
      '      <div class="username-input user-input">\n' +
      '        <span class="login-text">用户名:</span>\n' +
      '        <input type="text">\n' +
      '      </div>\n' +
      '      <div class="pwd-input user-input">\n' +
      '        <span class="login-text">密码:</span>\n' +
      '        <input type="password">\n' +
      '      </div>\n' +
      '      <div class="btn-wrapper">\n' +
      '        <button class="confrim-btn">确定</button>\n' +
      '        <button class="clear-btn">清空</button>\n' +
      '      </div>\n' +
      '    </div>'
    //插入元素
    document.body.insertBefore(mask,document.body.childNodes[0])
    //注册关闭弹框
    Login.getCloseMask()
  }
  // 获取元素
  static getLoginDom(cls){
    return document.querySelector('cls')
  }
  // 关闭弹窗
  static getCloseMask(){
    this.getLoginDom('.closeBtn').addEventListener('click',()=>{
      this.getLoginDom('mask-layer').style="display:none"
    })
  }
  // 获取实例
  static getInstance(){
    if(!this.instance){
      this.instance = new Login()
    }else{
      this.getLoginDom('mask-layer').removeAttribute('style')
    }
    return this.instance
  }
}
// 点击弹出
Login.getInstance();

```
##### 工厂模式
##### 将逻辑封装在一个函数中，就可以视为工厂模式。根据抽象程度，可以分为简单工厂，工厂方法，抽象工厂。
##### 简单工厂
```
et Factory = function (role) {
  function User(opt) {
    this.name=opt.name
    this.pages = opt.pages
  }
  switch (role) {
    case 'admin':
      return new User({name:'admin',pages:['首页','配置页']});
      break;
    case 'user':
      return new User({name:'user',pages:['首页']});
      break;
    default:
      throw new Error('参数错误');
  }
}

```
##### 工厂方法-实例化对象工厂类，可以理解为创建类，批量生产。
```
let Factory = function (role) {
  if(this instanceof Factory){
    var s = new this[role]()
    return s
  }else{
    return new Factory(role)
  }
}
Factory.prototype={
  admin:function () {
    this.name='admin'
    this.pages=['首页','配置页']
  },
  user:function () {
    this.name='user'
    this.pages=['首页']
  }
}

```
##### 抽象工厂-抽象类可以理解为，子类继承父类，不是直接创建实例
```
ar Factory = function (supType,superType) {
  if(typeof Factory.superType==='function'){
    function F() {}
    // 继承父类
    F.prototype = new Factory[superType]()
    supType.constructor = supType
    supType.prototype = new F()
  }else{
    throw new Error('未创建该抽象类');
  }
}
Factory.QQType =function () {
  this.type=' QQType '
}
Factory.QQType.prototype = {
  getName : function () {
    return new Error('抽象方法不能调用');
  }
}
//普通qq用户子类
function QQType(name) {
  this.name = name;
  this.pages = ['首页', '配置页']
}
//抽象工厂实现qq类的继承
Factory(QQType, 'QQType');
//子类中重写抽象方法
QQType.prototype.getName = function() {
  return this.name;
}
//实例化qq用户
let QQTypea = new QQType('微信小李');
console.log(QQTypea.getName(), QQTypea.type); //微信小李 QQType
let QQTypeb = new QQType('微信小王');
console.log(QQTypeb.getName(), QQTypeb.type); //微信小王 QQType

```
##### 应用场景 类似控制路由权限 饮料机…
##### 三策略模式
##### 定义一系列算法，一个一个封装起来，并且可以相互替换。策略模式= 策略类(具体算法)+环境类(Context)可变的参数拿出来，做计算工具函数，取最终结果。策略将算法随机组合，灵活应用，算法是不变的。
##### 优势：有效避免多重条件判断，更容易维护
```
*简单策略模式*/
//年终奖计算
const Bouns = {
  S(salary){
    return salary*3
  },
  A(salary){
    return salary*2
  },
  B(salary){
    return salary*1
  },
  C(salary){
    return salary*0.5
  }
}
// 冻结不改
Object.freeze(Bouns)
// 环境类Context
// @params {String} type 绩效等级
// @params {Number} salary 工资
const calculateBouns=function(type,salary){
  return Bouns[type](salary)
}

```
##### 应用场景（表单验证）
```
/*策略模式--表单验证*/
//定义固定算法
const Verify = {
  // 是否为空
  isEmpty(value,errorMsg){
    if(value===''){
      return errorMsg
    }
  },
  minLength(value,errorMsg,length){
    if(value.length<length){
      return errorMsg
    }
  },
  // 手机号验证
  formatPhone(value,errorMsg){
    if(!/(^1[3|5|8][0-9]{9}$)/.test(value)) {
      return errorMsg
    }
  },
}
Object.freeze(Verify)

/*
* 模仿vue表单验证实现
* */
// 定义Validator类
var Validator = function () {
  this.cache=[] // 保存校验规则
}
Validator.prototype.add=function (dom,rules) {
  var self = this
  for(var i=0,rule;rule=rules[i++];){
    (function (rule) {
      let verifyArr = rule.split(':')
      let errorMsg = rule.errorMsg
      self.cache.push(function () {
        let verify = verifyArr.shift()
        verifyArr.unshift(dom.value)
        verifyArr.push(errorMsg)
        return verifies[verify].apply(dom,verifyArr)
      })
    })(rule)
  }
}
Validator.prototype.start=function () {
  for(var i = 0, validatorFunc; validatorFunc = this.cache[i++]; ) {
    var msg = validateFunc(); // 开始效验 并取得效验后的返回信息
    if(msg) {
      return msg;
    }
  }
}
// 代码调用
var registerForm = document.getElementById('registerForm');
var validateFunc = function () {
  var validator = new Validator()
  validator.add(registerForm.userName,[
    {Verify: 'isNotEmpty',errorMsg:'用户名不能为空'},
    {Verify: 'minLength:6',errorMsg:'用户名长度不能小于6位'}
  ])
  var errorMsg = validator.start()
  return errorMsg
}
// 点击确定提交
registerForm.onsubmit = function(){
  var errorMsg = validateFunc();
  if(errorMsg){
    alert(errorMsg);
    return false;
  }
}

```
##### 四代理模式
##### 为一个对象提供一个代用品或占位符，以便控制对它的访问。（为一个对象找一个替代对象，来方便对原对象的访问。）代理模式分为：虚拟代理，保护代理，缓存代理
##### 1保护代理（类似过滤 提前判断）提前过滤一些不必要的东西，就是保护代理。
```
/*保护模式*/
function sendMsg(msg){
  console.log(msg)
}
function proxySendMsg(msg){
  if(!msg) {
    console.log('deny')
    return
  }
  // 过滤
  msg = (''+msg).replace('菜鸟','')
  sendMsg(msg)
}
/*es6 proxy 实现成绩过滤*/
const scoreList={
  zs:'58',
  ls:'89'
}
const scoreProxy = new Proxy(scoreList,{
  get:function (scoreList,name) {
    if(scoreList[name]>60){
      console.log('输出成绩')
      return scoreList[name]
    }else{
      console.log('不及格成绩不能输出')
    }
  }
})
scoreProxy['zs'] //不及格成绩不能输出
scoreProxy['ls'] //输出成绩 89

```
##### 2虚拟代理,把一些开销大的对象，按需加载。
```
/*不使用模式，耦合性太高*/
var myImageA = (function(){
  var imgNode = document.createElement("img");
  document.body.appendChild(imgNode);
  var img = new Image();
  img.onload = function(){
    imgNode.src = this.src;
  };
  return {
    setSrc: function(src) {
      imgNode.src = "http://img.lanrentuku.com/img/allimg/1212/5-121204193R0.gif";
      img.src = src;
    }
  }
})();
// 调用方式
myImageA.setSrc("https://www.baidu.com/img/bd_logo1.png");

/*使用虚拟模式*/
/*实现图片懒加载*/
var myImage = (function () {
  var imgNode = document.createElement('img')
  document.body.appendChild(imgNode)
  return{
    setSrc:function (url) {
      imgNode.src = url
    }
  }
})()
var proxyImage = (function () {
  var img = new Image()
  img.onload=function () {
    myImage.setSrc(this.src)
  }
  return {
    setSrc:function (src) {
      myImage.setSrc('http://seopic.699pic.com/photo/40007/8839.jpg_wh1200.jpg');
      img.src = src;
    }
  }
})()
proxyImage.setSrc('http://seopic.699pic.com/photo/40006/7735.jpg_wh1200.jpg')

```
##### 3缓存代理,开销大的运算，避免重复运算，暂时缓存，提高性能。
```
/*缓存模式*/
var mult = function () {
  var a=1;
  for(var i=0;i<arguments.length;i++){
    a=a*arguments[i]
  }
  return a
}
var plus = function () {
  var a=0;
  for(var i=0;i<arguments.length;i++){
    a=a+=arguments[i]
  }
  return a
}
// 代理缓存
var proxyFun=function (fn) {
  var cache = {} // 缓存对象
  return function () {
    var args = Array.prototype.join.call(arguments,',')
    if(args in cache){
      return cache[args] // 使用缓存代理
    }
    return cache[args]=fn.apply(this,arguments)
  }
}
var proxyMult=proxyFun(mult)
proxyMult(1,2,3,4) // 24
proxyMult(1,2,3,4) // 缓存中24

```
##### 五发布订阅
##### 也称为观察者模式，定义了对象间一对多的依赖关系，当一个对象发生改变时，依赖于当前对象的所有对象都会得到通知。
##### (解决问题：当很多人询问房子什么开盘，不需要每个月每天无限电话来询问，而是留下电话相当于订阅，如果开盘，开发商统一发布通知就是发布)
```
/*
*发布订阅模式
*/
/*经典发布订阅模式*/
//订阅
document.body.addEventListener('click',function () {
  console.log('click1')
})
//发布
document.body.click() // click1
/*自定义发布订阅*/
class Event{
  constructor(){
    this._cache=[] // 存储事件数据结构
  }
  // 订阅
  on (type,callback){
    let fns = (this._cache[type]=this._cache[type]||[])
    if(fns.indexOf(callback)!==-1){
      fns.push(callback)
    }
    return this
  }
  // 发布 emit
  trigger(type,data){
    let fns = this._cache[type]
    if(Array.isArray(fns)){
      fns.forEach((fn)=>{
        fn(data)
      })
    }
    return this
  }
  // 取消订阅
  off(type,callback){
    let fns = this._cache[type]
    if(Array.isArray(fns)){
      if(callback){
        let index = fns.indexOf(callback)
        if(index!=-1){
          fns.splice(index,1)
        }
      }else{
        fns.length = 0
      }
    }
    return this
  }
}

```
##### 应用场景—互联网商城登陆
假如开发一个商城，网站有header nav 消息列表 购物车
强耦合写法，都是登陆模块中的
```
Login.success(function(data){
	header.setAvatar(data.avatar)
   nav.setAvatar(data.avatar)
   message.refresh()
   cart.refresh()
   // 忽然增加地址刷新
   address.refresh()
})

```
采用发布订阅，解除耦合度

```
/*应用场景--互联网商城登陆模块*/
$.ajax('http:xxx',function (data) {
  login.trigger('loginsuccess',data)
})
// 各模块监听登陆
var header=(function () {
  login.on('loginsuccess',function (data) {
    header.setAvatar(data.avatar)
  })
  return{
    setAvatar:function (avatar) {
      console.log('设置头部头像成功')
    }
  }
})
var cart=(function () {
  login.on('loginsuccess',function (data) {
    cart.refresh(data)
  })
  return{
    refresh:function (data) {
      console.log('刷新成功')
    }
  }
})
// 忽然新增地址刷新
var address = (function (data) {
  login.on('loginsuccess',function (data) {
    address.refresh(data)
  })
  return {
    refresh:function (data) {
      console.log('刷新地址成功')
    }
  }
})

```
##### 优点：对象之间解耦1时间解耦2对象解耦，用于MVC MVVM框架
##### 缺点：1创建订阅者函数不一定会被执行，驻留内存内有一定开销。2弱化对象间的联系，出现问题难以追踪维护。
##### 六装饰
##### 动态给对象增加职责的方式，称为装饰者模式。在不改变对象自身的基础上，在程序运行期间给对象动态添加职责。和继承相比，装饰者模式更加便捷。装饰者，也称为包装器，一个对象嵌套在另一个对象中，形成包装链。(出现原因,我们不想维护原有对象，但是有需要在原有对象基础上添加功能。)
```
/*
* 装饰函数
* */
var a = function () {
  alert(1)
}
//需要增加alert(2)
//在原函数基础上写 
var a = function () {
  alert(1)
  alert(2)
}
// 使用装饰函数
var a = function () {
  alert(1)
}
var _a = a
a=function () {
  _a()
  alert(2)
}

```
问题：需要维护很多中间变量，可能会导致中间变量越来越多。
##### AOP装饰函数
```
/*污染原型写法*/
Function.prototype.before=function (beforeFn) {
  var _self = this // 保存原函数引用
  return function () {
    beforeFn.apply(this,arguments)
    return _self.apply(this,arguments)
  }
}
Function.prototype.after=function (afterFn) {
  var _self = this // 保存原函数引用
  return function () {
    afterFn.apply(this,arguments)
    return _self.apply(this,arguments)
  }
}
window.onload(){
  alert(1)
}
window.onload=(window.onload||function () {}).after(function () {
  alert(2)
}).after(function () {
  alert(3)
})
/*不污染原型变通写法*/
var before = function (fn,beforeFn) {
  return function () {
    beforeFn.apply(this,arguments)
    return fn.apply(this,arguments)
  }
}
var a = before(
  function () {alert(1)},
  function () {alert(2)}
  )
a = before(a,function () {
  alert(3)
})
a()//1,2,3
```
##### 应用场景
1预防CSRF攻击，在http 中添加token
```
/*预防CSRF攻击，在http中加入Token参数*/
/*一般写法*/
var getToken=function(){
  return "token"
}
var ajax = function (type,url,param) {
  param = param||{}
  param.token = getToken()
}
/*AOP写法*/
var ajax = function (type,url,param) {
  console.log(param)
}
ajax=ajax.before(function (type,url,param) {
  param.token = getToken()
})
ajax('get','http:xxx',{name:json})
// {name:json,token:"token"}

```
2使用表单校验插件
```
/*表单校验*/
var validata=function () {
  if(username.value==''){
    return false
  }
}
var submit=function () {
  var param = {
    username:username.value
  }
  $.ajax('http:xxx',param)
}
// 定义装饰函数
Function.prototype.before=function(beforefn){
  var _self = this
  return function () {
    if(beforefn.apply(this,arguments)===false){
      return
    }
    return _self.apply(this,arguments)
  }
}
// 调用装饰函数
submit=submit.before(validata)
submitBtn.onclick=function () {
  submit()
}

```
##### 七适配器模式(相对比较简单的一种模式)
##### 适配器别名包装器，将原接口转换为符合客户要求的另外一个接口，然后客户只需要和适配器打交道就好了。
```
/*
* 适配器模式
* */
/*简单例子--调用谷歌百度地图*/
var gooleMap={
  show:function () {
    console.log('调用谷歌地图')
  }
}
var baiduMap={
  baiduShow:function () {
    console.log('调用百度地图')
  }
}
var renderMap=function (map) {
  if(map.show instanceof Function){
    map.show()
  }
}
renderMap(gooleMap)//调用谷歌地图
renderMap(baiduMap)//没有show方法
// 百度地图适配器
var baiduMapAdpter={
  show:function f() {
    return baiduMap.baiduShow()
  }
}
renderMap(gooleMap)//调用谷歌地图
renderMap(baiduMapAdpter)//调用百度地图
/*应用场景-常见数据结构之改变*/
var getZJCity = function () {
  var zjCity = [
    {name:'hangzhou',id:12},
    {name:'ningbo',id:13},
  ]
  return zjCity
}
var render = function (fn) {
  document.write(JSON.stringify(fn()))
}
render(getZJCity)
// 忽然要采用新的数据结构
var zjCity={
  hangzhou:12,
  ningbo:13,
}
// 添加适配器(将老数据转为新数据)
getZJCityAdapter=function (oldfn) {
  let address = {}
  oldAddress = oldfn()
  for(var i=0,c;c=oldAddress[i++];){
    address[c.name] = c.id
  }
  return function () {
    return address
  }
}
render(getZJCityAdapter(getZJCity))

```



