- `@Configuration`
	- ```java
	  import org.springframework.context.annotation.Configuration;
	  ...
	    
	  @Configuration
	  public class SportConfig {
	  	...
	  }
	  
	  ```
-
- `@Bean`
	- ```java
	  import org.springframework.context.annotation.Bean;
	  import org.springframework.context.annotation.Configuration;
	  ...
	  
	  @Configuration
	  public class SportConfig {
	      // custom bean id
	      @Bean("aquatic")
	      // bean id default to the method name
	      public Coach swimCoach() {
	          return new SwimCoach();
	      }
	  }
	  
	  ```