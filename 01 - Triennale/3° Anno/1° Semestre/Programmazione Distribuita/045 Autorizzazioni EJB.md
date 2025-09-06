---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Enterprise Java Bean
cssclasses:
  - 
---
>Lo scopo principale del modello di **Autorizzazioni EJB** è *controllare l'accesso al codice di business*. In Java EE l'autenticazione è gestita dal tier web (o da un'applicazione client); il *Principal* (l'identità di un utente) e i suoi ruoli vengono quindi passati al tier EJB e l'EJB controlla se l'utente autenticato è autorizzato ad accedere a un metodo in base al suo ruolo. 

L'autorizzazione può essere fatta in modo dichiarativo o programmatico.

### Autorizzazione Dichiarativa
>L'**Autorizzazione Dichiarativa** può essere definita nel bean tramite annotazioni o nel [[022 Deployment Descriptor|Deployment Descriptor]]. L'autorizzazione dichiarativa *comporta la dichiarazione di ruoli*, *l'assegnazione di permessi ai metodi* (o all'intero bean) o la modifica temporanea di un'identità di sicurezza.

Ognuna delle seguenti annotazioni può essere usata sul bean e/o sul metodo.


| Annotazione     | Bean | Metodo | Descrizione                                                                                                                                                                                                                                                                                                                         |
| --------------- | ---- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `@PermitAll`    | ✔️   | ✔️     | Indica che il metodo specificato (o l'intero bean) è accessibile da chiunque (tutti i ruoli sono consentiti).                                                                                                                                                                                                                       |
| `@DenyAll`      | ✔️   | ✔️     | Indica che nessun ruolo è autorizzato ad eseguire il metodo specificato o tutti i metodi del bean (tutti i ruoli sono negati). Può essere utile per negare l'accesso a un metodo in un determinato ambiente (ad esempio, il metodo `launchNuclearWar()` dovrebbe essere consentito solo in produzione ma non in un ambiente di test). |
| `@RolesAllowed` | ✔️   | ✔️     | Indica che un ruolo (`String`) o una lista di ruoli è autorizzata ad eseguire il metodo specificato (o l'intero, allora tutti i metodi business erediteranno l'accesso al ruolo del bean bean).                                                                                                                                                                                                                                    |
| `@DeclareRoles` | ✔️   | ❌     | Definisce ruoli per la verifica della sicurezza.                                                                                                                                                                                                                                                                                    |
| `@RunAs`        | ✔️   | ❌     | Assegna temporaneamente un nuovo ruolo a un Principal.                                                                                                                                                                                                                                                                             |

> [!example]- <font color="orange">Esempio</font>
>```Java
>@Stateless
>@RolesAllowed({"user", "employee", "admin"})
>public class ItemEJB {
>	// I ruoli disponibili per questo bean sono: "user", "emplyee", "admin"
>	
>	@PersistenceContext(unitName = "chapter08PU")
>	private EntityManager em;
>
>	@PermitAll
>	public Book findBookById(Long id) {
>		// Accessibile a tutti i ruoli, anche quelli
>		// non elencati in @RolesAllowed
>		return em.find(Book.class, id);
>	}
>	
>	public Book createBook(Book book) {
>		// Accessibile a tutti i ruoli specificati in @RolesAllowed
>		em.persist(book);
>		return book;
>	}
>	
>	@RolesAllowed("admin")
>	public void deleteBook(Book book) {
>		// Accessibile solo ad "admin"
>		em.remove(em.merge(book));
>	}
>
>	@DenyAll
>	public Book findConfidentialBook(Long secureId) {
>		// Nessuno può accedere a questo metodo
>		return em.find(Book.class, secureId);
>	}
>}
>```

### Autorizzazione da Programma
A volte è necessario un controllo maggiore per autorizzare l'accesso (consentire un blocco di codice invece dell'intero metodo, consentire o negare l'accesso a un individuo invece che a un ruolo, ecc.). 

>È possibile utilizzare l'**Autorizzazione da Programma** per *consentire o bloccare selettivamente l'accesso a un ruolo o a un committente*. 

Questo perché si ha accesso diretto all'interfaccia `java.security.Principal` e al contesto [[041 Enterprise Java Beans|EJB]] per verificare il ruolo del Principal nel codice.

L'interfaccia `SessionContext` definisce i seguenti metodi relativi alla sicurezza:
- `isCallerInRole()`: restituisce un booleano e verifica se il chiamante ha un determinato ruolo di sicurezza.
- `getCallerPrincipal()`: restituisce il `java.security.Principal` che identifica il chiamante.

> [!example]- <font color="orange">Esempio</font>
>```Java
>@Stateless
>public class ItemEJB {
>	@PersistenceContext(unitName = "chapter08PU")
>	private EntityManager em;
>	
>	@Resource
>	private SessionContext ctx;
>	
>	public void deleteBook(Book book) {
>		if (!ctx.isCallerInRole("admin"))
>			throw new SecurityException("Only admins are allowed");
>		em.remove(em.merge(book));
>	}
>	
>	public Book createBook(Book book) {
>		if (ctx.isCallerInRole("employee") && !ctx.isCallerInRole("admin")) {
>			book.setCreatedBy("employee only");
>		} else if (ctx.getCallerPrincipal().getName().equals("paul")) {
>			book.setCreatedBy("special user");
>		}
>		em.persist(book);
>		return book;
>	}
>}
>```
>
>Prima di tutto, il bean deve ottenere un riferimento al suo contesto (utilizzando l'annotazione `@Resource`). 
>
>Con questo contesto, il metodo `deleteBook()` può verificare se il chiamante ha o meno un ruolo di amministratore. In caso contrario, viene lanciata una `java.lang.SecurityException` per notificare all'utente la violazione dell'autorizzazione. Il metodo `createBook()` esegue alcune operazioni logiche utilizzando i ruoli e il Principal. 
>
>Si noti che il metodo `getCallerPrincipal()` restituisce un oggetto `Principal`, che ha un nome. Il metodo controlla se il nome del Principal è `"paul"` e quindi imposta il valore `"special user"` all'entità book.

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