## Autowired和Resource关键字的区别？
### @Autowired
@Autowired是Spring提供的用于bean对象注入使用的注解，默认使用byType的方式注入bean（byType是指根据类Class匹配bean），当同一个类有多个实现时，则通过byName(根据bean的id属性进行匹配)确定唯一一个指定的bean
1. 当使用Autowired注入的bean存在多个实现时，会报错，解决方案有两种
- 改变变量名：变量名对应实现类的类名（开头小写）
- 辅助使用@Qualifier注解指定bean

### @Resource
@Resource是Java EE提供的bean注入注解 <br>
默认根据byName注入，匹配失败时则使用byType进行注入；在没有指定name属性的情况下，@Resource要求被注入的Bean必须在容器中有唯一的引用，否则会抛出异常。
### 区别
- @Autowired是Spring特有的注解，而@Resource是Java EE标准的注解。
- @Autowired通过类型进行匹配，如果类型匹配到多个Bean，则根据属性名称进行匹配。
- @Resource默认按照属性名称进行匹配，如果找不到匹配的Bean，则尝试按照属性类型进行匹配。
  
<br>

## @Resource的装配顺序
- 既没指定name属性，也没指定type属性：默认通过byName方式注入，如果byName匹配失败，则使用byType方式注入
- 指定name属性：通过byName方式注入，把变量名和IOC容器中的id去匹配，匹配失败则报错
- 指定type属性：通过byType方式注入，在IOC容器中匹配对应的类型，如果匹配不到或者匹配到多个则报错
- 同时指定name属性和type属性：在IOC容器中匹配，名字和类型同时匹配则成功，否则失败

<br>


## Spring注入的方式有哪些？分别有什么优缺点？
- 构造函数注入（Constructor Injection）：通过构造函数将依赖项传递给目标对象。
  - 优点：
明确地指定了依赖项，使得代码易于理解和维护。
确保目标对象在实例化时具有所有必需的依赖项，避免了对象创建后依赖项不完整的情况。
  - 缺点：
当依赖项较多时，构造函数参数列表可能会变得冗长。
在使用构造函数注入时，如果需要更改依赖项，需要修改目标对象的构造函数及其所有调用的地方。
- Setter 方法注入（Setter Injection）：通过 setter 方法设置依赖项。
  - 优点：
避免了冗长的构造函数参数列表，代码可读性更好。
可以灵活地更改依赖项，只需修改相应的 setter 方法即可。
  - 缺点：
目标对象可能在未设置依赖项的情况下被实例化，可能导致空指针异常。
依赖项可以在任何时候被修改，这可能导致对象的状态不稳定。
- 字段注入（Field Injection）：通过直接注入依赖项的字段来完成依赖注入。
  - 优点：
简化了代码，不需要编写复杂的构造函数或 setter 方法。
可以通过使用依赖注入框架来自动完成注入过程。
  - 缺点：
降低了目标对象的可测试性，因为无法在构造函数或 setter 方法中传递模拟的依赖项。
对象的依赖关系在代码中不明确可见，可能增加代码的理解难度。

<br>

## Spring常见的配置方式有哪些？
- 基于XML配置
- 基于注解配置
- 基于Java API配置

<br>

## 说说你对SpringMVC的理解
### MCV的分别解释
- M：Model，模型，表示程序数据和业务逻辑，负责对数据的封装、数据业务逻辑处理以及与数据库文件等数据源进行交互
- V：View，视图，定义用户使用界面的外观和布局，将数据可视化展示给用户
- C：Controller，控制器
  - 接收用户的输入并作出响应。
  - 处理用户请求，并根据请求调度适当的模型和视图。
  - 处理用户的交互逻辑，更新模型的状态，并决定显示哪个视图。

<br>

## SpringMVC的基本工作流程
1. 用户发送请求至前端控制器DispatcherServlet。
2. DispatcherServlet收到请求调用HandlerMapping处理器映射器。
3. 处理器映射器找到具体的处理器(可以根据xml配置.注解进行查找)，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。
4. DispatcherServlet调用HandlerAdapter处理器适配器。
5. HandlerAdapter经过适配调用具体的处理器(Controller，也叫后端控制器)。
6. Controller执行完成返回ModelAndView。
7. HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet。
8. DispatcherServlet将ModelAndView传给ViewReslover视图解析器。
9. ViewReslover解析后返回具体View。
10. DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）。
11. DispatcherServlet响应用户
> 
> DispatcherServlet：作为前端控制器，整个流程控制的中心，控制其它组件执行，统一调度，降低组件之间的耦合性，提高每个组件的扩展性<br>
> HandlerMapping：通过扩展处理器映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。<br>
> HandlAdapter：通过扩展处理器适配器，支持更多类型的处理器。<br>
> ViewResolver：通过扩展视图解析器，支持更多类型的视图解析，例如：jsp、freemarker、pdf、excel等。 <br>

<br>

## SpringMVC的常用注解
@RequestMapping、@RequestBody、@ResponseBody

<br>

## 什么是AOP？
AOP（面向切面编程）是一种软件开发的编程范式，旨在解决应用程序中横切关注点的模块化问题。<br>
在 AOP 中，应用程序的功能被划分为核心关注点和横切关注点。核心关注点表示应用程序的主要业务逻辑，而横切关注点是与核心关注点交叉的功能，比如日志记录、异常处理等。通过 AOP，我们可以将这些横切关注点从核心代码中分离出来，形成可重用的切面，并将其应用到需要的地方，避免了代码的重复和耦合。

<br>

## Spring中AOP的代理实现有哪些？
- 静态代理（AspectJ）:在编译阶段生成AOP代理类，因此也称为编译时增强，他会在编译阶段将AspectJ(切面)织入到Java字节码中，运行的时候就是增强之后的AOP对象。
``` Java
// 定义目标接口
public interface UserService {
    void saveUser();
}

// 目标类实现目标接口
public class UserServiceImpl implements UserService {
    @Override
    public void saveUser() {
        System.out.println("保存用户信息");
    }
}

// 定义代理类，实现目标接口
public class UserServiceProxy implements UserService {
    private final UserService target; // 持有目标对象的引用

    public UserServiceProxy(UserService target) {
        this.target = target;
    }

    @Override
    public void saveUser() {
        System.out.println("开始事务");
        target.saveUser(); // 调用目标对象的方法
        System.out.println("提交事务");
    }
}

// 使用静态代理
public class Main {
    public static void main(String[] args) {
        UserService target = new UserServiceImpl(); // 创建目标对象
        UserService proxy = new UserServiceProxy(target); // 创建代理对象
        proxy.saveUser(); // 通过代理对象调用方法
    }
}


```
- 动态代理（SpringAOP）:不会去修改字节码，而是每次运行时在内存中临时为方法生成一个AOP对象，这个AOP对象包含了目标对象的全部方法，并且在特定的切点做了增强处理，并回调原对象的方法。
  - JDK动态代理：JDK 代理基于接口来生成代理类。当使用 JDK 代理时，需要定义一个接口，并创建一个实现该接口的代理类。代理类实现了目标接口，并持有一个 InvocationHandler 对象。当调用代理对象的方法时，实际上会被转发到 InvocationHandler 的 invoke 方法。在 invoke 方法中，可以通过反射机制调用真实对象的对应方法，并在必要时添加额外的逻辑。**因此，JDK 代理只能代理实现了接口的类**。
  - CGLIB动态代理：CGLIB（Code Generation Library）代理是通过继承实现的动态代理方式。它可以代理没有实现接口的类。CGLIB 通过生成目标类的子类来实现代理，子类继承了目标类的所有方法，并重写其中需要增强的方法。


<br>

## 什么是切点？什么是通知？有哪些类型的通知？
### 切点（Point）
程序运行中的一些时间点, 例如一个方法的执行, 或者是一个异常的处理.在 Spring AOP 中, join point 总是方法的执行点。  
### 通知（Advice）
特定 JoinPoint 处的 Aspect 所采取的动作称为 Advice。
- before：前置通知，在一个方法执行前被调用。
- after: 在方法执行之后调用的通知，无论方法执行是否成功。
- after-returning: 仅当方法成功完成后执行的通知。
- after-throwing: 在方法抛出异常退出时执行的通知。
- around: 在方法执行之前和之后调用的通知。

<br>

## 如何理解Spring IOC
Spring IOC（Inversion of Control，控制反转）是Spring框架的核心概念之一<br>
主要核心概念有以下几点：
- 控制反转（Inversion of Control）：传统的程序设计中，组件之间的依赖关系由开发人员手动创建和管理。而在IOC中，控制权被反转，即不再由开发人员显式地创建和管理对象的依赖关系，而是由容器负责管理对象的创建、组装和生命周期。

- 容器（Container）：Spring IOC容器是一个负责管理应用程序中对象的创建和依赖关系的容器。它会读取配置信息，实例化对象，并将对象之间的依赖关系进行注入。

- Bean（Bean）：在Spring中，被容器所管理的对象通常被称为"Bean"。Bean是通过配置文件或注解定义的，它们可以是任何Java对象，包括普通的POJO（Plain Old Java Object）、服务类、数据访问对象等。

- 依赖注入（Dependency Injection）：依赖注入是IOC的重要特征，它指的是容器自动将对象所依赖的其他对象注入到它们当中，使得对象之间的依赖关系得以解耦。通过依赖注入，对象不需要自行创建或查找它们所依赖的其他对象，而是由容器负责协调和管理依赖关系。

- Spring的IOC有三种注入方式 ：构造器注入、setter方法注入、根据注解注入。

<br>

## 说一说SpringIOC中 Bean的生命周期
1. 实例化：Spring 容器根据配置信息或注解创建 Bean 的实例。这可以基于构造函数实例化或工厂方法实例化。

2. 属性设置：通过依赖注入（Dependency Injection），Spring 将配置的属性值或引用注入到 Bean 中。

3. 初始化方法调用：如果 Bean 实现了 InitializingBean 接口，或者在配置文件中通过 init-method 属性指定了初始化方法，那么在依赖注入完成后，Spring 会调用该方法进行初始化操作。

4. 使用中：Bean 可以正常使用，它可以被其他 Bean 引用和调用。

5. 销毁方法调用：如果 Bean 实现了 DisposableBean 接口，或者在配置文件中通过 destroy-method 属性指定了销毁方法，那么在容器关闭时，Spring 会调用该方法进行清理操作。

<br>

## Spring中支持哪几种Bean的作用域
- singleton：默认，每个容器中只有一个bean的实例，单例的模式由BeanFactory自身来维护。
- prototype：为每一个bean请求提供一个实例。
- request：为每一个网络请求创建一个实例，在请求完成以后，bean会失效并被垃圾回收器回收。
- session：与request范围类似，确保每个session中有一个bean的实例，在session过期后，bean会随之失效。
- global-session：全局作用域

<br>


## BeanFactory和 ApplicationContexts的区别
- 初始化时机：BeanFactory是Spring的最基本容器，它负责管理应用程序中的Bean对象。它使用延迟初始化策略，即在首次访问Bean时才进行实例化。相比之下，ApplicationContext在容器启动时就实例化所有的单例Bean对象，并在启动过程中进行依赖注入和初始化。

- 功能扩展：ApplicationContext是BeanFactory的扩展，提供了更多的功能和特性。例如，ApplicationContext支持国际化、事件发布、资源加载、AOP（面向切面编程）等功能，而BeanFactory只提供了基本的Bean管理功能。

- 自动装配：ApplicationContext支持自动装配（autowiring），可以根据依赖关系自动将Bean注入到其他Bean中，而BeanFactory需要手动配置依赖关系。

- 资源加载：ApplicationContext能够加载各种外部资源，如属性文件、数据库连接等。它也提供了更灵活的资源获取方式，例如可以通过类路径、URL等方式获取资源。BeanFactory则相对简单，主要用于Bean管理和创建。

- 性能：由于ApplicationContext在启动时进行了预加载和初始化，因此在运行时通常比BeanFactory稍微重量级一些。但是，这种额外的开销可以换来更高的开发效率和更好的功能扩展性。


<br>

## 什么是事务？
回答：事务是一组相关的数据库操作，它们被当作一个逻辑单元进行执行。事务要么全部成功执行，要么全部回滚，以保持数据的一致性。

## Spring事务的特点是什么？
回答：Spring事务具有以下特点：
- 原子性（Atomicity）：事务中的所有操作要么全部成功，要么全部失败。
- 一致性（Consistency）：事务结束后，数据库从一个一致性状态转移到另一个一致性状态。
- 隔离性（Isolation）：一个事务的执行不应该被其他事务干扰。
- 持久性（Durability）：一旦事务提交，其结果应该是永久性的。

## Spring中如何声明式地管理事务？
回答：可以使用Spring的声明式事务管理来管理事务。通过在方法上添加@Transactional注解或者在XML配置文件中配置事务的传播行为和回滚规则，Spring将自动管理这些方法的事务。

## 什么是事务传播行为（Transaction Propagation）？
回答：事务传播行为定义了一个方法执行时如何参与到现有的事务中。Spring提供了多种事务传播行为选项，如REQUIRED、SUPPORTS、REQUIRES_NEW等。

## 什么是事务隔离级别（Transaction Isolation Level）？
回答：事务隔离级别定义了一个事务与其他事务的可见性和影响度。常见的事务隔离级别包括READ_UNCOMMITTED、READ_COMMITTED、REPEATABLE_READ和SERIALIZABLE。


## Spring中的事务回滚是如何实现的？
回答：当一个方法被标记为@Transactional并且抛出了RuntimeException或Error时，Spring会自动回滚事务。此外，可以通过编程方式调用TransactionStatus的setRollbackOnly()方法来手动回滚事务。

























































