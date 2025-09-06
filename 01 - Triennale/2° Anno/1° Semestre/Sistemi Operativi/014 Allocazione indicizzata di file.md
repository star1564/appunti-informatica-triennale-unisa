---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: File System
cssclasses:
  - 
---
L'**Allocazione Indicizzata** raggruppa tutti i puntatori dell'[[013 Allocazione linkata di file|allocazione linkata]] in una sola locazione: il *Blocco Indice*.

Questo tipo di allocazione è quello usato da UNIX.

Ogni file ha un suo blocco indice: si tratta di un array contenente gli indirizzi dei blocchi di cui il file è costituito.

![[Pasted image 20221006163623.png|650]]

Una volta creato il file, tutti i puntatori del blocco indice sono impostati a `null`. Quando c'è la necessità di un nuovo blocco, si alloca mettendo il suo indirizzo nell'i-esima posizione del blocco indice.

#### MAPPATURA INDIRIZZI LOGICI/FISICI
Dato un Logical Address possiamo calcolare con la seguente divisione:
$$LA/\textcolor{#CC241D}{512} = \begin{cases} Q \\ R \end{cases}$$
- Indice nel blocco indice: $Q$;
- Indice del blocco: $R$.

#### SVANTAGGI
Non è possibile avere dei file più lunghi che richiedono molti indici nel blocco indice (che ha dimensione fissa).

## SCHEMA CONCATENATO
Per permettere la presenza di file lunghi, vengono *collegati* tra loro parecchi blocchi indice. Ciò è fatto ponendo come ultima parola di un blocco indice il *puntatore al prossimo blocco indice*.

![[Pasted image 20221006164252.png|500]]

#### MAPPATURA INDIRIZZI LOGICI/FISICI
Dato un Logical Address possiamo calcolare con la seguente divisione:
$$LA/\textcolor{#CC241D}{(511\times512)} = \begin{cases} Q_1 \\ R_1 \end{cases}$$
- Blocco della tabella indice: $Q_1+1$;
$R_1$ è usato come segue: $$R_1/\textcolor{#CC241D}{512} = \begin{cases} Q_2 \\ R_2 \end{cases}$$
- Indice nel blocco della tabella indice: $Q_2$;
- Indice nel blocco di file: $R_2$;

#### ALLOCAZIONE IND. A DUE LIVELLI
Scorrere all'interno dei blocchi indici concatenati potrebbe essere poco efficiente se il file è molto lungo. 
Per questo si crea un allocazione indicizzata a due livelli (si potrebbe andare anche a tre livelli).

![[Pasted image 20221006164727.png|615]]

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