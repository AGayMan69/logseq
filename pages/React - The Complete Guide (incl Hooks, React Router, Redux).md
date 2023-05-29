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
- #### Section 3: React Components Basics
  id:: 64706f4e-1a1b-4dd0-bf6c-a0aea14002af
	- **First Component** #React/Component
		- Create our first custom component
			- ```jsx
			  function ExpenseItem() {
			    return <h2>Expense item!</h2>
			  }
			  
			  export default ExpenseItem;
			  ```
		- Use `export default` to export the `ExpenseItem` Component
		-
		- Reuse custom component `<ExpenseItem>` at `App.js` with `import`
			- ```jsx
			  import ExpenseItem from "./ExpenseItem";
			  
			  function App() {
			    return (
			      <div>
			        <h2>Let's get started!</h2>
			        <ExpenseItem></ExpenseItem>
			      </div>
			    );
			  }
			  
			  export default App;
			  ```
	- #### Root element #React/Component
		- > There must be only one root element exist in the *react component*
			- ```jsx
			  function ExpenseItem() {
			    // More than one root element, doesn't work
			    return <div>Date
			    </div>
			      <div><h2>Title</h2><div>Amount</div></div>;
			  }
			  
			  export default ExpenseItem;
			  ```
		- One root element *react component*
			- ```jsx
			  function ExpenseItem() {
			    return (
			      <div>
			        <div>Date</div>
			        <div>
			          <h2>Title</h2>
			          <div>Amount</div>
			        </div>
			      </div>
			    );
			  }
			  
			  export default ExpenseItem;
			  ```
	- #### Css styling to Component #React/Component #React/Styling
		- > [[$red]]==.css== file must be explicitly tell **React** to *inject* to the application
		- use `import` to inject the [[$red]]==.css== file to the component directly
			- ```jsx
			  import "./ExpenseItem.css";
			  
			  function ExpenseItem() {
			    return (
			  	...
			    );
			  }
			  
			  export default ExpenseItem;
			  ```
			- #+BEGIN_IMPORTANT
			  Notice that **React** use `className` instead of `class`, because the code inside `return(...)` is [[$red]]==jsx== but not [[$red]]==html== code
			  #+END_IMPORTANT
	- #### Displaying with Expressions #React/Component
		- **React** component can display data with `{}`  {{cloze (expression)}}
			- ```jsx
			  function ExpenseItem() {
			    // js code should place inside the function component
			    const expenseDate = new Date(2021, 2, 28);
			    const expenseTitle = "Car Insurance";
			    const expenseAmount = 294.67;
			  
			    return (
			      <div className="expense-item">
			        <div>{expenseDate.toISOString()}</div>
			        <div className="expense-item__description">
			          <h2>{expenseTitle}</h2>
			          <div className="expense-item__price">${expenseAmount}</div>
			        </div>
			      </div>
			    );
			  }
			  
			  export default ExpenseItem;
			  ```
	- DOING Passing data with `props` #React/Component #React/State
	  :LOGBOOK:
	  CLOCK: [2023-05-27 Sat 13:04:48]
	  :END:
		- declare *expenses* for component data
			- ```jsx
			  const expenses = [
			      {
			        id: "e1",
			        title: "Toilet Paper",
			        amount: 94.12,
			        date: new Date(2020, 7, 14),
			      },
			      { id: "e2", title: "New TV", amount: 799.49, date: new Date(2021, 2, 12) },
			      {
			        id: "e3",
			        title: "Car Insurance",
			        amount: 294.67,
			        date: new Date(2021, 2, 28),
			      },
			      {
			        id: "e4",
			        title: "New Desk (Wooden)",
			        amount: 450,
			        date: new Date(2021, 5, 12),
			      },
			    ];
			  ```
		- passing the *expenses* data to the component through `{}`
			- ```jsx
			  import ExpenseItem from "./ExpenseItem";
			  
			  function App() {
			    return (
			      <div>
			        <h2>Let's get started!</h2>
			        <ExpenseItem
			          title={expenses[0].title}
			          amount={expenses[0].amount}
			          date={expenses[0].date}
			        ></ExpenseItem>
			        <ExpenseItem
			          title={expenses[1].title}
			          amount={expenses[1].amount}
			          date={expenses[1].date}
			        ></ExpenseItem>
			        <ExpenseItem
			          title={expenses[2].title}
			          amount={expenses[2].amount}
			          date={expenses[2].date}
			        ></ExpenseItem>
			        <ExpenseItem
			          title={expenses[3].title}
			          amount={expenses[3].amount}
			          date={expenses[3].date}
			        ></ExpenseItem>
			      </div>
			    );
			  }
			  
			  export default App;
			  ```
		- In [[$red]]==ExpenseItem.js== handle the props.date data passed by [[$red]]==App.js==
			- ```jsx
			  import "./ExpenseItem.css";
			  
			  function ExpenseItem(props) {
			    return (
			      <div className="expense-item">
			        <div>{props.date.toISOString()}</div>
			        <div className="expense-item__description">
			          <h2>{props.title}</h2>
			          <div className="expense-item__price">${props.amount}</div>
			        </div>
			      </div>
			    );
			  }
			  
			  export default ExpenseItem;
			  ```
	- #### DOING Splitting multiple component #React/Component
	  :LOGBOOK:
	  CLOCK: [2023-05-27 Sat 13:04:44]
	  :END:
		- draw
		- In [[$red]]==ExpenseItem.js==
			- ```jsx
			  function ExpenseItem(props) {
			    return (
			      <div className="expense-item">
			        <ExpenseDate date={props.date} />
			   		...
			      </div>
			    );
			  }
			  ```
		- In [[$red]]==ExpenseDate.js==
			- ```jsx
			  import "./ExpenseDate.css";
			  
			  function ExpenseDate(props) {
			    const month = props.date.toLocaleString("en-US", { month: "long" });
			    const day = props.date.toLocaleString("en-US", { day: "2-digit" });
			    const year = props.date.getFullYear();
			  
			    return (
			      <div className="expense-date">
			        <div className="expense-date__month">{month}</div>
			        <div className="expense-date__day">{day}</div>
			        <div className="expense-date__year">{year}</div>
			      </div>
			    );
			  }
			  
			  export default ExpenseDate;
			  ```
	- #### Wrapper component #React/Component
		- **To create reusable component, there are few points to remind**
			- In [[$red]]==Card.js==, we need to add `props.children` to simulate the act of `div` wrapping the children
				- ```jsx
				  ...
				  
				  function Card(props) {
				  	...
				    return <div className={classes}>{props.children}</div>;
				  }
				  
				  export default Card;
				  ```
			- Unlike **html tags**, we also need to pass the class name into the custom component with `props`
				- ```jsx
				  ...
				  
				  function Card(props) {
				    const classes = "card " + props.className;
				    return ...;
				  }
				  
				  export default Card;
				  ```
		- **Usage of the wrapper component**
			- ```jsx
			  function ExpenseItem(props) {
			    return (
			      <Card className="expense-item">
			          ...
			      </Card>
			    );
			  }
			  ```
- #### Section 4: React State & Events
  id:: 6471c9a5-c112-48f7-b8ce-849f01ec1926
	- Adding `Event Listener` #React/Component #React/DOM #React/Event
		- In **React**, event listener is added in jsx with the property provided by **React** such as `onClick`, `onBlur`, etc...
			- ```jsx
			  const ExpenseItem = (props) => {
			    const clickHandler = () => {
			      console.log("Clicked !!!");
			    };
			    return (
			      <Card className="expense-item">
			  		...
			        <button onClick={clickHandler}>Change Title</button>
			      </Card>
			    );
			  };
			  
			  ```
	- > Component function are just *function call* #React/Component #React/State
		- React doesn't rerender whenever the values of the `props` change
			- ```jsx
			  const ExpenseItem = (props) => {
			    let title = props.title;
			  
			    // not update due to how react works
			    const clickHandler = () => {
			      title = "Updated";
			    };
			    return (
			      <Card className="expense-item">
			        <div className="expense-item__description">
			          <h2>{title}</h2>
			        </div>
			        <button onClick={clickHandler}>Change Title</button>
			      </Card>
			    );
			  };
			  ```
	- First React `State` #React/State #React/Hook
		- We create state in **React** using `useState`
			- ```jsx
			    const [title, setTitle] = useState(props.title);
			  ```
			- `useState` create a *special* variable where changes will lead the component function to be executed again
			- To change the variable, instead of using `title = 'something'`, in **React** we need to call the special functoin `setTitle()` to set the value of the variable and tell **React** to re-evaluated this component function
		- **==Example usage==**
			- ```jsx
			  const ExpenseItem = (props) => {
			    const [title, setTitle] = useState(props.title);
			  
			    const clickHandler = () => {
			      setTitle("Clicked !!!");
			    };
			    return (
			      <Card className="expense-item">
			        <div className="expense-item__description">
			          <h2>{title}</h2>
			        </div>
			        <button onClick={clickHandler}>Change Title</button>
			      </Card>
			    );
			  };
			  
			  export default ExpenseItem;
			  
			  ```
	-
	- Updating multiple `State` #React/State #React/Hook
		- setting a variable that store multiple `State`
			- ```jsx
			    const [userInput, setUserInput] = useState({
			      enteredTitle: "",
			      enteredAmount: "",
			      enteredDate: "",
			    });
			  ```
		- Update `handler` to the following
			- ```jsx
			    const DateChangeHandler = (event) => {
			      // setEnteredDate(event.target.value);
			      setUserInput((prevState) => {
			        return { ...prevState, enteredDate: event.target.value };
			      });
			    };
			  ```
	- Two-Way Binding #React/State
		- ```jsx
		            <input
		              type="text"
		              value={userInput.enteredTitle}
		              onChange={titleChangeHandler}
		            />
		  ```
		- ```jsx
		    const titleChangeHandler = (event) => {
		      // setEnteredTitle(event.target.value);
		      // React gives latest snapshot of the state
		      setUserInput((prevState) => {
		        return { ...prevState, enteredTitle: event.target.value };
		      });
		    };
		  ```
	- *Bottom-up* communication #React/State
		- First create a `Handler` from the parent component
			- ```jsx
			    const saveExpenseDataHandler = (enteredExpenseData) => {
			      const expenseData = {
			        ...enteredExpenseData,
			        id: Math.random().toString(),
			      };
			      console.log(expenseData);
			      props.onAddExpense(expenseData);
			    };
			  ```
		- Passing the `Handler` through props to children component
			- ```jsx
			  <div className="new-expense">
			        <ExpenseForm onSaveExpenseData={saveExpenseDataHandler} />
			      </div>
			  ```
		- Calling the `Handler` from the children component
			- ```jsx
			    const submitHandler = (event) => {
			      event.preventDefault();
			  	...
			      props.onSaveExpenseData(expenseData);
			      ...
			    };
			  ```
- #### Section 5: React LIst & Conditional content
  id:: 6472005c-32d1-405a-ae78-73c6fabe5250
	-