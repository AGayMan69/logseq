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
		-