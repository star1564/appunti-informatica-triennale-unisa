---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Enterprise Edition
cssclasses:
  - 
---
>Un **Interceptor** è un servizio di [[027 Context e Dependency Injection|CDI]] (simile ai [[020 Filtri Servlet|Filtri Servlet]]) che permette di *intercettare* (ovvero catturare e gestire) *le chiamate a metodi* in altre classi. Sono spesso usati per aggiungere funzionalità a dei metodi senza modificarne il codice. È molto utile per eseguire il *debugging* del codice.
>
>Per loro natura, *devono essere totalmente all'oscuro* della [[003 Fasi dello Sviluppo del Programma#SINTATTICA E SEMANTICA|Semantica]] delle azioni che intercettano.

![[Pasted image 20231022110532.png]]

Il [[021 Container Java EE|container]] si occupa di chiamare gli interceptor (con un comportamento definito dal programmatore) prima e/o dopo l'invocazione del metodo.

> [!example]- <font color="orange">Esempio</font>
>Si vuole registrare il tempo di esecuzione di un metodo attraverso un logger, senza modificarne il codice originale.
>
>![[Immagine 2023-10-22 112108.png]]

## Classe Interceptor
### Creare un Interceptor
Supponiamo di voler loggare il tempo passato per l'esecuzione dei metodi di una classe.

```Java
public class MyClass {	
	public void metodo1() {	System.out.println("Dentro il metodo1"); }
	public void metodo2() {	System.out.println("Dentro il metodo2"); }
}
```

Si può specificare un nuovo Interceptor attraverso l'annotazione **`@Interceptor`**.

```Java
import javax.interceptor.Interceptor;

@Interceptor
public class MyInterceptor {
	// metodi interceptor...
}
```


> [!warning] Aggiornare `beans.xml`
> Per impostazione predefinita, tutti gli interceptor sono disabilitati. È necessario abilitarli nel `beans.xml` nella cartella `WEB-INF`.
>```xml
><beans xmlns="http://xmlns.jcp.org/xml/ns/javaee"
>       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
>       xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/beans_1_1.xsd"
>       version="1.1" bean-discovery-mode="all">
>
>    <interceptors>
>        <class>percorso.del.package.nomeClasseInterceptor</class>
>    </interceptors>
></beans>
>```
### Abbinare un Interceptor
Per poter abbinare l'interceptor alla classe che bisogna intercettare ci sono due modi:
- Usare l'annotazione **`@Interceptors(nomeClasseInterceptor.class)`**.
- Usare il **binding**, ovvero una *annotazione personalizzata* che funge da `@Interceptors`, attraverso **`@InterceptorBinding`**.
	- Si deve inserire l'annotazione personalizzata sia sulla classe dell'interceptor che sul punto che si vuole intercettare.
	- Con questo modo si perde la possibilità di ordinare multipli interceptors. Per risolvere questo problema si inserisce una `@Priority(priorityInt)` che indica la *priorità* di ciascun interceptor. Più l'intero è basso, maggiore è la priorità. Si inserisce sulla classe dell'interceptor.

> [!example]- <font color="orange">Esempio con `@Interceptors`</font>
>```Java
>public class MyClass1 {
>	// In questo caso, l'interceptor viene attivato solamente quando 
>	// viene chiamato il metodo1
>	
>	@Interceptors(MyInterceptor.class)
>	public void metodo1() {	System.out.println("Dentro il metodo1"); }
>	
>	public void metodo2() {	System.out.println("Dentro il metodo2"); }
>}
>
>// --------------------------------------------------
>
>@Interceptors(MyInterceptor.class)
>public class MyClass2 {
>	// In questo caso, l'interceptor viene attivato quando
>	// viene chiamato il metodo1 oppure il metodo2
>
>	public void metodo1() {	System.out.println("Dentro il metodo1"); }
>	
>	public void metodo2() {	System.out.println("Dentro il metodo2"); }
>}
>```

> [!example]- <font color="orange">Esempio con Binding</font>
>Situazione analoga a quella precedente ma si usa una annotazione creata con:
>```Java
>import java.lang.annotation.ElementType;
>import java.lang.annotation.Retention;
>import java.lang.annotation.RetentionPolicy;
>import java.lang.annotation.Target;
>import javax.interceptor.InterceptorBinding;
>
>@InterceptorBinding
>@Retention(RetentionPolicy.RUNTIME)
>@Target({ElementType.METHOD, ElementType.TYPE})
>public @interface Logged {}
>// Si usa @Logged invece di @Interceptors(MyInterceptor.class)
>```
>
>```Java
>import javax.interceptor.Interceptor;
>import javax.annotation.Priority;
>
>@Interceptor
>@Logged           // Annotazione personalizzata per il binding
>@Priority(200)    // Priorità interceptor
>public class MyInterceptor {
>	// metodi interceptor...
>}
>```
>```Java
>public class MyClass1 {
>	// In questo caso, l'interceptor viene attivato solamente quando 
>	// viene chiamato il metodo1
>	
>	@Logged
>	public void metodo1() {	System.out.println("Dentro il metodo1"); }
>	
>	public void metodo2() {	System.out.println("Dentro il metodo2"); }
>}
>
>// --------------------------------------------------
>
>@Logged
>public class MyClass2 {
>	// In questo caso, l'interceptor viene attivato quando
>	// viene chiamato il metodo1 oppure il metodo2
>
>	public void metodo1() {	System.out.println("Dentro il metodo1"); }
>	
>	public void metodo2() {	System.out.println("Dentro il metodo2"); }
>}
>```

### Usare un Interceptor
Per utilizzare altrove una classe che viene intercettata, si usa la [[@Inject]].

> [!example]- <font color="orange">Esempio</font>
>```Java
>import java.io.IOException;
>import java.io.PrintWriter;
>import javax.inject.Inject;
>import javax.servlet.ServletException;
>import javax.servlet.annotation.WebServlet;
>import javax.servlet.http.HttpServlet;
>import javax.servlet.http.HttpServletRequest;
>import javax.servlet.http.HttpServletResponse;
>
>@WebServlet(name = "NewServlet", urlPatterns = {"/NewServlet"})
>public class NewServlet extends HttpServlet {
>
>	// Inject della classe
>	@Inject
>	MyClass mc;
>	
>	protected void doGet(HttpServletRequest request, HttpServletResponse response)
>			throws ServletException, IOException {
>
>		// Chiamate ai metodi
>		mc.metodo1(); 
>		mc.metodo2();
>		
>		response.setContentType("text/html;charset=UTF-8");
>		try ( PrintWriter out = response.getWriter()) {
>			out.println("<!DOCTYPE html>");
>			out.println("<html>");
>			out.println("<head>");
>			out.println("<title>Servlet NewServlet</title>");			
>			out.println("</head>");
>			out.println("<body>");
>			out.println("<h1>Controlla i Log di GlassFish</h1>");
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

## Metodi Interceptor
Esistono quattro tipi di metodi interceptor:
- Associati ad un costruttore della classe target: **`@AroundConstruct`**;
- Associati ad uno specifico business method: **`@AroundInvoke`**;
- Di timeout: **`@AroundTimeout`**;
- Call-back del [[026 Managed Beans - CDI Bean#Ciclo di Vita|ciclo di vita]]: **`@PostConstruct`** e **`@PostDestroy`**.

È possibile inserire un metodo interceptor direttamente nel bean interessato.
### Sintassi
La sintassi è la seguente:

```Java
Object methodName(InvocationContext ic) throws Exception;
```

Le seguenti regole si applicano a tutti e quattro i tipi di interceptor:
- Il metodo può essere `public`, `private`, `protected`, ma non deve essere `static` o `final`;
- Il metodo deve avere come parametro un `javax.interceptor.InvocationContext` e deve ritornare un `Object`, ovvero il risultato del metodo target dell'invocazione;
- Il metodo può lanciare una eccezione.

L'oggetto `InvocationContext` permette di concatenare diversi interceptors, in modo che il contesto sia passato da uno all'altro, aggiungendo se necessario delle informazioni. Inoltre restituisce l'oggetto che il metodo intercettato restituisce.

> [!tip]- @AroundInvoke
>![[@AroundInvoke]]

> [!note]- Ottenere un [[041 Enterprise Java Beans|EJB]] in un interceptor
>```Java
>import javax.interceptor.AroundInvoke;
>import javax.interceptor.Interceptor;
>import javax.interceptor.InvocationContext;
>import javax.naming.InitialContext;
>
>@Interceptor
>public class MyInterceptor {
>
>    @AroundInvoke
>    public Object intercept(InvocationContext context) throws Exception {
>        // Usa InitialContext per cercare l'EJB
>        MyEJB myEJB = (MyEJB) new InitialContext().lookup("java:global/your-application-name/your-module-name/MyEJB");
>        myEJB.doSomething();
>        return context.proceed();
>    }
>}
>```


___
[[000 Indice PD|↖ Ritorna all'indice ↖]]

```dataviewjs
/*
function extractUpperCaseLetters(inputString) {
	// Use a regular expression to match uppercase letters (A-Z)
	const uppercaseLetters = inputString.match(/[A-Z]/g);
	
	// Check if uppercaseLetters is not null, and join the matched letters into a string
	if (uppercaseLetters !== null) {
		return uppercaseLetters.join('');
	} else {
	    // If no uppercase letters were found, return an empty string
	    return '';
	}
}
*/

function extractNumberFromString(inputString) {
	const match = inputString.match(/^\d{3}/);
	
	if (match) {
		return match[0];
	} else {
		return null; // Return null if no match is found
	}
}

function startsWithNumber(inputString, targetNumber) {
  // Use a regular expression to check if the string starts with the target number
  const pattern = new RegExp(`^${targetNumber}`);
  return pattern.test(inputString);
}

function sort2Array(array){
	let res = []
	
	if (array[0] == undefined)
		res.push(array[1], array[1])
	else if (array[1] == undefined)
		res.push(array[0], array[0])
	else if (parseInt(extractNumberFromString(array[0])) > parseInt(extractNumberFromString(array[1])))
		res.push(array[1], array[0])
	else
		res.push(array[0], array[1])
	
	return res
}

let toDisplay = []
function searchPrevAndNext(tag, currentNumber) {
	const prevNumber = (parseInt(currentNumber) - 1).toString().padStart(3, "0");
	const nextNumber = (parseInt(currentNumber) + 1).toString().padStart(3, "0");
	
	let prevAndNext = [(dv.pages(tag).where(p => startsWithNumber(p.file.name, prevNumber) || startsWithNumber(p.file.name, nextNumber)).file.name)]
	
	// Prev = [0]; Next = [1]
	let sortedPrevAndNext = sort2Array(prevAndNext[0])
	
	if (currentNumber == "001"){ 
		// Se è la prima pagina aggiungi solo il prossimo
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva →" + "]]"
		toDisplay.push(nextLink)
	} else if (prevAndNext[0][1] == undefined){
		// Se è l'ultima pagina aggiungi solo il precedente
		let prevLink = "[[" + sortedPrevAndNext[0] + "|" + "← Nota Precedente" + "]]"
		toDisplay.push(prevLink)
	} else {
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva →" + "]]"
		toDisplay.push(nextLink)
		
		let prevLink = "[[" + sortedPrevAndNext[0] + "|" + "← Nota Precedente" + "]]"
		toDisplay.push(prevLink)
	}
	
	
}

if (dv.current().tags[0] == null || dv.current().tags[0] == undefined){
	dv.header(1, "Errore - Inserire il tag nelle proprietà del file")
} else {
	let tag = "#" + dv.current().tags[0]

	// Purtroppo obsidian non riesce a leggere i link e a creare connessioni nel grafo,
	// quindi ho disattivato il link all'indice automatico e la funzione extractUpperCaseLetters
	//let indexName = "000 Indice " + extractUpperCaseLetters(tag)
	//let index = dv.page(indexName).file
	//let indexLink = "[[" + index.name + "|" + "↖ Ritorna all'indice ↖" + "]]"
	//toDisplay.push(indexLink)
	
	let currentPage = dv.current().file
	let currentPageNumber = extractNumberFromString(currentPage.name)
	
	searchPrevAndNext(tag, currentPageNumber)
	
	dv.list(toDisplay)
}
```

Altri collegamenti: 
- [Github Libro](https://github.com/agoncal/agoncal-book-javaee7/blob/master/chapter02/chapter02-putting-together/src/main/java/org/agoncal/book/javaee7/chapter02/LoggingInterceptor.java)