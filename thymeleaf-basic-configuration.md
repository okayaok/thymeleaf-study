## Thymeleaf3+Spring4基本配置

使用[Thymeleaf3+Spring4](http://www.thymeleaf.org/doc/articles/thymeleaf3migration.html)需要引入相应的jar包：thymeleaf:3.0.0和thymeleafspring4:3.0.0; 下面介绍两个常用的配置方法，maven和gradle。

1.maven配置：

* Thymeleaf3配置：
  ```
  <dependency> 
  <groupId>org.thymeleaf</groupId>
  <artifactId>thymeleaf</artifactId>
  <version>3.0.0.RELEASE</version>
  </dependency> 
  ```


* thyeleaf-spring4配置：
  ```
  <dependency> 
      <groupId>org.thymeleaf</groupId>     
      <artifactId>thymeleaf-spring4</artifactId> 
      <version>3.0.0.RELEASE</version>
  </dependency> 
  ```


2.gradle配置：

* thymeleaf3配置：
  > compile group: 'org.thymeleaf', name: 'thymeleaf', ve rsion: '3.0.0.RELEASE'


* thymeleaf-spring4配置：
  > compile group: 'org.thymeleaf', name: 'thymeleaf-spri ng4', version: '3.0.0.RELEASE'


## Spring的配置

### Spring的xml配置

1.配置viewResolver：

```
<bean id="viewResolver" class="org.thymeleaf.spring4. view.ThymeleafViewResolver"> 
 <property name="templateEngine" ref="templateEngine " /> 
 <property name="characterEncoding" value="UTF-8" />; 
</bean>
```

2.配置templateEngine：

```
<bean id="templateEngine" class="org.thymeleaf.sprin g4.SpringTemplateEngine"> 
    <property name="templateResolver" ref="templateRes olver" /> 
    <property name="enableSpringELCompile" value="true " />
</bean>
```

3.配置templateResolver:：

```
<bean id="templateResolver" class="org.thymeleaf.sp ring4.templateresolver.SpringResourceTemplateResolver "> 
    <property name="prefix" value="/WEB-INF/templates/" />
    <property name="suffix" valeu="" />
    <property name="templateMode" value="HTML" />
    <property name="characterEncoding" value="UTF-8" />
</bean> 
```

### Spring的java配置：

```java
@Configuration
@EnableWebMvc
@ComponentScan("com.thymeleafexamples")    
public class ThymeleafConfig extends WebMvcConfigurerAdap ter implements ApplicationContextAware {

    private ApplicationContext applicationContext;

    public void setApplicationContext(ApplicationContext appl icationContext) { 
        this.applicationContext = applicationContext; 
    }

    /**
      * 配置ViewResolver 
      */ 
    @Bean 
    public ViewResolver viewResolver() { 
        ThymeleafViewResolver resolver = new ThymeleafViewResol ver(); 
        resolver.setTemplateEngine(templateEngine()); 
        resolver.setCharacterEncoding("UTF-8"); return resolver; 
    }

    /** 
    * 配置TemplateEngine 
    */ 
    @Bean 
    public TemplateEngine templateEngine() { 
        SpringTemplateEngine engine = new SpringTemplateEngine( ); 
        engine.setEnableSpringELCompiler(true); 
        engine.setTemplateResolver(templateResolver()); 
        return engine; 
    }

    /** 
    * 配置TemplateResolver 
    */ 
    private ITemplateResolver templateResolver() { 
        SpringResourceTemplateResolver resolver = new SpringResourceTemplateResolver();      
        resolver.setApplicationContext(applicationContext); resolver.setPrefix("/WEB-INF/templates/");          
        resolver.setSuffix(""); 
        resolver.setTemplateMode(TemplateMode.HTML); 
        return resolver; 
    }
} 
```

### Thymeleaf Layout Dialect标签配置

1.引入相关的jar包:nz.net.ultraq.thymeleaf:thymeleaf-layout-dialect:2.0.3

* maven引入jar包配置：

```
<dependency>  
    <groupId>nz.net.ultraq.thymeleaf</groupId>
    <artifactId>thymeleaf-layout-dialect</artifactId>   
    <version>2.0.3</version>
</dependency> 
```

* gradle引入jar包的配置：

> compile group: 'nz.net.ultraq.thymeleaf', name: 'thym eleaf-layout-dialect', version: '2.0.3'

2.在SpringTemplateEngine中配置layout标签的解析

* xml文件配置代码：
  ```
  <bean id="templateEngine" class="org.thymeleaf.spring4.SpringTemplateEngine"> 
      <property name="templateResolver" ref="templateRe solver" /> 
      <property name="additionalDialects"> 
          <set> 
              <bean class="nz.net.ultraq.thymeleaf.Layo utDialect"/> 
          </set> 
      </property>  
  </bean> 
  ```


* java代码配置：

  ```java
  /** 
    * 配置TemplateEngine 
    */ 
  @Bean 
  public TemplateEngine templateEngine() { 
      SpringTemplateEngine engine = new SpringTemplateEng ine();
      engine.setEnableSpringELCompiler(true);
      engine.addDialect(new LayoutDialect());
      engine.setTemplateResolver(templateResolver());
      return engine;
  }
  ```


3.在HTML界面关于layout标签的使用

* 比如我创建了一个带有layout标签的主样式界面，引入layout标签的代 码如下所示：
  > xmlns:layout="http:\/\/www.ultraq.net.nz\/thymeleaf\/layout"


* 使用layout标签的代码如下所示：

  ```
  <div id="content">
      <section layout:fragment="content"></section>
  </div> 
  ```

  我在子界面引入该主界面的样式，同样需要在子界面引入layout标 签，并同时使用layout:decorate="主界面的路径"引入主界面。代码如 下：

  > &lt;html xmlns:layout="http:\/\/www.ultraq.net.nz\/thymele af\/layout" layout:decorate="admin\/layouts\/main.html"&gt;

  将子界面的内容加到主样式界面的 中，代码如下：

  > &lt;section layout:fragment="content"&gt; &lt;子界面的内容&gt; &lt;\/section&gt;


4.使用Thymeleaf Spring Data Dialect标签实行分页

* maven方式引入jar包：

  ```
  <dependency> 
      &lt;groupId&gt;io.github.jpenren&lt;\/groupId&gt;
      &lt;artifactId&gt;thymeleaf-spring-data-dialect&lt;\/artifact Id&gt;
      &lt;version&gt;3.1.0&lt;\/version&gt; 
  </dependency> 
  ```


* gradle方式引入jar包：

  > compile group: 'io.github.jpenren', name: 'thymeleafspring-data-dialect', version: '3.1.0'


5.添加Spring Data Dialect到已经配置好的Thymeleaf Template Engine中

* xml配置代码：

```
<bean id="templateEngine" class="org.thymeleaf.spring4.SpringTemplateEngine">     
    &lt;property name="templateResolver" ref="templateRe solver" \/&gt;
    &lt;property name="additionalDialects"&gt;
        &lt;set&gt;
            &lt;bean class="org.thymeleaf.dialect.spring data.SpringDataDialect"\/&gt;
        &lt;\/set&gt; 
    &lt;\/property&gt; 
</bean> 
```

* java代码配置：

```java
/**
  * 配置templateEngine
  */
@Bean 
public TemplateEngine templateEngine() { 
    SpringTemplateEngine engine = new SpringTemplateEng ine(); 
    engine.setEnableSpringELCompiler(true); 
    engine.addDialect(new SpringDataDialect()); 
    engine.setTemplateResolver(templateResolver()); 
    return engine;
}
```

6.页面引用

* 引入分页的sd标签，代码如下：

> &lt;html xmlns:sd="http:\/\/www.thymeleaf.org\/spring-data" &gt;&lt;\/html&gt;

* 引入分页信息，代码如下：

```
<div class="row">  
    &lt;div class="col-sm-6"&gt;
    &lt;div sd:pagination-summary=""&gt;info&lt;\/div&gt; &lt;\/div&gt; &lt;div class="col-sm-6"&gt; &lt;nav class="pull-right"&gt; &lt;ul class="pagination" sd:pagination="full"&gt; &lt;!-- Pagination created by SpringDataDialec t, this content is just for mockup --&gt; &lt;li class="disabled"&gt;&lt;a href="\#" aria-label ="Previous"&gt;&lt;span aria-hidden="true"&gt;«&lt;\/span&gt;&lt;\/ a&gt;&lt;\/li&gt; &lt;li class="active"&gt;&lt;a href="\#"&gt;1 &lt;span clas s="sr-only"&gt;\(current\)&lt;\/span&gt;&lt;\/a&gt;&lt;\/li&gt; &lt;\/ul&gt; &lt;\/nav&gt; &lt;\/div&gt; &lt;\/div&gt;
```

**注意：当使用Thymeleaf Spring data dialect时，Page和Pageable必须使用 org.springframework.data.domain.Page和 org.springframework.data.domain.Pageable!**

