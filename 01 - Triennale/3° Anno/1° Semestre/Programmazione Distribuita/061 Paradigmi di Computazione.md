---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Cloud Computing
cssclasses: 
---
## Tipologie di Calcolo
Il panorama delle tecnologie di calcolo si articola in diverse modalità, ognuna con caratteristiche specifiche:
- **Calcolo Monolitico**: caratterizzato da un approccio centralizzato.
- **Calcolo Concorrente**:
	- *Calcolo Parallelo*: operativo in modalità strettamente accoppiata.
	- *Calcolo Distribuito*: caratterizzato da un accoppiamento più debole, con nodi autonomi.
	- *Calcolo su Cloud*: basato sul concetto di "utility/service computing".

Il paradigma di computazione ha subito notevoli cambiamenti, passando dal calcolo monolitico al calcolo distribuito, con una transizione da high-performance computing a high-throughput computing.

![[Pasted image 20231203092529.png|550]]

![[053 Web Service#Service Oriented Architecture|Service Oriented Architecture]]

### High Performance Computing (HPC)
L'**High Performance Computing** (*alta performance*) è orientato alle esigenze scientifiche, come fisica, chimica e manufacturing, con una particolare attenzione alle operazioni in [[011 Virgola Mobile|floating point]].

### High Throughput Computing (HTC)
L'**High Throughput Computing** (*alto numero di compiti completati in un'unità di tempo*) si concentra sulla gestione di grandi quantità di dati, con applicazioni prevalentemente orientate alla ricerca su Internet e ai servizi web. 

## Modelli per il Calcolo Distribuito e su Cloud
### Utility Computing
Il modello di business di **utility computing**, presente sia nel cloud che in altri paradigmi, introduce il concetto di *fornitura di capacità computazionale come un servizio a pagamento*. Il cliente riceve risorse computazionali in cambio di un canone.

![[Pasted image 20231223105543.png|500]]

### Cluster di Computer
I **Cluster** di computer rappresentano una rete di nodi di calcolo interconnessi a livello SAN, [[LAN]] o [[WAN]]. 

Principali caratteristiche:
- Nodi omogenei con controllo distribuito, spesso con sistema operativo UNIX o Linux;
- Utilizzati in applicazioni ad alte prestazioni come calcolo scientifico, motori di ricerca e servizi web.

![[Pasted image 20231223110929.png|700]]

### Reti Peer-to-Peer (P2P)
![[Modello Peer-to-Peer]]

### Grids di Dati o di Calcolo
Cluster eterogenei uniti da collegamenti di rete ad alta velocità su siti di risorse selezionati. Caratteristiche chiave:
- *Controllo centralizzato*, orientato al server, con sicurezza autenticata;
- Utilizzati per il supercalcolo distribuito, risoluzione globale di problemi e servizi di data center.

![[Pasted image 20231223111222.png|600]]

### Piattaforme Cloud
Le piattaforme **Cloud** rappresentano *cluster virtualizzati di server* attraverso accordi di livello di servizio, ovvero **Service Level Agreement** (*SLA*), che dettano il canone da pagare per la fornitura dei servizi. 

Caratteristiche principali:
- Fornitura *dinamica* di risorse, inclusi server, storage e reti;
- Utilizzate per migliorare la ricerca web, il calcolo di utilità e i servizi di outsourcing.

**Caratteristiche delle Piattaforme Cloud**:
- Calcolo ridondante;
- Auto-recupero;
- Modelli di calcolo altamente *scalabili*;
- Tolleranza ai malfunzionamenti.

![[Pasted image 20231223111326.png|600]]

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