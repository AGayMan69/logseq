- ### Step 1: Add new method to [[Data Access Object (DAO)]] interface
	- ```java
	  public interface StudentDAO {
	  	...
	      Student findById(Integer id);
	  }
	  ```
-
- ### Step 2: Add new method to [[Data Access Object (DAO)]] implementation
	- ```java
	  @Repository // for Spring component scanning
	  public class StudentDAOImpl implements StudentDAO{
	  	...
	        
	      @Override
	      public Student findById(Integer id) {
	        	// `Student.class` for entity class, `id` for primary key
	          return entityManager.find(Student.class, id);
	        	// return null if not found
	      }
	  }
	  ```
	- Since we are doing a query, there is no need to add [@Transactional](Spring Boot/Concept/Data Persistence/JPA/Transactional)
-
- ### Step 3: Update main app
	- ```java
	  @SpringBootApplication
	  public class CruddemoApplication {
	  	...
	  
	  	@Bean
	  	public CommandLineRunner commandLineRunner(StudentDAO studentDAO) {
	  		return runner -> {
	  			readStudent(studentDAO);
	  		};
	  	}
	  
	  	private void readStudent(StudentDAO studentDAO) {
	  		// create a student object
	  		System.out.println("Creating new student object ...");
	  		Student tempStudent = new Student("Daffy", "Duck", "daffy@luv2code.com");
	  
	  		// save the student
	  		System.out.println("Saving the student ...");
	  		studentDAO.save(tempStudent);
	  
	  		// display id of the saved student
	  		int theId = tempStudent.getId();
	  		System.out.println("Saved student. Generated id: " + theId);
	  
	  		// retrieve student based on the id: primary key
	  		System.out.println("Retrieving student with id: " + theId);
	  		Student myStudent = studentDAO.findById(theId);
	  
	  		// display student
	  		System.out.println("Found the student: " + myStudent);
	  	}
	    
	    	...
	  }
	  ```