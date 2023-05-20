id:: 6469174d-6f97-4114-b3e0-d86505ca8f2e
```java
@Component // Mark this class as a spring bean
public class CricketCoach implements Coach{
    @Override
    public String getDailyWorkout() {
        return "Practice fast bowling for 15 minutes";
    }
}
```
