- Author(s): [[N/A]]
  Url: [[https://www.typescriptlang.org/docs/handbook/intro.html]]
  tags:: #document #programming #[[software development]]
- #### Section 1: Basics
  id:: 648ae680-d37b-4fe7-b568-a9ecd0560337
  collapsed:: true
	- #### TypeScript Usage #TypeScript/Error
		- In general, TypeScript are used to check for...
			- *Typos*
			  logseq.order-list-type:: number
			  ```ts
			  const announcement = "Hello World!";
			   
			  announcement.toLocaleLowercase();
			  announcement.toLocalLowerCase();
			  ```
			- *Uncalled function*
			  logseq.order-list-type:: number
			  ```ts
			  function flipCoin() {
			    // Meant to be Math.random()
			    return Math.random < 0.5;
			  Operator '<' cannot be applied to types '() => number' and 'number'.2365Operator '<' cannot be applied to types '() => number' and 'number'.
			  }
			  ```
			- *Logic errors*
			  logseq.order-list-type:: number
			  ```ts
			  const value = Math.random() < 0.5 ? "a" : "b";
			  if (value !== "a") {
			    // ...
			  } else if (value === "b") {
			  This comparison appears to be unintentional because the types '"a"' and '"b"' have no overlap.2367This comparison appears to be unintentional because the types '"a"' and '"b"' have no overlap.
			    // Oops, unreachable
			  }
			  ```
			- logseq.order-list-type:: number
	- #### TypeScript Compiling Options #TypeScript/Complie
		- **Emitting with Errors**
			- with the `--noEmitOnError` flag, whenever there's an errors in the code, the output `js` file will never gets updated
			- ```zsh
			  tsc --noEmitOnError hello.ts
			  ```
		- **Downleveling**
			- By default TypeScript targets ES3, to specify a newer version, try to run with `--target version` to change TypeScript to target another version
			- ```zsh
			  tsc --target es2015 hello.ts
			  ```
			- ==Default example==
				- ```ts
				  `Hello ${person}, today is ${date.toDateString()}!`;
				  ```
				- ```js
				  // default compile version ES3 doesn't support template literal
				  "Hello ".concat(person, ", today is ").concat(date.toDateString(), "!");
				  ```
		- **Strictness**
			- #+BEGIN_NOTE
			  TypeScript has several type-checking strictness flags that can be turned on
			  #+END_NOTE
			- **`noImplicitAny`**
				- TypeScript doesn't try to infer types, instead will fall back the type `any`
				- Enable the `noImplicitAny` will issue error on variables whose type is implicitly inferred as `any`
			- **`strictNullChecks`**
				- By default, values like `null` and `undefined` are assignable to any other type
				- Enable the `strictNullChecks` flag makes handling `null` and `undefined` more explicit
- #### Section 2: Everyday Types
  id:: 648ae690-8493-430f-8442-cf5dc891a873
	- #### Function Annotation #TypeScript/Type #TypeScript/Function
		- **Anonymous Function**
			- TypeScript determine how it's going to *be called*, the parameters of that function are inferred
				- ```ts
				  const names = ["Alice", "Bob", "Eve"];
				   
				  // Contextual typing for function - parameter s inferred to have type string
				  names.forEach(function (s) {
				    console.log(s.toUpperCase());
				  });
				   
				  // Contextual typing also applies to arrow functions
				  names.forEach((s) => {
				    console.log(s.toUpperCase());
				  });
				  ```
				- TypeScript used the types of the `forEach` function, along with the inferred type of the array, to determine the type `s` will have
		- **Object Types**
			- ```ts
			  // The parameter's type annotation is an object type
			  function printCoord(pt: { x: number; y: number }) {
			    console.log("The coordinate's x value is " + pt.x);
			    console.log("The coordinate's y value is " + pt.y);
			  }
			  printCoord({ x: 3, y: 7 });
			  ```
			- #+BEGIN_NOTE
			  use `,` or `;` to separate the properties, last separator is optional either way
			  if type part of the property is not specify, it will be assumed to be `any`
			  #+END_NOTE
			-
	- #### Optional Properties #TypeScript/Type #TypeScript/Optional
		- Add `?` after the property name to make it *optional*
			- ```ts
			  function printName(obj: { first: string; last?: string}) {
			    ...
			  }
			  
			  // Both OK
			  printName({ first: "Bob", last: "Chan"});
			  printName({ first: "Gay"});
			  ```
		- ` 
		  #+BEGIN_NOTE
		  undefined` will be returned (rather than *runtime error*), if access a property that doesn't exist
		  #+END_NOTE
		- Should check for `undefined` before accessing an *optional* property
			- ```ts
			  function printName(obj: { first: string; last?: string}) {
			    // Error case
			    console.log(obj.last.toUpperCase());
			    // 'obj.last' is possibly 'undefined'.18048'obj.last' is possibly 'undefined'.
			    
			    if (obj.last !== undefined) {
			      // OK
			      console.log(obj.last.toUpperCase());
			    }
			    
			    // safe alternative using JavaScript syntax
			    console.log(obj.last?.toUpperCase());
			  }
			  ```
		-
	- #### Union Types #TypeScript/Type #TypeScript/Union
		- **Defining union type**
			- ```ts
			  function printId(id: number | string) {
			    console.log("Your ID is: " + id);
			  }
			  // OK
			  printId(101);
			  // OK
			  printId("202");
			  // Error
			  printId({ myID: 22342 });
			  // Argument of type '{ myID: number; }' is not assignable to parameter of type 'string | number'.2345Argument of type '{ myID: number; }' is not assignable to parameter of type 'string | number'.
			  ```