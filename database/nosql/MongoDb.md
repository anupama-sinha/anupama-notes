> MongoDB is a document database with the scalability and flexibility that you want with the querying and indexing that you need(Refer [Source](https://www.mongodb.com/what-is-mongodb))

### Why MongoDb
* Document Database(Key Value pairs)
* Document stored in collections
* High availabilty due to replica set(Automatic Failover & Data Redundancy)
* High Horizontal Scaling/Sharding(Keeping data across Distributed Servers)
* Dynamic DB Structure
* Frequent access of coherant documents
* Huge I/O for huge RW requests
* When whole data and index storage in RAM thresold surpassed
* CAP(Consistency,Availability,Partition Tolerance) & PIE(Pattern Flexibility,Infinite Scaling, Efficiency) Theorem suggests so.

### Points to Note
* Default Port : 27017
* Sort attribute is BSON Object
* Sorting can be done in direct ascending(1) or descending(-1) i.e. invesely proportional) format
* All prefix attributes have to be indexed mandatorily for any random key sorting
* No indexing leads to Blocked indexing which uses 100 MB of memory.[Winning Plan Stage : SORT,Scan Type: COLLSCAN(), direction : forward/backward]
* Indexed sorting is non blocking[Winning Plan Stage : FETCH, Scan Type: IXSCAN(), direction : forward/backward]
* Check SQL Queries with property : spring.jpa.show-sql=true
* Supports denormalized schema as no support for JOINs as in RDBMS

### Download & Setup
* Download [Mongo DB Community Server with Compass GUI](https://www.mongodb.com/try/download/community)
* Environment Variables - PATH(Append at end) : C/ProgramFiles/mongodb/bin
* Create data folder : "C:\data\db"

### Various Commands 
|Decription|Commands|
|--|--|
|Start Mongo DB Server| mongod --port 27017 -dbpath "C:\data\db"|
|Connect to Mongo DB Server | mongo |
|Check Collections | show collections |
|Check Databases | show dbs|
|Use a Database | use db-name|
|Check current Database | db |
|Insert data to database | db.collection-name.insert({"id":"1","name":"Anupama","domain":"Dev"})|
|Get all records | db.collection-name.find()|
|Get all records in good format | db.collection-name.find().pretty()|
|Get a specific id | db.collection-name.find({id:"1"})|
|Get sorted data in Ascending Order based on id | db.collection-name.find().sort({id:1})|
|Get sorted data in Descending Order based on id | db.collection-name.find().sort({id:-1})|
|Get sorted data in Ascending Order based on multiple fields | db.collection-name.find().sort({id:1,name:-1})|
|Check database query strategy | db.collection-name.find({id:"1"}).explain()|
|Setting limit to Row Size | db.collection-name.find({id:"1"}).limit(2)|
|Check Database Stats | db.stats()|
|Get DB Help| db.help()|
|Create Index|db.User.createIndex({_id:1})|
|Check DB Indexes | db.collection-name.getIndexes()|
|Create Hashed Indexes | db.User.createIndex({_id:"hashed"})|

### Dependencies
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
    <version>2.3.3.RELEASE</version>
</dependency>

```
### Properties
> spring.data.mongodb.uri=mongodb://root:root@localhost:27017/MongoDb

### Spring Boot Implementation
#### POJO
```java
@Document(collection="Department")
@Getter
@Setter
public class Department {
	@Indexed(name = "id",direction=IndexDirection.ASCENDING)
	@Id
    private String id;
	
    @Indexed(name = "name",direction=IndexDirection.ASCENDING)
    @Field(name="name")
    private String name;
    
    @Transient
    private String domain;
	
}
```
#### Repository
```java
@Repository
public interface DepartmentRepository  extends MongoRepository<Department, String> {
	
	//Option#1 : Have query for complex queries-Database changes not supported. Not type safety. Better go with Query DSL
	@Query(value = "{'department.name': ?0}", fields = "{'department' : 0}")
	Department findDepartmentByName(String name);
	
	//Option#2 : Auto generated Spring Data Queries with Field Names(Long Method Names)
	Collection<Department> findDepartmentByName(String name);
	Collection<Department> findByNameStartingWith(String regexp);
	Collection<Department> findByYearBetween(int yearGT, int yearLT);
	Collection<Department> findByNameLikeOrderByYearAsc(int year);
	@Query("{ 'name' : { $regex: ?0 } }")
	Collection<Department> findDepartmentByRegexpName(String regexp);
	@Query("{ 'age' : { $gt: ?0, $lt: ?1 } }")
	Collection<Department> findDepartmentByYearBetween(int yearGT, int yearLT);
}
```	

#### Controller using MongoRepository
* Easier but low level control not acheived
```java
@RestController
@RequestMapping("deptartments")
public class MongoRepoController {
	
	private DepartmentRepository deptRepository;
	
	@PostMapping
	public String save(@RequestBody Department department) {
		//Option 1 : Checks ID, then accordingly inserts or updates
		deptRepository.save(department);			
		
		//Option 2 : No ID-New ID generated-Else duplicate key exception
		deptRepository.insert(department);			
		
		//Option 3 : Batch Insert
		Collection<Department> dept = Arrays.asList(department,department);
		deptRepository.insert(dept);
		
		return "Department saved successfully";
	}
	
	@GetMapping
	public Collection<Department> findAll() {
		Predicate predicate = qDepartment.name.eq("Computers");
		List<Department> users = (List<Department>) deptRepository.findAll(predicate);*/
		return deptRepository.findAll();
	}
	
	@DeleteMapping
	public void deleteById(@PathVariable String id) {
		Query query = new Query();
		query.addCriteria(Criteria.where("id").is(id));
		deptRepository.deleteById(id);
	}
}
```	

#### Controller using Template
* Difficult but low level control for complex query achieved
```java
@RestController
@RequestMapping("departments-template")
public class MongoTemplateController {
	
	private MongoTemplate mongoTemplate;
	
	@PostMapping
	public String save(@RequestBody Department department) {
		//Option 1 : Checks ID, then accordingly inserts or updates
		mongoTemplate.save(department);			
		
		//Option 2 : No ID-New ID generated-ELse duplicate key exception
		mongoTemplate.insert(department);		
		
		//Option 3 : Batch Insert
		Collection<Department> dept = Arrays.asList(department,department);
		mongoTemplate.insertAll(dept);
		
		return "Department saved successfully";
	}
	
	@GetMapping
	public Collection<Department> findAll() {
		return mongoTemplate.findAll(Department.class);
	}
	
	@GetMapping
	public List<Department> findById(@PathVariable String id) {
		Query query = new Query();
		query.addCriteria(Criteria.where("id").is(id));
		query.addCriteria(Criteria.where("name").regex("^A"));	
		query.addCriteria(Criteria.where("name").regex("c$"));
		query.addCriteria(Criteria.where("year").lt(2005).gt(2020));
		
		//Pageable Request
		final Pageable pageableRequest = PageRequest.of(0, 2);
		query.with(pageableRequest);
		
		List<Department> dept = mongoTemplate.find(query, Department.class);
		return dept;
	}
	
	@DeleteMapping
	public void deleteById(@PathVariable String id) {
		Query query = new Query();
		query.addCriteria(Criteria.where("id").is(id));
		mongoTemplate.remove(query, Department.class);
	}
}
```
	
### Query DSL(Work in Progress)

### Data Migration using MongoBee(Work in Progress)

### References
* https://docs.mongodb.com/guides/
* https://docs.mongodb.com/manual/introduction/
* https://youtu.be/EE8ZTQxa0AM
