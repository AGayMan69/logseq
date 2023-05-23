- ==Constructor injection==
	- **How it normally works**
		- Create a spring bean using [@Component](Spring Boot/Annotation/Dependency Injection/Component)
		- {{embed ((6469174d-6f97-4114-b3e0-d86505ca8f2e))}}
		- Add annotation to the constructor using [@Autowired](Spring Boot/Annotation/Dependency Injection/Autowired)
		- {{embed ((64691767-9988-4e09-ab83-0806670c62f6))}}
	-
	- **Behind the scene**
		- [[Spring Boot]] will create a new instance of a default implementation of `Coach` ( `Coach` is the dependency of `DemoController` ) - `CricketCoach`, it also inject the dependency
		- ```java
		  Coach theCoach = new CricketCoach();
		  DemoController demoConroller = new DemoController(theCoach);
		  ```
	-
	- **What Situation to use**
		- When the class have **required dependencies**
		- Recommended by the [spring.io](https://spring.io/) development team
	-
-
- ==Setter injection==
	- **How it normally works**
		- Create a spring bean using [@Component](Spring Boot/Annotation/Dependency Injection/Component)
		- {{embed ((6469174d-6f97-4114-b3e0-d86505ca8f2e))}}
		- Add annotation to the setter using [@Autowired](Spring Boot/Annotation/Dependency Injection/Autowired)
		- {{embed ((1aed9080-9f1e-43d9-9fb4-a0ce9757f3e3))}}
	-
	- **It can also inject to any method name**
		- ```java
		  @RestController // rest controller
		  public class DemoController {
		  	...
		      @Autowired
		      public void doSomeStuff(Coach myCoach) {
		          this.myCoach = myCoach;
		      }
		  	...
		  }
		  ```
	-
	- **Behind the scene**
		- [[Spring Boot]] will create a new instance of a default implementation of `Coach`(`Coach` is the dependency of `DemoController`) - `CricketCoach`, it also inject the dependency through setter injection.
		- ```java
		  Coach theCoach = new CricketCoach();
		  DemoController demoConroller = new DemoController(theCoach);
		  ```
		- **What Situation to use**
			- When the class have **optional dependencies**
				- {{embed ((64691767-9988-4e09-ab83-0806670c62f6))}}
				- #### `@Autowire` (constructor injection)
				  collapsed:: true
					- ```java
					  @Autowired // Inform Spring to inject the required dependency (optional if only 1 constructor)
					  DemoController(Coach myCoach) {
					     // Coach only have 1 child so CricketCoach is injected
					     this.myCoach = myCoach;
					  }
					  ```
			- **optional dependencies** mean if the dependency is not provided, your app can provide some default logic
			- Recommended by the [spring.io](https://spring.io/) development team
-
- ==Field Injection==
	- **No longer cool**
		- Field injection was popular on the early days
		- It makes the code harder for unit testing
	-
	- **How it normally works**
		- Inject dependency by setting the field values on the class directly (private fields as well)
		- Accomplished by using [[Java Reflection]]
		- using [@Autowired](Spring Boot/Annotation/Dependency Injection/Autowired)
		- {{embed ((0d2fadb6-2ad9-4bd0-8c0f-c327ea2d3788))}}
	-
	- **What Situation to use**
		- When the class have **required dependencies**
		- Not Recommended by the [spring.io](https://spring.io/) development team