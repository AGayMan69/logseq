- Author(s): [[N/A]]
  Url: [[https://github.com/gibbok/typescript-book#typescript-an-introduction]]
  tags:: #document #programming #[[software development]]
- #### TypeScript code generation #[[TypeScript/Code generation]]
	- The TypeScript compiler has two main responsibilities:
		- checking for type errors
		  logseq.order-list-type:: number
		- and compile code to *JavaScript*
		  logseq.order-list-type:: number
		- bro
		  logseq.order-list-type:: number
	- Types do not affect the execution of the code in a JavaScript engine
	- Types are completely **erased** during compilation
	- TypeScript can still output JavaScript even with `type errors`
	- #+BEGIN_WARNING
	  It's not possible to check TypeScript types at *runtime*
	  #+END_WARNING
		- ==Example==
			- ```ts
			  interface Animal {
			    name: string;
			  }
			  
			  interface Dog extends Animal {
			    bark: () => void;
			  }
			  interface Cat extends Animal {
			    meow: () => void;
			  }
			  const makeNoise = (animal: Animal) => {
			    if (animal instanceof Dog) {
			      // 'Dog' only refers to a type, but is being used as a value here.
			      // ...
			    }
			  }
			  ```
		- Since the types are **erased after compilation**, there is no way to run this code in JavaScript
		- We can achieve with *"tagged union"*
			- ```ts
			  interface Dog {
			    kind: 'dog'; // Tagged union
			    bark: () => void;
			  }
			  interface Cat {
			    kind: 'cat'; // Tagged union
			    meow: () => boid;
			  }
			  type Animal = Dog | Cat;
			  
			  const makeNoise = (animal: Animal) => {
			    if (animal.kind === 'dog') {
			      animal.bark();
			    } else {
			      animal.meow();
			    }
			  };
			  
			  const dog: Dog = {
			    kind: 'dog',
			    bark: () => console.log('bark'),
			  };
			  makeNoise(dog);
			  ```
		- It is possible for a value at a runtime to have a type different form the one declared in the type declaration.
			- ==Example==
				- ```ts
				  function add(a: number, b: string): number {
				    return a + b;
				  }
				  ```
				- At runtime, when we call the function with two numbers, the addition operator is not defined for adding *a number* and *a string*. Hence, runtime error and the program will crash
	- `class` keyword can be used as a type and value at runtime
		- ==Example==
			- ```ts
			  class Animal {
			      constructor(public name: string) {}
			  }
			  class Dog extends Animal {
			      constructor(public name: string, public bark: () => void) {
			          super(name);
			      }
			  }
			  class Cat extends Animal {
			      constructor(public name: string, public meow: () => void) {
			          super(name);
			      }
			  }
			  type Mammal = Dog | Cat;
			  
			  const makeNoise = (mammal: Mammal) => {
			      if (mammal instanceof Dog) {
			          mammal.bark();
			      } else {
			          mammal.meow();
			      }
			  };
			  
			  const dog = new Dog('Fido', () => console.log('bark'));
			  makeNoise(dog);
			  ```
		- In *JavaScript*, a *"class"* has a *"prototype"* property, and the `instanceof` operator can be used to test if the prototype of a constructor appears anywhere in the *prototype chain* of an object
	- TypeScript does introduce some build time overhead
- #### Structural Typing #TypeScript/Type
	- Types are determined by their structures
		- ```ts
		  type X = {
		    a: string;
		  };
		  type Y = {
		    a: string;
		  };
		  const x: X = { a: 'a' };
		  const y: Y = x; // Valid
		  ```
- #### Comparison Rules #TypeScript/Type #TypeScript/Comparison
	- A type *'X'* is compatible with *'Y'* if *'Y'* has at least the same members as *'X'*.
		- ```ts
		  type X = {
		    a: string;
		  };
		  const y = { a: 'A', b: 'B' }; // Valid, as it has at least the same members as x
		  const r: X = y;
		  ```
	- *Function parameters* are compared by types, not names:
		- ```ts
		  type X = (a: number) => void;
		  type Y = (a: number) => void;
		  let x: X = (j: number) => undefined;
		  let y: Y = (k: number) => undefined;
		  y = x; // Valid
		  x = y; // Valid
		  ```
	- *Function return types* must be the same
		- ```ts
		  type X = (a: number) => undefined;
		  type Y = (a: number) => number;
		  let x: X = (a: number) => undefined;
		  let y: Y = (a: number) => 1;
		  y = x; // Invalid
		  x = y; // Invalid
		  ```
	- The return type of a source function must be a **subtype** of the return type of a target function
		- ```ts
		  let x = () => ({ a: 'A' });
		  let y = () => ({ a: 'A', b: 'B' });
		  x = y; // Valid
		  y = x; // Invalid member b is missing
		  ```
	- Discarding function parameters
		- ```ts
		  [1, 2, 3].map((element, _index, _array) => element + 'x')
		  
		  type X = (a: number) => undefined;
		  type Y = (a: number, b: number) => undefined;
		  let x: X = (a: number) => undefined;
		  let y: Y = (a: number) => undefined; // Missing b parameter
		  y = x; // Valid
		  ```
	- Rest parameter is treated as optional parameters
		- ```ts
		  type X = (a: number, ...rest: number[]) => undefined;
		  let x: X = a => undefined; //valid
		  ```
	- Source and target parameters are assignable to supertypes or subtypes
	  ```ts
	  // Supertype
	  class X {
	      a: string;
	      constructor(value: string) {
	          this.a = value;
	      }
	  }
	  // Subtype
	  class Y extends X {}
	  // Subtype
	  class Z extends X {}
	  
	  type GetA = (x: X) => string;
	  const getA: GetA = x => x.a;
	  
	  // Bivariance does accept supertypes
	  console.log(getA(new X('x'))); // Valid
	  console.log(getA(new Y('Y'))); // Valid
	  console.log(getA(new Z('z'))); // Valid
	  ```
	- Enums are pretty much like numbers, but can't compare Enum values with different Enum
		- ```ts
		  enum X {
		      A,
		      B,
		  }
		  enum Y {
		      A,
		      B,
		      C,
		  }
		  const xa: number = X.A; // Valid
		  const ya: Y = 0; // Valid
		  X.A === Y.A; // Invalid
		  ```
	- Class instances check for their private and protected members
		- ```ts
		  
		  Instances of a class are subject to a compatibility check for their private and protected members:
		  
		  class X {
		      public a: string; 
		      constructor(value: string) {
		          this.a = value;
		      }
		  }
		  
		  class Y {
		      private a: string;
		      constructor(value: string) {
		          this.a = value;
		      }
		  }
		  
		  let x: X = new Y('y'); // Invalid, X have public member `a`, Y have private member `a`
		  ```
	- Does not check different inheritance hierarchy
		- ```ts
		  class X {
		      public a: string;
		      constructor(value: string) {
		          this.a = value;
		      }
		  }
		  class Y extends X {
		      public a: string;
		      constructor(value: string) {
		          super(value);
		          this.a = value;
		      }
		  }
		  class Z {
		      public a: string;
		      constructor(value: string) {
		          this.a = value;
		      }
		  }
		  let x: X = new X('x');
		  let y: Y = new Y('y');
		  let z: Z = new Z('z');
		  x === y; // Valid
		  x === z; // Valid even if z is from a different inheritance hierarchy
		  ```
	- Generics are compared based on the *resulting type* after **applying** the generic parameter
		- ```ts
		  interface X<T> {
		      a: T;
		  }
		  let x: X<number> = { a: 1 };
		  let y: X<string> = { a: 'a' };
		  x === y; // Invalid as the type argument is used in the final structure
		  ```
		- ```ts
		  interface X<T> {}
		  const x: X<number> = 1;
		  const y: X<string> = 'a';
		  x === y; // Valid as the type argument is not used in the final structure
		  ```
	- When generics do not specified their type, all unspecified arguments are as types with "any"
		- ```ts
		  type X = <T>(x: T) => T;
		  type Y = <K>(y: K) => K;
		  let x: X = x => x;
		  let y: Y = y => y;
		  x = y; // Valid
		  ```
	- More
		- ```ts
		  let a: number = 1;
		  let b: number = 2;
		  a = b; // Valid, everything is assignable to itself (number)
		  
		  let c: any;
		  c = 1; // Valid, all types are assignable to any
		  
		  let d: unknown;
		  d = 1; // Valid, all types are assignable to unknown
		  
		  let e: unknown;
		  let e1: unknown = e; // Valid, unknown is only assignable to itself and any
		  let e2: any = e; // Valid
		  let e3: number = e; // Invalid
		  
		  let f: never;
		  f = 1; // Invalid, nothing is assignable to never
		  
		  let g: void;
		  let g1: any;
		  g = 1; // Invalid, void is not assignable to or from anything expect any
		  g = g1; // Valid
		  ```
	- #+BEGIN_NOTE
	  when *"strctNullChecks"* is enabled, *"null"* and *"undefined"* are treated similarly to *"void"*; otherwise, they are similar to "never"
	  #+END_NOTE
- #### Types #TypeScript/Type
	- |Set Term|TypeScript|Notes|
	  |--|--|--|
	  |Empty set|`never`|"never" contains anything apart itself|
	  |Single element set|`undefined` / `null` / `literal type`||
	  |Finite set|`boolean` / `union` ||
	  |Infinite set|`string` / `number` / `object`||
	  |Universal set|`any` / `unknown`|Every element is a member of "any" and every set is a subset of it / "unknown" is a type-safe counterpart of "any"|
	  ||||
	- |TypeScript|Set term|Example|
	  |--|--|--|
	  |never|∅ (empty set)|const x: never = 'x'; // Error: Type 'string' is not assignable to type 'never'|
	  |Literal type|Single element set|type X = 'X';|
	  |||type Y = 7;|
	  |Value assignable to T|Value ∈ T (member of)|type XY = 'X' <code>&#124;</code> 'Y';|
	  |||const x: XY = 'X';|
	  |T1 assignable to T2|T1 ⊆ T2 (subset of)|type XY = 'X' <code>&#124;</code> 'Y';|
	  |||const x: XY = 'X';|
	  |||const j: XY = 'J'; // Type '"J"' is not assignable to type 'XY'.|
	  |T1 extends T2|T1 ⊆ T2 (subset of)|type X = 'X' extends string ? true : false;|
	  |T1 <code>&#124;</code> T2|T1 ∪ T2 (union)|type XY = 'X' <code>&#124;</code> 'Y';|
	  |||type JK = 1 <code>&#124;</code> 2;|
	  |T1 & T2|T1 ∩ T2 (intersection)|type X = { a: string }|
	  |||type Y = { b: string }|
	  |||type XY = X & Y|
	  |||const x: XY = { a: 'a', b: 'b' }|
	  |unknown|Universal set|const x: unknown = 1|
- #### Type Declarations and Type Assertions #Typescript/Type
	- **Type Declaration**
		- ```ts
		  type X = {
		    a: string;
		  };
		  
		  const x: X = {
		    // Type declaration
		    a: 'a',
		  }
		  ```
		- TypeScript will report an error if the variable is not in the specified format
		- ```ts
		  type X = {
		    a: string;
		  };
		  
		  const x: X = {
		    a: 'a',
		    b: 'b', // Error
		  }
		  ```
	- **Type Assertion**
		- add an assertion using the `as` keyword
			- ```ts
			  type X = {
			    a: string;
			  };
			  const x ={ 
			  	a: 'a',
			    	b: 'b',
			  } as X;
			  ```
		- ==Example==
			- Type assertion are use to specified a type to be more specific type
				- ```ts
				  const myInput = document.getElementById('my_input') as HTMLInputElement;
				  ```
			- Type assertion can also be used to remap keys
				- ```ts
				  type J<Type> = {
				      [Property in keyof Type as `prefix_${string & Property}`]: () => Type[Property;]
				  };
				  type X = {
				      a: string;
				      b: number;
				  };
				  type Y = J<X>;
				  ```
	- **Non-null assertion**
		- `!` tell TypeScript that the value cannot be null or undefined
			- ```ts
			  let x: null | number;
			  let y = x!; // number
			  ```
	- **Ambient Declarations**
		- ambient declaration are files that describes types for *JavaScript* code, which have file extension name `.d.ts.`
		- They are usually used for annotate existing *JavaScript* libraries with types
		- Common libraries types can installed using
			- ```bash
			  npm install --save-dev @types/library-name
			  ```
		- Using own ambient declaration
			- ```ts
			  /// <reference path="./library-types.d.ts" />
			  ```
- #### Property Checking and Excess Property Checking #TypeScript/Type #[[TypeScript/Type Check]]
	- *Excess Property Checking* is performed when assigning **object literals** to variables, *or* passing them as arguments to the **function's excess property**
		- ```ts
		  type X = {
		    a: string;
		  }
		  const y = { a: 'a', b: 'b' };
		  const x: X = y; // Valid because structural typing
		  const w: X = { a: 'a', b: 'b' }; // Invalid because excess property checking
		  ```
- #### Weak Types #TypeScript/Type #TypeScript/Optional
	- a type is considered weak when it contains **all-optional** properties
		- ```ts
		  type X = {
		    a?: string;
		    b?: string;
		  }
		  ```
	- If no overlap, assign anything to a weak type will throws an error
		- ```ts
		  type Options = {
		    a?: string;
		    b?: string;
		  };
		  
		  const fn = (options: Options) => undefined;
		  
		  fn({ c: 'c' }); // Invalid
		  fn({ c: 'c' } as Options); // Valid
		  fn({ a: 'bro', c: 'bro'}); // Invalid
		  ```
	- can be solve using *index signature*
		- ```ts
		  type Options = {
		    [prop: string]: unknown;
		    a?: string;
		    b?: string;
		  };
		  
		  const fn = (options: Options) => undefined;
		  fn({ c: 'C' }); // Valid
		  ```
- #### Strict Object Literal Checking (Freshness)
	- Is a features that helps catch *excess* or *misspelled properties* that would go unnoticed in normal type checks
	- ==Example==
		- ```ts
		  type X = { a: string };
		  type Y = { a: string; b: string };
		  
		  let x: X;
		  x = { a: 'a', b: 'b' }; // Freshness check: Invalid assignment
		  var y: Y;
		  y = { a: 'a', bx: 'bx' }; // Freshness check: Invalid assignment
		  
		  const fn = (x: X) => console.log(x.a);
		  
		  fn(x);
		  fn(y); // No errors, structurally type compatible
		  
		  fn({ a: 'a', bx: 'b' }); // Freshness check: Invalid argument
		  ```
- #### Type Inference #TypeScript/Inference
	- TypeScript can infer types when no annotation during
		- Variable initialization
		  logseq.order-list-type:: number
			- logseq.order-list-type:: number
			  ```ts
			  let num = 10; // num: number
			  ```
		- Member initialization
		  logseq.order-list-type:: number
			- logseq.order-list-type:: number
			  ```ts
			  class Person {
			    name = "John"; // name: string
			  }
			  ```
		- Setting defaults for parameters
		  logseq.order-list-type:: number
			- logseq.order-list-type:: number
			  ```ts
			  function greet(name = "Anonymous") {
			    console.log(`Hello, ${name}!`)
			  }
			  ```
		- Function return type
		  logseq.order-list-type:: number
			- logseq.order-list-type:: number
			  ```ts
			  function add(a: number, b: number) {
			    return a + b;
			  }
			  ```
	- ==Example==
		- ```ts
		  let x = [1, 'x', 1, null]; // The type inferred is: (string | number | null)[]
		  ```
		- Return a union type if common types cannot be found
			- ```ts
			  let x = [new RegExp('x'), new Date()]; // Type inferred is: (RegExp | Date)[]
			  ```
	- #+BEGIN_NOTE
	  TypeScript utilizes **"contextual typing"** based on the variable's location to infer types. In the following example, the compiler knows that `e` is of type `MouseEvent` because of the `click` event type defined in the `lib.d.ts` file, which contains ambient declarations for various common JavaScript constructs and the DOM
	  #+END_NOTE
		- ```ts
		  window.addEventListener('click', function (e) {}); // e inferred type is MouseEvent
		  ```
- #### Type Widening/Narrowing
	- ==Example==
		- ```ts
		  let x = 'x'; // TypeScript infers as string, a wide type
		  let y: 'y' | 'x' = 'y'; // y types is a union of literal types
		  y = x; // Invali Type 'string' is not assignable to type '"x" | "y"'.
		  ```
		- use `const` keyword then declaring a variable results in a *narrower* type inference in TypeScript
		- ```ts
		  const x = 'x' // TypeScript infers the type of x as 'x', a narrower type
		  let y: 'y' | 'x' = 'y';
		  y = x; // Valid: The type of x is inferred as 'x'
		  ```
	- ==Generics Example==
		- ```ts
		  function identity<T>(value: T) {
		      // No const here
		      return value;
		  }
		  const values = identity({ a: 'a', b: 'b' }); // Type infered is: { a: string; b: string; }
		  ```
		- ```ts
		  function identity<const T>(value: T) {
		      // Using const modifier on type parameters
		      return value;
		  }
		  const values = identity({ a: 'a', b: 'b' }); // Type infered is: { a: "a"; b: "b"; }
		  ```
- #### Explicit Type Annotation #TypeScript/Type #TypeScript/Annotation
	- ```ts
	  const v = {
	    x: 1, // Inferred type: number (widening)
	  };
	  v.x = 3; // Valid
	  ```
	- ```ts
	  const v: { x: 1 | 2 | 3 } = {
	    x: 1, // x is now a union of literal types: 1 | 2 | 3
	  };
	  v.x = 3; // Valid
	  ```
- #### Const assertion #TypeScript/Assertion
	- `const` type can be used to assert a single property
		- ```ts
		  const v = {
		    x: 3 as const,
		  };
		  v.x = 3;
		  ```
	- on an entire object
		- ```ts
		  const v = {
		    x: 1, // readonly
		    y: 2, // readonly
		  } as const;
		  ```
	- ```ts
	  const x = [1, 2, 3]; // number[]
	  const y = [1, 2, 3] as const; // Tuple of readonly [1, 2, 3]
	  ```
- #### Type Narrowing #TypeScript/Narrowing
	- TypeScript is narrowed down to a more specific type, base on
	- **Conditions**
		- ```ts
		  let x: number | undefined = 10;
		  
		  if (x !== undefined) {
		    x += 100; // The type is number, which had been narrowed by the condition
		  }
		  ```
	- **Throwing or returning**
		- ```ts
		  let x: number | undefined = 10;
		  
		  if (x === undefined) {
		    throw 'error';
		  }
		  x += 100;
		  ```
	- *Other ways*
		- `instance of ` check object instance is a class
		  logseq.order-list-type:: number
		- `in` check property exist in an object
		  logseq.order-list-type:: number
		- `typeof` check type of a value
		  logseq.order-list-type:: number
		- `Array.isArray()` check if a value is an array
		  logseq.order-list-type:: number
- #### Discriminated union #TypeScript/Union
	- *discriminated union* is a pattern that an *explicit tag* is added (in this case `type`) to distinguish different types within a union
		- ```ts
		  type A = { type: 'type_a'; value: number };
		  type B = { type: 'type_b'; value: string };
		  
		  const x = (input: A | B): string | number => {
		    switch(input.type) {
		      case 'type_a':
		        return input.value + 100; // type is A
		      case 'type_b':
		        return input.value + 'extra'; // type is B
		    }
		  }
		  ```
- #### User-defined type guards #[[TypeScript/Type Guard]]
	- It is possible to create a helper function *"user-defined type guard"* to help TypeScript to determine a type
	- ==Type Predicate Example==
		- ```ts
		  const data = ['a', null, 'c', 'd', null, 'f'];
		  
		  const r1 = data.filter(x => x != null); // The type is (string | null)[], TypeScript was not able to infer the type properly
		  
		  const isValid = (item: string | null): item is string => item !== null; // Custom type guard using "Type Predicate"
		  
		  const r2 = data.filter(isValid); // The type is fine now string[], by using the predicate type guard we were able to narrow the type
		  ```
- #### Primitve Types
	- #+BEGIN_NOTE
	  TypeScript supports 7 *primitive types*, primitive types is not an object and does not have any methods associated with it. All primitive types are **immutable** - meaning their values cannot be changed once they are assigned
	  #+END_NOTE
	- **string**
		- multiple lines
			- ```ts
			  let sentence: string = `xxx,
			    yyy`;
			  ```
	- **boolean**
		- ```ts
		  const isReady: boolean = true
		  ```
	- **number**
		- `number` represented with a *64-bit floating point value*, can represent *integers* and *fractions*
			- ```ts
			  const decimal: number = 10;
			  const hexadecimal: number = 0xa00d; // Hexadecimal starts with 0x
			  const binary: number = 0b1010; // Binary starts with 0b
			  const octal: number = 0o633; // Octal starts with 0c
			  
			  ```
	- **bigInt**
		- ```ts
		  const x: bigint = BigInt(9007199254740991);
		  const y: bigint = 9007199254740991n;
		  ```
		- #+BEGIN_NOTE
		  `bigInt` values cannot be mixed with `number` and cannot used with built-in `Math`
		  #+END_NOTE
	- **symbol**
		- creates a globally unique reference
			- ```ts
			  let sym = Symbol("x"); // Type symbol
			  ```
	- **null and undefined**
		- both represent `absence` of any value or no value
		- `undefined` type means the value is **not assigned or initialised** or indicates an unintentional **absence** of value
		- `null` means we intentionally to have this field to have no value, **intentional absence of value**
	- **Array**
		- ```ts
		  const x: string[] = ['a', 'b'];
		  const y: Array<string> = ['a', 'b'];
		  const j: Array<string | number> = ['a', 1, 'b', 2]; // Union
		  ```
		- *Readonly*
		- ```ts
		  const x: readonly string[] = ['a', 'b']; // Readonly modifier
		  const y: ReadonlyArray<string> = ['a', 'b'];
		  const j: ReadonlyArray<string | number> = ['a', 1, 'b', 2];
		  j.push('x'); // Invalid
		  ```
		- *Tuple*
		- ```ts
		  const x: [string, number] = ['a', 1];
		  const y: readonly [string, number] = ['a', 1];
		  ```
	- **Any**
		- usually use for migration from *JavaScript* to *TypeScript*, instead avoid using `any` type if possible
- #### Type Annotations #TypeScript/Annotation
	- ==Example==
		- ```ts
		  function sum(a: number, b: number) { // add type annotations to parameters
		    return a + b;
		  }
		  ```
		- ```ts
		  const sum = (a: number, b: number) => a + b; // anonymous
		  ```
		- ```ts
		  const sum = (a = 10, b: number): number => a + b; // return type annotations can be added to functions
		  ```
	- #+BEGIN_NOTE
	  annotating type signatures but not the body *local variables* and always add type to *object literals*
	  #+END_NOTE
- #### Object Types #TypeScript/object
	- Type annotate object with `interface` or a `type alias`
		- ```ts
		  interface Y {
		    b: number;
		  }
		  type X = {
		    a: number;
		  };
		  ```
	- anonymously
		- ```ts
		  const sum = (x: { a: number; b: number }) => x.a + x.b;
		  console.log(sum({ a: 5, b: 1 }));
		  ```
- #### Optional Properties #Typescript/Optional
	- ```ts
	  type X = {
	      a: number;
	      b?: number; // Optional
	  };
	  ```
	- specify a default value on optional property
	- ```ts
	  type X = {
	      a: number;
	      b?: number;
	  };
	  const x = ({ a, b = 100 }: X) => a + b;
	  ```
- #### Index signature #[[TypeScript/Index signature]]
	- The idea of *index signatures* is to type objects of unknown structure
	- ==Example==
		- ```ts
		  type K = {
		    [name: string | number]: string;
		  };
		  const k: K = { x: 'x', 1: 'b' };
		  console.log(k['x']);
		  console.log(k[1]);
		  console.log(k['1']); // same result as k[1]
		  ```
	- #+BEGIN_NOTE
	  Because *JavaScript* auto converts `number` index with `string` so `k[1]` or `k["1"]` return the same value
	  #+END_NOTE
- #### Extending Types #TypeScript/extends
	- It is possible to extend an `interface`
	- ```ts
	  interface A {
	      a: string;
	  }
	  interface B {
	      b: string;
	  }
	  interface Y extends A, B {
	      y: string;
	  }
	  ```
	- #+BEGIN_NOTE
	  The `extends` keyword works only on interfaces and classes
	  #+END_NOTE
	- ```ts
	  type A = {
	      a: number;
	  };
	  type B = {
	      b: number;
	  };
	  type C = A & B;
	  ```
	- #+BEGIN_NOTE
	  It is possible to extend a type using interface but no vice versa
	  #+END_NOTE
	- ```ts
	  type A = {
	    a: string;
	  };
	  // extends only works on interface and classes
	  interface B extends A {
	    b: string;
	  }
	  ```
- #### Intersection Types #TypeScript/intersection
	- ```ts
	  type A = {
	      a: string;
	  };
	  type B = {
	      b: string;
	  };
	  type C = A & B;
	  
	  // or
	  interface X {
	      x: string;
	  }
	  interface Y {
	      y: string;
	  }
	  type J = X & Y;
	  ```
- #### Literal Types #[[TypeScript/Literal Types]]
	- *Literal Type* is a single element set from a collective type
		- *numbers*, *strings*, *booleans*
		- ```ts
		  const a = 'a'; // string literal type
		  const b = 1; // numeric literal type
		  const c = true; // boolean literal type
		  ```
	- ==Type alias union Example==
		- ```ts
		  type O = 'a' | 'b' | 'c';
		  ```
- #### Literal Inference #TypeScript/Inference #[[TypeScript/Literal Types]]
	- ```ts
	  const x = 'x'; // literal type of x, because this value cannot be changed
	  let y = 'y'; // string, as we can change this value
	  ```
	- ==More Example==
		- ```ts
		  type X = 'a' | 'b';
		  
		  let o = {
		      x: 'a', // this is a wider string
		  };
		  
		  const fn = (x: X) => `${x}-foo`;
		  
		  console.log(fn(o.x)); // Argument of type 'string' is not assignable to parameter of type 'X'
		  
		  let o = {
		      x: 'a' as const,
		  };
		  ```
- #### Enums #TypeScript/Enum
	- `enum` is a set of named constant values
		- ```ts
		  enum Color {
		      Red = '#ff0000',
		      Green = '#00ff00',
		      Blue = '#0000ff',
		  }
		  ```
	- **Numeric enums**
		- ```ts
		  enum Size {
		      Small, // value starts from 0
		      Medium,
		      Large,
		  }
		  
		  // assign custom values explicitly
		  enum Size {
		      Small = 10,
		      Medium,
		      Large,
		  }
		  console.log(Size.Medium); // 11
		  ```
	- **String enums**
		- ```ts
		  enum Language {
		      English = 'EN',
		      Spanish = 'ES',
		  }
		  ```
	- #+BEGIN_NOTE
	  TypeScript allow heterogeneous enums - string and numeric members can coexist
	  #+END_NOTE
	- **Constant enums**
		- the values are known at compile time and are inline wherever the enum is used
			- ```ts
			  const enum Language {
			    English = 'EN',
			    Spanish = 'ES',
			  }
			  console.log(Language.English);
			  
			  // will be compiled into
			  console.log('EN' /* Language.English */);
			  ```
	- **Reverse mapping**
		- reverse mapping able to retrieve the enum member name from its value
	- **Ambient enums**
		- means a type of enum that is defined in a declaration file (`*.d.ts`) without an associated implementation that can be used in a *type-safe* way across different files **without importing** the implementation details in each file
	- **Computed and constant members**
		- *constant member* is a member whose value is set at *compile-time* and cannot change during **runtime**
		- *computed members* allowed both *regular* and *const enums*
		- ```ts
		  // constant members
		  enum Color {
		    Red = 1,
		    Green = 5,
		    Blue = Red + Green,
		  }
		  console.log(Color.Blue); // 6 generation at compilation time
		  
		  enum Color {
		    Red = 1,
		    Green = Math.pow(2, 2),
		    Blue = Math.floor(Math.random() * 3) + 1,
		  }
		  console.log(Color.Blue); // random number generated at run time
		  ```
	- #+BEGIN_NOTE
	  Enums are denoted by unions comprising their member types, for example, E represents the union `E.A | E.B | E.C`
	  #+END_NOTE
		- ```ts
		  const identity = (value: number) => value;
		  
		  enum E {
		    A = 2* 5, // Numberic literal
		    B = 'bar', // String literal
		    C = identity(42), // Opaque computed
		  }
		  
		  console.log(E.C); //42
		  ```
- #### Narrowing #TypeScript/Narrowing
	- **`typeof` type guards**
		- checks the *type* of a variable based on its *built-in JavaScript type*
			- ```ts
			  const fn = (x: number | string): number => {
			    if (typeof x === 'number') {
			      return x + 1; // x is number
			    }
			    return -1;
			  };
			  ```
	- **Truthiness narrowing**
		- Check whether a variable is truth or falsy to narrow its type
		- ```ts
		  const printName = (name: string | null | undefined) => {
		    if (name) {
		      console.log(name.toUpperCase());
		    } else {
		      console.log('No name specified');
		    }
		  }
		  ```
	- **Equality narrowing**
		- check whether a variable is equal to a **specific value** or not
		- ```ts
		  const logMessage = (status: 'success' | 'error') => {
		    switch (status) {
		      case 'success':
		        console.log('Operation was successful!');
		        break;
		      case 'error':
		        console.log('An error occurred.');
		        break;
		    }
		  }
		  ```
	- **`in` operator narrowing**
		- ```ts
		  type Dog = {
		    name: string;
		    breed: string;
		  }
		  
		  type Cat = {
		    name: string;
		    likesCream: boolean;
		  };
		  
		  const printPet = (pet: Dog | Cat) => {
		    if ('breed' in pet) {
		      console.log(``This is a ${pet.breed} dog nameed ${pet.name}.``);
		    } else {
		      console.log(
		      ``This is a cat named ${pet.name} that ${
		        	pet.likesCream ? 'likes' : "doesn't like"
		  	} cream.``
		      );
		    }
		  }
		  ```
	- **`instanceof` narrowing**
		- checking an object is an *instance* of a certain class or *interface*
			- ```ts
			  class Square {
			    constructor(public width: number) {}
			  }
			  class Rectangle {
			    constructor(public width: number, public height: number) {}
			  }
			  function area(shape: Square | Rectangle) {
			    if (shape instanceof Square) {
			      return shape.width * shape.width;
			    } else {
			      return shape.width * shape.height;
			    }
			  }
			  const square = new Square(5);
			  const rectangle = new Rectangle(5, 10);
			  console.log(area(square)); // 25
			  console.log(area(rectangle)); // 50
			  ```
- #### Control flow analysis #[[TypeScript/Control Flow]] #TypeScript/Narrowing
	- *code flow analysis* can be applied to code within an *if* statement, *conditional expression* and *discriminant property accesses*
	- ```ts
	  const f1 = (x: unknown) => {
	      const isString = typeof x === 'string';
	      if (isString) {
	          x.length;
	      }
	  };
	  
	  const f2 = (
	      obj: { kind: 'foo'; foo: string } | { kind: 'bar'; bar: number }
	  ) => {
	      const isFoo = obj.kind === 'foo';
	      if (isFoo) {
	          obj.foo;
	      } else {
	          obj.bar;
	      }
	  };
	  ```
	- ==Example where narrowing does not occur==
	- ```ts
	  const f1 = (x: unknown) => {
	      let isString = typeof x === 'string';
	      if (isString) {
	          x.length; // error, no narrowing because isString it is not const
	      }
	  };
	  
	  const f6 = (
	      obj: { kind: 'foo'; foo: string } | { kind: 'bar'; bar: number }
	  ) => {
	      const isFoo = obj.kind === 'foo';
	      obj = obj;
	      if (isFoo) {
	          obj.foo; // Error, no narrowing because obj is assigned in function body
	      }
	  };
	  ```
	- #+BEGIN_NOTE
	  up to five levels of indirection are analyzed in *conditional expressions*
	  #+END_NOTE
- #### Type predicates #TypeScript/Narrowing
	- *type predicate* are functions that return a boolean value to narrow the type of a variable
	- ```ts
	  const isString = (value: unknown): value is string => typeof value === "string";
	  
	  const foo = (bar: unknown) => {
	    if (isString(bar)) {
	      console.log(bar.toUpperCase());
	    } else {
	      console.log("not a string");
	    }
	  }
	  ```
- #### Discriminated unions #TypeScript/Narrowing
	- just a union type that have a common property *known as* **discriminant** to narrow down the set of possible types for the union
	- ```ts
	  type Square = {
	      kind: 'square'; // Discriminant
	      size: number;
	  };
	  
	  type Circle = {
	      kind: 'circle'; // Discriminant
	      radius: number;
	  };
	  
	  type Shape = Square | Circle;
	  
	  const area = (shape: Shape) => {
	      switch (shape.kind) {
	          case 'square':
	              return Math.pow(shape.size, 2);
	          case 'circle':
	              return Math.PI * Math.pow(shape.radius, 2);
	      }
	  };
	  
	  const square: Square = { kind: 'square', size: 5 };
	  const circle: Circle = { kind: 'circle', radius: 2 };
	  
	  console.log(area(square)); // 25
	  console.log(area(circle)); // 12.566370614359172
	  ```
- #### `never` narrowing #TypeScript/Narrowing
	- when a variable is narrowed to a type that cannot contain any values, the compiler will infer that the variable must be of the `never` type
	- ```ts
	  const printValue = (val: string | number) => {
	      if (typeof val === 'string') {
	          console.log(val.toUpperCase());
	      } else if (typeof val === 'number') {
	          console.log(val.toFixed(2));
	      } else {
	          // val has type never here because it can never be anything other than a string or a number
	          const neverVal: never = val;
	          console.log(`Unexpected value: ${neverVal}`);
	      }
	  };
	  ```
- #### Tuple Type #TypeScript/Type
	- tuple respresent an array with a fixed number of elements and their types, tuple type specify the *number of element* and type in **a fixed order**
	- ```ts
	  type Point = [number, number];
	  ```
	- Fixed length tuple
	- ```ts
	  const x = [10, 'hello'] as const;
	  x.push(2); // Error
	  ```
- #### Type Indexing #TypeScript/Indexing
	- ```ts
	  type Dictionary<T> = {
	      [key: string]: T;
	  };
	  const myDict: Dictionary<string> = { a: 'a', b: 'b' };
	  console.log(myDict['a']); // return a
	  ```
- #### Mapped types #TypeScript/Type
	- create new types based on an existing type
		- ```ts
		  type MyMappedType<T> = {
		    [P in keyof T]: T[P][];
		  };
		  type MyType = {
		    foo: string;
		    bar: number;
		  };
		  type MyNewType = MyMappedType<MyType>;
		  /*
		  MyNewType = {
		  	foo: string[]
		      bar: number[]
		  }
		  */
		  const x: MyNewType = {
		    foo: ['hello', 'world'],
		    bar: [1, 2, 3],
		  };
		  ```
- #### Conditional Types #TypeScript/Type
	- ```ts
	  type IsArray<T> = T extends any[] ? true : false;
	  
	  const myArray = [1, 2, 3];
	  const myNumber = 42;
	  
	  type IsMyArrayAnArray = IsArray<typeof myArray>; // Type true
	  type IsMyNumberAnArray = IsArray<typeof myNumber>; // Type false
	  ```
- #### Distributive conditional types #TypeScript/Type
	- ```ts
	  type Nullable<T> = T extends any ? T | null : never;
	  type NumberOrBool = number | boolean;
	  type NullableNumberOrBool = Nullable<NumberOrBool>; // number | boolean | nul
	  ```
- #### "infer" Type inference in conditional types #TypeScript/Type
	- `infer` keyword is used in conditional types to (extract) the type of a generic parameter
	- ```ts
	  type ElementType<T> = T extends (infer U)[] ? U : never;
	  type Numbers = ElementType<number[]>; // number
	  type Strings = ElementType<string[]>; // string
	  ```
- #### Predefined conditional types
	- predefined conditional types are built-in conditional types provided by the language
	- `Exclude<UnionType, ExcludedType>` - *removes* all the types from Type that are assignable to ExcludedType
	  logseq.order-list-type:: number
	- `Extract<Type, Union>` This type *extracts all the types* from Union that are assignable to Type
	  logseq.order-list-type:: number
	- `NonNullable` removes *null* and *undefined* from Type
	  logseq.order-list-type:: number
	- `ReturnType` extracts the *return type* of a function Type
	  logseq.order-list-type:: number
	- `Parameters`: This type *extracts the parameter* types of a function Type.
	  logseq.order-list-type:: number
	- `Required`: This type makes all properties in Type *required*.
	  logseq.order-list-type:: number
	- `Partial`: This type makes all properties in Type *optional*.
	  logseq.order-list-type:: number
	- `Readonly`: This type makes all properties in Type *readonly*.
	  logseq.order-list-type:: number
- #### Template Union Types #TypeScript/Union
	- ```ts
	  type Status = 'active' | 'inactive';
	  type Product = 'p1' | 'p2';
	  type ProductId = `id-${Products}-${Status}`; // "id-p1-active" | "id-p1-inactive" | "id-p2-active" | "id-p2-inactive"
	  ```
- #### Unknown type #TypeScript/Type
	- `unknown` requires a type check or assertion before it can be used in a specific way, it's like a *type-safe* alternative to `any`
	- ```ts
	  let value: unknown;  
	  
	  let value1: unknown = value; // Valid
	  let value2: any = value; // Valid
	  let value3: boolean = value; // Invalid
	  let value4: number = value; // Invalid
	  ```
	- ```ts
	  const add = (a: unknown, b: unknown): number | undefined =>
	      typeof a === 'number' && typeof b === 'number' ? a + b : undefined;
	  console.log(add(1, 2)); // 3
	  console.log(add('x', 2)); // undefined
	  ```
- #### Void Type #TypeScript/Type
	- represent a function does not return a value
	- ```ts
	  const sayHello = (): void => {
	      console.log('Hello!');
	  };
	  ```
- #### Never type #TypeScript/Type
	- `never` type indicate a function or expressions that never *return* or *throw an error*
	- ```ts
	  const throwError = (message: string): never => {
	      throw new Error(message);
	  };
	  ```
	- ```ts
	  type Direction = 'up' | 'down';
	  const move = (direction: Direction): void => {
	      switch (direction) {
	          case 'up':
	              // move up
	              break;
	          case 'down':
	              // move down
	              break;
	          default:
	              const exhaustiveCheck: never = direction;
	              throw new Error(`Unhandled direction: ${exhaustiveCheck}`);
	      }
	  };
	  ```
- #### Get & Set #TypeScript/class
	- ```ts
	  class MyClass {
	      private _myProperty: string;
	  
	      constructor(value: string) {
	          this._myProperty = value;
	      }
	      get myProperty(): string {
	          return this._myProperty;
	      }
	      set myProperty(value: string) {
	          this._myProperty = value;
	      }
	  }
	  ```
- #### TypeScript/Interface
	- **Merging** allows you to combine *multiple declarations* of the same name into a *single definition*, where *types* cannot do that
	- ```ts
	  interface X {
	      a: string;
	  }
	  
	  interface X {
	      b: number;
	  }
	  
	  const person: X = {
	      a: 'a',
	      b: 7,
	  };
	  ```
	- **Extension** is a mechanism to add additional properties or methods to an existing type
	- ```ts
	  interface Animal {
	      name: string;
	      eat(): void;
	  }
	  
	  interface Bird extends Animal {
	      sing(): void;
	  }
	  
	  const dog: Bird = {
	      name: 'Bird 1',
	      eat() {
	          console.log('Eating');
	      },
	      sing() {
	          console.log('Singing');
	      },
	  };
	  ```
	- ```ts
	  interface A {
	      x: string;
	      y: number;
	  }
	  
	  type B = A & {
	      j: string;
	  };
	  
	  const c: B = {
	      x: 'x',
	      y: 123,
	      j: 'j',
	  };
	  ```
	- #+BEGIN_NOTE
	  an *interface* cannot extend a complex type like a *union type*.
	  #+END_NOTE
- #### Type Vs Interface #Typescript/Interface #TypeScript/Type
- #+BEGIN_NOTE
  *Types* are more flexible when it comes defining union and intersection types
  #+END_NOTE
- #+BEGIN_NOTE
  *Interfaces* doesn't have built-in support for intersection types 
  #+END_NOTE
- ```ts
  type Department = 'dep-x' 'dep-y'; // Union
  
  type Person = {
    name: string;
    age: number;
  };
  
  type Employee = {
    id: number;
    department: Department;
  };
  
  type EmployeeInfo = Person & Employee; //Intersection
  
  interface A {
    x: 'x';
  } 
  
  interface B {
    y: 'y';
  }
  
  type C = A | B; // Union of interfaces
  ```
- #### Decorators #TypeScript/Decorators
	- `Decorators` provide a mechanism to
		- Watching property changes
		  logseq.order-list-type:: number
		- Watching method calls
		  logseq.order-list-type:: number
		- Adding extra properties or methods
		  logseq.order-list-type:: number
		- Runtime validation
		  logseq.order-list-type:: number
		- Automatic serialization and deserialization
		  logseq.order-list-type:: number
		- Logging
		  logseq.order-list-type:: number
		- Authorization and authentication
		  logseq.order-list-type:: number
		- Error guarding
		  logseq.order-list-type:: number
	- **Class decorators**
		- ```ts
		  type Constructor<T = {}> = new (...args: any[]) => T;
		  
		  function toString<Class extends Constructor> {
		    Value: Class,
		      context: ClassDecoratorContext<Class>
		  } {
		    return class extends Value {
		          constructor(...args: any[]) {
		            super(...args);
		            console.log(JSON.stringify(this));
		            console.log(JSON.stringify(context));
		          }
		    };
		  }
		    
		  @toString
		    class Person {
		      name: string;
		      
		      constructor(name: string) {
		        this.name = name;
		      }
		      
		      greet() {
		  		return 'Hello, ' + this.name;
		      }
		  
		    }
		  const person = new Person('Simon');
		    /* Logs:
		  {"name":"Simon"}
		  {"kind":"class","name":"Person"}
		  */
		  ```
	- **Property Decorator**
		- *Property decorators* are used for modifying the behavior of a property
		- ```ts
		  function upperCase<T>(
		      target: undefined,
		      context: ClassFieldDecoratorContext<T, string>
		  ) {
		      return function (this: T, value: string) {
		          return value.toUpperCase();
		      };
		  }
		  
		  class MyClass {
		      @upperCase
		      prop1 = 'hello!';
		  }
		  
		  console.log(new MyClass().prop1); // Logs: HELLO!
		  ```
	- **Method Decorator**
		- ```ts
		  function log<This, Args extends any[], Return>(
		      target: (this: This, ...args: Args) => Return,
		      context: ClassMethodDecoratorContext<
		          This,
		          (this: This, ...args: Args) => Return
		      >
		  ) {
		      const methodName = String(context.name);
		  
		      function replacementMethod(this: This, ...args: Args): Return {
		          console.log(`LOG: Entering method '${methodName}'.`);
		          const result = target.call(this, ...args);
		          console.log(`LOG: Exiting method '${methodName}'.`);
		          return result;
		      }
		  
		      return replacementMethod;
		  }
		  
		  class MyClass {
		      @log
		      sayHello() {
		          console.log('Hello!');
		      }
		  }
		  
		  console.log(new MyClass().sayHello()); // Logs: Hello!
		  ```
	- **Getter and Setter Decorators**
		- change or enhance the behavior of class accessors.
		- ```ts
		  function range<This, Return extends number>(min: number, max: number) {
		      return function (
		          target: (this: This) => Return,
		          context: ClassGetterDecoratorContext<This, Return>
		      ) {
		          return function (this: This): Return {
		              const value = target.call(this);
		              if (value < min || value > max) {
		                  throw 'Invalid';
		              }
		              Object.defineProperty(this, context.name, {
		                  value,
		                  enumerable: true,
		              });
		              return value;
		          };
		      };
		  }
		  
		  class MyClass {
		      private _value = 0;
		  
		      constructor(value: number) {
		          this._value = value;
		      }
		      @range(1, 100)
		      get getValue(): number {
		          return this._value;
		      }
		  }
		  
		  const obj = new MyClass(10);
		  console.log(obj.getValue); // Valid: 10
		  
		  const obj2 = new MyClass(999);
		  console.log(obj2.getValue); // Throw: Invalid!
		  ```
- #### Inheritance