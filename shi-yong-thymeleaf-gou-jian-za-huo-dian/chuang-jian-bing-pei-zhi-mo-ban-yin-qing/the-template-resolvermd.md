让我们从模板解析器开始：


```java
ServletContextTemplateResolver templateResolver = 
        new ServletContextTemplateResolver(servletContext);
```


模板解析器是实现Thymeleaf API接口（org.thymeleaf.templateresolver.ITemplateResolver）的对象。


```java
public interface ITemplateResolver {

    ...
  
    /*
     * Templates are resolved by their name (or content) and also (optionally) their 
     * owner template in case we are trying to resolve a fragment for another template.
     * Will return null if template cannot be handled by this template resolver.
     */
    public TemplateResolution resolveTemplate(
            final IEngineConfiguration configuration,
            final String ownerTemplate, final String template,
            final Map<String, Object> templateResolutionAttributes);
}

```
这些对象负责确定如何访问页面模板，在我们的应用中，使用ServletContextTemplateResolver意味着要从servletContext中获取模板文件作为资源(在每个Java web应用中都存在servletContext对象，通常情况下从web应用程序的根目录获取资源)。

但是对于模板解析器来说，我们还需要配置一些参数。
第一，配置模板模式，这里为html
`templateResolver.setTemplateMode(TemplateMode.HTML);`

对ServletContextTemplateResolver来说，html是默认的解析模板，但是我们还是要去配置它，以便代码知道发生了什么。


```
templateResolver.setPrefix("/WEB-INF/templates/");
templateResolver.setSuffix(".html");
```
前缀prefix和后缀suffix会修改我们传递给引擎的模板名称，以获取要使用的真实资源名称。例如：我们访问"product/list"就相当于去查找：

```
servletContext.getResourceAsStream("/WEB-INF/templates/product/list.html")

```
配置可选项，解析模板可以缓存多久可以通过cacheTTLMs进行设置：

```
templateResolver.setCacheTTLMs(3600000L);

```
如果达到了最大缓存量，最先放入缓存的文件将会被优先清除。

> 用户可以通过实现ICacheManager接口来定义缓存的特性和大小，也可以通过修改StandardCacheManager对象来管理默认缓存。

关于模板解析还有很多要学习的地方，但接下来让我们看看如何创建模板引擎对象。






