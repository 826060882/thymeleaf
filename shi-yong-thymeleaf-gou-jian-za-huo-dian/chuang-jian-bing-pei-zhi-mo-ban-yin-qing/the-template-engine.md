Template Engine对象是org.thymeleaf.ITemplateEngine接口的实现。一种实现方式是通过org.thymeleaf.TemplateEngine，下面创建一个它的实例：

```java
templateEngine = new TemplateEngine();
templateEngine.setTemplateResolver(templateResolver);

```
很简单，不是吗，我们仅仅需要创建一个实例然后注入模板解析器。
对于 TemplateEngine来说templateResolver是必须的参数，虽然之后还有其他参数将被覆盖（消息解析器，缓存大小等）。到现在为止，我们需要的都有了。

模板引擎准备完毕，接下来我们可以用thymeleaf来创建页面了。

