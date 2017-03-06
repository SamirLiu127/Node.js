## 創建http server
```
var http = require('http');
http.createServer(function (request, response) {
response.writeHead(200, {'Content-Type': 'text/plain'});
response.end('Hello World\n');
}).listen(8888);
console.log('Server running at http://127.0.0.1:8888/');
```
## Node.js 模組系統
`exports`是模組公開接口<br/>
`require`用於對外部獲取一個模組的接口，即所獲取模組的exports對象
```
//main.js
var hello = require('./hello');
hello.world();
```
```
//hello.js
exports.world = function() {
  console.log('Hello World');
}
```
`module.exports`在外部引用該模組時，其接口對象就是要輸出的hey本身，而不是exports
```
//main.js
var Hello = require('./hello');
hello = new Hello();
hello.setName('Sims');
hello.sayHello();
```
```
//hello.js
function hey() {
	this.setName = function(thyName) {
		name = thyName;
	};
	this.sayHello = function() {
		console.log('Hello ' + name);
	};
};
module.exports = hey;
```
## Node.js 事件
### EventEmitter介绍
`events.EventEmitter` 事件發送與事件監聽器功能封裝
```
//event.js
var EventEmitter = require('events').EventEmitter;
var event = new EventEmitter();

//註冊事件監聽(可同時設多個)
event.on('some_event', function() {
    console.log('some_event occured.');
});
setTimeout(function() {
    //發送事件
    event.emit('some_event');
}, 1000);
```
#### EventEmitter常用的API
EventEmitter.on(event, listener)、emitter.addListener(event, listener) 為指定事件註冊一個監聽器<br>
EventEmitter.emit(event, [arg1], [arg2], [...]) 發射 event 事件，並傳入多個參數<br>
EventEmitter.once(event, listener) 註冊單次監聽器<br>
EventEmitter.removeListener(event, listener) 移除指定事件的某個監聽器<br>
EventEmitter.removeAllListeners([event]) 移除所有事件的所有監聽器，若指定event，則移除指定事件的所有監聽器<br>
### error 事件
`emitter.emit('error')`
EventEmitter 定義了一個特殊的事件 error，在遇到錯誤時會發射error事件<br>
當 error 被發射時，EventEmitter 規定如果沒有響應的監聽器，Node.js 會把它當作異常，退出程序並印出錯誤<br>
### 繼承 EventEmitter
大多數時候我們不會直接使用EventEmitter，而在對像中繼承它。包括fs、net、http在內的，只要是支持事件響應的核心模塊都是EventEmitter的子類。<br>
為什麼要這樣做呢？原因有兩點：<br>
首先，具有某個實體功能的對象實現事件符號語義，事件的監聽和發射應該是一個對象的方法。<br>
其次JavaScript的對像機制是基於原型的，支持部分多重繼承，繼承EventEmitter不會打亂對象原有的繼承關係。<br>
## Node.js 函數
