---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: File System
cssclasses:
  - 
---
Un disco può essere diviso in diverse partizioni, ognuna di queste ha un proprio **File System**, che *definisce come i file sono conservati e recuperati* da un dispositivo di [[016 Memoria di Massa|memorizzazione secondario]].

Per <font color="#00C575">leggere</font> un blocco dal disco, lo si deve portare in [[030 Memoria Centrale|memoria principale]].

Per <font color="#FF6611">modificare</font> un blocco, lo si deve portare in memoria principale, per poi riscriverlo nella stessa posizione di prima nel disco.

![[Pasted image 20221006144022.png|800]]

Ogni file system ha:
- *Boot control block*: Informazioni necessarie per il caricamento del sistema operativo all'avvio;

- *Super block*: un contenitore di [[Metadato|metadati]] riguardanti l'intero file system, come il numero totale di blocchi, numero totale di i-node, ecc.;

- *[[015 Gestione dello spazio libero#VETTORE DI BIT BITMAP|Bitmap Blocchi]]*: un array monodimensionale che identifica i blocchi usati e non usati;

- *i-list*: una lista contenente tutti gli [[File Control Block|i-node (FCB)]] del file system;

- *Data Blocks*: zona in cui i file e le directory sono contenuti.

---
### I-NODE E DATA BLOCKS
![[Pasted image 20221006153318.png]]

In questo caso abbiamo la seguente sistemazione:
```text
Cartella/  (i-node 1267)
	└── test_dir/  (i-node 2549)
		└── (nessun file presente)
```

In ogni blocco directory ci sono sempre due file:
- `.` rappresenta la directory attuale;
- `..` rappresenta la directory padre (precedente).

---
### METODI DI ALLOCAZIONE DI BLOCCHI
Un metodo di allocazione specifica il modo in cui i blocchi di un file vengono allocati nel disco:
- [[012 Allocazione contigua di file|Allocazione contigua]];
- [[013 Allocazione linkata di file|Allocazione linkata]];
- [[014 Allocazione indicizzata di file|Allocazione indicizzata]]. 

Ciascuno di questi metodi presenta vantaggi e svantaggi.

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
