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
模块管理
```
var modle = (function(){
var modle1={}
  var modle2={}
  return { modle1: modle1, modle2: modle2}	
})()

```
登陆弹框
保证一次创建而不是多次创建，创建好只需要display:none或者block就可以
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


