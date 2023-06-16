- > Author(s): [[N/A]]
  Url: [[https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html]]
  tags:: #document #programming #[[software development]]
- #### Types by Inferences #TypeScript/Type
	- #+BEGIN_NOTE
	  Typescript generate types with assigning value
	  #+END_NOTE
		- ```ts
		  let helloWorld = "Hello World";
		          let helloWorld: string
		  ```
- #### Defining types #TypeScript/Type #Typescript/Interface
	- ```ts
	  interface User {
	    name: string;
	    id: number;
	  }
	  
	  const user: User = {
	    username: "Hayes",
	  /*Type '{ username: string; id: number; }' is not assignable to type 'User'.
	    Object literal may only specify known properties, and 'username' does not exist in type 'User'.2322Type '{ username: string; id: number; }' is not assignable to type 'User'.
	    Object literal may only specify known properties, and 'username' does not exist in type 'User'. */
	    id: 0,
	  };
	  ```
- #### *Type checking* with interface #TypeScript/Type #Typescript/Interface
	- ```ts
	  interface User {
	    name: string;
	    id: number;
	  }
	   
	  class UserAccount {
	    name: string;
	    id: number;
	   
	    constructor(name: string, id: number) {
	      this.name = name;
	      this.id = id;
	    }
	  }
	   
	  const user: User = new UserAccount("Murphy", 1);
	  ```
- #### Annotating parameters of functions with interfaces #TypeScript/Type #Typescript/Interface #TypeScript/Function
	- ```ts
	  function deleteUser(user: User) {
	    // ...
	  }
	   
	  function getAdminUser(): User {
	    //...
	  }
	  ```
- #### Primitive types in JavaScript #[[TypeScript/Primitive Types]] #TypeScript/Type
	- |Primitive Types|Description|
	  |--|--|
	  |`boolean`|JavaScript Primitive Type|
	  |`bigint`|JavaScript Primitive Type|
	  |`null`|JavaScript Primitive Type|
	  |`number`|JavaScript Primitive Type|
	  |`string`|JavaScript Primitive Type|
	  |`symbol`|JavaScript Primitive Type|
	  |`undefined`|JavaScript Primitive Type|
	  |`any`|allow anything|
	  |`unknown`|ensure someone using this type to declares the type|
	  |`never`|never this type|
	  |`void`|function which returns `undefined` or no return value|
- #### Type & Interfaces #TypeScript/Type #Typescript/Interface
	- #+BEGIN_NOTE
	  Use `interface` if possible, unless need specific features with `type`
	  #+END_NOTE
	- **Type intersection**
		- `type`
		- ```ts
		  type Owl = { nocturnal: true } & BirdType;
		  type Robin = { nocturnal: false } & BirdInterface;
		  ```
		- `interface`
		- ```ts
		  interface Peacock extends BirdType {
		    colourful: true;
		    flies: false;
		  }
		  interface Chicken extends BirdInterface {
		    colourful: false;
		    flies: false;
		  }
		  ```
	- #+BEGIN_NOTE
	  `interfaces` has better error message than `type`
	  #+END_NOTE