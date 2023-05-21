- #+BEGIN_NOTE
  For mixing both of them, [@Qualifier](Spring Boot/Concept/Dependency Injection/Qualifier) has higher priority than [@Primary](Spring Boot/Concept/Dependency Injection/Primary)
  #+END_NOTE
-
- For example if we have `@Qualifier("cricketCoach)` and `@Primary` added to TrackCoach, Spring Boot will make use of `CricketCoach
-
- **What situation to use**
	- [@Qualifier](Spring Boot/Concept/Dependency Injection/Qualifier) are more specific, and higher priority
	- [@Primary](Spring Boot/Concept/Dependency Injection/Primary) doesn't change the object source code