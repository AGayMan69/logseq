- #### `@Qualifier` (constructor injection)
  id:: 6469225e-850e-47b4-8cde-a6f3b720c085
	- ```java
	  ...
	  import org.springframework.beans.factory.annotation.Qualifier;
	  
	  @RestController // rest controller
	  public class DemoController {
	  	...
	      @Autowired
	      public DemoController(@Qualifier("baseballCoach") Coach myCoach) {
	          this.myCoach = myCoach;
	      }
	  	...
	  }
	  ```
#### `@Qualifier` (setter injection)
id:: 6469225e-540c-4efb-ba73-0c2979e1da5a
	- ```java
	  ...
	  import org.springframework.beans.factory.annotation.Qualifier;
	  - @RestController // rest controller
	  public class DemoController {
	  	...
	      @Autowired
	      public void setMyCoach(@Qualifier("trackCoach") Coach myCoach) {
	          this.myCoach = myCoach;
	      }
	  	...
	  }
	  ```