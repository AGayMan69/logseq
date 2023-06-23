- Author(s): [[N/A]]
  Url: [JavaScript Guide - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
  tags:: #document #programming #[[software development]]
- #### Using Promises
  id:: 649278b3-2fec-43f4-9a4a-404465f13e8b
	- #### What is Promises #JavaScript/Promises #JavaScript/Async
		- A *Promise* is a returned object to which you attach callbacks, **instead** of passing callbacks into a function
		- ```js
		  // approach 1: passing callback to function 
		  function successCallback(result) {
		    console.log(`Audio file ready at URL: ${result}`);
		  }
		  
		  function failureCallback(error) {
		    console.error(`Error generating audio file: ${error}`);
		  }
		  
		  createAudioFileAsync(audioSettings, successCallback, failureCallback);
		  
		  ```
		- ```js
		  createAudioFileAsync(audioSettings).then(successCallback, failureCallback);
		  ```
	- #### Promises Chaining #JavaScript/Promises #JavaScript/Async
		- Promise can be used to avoid the *classic callback pyramid of doom*
			- ```js
			  doSomething(function (result) {
			    doSomethingElse(result, function (newResult) {
			      doThirdThing(newResult, function (finalResult) {
			        console.log(`Got the final result: ${finalResult}`);
			      }, failureCallback);
			    }, failureCallback);
			  }, failureCallback);
			  ```
		- We can accomplish this by creating a *promise chain*
		- ```js
		  const promise = doSomething();
		  const promise2 = promise.then(successCallback, failureCallback);
		  ```
		- The second `promise2` represents the completion of `doSomething()`, `successCallback` or `failureCallback` which can be another async function that return a **promise**. The callbacks added to `promise2` get queued behind the promise returned by either `successCallback` or `failureCallback`
		- #+BEGIN_NOTE
		  `catch(failureCallback)` is short for `then(null, failureCallback)`
		  #+END_NOTE
			- ```js
			  doSomething()
			  	.then((result) => doSomethingElse(result))
			  	.then((newResult) => doThirdThing(newResult))
			  	.then((finalResult) => {
			    		console.log(`Got the final result: ${finalResult}`)
			  	})
			  	.catch(failureCallback);
			  ```
		- ==Example==
			- ```js
			  doSomething()
			    .then((url) =>
			      fetch(url)
			        .then((res) => res.json())
			        .then((data) => {
			          listOfIngredients.push(data);
			        }),
			    )
			    .then(() => {
			      console.log(listOfIngredients);
			    });
			  ```
			- ```js
			  doSomething()
			    .then((url) => fetch(url))
			    .then((res) => res.json())
			    .then((data) => {
			      listOfIngredients.push(data);
			    })
			    .then(() => {
			      console.log(listOfIngredients);
			    });
			  ```
	- #### Nesting #JavaScript/Promises #JavaScript/Async
		- Nesting is a control structure to limit the scope of `catch` statements, a nested `catch` only catches failure in its scope and below
		- ==Example==
			- ```js
			  doSomethingCritical()
			    .then((result) =>
			      doSomethingOptional(result)
			        .then((optionalResult) => doSomethingExtraNice(optionalResult))
			        .catch((e) => {}),
			    ) // Ignore if optional stuff fails; proceed.
			    .then(() => moreCriticalStuff())
			    .catch((e) => console.error(`Critical failure: ${e.message}`));
			  ```
			- The inner error-silencing `catch` handler only catches failures from `doSomethingOptional()` and `doSomethingExtraNice()`. If `doSomethingCritical()` fails, its error is caught by the final (out) `catch` only
	- #### Chaining after a catch #JavaScript/Promises #JavaScript/Async
		- It's possible to chain *after* a failure
			- ```js
			  new Promise((resolve, reject) => {
			    console.log("Initial");
			    resolve();
			  })
			  	.then(() => {
			    		throw new Error("Something failed");
			    
			    		console.log("Do this");
			  	})
			  	.catch(() => {
			    		console.error("Do that");
			  	})
			  	.then(() => {
			    		console.log("Do this, no matter what happened before");
			  	});
			  ```
		- #+BEGIN_NOTE
		  Notice that "Do this" does not displayed because the "Something failed" caused a rejection
		  #+END_NOTE
	- #### Common mistakes #JavaScript/Promises #JavaScript/Async
		- ==Example==
		- ```js
		  doSomething()
		  	.then(function (result) {
		    		// Forgot to return promise from inner chain + unnecessary nesting
		    		doSomethingElse(result).then((newResult) => doThirdThing(newResult));
		  	})
		  	.then(() => doFourthThing());
		  // Forgot to terminate chain with a catch!
		  ```
		- The first mistake is to not chain things together properly, as the new promise has no return, we will have two independent chains racing. It means `doFourthThing()` won't wait for `doSomethingElse()` or `doThirdThing()` to finish, also lead to uncaught error because of separate error handling
		- The second mistake is that it limits the scope of inner error handlers, may lead to uncaught errors
		- The third mistakes is forgetting to terminate chains with `catch`
		- #+BEGIN_NOTE
		  Unterminated promise chains lead to uncaught promise rejections in most browsers
		  #+END_NOTE
		- [[#green]]==Good rule of thumb==
			- ```js
			  doSomething()
			  	.then(function (result) {
			    		// If using a full function expression: return the promise
			    		return doSomethingElse(result);
			  	})
			  	// If using arrow functions: omit the braces and implicity return the result
			  	.then((newResult) => doThirdThing(newResult));
			  	// Even if the previous chained promise returns a result, the next one
			  	// doesn't necessarily have to use it. You can pass a handler that doesn't
			  	// consume any result.
			  	.then((/* result ignored */)) => doFourthThing()
			  	// Always end the promise chain with a catch handler to avoid any
			  	// unhandled rejections
			  	.catch((error) => console.eror(error));
			  ```
		- #+BEGIN_NOTE
		  `{} => x` is short for `() => { return x; }`.
		  #+END_NOTE
	- #### Error handling #Javascript/Promises #JavaScript/Async
		- Previously, the promise chain example:
			- ```js
			  doSomething()
			    .then((result) => doSomethingElse(result))
			    .then((newResult) => doThirdThing(newResult))
			    .then((finalResult) => console.log(`Got the final result: ${finalResult}`))
			    .catch(failureCallback);
			  ```
		- Is pretty much this
			- ```js
			  async function foo() {
			    try {
			      const result = await doSomething();
			      const newResult = await doSomethingElse(result);
			      const finalResult = await doThirdThing(newResult);
			      console.log(`Got the final result: ${finalResult}`);
			    } catch (error) {
			      failureCallback(error);
			    }
			  }
			  ```
	- #### Promise rejection events #Javascript/Promises #JavaScript/Async
		- #+BEGIN_NOTE
		  If a promise rejection event is not handled by any handler, it bubbles to the top of the call stack
		  #+END_NOTE
		- Whenever a *promise* is rejected, one of the two events is sent to the global scope(either `window` or `Worker`)
		- The two event are `unhandledrejection` and `rejectionhandled`
			- `unhandledrejection`
				- Sent when a promise is rejected but there is no rejection handler available
			- `rejectionhandled`
				- Sent when a handler is attached to a rejected promise that has already caused an `unhandledrejection` event
		- Both cases, the event (`PromiseRejectionEvent` type) has as members
			- `promise`
				- property indicating the promise that was rejected
			- `reason`
				- property that provides the reason given for the promise to be rejected
			- These make it possible to offer *fallback* error handling for promises
			- These handler are global per context, all errors will go to the **same event handlers**
		- #+BEGIN_NOTE
		  In *Node.js*, handling promise rejection is quite different. Node.js `unhandledRejection` event is captured by adding a handler
		  #+END_NOTE
			- ```js
			  process.on("undhandledRejection", (reason, promise) => {
			    // Add code here to examine the "promise" and "reason" values
			  })
			  ```
			- For prevent the error from being logged to the console (the default action), that's necessary to add that `process.on()` listener; An `preventDefault` of browser runtime is not required
	- #### Composition #JavaScript/Promises #JavaScript/Async
		- There are 4 *composition* tools for running asynchronous operations concurrently: `Promise.all()`, `Promise.allSettled()`, `Promise.any()`, and `Promise.race()`
		- `Promise.all()`
			- Starting operations at the same time and wait for them all to finish like this:
			- ```js
			  Promise.all([func1(), func2(), func3()]).then(([result1, result2, result3]) => {
			    // use result1, result2, and result3
			  });
			  ```
			- If one of the promises in the array rejects, `Promise.all()` immediately rejects the returned promise and aborts the other operations
			- `Promise.allSettled()` is another composition tool that *ensures all operations are complete* before resolving
			- These methods all run promises **concurrently**
		- `Promise.resolve()`
			- use for sequential composition
			- ```js
			  [func1, func2, func3]
			  	.reduce((p, f) => p.then(f), Promise.resolve())
			  	.then((result3) => {
			    		/* use result3 */
			  	});
			  ```
			- a more reusable compose function
			- ```js
			  const applyAsync = (acc, val) => acc.then(val);
			  const composeAsync =
			    (...funcs) =>
			  	(x) => 
			  		funcs.reduce(applyAsync, Promise.resolve(x));
			  
			  // usage
			  const transformData = composeAsync(func1, func2, func3);
			  const result3 = transformData(data);
			  ```
			- The `composeAsync()` function accepts any number of functions as arguments and return a new function that accepts an initial value to passed through
			- ```js
			  let result;
			  for (const f of [func1, func2, func3]) {
			    result = await f(result);
			  }
			  
			  ```
	- #### Creating a Promise around an old callback API #JavaScript/Promises #JavaScript/Async
		- A `Promise` can be created from scratch using its *constructor*. This can be required to wrap old APIs
		- ==Example==
			- ```js
			  setTimeout(() => saySomething("10 seconds passed"), 10 * 100);
			  // If `saySomething() fails or contains a programming error, nothing catch it`
			  ```
			- workaround (wrapping `setTimeout` in a promise)
			- ```js
			  const wait = (ms) => new Promise((resolve) => setTimeout(resolve, ms));
			  
			  wait(10 * 1000)
			  	.then(() => saySomething("10 seconds"))
			  	.catch(failureCallback);
			  ```
			- The promise constructor takes an executor function that lets us resolve or reject a promise manually
	- #### Guarantees #Javascript/Promises #JavaScript/Async
		- In the callback-based API, the callback may be called synchronously or asynchronously
		- The design below is strongly discouraged because it leads to the "state of Zalgo" see [Designing APIs for Asynchrony](https://blog.izs.me/2013/08/designing-apis-for-asynchrony/)
		- ```js
		  function doSomething(callback) {
		    if (Math.random() > 0.5) {
		      callback();
		    } else {
		      setTimeout(() => callback(), 1000);
		    }
		  }
		  ```
		- The job of maintaining the callback queue and deciding when to call the callbacks is delegated to the promise implementation, including:
			- Callbacks added with `then()` will never be invoked *before* the [completion of the current run](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop#run-to-completion) of the JavaScript event loop
			  logseq.order-list-type:: number
			- These callbacks will be invoked even if they were added *after* the success or failure of the asynchronous operation that the promise represents
			  logseq.order-list-type:: number
			- Multiple callbacks may be added by calling `then` several times. They will be invoked **one after another**
			  logseq.order-list-type:: number
		- The functions passed to `then()` will never be called **synchronously**, even with resolved promise:
			- ```js
			  Promise.resolve().then(() => console.log(2));
			  console.log(1);
			  // Logs: 1, 2
			  ```
		- The passed-in function is put on a microtask queue, it runs later just before control is returned to the event loop
			- ```js
			  const wait = (ms) => new Promise((resolve) => setTimeout(resolve, ms));
			  
			  wait(0).then(() => console.log(4));
			  Promise.resolve()
			  	.then(() => console.log(2))
			  	.then(() => console.log(3));
			  console.log(1); // 1, 2, 3, 4
			  ```
	- #### Task queues vs microtasks #JavaScript/Promises #JavaScript/Async
		- #+BEGIN_NOTE
		  Promise callbacks are handled as a *microtask*, where `setTimeout()` callbacks are handled as *task queues*
		  #+END_NOTE
			- ```js
			  const promise = new Promise((resolve, reject) => {
			    console.log("Promise callback");
			    resolve();
			  }).then((result) => {
			    console.log("Promise callback (.then)");
			  });
			  
			  setTimeout(() => {
			    console.log("event-loop cycle: Promise (fulfilled)", promise);
			  }, 0);
			  
			  console.log("Promise (pending)", promise)
			  ```
		- Result
			- ```js
			  // Promise callback
			  // Promise (pending) Promise {<pending>}
			  // Promise (.then)
			  // event-loop cycle: Promise (fulfilled) Promise {<fulfilled>}
			  ```
	- #### When promises and tasks collide #JavaScript/Promises #JavaScript/Async
		- whenever you have promises and task which firing in unpredictable orders, it's possible to use a microtask to check status or balance out the promises
		- see the [microtask guide](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide) for `queueMicrotask()`