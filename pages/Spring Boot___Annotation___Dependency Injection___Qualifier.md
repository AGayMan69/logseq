- ==Constructor injection==
  id:: 6469225e-850e-47b4-8cde-a6f3b720c085
	- id:: 6469225e-a575-491b-8e41-0edd035e9214
	  ```java
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
- ==Setter injection==
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