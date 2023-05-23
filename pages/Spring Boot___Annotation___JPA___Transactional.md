- ```java
  import jakarta.transaction.Transactional;
  ...
  @Repository 
  public class StudentDAOImpl implements StudentDAO{
  	...
  
      @Override
      @Transactional
      public void save(Student theStudent) {
          entityManager.persist(theStudent);
      }
  }
  ```