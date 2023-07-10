- Author(s): [[N/A]]
  Url: [[https://github.com/gibbok/typescript-book#typescript-an-introduction]]
  tags:: #document #programming #[[software development]]
- #### TypeScript code generation #[[TypeScript/Code generation]]
	- The TypeScript compiler has two main responsibilities:
		- checking for type errors
		  logseq.order-list-type:: number
		- and compile code to *JavaScript*
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
				  type j<Type> = {
				    [Property in keyof Type as `prefix_${string & Property}`]: () => Type[Property];
				  };
				  type X = {
				  	a: string;
				  	b: number;
				  }
				  type Y = J<X>;
				  ```