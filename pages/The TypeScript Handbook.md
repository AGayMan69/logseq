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
		  `undefined` will be returned (rather than *runtime error*), if access a property that doesn't exist
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
		- **Working with Union Types**
			- #+BEGIN_NOTE
			  TypeScript will only allow an operation if it is *valid* for every member in the **union**
			  #+END_NOTE
			- ```ts
			  function printId(id: number | string) {
			    console.log(id.toUpperCase());
			    /* Property 'toUpperCase' does not exist on type 'string | number'.
			    Property 'toUpperCase' does not exist on type 'number'.2339Property 'toUpperCase' does not exist on type 'string | number'.
			    Property 'toUpperCase' does not exist on type 'number'. */
			  }
			  ```
			- Solution to this is to *narrow* the union with code
				- ```ts
				  function printId(id: number | string) {
				    if (typeof id === "string") {
				      // in this branch, id is of type 'string'
				      console.log(id.toUpperCase());
				    } else {
				      // Here, id is of type 'number'
				      console.log(id);
				    }
				  }
				  ```
			- Another example with Array
				- ```ts
				  function welcomePeople(x: string[] | string) {
				    if (Array.isArray(x)) {
				      // Here: 'x' is 'string[]'
				      console.log("Hello", x.join(" and "));
				    } else {
				      // Here: 'x' is 'string'
				      console.log("Welcome lone traveler " + x);
				    }
				  }
				  ```
			- not need for *narrowing* if shared in common
				- ```ts
				  // Return type is inferred as number[] | string
				  function getFirstThree(x: number[] | string) {
				    return x.slice(0, 3);
				  }
				  ```
	- #### Type Alias #TypeScript/Type #[[TypeScript/Type Alias]]
		- #+BEGIN_NOTE
		  *type* alias is exactly that - *name* for any *type*. which can be used more than once and refer to it by a single name
		  #+END_NOTE
		- ==Example==
			- ```ts
			  type Point = {
			    x: number;
			    y: number;
			  };
			   
			  // Exactly the same as the earlier example
			  function printCoord(pt: Point) {
			    console.log("The coordinate's x value is " + pt.x);
			    console.log("The coordinate's y value is " + pt.y);
			  }
			  ```
		- ==Union Example==
			- ```ts
			  type ID = number | string;
			  ```
		- #+BEGIN_NOTE
		  *type* is just an alias, when using the alias, it's exactly as if you had written the aliased type
		  #+END_NOTE
			- ```ts
			  type UserInputSanitizedString = string;
			   
			  function sanitizeInput(str: string): UserInputSanitizedString {
			    return sanitize(str);
			  }
			   
			  // Create a sanitized input
			  let userInput = sanitizeInput(getInput());
			   
			  // Can still be re-assigned with a string though
			  userInput = "new input";
			  ```
	- #### Interfaces #TypeScript/Type #Typescript/Interface
		- #+BEGIN_NOTE
		  An *interface declaration* is another way to name an object type
		  #+END_NOTE
			- ```ts
			  interface Point {
			    x: number;
			    y: number;
			  }
			  
			  function printCoord(pt: Point) {
			    console.log("The coordinate's x value is " + pt.x);
			    console.log("The coordinate's y value is " + pt.y);
			  }
			  
			  printCoord({ x: 100, y: 100 });
			  ```
	- #### Difference between Type aliases and Interfaces
		- #+BEGIN_NOTE
		  They are pretty much the same, **except** a *type* cannot be re-opened to add new properties vs an interface which is always extendable
		  #+END_NOTE
		- ==Example (Extending)==
			- *interface*
				- ```ts
				  interface Animal {
				    name: string;
				  }
				  
				  interface Bear extends Animal {
				    honey: boolean;
				  }
				  
				  const bear = getBear();
				  bear.name;
				  bear.honey;
				  ```
			- *type*
				- ```ts
				  type Animal = {
				    name: string;
				  }
				  
				  type Bear = Animal & {
				    honey: boolean;
				  }
				  
				  const bear = getBear();
				  bear.name
				  bear.honey;
				  ```
		- ==Example (Adding new fields)==
			- *interface*
				- ```ts
				  interface Window {
				    title: string;
				  }
				  
				  interface Window {
				    ts: TypeScriptAPI;
				  }
				  
				  const src = 'const a = "Hello World"';
				  window.ts.transpileModule(src, {});
				  ```
			- *type*
				- #+BEGIN_WARNING
				  *type* **cannot** be changed after being created
				  #+END_WARNING
				- ```ts
				  A type cannot be changed after being created
				  
				  type Window = {
				    title: string;
				  }
				  
				  type Window = {
				    ts: TypeScriptAPI;
				  }
				  
				   // Error: Duplicate identifier 'Window'.
				  ```
		- #+BEGIN_NOTE
		  *type alias* might appear in [[#red]]==error messages==, while *interface* will always be named in error messages
		  #+END_NOTE
		- #+BEGIN_NOTE
		  *type alias* might not participate in **declaration merging** but interface can
		  #+END_NOTE
		- #+BEGIN_NOTE
		  *interface* may only be used to **declare the shapes of objects**, but not *rename primitives*
		  #+END_NOTE
			- ```ts
			  // Here's two examples of 
			  // using types and interfaces
			  // to describe an object 
			  
			  interface AnObject1 {
			      value: string
			  }
			  
			  type AnObject2 = {
			      value: string
			  }
			  
			  // Using type we can create custom names
			  // for existing primitives:
			  
			  type SanitizedString = string
			  type EvenNumber = number
			  
			  // This isn't feasible with interfaces
			  interface X extends string {
			  
			  }
			  /* An interface cannot extend a primitive type like 'string'; an interface can only extend named types and classes
			  'extends' clause of exported interface 'X' has or is using private name 'string'. */
			  ```
		- #+BEGIN_NOTE
		  *interface* names will always appear in their original form in error messages
		  #+END_NOTE
			- ```ts
			  interface Mammal {
			      name: string
			  }
			  
			  function echoMammal(m: Mammal) {
			      console.log(m.name)
			  }
			  
			  // e.g. The error below will always use the name Mammal 
			  // to refer to the type which is expected:
			  echoMammal({ name: 12343 })
			  
			  // The type of `m` here is the exact same as mammal,
			  // but as it's not been directly named, TypeScript
			  // won't mention it in the error messaging
			  
			  function echoAnimal(m: { name: string }) {
			      console.log(m.name)
			  }
			  
			  echoAnimal({ name: 12345 })
			  ```
	- #### Type Assertion #TypeScript/Type #[[[TypeScript/Type Assertion]]
		- Use to provide information that TypeScript can't know about
			- ```ts
			  const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
			  ```
		- #+BEGIN_NOTE
		  type assertions are removed by the compiler and won't affect the runtime behaviour of your code
		  #+END_NOTE
		- ==Angle-bracket alternative==
			- ```ts
			  const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas");
			  ```
		- #+BEGIN_WARNING
		  There won't be an exception or *null* generated if the type assertion is wrong
		  #+END_WARNING
		- TypeScript only allows type assertions which convert to a *more specific* or *less specific* version of type
			- ```ts
			  const x = "hello" as number;
			  // Conversion of type 'string' to type 'number' may be a mistake because neither type sufficiently overlaps with the other. If this was intentional, convert the expression to 'unknown' first.2352Conversion of type 'string' to type 'number' may be a mistake because neither type sufficiently overlaps with the other. If this was intentional, convert the expression to 'unknown' first.Try
			  
			  ```
			- tricks to bypass
				- ```ts
				  const a = (expr as any) as T;
				  ```
	- #### Literal Types #TypeScript/Type
		- how TypeScript creates types for literals is also apply with *var* and *let* that allow for changing what is held inside the variable
			- ```ts
			  let changingString = "Hello World";
			  changingString = "Olá";
			  // Because `changingString` can represent any possible string
			  changingString;
			  // hover: 'let changingString: string'
			  
			  const constantString = "Hello World";
			  // Because `constantString` can only represent 1 possible string, it
			  // has a litera type representation
			  constantString;
			  // hover: 'const constantString: "Hello World"'
			  ```
		- ==Combining literals==
			- string literals
				- ```ts
				  functoin printText(s: string, alignment: "left" | "right" | "center") {
				    // ...
				  }
				  printText("Hello, world", "left");
				  printText("G'day, mate", "centre");
				  // Argument of type '"centre"' is not assignable to parameter of type '"left" | "right" | "center"'.2345Argument of type '"centre"' is not assignable to parameter of type '"left" | "right" | "center"'
				  ```
			- numeric literals
				- ```ts
				  function compare(a: string, b: string): -1 | 0 | 1 {
				    return a === b ? 0 : a > b ? 1 : -1;
				  }
				  ```
			- non-literal types
				- ```ts
				  interface Options {
				    width: number;
				  }
				  function configure(x: Options | "auto") {
				    // ...
				  }
				  configure({width: 100 });
				  configure("auto");
				  configure("automatic");
				  // Argument of type '"automatic"' is not assignable to parameter of type 'Options | "auto"'.2345Argument of type '"automatic"' is not assignable to parameter of type 'Options | "auto"'
				  ```
			- #+BEGIN_NOTE
			  There's also a *boolean literals* with *boolean* represent an alias for the union *true | false*
			  #+END_NOTE
	- #### Literal Inference
		- When initialising a variable with an **object**, TypeScript assumes that the properties of that object might change values later
			- ```ts
			  const obj = { counter: 0 };
			  if (someCondition) {
			    obj.counter = 1;
			  }
			  ```
		- #+BEGIN_NOTE
		  TypeScript doesn't assume the assignment of *1* to a field which previously had *0* is an error.
		  *obj.counter* must have the type *number*, not *0*
		  It is because *types* are used to determine both *reading* and *writing* behaviour.
		  #+END_NOTE
		- error occur while passing the object to function
			- ```ts
			  declare function handleRequest(url: string, method: "GET" | "POST"): void;
			  
			  const req = { url: "https://example.com", method: "GET" };
			  handleRequest(req.url, req.method);
			  // Argument of type 'string' is not assignable to parameter of type '"GET" | "POST"'.2345Argument of type 'string' is not assignable to parameter of type '"GET" | "POST"'
			  ```
			- ==Work around 1 (adding type assertion):==
				- ```ts
				  // Change 1:
				  const req = { url: "https://example.com", method: "GET" as "GET" };
				  
				  // Change 2:
				  handleRequest(req.url, req.method as "GET");
				  ```
				- Change 1 means '`req.method` to always have the literal type `"GET"`', it can also prevent the possible assignment of "GUESS" to the field
				- Change 2 means '`req.method` may has the value `"GET"` for other reasons'
			- ==Work around 2 (convert to type literals):==
				- ```ts
				  const req = { url: "https://example.com", method: "GET" } as const;
				  handleRequest(req.url, req.method);
				  ```
				- The *as const* suffix acts like *const* but for the type system, it ensuring that all properties are assigned the **literal type** instead of *string* or *number*
	- #### `null` and `undefined` #TypeScript/Type
		- #+BEGIN_NOTE
		  In JavaScript, there is two primitive values to represent absent of value: *null* and *undefined*
		  
		  TypeScript has two corresponding *types* by the same names, both have behaviour depends on whether having the `strictNullChecks` option on
		  #+END_NOTE
	- #### `strictNullChecks no` #TypeScript/Type
		- With `strictNullChecks` on, when a value is *null* or *undefined*, you will need to test for those values before using methods or properties on that value
			- ```ts
			  function doSomething(x: string | null) {
			    if (x === null ) {
			      // do nothing
			    } else {
			      console.log("Hello, " + x.toUpperCase());
			    }
			  }
			  ```
	- #### Non-null Assertion Operator (Postfix `!`) #TypeScript/Type #TypeScript/null
		- Writing `!` after any expression is effectively a type assertion that the value isn't `null` or `undefined`:
			- ```ts
			  function liveDangerously(x: number | null | undefined) {
			    // No error
			    consolo.log(x!.toFixed());
			  }
			  ```
		- #+BEGIN_WARNING
		  This doesn't change the runtime behaviour of the code, it's important to **only** use `!` when the value you know *can't be* `null` or `undefined`
		  #+END_WARNING
	- #### Enums #TypeScript/Enum
		- *Enum* is added by TypeScript to allows for describing a value which could be one of a set of possible named constants
	- #### Less Common Primitives #[[TypeScript/Primitive Types]]
		- `bigint`
			- Introduced at *ES2020* onwards, used for very large integers, `BigInt`
			- ```ts
			  // Creating a bigint via the BigInt function
			  const oneHundred: bigint = BigInt(100);
			  
			  // Creating a BigInt via the literal syntax
			  const anotherHundred: bigint = 100n;
			  ```
		- `symbol`
			- used to create a globally unique reference, `Symbol()`
			- ```ts
			  const firstName = Symbol("name");
			  const secondName = Symbol("name");
			  
			  if (firstName === secondName) {
			    // This comparison appears to be unintentional because the types 'typeof firstName' and 'typeof secondName' have no overlap.2367This comparison appears to be unintentional because the types 'typeof firstName' and 'typeof secondName' have no overlap.
			    // Can't ever happen
			  }
			  ```
	-
- #### Section 3: Narrowing
  id:: 64912b20-5104-4737-887a-a7c4d1466a85
	- #### `typeof` type guards #[[TypeScript/Type Guard]] #TypeScript/Narrowing
		- `typeof` return a certain set of string:
			- |**Return**||
			  |--|--|
			  |`"string"`||
			  |`"number"`||
			  |`"bigint"`||
			  |`"symbol"`||
			  |`"undefined"`||
			  |`"object"`||
			  |`"function"`||
		- #+BEGIN_NOTE
		  In TypeScript, checking against the value returned by `typeof` is a type guard
		  #+END_NOTE
		- Notice that `typeof` doesn't return the string `null`
			- ```ts
			  function printAll(strs: string | string[] | null) {
			    if (typeof strs === "object") {
			      for (const s of strs) {
			  // 'strs' is possibly 'null'.18047'strs' is possibly 'null'.
			        console.log(s);
			      }
			    } else if (typeof strs === "string") {
			      console.log(strs);
			    } else {
			      // do nothing
			    }
			  }
			  ```
			- That's why we need **"truthiness" checking**
	- #### Truthiness narrowing #TypeScript/Narrowing #[[TypeScript/Truthiness]]
		- In JavaScript, the following values are all coerced to *false*
			- |**Values**||
			  |--|--|
			  |`0`||
			  |`NaN`||
			  |`""` (the empty string)||
			  |`On` (the `bigint` version of zero)||
			  |`null`||
			  |`undefined`||
		- `Boolean` function can coerced the value to type *boolean*
			- ```ts
			  // both these result in 'true'
			  Boolean("hello"); // type: boolean, value: true
			  !!"world"; // type: true, value: true
			  ```
			- This is used to added for type guarding against `null` or `undefined`
				- ```ts
				  function printAll(strs: string | string [] | null) { 
				    	// this got rid of the error: 'TypeError: null is not iterable'
				  	if (strs && typeof strs === "object") {
				        for (const s of strs) {
				          console.log(s);
				        } else if (typeof strs === "string") {
				          console.log(strs);
				        }
				      }
				  }
				  ```
			- #+BEGIN_WARNING
			  Don't do this !
			  #+END_WARNING
				- ```ts
				  function printAll(strs: string | string[] | null) {
				    // DON'T DO THIS !!!!
				    if (strs) {
				      if (typeof strs === "object") {
				        for (const s of strs) {
				          console.log(s);
				        }
				      } else if (typeof strs === "string") {
				        console.log(strs);
				      }
				    }
				  }
				  ```
				- There's a subtle downside: *we may no longer check for empty string case*
		- Boolean negations with `!`
			- it filter out from negated branches
			- ```ts
			  function multiplyAll(
			    values: number[] | undefined,
			    factor: number
			  ): number[] | undefined {
			    if (!values) {
			      return values;
			    } else {
			      return values.map((x) => x * factor);
			    }
			  }
			  ```
	- #### Equality narrowing #TypeScript/Narrowing #TypeScript/Equality
		- TypeScript uses *switch* statements and equality checks like `===`, `!==`, `==` and `!=`
			- ```ts
			  unction example(x: string | number, y: string | boolean) {
			    if (x === y) {
			      // We can now call any 'string' method on 'x' or 'y'.
			      x.toUpperCase();
			            (method) String.toUpperCase(): string
			      y.toLowerCase();
			            (method) String.toLowerCase(): string
			    } else {
			      console.log(x);
			                 (parameter) x: string | number
			      console.log(y);
			                 (parameter) y: string | boolean
			    }
			  }
			  ```
		- Effective way of correctly removes the `null` from the type of `strs`
			- ```ts
			  function printAll(strs: string | string[] | null) {
			    if (strs !== null) {
			      if (typeof strs === "object") {
			        for (const s of strs) {
			          			// (parameter) strs: string[]
			          console.log(s);
			        }
			      } else if (typeof strs === "string") {
			        console.log(strs);
			        				// (parameter) strs: string
			      }
			    }
			  }
			  ```
		- `== null` checks whether it's potentially `undefined` or `null`, the same applies to `== undefined`
			- ```ts
			  interface Container {
			    value: number | null | undefined;
			  }
			  
			  function mutiplyValue(container: Container, factor: number) {
			    // Remove both 'null' and 'undefined' from the type.
			    if (container.value != null) {
			      console.log(container.value);
			      						// (property) Container.value: number
			      // now we can safely multiply 'container.value'.
			      container.value *= factor;
			    }
			  }
			  ```
	- #### The `in` operator narrowing #TypeScript/Narrowing #[[TypeScript/Operator Narrowing]]
		- JavaScript has an operator for determining if an object has a property with a name
		- The **"true"** branch narrows *x*'s types which have either an optional or required property *value*
		- The **"false"** branch narrows to types which have an optional or missing property *value*
		- ```ts
		  type Fish = { swim: () => void };
		  type Bird = { fly: () => void };
		  
		  function move(animal: Fish | Bird) {
		    if ("swim" in animal) {
		      return animal.swim();
		    }
		    return animal.fly();
		  }
		  ```
		- ```ts
		  type Fish = { swim: () => void };
		  type Bird = { fly: () => void };
		  type Human = { swim?: () => void; fly?: () => void };
		  
		  function move(animal: Fish | Bird | Human) {
		    if ("swim" in animal) {
		      animal;
		      	// (parameter) animal: Fish | Human
		    } else {
		      animal;
		      	// (parameter) animal: Bird | Human
		    }
		  
		  }
		  ```
	- #### `instanceof` narrowing #TypeScript/Narrowing #[[TypeScript/Instance of Narrowing]]
		- `instanceof` check whether or not a value  is an "instance" of another value, more specifically, `x instanceof Foo` checks whether the *prototype* chain of x contains `Foo.prototype`
		- ==Example==
			- ```ts
			  function logValue(x: Date | string) {
			    if (x instanceof Date) {
			      console.log(x.toUTCString());
			      		// (parameter) x: Date
			    } else {
			      console.log(x.toUpperCase());
			      		// (parameter) x: string
			    }
			  }
			  ```
	- #### Assignments  #TypeScript/Narrowing
		- when we assign to any variable, TypeScript will looks at the right side of the assignment and *narrow* it down
		- ```ts
		  let x = Math.random() < 0.5 ? 10 : "Hello World!";
		  // let x: string | number
		  x = 1;
		  
		  console.log(x);
		  		// let x: number
		  x = "goodbye!";
		  
		  console.log(x);
		  		// let x: string
		  ```
		- #+BEGIN_NOTE
		  Notice that we were still able to assign a *string* to *x*. This is because the **declared type** of x is `string | number`, which is the type that *x* started with
		   to 
		  #+END_NOTE
		- If we'd assigned a `boolean` to *x* we'd have seen an error since that wasn't part of the **declared type**
		- ```ts
		  let x = Math.random() < 0.5 ? 10 : "hello world!";
		     let x: string | number
		  x = 1;
		   
		  console.log(x);
		             let x: number
		  x = true;
		  Type 'boolean' is not assignable to type 'string | number'.2322Type 'boolean' is not assignable to type 'string | number'.
		   
		  console.log(x);
		             let x: string | number
		  ```
	- #### Control flow analysis #TypeScript/Narrowing #[[TypeScript/Control Flow]]
		- The analysis of code based on reachability is called *control flow* analysis, TypeScript uses this flow analysis to narrow types as it encounters *type guards* and *assignments*
		- ==Example==
			- ```ts
			  function example() {
			    let x: string | number | boolean;
			   
			    x = Math.random() < 0.5;
			   
			    console.log(x);
			               let x: boolean
			   
			    if (Math.random() < 0.5) {
			      x = "hello";
			      console.log(x);
			                 let x: string
			    } else {
			      x = 100;
			      console.log(x);
			                 let x: number
			    }
			   
			    return x;
			          let x: string | number
			  }
			  ```
	- #### Using type predicates #[[TypeScript/Type Predicate]]
		- A predicate takes the form `parameterName is Type` where `parameterName` must be the name of a parameter from the current function signature