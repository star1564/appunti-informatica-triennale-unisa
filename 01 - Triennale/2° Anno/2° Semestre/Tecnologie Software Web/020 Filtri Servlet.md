---
aliases: 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: JSP
cssclasses:
  - 
---
> I **Filtri dei Servlet** sono usati per il *preprocessing* delle richieste ed il *postprocessing* delle risposte che rispettivamente entrano ed escono da una applicazione Web.

Quando un [[011 Web Dinamico#CONTAINER / APPLICATION SERVER|Servlet Container]] chiama un metodo in un [[013 Servlet|Servlet]] per un [[Client]], la [[004 Richiesta e Risposta HTTP#RICHIESTA|richiesta HTTP]] del client viene, di default, *passata direttamente al servlet*. Analoga la situazione della [[004 Richiesta e Risposta HTTP#RISPOSTA|risposta]], viene generata dal servlet e viene *passata direttamente al client*.

Però, ci sono molti casi in cui sarebbe utile il *processamento della richiesta prima dell'arrivo al servlet* (preprocessing), o addirittura il *processamento della risposta*. Questi possono essere eseguiti attraverso i filtri.

![[Pasted image 20230418111900.png|300]]

Normalmente i filtri vengono usati solo se l'applicazione Web *utilizza più di un servlet*. Se si utilizza un solo servlet, si può fare tutto il necessario direttamente nel servlet stesso.

> [!example]+ <font color="orange">Esempio utilizzo del postprocessing</font>
> Un servlet, o un gruppo di servlet all'interno di una applicazione Web, può generare una risposta contenente dati sensibili che non dovrebbero essere mandati in forma normale, specialmente se la connessione è su un [[Protocollo]] non [[005 Sicurezza per HTTP|sicuro]] (come [[003 HTTP|HTTP]]).
> Pertanto, un filtro (che esegue il postprocessing) potrebbe criptare la risposta. Questo sempre se il client è capace di decriptare i messaggi.

---
### Filtro in Java
Un filtro non è altro che una classe java in cui si specificano tre metodi.

```Java
public class MyGenericFilter implements Filter {
  public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
      throws java.io.IOException, javax.servlet.ServletException {  //1
    response.setContentType("text/html");
	PrintWriter out = response.getWriter();
	
	String name = request.getServerName();
	out.println("Name " + name + ", Time " + new Date().toString());
    
    chain.doFilter(request,response);                               //2
  } 

  public void init(FilterConfig filterConfig) {                     //3
    String testParam = config.getInitParameter("test-param");
    System.out.println("Test Param: " + testParam);
  } 

  public void destroy() {                                           //4
  }
}
```

1. Il metodo `doFilter()` contiene il codice che implementa il filtro.
2. In un caso generico, si chiama il chain filter.
3. Il metodo `init()` è utilizzato per ottenere i parametri iniziali oppure per salvare la configurazione del filtro in una variabile.
4. Il metodo `destroy()` può essere usato per completare qualsiasi finalizzazione.


---
### Configurazione Filtri in web.xml
I filtri si possono impostare all'interno del file `web.xml`. L'*ordine* dell'utilizzo dei filtri dipende da come sono configurati nell'xml:
- Il primo filtro in `web.xml` sarà il primo ad essere usato nella *richiesta*;
- L'ultimo filtro in `web.xml` sarà l'ultimo ad essere usato nella *richiesta*.

Da notare che l'ordine è invertito durante la *risposta*.

#### Sintassi
Supponiamo di avere due filtri da implementare.
1. La prima cosa da fare è quella di specificare il filtro con `<filter>`, dove si inserisce il nome (che utilizzeremo nel mapping) e la classe.
2. Eventualmente specificare i parametri iniziali. 
3. Poi bisogna mappare il nome di un filtro al path in cui questo è accessibile (se è `/*` allora sarà disponibile in tutta l'applicazione).

```xml
<filter>
	<filter-name>Nome Filtro 1</filter-name>
	<filter-class>Filtro1</filter-class>
	<init-param>
		<param-name>test-param</param-name>
		<param-value>valore</param-value>
	</init-param>
</filter>

<filter>
	<filter-name>Nome Filtro 2</filter-name>
	<filter-class>Filtro2</filter-class>
</filter>

<filter-mapping>
	<filter-name>Nome Filtro 1</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>

<filter-mapping>
	<filter-name>Nome Filtro 2</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
```

In questo caso, nella richiesta si attiveranno in ordine `Filtro1` poi `Filtro2`; invece nella risposta si attiveranno `Filtro2` poi `Filtro1`.

#### Annotazione WebFilter
Alternativamente si può usare l'[[Annotazioni|Annotazione]] `@WebFilter` per inserire automaticamente il filtro in `web.xml` (però bisogna modificare l'ordine manualmente).

Questa è la sintassi:
```Java
@WebFilter(filterName = "Filtro1", urlPatterns = {"/*"}, initParams = {
	@WebInitParam(name = "test-param", value = "valore")}
)
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
- [Oracle](https://docs.oracle.com/cd/B14099_19/web.1012/b14017/filters.htm)