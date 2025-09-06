---
aliases: 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: JSP
cssclasses:
  - 
---
I due aspetti di **Sicurezza** principali nelle applicazioni web sono:
- *Impedire agli utenti non autorizzati di accedere a dati sensibili*.
	- Questo processo prevede la **Restrizione** degli accessi (identificando quali *risorse* necessitano di protezione e chi dovrebbe accedervi) e l'**Autenticazione** (identificando gli *utenti* per determinare se sono uno di quelli autorizzati)
- *Impedire agli attaccanti di rubare dati di rete mentre questi sono in transito*.
	- Questo processo prevede l'uso di TLS (Transport Layer Security) o SSL (Secure Sockets Layer) per crittografare il traffico tra il browser e il server. Compongono l'[[005 Sicurezza per HTTP|HTTPS]].

---
### Restrizione degli accessi
Esistono due modalità per la restrizione degli accessi:
- Restrizione degli accessi in **Maniera Dichiarativa**:
	- Le singole [[013 Servlet|Servlet]] o [[016 JSP|pagine JSP]] *non necessitano di alcun codice atto alla sicurezza*. 
	- La restrizione degli accessi è gestita dal [[011 Web Dinamico#CONTAINER / APPLICATION SERVER|Servlet Container]] (dichiarando dei parametri di configurazione in `web.xml`).
- Restrizione degli accessi in **Maniera Programmatica**:
	- Le Servlet e le pagine JSP *devono garantire la restrizione degli accessi*.
	- Per impedire l'accesso non autorizzato, ogni Servlet o pagina JSP protetta deve verificare che l'utente sia autenticato e che abbia il permesso per accedere alla Servlet o pagina JSP richiesta.

#### Implementazione (maniera programmatica)
1. Separare in due gruppi le Servlet o pagine JSP:
	- Quelle accessibili ai soli *admin*.
		- Inserirle in una cartella chiamata `admin/`.
		- Url: `/admin/myServlet`.
	- Quelle accessibili agli *utenti registrati e agli admin*.
		- Inserirle in una cartella chiamata `common/`.
		- Url: `/common/myServlet2`.
2. Inserire in un database i *nomi utente* e *password* (le password devono essere conservate attraverso una [[Funzione Hash]]).
3. Se l'utente o admin è autorizzato dopo il *login*, salvare un [[014 Session Tracking|token nella sessione]].
4. In ogni Servlet o pagina JSP protetta, assicurarsi che l'utente sia autenticato (ovvero *controllare se il token è nella sessione*). 
	- Se non è autorizzato, lo si rimanda alla pagina di login.
5. Quando l'utente esegue il *logout*, invalido la sessione con `session.invalidate()`.

> [!example]- <font color="orange">Esempio</font>
>```Java
>@WebServlet("/Login")
>public class Login extends HttpServlet {
>	protected void doPost(HttpServletRequest request, HttpServletResponse response)
>		throws ServletException, IOException {
>
>		String username = request.getParameter("username");
>		String password = request.getParameter("password");
>		List<String> errors = new ArrayList<>();
>		RequestDispatcher dispatcherToLoginPage = request.getRequestDispatcher("login.jsp");
>
>		if(username == null || username.trim().isEmpty()) {
>			errors.add("Il campo username non può essere vuoto!");
>		}
>		if(password == null || password.trim().isEmpty()) {
>			errors.add("Il campo password non può essere vuoto!");
>		}
>		
>		if (!errors.isEmpty()) {
>			request.setAttribute("errors", errors);
>			dispatcherToLoginPage.forward(request, response);
>			return; // Da notare il return
>		}
>		
>		// Andare a vedere per ogni controllo nel database
>		if(username.equals("admin") && password.equals("mypass")){ //admin
>			//inserisco il token nella sessione
>			request.getSession().setAttribute("isAdmin", Boolean.TRUE);
>			response.sendRedirect("admin/protected.jsp");
>		} else if (username.equals("user") && password.equals("mypass")){ //user
>			//inserisco il token nella sessione
>			request.getSession().setAttribute("isAdmin", Boolean.FALSE);
>			response.sendRedirect("common/protected.jsp");
>		} else {
>			errors.add("Username o password non validi!");
>			request.setAttribute("errors", errors);
>			dispatcherToLoginPage.forward(request, response);
>		}
>	}
>```


> [!tip] Utilizzare un [[020 Filtri Servlet|filtro servlet]] per evitare di scrivere lo stesso codice di controllo autorizzazione su servlet diverse



---
### Attacchi XSS e ClickJacking

> [!quote] Descrizione
>- **Cross-site Scripting** (XSS): consente ad attaccanti di iniettare script lato client nelle pagine Web visualizzate da altri utenti.
>- **ClickJacking**: è un attacco che ha come fine quello di indurre un utente a fare clic su qualcosa di diverso da ciò che l'utente percepisce, rivelando potenzialmente informazioni riservate o consentendo ad altri di assumere il controllo del proprio computer.

> [!note]- Come difendersi
> Aggiungere al file `web.xml` il seguente codice:
>```XML
><filter>
>	<filter-name>httpHeaderSecurity</filter-name>
>	<filter-class>org.apache.catalina.filters.HttpHeaderSecurityFilter</filter-class>
>	
>	<init-param>
>		<param-name>xssProtectionEnabled</param-name>
>		<param-value>true</param-value>
>	</init-param>
>	
>	<init-param>
>		<param-name>antiClickJackingEnabled</param-name>
>		<param-value>true</param-value>
>	</init-param>
>	
>	<init-param>
>		<param-name>antiClickJackingOption</param-name>
>		<param-value>DENY</param-value>
>	</init-param>
></filter>
>
><filter-mapping>
>	<filter-name>httpHeaderSecurity</filter-name>
>	<url-pattern>/*</url-pattern>
></filter-mapping>
>```


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