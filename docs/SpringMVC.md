# Intro

The Spring MVC Framework follows the Model-View-Controller architectural design pattern which works around the Front Controller i.e. the Dispatcher Servlet.

The Dispatcher Servlet handles and dispatches all the incoming HTTP requests to the appropriate controller. It uses @Controller and @RequestMapping as default request handlers. The @Controller annotation defines that a particular class is a controller. @RequestMapping annotation maps web requests to Spring Controller methods. The terms model, view, and controller are as follows:

- View: Render the model data generate HTML output
- Controller: Processes user requests and passes to view for rendering
- Model: encapsulates application data

![alt text](https://media.geeksforgeeks.org/wp-content/uploads/20231106150237/Spring-MVC-Framework-Control-flow-Diagram.png)