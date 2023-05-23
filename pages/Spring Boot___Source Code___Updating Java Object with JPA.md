- ### Step 1: Add new method to [[Data Access Object (DAO)]] interface
	- ```java
	  public interface StudentDAO {
	  	...
	        
	      void update(Student theStudent);
	  }
	  ```
-
- ### Step 2: Add new method to [[Data Access Object (DAO)]] implementation
	- ```java
	  import com.bedge.cruddemo.entity.Student;
	  import jakarta.persistence.EntityManager;
	  import jakarta.transaction.Transactional;
	  ...
	  
	  @Repository // for Spring component scanning
	  public class StudentDAOImpl implements StudentDAO {
	      private EntityManager entityManager;
	  	...
	  
	      @Override
	      @Transactional
	      public void update(Student theStudent) {
	          entityManager.merge(theStudent);
	      }
	  }
	  
	  ```
-
- ### Step 3: Update main app
- ```java
  @SpringBootApplication
  public class CruddemoApplication {
  	...
        
      @Bean
      public CommandLineRunner commandLineRunner(StudentDAO studentDAO) {
          return runner -> {
  
              updateStudent(studentDAO);
          };
      }
  
      private void updateStudent(StudentDAO studentDAO) {
  
          // retrieve student based on the id: primary key
          int studentId = 1;
          System.out.println("Getting student with id: " + studentId);
          Student myStudent = studentDAO.findById(studentId);
  
          // change first name to "Scooby"
          System.out.println("Updating student ...");
          myStudent.setFirstName("John");
  
          // update the student
          studentDAO.update(myStudent);
  
          // display the updated student
          System.out.println("Updated student: " + myStudent);
      }
    
  }
  ```
- ### Updating multiple object
	- ```java
	  int numRowsUpdated = entityManager.createQuery(
	  							"UPDATE Student SET lastName=`Tester`")
	    							.executeUpdate();	
	  ```