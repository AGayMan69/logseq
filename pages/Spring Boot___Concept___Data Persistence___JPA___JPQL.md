- > Query language for retrieving objects of JPA
- similar to [[SQL]]
	- *where, like, order by, join, in, etc...*
-
- JPQL is based on **entity name** and **entity fields**
- ==Getting all students==
	- ```java
	  TypedQuery<Student> theQuery = entityManager.createQuery("FROM Student", Student.class);
	  List<Student>students =  theQuery.getResultList();
	  ```
-
- ==With parameter==
	- ```java
	  TypedQuery<Student> theQuery = entityManager.createQuery(
	                                      "FROM Student WHERE lastName=:theData", Student.class);
	  List<Student>students =  theQuery.getResultList();
	  ```
-
- ==Like predicate==
	- ```java
	  TypedQuery<Student> theQuery = entityManager.createQuery(
	                                      "FROM Student WHERE email LIKE `@luv2code.com`", Student.class);
	  List<Student>students =  theQuery.getResultList();
	  ```
-
- ==Named Parameters==
	- ```java
	      public List<Student> findByLastName(String theLastName) {
	          TypedQuery<Student> theQuery = entityManager.createQuery(
	                                          "FROM Student WHERE lastName=:theData", Student.class);
	  
	          theQuery.setParameter("theData", theLastName);
	  
	          return theQuery.getResultList();
	      }
	  ```