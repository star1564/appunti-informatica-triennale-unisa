---
aliases:
  - Legge di Moore
  - Legge di Sarnoff
  - Legge di Metcalfe
  - Legge di Reed
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Introduzione
cssclasses:
  - 
---
Diverse "leggi" empiriche elaborate negli anni si sono provate fedeli nel prevedere la *velocità di evoluzione dell'hardware e delle reti*.

> [!note]+ "Leggi"
> Invece di essere vere e proprie leggi, si trattano piuttosto di *principi o tendenze* osservate nell'ambito della tecnologia dell'informazione e delle telecomunicazioni. Quindi possono essere soggette a modifiche nel tempo.

## Legge di Moore
> La **Legge di Moore** afferma che *il numero di transistor su un microprocessore*, e di conseguenza la potenza di elaborazione, *raddoppierà approssimativamente ogni due anni*, mentre il costo per transistor diminuirà.

## Leggi che regolano le Reti
Lo sviluppo di una rete dipende dalla utilità che questa porta ad una platea di utenti. Quanto più ampiamente utilizzabili sono i servizi che si possono immaginare, tanto maggiore è il loro valore commerciale.

Le seguenti "leggi" definiscono *il valore di una rete* con riferimento al numero di connessioni.

### Legge di Sarnoff
> La **Legge di Sarnoff** afferma che il valore o utilità di una rete di [[002 Rappresentazione di Reti#Broadcast|broadcast]] è *direttamente proporzionale al numero di utenti*. $$V = a \cdot N$$

Dove:
- $V$ rappresenta il valore economico o finanziario dell'industria della radiodiffusione;
- $N$ rappresenta il numero di canali o stazioni radiofoniche disponibili;
- $a$ è un coefficiente moltiplicativo che rappresenta il valore medio generato da ciascun canale o stazione.

![[Pasted image 20230926134949.png]]

Quindi *più persone sono collegate, maggiore è il valore della rete*.

Non incoraggia necessariamente l'integrazione delle reti. Invece, questa legge si basa sul concetto che un numero limitato di canali possa avere un alto valore economico, a condizione che la programmazione o i contenuti siano attraenti per il pubblico. 

In tal caso, non è necessario un grande numero di canali per avere successo. Tuttavia, la tendenza a integrare servizi tra diverse stazioni o reti può comunque verificarsi per ottimizzare le risorse o per offrire un'offerta di contenuti più ampia.

### Legge di Metcalfe
> La **Legge di Metcalfe** afferma che il valore di una rete cresce *proporzionalmente al quadrato del numero di nodi* o dispositivi collegati a quella rete. $$V=a\cdot N+b\cdot N^2$$

Quindi, se il numero di nodi raddoppia, il valore della rete quadruplica. 

![[Pasted image 20230926135000.png]]

Questo principio si basa sull'idea che ogni nodo aggiunto a una rete *può comunicare con ogni altro nodo*, creando così un aumento esponenziale delle connessioni e delle opportunità di comunicazione.

L'integrazione delle reti consente di aumentare il numero di partecipanti o utenti complessivi. Quando le reti si fondono o si integrano, il loro numero di utenti combinato è spesso maggiore della somma delle loro basi di utenti individuali. Ciò può portare a una crescita significativa del valore complessivo delle reti fuse.

### Legge di Reed
>La **Legge di Reed**, in riferimento alle *reti sociali*, afferma che il valore di una rete sociale è direttamente proporzionale ad una funzione esponenziale in $N$. $$V=a\cdot N + b\cdot N^2 + c\cdot 2^N$$

![[Pasted image 20230926135011.png]]

Il valore di una rete cresce in modo esponenziale se associato a gruppi con interessi comuni.

L'effetto di rete positivo è evidente quando le reti si fondono o si integrano, in quanto l'aumento del numero di partecipanti conduce a un incremento delle opportunità di connessione e interazione tra utenti.


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