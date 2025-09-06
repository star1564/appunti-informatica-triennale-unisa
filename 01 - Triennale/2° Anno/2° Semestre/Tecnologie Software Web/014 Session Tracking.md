---
aliases: 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: Web Server
cssclasses:
  - 
---
Dato che l'[[003 HTTP|HTTP]] è un [[Protocollo]] [[003 HTTP#^18998a|Stateless]], non ci sono meccanismi nativi per il *mantenimento dello stato* tra diverse richieste provenienti dallo stesso [[Client]].

>Le applicazioni [[001 Introduzione a TSW|Web]] hanno spesso bisogno di uno **Stato**, quindi per mantenere traccia delle informazioni di stato si sfrutta la  **Sessione** (*Session Tracking*), che può essere implementata attraverso uno solo di questi metodi:
>- [[Cookie]] (metodo elementare);
>- [[008 Inviare Form|Form]] nascosto;
>- [[002 URL|URL]] Rewriting;
>- HttpSession.

---
### Cookie
Un [[Cookie]] contiene un certo numero di informazioni, tra cui:
- Una *coppia nome/valore*, utilizzata per identificare il client;
- Il *dominio* internet dell'applicazione che ne fa uso;
- Il *path* dell'applicazione su cui sarà valido il cookie;
	- Se impostato, il cookie sarà disponibile solo sull'[[002 URL|URL]] specificato.
	- Se non impostato, il cookie sarà disponibile su tutto il sito.
- Una *data di scadenza* espressa in secondi;
	- Se `-1` il cookie scadrà alla chiusura del browser. 
- Un valore booleano per definirne il *livello di sicurezza*.

Si utilizzano vari [[Metodi]], inclusi nella classe `Cookie`: 
- **`getCookies()`** nella [[004 Richiesta e Risposta HTTP#RICHIESTA|richiesta]] per ottenere i cookie; 
- **`addCookie()`** nella [[004 Richiesta e Risposta HTTP#RISPOSTA|risposta]] per aggiungere dei cookie;
- *`setSecure()`* nell'[[Oggetti|Oggetto]] `Cookie` per forzare a inviare il cookie solo su [[005 Sicurezza per HTTP|HTTPS]].

> [!example]- <font color="orange">Esempio</font>
>*Creazione Cookie*
>```Java
>Cookie c = new Cookie("NomeCookie", "valore");
>c.setSecure(true);
>c.setMaxAge(3600); // 1 ora (3600 secondi)
>c.setPath("/") // Valido su tutto il sito
>
>// Aggiunta cookie alla risposta
>response.addCookie(c);
>```
>
>*Lettura Cookie*
>```Java
>Cookie[] cookies = request.getCookies();
>if (cookies != null) {
>  for (Cookie cookie : cookies) {
>    String name = cookie.getName();
>    String value = cookie.getValue();
>    // ...
>  }
>}
>```

Dopo aver utilizzato la `addCookie()` è possibile riutilizzare la variabile per istanziare un altro cookie.

---
### Metodi avanzati per la Sessione
Se il browser ha disabilitato l'utilizzo dei cookie, si utilizza la **Sessione** con uno dei seguenti mezzi.
La Sessione contiene vari dati ed è identificata univocamente da un **Session ID**.

#### Form Nascosto
Nel caso di un **Form Nascosto** si utilizza un textfield per mantenere lo stato di un utente.

Conserviamo le informazioni in questo campo nascosto e le mandiamo ad un altro servlet.

```HTML
<input type="hidden" name="uname" value="John Smith">
```

#### URL Rewriting
Attraverso l'id possiamo avere la session tracking con l'utilizzo dell'**URL rewriting**.

L'identificatore della sessione viene aggiunto all'URL della pagina che l'utente sta visitando. Ad esempio, l'URL potrebbe apparire come `http://www.example.com/index.html?sessionid=1234`. 

Questo identificatore viene quindi utilizzato dal server per associare l'utente alla sua sessione.

> [!info] Codificare l'URL
> È buona prassi *codificare sempre le URL* generate dalle [[013 Servlet|Servlet]] usando il metodo `encodeURL()` di `HttpServletResponse`.

#### HttpSession
L'accesso alla sessione avviene attraverso l'[[018 Interfacce|Interfaccia]] **`HttpSession`**, grazie alla quale si può ottenere un riferimento con il metodo *`getSession(boolean createNew)`*.
- Se viene passato `true`, ritorna la sessione esistente o se non esiste ne crea una nuova.
- Se viene passato `false`, ritorna, se possibile, la sessione esistente, altrimenti ritorna `null`.

![[Pasted image 20230405091649.png|500]]

Si possono memorizzare dati specifici dell'utente negli *attributi della sessione* sotto forma di coppie nome/valore.

> [!example]- <font color="orange">Esempio</font>
>```Java
>HttpSession session = request.getSession();
>// Metti in sc la sessione caricata dall'attributo
>Cart sc = (Cart) session.getAttribute("shoppingCart");
>sc.addItem(item);
>...
>// Rimuovi l'attributo
>session.removeAttribute("shoppingCart");
>...
>// Imposta l'attributo
>session.setAttribute("shoppingCart", new Cart());
>...
>// Returns an Enumeration of String objects containing the names of all the objects bound to this session
>Enumeration e = session.getAttributeNames();
>while(e.hasMoreElements())
>	out.println("Key: " + (String) e.nextElements());
>```

Si può ottenere l'*ID della Sessione* attraverso il metodo `getID()`, che ritorna una stringa. 

Per "chiudere" una sessione si utilizza il metodo `invalidate()`.

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
- [Javapoint](https://www.javatpoint.com/session-tracking-in-servlets)