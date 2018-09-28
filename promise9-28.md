####以下代码输出：
		
	//1
	setTimeout(function(){
	    console.log(1);
	}, 0)
	new Promise(function executor(resolve){
    	console.log(2);
		for(var i = 0; i < 1000; i++){
        	i = 9999 && resolve();
    	}
    	console.log(3);
	}).then(function(){
    	console.log(4);
	})
	console.log(5);
	
输出：2 3 5 4 1 
	
promise 是异步的 而执行器里面的代码是同步的	

	//2
	let p = new Promise((resolve, reject) => {
    	resolve('success');
    	reject('error');
    	resolve('success2');
		});
	p.then(str => {
    	conosle.log(str);
	}).catch(e => {
    	console.log(e);
	});

输出：success 

状态一经改变后，就不会再次改变。

如上述题目代码所示，Promise一旦执行了resolve函数后，就不会再执行reject和其他的resolve函数了。一旦Promise执行了reject函数，将会被catch函数捕获，执行catch中的代码
	
	//3
	Promise.resolve(1)
  		.then(2)
  		.then(Promise.resolve(3))
  		.then(console.log)

输出：1

promise.then 必须传入函数，而其他值会直接穿透

####看以下代码进行分析四种Promise的区别是什么？是什么原因导致了不同的执行流程？

	// 假设doSomething和doSomethingElse返回的都是promise实例
	// 问题一
	doSomething()
  		.then(function () {
    		return doSomethingElse();
  		})
  		.then(finalHandler);
	// 问题二
	doSomething()
  		.then(function () {
    		doSomethingElse();
  		})
  		.then(finalHandler);
	// 问题三
	doSomething()
  		.then(doSomethingElse())
  		.then(finalHandler);
	// 问题四
	doSomething()
  		.then(doSomethingElse)
  		.then(finalHandler);

1、一个常见的promise示例，所以调用顺序为doSomething()=>doSomethingElse()=>finalHandler

2、由于第一个promise的then里面没有return，所以调用顺序为doSomething()=>{doSomethingElse()，finalHandler}，后面两个几乎同时调用

3、第一个then传入的是一个doSomething实例，在传入的时候就已经执行，而promise.then只能接收一个函数，所以调用顺序为
{doSomething()，doSomethingElse()}=>finalHandler 当doSomething执行完成时调用finalHandler

4、和第一个没有什么区别，一个常见的promise示例，所以调用顺序为doSomething()=>doSomethingElse()=>finalHandler


####实现一个简单的promise