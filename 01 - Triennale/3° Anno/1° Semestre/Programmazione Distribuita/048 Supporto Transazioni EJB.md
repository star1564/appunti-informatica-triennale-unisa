---
aliases: CMT, BMT, Container-Managed Transaction, Bean-Managed Transaction
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Enterprise Java Bean
cssclasses:
  - 
---
Grazie all'astrazione fornita da [[047 Transazioni locali e distribuite|JTA]], è possibile sviluppare un'applicazione transazionale con gli [[041 Enterprise Java Beans|EJB]] in modo molto semplice, lasciando al [[021 Container Java EE|container]] il compito di implementare i protocolli di transazione di basso livello, come il commit a due fasi o la propagazione del contesto della transazione.

Ci sono due tipi di supporti alle transizioni: *Container-Managed Transaction* o *Bean-Managed Transaction*.

### Container-Managed Transaction
>*Le transazioni sono naturali per gli EJB* e per impostazione predefinita, **ogni metodo viene automaticamente avvolto in una transazione**. Questo comportamento predefinito è noto come **Container-Managed Transaction** (*CMT*), perché le transazioni sono gestite dal contenitore EJB. Infatti, non è necessario usare JTA.


Per via del [[035 Object Relational Mapping|Configuration by Exception]], non c'è bisogno di inserire annotazioni o di implementare delle interfacce, visto che è il comportamento predefinito.

> [!example]- <font color="orange">Esempio</font>
>```Java
>@Stateless
>public class ItemEJB {
>	@PersistenceContext(unitName = "chapter09PU")
>	private EntityManager em;
>	
>	@Inject
>	private InventoryEJB inventory;
>	
>	public List<Book‎ > findBooks() {
>		TypedQuery<Book‎ > query = em.createNamedQuery(FIND_ALL, Book.class);
>		return query.getResultList();
>	}
>	
>	public Book createBook(Book book) {
>		em.persist(book);
>		inventory.addItem(book);
>		return book;
>	}
>}
>```

![[Pasted image 20231117105558.png|500]]

Dopo che il client invoca un metodo, questa chiamata è *intercettata dal container*. Quest'ultimo controlla immediatamente, prima di invocare il metodo, se un *contesto di transazione* è associato alla chiamata. 
Per impostazione predefinita, se non è disponibile un contesto di transazione, il contenitore *inizia una nuova transazione* prima di entrare nel metodo e quindi invoca il metodo chiamato. 
Una volta che il metodo esce, il contenitore esegue automaticamente il commit della transazione o la annulla.

### Bean-Managed Transaction
In alcuni casi, CMT potrebbe non fornire la giusta soluzione richiesta (ad esempio, *un metodo non può generare più di una transazione*). 

>Per risolvere questo problema, gli EJB offrono un modo programmatico per gestire le transazioni con le **Bean-Managed Transaction** (*BMT*). BMT consente di gestire *esplicitamente i confini delle transazioni* (inizio, commit, rollback) *utilizzando JTA*.

Per disattivare la modalità CMT predefinita e passare alla modalità BMT, un bean deve semplicemente utilizzare l'annotazione `javax.ejb.TransactionManagement`.

```Java
@Stateless
@TransactionManagement(TransactionManagementType.BEAN)
public class ItemEJB {...}
```

Con la demarcazione BMT, l'applicazione richiede la transazione e il contenitore EJB crea la transazione fisica e si occupa di alcuni dettagli di basso livello. Inoltre, non propaga le transazioni da un BMT all'altro. 

L'interfaccia principale utilizzata per eseguire il BMT è `javax.transaction.UserTransaction`. Permette al bean di delimitare una transazione, ottenere il suo stato, impostare un timeout e così via. La `UserTransaction` viene istanziata dal contenitore EJB e resa disponibile tramite [[@Inject|dependency injection]], ricerca [[025 Risorse e JNDI|JNDI]].

> [!example]- <font color="orange">Esempio</font>
>```Java
>@Stateless
>@TransactionManagement(TransactionManagementType.BEAN) // Disabilita CMT
>public class InventoryEJB {	
>	@PersistenceContext(unitName = "chapter09PU")
>	private EntityManager em;
>	
>	@Resource
>	private UserTransaction ut;
>	
>	public void oneItemSold(Item item) {
>		try {
>			ut.begin(); // Comincia la transazione
>			
>			item.decreaseAvailableStock();
>			sendShippingMessage();
>			
>			if (inventoryLevel(item) == 0)
>				ut.rollback(); // Fai il rollback in caso di errore
>			else
>				ut.commit(); // Fai il commit per confermare
>		} catch (Exception e) {
>			ut.rollback(); // Fai il rollback in caso di errore
>		}
>		sendInventoryAlert();
>	}
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