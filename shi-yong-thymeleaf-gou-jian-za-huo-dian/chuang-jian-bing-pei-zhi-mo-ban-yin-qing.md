过滤器中的process()方法包含以下代码：

```
ITemplateEngine templateEngine = this.application.getTemplateEngine();
```
这意味着GTVGApplication类负责创建和配置Thymeleaf应用程序中最重要的对象之一：TemplateEngine实例（ITemplateEngine接口的实现）。

org.thymeleaf.TemplateEngine 对象初始化：


```java
public class GTVGApplication {
  
    
    ...
    private static TemplateEngine templateEngine;
    ...
    
    
    public GTVGApplication(final ServletContext servletContext) {

        super();

        ServletContextTemplateResolver templateResolver = 
                new ServletContextTemplateResolver(servletContext);
        
        // HTML is the default mode, but we set it anyway for better understanding of code
        templateResolver.setTemplateMode(TemplateMode.HTML);
        // This will convert "home" to "/WEB-INF/templates/home.html"
        templateResolver.setPrefix("/WEB-INF/templates/");
        templateResolver.setSuffix(".html");
        // Template cache TTL=1h. If not set, entries would be cached until expelled by LRU
        templateResolver.setCacheTTLMs(Long.valueOf(3600000L));
        
        // Cache is set to true by default. Set to false if you want templates to
        // be automatically updated when modified.
        templateResolver.setCacheable(true);
        
        this.templateEngine = new TemplateEngine();
        this.templateEngine.setTemplateResolver(templateResolver);
        
        ...

    }

}

```
有很多方式去配置TemplateEngine，上面几行代码已经可以满足我们的需要。



