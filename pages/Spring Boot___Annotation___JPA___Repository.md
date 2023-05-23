- ```java
  import com.bedge.cruddemo.entity.Student;
  import jakarta.persistence.EntityManager;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Repository;
  
  @Repository // for Spring component scanning
  public class StudentDAOImpl implements StudentDAO{
      // define field for entity manager
      private EntityManager entityManager;
  
      // inject entity manager using constructor injection
      @Autowired
      public StudentDAOImpl(EntityManager entityManager) {
          this.entityManager = entityManager;
      }
  
  	...
  }
  ```