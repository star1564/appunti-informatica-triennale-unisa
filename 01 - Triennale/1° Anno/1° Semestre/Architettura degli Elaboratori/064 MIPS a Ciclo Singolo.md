---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: MIPS - Processore
cssclasses:
  - 
---
In questa implementazione, ciascuna istruzione impiega un [[057 Clock|ciclo singolo di clock]], dove l'esecuzione di tutte le istruzioni ha la stessa durata.

![[fe00a941-0abc-40f5-9b2e-5d458e45b61e.png|900]]

I segnali di controllo dei componenti sono generati dall'[[066 Control Unit Generale|Unità di Controllo Generale]].

In particolare i [[021 Multiplexer|multiplexer]] hanno le seguenti funzioni:
- *RegDst*: ha la funzione di portare al banco dei registri come indirizzo del registro di destinazione quello appropriato per il formato attuale.
	- *1* se il formato è R (add, sub, ...);
	- *0* se il formato è una lw (per la sw il registro di scrittura è evitato).
- *ALUSrc*: ha la funzione di portare al secondo operando della ALU il dato adatto in base al formato attuale:
	- *0* se il formato è R oppure una beq;
	- *1* se il formato è I (tranne beq).
- *PCSrc*: ha la funzione di portare al PC l'indirizzo giusto della prossima istruzione:
	- *0* se dobbiamo andare alla prossima istruzione (PC+4);
	- *1* se abbiamo una beq e dobbiamo saltare istruzioni.
- *MemtoReg*: ha la funzione di portare il giusto output risultato nel registro di scrittura in base al formato attuale:
	- *0* se abbiamo una R;
	- *1* se abbiamo una lw.

## FASE FETCH

Questa prima fase è sempre eseguita per tutti i tipi di istruzioni.
![[Pasted image 20211201165243.png|600]]

## FASE REG-EXE-MEM-WB

### ISTRUZIONI DI TIPO R
Per le istruzioni con [[048 Formati delle istruzioni MIPS#FORMATO R|formato R]], vengono usati *tutti gli input ed output del registro* (RN1, RN2, WN, RD1, RD2, WD).
Si salta la fase di salvataggio in memoria, infatti si va a salvare nel registro specificato nell'rd.

Abbiamo per esempio l'istruzione `add`:

![[d088f4ca-a8a0-4759-948f-1cb2baa5a594.png|900]]

- *RegDst*: 1
- *RegWrite*: 1
- *ALUSrc*: 0
- *PCSrc*: 0
- *MemWrite*: 0
- *MemRead*: 0
- *MemtoReg*: 0

### ISTRUZIONI DI TIPO I
Per le istruzioni con [[048 Formati delle istruzioni MIPS#FORMATO I|formato I]] si dovrebbe tenere conto degli ultimi 16 bit a destra; quindi se si deve sommare la costante a 16 bit con il contenuto a 32 bit di un registro, si estende la costante a 32 bit con lo [[051 Istruzioni Logiche MIPS#SHIFT ARITMETICO A DESTRA|shift aritmetico a destra]].

![[Pasted image 20211201171441.png|300]]

#### LW, SW
Prendendo in considerazione le istruzioni `lw` ed `sw`, dobbiamo poter leggere e scrivere nella Memoria Dati, pertanto, nell'istruzione `lw` abbiamo:

![[ab024e57-8f75-4366-9632-2b2cfad12611.png|900]]

- *RegDst*: 0
- *RegWrite*: 1
- *ALUSrc*: 1
- *PCSrc*: 0
- *MemWrite*: 0
- *MemRead*: 1
- *MemtoReg*: 1

Per l'istruzione `sw` abbiamo:

![[4a57cdce-ab59-408f-8d17-02ca1d1dac62.png|900]]

- *RegDst*: 0 (Non Importante per via di RegWrite)
- *RegWrite*: 0
- *ALUSrc*: 1
- *PCSrc*: 0
- *MemWrite*: 1
- *MemRead*: 0
- *MemtoReg*: 0 (Non Importante per via di RegWrite)

#### BEQ
Invece per operare con la `beq` dobbiamo solo vedere se due elementi sono uguali nella ALU e calcolare l'indirizzo a cui saltare sommando (PC+4) + ($costante^2$) con lo shift a sinistra di 2.

Per l'istruzione `beq` abbiamo:

![[01110a89-f744-4b25-8687-74b89a9b3349.png|900]]

- *RegDst*: 0 (Non Importante per via di RegWrite)
- *RegWrite*: 0
- *ALUSrc*: 0
- *PCSrc*: 1
- *MemWrite*: 0
- *MemRead*: 0
- *MemtoReg*: 0 (Non Importante per via di RegWrite)


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