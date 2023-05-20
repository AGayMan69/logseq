- #### `@Autowire` (constructor injection)
  id:: 64691767-9988-4e09-ab83-0806670c62f6
	- ```java
	  @Autowired // Inform Spring to inject the required dependency (optional if only 1 constructor)
	  DemoController(Coach myCoach) {
	     // Coach only have 1 child so CricketCoach is injected
	     this.myCoach = myCoach;
	  }
	  ```
- #### `@Autowired` (setter injection)
  id:: 1aed9080-9f1e-43d9-9fb4-a0ce9757f3e3
	- ```java
	  @RestController // rest controller
	  public class DemoController {
	      // dependency
	      private Coach myCoach;
	  
	      @Autowired
	      public void setMyCoach(Coach myCoach) {
	          this.myCoach = myCoach;
	      }
	  
	      @GetMapping("/dailyworkout")
	      public String getDailyWorkout() {
	          return myCoach.getDailyWorkout();
	      }
	  }
	  ```
- #### `@Autowired` (field injection)
  id:: 0d2fadb6-2ad9-4bd0-8c0f-c327ea2d3788
	- ```java
	  @RestController // rest controller
	  public class DemoController {
	      
	      @Autowired
	      private Coach myCoach;
	  
	  	// no need for constructors or setters
	    
	      @GetMapping("/dailyworkout")
	      public String getDailyWorkout() {
	          return myCoach.getDailyWorkout();
	      }
	  }
	  ```