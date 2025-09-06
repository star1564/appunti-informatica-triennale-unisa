---
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: GlassFish Help
cssclasses:
  - 
---
## Creare un nuovo progetto [[027 Context e Dependency Injection|CDI]]
`New Project ➡️ Java with Ant ➡️ Java Web ➡️ Web Application`

![[Pasted image 20231119170001.png]]

**Crea un nuovo package**:
- `Source Packages (Tasto destro) ➡️ New ➡️ Java Package...` (dargli un nome)

**Crea una servlet**:
- `(Tasto destro sul progetto) ➡️ New... ➡️ Servlet...`
- Check su `Add information to deplyment descriptor (web.xml)`
- Imposta `URL Pattern(s)` a `/` per avere un accesso rapido al sito

## Funzionalità CDI
**Crea un nuovo `beans.xml`**:
- `(Tasto destro sul progetto) ➡️ New... ➡️ beans.xml (CDI Configuration File)`
- Selezionare la modalità di discovery `all` nel file `Web Pages/WEB-INF/beans.xml` (oppure in `Configuration Files`).

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://xmlns.jcp.org/xml/ns/javaee"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/beans_1_1.xsd"
       bean-discovery-mode="all">
</beans>
```

> [!example]- <font color="orange">Esempio con l'inject di una VARIABILE SEMPLICE (String)</font>
>*Producer Class*:
>```Java
>import javax.enterprise.inject.Produces;
>
>public class StringProducer {
>	@Produces
>	private String str = "hello";
>}
>```
>
>*Servlet*:
>```Java
>import java.io.IOException;
>import java.io.PrintWriter;
>import javax.inject.Inject;
>import javax.servlet.ServletException;
>import javax.servlet.http.HttpServlet;
>import javax.servlet.http.HttpServletRequest;
>import javax.servlet.http.HttpServletResponse;
>
>public class MyServlet extends HttpServlet {
>
>	@Inject
>	private String str;
>	
>	@Override
>	protected void doGet(HttpServletRequest request, HttpServletResponse response)
>			throws ServletException, IOException {
>		response.setContentType("text/html;charset=UTF-8");
>		try ( PrintWriter out = response.getWriter()) {
>			out.println("<!DOCTYPE html>");
>			out.println("<html>");
>			out.println("<head>");
>			out.println("<title>Servlet NewServlet</title>");			
>			out.println("</head>");
>			out.println("<body>");
>			out.println("<h1>Servlet NewServlet at " + request.getContextPath() + "</h1>");
>			out.println("<p>" + str + "</p>");
>			out.println("</body>");
>			out.println("</html>");
>		}
>	}
>
>	@Override
>	protected void doPost(HttpServletRequest request, HttpServletResponse response)
>			throws ServletException, IOException {
>		doGet(request, response);
>	}
>}
>```

> [!example]- <font color="orange">Esempio con l'inject di una CLASSE</font>
>```Java
>import javax.inject.Inject;
>
>@MyServiceQualifier
>public class MyServiceImpl implements MyService {	
>	@Override
>	public String saySomething(String s) {
>		return "MyService says: " + s;
>	}
>}
>```
>
>```Java
>public interface MyService {
>	String saySomething(String s);
>}
>```
>
>```Java
>import static java.lang.annotation.ElementType.*;
>import java.lang.annotation.Retention;
>import static java.lang.annotation.RetentionPolicy.RUNTIME;
>import java.lang.annotation.Target;
>import javax.inject.Qualifier;
>
>@Qualifier
>@Retention(RUNTIME)
>@Target({METHOD, FIELD, PARAMETER, TYPE})
>public @interface MyServiceQualifier {}
>```
>
>```Java
>// All'interno della servlet
>
>@Inject @MyServiceQualifier
>private MyService service;
>```

## Avviare GlassFish e Compilare il progetto
Assicurarsi di avere la porta 8080 libera.

- `(Tasto destro sul progetto) ➡️ Deploy ➡️ (Tasto destro sul progetto) ➡️ Run`

Per andare sulla console di amministratore GlassFish andare su `localhost:4848`.

%%
[[000 Indice PD|↖ Ritorna all'indice ↖]]
%%