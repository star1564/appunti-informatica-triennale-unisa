---
aliases:
  - Albero
cssclasses:
  - 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Alberi e Grafi
---

### Albero Basilare
>Sia $G$ un [[012 Grafi|Grafo]] con $n$ nodi. Queste affermazioni forniscono la definizione di un *albero basilare* (su cui si basano tutti i tipi di alberi):
>1. $G$ è [[012 Grafi#^bf9a77|connesso]];
>2. $G$ non contiene [[016 Cicli nei grafi|cicli]];
>3. $G$ ha esattamente $n-1$ archi.

![[Immagine 2023-06-07 104252.png|400]]

---
### Albero Radicato
Gli alberi radicati sono la tipologia comune di alberi.

>Un **Albero Radicato** è una struttura per rappresentare organizzazioni gerarchiche di dati, procedimenti decisionali enumerativi ed altro.

Ogni albero radicato è composto da:
- Un *Nodo Intermedio*, questo ha un arco entrante e può avere almeno un arco uscente; ^3090b6
- Le *Foglie*, queste sono dei nodi che hanno un arco entrante ma hanno zero archi in uscita; ^d91f38
- Una *Radice*, questa è la base su cui si fonda l'intero albero, non ha archi entranti e può avere zero o più archi uscenti, (se ha zero archi uscenti allora l'intero albero ha solo un elemento). ^0c339a

Ad ogni tipo di nodo viene solitamente associato un valore, detto etichetta del nodo.
Viene introdotta qui la relazione "genitore-figlio" (o padre-figlio) che caratterizza gli alberi.

![[6f2eeccf-d651-4287-a7f5-c8693326a987.jpg]] 
^f4688c

I nodi degli alberi e gli alberi stessi hanno diverse proprietà, tra cui:
- Il *Grado di un Nodo*: questo è il numero di figli di un nodo;
- L'*Ordine dell'Albero*: questo è il grado massimo tra tutti i nodi;
- Il *Cammino*: è una sequenza di nodi $<n_0,n_1,...,n_k>$ dove il nodo $i$-esimo è il padre del nodo $n_i+1$, per $0\leq i< k$.
- Il *Livello di un Nodo*: è la lunghezza del cammino dalla radice al nodo, questo viene definito ricorsivamente (il livello della radice è 0, il livello di un nodo non radice è $1$ + il livello del padre);
- L'*Altezza dell'Albero*: è la lunghezza del più lungo cammino nell'albero, parte dalla radice e termina in una foglia. ^99df61

---
### Foresta
Una **Foresta** è un *insieme di alberi*, dove ogni albero è un sottografo connesso senza cicli. Ciò significa che esiste un percorso unico tra ogni coppia di nodi all'interno di ciascun albero, ma *non esiste alcun percorso tra i nodi di alberi diversi*. Ogni albero all'interno di una foresta è considerato una [[012 Grafi#Componente Connessa|componente connessa]].

![[Pasted image 20230608121949.png]]

#

___
[[000 Indice PA|↖ Ritorna all'indice ↖]]

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