---
aliases:
  - Servlet
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: Web Server
cssclasses:
  - 
---
>Un **Servlet** è un componente di software che viene eseguito su un [[Server]] web per *elaborare le richieste* [[003 HTTP|HTTP]] e *generare le risposte* da inviare a uno o più client. 

È una classe Java che estende la classe `HttpServlet` e che viene utilizzata per implementare la logica di gestione delle richieste HTTP e delle risposte del server.

Tipicamente sono collocate e **gestite** all'interno di [[011 Web Dinamico#CONTAINER / APPLICATION SERVER|Application Server]] come, ad esempio, [[012 Apache Tomcat|Tomcat]].

Consentono di implementare con relativa semplicità anche svariate funzionalità complesse e importanti come, ad esempio, l'*accesso ad un database*.

Il suo utilizzo è un esempio di [[009 Client-side e Server-side#SERVER SIDE|programmazione in server side]] e deve [[Keyword extends|estendere]] la classe `HttpServlet`.

![[Pasted image 20230330085941.png]]

#### Servlet Context
Quando un [[011 Web Dinamico#CONTAINER / APPLICATION SERVER|Web Container]] inizializza un Servlet, questo crea un **Servlet Context**, che fornisce al servlet dei metodi per comunicare con il container.

Il Servlet Context rappresenta il contesto dell'applicazione web, ovvero le informazioni relative all'applicazione stessa, come il percorso di radice dell'applicazione, i parametri di configurazione, gli attributi condivisi tra i componenti web e altro ancora.

![[Pasted image 20230418104515.png|650]]

---
### HttpServletRequest E HttpServletResponse
Il flusso di esecuzione di una servlet è incentrato su due componenti fondamentali: la [[004 Richiesta e Risposta HTTP|richiesta e la risposta]].

Un oggetto di tipo `HttpServletRequest` consente ad una Servlet di *ricavare svariate informazioni sul sistema e sull'ambiente relativo al client*. 

L'oggetto di tipo `HttpServletResponse`, invece, costituisce *la risposta da inviare al client* e che può essere dipendente da svariati data source.

---
### I metodi principali
Creare una Servlet vuol dire, in termini pratici, definire una classe che derivi dalla classe `HttpServlet`. 

I [[Metodi]] più comuni per molti dei quali si è soliti eseguire l'[[Annotazioni#OVERRIDE|overriding]] nella classe derivata sono i seguenti:
- **`init()`**: 
	- viene invocato *soltanto una volta* dall'application server al termine del caricamento della servlet ed appena prima che la servlet stessa inizi ad esaudire le richieste che le pervengono.

- **`service(HttpServletRequest req, HttpServletResponse resp)`**: 
	- viene invocato al termine del metodo `init()`. Gestisce la richiesta contenuta nell'oggetto `req`, inviando poi la risposta attraverso l'oggetto `resp`. Il metodo `service()` si occupa di *smistare le chiamate ad altri metodi* (come `doGet()` o `doPost()`), in funzione della tipologia di richiesta HTTP.

- **`doGet(HttpServletRequest req, HttpServletResponse resp)`**:
	- gestisce le richieste HTTP di tipo [[004 Richiesta e Risposta HTTP#^86d27c|GET]] e viene invocato da `service()`.

- **`doPost(HttpServletRequest req, HttpServletResponse resp)`**:
	- gestisce le richieste HTTP di tipo [[004 Richiesta e Risposta HTTP#^9fc0de|POST]] e viene invocato da `service()`.

- **`destroy()`**:
	- È invocato direttamente dall'application server per liberare la memoria usata da una servlet.

Il terzo e quarto metodo sono quelli in cui si scrive codice personale.

---
> [!example]- <font color="orange">Esempio</font>
>Una servlet che visualizza la conversione da gradi Celsius a Fahrenheit.
>
>Abbiamo suddiviso la pagina HTML generata dalla servlet in 3 parti: 
>- La parte superiore (`TOP`);
>- La parte inferiore (`BOTTOM`);
>- La parte centrale. 
>
>Le prime due sono rappresentate da stringhe statiche, definite come costanti (`PAGE_TOP`, `PAGE_BOTTOM`). 
>La parte centrale, invece, è generata dinamicamente e gestita dal metodo `doGet()` della Servlet.
>
>Si noti come, per comodità, sia stato fatto l'override anche del metodo `doPost()`, il cui codice si preoccupa semplicemente di inoltrare la richiesta al metodo `doGet()`.
>
>Si può accedere alla pagina web attraverso `http://localhost/Nome_Progetto/CelsiusToFahrenheit`, dove `/CelsiusToFahrenheit` è specificato dall'annotazione `@WebServlet()`.
>
>```Java
>import java.io.IOException;  
>import javax.servlet.ServletException;  
>import javax.servlet.http.HttpServletRequest;  
>import javax.servlet.http.HttpServletResponse;  
>import java.text.DecimalFormat;  
>import java.io.PrintWriter;  
>import javax.servlet.http.HttpServlet;
>
>// La seguente annotazione specifica il path da inserire nell'URL per accedere al servlet
>@WebServlet("/CelsiusToFahrenheit")
>public class CelsiusToFahrenheit extends HttpServlet {  
>	private static final long serialVersionUID = 1L;
>
>	public CelsiusToFahrenheit() {
>		super();
>	}
>
>	private static final DecimalFormat FMT = new DecimalFormat("#0.00");
>
>	// HTML all'inizio della pagina, non cambia mai
>	private static final String PAGE_TOP = ""  
>		+ "<html>"  
>		+ "<head>"  
>		+ "<title>Tabella di conversione da Celsius a Fahrenheit</title>"  
>		+ "</head>"  
>		+ "<body>"  
>		+ "<h3>Tabella di conversione da Celsius a Fahrenheit</h3>"  
>		+ "<table>"
>		+ "<tr>"  
>		+ "<th>Celsius</th>"  
>		+ "<th>Fahrenheit</th>"  
>		+ "</tr>";
>
>	// HTML alla fine della pagina, non cambia mai
>	private static final String PAGE_BOTTOM = ""  
>		+ "</table>"
>		+ "<p>Fine della pagina</p>"
>		+ "</body>"  
>		+ "</html>";
>
>	// Metodo per gestire la richiesta GET
>	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
>		response.setContentType("text/html");  
>		
>		PrintWriter out = response.getWriter();
>		out.println(PAGE_TOP); // Stampa l'inizio della pagina  
>	
>		// Stampa l'HTML generato dinamicamente, cambia per ogni iterazione
>		for(double cels = 15; cels <= 35; cels += 1.0) {  
>			Double fahre = (cels * 1.8) + 32;  
>			out.println("<tr>");  
>			out.println("<td>" + FMT.format(cels) + "</td>");  
>			out.println("<td>" + FMT.format(fahre) + "</td>");  
>			out.println("</tr>");  
>		}  
>		
>		out.println(PAGE_BOTTOM); // Stampa la fine della pagina  
>	}
>
>	// Non accettiamo le richieste POST, il modo migliore in questo caso è quello di far eseguire doGet
>	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
>		this.doGet(request,response);  
>	}
>}
>```
>
>![[Pasted image 20230330094444.png]]

---
### Prelevare informazioni dall'URL di una richiesta
Le informazioni mandate sia con GET che con POST si possono ricavare attraverso dei metodi specifici, quello più importante è **`getParameter(String par)`**.

#### Da GET
Se all'interno di una richiesta GET viene inserita nell'[[002 URL|URL]] una o più query, 

*URL*: `http://.../HelloServlet?name=Mario`

*Servlet*:
```Java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
	response.setContentType("text/html");  
	PrintWriter out = response.getWriter();

	String name = request.getParameter("name");
	String result = ""
		+ "<html>"
		+ "<head><title>Hello</title></head>"
		+ "<body>Hello " + name + "!</body>"
		+ "</html>"

	out.println(result);  
}
```

#### Da POST
Quando vengono [[008 Inviare Form|inviati dei form]] con il metodo POST `getParameter()` si usa in modo molto simile al GET.

*HTML*:
```HTML
<form action="HelloServlet" method="post">
	First Name: <input type="text" name="firstname"/><br/>
	Last Name: <input type="text" name="lastname"/>
	<input type="submit">
</form>
```

*Servlet*:
```Java
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
	response.setContentType("text/html");  
	PrintWriter out = response.getWriter();

	String first_name = request.getParameter("firstname");
	String last_name = request.getParameter("lastname");
	
	String result = ""
		+ "<html>"
		+ "<head><title>Hello</title></head>"
		+ "<body>Hello " + first_name + " " + last_name + "!</body>"
		+ "</html>";

	out.println(result);  
}
```


___
[[000 Indice TSW|↖ Ritorna all'indice ↖]]

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
- [HTML.it](https://www.html.it/articoli/primi-passi-con-le-servlet/)
