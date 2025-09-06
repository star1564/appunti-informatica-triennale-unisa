---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Memoria
cssclasses:
  - 
---
Il SO conserva una **Lista di Frame Liberi**. 
Quando un [[Processo (Job)|processo]] parte, il sistema esamina la sua dimensione espressa in pagine, quindi se richiede $n$ pagine, devono essere disponibili almeno $n$ frame che vengono presi da questa lista.


<center>Frame liberi; (a) prima e (b) dopo l'allocazione</center>

La tabella delle pagine è mantenuta nella [[030 Memoria Centrale|Memoria Principale]] assieme al processo.

## TLB
La memorizzazione della tabella delle pagine nella memoria principale può favorire [[020 Process Control Block e Cambio di Contesto#CAMBIO DI CONTESTO|cambi di contesto]] più rapidi, infatti il contenuto del puntatore che punta alla tabella delle pagine può essere cambiato per rispecchiare le modifiche avvenute.

Ma questo può anche comportare tempi di accesso alla memoria più lenti. Ogni accesso a dati o istruzioni richiede **due accessi alla memoria**: 
- uno per accedere alla tabella delle pagine;
- uno per recuperare i dati o istruzioni.

La soluzione tipica a questo problema consiste nell'impiego del **TLB** (Translation Look-aside Buffer), una speciale e piccola [[070 Gerarchia di Memoria#MEMORIA CACHE|cache]] hardware per l'indicizzazione veloce.

La TLB è una memoria associativa ad alta velocità in cui ogni elemento consiste di due parti appartenenti a coppie accedute *frequentemente*:
- una chiave (numero di pagina);
- un valore (numero del frame).

Quando si presenta un elemento, la memoria associativa lo confronta contemporaneamente con tutte le chiavi. 
Se trova una corrispondenza (*hit*) ritorna il valore correlato, altrimenti (*miss*) si va a cercare nella tabella delle pagine.

![[Pasted image 20221201120717.png|700]]
<center>Hardware di paginazione con TLB</center>

---
### TEMPO DI ACCESSO EFFETTIVO (EAT)
La formula del calcolo per il **Tempo di Accesso Effettivo (EAT)** utilizza quesiti valori:
- $\color{#CC241D}\alpha$, *Hit Ratio*: la percentuale di volte che il numero di pagina di interesse si trova nel TLB;
- $\color{#CC241D}\epsilon$, *Tempo di Accesso alla TLB*;
- $\color{#CC241D}t$, *Tempo di Accesso alla Memoria*.

$$EAT = [\alpha \cdot (\epsilon + t)] + [(1-\alpha) \cdot (\epsilon + (2\cdot t))]$$
La prima parte rappresenta il caso di successo, in cui con probabilità $\alpha$ trovo l'indirizzo nella TLB.
La seconda parte rappresenta il caso di insuccesso, con *miss rate* di $1-\alpha$.

> [!example]- <font color="orange">Esempio</font>
> - $\alpha = 75\% = 0,75$
> - $\epsilon = 0$
> - $t = 200$
> $$EAT = [0,75 \cdot (0 + 200)] + [0,25 \cdot (0 + 200 + 200)] = 250$$

## PROTEZIONE DELLA MEMORIA PRINCIPALE
In un ambiente paginato, la protezione della memoria centrale è assicurata dai **Bit di Protezione** associati ai frame. Normalmente questi si trovano nella tabella delle pagine.
Un bit può determinare se una pagina si può leggere e scrivere oppure soltanto leggere.

---
> [!info]+ Pagine Condivise
>Un vantaggio della paginazione risiede nella possibilità di *Condividere* codice comune, il che è particolarmente importante in un ambiente con più processi.
>![[Pasted image 20221201125045.png|600]]

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