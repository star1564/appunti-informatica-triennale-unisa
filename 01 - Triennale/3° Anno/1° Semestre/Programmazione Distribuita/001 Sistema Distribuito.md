---
aliases:
  - Sistema Distribuito
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Introduzione
cssclasses:
  - 
---
>Un **Sistema Distribuito** consiste di un insieme di macchine, *ognuna indipendente e gestita in maniera autonoma*, connesse attraverso una [[001 Introduzione a RC|Rete]].

Quindi, un sistema distribuito è costituito da un insieme di applicazioni logicamente indipendenti che collaborano per il perseguimento di obiettivi comuni attraverso una infrastruttura di comunicazione hardware e software.

![[Pasted image 20230929160230.png]]

Ogni nodo del sistema distribuito comunica e coordina il proprio lavoro attraverso uno strato [[001 Introduzione a IS|software]] detto **[[005 Middleware|Middleware]]**, in maniera che l'utente percepisca il sistema come *un unica entità integrata*.

> [!question]- Differenza tra una rete di calcolatori e un sistema distribuito
> In una rete di calcolatori gli utenti vedono le macchine effettive e il sistema non compie alcun tentativo di far apparire e agire le macchine in un modo coerente dato un input. Questo accade invece in un sistema distribuito.

## Caratteristiche di un Sistema Distribuito
Un sistema distribuito ha le seguenti caratteristiche:
- **Remoto**: i componenti del sistema sono potenzialmente localizzate su macchine diverse, sia locali che remote;
- **Concorrenza**: un sistema distribuito è per sua stessa natura [[Concorrenza|concorrente]], ma non esistono strumenti come lock e [[028 Semafori|semafori]];
- **Assenza di uno stato globale**: non esiste un modo per poter determinare lo stato globale del sistema, in quanto la distanza e la eterogeneità del sistema non permette di definire con certezza lo stato in cui si trova ciascun nodo;
- **Malfunzionamenti Parziali**: ogni componente di un sistema distribuito può smettere di funzionare ma questo fallimento non deve intralciare la funzionalità di macchine che si trovano altrove, permettendo l'utilizzo (anche parziale) dell'applicazione;
- **Eterogeneità**: un sistema distribuito è eterogeneo per tecnologia hardware e software;
- ***Autonomia***: un sistema distribuito non ha un singolo punto dal quale può essere controllato, coordinato e gestito, quindi la collaborazione va ottenuta mediando le richieste del sistema distribuito con quelle del sistema che gestisce ciascun nodo;
- ***Evoluzione***: i sistemi distribuiti possono cambiare in maniera sostanziale durante la sua vita:
	- La *flessibilità* di un sistema distribuito deve assicurare che la migrazione verso ambienti diversi (con tecnologie e applicazioni diverse) può essere assecondata con successo;
	- Questo accade attraverso standard aperti, architettura modulare, scalabilità, **mobilità** dei nodi, servizi e delle risorse, ecc.

## I requisiti non funzionali di un Sistema Distribuito
Questi aspetti specifici hanno a che fare con la *qualità del sistema* riconosciuti in modo globale ma non identificabili in una specifica parte del sistema, che hanno un impatto significativo.

Questi requisiti non funzionali specificano che la progettazione deve puntare a realizzare sistemi distribuiti che:
- siano **aperti**, in modo da supportare la *portabilità di esecuzione* e di *[[Interoperabilità]]* attraverso interfacce e servizi ben documentati;
- siano **integrati**, in modo da poter *incorporare sistemi* e risorse *differenti* senza la necessità di utilizzare strumenti specifici;
- siano **modulari**, consentendo a ciascun componente di operare *autonomamente* con un grado definito di interdipendenza verso il resto del sistema (deve essere collegato agli altri componenti del sistema);
- supportino la **federazione** dei sistemi, in modo da *unire diversi sistemi* sia dal punto architetturale che amministrativo;
- siano **facilmente gestibili**;
- forniscano supporto per la **qualità del servizio**, per poter fornire i servizi con disponibilità e affidabilità, con una buona *tolleranza ai malfunzionamenti*;
- siano **scalabili**, per aumentare notevolmente la platea di utenti che accedono ai servizi forniti dal sistema e *gestire carichi di lavoro crescenti* senza dover cambiare la sua struttura o organizzazione;
- siano **sicuri**;
- offrano **[[003 Trasparenza di un Sistema Distribuito|trasparenza]]**, mascherando i dettagli e le differenze della architettura sottostante.



## Motivazioni di un Sistema distribuito
In generale, i sistemi distribuiti rispondono a motivazioni sia di *tipo economico* che di *natura tecnologica*.

### Motivazioni Economiche
Per quanto riguarda il contesto **economico**, i sistemi distribuiti rispondono in maniera completa alle esigenze del mercato, caratterizzato da numerose e frequenti *acquisizioni, integrazioni e fusioni* di aziende. Quindi c'è il bisogno di affrontare in tempi brevi l'integrazione di sistemi di diverse aziende.

Permettono la separazione del sistema informativo in aziende in caso di *downsizing*, ovvero quando aziende vengono separate dalla "casa madre".

Rispondono al *time to market*, ovvero il tempo necessario per un prodotto di passare dallo sviluppo al mercato nel più breve tempo possibile.

Infine, *la diffusione di [[001 Introduzione a RC#^399a10|Internet]]* implica che qualsiasi servizio è potenzialmente accessibile da una platea smisurata di utenti, provocando picchi di carico non previsti che potrebbero buttare a terra un sistema semplice.

### Motivazioni Tecnologiche
Lo sviluppo **tecnologico** dell'informatica è sempre stato condotto dal *rapidissimo sviluppo delle tecnologie hardware*. 

Questo sviluppo continuo ha condotto alla creazione di tecniche e metodi per sistemi software complessi, in grado di *poter utilizzare al meglio queste componenti* di sempre maggiori prestazioni.


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