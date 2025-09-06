---
aliases:
  - JTA
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Enterprise Java Bean
cssclasses:
  - 
---
Affinché le transazioni funzionino e rispettino le proprietà [[ACID]], devono essere presenti diversi componenti.
## Transazioni locali con JTA
>In un'applicazione in cui vengono apportate varie modifiche a una sola risorsa (come un database) è sufficiente utilizzare una **Transazione Locale JTA** (*Java Transaction API*). Questo tipo di transazione *coinvolge una singola risorsa* e impiega la sua [[006 API a Chiamate di Sistema|API]] specifica quando si tratta di transazioni locali con una sola risorsa transazionale.

![[Pasted image 20231117101015.png|200]]

In questo schema si ha:
- **Risorsa**: l'*archivio persistente* da cui si legge o si scrive (un database, una destinazione di messaggi, ecc.);
- **Gestore delle Risorse**: è responsabile della gestione delle risorse e *della loro registrazione con il gestore delle transazioni*. Un esempio di gestore di risorse è un driver per un database relazionale ([[026 JDBC|JDBC]]) o una risorsa JMS.
- **Transaction Manager**: il componente centrale *responsabile della gestione delle operazioni transazionali*. Crea le transazioni per conto dell'applicazione, informa il gestore delle risorse che sta partecipando a una transazione (un'operazione nota come *enlistment*) ed *esegue il commit o il rollback* sul gestore delle risorse.

Non è responsabilità dell'applicazione preservare le proprietà ACID. L'applicazione decide semplicemente di eseguire il commit o il rollback della transazione e il gestore delle transazioni prepara tutte le risorse per far sì che la transazione avvenga con successo.

## Transazioni Distribuite con XA e JTS
Molte applicazioni aziendali utilizzano più di una risorsa (come database separati). È quindi necessario *gestire le transazioni su più risorse* o su *risorse distribuite in rete*. Queste transazioni a livello aziendale richiedono un coordinamento speciale che coinvolge **XA** (*eXtended Architecture*) e **JTS** (*Java Transaction Service*).

### Transazioni su più risorse (XA)
>Utilizzano la **demarcazione delle transazioni** tra diverse risorse. Ciò significa che, *nella stessa unità di lavoro, l'applicazione può persistere i dati su più database*, per esempio.

![[Pasted image 20231117102249.png|300]]

Per avere una transazione affidabile su più risorse, il gestore delle transazioni deve utilizzare un'*interfaccia di gestione delle risorse XA* (supportato da JTA) per preservare le proprietà ACID. Ciò consente ai gestori di risorse eterogenei di diversi fornitori di [[Interoperabilità|interoperare]] attraverso un'interfaccia comune. 

XA utilizza un **commit a due fasi** (2pc) per garantire che *tutte le risorse effettuino il commit o il rollback* di una particolare transazione *simultaneamente*.

> [!example]- <font color="orange">Esempio</font>
>In un trasferimento di fondi, supponiamo che il conto di risparmio venga addebitato su un primo database e che la transazione venga eseguita con successo. Poi il conto corrente viene accreditato su un secondo database, ma la transazione fallisce. Dovremmo tornare al primo database e annullare le modifiche impegnate. Per evitare questo problema si può usare il commit a due fasi.


Il commit a due fasi esegue un'ulteriore *fase preparatoria prima del commit finale*. 

- Durante la **fase 1**, ogni gestore di risorse *viene informato* tramite un comando "prepare" che sta per essere emesso un commit. Questo permette ai gestori delle risorse di *dichiarare se possono applicare o meno le loro modifiche*. 

- Se *tutti* i resource manager *rispondono di essere pronti*, la transazione è autorizzata a procedere e a tutti i gestori di risorse viene chiesto di eseguire il commit nella **fase 2**.

![[Pasted image 20231117103232.png|500]]

### Transazioni su reti (JTS)
>Nella maggior parte dei casi, le risorse sono *distribuite in rete*. Un sistema di questo tipo si basa su **JTS**, che implementa le specifiche *Object Transaction Service* (OTS), *Object Management Group* (OMG) consentendo ai gestori di transazioni di partecipare a transazioni distribuite attraverso la rete con *Internet Inter-ORB Protocol* (IIOP). 

Rispetto a quello visto precedentemente, in cui esisteva un solo gestore di transazioni, questo sistema permette di *distribuire il gestore di transazioni tra diversi computer* e quindi gestire transazioni tra diversi database di diversi fornitori. 

![[Pasted image 20231117104005.png|400]]

> [!note] Per usare questo servizio, **basta usare JTA**, che si interfaccia con JTS a un livello superiore

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