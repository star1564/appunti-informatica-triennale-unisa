---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: MIPS - Processore
cssclasses:
  - 
---
È una tecnica di implementazione hardware di un set di istruzioni, utilizzata da quasi tutti gli elaboratori.

>Prevede la *sovrapposizione temporale* dell'esecuzione delle varie istruzioni per velocizzare l'intera esecuzione dell'istruzione.

^150a30

Infatti **aumenta il throughput** (numero di istruzioni eseguite nell'unità di tempo) e **non diminuisce la latenza** (tempo di esecuzione della singola istruzione). ^806fdc

## TEMPISTICHE ISTRUZIONI
Supponiamo che i tempi richiesti per le varie fasi sono rappresentati in tabella.

![[Pasted image 20211213094628.png]]

Nel pipeline ciascuno dei 5 stadi impiega un [[057 Clock#CLOCK|ciclo di Clock]], quindi la durata del ciclo di clock sarà uguale alla durata dello stadio più lento (200 ps), anche se alcuni stadi impiegano meno tempo.

Consideriamo tre istruzioni `lw` (le più lente) in sequenza:
![[Immagine 2021-12-13 095143 1.png|800]]

È importante notare che le operazioni di scrittra (da 100 ps) *avvengono sempre nella prima parte* del ciclo di clock; mentre quelle di lettura (sempre da 100 ps) nella *seconda parte*.

## PROGETTAZIONE

Nell'approccio basato su pipeline sono usati i [[056 Data Path del Processore MIPS#^f11366|cinque stadi]].

Nel seguente disegno si considera una versione del [[064 MIPS a Ciclo Singolo|datapath a ciclo singolo]] semplificata, dove abbiamo spostato il multiplexer PCSrc allo stadio IF.
![[Pasted image 20211213100635.png|800]]

Per consentire la condivisione delle varie unità funzionali tra i vari stadi della pipeline, aggiungiamo dei **registri per salvare i dati**.

Ad ogni ciclo di clock le informazioni procedono da un registro di pipeline al successivo. I nomi di questi registri sono:
- *Registro IF/ID* (*I*nstruction *F*etch/*I*nstruction *D*ecode);
- *Registro ID/EX* (*I*nstruction *D*ecode/*E*xecute);
- *Registro EX/MEM* (*E*xecute/*MEM*ory Access);
- *Registro MEM/WB* (*MEM*ory Access/*W*rite *B*ack).

![[Immagine 2021-12-13 101300.png|800]]

Però con questo disegno abbiamo un problema. 
Tenendo conto di una `lw`, nella fase WB, il numero del registro da scrivere è contenuto nel registro IF/ID, ma tale registro contiene un'istruzione successiva alla lw.

![[Pasted image 20211213102135.png]]

Una **soluzione** è quella di *preservare il numero del registro di destinazione* di lw. Il numero del registro viene scritto/trasportato nei registri di pipeline.

![[Pasted image 20211213102425.png|800]]

## UNITÀ DI CONTROLLO CON PIPELINE E PROGETTO FINALE

Abbiamo la necessità di usare la stessa [[066 Control Unit Generale|Unità di Controllo Generale]] e della stessa [[065 Control Unit ALU|Unità di Controllo della ALU]], ma questa volta dobbiamo trasportare i bit di controllo nelle varie fasi della pipeline. Per questo usiamo i registri intermedi.

![[Immagine 2021-12-13 103140.png|1000]]


___
[[000 Indice AdE|↖ Ritorna all'indice ↖]]

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