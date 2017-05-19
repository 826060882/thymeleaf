为了更好地解释使用Thymeleaf处理模板所涉及的概念，本教程将使用一个demo，您可以从文章开头链接处下载本demo。

本应用程序是我们虚构的一个杂货店，它将为我们提供场景去验证thymeleaf的功能。

简单介绍一下应用程序中涉及到的几个实体：Products Customers Orders Comments 图示如下：
![](http://www.thymeleaf.org/doc/tutorials/3.0/images/usingthymeleaf/gtvg-model.png)

我们的应用有简单的service层，service层由包含方法的service对象组成，例如：
```java
public class ProductService {

    ...

    public List<Product> findAll() {
        return ProductRepository.getInstance().findAll();
    }

    public Product findById(Integer id) {
        return ProductRepository.getInstance().findById(id);
    }
    
}

```
在web层，有一个过滤器会根据请求的url委托执行启用thymeleaf的命令。


```java
private boolean process(HttpServletRequest request, HttpServletResponse response)
        throws ServletException {
    
    try {

        // This prevents triggering engine executions for resource URLs
        if (request.getRequestURI().startsWith("/css") ||
                request.getRequestURI().startsWith("/images") ||
                request.getRequestURI().startsWith("/favicon")) {
            return false;
        }

        
        /*
         * Query controller/URL mapping and obtain the controller
         * that will process the request. If no controller is available,
         * return false and let other filters/servlets process the request.
         */
        IGTVGController controller = this.application.resolveControllerForRequest(request);
        if (controller == null) {
            return false;
        }

        /*
         * Obtain the TemplateEngine instance.
         */
        ITemplateEngine templateEngine = this.application.getTemplateEngine();

        /*
         * Write the response headers
         */
        response.setContentType("text/html;charset=UTF-8");
        response.setHeader("Pragma", "no-cache");
        response.setHeader("Cache-Control", "no-cache");
        response.setDateHeader("Expires", 0);

        /*
         * Execute the controller and process view template,
         * writing the results to the response writer. 
         */
        controller.process(
                request, response, this.servletContext, templateEngine);
        
        return true;
        
    } catch (Exception e) {
        try {
            response.sendError(HttpServletResponse.SC_INTERNAL_SERVER_ERROR);
        } catch (final IOException ignored) {
            // Just ignore this
        }
        throw new ServletException(e);
    }
    
}

```
IGTVGController：


```java
public interface IGTVGController {

    public void process(
            HttpServletRequest request, HttpServletResponse response,
            ServletContext servletContext, ITemplateEngine templateEngine);    
    
}

```
我们现在要做的就是创建IGTVGController接口的实现，然后使用ITemplateEngine对象从服务层和处理模板中检索数据。

最后，会看到这样项目主页：
![](http://www.thymeleaf.org/doc/tutorials/3.0/images/usingthymeleaf/gtvg-view.png)

现在让我们了解下模板引擎是怎么初始化的。

