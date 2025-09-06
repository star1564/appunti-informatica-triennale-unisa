---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Enterprise Java Bean
cssclasses:
  - 
---
Il [[021 Container Java EE|container]] è responsabile della gestione del ciclo di vita dell'[[041 Enterprise Java Beans|EJB]]. 

Tutti i [[041 Enterprise Java Beans#^a4706e|Session Bean]] hanno due fasi ovvie nel loro ciclo di vita: la *creazione e la distruzione*. Inoltre, gli [[042 Tipi di Session Beans|Stateful Beans]] passano attraverso le fasi di passivazione e attivazione. 

Analogamente ai [[040 Ciclo di Vita di una Entità|metodi di callback utilizzati nelle entità]], gli EJB consentono al contenitore, durante alcune fasi della loro vita, di invocare automaticamente metodi annotati (`@PostConstruct`, `@PreDestroy`, ecc.). Questi metodi possono inizializzare le informazioni sullo stato del bean, cercare risorse tramite [[025 Risorse e JNDI|JNDI]] o rilasciare connessioni al database.

## Stateless e Singleton
Gli [[042 Tipi di Session Beans|Stateless Beans]] e i [[042 Tipi di Session Beans|Singleton Beans]] hanno in comune il fatto che non mantengono uno stato conversazionale con il client. Quindi il loro ciclo di vita è il seguente:

1. Il ciclo di vita di un session bean stateless o singleton *inizia quando un client richiede un riferimento al bean* (utilizzando la [[043 Come usare un EJB|dependency injection o il lookup JNDI]]). Il contenitore **crea una nuova istanza** del bean di sessione.
	- Nel caso di un singleton, può anche iniziare quando il contenitore viene avviato (usando l'annotazione `@Startup`). 
2. Se l'istanza appena creata utilizza l'iniezione di dipendenze tramite annotazioni (`@Inject`, `@Resource`, `@EJB`, `@PersistenceContext`, ecc.) o [[022 Deployment Descriptor|Deployment Descriptor]], *il contenitore inietta tutte le risorse necessarie*.
3. Se l'istanza ha un metodo annotato con `@PostContruct`, *il contenitore lo invoca*.
4. L'istanza del bean *elabora la chiamata invocata dal client* e rimane in modalità **Ready** per *elaborare le chiamate future*. 
	- I bean stateless rimangono in modalità ready finché il contenitore non libera spazio nel pool. 
	- I singleton rimangono in modalità ready finché il contenitore non viene chiuso.
5. Il contenitore non ha più bisogno dell'istanza. Pertanto *invoca il metodo annotato con `@PreDestroy`*, se presente, e **distrugge l'istanza** del bean.

![[Pasted image 20231115163909.png|500]]

## Stateful
Gli [[042 Tipi di Session Beans#Stateful Beans|Stateful Beans]] conservano lo stato conversazionale con il client, quindi hanno un ciclo di vita leggermente diverso:

1. Il ciclo di vita di un session bean stateful *inizia quando un client richiede un riferimento al bean* (utilizzando la dependency injection o il lookup JNDI). Il contenitore **crea una nuova istanza** del bean di sessione e **lo conserva in memoria**.
2. Se l'istanza appena creata utilizza l'iniezione di dipendenze tramite annotazioni (`@Inject`, `@Resource`, `@EJB`, `@PersistenceContext`, ecc.) o Deployment Descriptor, *il contenitore inietta tutte le risorse necessarie*.
3. Se l'istanza ha un metodo annotato con `@PostContruct`, *il contenitore lo invoca*.
4. Il bean *esegue la chiamata richiesta* e **rimane in memoria principale**, in attesa per successive chiamate di richiesta dal client.
5. Se il *client rimane inattivo* per un certo periodo di tempo, il contenitore invoca il metodo annotato con `@PrePassivate`, se presente, e **rende passiva l'istanza del bean** nella memoria permanente.
6. Se il client *invoca un bean passivo*, il contenitore *lo riattiva in memoria principale* e invoca il metodo annotato con `@PostActivate`, se presente.
7. Se il client non invoca un'istanza di bean passivo per il *periodo di timeout della sessione*, il contenitore **la distrugge**.
8. Alternativamente, se il client chiama un metodo annotato da `@Remove`, il container invoca il metodo annotato con `@PreDestroy`, se presente, e **distrugge l'istanza del bean**.


![[Pasted image 20231115183907.png|600]]

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