---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Memoria
cssclasses:
  - 
---
La [[034 Memoria Virtuale|Memoria Virtuale]] può essere implementata attraverso la **Paginazione su Richiesta** (demand paging).

Si consideri il caricamento in memoria di un eseguibile residente sul disco.

Il concetto generale alla base della paginazione su richiesta è di caricare in memoria una pagina *solo quando è richiesta* durante l'esecuzione del programma. 
In questo modo le [[031 Paginazione|pagine]] che non vengono usate non sono mai caricate in memoria, evitando lo spreco dello spazio.

Quando la pagina richiesta viene portata in memoria, si aggiorna la tabella delle pagine.
Ovviamente bisogna avere almeno un frame libero nella memoria principale per poter caricare la nuova pagina richiesta dal processo.

#### BIT DI VALIDITÀ
Per poter gestire quali pagine del processo sono realmente in memoria principale o meno, a ciascun elemento della tabella delle pagine si associa un ulteriore bit, detto **Bit di Validità**. 

Tale bit, impostato a `valido` (**`v`**), indica che la pagina corrispondente è nella memoria fisica, quindi è una pagina valida.
Se il bit è impostato a `invalido` (**`i`**), si indica che la pagina non è nella memoria fisica.

![[Pasted image 20221216111622.png|600]]
<center>Tabella delle pagine quando alcune pagine non si trovano nella memoria principale</center>

Inizialmente il bit è impostato ad `i` per tutti gli elementi della tabella.

#### PAGE FAULT
Se, durante la traduzione dell'indirizzo, il bit di validità dell'elemento della tabella delle pagine è `i`, questo significa che è avvenuto un *riferimento* ad una pagina non presente in memoria. Questo fenomeno provoca l'invio di una [[Interrupt e Trap#^4b8f83|trap]] di **page fault** al sistema operativo.

Il SO esamina una tabella interna per il processo da cui deriva la trap e decide cosa fare:
1. Se il riferimento non era valido, si termina il processo;
2. Se il riferimento era valido, ma la pagina non era ancora stata portata in memora, se ne effettua il caricamento. In questo caso:
	3. si individua un frame libero;
	4. si sposta la pagina nel frame;
	5. si modifica la tabella delle pagine, impostando il bit di validità a `v`;
	6. si riavvia l'istruzione interrotta dall'eccezione.

![[Pasted image 20221216113229.png|580]]

#### PRESTAZIONI DELLA PAGINAZIONE SU RICHIESTA
La paginazione su richiesta può avere un effetto rilevante sulle prestazioni. 

Per calcolare il **Tempo di Accesso Effettivo** alla Memoria si utilizza questa formula, dove:
- $\color{#CC241D}p$, *Probabilità di Page Fault*: 
	- se $p=0$ non ci sono mancanze di pagine;
	- se $p=1$ ogni riferimento è una mancanza di pagina.
- $\color{#CC241D}ma$, *Tempo di Accesso alla Memoria*;
- $\color{#CC241D}tg$, *Tempo di Gestione del Page Fault*, somma di:
	- Page Fault Overhead;
	- Lettura della pagina;
	- Overhead ripresa.

$$EAT = (1-p)\cdot ma + (p\cdot tg)$$

> [!example]- <font color="orange">Esempio</font>
> - $p$: 1 su 1000 accessi causa page fault.
> - $ma = 200\ nanosec$
> - $tg = 8\ millisec = 8.000.000\ nanosec$
> $$(1-p)\cdot 200 + (p\cdot 8.000.000) = (1-p)\cdot ma + (p\cdot tg) = 200+7.999.800\cdot p$$

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