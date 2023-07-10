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
  collapsed:: true
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
  collapsed:: true
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
		- For defining a *user-defined type guard*, a predicate takes the form `parameterName is Type` where `parameterName` must be the name of a parameter from the current function signature
		- ==Example==
			- ```ts
			  function isFish(pet: Fish | Bird): pet is Fish {
			    return (pet as Fish).swim !== undefined;
			  }
			  ```
			- ```ts
			  const zoo: (Fish | Bird) [] = [getSmallPet(), getSmallPet(), getSmallPet()];
			  const underWater1: Fish[] = zoo.filter(isFish);
			  // or, equivalently
			  const underWater2: Fish[] = zoo.filter(isFish) as Fish[];
			  
			  // more complex examples
			  const underWater3: Fish[] = zoo.filter((pet): pet is Fish => {
			  	if (pet.name === "sharkey") return false;
			  	return isFish(pet);
			  });
			  ```
		- #+BEGIN_NOTE
		  classes can also use [`this is Type`](https://www.typescriptlang.org/docs/handbook/2/classes.html#this-based-type-guards) to narrow their type 
		  #+END_NOTE
	- #### Assertion functions #TypeScript/Narrowing
		- Types can also be narrowed using [Assertion function](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#assertion-functions)
	- #### Discriminated unions #TypeScript/Narrowing #Typescript/Interface #TypeScript/Discrimination
		- Most of the time in JavaScript we'll be dealing with slightly more complex structure
		- ==Example==
			- ```ts
			  // attempt 1
			  interface Shape {
			    kind: "circle" | "square";
			    radius?: number;
			    sideLength?: number;
			  }
			  ```
			- ```ts
			  function getArea(shape: Shape) {
			    if (shape.kind === "circle") {
			      return Math.PI * shape.radius ** 2;
			      // 'shape.radius' is possibly 'undefined'.18048'shape.radius' is possibly 'undefined'.
			    }
			  }
			  ```
			- Since the type checkers doesn't know that radius must be exists on circle (which we didn't provide sufficient information)
			- ```ts
			  // attempt 2
			  interface Circle {
			    kind: "circle";
			    radius: number;
			  }
			  
			  interface Square {
			    kind: "square";
			    sideLength: number;
			  }
			  
			  type Shape = Circle | Square;
			  ```
			- ```ts
			  function getArea(shape: Shape) {
			    if (shape.kind === "circle") {
			      return Math.PI * shape.radius ** 2;
			      				// (parameter) shape: Circle
			    }
			  }
			  ```
			- This got rid of the error
			- #+BEGIN_NOTE
			  When every type in a union contains a common property with literal types, TypeScript considers that to be a **discriminated union**, and can *narrow* out the members of the union
			  #+END_NOTE
			- In this case, `kind` was that common property (which considered a **discriminant** property of *Shape*). Which narrowed `shape` down to the type `Circle`
			- ==Switch case example==
				- ```ts
				  function getArea(shape: Shape) {
				    switch (shape.kind) {
				      case "circle":
				        return Math.PI * shape.radius ** 2;
				        				// (parameter) shape: Circle
				      case "square":
				        return shape.sideLength ** 2;
				        	// (parameter) shape: Square
				    }
				  }
				  ```
	- #### The `never` type #TypeScript/Never #TypeScript/Narrowing
		- Whenever you narrowing of union, which have removed all possibilities and have nothing left. TypeScript will use a `never` type to represent
	- #### Exhaustiveness checking #TypeScript/Narrowing #TypeScript/Never
		- We can use `never` to do exhaustive checking
		- ==Example==
			- ```ts
			  type Shape = Circle | Square;
			  
			  function getArea(shape: Shape) {
			    switch (shape.kind) {
			      case "circle":
			        return Math.PI * shape.radius ** 2;
			      case "square":
			        return shape.sideLength ** 2;
			      default:
			        const _exhaustiveCheck: never = shape;
			        return _exhaustiveCheck;
			    }
			  }
			  ```
			- Adding a new member (which narrow in the return *never* in switch statement)to the `Shape` union will cause an error.
				- ```ts
				  interface Triangle {
				    kind: "triangle";
				    sideLength: number;
				  }
				  
				  type Shape = Circle | Square | Triangle
				  
				  function getArea(shape: Shape) {
				    switch (shape.kind) {
				      case "circle":
				        return Math.PI * shape.radius ** 2;
				      case "square":
				        return shape.sideLength ** 2;
				      default:
				        const _exhaustiveCheck: never = shape;
				        return _exhaustiveCheck;
				        /* Type 'Triangle' is not assignable to type 'never'.2322Type 'Triangle' is not assignable to type 'never'.
				        return _exhaustiveCheck; */
				  }
				  ```
- #### Section 4: More on Functions
  id:: 64927755-385c-43d8-891e-da43e66063ea
  collapsed:: true
	- #### Function Type Expression #TypeScript/Function #TypeScript/Type
		- ```ts
		  function greeter(fn: (a: string) => void) {
		    fn("Hello, World");
		  }
		  
		  function printToConsole(s: string) {
		    console.log(s);
		  }
		  
		  greeter(printToConsole);
		  ```
		- we can use a type alias to name a function type
			- ```ts
			  type GreetFunction = (a: string) => void;
			  function greeter(fn: GreetFunction) {
			    // ...
			  }
			  ```
	- #### Call Signatures #TypeScript/Type #TypeScript/Function
		- The function type expression syntax doesn't allow for declaring properties like in JavaScript (functions can have properties in addition to being callable)
		- we can describe this kind of callable (with properties) with `call signature`
			- ```ts
			  type DescribableFunction = {
			    description: string;
			    (someArg: number): boolean;
			  };
			  
			  function doSomething(fn: DescribableFunction) {
			    console.log(fn.description + " returned " + fn(6));
			  }
			  
			  function myFunc(someArg: number) {
			    return someArg > 3;
			  }
			  
			  myFunc.description = "default description";
			  
			  doSomething(myFunc);
			  ```
	- #### Construct Signatures #TypeScript/Function #TypeScript/Type
		- In JavaScript, functions can be invoked with the `new` keyword, TypeScript *refers* to these as *constructor* because they created a new object
		- In TypeScript, we can add the `new` keyword in front of a call signature to write a `construct signature`
			- ```ts
			  type SomeConstructor = {
			    new (s: string): SomeObject;
			  };
			  
			  function fn(ctor: SomeConstructor) {
			    return new ctor("hello");
			  }
			  ```
		- `Date` object example
			- ```ts
			  interface CallOrConstruct {
			    new (s: string): Date;
			    (n?: number): string;
			  }
			  ```
	- #### Generic Functions #TypeScript/Type #TypeScript/Function #TypeScript/Generics
		- In TypeScript, *generics* are used when we want to describe a correspondence between two values
		- By declaring a *type* parameter in the function signature
			- ```ts
			  function firstElement<Type>(arr: Type[]): Type | undefined {
			    return arr[0];
			  }
			  ```
		- After adding a type parameter `Type` to this function, the link between the input and the output of the function are created. Whenever we call it, a more specific type comes out
			- ```ts
			  // s is of type 'string'
			  const s = firstElement(["a", "b", "c"]);
			  // n is of type 'number'
			  const n = firstElement([1, 2, 3]);
			  // u is of type undefined
			  const u = firstElement([]);
			  ```
	- #### Inference #TypeScript/Type #TypeScript/Generics
		- We don't have to specify `Type`, the type was *inferred* by TypeScript
		- multiple type parameters example
			- ```ts
			  function map<Input, Output>(arr: Input[], func: (arg: Input => Output): Output[] {
			    return arr.map(func);
			  })
			  
			  // Parameter 'n' is of type 'string'
			  // 'parsed' is of type 'number[]'
			  const parsed = map(["1", "2", "3"], (n) => parseInt(n));
			  // In this case, type `Input`: "string", type `Output`: "number"
			  ```
	- #### Constraint #TypeScript/Generics
		- To limit the functions on a certain subset of *values*, we *constrain* the type parameter to that **type** by writing an `extends` clause
			- ==Example==
				- ```ts
				  function longest<Type extends { length: number }>(a: Type, b: Type) {
				    if (a.length > b.length) {
				      return a;
				    } else {
				      return b;
				    }
				  }
				  
				  // longerArray is of type 'number[]'
				  const longerArray = longest([1, 2], [1, 2, 3]);
				  
				  // longerString is of type 'alice' | 'bob'
				  const longerString = longest("alice", "bob");
				  
				  // Error! Numers don't have a 'length' property
				  const notOK = longest(10, 100);
				  // Argument of type 'number' is not assignable to parameter of type '{ length: number; }'.2345Argument of type 'number' is not assignable to parameter of type '{ length: number; }'.
				  ```
				- Because we *constrained* `Type` to `{ length: number }`, we allowed to access the `.length` property of the `a` and `b` parameters
			- [[#red]]==Wrong example==
				- ```ts
				  function minimumLength<Type extends { length: number }>(
				    obj: Type,
				    minimum: number
				  ): Type {
				    if (obj.length >= minimum) {
				      return obj;
				    } else {
				      return { length: minimum };
				  /*Type '{ length: number; }' is not assignable to type 'Type'.
				    '{ length: number; }' is assignable to the constraint of type 'Type', but 'Type' could be instantiated with a different subtype of constraint '{ length: number; }'.2322Type '{ length: number; }' is not assignable to type 'Type'.
				    '{ length: number; }' is assignable to the constraint of type 'Type', but 'Type' could be instantiated with a different subtype of constraint '{ length: number; }'.
				    }
				    */
				  } 
				  ```
				- The problem in this function is that it promises to return the **same** kind of object as was passed in, not just some object that *match* the constraint
	- #### Manually specifying the type #TypeScript/Type #TypeScript/Generics
		- Sometimes TypeScript can't infer the intended type arguments in a generic call
			- ```ts
			  function combine<Type>(arr1: Type[], arr2: Type[]): Type[] {
			    return arr1.concat(arr2);
			  }
			  
			  const arr = combine([1, 2, 3], ["hello"]);
			  // Type 'string' is not assignable to type 'number'.2322Type 'string' is not assignable to type 'number'.
			  ```
		- We have to manually **specify the type**
			- ```ts
			  const arr = combine<string | number>([1, 2, 3], ["hello"]);
			  ```
	- #### Writing Good Generics function #TypeScript/Type #TypeScript/Generics
		- ==Example 1==
			- ```ts
			  function firstElement1<Type>(arr: Type[]) {
			    return arr[0];
			  }
			  
			  function firstElement2<Type extends any[]>(arr: Type) {
			    return arr[0];
			  }
			  
			  // a: number (pog)
			  const a = firstElement1([1, 2, 3]);
			  // b: any (deadge)
			  const b = firstElement2([1, 2, 3]);
			  ```
			- The `firstElement2`'s inferred return type is `any` because TypeScript has to resolve the `arr[0]` expression using the constraint type
		- ==Example 2==
			- ```ts
			  function filter1<Type>(arr: Type[], func: (arg: Type) => boolean): Type[] {
			    return arr.filter(func)
			  }
			  
			  function filter2<Type, Func extends (args: Type) => boolean>(
			  	arr: Type[],
			      func: Func
			  ): Type[] {
			      return arr.filter(func);
			  }
			  ```
			- the type parameter `Func` *doesn't relate two values*, the callers need to specify type arguments manually, `Func` doesn't do anything
		- ==Example 3==
			- ```ts
			  // bro why
			  function greet<Str extends string>(s: Str) {
			    console.log("Hello, " + s);
			  }
			  
			  greet("world");
			  
			  // do this bro
			  function greet(s: string) {
			    console.log("Hello, " + s);
			  }
			  ```
		- #+BEGIN_NOTE
		  type parameters are for relating the types of multiple values, *strongly reconsider* if you actually need it
		  #+END_NOTE
	- #### More about optional #TypeScript/Function #TypeScript/Optional
		- TypeScript can also take a variable number of arguments by using *optional*
			- ```ts
			  function f(x?: number) {
			    // ...
			  }
			  
			  f(); // OK
			  f(10); // OK
			  ```
		- Although the parameter is specified as type `number`, the `x` parameter actually have the type `number | undefined`
		- A *default* value can also be set
			- ```ts
			  function f(x = 10) {
			    // ...
			  }
			  ```
		- [[#red]]==Common mistake==
			- ```ts
			  function myForEach(arr: any[], callback: (arg: any, index?: number) => void) {
			    for (let i = 0; i < arr.length; i++) {
			      callback(arr[i], i);
			    }
			  }
			  
			  myForEach([1, 2, 3], (a, i) => {
			    console.log(i.toFixed());
			    // 'i' is possibly 'undefined'.18048'i' is possibly 'undefined'.
			  })
			  ```
	- #### Overload signatures #TypeScript/Function
		- In TypeScript, we can specify a function that can be called in different ways by writing *overload signatures*
		- ==Examples==
			- ```ts
			  function makeDate(timestamp: number): Date;
			  function makeDate(m: number, d: number, y: number): Date;
			  function makeDate(mOrTimestamp: number, d?: number, y?: number): Date {
			    if (d !== undefined && y !== undefined) {
			      return new Date(y, mOrTimestamp, d);
			    } else {
			      return new Date(mOrTimestamp);
			    }
			  }
			  
			  const d1 = makeDate(12345678);
			  const d2 = makeDate(5, 5, 5);
			  const d3 = makeDate(1, 3);
			  // No overload expects 2 arguments, but overloads do exist that expect either 1 or 3 arguments.2575No overload expects 2 arguments, but overloads do exist that expect either 1 or 3 arguments.
			  ```
		- The first two signatures are called the *overload signatures*, after we have an *implementation signature*, but this signature can't be called directly
			- ```ts
			  function fn(x: string): void;
			  
			  function fn() {
			    // ...
			  }
			  
			  // Expected to be able to with zero arguments
			  fn();
			  // Expected 1 arguments, but got 0
			  ```
		- #+BEGIN_NOTE
		  The signature of the implementation is not visible from the outside. When writing an overload function, you should always have two or more signatures above the implementation of the function
		  #+END_NOTE
		- The implementation signature must also be **compatible** with the *overload signatures*
		- ==Example==
			- ```ts
			  function fn(x: boolean): void;
			  // Argument isn't right
			  function fn(x: string): void;
			  // This overload signature is not compatible with its implementation signature.
			  function fn(x: boolean) {}
			  ```
			- ```ts
			  function fn(x: string): string;
			  // Return type isn't right
			  function fn(x: number): boolean;
			  // This overload signature is not compatible with its implementation signature.
			  function fn(x: string | number) {
			    return "oops";
			  }
			  ```
		- ##### Guidelines of using function overload
			- Consider a function that return the length of a `string` or an `array`:
				- ```ts
				  function len(s: string): number;
				  function len(arr: any[]): number;
				  function len(x: any) {
				    return x.length;
				  }
				  ```
				- ```ts
				  len(""); // OK
				  len([0]); // OK
				  len(Math.random() > 0.5 ? "hello" : [0]);
				  /* Overload 1 of 2, '(s: string): number', gave the following error.
				      Argument of type 'number[] | "hello"' is not assignable to parameter of type 'string'.
				        Type 'number[]' is not assignable to type 'string'.
				    Overload 2 of 2, '(arr: any[]): number', gave the following error.
				      Argument of type 'number[] | "hello"' is not assignable to parameter of type 'any[]'.
				        Type 'string' is not assignable to type 'any[]'. */
				  ```
			- Because both overloads have the same argument count and same return type, we can *instead* write a non-overloaded version of the function
				- ```ts
				  function len(x: any[] | string) {
				    return x.length;
				  }
				  ```
			- #+BEGIN_NOTE
			  Always prefer parameters with union types instead of overloads when possible
			  #+END_NOTE
	- #### Infer `this` #TypeScript/Function #TypeScript/Scope
		- TypeScript will infer what the `this` should be in a function
			- ```ts
			  const user = {
			    id: 123,
			    
			    admin: false,
			    becomeAdmin: function () {
			      this.admin = true;
			    }
			  }
			  ```
		- TypeScript uses that syntax space to let you declare the type for `this` in the function body
			- ```ts
			  interface DB {
			    filterUsers(filter: (this: User) => boolean): User[];
			  }
			  
			  const db = getDB();
			  const admins = db.filterUsers(function (this: User) {
			    return this.admin;
			  });
			  
			  const adminbro = db.filterUsers(() => this.admin)
			  /*
			  The containing arrow function captures the global value of 'this'.
			  Element implicitly has an 'any' type because type 'typeof globalThis' has no index signature.
			  */
			  ```
			- #+BEGIN_NOTE
			  arrow function can't get this behavior, use `function` if you want to have `this` as a parameter
			  #+END_NOTE
	- #### Return value #TypeScript/Function
		- In Typescript, `void` represent any time a function doesn't have any `return` statement, *or* doesn't return any explicit value from those return statements
		- However in JavaScript, a function that doesn't return any value will implicitly return the value `undefined`
		- #+BEGIN_WARNING
		  `void` is **not** the same as `undefined`
		  #+END_WARNING
	- #### Type `object` #TypeScript/Type
		- The special type `object` isn't a primitive (`string`, `number`, `bigint`, `boolean`, `symbol`, `null` or `undefined`)
		- #+BEGIN_NOTE
		  This is different from the *empty object* type `{ }`, also different from the global type `Object`
		  
		  **Always use `object`!!!**
		  #+END_NOTE
	- #### Type `unknown` #TypeScript/Type
		- The `unknown` type represents *any* value. Similar to `any` type, but is rather safer because it's not legal to **do anything** with an `unknown` value
			- ```ts
			  function f1(a: any) {
			    a.b(); // OK
			  }
			  
			  function f2(a: unknown) {
			    a.b();
			    // 'a' is of type 'unknown'.
			  }
			  ```
		- This is used for describing function type without having `any` values in the function body
			- ```ts
			  function safeParse(s: string): unknown {
			    return JSON.parse(a);
			  }
			  
			  // Need to be careful with `obj`
			  const obj = safeParse(someRandomString);
			  ```
	- #### Type `never` #TypeScript/Type #TypeScript/Never
		- some function **never** return a value
			- ```ts
			  function fail(msg: string): never {
			    throw new Error(msg);
			  }
			  ```
		- The `never` type represents values which are *never* observed. In this case mean the function throw an exception or terminates execution of the program
		- `never` also appears when TypeScript determines there's nothing left in a *union*
			- ```ts
			  function fn(x: string | number) {
			    if (typeof x === "string") {
			      // do something
			    } else if (typeof x === "number") {
			      // do something else
			    } else {
			      x; // has type 'never';
			    }
			  }
			  ```
	- #### Type `Function` #TypeScript/Type #TypeScript/Function
		- The global type `Function` describes properties like `bind`, `call`, `apply` and others present on all function values in JavaScript
			- ```ts
			  function doSomething(f: Function) {
			    return f(1, 2, 3);
			  }
			  ```
			- #+BEGIN_WARNING
			  This is an *untyped function call* having a unsafe `any` return type, it is best to avoid
			  #+END_WARNING
			- The type `() => void` is generally safer
	- #### Rest parameter #TypeScript/Function
		- Besides using optional parameters or overloads to make functions that can accept a variety of fixed argument counts
		- *rest parameters* can take an *unbound* number of arguments
		- *rest parameters* appears after all other parameters
			- ```ts
			  function multiply(n: number, ...m: number[]) {
			    return m.map((x) => n * x);
			  }
			  
			  // 'a' gets value [10, 20, 30, 40]
			  const a = multiply(10, 1, 2, 3, 4);
			  ```
	- #### Spread operator #TypeScript/Function
		- *spread syntax* can be used to *provide* a variable number of arguments from an iterable object
			- ```ts
			  const arr1 = [1, 2, 3];
			  const arr2 = [4, 5, 6];
			  arr1.push(...arr2);
			  ```
		- TypeScript does not assume that array are immutable
			- ```ts
			  // Inferred type is number[] -- "an array with zero or more numbers",
			  // not specifically two numbers
			  const args = [8, 5];
			  const angle = Math.atan2(...args);
			  // A spread argument must either have a tuple type or be passed to a rest parameter.
			  ```
		- fix:
			- ```ts
			  // Inferred as 2-length tuple
			  const args = [8, 5] as const;
			  // OK
			  const angle = Math.atan2(...args);
			  ```
	- #### Object destructuring #TypeScript/Function #TypeScript/object
		- In TypeScript, *object destructuring* can be used to unpack objets provided as an argument into one or more local variables in the function body
			- ```ts
			  function sum({ a, b, c }: { a: number; b: number; c: number }) {
			    console.log(a + b + c);
			  }
			  ```
		- can be used with named type as well
			- ```ts
			  // Same as prior example
			  type ABC = { a: number; b: number; c: number};
			  function sum({ a, b, c}: ABC) {
			    console.log(a + b + c);
			  }
			  ```
			-
	- #### Type `void` #TypeScript/Type #TypeScript/Function
		- The `void` return type for function can sometimes produce some unusual, but expected behavior
		- `void` return type `(type voidFunc = () => void)`, when implemented, can return *any* other value, but it will be ignored
		- The following implementations of the type `() => void` is valid
			- ```ts
			  type voidFunc = () => void;
			  
			  const f1: voidFunc = () => {
			    return true;
			  }
			  
			  const f2: voidFunc = () => true;
			  
			  const f3: voidFunc = function () {
			    return true;
			  }
			  
			  const v1 = f1();
			  const v2 = f2();
			  const v3 = f3();
			  
			  // it will all return void
			  ```
		- This behavior exists to make the following code is valid (`Array.prototype.push` return a number and the `Array.prototype.forEach` method expects a function with a return type of `void`)
			- ```ts
			  const src = [1, 2, 3];
			  const dst = [0];
			  
			  src.forEach((el) => dst.push(el))
			  ```
		- #+BEGIN_NOTE
		  There is one special case, when a literal function definition has a `void` return type, the function must **not** return anything
		  #+END_NOTE
			- ```ts
			  function f2(): void {
			    // @ts-expect-error
			    return true;
			  }
			  
			  const f3 = function (): void {
			    // @ts-expect-error
			    return true;
			  }
			  ```
- #### Section 5: Object Types
  id:: 649bd061-9cee-438d-8a74-3755f13c1d4e
  collapsed:: true
	- #### Object type refresher #TypeScript/Type #TypeScript/object #[[TypeScript/Type Alias]] #Typescript/Interface
		- In JavaScript, the fundamental way that we group and pass around data is through objects. In TypeScript, we represent those through *object types*
		- *anonymous object*
			- ```ts
			  function greet(person: { name: string; age: number }) {
			    return "Hello " + person.name;
			  }
			  ```
		- *object type by using interface*
			- ```ts
			  interface Person {
			    name: string;
			    age: number;
			  }
			  
			  function greet(person: Person) {
			    return "Hello " + person.name;
			  }
			  ```
		- *object type by using type alias*
			- ```ts
			  type Person = {
			    name: string;
			    age: number;
			  }
			  
			  function greet(person: Person) {
			    return "Hello " + person.name;
			  }
			  ```
	- #### Property Modifier #TypeScript/object
		- each property in an object type can specify a couple of things:
			- the type 
			  logseq.order-list-type:: number
			- whether the property is optional
			  logseq.order-list-type:: number
			- whether the property can be written to
			  logseq.order-list-type:: number
	- #### Optional Properties #TypeScript/object #TypeScript/Optional
		- for objects that *might* have a property set. those properties can mark as *optional* by adding `?`
			- ```ts
			  interface PaintOptions {
			    shape: Shape;
			    xPos?: number;
			    yPos?: number;
			  }
			  
			  function paintShape(opts: PaintOptions) {
			    // ...
			  }
			  
			  const shape = getShape();
			  paintShape({ shape });
			  paintShape({ shape, xPos: 100 });
			  paintShape({ shape, yPos: 100 });
			  paintShape({ shape, xPos: 100, yPos: 100 });
			  ```
		- when we do under `strictNullChecks`, TypeScript will tell us they're potentially `undefined`
			- ```ts
			  function paintShape(opts: PaintOptions) {
			    let xPos = opts.xPos;
			                     (property) PaintOptions.xPos?: number | undefined
			    let yPos = opts.yPos;
			                     (property) PaintOptions.yPos?: number | undefined
			    // ...
			  }
			  ```
		- even if the property is not set, we can still access it (`undefined`)
			- ```ts
			  function paintShape(opts: PaintOptions) {
			    let xPos = opts.xPos === undefined ? 0 : opts.xPos;
			    // let xPos: number
			    let yPos = opts.yPos === undefined ? 0 : opts.yPos;
			    // let yPos: number
			  }
			  ```
		- setting defaults
			- ```ts
			  function paintShape({ shape, xPos = 0, yPos = 0 }: PaintOptions) {
			    console.log("x coordinate at", xPos);
			    								// (parameter) xPos: number
			    console.log("y coordinate at", yPos);
			    								// (parameter) yPos: number
			  }
			  ```
			- #+BEGIN_NOTE
			  There is currently no way to place **type annotations** within *destructuring patterns*
			  #+END_NOTE
				- ```ts
				  function draw({ shape: Shape, xPos: number = 100 /*...*/ }) {
				    render(shape);
				  // Cannot find name 'shape'. Did you mean 'Shape'?
				    render(xPos);
				  // Cannot find name 'xPos'.
				  }
				  ```
				- In the object *destructuring pattern*, `shape: Shape` means "grab the property `shape` and redefine it locally as a variable named `Shape`
	- #### `readonly` Properties #TypeScript/object
		- Properties can be marked as `readonly`, although it cannot change any behavior during runtime, a property marked as `readonly` can't be written to during *type-checking*
			- ```ts
			  interface SomeType {
			    readonly prop: string;
			  }
			  
			  function doSomething(obj: SomeType) {
			    // reading value
			   	console.log(`prop has the value '${obj.prop}'.`);
			    
			    // trying to re-assign it
			  	obj.prop = "hello";
			    // Cannot assign to 'prop' because it is a read-only property.
			  }
			  ```
		- `readonly` modifier doesn't necessarily imply that a value is totally immutable,  it just means the property itself can't be re-written to
			- ```ts
			  interface Home {
			    readonly resident: { name: string; age: number };
			  }
			  
			  function visitForBirthday(home: Home) {
			    // we can read and update properties from 'home.resident'
			    console.log(`Happy birthday ${home.resident.name}!`);
			    home.resident.age++;
			  }
			  
			  function evict(home: Home) {
			    home.resident = {
			      // Cannot assign to 'resident' because it is a read-only property.
			      name: "Victor the Evictor",
			      age: 42,
			    };
			  }
			  ```
		- TypeScript doesn't factor in whether properties on two types are `readonly` when checking whether those types are compatible
		- `readonly`
			- ```ts
			  interface Person {
			    name: string;
			    age: number;
			  }
			  
			  interface ReadonlyPerson {
			    readonly name: string;
			    readonly age: number;
			  }
			  
			  let writablePerson: Person = {
			    name: "Person McPersonface",
			    age: 42,
			  };
			  
			  // works
			  let readonlTyPerson: ReadonlyPerson = writablePerson;
			  
			  console.log(readonlyPerson.age); // prints '42'
			  writablePerson.age++
			  console.log(readonlyPerson.age); // prints '43'
			  ```
	- #### Index Signatures #TypeScript/Never
		- Index signatures can be used for placeholder, for no knowing the *name* of a type's properties, but only the *shape* of the value
		- ```ts
		  interface StringArray {
		    [index: number]: string;
		  }
		  
		  const myArray: StringArray = getStringArray();
		  const secondItem = myArray[1];
		  		//  const secondItem: string
		  ```
		- Only some types are allowed: `string`, `number`, `symbol`
		- Index signatures also enforce that all properties match their return type
			- ```ts
			  interface NumberDictionary {
			    [index: string]: number;
			    
			    length: number; // ok
			    name: string;
			    // Property 'name' of type 'string' is not assignable to 'string' index type 'number'.
			  }
			  ```
		- Use union of the property types to accept different types
			- ```ts
			  interface NumberOfStringDictionary {
			    [index: string]: number | string;
			    length: number; // ok, length is a number
			    name: string; // ok, name is a string
			  }
			  ```
		- Adding `readonly` to prevent assignment to their indices
			- ```ts
			  interface ReadonlyStringArray {
			    readonly [index: number]: string;
			  }
			  
			  let myArray: ReadonlyStringArray = getReadOnlyStringArray();
			  myArray[2] = "Mallory";
			  // Index signature in type 'ReadonlyStringArray' only permits reading.
			  ```
	- #### Excess property checking #TypeScript/object
		- ```ts
		  interface SquareConfig {
		    color?: string;
		    width?: number;
		  }
		  
		  function createSquare(config: SquareConfig): { color: string; area: number } {
		    return {
		      color: config.color || "red",
		      area: config.width ? config.width * config.width : 20,
		    };
		  }
		  
		  let mySquare = createSquare({ colour: "red", width: 100 });
		  /*
		  Argument of type '{ colour: string; width: number; }' is not assignable to parameter of type 'SquareConfig'.
		    Object literal may only specify known properties, but 'colour' does not exist in type 'SquareConfig'. Did you mean to write 'color'
		  */
		  ```
		- In TypeScript, Object literals get special treatment and undergo *excess property checking* when assigning them to other variables
		- if an object literal has any properties that the **"target type"** doesn't have, you'll get an error
		- We can get around these check with *type assertion*
			- ```ts
			  let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);
			  ```
		- Another approach is using *string index signature*
			- ```ts
			  interface SquareConfig {
			    color?: string;
			    width?: number;
			    [propName: string]: any;
			  }
			  ```
		- Hacky way
			- ```ts
			  let squareOptions = { colour: "red", width: 100 };
			  let mySquare = createSquare(squareOptions);
			  ```
			- assigning the object to another variable prevent undergo excess property checks
		- Above approach works as long as it have a common property to the type, if not it will fail
			- ```ts
			  let squareOptions = { colour: "red" };
			  let mySquare = createSquare(squareOptions);
			  // Type '{ colour: string; }' has no properties in common with type 'SquareConfig'.
			  ```
	- #### Extends #TypeScript/object #TypeScript/extends
		- for example if we want different version of the type, in this case ,more specific one, we can *extend* the original type
			- ```ts
			  interface BasicAddress {
			    name?: string;
			    street: string;
			    city: string;
			    country: string;
			    postalCode: string;
			  }
			  
			  interface AddressWithUnit extends BasicAddress {
			    unit: string;
			  }
			  ```
		- ==Multiple extends example==
			- ```ts
			  interface Colorful {
			    color: string;
			  }
			  
			  interface Circle {
			    radius: number;
			  }
			  
			  interface ColorfulCircle extends Colorful, Circle {}
			  
			  const cc: ColorfulCircle = {
			    color: "red",
			    radius: 42,
			  }
			  ```
	- #### Intersection #TypeScript/object #TypeScript/intersection
		- TypeScript provides another construct call *intersection types* to combine existing object types
			- ==With interfaces==
			- ```ts
			  interface Colorful {
			    color: string;
			  }
			  interface Circle {
			    radius: number;
			  }
			  
			  type ColorfulCircle = Colorful & Circle;
			  ```
			- ==With type alias==
			- ```ts
			  function draw(circle: Colorful & Circle) {
			    console.log(`Color was ${circle.color}`);
			    console.log(`Radius was ${circle.radius}`);
			  }
			  
			  // okay
			  draw({ color: "blue", radius: 42 });
			  
			  // bro 
			  draw({ color: "red", raidus: 42 });
			  
			  /*
			  Argument of type '{ color: string; raidus: number; }' is not assignable to parameter of type 'Colorful & Circle'.
			    Object literal may only specify known properties, but 'raidus' does not exist in type 'Colorful & Circle'. Did you mean to write 'radius'?
			  */
			  
			  
			  ```
	- #### Object generic type #TypeScript/object #TypeScript/Generics
		- ```ts
		  interface Box {
		    contents: any;
		  }
		  
		  let x: Box = {
		    contents: "hello world",
		  };
		  
		  // we could check 'x.contents'
		  if (typeof x.contents === "string") {
		    console.log(x.contents.toLowerCase());
		  }
		  
		  // or we could use a type assertion
		  console.log((x.contents as string).toLowerCase());
		  ```
		- Generic for having different `Box` types for every type of `contents`
		- ```ts
		  interface Box<Type> {
		    content: Type;
		  };
		  
		  type Box<Type> = {
		    contents: Type;
		  };
		  ```
		- Using *type aliases* to write generic helper types
			- ```ts
			  type OrNull<Type> = Type | null;
			  
			  type OneOrMany<Type> = Type | Type[];
			  
			  type OneOrManyOrNull<Type> = OrNull<OneOrMany<Type>>;
			  		// type OneOrManyOrNull<Type> = OneOrMany<Type> | null
			  type OneOrManyOrNullStrings = OneOrManyOrNull<string>;
			  			// type OneOrManyOrNullStrings = OneOrMany<string> | null
			  ```
		- Common example (Array)
			- ```ts
			  function doSomething(value: Array<string>) {
			    // ...
			  }
			  
			  let myArray: string[] = ["hello", "world"];
			  
			  // either of these work!
			  doSomething(myArray);
			  doSomething(new Array("hello", "world"));
			  ```
		- #+BEGIN_NOTE
		  JavaScript provides many other data structures which are generic, like `Map<K, V>`, `Set<T>`, and `Promise<T>`.
		  #+END_NOTE
	- #### `ReadonlyArray` #TypeScript/object #TypeScript/Generics #TypeScript/Array
		- The `ReadonlyArray` is a special type that describes arrays that shouldn't be changed
			- function doStuff(values: ReadonlyArray<string>) {
			    // We can read from 'values' ...
			    const copy = values.slice();
			    console.log(`The first value is ${values[0]}`);
			    
			    // ...but we can't mutate 'values'.
			    values.push("hello!");
			    // Property 'push' does not exist on type 'readonly string[]'.
			  }
		- #+BEGIN_WARNING
		  Unlike `Array`, there isn't a `ReadonlyArray` constructor that can use
		  #+END_WARNING
			- ```ts
			  new ReadonlyArray("red", "green", "blue");
			  // 'ReadonlyArray' only refers to a type, but is being used as a value here.
			  
			  
			  // we can assign the regular array to `ReadonlyArray`
			  const roArray: ReadonlyArray<string> = ["red", "green", "blue"];
			  ```
		- Just like `Type[]`, TypeScript provides a shorthand for `ReadonlyArray<Type>` with `readonly Type[]`
			- ```ts
			  function doStuff(values: readonly string[]) {
			    // We can read from 'values' ...
			    const copy = value.slice();
			    console.log(`The first value is ${values[0]}`);
			    
			    // can't mutate 'values'.
			    values.push("hello!");
			    // Property 'push' does not exist on type 'readonly string[]'.
			  }
			  ```
		- `readonly`'s assignability isn't bidirectional between regular `Array` and `ReadonlyArray`
			- ```ts
			  let x: readonly string[] = [];
			  let y: string[] = [];
			  
			  x = y;
			  y = x;
			  
			  // The type 'readonly string[]' is 'readonly' and cannot be assigned to the mutable type 'string[]'.
			  ```
	- #### Tuple Type #TypeScript/Type #TypeScript/Array
		- A `tuple` *type* is another `Array` that knows exactly know how many *elements* it contain, exactly *which types* it contains at **specific position**
			- ```ts
			  type StringNumberPair = [string, number];
			  ```
		- It describes arrays whose `0` index contains a `string` and whose `1` index contains a `number`
			- ```ts
			  function doSomething(pair: [string, number]) {
			    const a = pair[0];
			    	// const a: string
			    const b = pair[1];
			    	// const b: number
			  }
			  
			  doSomething(["hello", 42]);
			  ```
			- ```ts
			  function doSomething1(pair: [string, number]) {
			    // ...
			    const c = pair[2];
			    // Tuple type '[string, number]' of length '2' has no element at index '2'.
			  }
			  ```
		- Destructuring tuple
			- ```ts
			  function doSomething(stringHash: [string, number]) {
			    const [inputString, hash] = stringHash;
			    
			    console.log(inputString);
			    				// const inputString: string
			    console.log(hash);
			    			// const hash: number
			  }
			  ```
			- #+BEGIN_NOTE
			  Tuple types are useful in heavily convention-based API, this give us flexibility in whatever we want to name our variable when we destructure them
			  #+END_NOTE
		- Simple tuple type
			- ```ts
			  interface StringNumberPair {
			    // specialized properties
			    length: 2;
			    0: string;
			    1: number;
			    
			    // Other 'Array<string | number>' members...
			    slice(start?: number, end?: number): Array<string | number>;
			  }
			  ```
		- tuple type with optional type
			- ```ts
			  type Either2dOr3d = [number, number, number?];
			  
			  function setCoordinate(coord: Either2dOr3c) {
			    const [x, y, z] = coord;
			    			// const z: number | undefined
			    
			    console.log(`Provided coordinates had ${coord.length} dimensions`);
			    												// (property) length: 2 | 3
			  }
			  ```
		- rest elements with tupe
			- ```ts
			  type StringNumberBooleans = [string, number, ...boolean[]];
			  type StringBooleansNumber = [string, ...boolean[], number];
			  type BooleansStringNumber = [...boolean[], string, number];
			  ```
				- `StringNumberBooleans` describes a tuple that first two element are `string` and `number`, end with any number of `boolean`
				  logseq.order-list-type:: number
				- `StringBooleansNumber` describes a tuple whose first element is `string` and then any number of `boolean` and end with `number`
				  logseq.order-list-type:: number
				- `BooleansStringNumber` describes a tuple start with any number of `boolean` and ending with a `string`, and then a `number`
				  logseq.order-list-type:: number
			- this kind of tuple has no set "length" unlike normal tuple
		- ==Why optional and rest element be useful?==
			- It allows TypeScript to *correspond* tuples with parameter lists
				- ```ts
				  function readButtonInput(...args: [string, number, ...boolean[]]) {
				    const [name, version, ...input] = args;
				    // ...
				  }
				  
				  // as an alternative to
				  function readButtonInput(name: string, version: number, ...input: boolean[]) {
				    // ...
				  }
				  ```
		- readonly tuple
			- ```ts
			  function doSomething(pair: readonly [string, number]) {
			    // ...
			  }
			  ```
			- ```ts
			  function doSomething(pair: readonly [string, number]) {
			    pair[0] = "hello!";
			    // Cannot assign to '0' because it is a read-only property.
			  }
			  ```
			- `const` assertions will inferred the tuple types with `readonly`
				- ```ts
				  let point = [3, 4] as const;
				  
				  function distanceFromOrigin([x, y]: readonly [number, number]) {
				    return Math.sqrt(x ** 2 + y **2);
				  }
				  
				  distanceFromOrigin(point);
				  ```
- #### Section 6: Type Manipulation
  id:: 649bd07e-2018-4ed6-9dfc-39d16d7b0840
	- #### Generics Refresher #TypeScript/Generics
		- declaring a generic function
			- ```ts
			  function identity<Type>(arg: Type): Type {
			    return arg;
			  }
			  ```
		- make the function to work on arrays of `Type`
			- ```ts
			  function loggingIdentity<Type>(arg: Type[]): Type[] {
			    console.log(arg.length);
			    return arg;
			  }
			  
			  // alternative way
			  function loggingIdentity<Type>(arg: Array<T)
			  ```
			-
	-