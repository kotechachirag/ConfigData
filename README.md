# Swagger for spring data rest(mongodb)

Create spring data rest application with either mongo/Jpa

To configure swagger follow below steps

> Notes(Swagger for spring data rest is introduce from version **2.6.1**)

### Add swagger related depedency

```
		<dependency>
		    <groupId>io.springfox</groupId>
		    <artifactId>springfox-swagger-ui</artifactId>
		   <version>2.6.1</version>
		</dependency>
		<dependency>
		    <groupId>io.springfox</groupId>
		    <artifactId>springfox-swagger2</artifactId>
		    <version>2.6.1</version>
		</dependency>
		<dependency>
		    <groupId>io.springfox</groupId>
		    <artifactId>springfox-data-rest</artifactId>
		    <version>2.6.1</version>
		</dependency>
```

### Add Repository
```
<repositories>
    <repository>
      <id>jcenter-snapshots</id>
      <name>jcenter</name>
      <url>https://jcenter.bintray.com/</url>
    </repository>
</repositories>
```

### Add annotation to enable swagger2 in application.class
```
@SpringBootApplication
@Configuration
@EnableSwagger2
```

### import SpringDataRestConfiguration.class
```
@Import({springfox.documentation.spring.data.rest.configuration.SpringDataRestConfiguration.class})
```
### Create Docket(Example main class)
```
@SpringBootApplication
@Configuration
@EnableSwagger2
@Import({springfox.documentation.spring.data.rest.configuration.SpringDataRestConfiguration.class})
@EnableMongoRepositories(basePackages = "com.demoswagger")
@ComponentScan(basePackages="com.demoswagger")
public class Application {

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
	
	@Bean
    public Docket newsApi() {
        return new Docket(DocumentationType.SWAGGER_2)
          
                .groupName("swagger-demo")
                .apiInfo(apiInfo())
                .select()
                .paths(PathSelectors.regex("/*.*product*.*"))
                .build();
    }
 
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Swagger-demo Title")
                .description("Swagger-demo Description")
                .build();
    }
}
```
**
- .paths(PathSelectors.regex("/*.*product*.*"))
- It means it will check the path and register only which contains "product"
- If we want to list all repository we can give like 
- .paths(PathSelectors.any())
**


### sample repository
```
@RepositoryRestResource(collectionResourceRel = "product", path = "product")
public interface ProductRepository extends MongoRepository<Product, String> {
	@RequestMapping(value="/{name}", method = RequestMethod.GET)
	@ApiOperation(value = "findbyname and title", notes = "Basic check with new mehod")
	Product findByName(@RequestParam("name") @Param("name") String name);
}
```

Run application you can verify swaggger configuratioon using url http://localhost:8080/swagger-ui.html
