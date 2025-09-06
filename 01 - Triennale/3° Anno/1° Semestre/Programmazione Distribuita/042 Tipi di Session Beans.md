---
aliases:
  - Session Bean
  - Stateless Beans
  - Stateful Beans
  - Singleton Beans
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Enterprise Java Bean
cssclasses:
  - 
---
## Stateless Beans
>Gli **Stateless Bean** sono i componenti [[041 Enterprise Java Beans|session bean]] più diffusi. Questi bean _non mantengono uno stato conversazionale_ con il client tra le chiamate di metodo ed ogni istanza può essere usata da ogni client.

Sono semplici, potenti ed efficienti e rispondono al compito comune di eseguire elaborazioni aziendali stateless. 

> [!question] Cosa significa stateless?
> Significa che un'attività non ha memoria di precedenti interazioni, quindi, essa deve essere completata con un'unica chiamata di metodo.

^68ea9a

Per ogni Stateless Bean, il container *mantiene un certo numero di istanze in memoria* (cioè un **pool**) e le condivide tra i client. Poiché i bean stateless non mantengono lo stato del client, tutte le istanze sono equivalenti. 

Quando un client invoca un metodo su un bean stateless, il contenitore *preleva un'istanza dal pool e la assegna al client*. Quando la richiesta del client termina, l'istanza *ritorna al pool* per essere riutilizzata. 

![[Pasted image 20231115153143.png|500]]

Il contenitore non garantisce lo stesso numero di istanze del bean stateless e non garantisce la stessa istanza per lo stesso cliente.


> [!example]- <font color="orange">Esempio</font>
>```Java
>@Stateless
>public class ItemEJB {
>	@PersistenceContext(unitName = "chapter07PU")
>	private EntityManager em;
>
>	// Metodo che esegue una query named sui libri (JPA)
>	public List<Book‎ > findBooks() {
>		TypedQuery<Book‎ > query = em.createNamedQuery(Book.FIND_ALL, Book.class);
>		return query.getResultList();
>	}
>
>	// Metodo che esegue una query named sui CD (JPA)
>	public List<CD‎ > findCDs() {
>		TypedQuery<CD‎ > query = em.createNamedQuery(CD.FIND_ALL, CD.class);
>		return query.getResultList();
>	}
>
>	// Crea un libro
>	public Book createBook(Book book) {
>		em.persist(book);
>		return book;
>	}
>
>	// Crea un CD
>	public CD createCD(CD cd) {
>		em.persist(cd);
>		return cd;
>	}
>}
>```

## Stateful Beans
>Gli **Stateful Beans** sono session beans che *mantengono uno stato conversazionale* con il client tra le diverse chiamate di metodo. Sono utili per compiti che devono essere eseguiti in più fasi, ognuna delle quali si basa sullo stato mantenuto in una fase precedente.

> [!question] Cosa significa stateful?
> Significa che un'attività ha memoria di precedenti interazioni.

Quando un client invoca un bean stateful nel server, il contenitore EJB *deve fornire la stessa istanza per ogni successiva invocazione di metodo*. I bean stateful non possono essere riutilizzati da altri client. 

![[Pasted image 20231115154610.png|500]]

Si ha quindi una *correlazione uno-a-uno tra un'istanza di bean e un client*. Per quanto riguarda lo sviluppatore, non è necessario alcun codice aggiuntivo, poiché il contenitore EJB gestisce automaticamente questa correlazione uno-a-uno.

> [!info]- Alto numero di istanze
>La correlazione uno-a-uno ha un prezzo perché, come si può intuire, se si hanno un milione di client, si avranno un milione di stateful bean in memoria. Per evitare un ingombro di memoria così grande, il contenitore *cancella temporaneamente i bean stateful dalla memoria prima che la richiesta successiva del client li riporti indietro*. Questa tecnica è chiamata **passivazione e attivazione**, funziona similarmente alla [[003 Trasparenza di un Sistema Distribuito#Trasparenza alla Persistenza|Trasparenza alla Persistenza]].

> [!example]- <font color="orange">Esempio</font>
>```Java
>@Stateful
>@StatefulTimeout(value = 20, unit = TimeUnit.SECONDS)
>public class ShoppingCartEJB {
>	private List<Item‎ > cartItems = new ArrayList<>();
>
>	// Aggiungi un oggetto al carrello
>	public void addItem(Item item) {
>		if (!cartItems.contains(item))
>		cartItems.add(item);
>	}
>	
>	// Altri metodi di business...
>
>	@Remove
>	public void checkout() {
>		//...
>		cartItems.clear();
>	}
>}
>```
>
>Si utilizza l'annotazione `javax.ejb.StatefulTimeout` per indicare il tempo consentito per rimanere in idle (con unità di tempo).
L'annotazione `javax.ejb.Remove` permette di rimuovere permanentemente l'istanza dalla memoria dopo l'invocazione del metodo che decora (`checkout()`).

## Singleton Beans
>I **Singleton Bean** sono session beans che vengono *istanziati una sola volta per applicazione* e che vengono *condivisi tra vari client*. Un singleton garantisce l'esistenza di una sola istanza di una classe nell'intera applicazione e fornisce un punto globale per accedervi. 

![[Pasted image 20231115160652.png|500]]

Un esempio pratico è quello di una [[070 Gerarchia di Memoria#MEMORIA CACHE|cache]] condivisa tra diversi client, implementata attraverso una `Hashmap`.

Innanzitutto, è necessario impedire la creazione di una nuova istanza con un costruttore privato. Poi bisogna istanziare l'unico singleton bean insieme alla `Hashmap`.

> [!example]- <font color="orange">Esempio senza EJB</font>
>Questa classe NON UTILIZZA GLI EJB.
>```Java
>public class CacheNormale {
>	private static Cache instance = new Cache(); // Unica istanza
>	private Map<Long, Object> cache = new HashMap<>();
>
>	// Costruttore privato vuoto.
>	private Cache() {}
>
>	// Metodo per ottenere l'istanza.
>	public static synchronized Cache getInstance() {
>		return instance;
>	}
>
>	public void addToCache(Long id, Object object) {
>		if (!cache.containsKey(id))
>		cache.put(id, object);
>	}
>
>	public void removeFromCache(Long id) {
>		if (cache.containsKey(id))
>		cache.remove(id);
>	}
>	
>	public Object getFromCache(Long id) {
>		if (cache.containsKey(id))
>			return cache.get(id);
>		else
>			return null;
>	}
>}
>```
>In questo modo, un client, per ottenere l'istanza `CacheNormale` deve scrivere 
>```Java
>CacheNormale.getInstance().addToCache(myObject);
>```


> [!example]- <font color="orange">Esempio con EJB</font>
>```Java
>@Singleton
>public class CacheEJB {
>	private Map<Long, Object> cache = new HashMap<>();
>	
>	public void addToCache(Long id, Object object) {
>		if (!cache.containsKey(id))
>		cache.put(id, object);
>	}
>	
>	public void removeFromCache(Long id) {
>		if (cache.containsKey(id))
>			cache.remove(id);
>	}
>
>	public Object getFromCache(Long id) {
>		if (cache.containsKey(id))
>			return cache.get(id);
>		else
>			return null;
>	}
>}
>```
>Il client deve solamente chiamare i metodi per gestire la cache.


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