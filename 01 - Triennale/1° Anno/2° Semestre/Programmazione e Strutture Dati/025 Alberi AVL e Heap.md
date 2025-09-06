---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Abstract Data Type
cssclasses:
  - 
---
## ALBERI BILANCIATI E $\Delta$-BILANCIATI
Un [[024 Alberi Binari di Ricerca|albero binario di ricerca]] si dice **Bilanciato** se controlla anche la sua altezza, facendo in modo da rimanere con una [[031 Notazioni Asintotiche|notazione computazionale]] $\color{#61AFEF}O(log(n))$.

Per fare ciò, un albero bilanciato di deve *ri-bilanciare* dopo l'aggiunta o la rimozione di un nodo.

Si dice invece **$\Delta$-Bilanciato** se per ogni nodo, la differenza in valore assoluto tra le altezze dei due sottoalberi è *minore o uguale a $\Delta$*.

Se $\Delta = 1$, allora abbiamo un albero bilanciato.

Ci sono diversi tipi di alberi bilanciati.

## ALBERI AVL
Gli **Alberi AVL** sono degli alberi $\Delta$-Bilanciati, infatti un nodo è bilanciato se l'altezza dei suoi sottoalberi differisce di 1 al massimo.

$$AVL(x)\iff\Big|altezza(x\to right)-altezza(x\to left)\Big|\leq 1$$ $$con\ AVL(x\to right)\ \land\ AVL(x\to left)$$

Per prevenire il non bilanciamento, bisogna aggiungere un marcatore ad ogni nodo, che può assumere i seguenti valori:
- $\color{#CC241D}-1$, se l'altezza del sottoalbero sinistro è maggiore (di 1) dell'altezza del sottoalbero destro;
- $\color{#CC241D}0$, se l'altezza del sottoalbero sinistro è uguale all'altezza del sottoalbero destro;
- $\color{#CC241D}+1$, se l'altezza del sottoalbero sinistro è minore (di 1) dell'altezza del sottoalbero destro.

Se si trova un $\color{#61AFEF}\pm2$ nell'albero, questo risulterà *sbilanciato*. 

![[23e9a277-e9c2-49b9-8f2f-7ae36c482320.jpg|500]]

L'inserimento o la rimozione di una foglia può provocare uno sbilanciamento dell'albero, perché l'indicatore di almeno uno dei nodi non rispetta più uno dei tre stati precedenti.

In tal caso bisogna ribilanciare l'albero con delle **Operazioni di Rotazione** (Semplice o Doppia) agendo sul nodo $x$ a profondità massima che presenta un non bilanciamento.
Questo nodo viene chiamato *Nodo Critico* e si trova sul percorso che va dalla radice al nodo foglia inserito.

![[0e503119-c9d4-4828-bedd-0f7fc0777e03.jpg|650]]

### ROTAZIONE SEMPLICE
- Inserimento nel sottoalbero sinistro del figlio sinistro del nodo critico;
- Inserimento nel sottoalbero destro del figlio destro del nodo critico.

![[f3ef6c65-6ec5-4983-9b8d-c2f1b39c7690.jpg|600]]

### ROTAZIONE DOPPIA
- Inserimento nel sottoalbero destro del figlio sinistro del nodo critico;
- Inserimento nel sottoalbero sinistro del figlio destro del nodo critico;

![[efcdacca-9841-4cd9-902b-b13c9430e232.jpg|600]]

## HEAP
Un **Heap** è un albero binario bilanciato con le seguenti proprietà:
- Le foglie (nodi con livello $h$) sono tutte addossate a sinistra;
- Ogni nodo $v$ ha la caratteristica che l'informazione ad esso associata è la più grande tra tutte le informazioni presenti nel sottoalbero che ha $v$ come radice.

Questo viene usato per realizzare code a priorità, con operazioni di inserimento di un elemento e rimozione del massimo.

![[5e96bfd9-81f6-4770-892c-81ac1c76c537.jpg]]

### OPERAZIONI DI INSERIMENTO
- Si inserisce il nuovo elemento come ultima foglia;
- Il nodo inserito risale lungo il percorso che porta la radice per individuare la posizione giusta.

### OPERAZIONI DI RIMOZIONE
- Si rimuove sempre la radice e si pone l'ultima foglia al posto della radice;
- Si scambia l'informazione contenuta nella radice con la maggiore dei suoi due sottoalberi, si ripete il procedimento fino ad arrivare ad una foglia.

___
[[000 Indice PSD|↖ Ritorna all'indice ↖]]

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