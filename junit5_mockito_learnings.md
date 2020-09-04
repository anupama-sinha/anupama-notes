# Junit 5 & Mockito Learnings

> It’s only human to make mistakes

### Why Unit Testing?
* Compile time errors handled by Compiler
* Maximum exceptions handled by Coder.
* But there can still be various scopes of logical errors and   missing out on handling exceptions and n-number of reasons always.

### History
| Year | Version |
| --- | --- |
|2002 |JUnit |
|2006 | JUnit4 |
| 2017 | JUnit5 |

### Why JUnit5?
1. More than 10 years old
2. Latest features(Functional Programming-Lambda,etc) not supported
3. Bugs & features got pilled up
4. Older versions supported monolithic architecture
5. Various new Test Design Patterns commonly used nowadays were not supported

### JUnit5 Architecture
1. **Helps in Running JUnit**
```
Test Engine/Platform/Runner/Core : junit-jupiter-engine
```

2. **APIs for writing Unit Tests**
```
JUnit API : junit-jupiter-api
Vintage API(Junit4 API & older ones supported) : junit-vintage-engine
3rd Party API : Support for 3rd Party APIs
```

### Annotations for Test Lifecycle Hooks

* @BeforeAll— Before beginning of class
* @BeforeEach— Before each Test cases 
* @Test — Test Instance(Lifecycle:PER_METHOD is default. Class instance created each time for Test)
* @AfterEach— After each Test
* @AfterAll — After everything in Class runs

Dependency across Tests isn’t a good practice.

### Best Practices for Unit Testing
1. Maintain Code Readability
2. Follow Assert Convention(expected, actual)
3. Ensure 100% code coverage as far as possible(Eg.Tools-EclEmma in IDE & Generate Jacoco reports)
4. Give Proper Name of methods or use Display Annotation
5. Mock single class
6. Use Assert & Verify for all Tests. Check for Exceptions wherever applicable.
7. Follow Test Driven Development(TDD)
8. Have sections for Given, When & Then
9. Use FIRST Principle(Fast, Independent, Repeatable, Self Validating, Timely)
10. Follow Test Design Patterns

### Mockito Framework Annotations
* @Mock : Mocks the dependency classes
* @InjectMocks : Mocks the Class Under Test(CUT)/System Under Test(SUT)
* @Captor : Allows to capture argument passed to methods for inspection
* @RunWith : Multiple Runners not allowed. So, Rules annotation can be used instead to include multiple runners.
* @Spy : Creates spies on real objects and helps in overriding too

### JUnit Evolution over the Years in Depth
## 1. Stubbing Classes
* Create Class Instance(Object)
* Requires change each time, class is changed. Not a good practice.

```
class SubtractOperationTest{
    @Test
    @Display("TestSubtractOperation")
    void testSubtractOperation(){
      SubtractOperation testClass = new SubtractOperation();
      
      //given
      int expected = 7;
      //when
      int actual = testClass.subtract(14,7);
      //then
      assertEquals(expected,actual,"Subtract Operation Testing");
    }
}
```

## 2. Mocking Classes
```xml
<dependency>
     <groupId>org.mockito</groupId>
     <artifactId>mockito-junit-jupiter</artifactId>
     <scope>test</scope>
 </dependency>
```
```
@ExtendWith(MockitoExtension.class)
class SubtractOperationMockTest{
    @InjectMocks
    SubtractOperation testClass
    
    @Mock
    CheckNumber mockCheckNumber;
    
    @Test
    @Display("TestSubtractOperation")
    void testSubtractOperation(){
      //given
      int expected = 7;
      //when
      when(mockCheckNumber.checkValidity()).andReturn(true);
      int actual = testClass.subtract(14,7);
      //then
      assertEquals(expected,actual,"Subtract Operation Testing");
    }
 }
 ```

## 3. Controller Testing with WebMvcTest annotation
* Loads only controller components of application(Controller,RestController,JsonComponent)
* Auto-configure Spring MVC, Jackson, Gson, Message converters, etc.
* Configures MockMVC — Doesn’t need to start the application completely as in Integration Testing
* Used with MockBean for adding mocks for dependencies in Spring Application Context
* Use MockMvc to mock different Request Mappings.

```
@WebMvcTest(ProductController.class)
@TestInstance(TestInstance.Lifecycle.PER_METHOD)
class ProductControllerTest{
    @Autowired
    private MockMvc mockMvc;
    
    @MockBean
    private ProductService mockProductService;
    
    @Test
    @Display("TestGetProduct")
    void testGetProduct(){
      //given
      RequestBuilder request = MockMvcRequestBuilders.get("/products");
      //when
      MvcResult result = mockMvc.perform(request).andExpect(status().isOk()).andReturn();
      //then
      assertEquals(200,result.getResponse().getContentAsString(),"Check Get Product");
    }
}
```

## 4. Service Layer Testing
* @InjectMocks : Creates objects and inject mocked dependencies. But tests actual object method.
* @Mock : Creates mock objects for the Autowired dependencies
* @MockitoJUnitRunner : In  JUnit4, MockitoAnnotations.initMocks(this) is called to initialize mock and inject in Test. So called before Test.
* @MockitoExtension : Initializes mocking in only specific test. JUnit Jupiter equivalent of JUnit4 MockitoJUnitRunner. Here MockBean doesn’t work
* @SpringExtension — Implements a lot more extensions than MockitoExtensions. Integrates the Spring TestContext Framework into JUnit 5’s Jupiter programming model. Hence, MockBean works with this.

```
@ExtendWith(MockitoExtension.class)
@TestInstance(TestInstance.Lifecycle.PER_METHOD)
class ProductServiceTest{
    @InjectMocks
    private ProductService mockProductService;
    
    @Mock
    private ProductRepository mockProductRepository;
    
    @Test
    @Display("TestGetProduct")
    void testGetProduct(){
      //given
      List<Product> products = new ArrayList<Products>(){
        new Product(1,"Fruits");
        new Product(2,"Vegetables");
      };
      //when
      when(mockProductRepository.findAll()).thenReturn(products);
      List<Product> resultProducts = mockProductService.getAllProducts();
      //then
      assertEquals(2,resultProducts.size(),"Check Count of Products");
      verify(mockProductRepository).findAll();
    }
}
```

## 5. Respository Layer Testing with DataJpaTest Annotation
* Configures in-memory embedded database
* Transactional & rolls back at end of @Test
* Scans @Entity classes & configures Spring Data JPA Repository annotated with @Repository

```
@DataJpaTest
class ProductRepositoryTest{
    @MockBean
    private ProductRepository mockProductRepository;
    
    @Test
    @Display("TestGetProduct")
    void testGetProduct(){
      //given
      List<Product> products = new ArrayList<Products>(){
        new Product(1,"Fruits");
        new Product(2,"Vegetables");
      };
      //when
      when(mockProductRepository.findAll()).thenReturn(products);
      List<Product> resultProducts = mockProductRepository.findAll();
      //then
      assertEquals(2,resultProducts.size(),"Check Count of Products");
      verify(mockProductRepository).findAll();
    }
}
```

## 6. Spying
* Almost creates a class
* Also can override a class 
* Only used for dependencies like @Mock. But not for SUT as @InjectMocks does

```
@ExtendWith(MockitoExtension.class)
class SubtractOperationSpyTest{
    @InjectMocks
    SubtractOperation testClass
    
    @Spy
    CheckNumber spyCheckNumber;
    
    @Test
    @Display("TestSubtractOperation")
    void testSubtractOperation(){
      //given
      int expected = 7;
      //when
      when(spyCheckNumber.checkValidity()).andReturn(true);
      int actual = testClass.subtract(14,7);
      //then
      assertEquals(expected,actual,"Subtract Operation Testing");
    }
 }
 ```

### Miscellaneous Points
1. @RunWith : Loads Spring Application Context and relevant beans. In JUnit5, all annotations have this by default. So not required.
2. JUnit5 supports JUnit4 as vintage engine.
3. Include all static imports to IDE Preference>Editor>Favourites
4. Surefire Plugin : Currently for JUnit compatibility needs Surefire provider & JUnit Jupiter Engine in plugin. By default checks *Test files. Change configuration if required for other name pattern check. Surefire reports are generated class basis.
5. Jacoco Reports generated for the Unit tests in test phase for report goal

### PowerMock Framework & its Need
* Mockito doesn’t allow mocking private, final & static method.[Good OOPs design]
* But there is a need time & again, so this framework can be used for such cases.
* Dependencies(Engine-powermock-api-mockito, API — powermock-module-junit4) 
Integration Testing with SpringBootTest Annotation
* Scans for SpringBootApplication annotation & loads entire application context from there.

### Integration Testing with SpringBootTest Annotation
* Scans for SpringBootApplication annotation & loads entire application context from there.

Will come up with a detailed separate post on Powermock Framework & Integration Testing. Stay tuned in. :smiley: :pray:
