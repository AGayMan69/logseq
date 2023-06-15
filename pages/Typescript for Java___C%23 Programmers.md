- > Author(s): [[N/A]]
  Url: [[https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-oop.html]]
  tags:: #document #programming #[[software development]]
- #### TypeScript vs OOP #TypeScript/Type
  id:: 648ae07a-0bf7-4eba-a0e5-8d97cce6015b
	- #+BEGIN_NOTE
	  In TypeScript, a type can be treated as a set of values
	  #+END_NOTE
	- ==Example==
		- ```ts
		  interface Pointlike {
		    x: number;
		    y: number;
		  }
		  interface Named {
		    name: string;
		  }
		   
		  function logPoint(point: Pointlike) {
		    console.log("x = " + point.x + ", y = " + point.y);
		  }
		   
		  function logName(x: Named) {
		    console.log("Hello, " + x.name);
		  }
		   
		  const obj = {
		    x: 0,
		    y: 0,
		    name: "Origin",
		  };
		   
		  logPoint(obj); // "x = 0, y = 0" 
		  logName(obj); // "Hello, Origin"
		  ```
		- There's no direct declartive relation between `Pointlike` and `obj`, also `Pointlike` type is also not declare at **runtime**
		- In this example, we can think of `obj` as being a member of both the `Pointlike` and `Named`, we can think of type as set
	- **Empty Types**
		- ```ts
		  ts
		  class Empty {}
		   
		  function fn(arg: Empty) {
		    // do something?
		  }
		   
		  // No error, but this isn't an 'Empty' ?
		  fn({ k: 10 });
		  ```
		- Because `{k: 10}` has all of the properties of `Empty` so it's a valid call
	- **Identical Types**
		- ```ts
		  class Car {
		    drive() {
		      // hit the gas
		    }
		  }
		  class Golfer {
		    drive() {
		      // hit the ball far
		    }
		  }
		  // No error?
		  let w: Car = new Golfer();
		  ```
		- No error occurred because both the *structure* of `Car` and `Golfer` are the same
	- **Reflection**
		- #+BEGIN_NOTE
		  TypeScript's type a fully erased.
		  Information like *instantiation of a generice type parameter* is not like available at **runtime**
		  #+END_NOTE