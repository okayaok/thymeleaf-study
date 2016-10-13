# Thymeleaf3+Spring4基本配置

1. 使用[Thymeleaf3+Spring4](http://www.thymeleaf.org/doc/articles/thymeleaf3migration.html)需要引入相应的jar包：thymeleaf:3.0.0和thymeleafspring4:3.0.0; 下面介绍两个常用的配置方法，maven和gradle。
  1. maven配置：

  * Thymeleaf3配置：

    > &lt;dependency&gt;
    > 
    > &lt;groupId&gt;org.thymeleaf&lt;\/groupId&gt;
    > 
    > &lt;artifactId&gt;thymeleaf&lt;\/artifactId&gt;
    > 
    > &lt;version&gt;3.0.0.RELEASE&lt;\/version&gt;
    > 
    > &lt;\/dependency&gt;

  * thyeleaf-spring4配置：

    > &lt;dependency&gt;
    > 
    > &lt;groupId&gt;org.thymeleaf&lt;\/groupId&gt;
    > 
    > &lt;artifactId&gt;thymeleaf-spring4&lt;\/artifactId&gt;
    > 
    > &lt;version&gt;3.0.0.RELEASE&lt;\/version&gt;
    > 
    > &lt;\/dependency&gt;


   2. gradle配置：

  * thymeleaf3配置：

    > compile group: 'org.thymeleaf', name: 'thymeleaf', ve rsion: '3.0.0.RELEASE'

  * thymeleaf-spring4配置：

    > compile group: 'org.thymeleaf', name: 'thymeleaf-spri ng4', version: '3.0.0.RELEASE'





 2. Spring的配置

1. Spring的xml配置

  * 配置viewResolver：
    > &lt;bean id="viewResolver" class="org.thymeleaf.spring4. view.ThymeleafViewResolver"&gt; 
    > 
    >     &lt;property name="templateEngine" ref="templateEngine " \/&gt; 
    > 
    >     &lt;property name="characterEncoding" value="UTF-8" \/&gt; 
    > 
    > &lt;\/bean&gt;


  * 配置templateEngine：
    > &lt;bean id="templateEngine" class="org.thymeleaf.sprin g4.SpringTemplateEngine"&gt; 
    > 
    >     &lt;property name="templateResolver" ref="templateRes olver" \/&gt; 
    > 
    >     &lt;property name="enableSpringELCompile" value="true " \/&gt;
    > 
    > &lt;\/bean&gt;

     
  * 配置templateResolver:： 
    > &lt;bean id="templateResolver" class="org.thymeleaf.sp ring4.templateresolver.SpringResourceTemplateResolver "&gt; 
    > 
    > &lt;property name="prefix" value="\/WEB-INF\/templates \/" \/&gt; 
    > 
    > &lt;property name="suffix" valeu="" \/&gt; &lt;property name="templateMode" value="HTML" \/&gt; 
    > 
    > &lt;property name="characterEncoding" value="UTF-8" \/&gt; 
    > 
    > &lt;\/bean&gt;





2. Spring的java配置代码如下：


> @Configuration@EnableWebMvc@ComponentScan\("com.thymeleafexamples"\) 
> 
>        public class ThymeleafConfig extends WebMvcConfigurerAdap ter implements ApplicationContextAware {
> 
>           private ApplicationContext applicationContext;
> 
>           public void setApplicationContext\(ApplicationContext appl icationContext\) { 
> 
>               this.applicationContext = applicationContext; 
> 
>           }
> 
>           \/\*\* \* 配置ViewResolver \*\/ 
> 
>          @Bean 
> 
>          public ViewResolver viewResolver\(\) { 
> 
>                ThymeleafViewResolver resolver = new ThymeleafViewResol ver\(\); 
> 
>                resolver.setTemplateEngine\(templateEngine\(\)\); 
> 
>                resolver.setCharacterEncoding\("UTF-8"\); return resolver; 
> 
>          }
> 
>           \/\*\* \* 配置TemplateEngine \*\/ 
> 
>          @Bean 
> 
>          public TemplateEngine templateEngine\(\) { 
> 
>              SpringTemplateEngine engine = new SpringTemplateEngine\( \); 
> 
>              engine.setEnableSpringELCompiler\(true\); 
> 
>              engine.setTemplateResolver\(templateResolver\(\)\); 
> 
>              return engine; 
> 
>          }
> 
>          \/\*\* \* 配置TemplateResolver \*\/ 
> 
>          private ITemplateResolver templateResolver\(\) { 
> 
>             SpringResourceTemplateResolver resolver = new SpringResourceTemplateResolver\(\);      
> 
>             resolver.setApplicationContext\(applicationContext\); resolver.setPrefix\("\/WEB-INF\/templates\/"\);          
> 
>             resolver.setSuffix\(""\); 
> 
>             resolver.setTemplateMode\(TemplateMode.HTML\); return resolver; 
> 
>          } 
> 
> }






