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




















































