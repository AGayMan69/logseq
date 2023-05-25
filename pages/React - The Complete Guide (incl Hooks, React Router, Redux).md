- > Author(s): [[Maximilian Schwarzmüller]]
  Url: [[https://www.udemy.com/course/react-the-complete-guide-incl-redux/]]
  Source code: [https://github.com/academind/react-complete-guide-code](https://github.com/academind/react-complete-guide-code) 
  tags:: #course #programming #[[software development]]
- #### Section 2: Javascript Refresher
  id:: 646e12dc-d932-4b17-98f6-c2d1da781f06
	- `let` and `const` #Javascript/syntax
		- > Use `let` if you want create a something a that is a variable
		- > Use `const` if you are planning on creating a constant value
		- ```js
		  const myName = 'Max';
		  console.log(myName);
		  
		  myName = 'Manu';
		  console.log(myName);
		  // TypeError: Assignment to constant variable.
		  
		  let secName = 'Max';
		  console.log(myName);
		  
		  secName = 'Manu';
		  console.log(myName);
		  // variable secName change to 'Manu'.
		  ```
	- `Arrow Functions` #Javascript/syntax
		- ```js
		  // Tranditional function
		  function printMyName(name) {
		    console.log(name);
		  }
		  
		  printMyName('Max');
		  
		  // Arrow function
		  const printMyName = name => {
		    console.log(name);
		  }
		  
		  printMyName('Max');
		  // Parentheses-less function
		  const printMyName = () => {
		    console.log("Max");
		  }
		  
		  printMyName('Max');
		  // One-liner
		  const multiply = (number) => number * 2;
		  ```
	- `Exports` & `Imports` #Javascript/syntax #Javascript/module
		- `person.js`
			- ```js
			  const person = {
			    name: 'Max'
			  }
			  
			  export default person
			  ```
		- `utility.js`
			- ```js
			  export const clean = () => {
			    return
			  }
			  
			  export const baseData = 10;
			  ```
		- `app.js`
			- **Default export**
				- ```js
				  // import with the name whatever we want
				  import person from './person.js'
				  import prs from './person.js'
				  ```
			- **Named export**
				- ```js
				  // import the stuff by its name
				  import { baseData } from './utility.js'
				  import { clean } from './utility.js'
				  
				  // import alias
				  import { baseData as bD } from './utility.js'
				  
				  // import as bundle
				  import * as bundled from './utility.js'
				  bundled.baseData
				  ```
	- `Classes` #Javascript/syntax #Javascript/OOP
		- ```js
		  class Human {
		    gender = 'male';
		  
		    printGender = () => {
		      console.log(this.gender);
		    }
		  }
		  
		  // Inheritance
		  class Person extends Human {
		    // calling constructor functoin behind the scene
		    name = 'Max';
		    gender = 'female';
		  
		    printMyName = () => {
		      console.log(this.name);
		    }
		  }
		  
		  const person = new Person();
		  // calling method from the class `Person`
		  person.printMyName(); // 'Max'
		  person.printGender(); // 'female'
		  ```
	- `Spread` & `Rest` Operator #Javascript/syntax
		- > `Spread` operator used to split up array elements *OR* object properties
			- ```js
			  // Spread operator
			  const numbers = [1, 2, 3];
			  const newNumbers = [...numbers, 4];
			  
			  console.log(newNumbers); // [1, 2, 3, 4]
			  
			  // Spread operator
			  const person = {
			    name: 'Max'
			  }
			  
			  const newPerson = {
			    ...person,
			    age: 28
			  }
			  
			  console.log(newPerson);
			  // [object Object] {
			  //   age: 28,
			  //   name: "Max",
			  // }
			  ```
		- `Rest` operator used to merge a list of function arguments into an array
			- ```js
			  // Rest operator
			  // filter all element in the array except 1
			  const filter = (...args) => {
			    return args.filter(el => el === 1);
			  }
			  
			  console.log(filter(1, 2, 3)); // [1]
			  ```
	- `Destructuring`
		- > `Destructuring` easily extract array elements *OR* object properties and store them in variables
		- ```js
		  // Array destructuring
		  [a, b] = ['Hello', 'Max']
		  console.log(a) // 'Hello'
		  console.log(b) // 'Max'
		  
		  // Array destructuring
		  const numbers = [1, 2, 3];
		  [num1, , num3] = numbers;
		  console.log(num1, num3);
		  // 1
		  // 3
		  ```
	- `Array Methods` #Javascript/syntax #Javascript/array
		- > `array.map()` take a function as a input and execute on every element on the array
			- ```js
			  const numbers = [1, 2, 3];
			  
			  // take a function as a input and execute on every element on the array
			  const doubleNumArray = numbers.map((num) => {
			    return num * 2;
			  })
			  
			  console.log(numbers); // [1, 2, 3]
			  console.log(doubleNumArray); // [2, 4, 6]
			  ```