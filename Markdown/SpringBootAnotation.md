# **Learn SpringBoot Anotation**

- [**Learn SpringBoot Anotation**](#learn-springboot-anotation)
  - [**@SprinBootApplication**](#sprinbootapplication)
  - [**@CrossOrigin**](#crossorigin)
  - [**@RestController**](#restcontroller)
  - [**@Entity. @Table. @Id**](#entity-table-id)
  - [**@Column**](#column)
  - [**@RequestMapping**](#requestmapping)
      - [**GET请求**](#get请求)
      - [**POST请求**](#post请求)
      - [**PUT请求**](#put请求)
      - [**DELETE请求**](#delete请求)
  - [**@GeneratedValue**](#generatedvalue)
  - [**@Autowired**](#autowired)

## **@SprinBootApplication**
    @SpringBootApplication 将 @SpringBootConfiguration、@EnableAutoConfiguration 和 @ComponentScan 这三个注解组合在了一起.
    

- **@SpringBootConfiguration** 注解标注该类为 Spring Boot 应用的配置类，通常与 **@Configuration** 注解一起使用.

- **@EnableAutoConfiguration** 注解是 Spring Boot 的核心注解之一，它会根据项目中引入的依赖来自动配置 Spring 应用程序.它使用了 Spring Boot 提供的 spring-boot-autoconfigure 模块来实现自动化配置.

- **@ComponentScan** 注解会自动扫描并加载当前包及其子包下的所有 **@Component、@Service、@Controller、@Repository** 等注解标注的类.

因此，**@SpringBootApplication** 注解的作用是定义一个 Spring Boot 应用程序，并启用自动配置和组件扫描.通常情况下，我们将它放在应用程序的入口类上，即包含 main 方法的类上，这样 Spring Boot 就能够正确地加载并运行应用程序.

## **@CrossOrigin**
    @CrossOrigin 用于处理跨域请求。跨域请求指的是，浏览器发出的请求地址和当前页面所在的地址不一致，例如从 http://localhost:8080 发出的请求访问了 http://api.example.com 上的资源，这种情况下就涉及到跨域请求.

默认情况下，浏览器会限制跨域请求的访问，以保护用户的隐私和安全.但在某些情况下，我们可能需要开启跨域请求，例如前后端分离的项目中，前端页面和后端接口可能运行在不同的服务器上，这时候就需要使用 **@CrossOrigin** 注解来开启跨域请求.

**@CrossOrigin** 注解可以放在类级别或方法级别上，用于指定哪些跨域请求是被允许的.它的主要属性包括：

- **origins**：允许访问的来源域名，默认为 * 表示允许所有来源访问.

- **methods**：允许访问的 HTTP 方法，默认为 GET、POST 和 HEAD.

- **allowedHeaders**：允许访问的请求头，默认为 * 表示允许所有请求头.

- **exposedHeaders**：允许访问的响应头，默认为空.
maxAge：预检请求的有效期，单位为秒，默认为 1800 秒（30 分钟）.

- **allowCredentials**：是否允许发送 Cookie，布尔类型，默认为 false.

下面的代码展示了如何在 Spring Boot 中使用 **@CrossOrigin** 注解开启跨域请求：
```java
@!/RestController
@RequestMapping("/api")
@CrossOrigin(origins = "http://localhost:4200")
public class ApiController {
    // ...
}
```

## **@RestController**
    @RestController 的作用是将一个类标记为 RESTful Web 服务的控制器（Controller）.

在 Spring Web 应用中，控制器是负责处理客户端请求并返回响应的核心组件.传统的 Spring MVC 应用中，控制器通常使用 **@Controller** 注解来定义，并使用 **@RequestMapping** 注解来处理请求.

而在使用 **@RestController** 注解的控制器中，每个方法都被视为一个可以接收 HTTP 请求并返回 JSON 或 XML 格式数据的 Web 服务方法.这是因为 **@RestController** 注解相当于同时使用了 **@Controller** 和 **@ResponseBody** 注解.

**@ResponseBody** 注解表示将方法的返回值直接写入 HTTP 响应体中，而不是作为视图名称或模型数据来解析.因此，在使用 **@RestController** 注解的控制器中，每个方法的返回值将被直接写入 HTTP 响应体中，以 JSON 或 XML 格式返回给客户端.

 例如，下面的代码展示了如何在 Spring Boot 中使用 @RestController 注解创建一个简单的 RESTful Web 服务：

```java
@RestController
public class GreetingController {
    @GetMapping("/greeting")
    public String greeting() {
        return "Hello, world!";
    }
}
```

## **@Entity. @Table. @Id**
    在使用 JPA（Java Persistence API）进行对象-关系映射时，通常需要用到以下三个注解：@Entity、@Table、@Id.

- **@Entity**：将 Java 类标识为实体类，表示该类的对象在数据库中可以被持久化存储.JPA 会将该类的每个实例映射到数据库中的一个表，并将该类的每个字段映射到表中的一个列.
- **@Table**：用于定义实体类对应的数据库表的信息，包括表名、索引、唯一约束等.如果没有指定 @Table 注解，则默认表名与实体类名相同.
- **@Id**：用于标识实体类的主键字段.JPA 要求每个实体类都必须有一个主键字段，用于标识该实体在数据库表中的唯一记录.@Id 注解可以与 **@GeneratedValue** 注解一起使用，指定主键的生成策略.
- 
以下是一个示例，演示了如何使用这三个注解：
```java
@Entity
@Table(name = "employees")
public class Employee {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private Long id;
    
    @Column(name = "first_name")
    private String firstName;
    
    @Column(name = "last_name")
    private String lastName;
    
    @Column(name = "email")
    private String email;
}
```
 在上面的代码中，我们使用 **@Entity** 注解标记了 Employee 类，表示它是一个实体类.使用 **@Table** 注解指定了该类对应的数据库表名为 "employees".使用 @Id 和 **@GeneratedValue** 注解标记了 id 字段，表示它是主键，并指定了主键的生成策略为自增长.firstName、lastName 和 email 字段将映射到数据库表的三个列中，分别对应表中的 "first_name"、"last_name" 和 "email" 列.

 在使用 JPA 进行数据访问时，我们可以通过 **EntityManager**对象来操作实体类.例如，以下代码演示了如何使用 EntityManager 对象查询数据库中的所有 Employee 对象：
 ```java
 public class EmployeeService {
    
    @PersistenceContext
    private EntityManager entityManager;
    
    public List<Employee> findAll() {
        return entityManager.createQuery("SELECT e FROM Employee e", Employee.class)
            .getResultList();
    }
}
```
在上面的代码中，我们使用 **@PersistenceContext** 注解标记了 **entityManager** 字段，表示它是一个 JPA 实体管理器对象.通过 **createQuery** 方法和 **JPQL**（Java Persistence Query Language）语句，我们可以查询数据库中的 Employee 对象并**将结果返回为一个 List 对象**.

## **@Column**
    @Column 是 JPA 中的一个注解，用于定义实体类中属性和数据库表中列之间的映射关系.通常情况下，实体类的属性名和数据库表的列名是不一样的，使用 @Column 注解可以指定属性对应的列的名称、类型、长度、精度等信息.

以下是 **@Column** 注解的常用属性：

- **name**：指定属性对应的数据库表中的列名，默认为属性名.
- **nullable**：指定该列是否允许为 null，默认为 true.
- **unique**：指定该列是否需要唯一约束，默认为 false.
- **length**：指定该列的长度，适用于字符串类型.
- **precision**：指定该列的数值精度，适用于浮点数类型.
- **scale**：指定该列的小数点后的位数，适用于浮点数类型.
- **columnDefinition**：指定该列的数据库原生类型和限制条件，用于在生成 DDL 语句时使用.
- **insertable**：指定该列是否允许插入，默认为 true.
- **updatable**：指定该列是否允许更新，默认为 true.

以下是一个示例，演示了如何在实体类中使用 **@Column** 注解：
```java
@Entity
@Table(name = "employees")
public class Employee {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private Long id;
    
    @Column(name = "first_name")
    private String firstName;
    
    @Column(name = "last_name")
    private String lastName;
    
    @Column(name = "email")
    private String email;
}
```
 在上面的代码中，我们使用 **@Column** 注解标记了 id、firstName、lastName 和 email 字段，分别指定了它们对应数据库表中的列名.例如，firstName 字段对应数据库表中的 "first_name" 列，email 字段对应 "email" 列.其他属性的默认值均使用了 JPA 规范中的默认值.

 ## **@RequestMapping**
    Spring 框架中的 @RequestMapping 注解是处理 HTTP 请求的核心注解，它支持 GET、POST、PUT、DELETE 等多种请求方法.

但为了让开发者更加方便和直观地处理 HTTP 请求，Spring 还提供了一些衍生注解，例如：

- #### **@GetMapping**
    处理[GET](#get请求)请求的注解，是 @RequestMapping(method = RequestMethod.GET) 的缩写.
    ```java
    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, world!";
    }
    ```
- #### **@PostMapping**
    处理[POST](#post请求)请求的注解，是 @RequestMapping(method = RequestMethod.POST) 的缩写.
    ```java
    @PostMapping("/login")
    public String login(String username, String password) {
        // ...
    }
- #### **@PutMapping**
    处理[**PUT**](#put请求)请求的注解，是 @RequestMapping(method = RequestMethod.PUT) 的缩写.
    ```java
    @PutMapping("/user")
    public void updateUser(User user) {
        // ...
    }
    ```
- #### **@DeleteMapping**
    处理 DELETE 请求的注解，是 @RequestMapping(method = RequestMethod.DELETE) 的缩写.
    ```java
    @DeleteMapping("/user/{id}")
    public void deleteUser(@PathVariable Long id) {
        // ...
    }
    ```
 这些衍生注解都是基于 @RequestMapping 注解的基础上做出的简化和优化，可以让开发者更加方便地处理 HTTP 请求，提高开发效率和代码可读性.此外，这些注解也可以使用 produces 和 consumes 属性指定请求和响应的内容类型，从而更加精细地控制接口的输入输出.
 

#### **GET请求**

    在 Web 应用程序中，GET 请求是最常用的一种 HTTP 请求方法.它通常用于从服务器获取资源或数据.GET 请求会将请求参数附加在 URL 的末尾，以查询字符串的形式传递给服务器.
    例如，下面的 URL 中包含了两个查询参数 name 和 age，分别对应的值为 张三 和 18：

```html
http://example.com/api/user?name=张三&age=18
```

 在服务器端，我们可以通过获取 URL 中的查询参数来获取客户端传递的数据.
 
 在 Spring Boot 中，处理 GET 请求可以使用 @GetMapping 注解.@GetMapping 注解用于将一个方法映射到处理 HTTP GET 请求的路径上.

 下面是一个简单的 Spring Boot GET 请求的例子：
 ```java
 @RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public User getUserById(@PathVariable Long id) {
        // 根据 ID 获取用户信息
        return user;
    }

    @GetMapping
    public List<User> getAllUsers() {
        // 获取所有用户信息
        return users;
    }
}
```
在上面的例子中，@GetMapping("/{id}") 将方法 getUserById 映射到处理路径 /api/users/{id} 上，其中 {id} 是一个路径变量.方法接受一个参数 id，从路径变量中获取.

@GetMapping 将方法 getAllUsers 映射到处理路径 /api/users 上.该方法不接受任何参数.

要测试 GET 请求，可以使用 HTTP 客户端或者类似 cURL 的命令行工具.以下是使用 cURL 发送 GET 请求的示例：
```bash
curl http://localhost:8080/api/users/123
```
在上面的命令中，请求 URL 为 "http://localhost:8080/api/users/123"，其中 123 是路径变量.

以下是获取所有用户信息的 GET 请求示例：
```bash
curl http://localhost:8080/api/users
```
在上面的命令中，请求 URL 为 http://localhost:8080/api/users

---

#### **POST请求**
    POST 请求是 HTTP 请求方法之一，用于向服务器提交数据.与 GET 请求不同，POST 请求将请求参数包含在请求体中，而不是在 URL 中.POST 请求适用于向服务器提交表单数据、上传文件等场景.

在 Spring Boot 中，处理 POST 请求可以使用 @PostMapping 注解.@PostMapping 注解用于将一个方法映射到处理 HTTP POST 请求的路径上.

下面是一个简单的 Spring Boot POST 请求的例子：
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @PostMapping
    public User createUser(@RequestBody User user) {
        // 创建用户
        return newUser;
    }
}
```
在上面的例子中，@PostMapping 将方法 createUser 映射到处理路径 /api/users 上.方法接受一个参数 user，从请求体中获取.

要测试 POST 请求，可以使用 HTTP 客户端或者类似 cURL 的命令行工具.以下是使用 cURL 发送 POST 请求的示例：
```json
curl -X POST -H "Content-Type: application/json" -d '{"name":"John Doe","email":"johndoe@example.com"}' http://localhost:8080/api/users
```
在上面的命令中，-X POST 指定请求方法为 POST，-H "Content-Type: application/json" 指定请求体的类型为 JSON 格式，-d '{"name":"John Doe","email":"johndoe@example.com"}' 指定请求体内容.请求 URL 为 "http://localhost:8080/api/users"

---

####  **PUT请求**
    PUT 请求是 HTTP 请求方法之一，用于更新或替换服务器上的资源.PUT 请求与 POST 请求类似，都是向服务器提交数据，但是 PUT 请求通常用于更新已有资源，而 POST 请求则用于创建新资源.

在 Spring Boot 中，处理 PUT 请求可以使用 @PutMapping 注解.@PutMapping 注解用于将一个方法映射到处理 HTTP PUT 请求的路径上.

下面是一个简单的 Spring Boot PUT 请求的例子：
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @PutMapping("/{id}")
    public User updateUser(@PathVariable Long id,     @RequestBody User user) {
        // 更新用户信息
        return updatedUser;
    }
}
```
在上面的例子中，@PutMapping("/{id}") 将方法 updateUser 映射到处理路径 /api/users/{id} 上，其中 {id} 是一个路径变量.方法接受两个参数：一个是 id，从路径变量中获取；另一个是 user，从请求体中获取.

要测试 PUT 请求，可以使用 HTTP 客户端或者类似 cURL 的命令行工具.以下是使用 cURL 发送 PUT 请求的示例：
```json
curl -X PUT -H "Content-Type: application/json" -d '{"name":"John Doe","email":"johndoe@example.com"}' http://localhost:8080/api/users/123
```
在上面的命令中，-X PUT 指定请求方法为 PUT，-H "Content-Type: application/json" 指定请求体的类型为 JSON 格式，-d '{"name":"John Doe","email":"johndoe@example.com"}' 指定请求体内容.请求 URL 为 "http://localhost:8080/api/users/123"，其中 123 是路径变量

#### **DELETE请求**

    在 Spring Boot 中，处理 DELETE 请求可以使用 @DeleteMapping 注解.@DeleteMapping 注解用于将一个方法映射到处理 HTTP DELETE 请求的路径上.

下面是一个简单的 Spring Boot DELETE 请求的例子：
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @DeleteMapping("/{id}")
    public void deleteUserById(@PathVariable Long id) {
        // 根据 ID 删除用户信息
    }
}
```
在上面的例子中，@DeleteMapping("/{id}") 将方法 deleteUserById 映射到处理路径 /api/users/{id} 上，其中 {id} 是一个路径变量.方法接受一个参数 id，从路径变量中获取.

要测试 DELETE 请求，可以使用 HTTP 客户端或者类似 cURL 的命令行工具.以下是使用 cURL 发送 DELETE 请求的示例：
```bash
curl -X DELETE http://localhost:8080/api/users/123
```
在上面的命令中，请求 URL 为 "http://localhost:8080/api/users/123"，其中 123 是路径变量.

## **@GeneratedValue**
    @GeneratedValue 是 JPA 注解之一，用于指定实体类中自动生成的主键字段的生成策略。

    在 JPA 中，@GeneratedValue 注解通常与 @Id 注解一起使用，用于标识实体类的主键字段。
@GeneratedValue 注解有不同的生成策略选项，常用的包括：

   - **GenerationType.IDENTITY**：使用数据库自动生成的主键（例如自增主键）作为实体类的主键。适用于支持自动生成主键的数据库，如 MySQL 的自增列。
        ```java
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        ```
   - **GenerationType.SEQUENCE**：使用数据库序列（Sequence）生成主键。适用于支持序列的数据库，如 Oracle。
        ```java
        @Id
        @GeneratedValue(strategy = GenerationType.SEQUENCE)
        private Long id;
        ```
   - **GenerationType.TABLE**：使用一个特定的数据库表来生成主键。每次生成主键时，都会从该表中获取下一个可用的主键值。
        ```java
        @Id
        @GeneratedValue(strategy = GenerationType.TABLE)
        private Long id;
        ```
   - **GenerationType.AUTO**：根据底层数据库的支持情况自动选择适当的主键生成策略。这通常是默认的生成策略。 
        ```java
        @Id
        @GeneratedValue(strategy = GenerationType.AUTO)
        private Long id;
        ```
通过使用 @GeneratedValue 注解，可以告诉 JPA 在插入新实体时如何生成主键值。具体选择哪种生成策略取决于应用程序的需求和底层数据库的支持情况。

## **@Autowired**
    @Autowired 用于实现自动装配（Autowired）功能，即自动将依赖对象注入到目标对象中。

    在 Spring 中，通过 @Autowired 注解可以完成对类成员、构造函数和方法参数的自动装配。当 Spring 容器创建对象时，会检查被 @Autowired 注解标记的字段、构造函数或方法参数的类型，并尝试在容器中查找匹配的 Bean 对象进行注入。

    使用 @Autowired 注解可以避免手动编写大量的依赖注入代码，提高开发效率和代码可读性。

下面是 @Autowired 注解的几种常见用法：

- 字段自动装配：
    ```java
    @Autowired
    private SomeDependency dependency;
    ```
- 构造函数自动装配：
    ```java
    private SomeDependency dependency;

    @Autowired
    public MyClass(SomeDependency dependency) {
    this.dependency = dependency;
    }
    ```
- 方法参数自动装配：
    ```java
    @Autowired
    public void setDependency(SomeDependency dependency) {
    this.dependency = dependency;
    }
    ```
需要注意的是，**@Autowired** 注解默认使用按类型匹配的方式进行自动装配。如果存在多个匹配的 Bean 对象，可以使用 **@Qualifier** 注解指定具体的 Bean 名称进行注入。另外，可以通过 **@Autowired** 的 **required** 属性来指定依赖是否必需，默认为 true，表示必需。

除了 **@Autowired** 注解，Spring 还提供了其他注解来实现依赖注入，如 **@Inject**、**@Resource** 等。它们的功能类似，但是注解名称和一些细节有所不同，具体使用哪个注解取决于项目的具体配置和需求。