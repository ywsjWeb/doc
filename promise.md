#什么是Promise
Promise是异步编程的一种解决方案，它有三种状态，分别是pending-进行中、resolved-已完成、rejected-已失败

当Promise的状态又pending转变为resolved或rejected时，会执行相应的方法，并且状态一旦改变，就无法再次改变状态，这也是它名字promise-承诺的由来
##基本使用
	// 方法1
	let promise = new Promise ( (resolve, reject) => {
    	if ( success ) {
        	resolve(a) // pending ——> resolved 参数将传递给对应的回调方法
    	} else {
        	reject(err) // pending ——> rejectd
    	}
	})
	promise.then(data=>{console.log(data)},err=>{console.log(err)})

#promise解决了什么问题
##回调地狱（金字塔）
		a(function (resultsFromA) {
  			b(resultsFromA, function (resultsFromB) {
   				 c(resultsFromB, function 	(resultsFromC) {
      					d(resultsFromC, function (resultsFromD) {
        					e(resultsFromD, function (resultsFromE) {
          						f(resultsFromE, function (resultsFromF) {
            						console.log(resultsFromF);
          						})
        					})
      					})
    				})
 				})
			});

回调信任问题

### 调用回调太早

		function result(data) {
			console.log( a );
		}

		var a = 0;
		ajax( "..pre-cached-url..", result );
		a++;
		//0？1

###调用回调太晚（或根本不调）

		// 一个使Promise超时的工具
		function timeoutPromise(delay) {
			return new Promise( function(resolve,reject){
			setTimeout( function(){
				reject( "Timeout!" );
				}, delay );
			});
		}

		// 为`foo()`设置一个超时
		Promise.race( [
			foo(),					// 尝试调用`foo()`
			timeoutPromise( 3000 )	// 给它3秒钟
		])
		.then(
			function(){
			// `foo(..)`及时地完成了！
			},
		function(err){
			// `foo()`不是被拒绝了，就是它没有及时完成
			// 那么可以考察`err`来知道是哪种情况
		});

###调用回调太少或太多次

根据定义，对于被调用的回调来讲 一次 是一个合适的次数。“太少”的情况将会是0次，和我们刚刚考察的从不调用是相同的。

Promise被定义为只能被解析一次。如果因为某些原因，Promise的创建代码试着调用resolve(..)或reject(..)许多次，或者试着同时调用它们俩，Promise将仅接受第一次解析，而无声地忽略后续的尝试。

因为一个Promise仅能被解析一次，所以任何then(..)上注册的（每个）回调将仅仅被调用一次。

当然，如果你把同一个回调注册多次（比如p.then(f); p.then(f);），那么它就会被调用注册的那么多次。响应函数仅被调用一次的保证并不能防止你砸自己的脚。

###没能传递必要的环境/参数
		Promise.then((res)=>{},(err)=>{})
这里的两个方法的参数最多只会接收一个参数，如果需要传多个需要手动将参数封装成一个对象传递

##兼容性
现代浏览器基本都兼容，edge不兼容promise.finally()，ie不兼容promise，如果需要在ie中是promise则需要引入promise ployfill
## 基本实现
[https://github.com/huanshen/Promise/tree/master](https://github.com/huanshen/Promise/tree/master)
#promise API
###Promise.resolve(..)
###Promise.reject(..)
###Promise.then(..)
###Promise.catch(..)
###Promise.all(..)
###Promise.race(..)
###Promise.finally(..)

##async/await

#参考文章
1. [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
2. [你不知道的Javascript(中)（回调）](https://github.com/getify/You-Dont-Know-JS/blob/1ed-zh-CN/async%20%26%20performance/ch2.md)
3. [你不知道的Javascript(中)（promise）](https://github.com/getify/You-Dont-Know-JS/blob/1ed-zh-CN/async%20%26%20performance/ch3.md)
4. [es6-promise](https://github.com/stefanpenner/es6-promise)
5. [promises/A+规范](http://www.ituring.com.cn/article/66566)