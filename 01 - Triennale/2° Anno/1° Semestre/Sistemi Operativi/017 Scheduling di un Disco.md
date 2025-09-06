---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Processi
cssclasses:
  - 
---
Una delle responsabilità del SO è quella di fare un uso efficiente dell'hardware.

Al driver del [[016 Memoria di Massa|disco rigido]] deve gestire, ad un certo istante, una serie di richieste di blocchi dati da parte di [[Processo (Job)|processi]].

Si potrebbe considerare di "servire il primo che richiede", ma non è detto che questo ragionamento sia il migliore per tutti i casi.

Analizziamo pertanto il **Tempo di Posizionamento**, composto da:
- *Tempo di Ricerca* (Seek Time): il tempo che impiega il braccio del disco a muovere le testine fino al cilindro contenente il settore desiderato.
- *Latenza di Rotazione*: è il tempo aggiuntivo speso in attesa che il disco faccia ruotare il settore desiderato sotto la testina.

Si consideri, per esempio, una coda di richieste per l'unità a disco nel seguente ordine (*con la posizione di partenza $53$)*: $\color{#61AFEF}98, 183, 37, 122, 14, 124, 65, 67$

Esistono principalmente cinque algoritmi di scheduling di un disco. 

#### FCFS (FIRST COME FIRST SERVED)
L'idea è quella di **servire le richieste nell'ordine in cui queste sono arrivate**.
Quindi la testina del disco parte dalla traccia 53, per poi andare alla 98, poi alla 183, poi alla 37, ecc.

Questo algoritmo *non è molto economico* in termini di tempo.

![[Pasted image 20221011184145.png|500]]
<center>Movimento totale della testina: 640 cilindri</center>

#### SSTF (SHORTEST SEEK TIME FIRST)
Seleziona **la richiesta con la traccia più vicina alla posizione attuale**. Quindi partendo da 53, la posizione più vicina è la 65; mi sposto a 65 e vedo la richiesta più vicina a quel punto, che è la 67; mi sposto li, ecc.

Questo algoritmo *potrebbe mettere in un'attesa indefinita certe richieste* (quelle più lontane).

![[Pasted image 20221011184648.png|500]]
<center>Movimento totale della testina: 263 cilindri</center>

#### SCAN
Per garantire che tutte le richieste possano sempre essere servite, si muove il braccio del disco **da un estremo all'altro** (di default da sinistra verso destra).

Partendo dalla 53 si muove verso lo 0. Nel tragitto verso lo 0 vengono servite tutte le varie richieste incontrate (37-14). Arrivato allo 0 verranno servite tutte le altre richieste in ordine crescente.

Il difetto principale di questo algoritmo è che *è obbligatorio andare agli estremi delle tracce*, sprecando tempo.

![[Pasted image 20221011190147.png|500]]
<center>Movimento totale della testina: 208 cilindri</center>

#### C-SCAN
Il braccio si muove da un capo all'altro del disco (default verso destra), servendo le richieste lungo il percorso. Quando raggiunge l'altro capo ritorna direttamente all'inizio del disco, senza servire alcuna richiesta durante il ritorno.

Anche qui si ha lo stesso problema della scan.

![[Pasted image 20221011190502.png|500]]
<center>Movimento totale della testina: 383 cilindri</center>

#### C-LOOK
Risolve i problemi della scan/c-scan. Il braccio arriva fin dove è presente la richiesta finale, per ciascuna delle due direzioni.
Appena arrivato alla richiesta con la traccia più grande, passa immediatamente a quella più piccola.

Va solamente alla prossima richiesta calcolata, non prende altre lungo la via.

![[Pasted image 20221011190840.png|500]]
<center>Movimento totale della testina: 322 cilindri</center>


___
[[000 Indice SO|↖ Ritorna all'indice ↖]]

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
- [[Es.SO.6 Scheduling disco con tempo]]