---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Alberi e Grafi
cssclasses:
  - 
---
> L'**Algoritmo di Prim** è un algoritmo di tipo [[011 Algoritmi Greedy|greedy]] utilizzato per trovare l'[[020 Minimo Sottografo Connesso Ricoprente|MST]] di un [[012 Grafi|Grafo]] non diretto e pesato.

> [!summary]+ Algoritmo di Prim
> La complessità di questo algoritmo è $O(|E|\log |V|)$, dove $|E|$ è il numero di archi e $|V|$ è il numero di nodi.
>1. Si seleziona un nodo di partenza arbitrario dal grafo;
>2. Si inizializza un insieme vuoto di archi che formeranno l'MST e un insieme di nodi visitati, che inizialmente contiene solo il nodo di partenza;
>3. Itera finché non sono stati visitati tutti i nodi:
>	- Seleziona l'arco con il peso minimo che collega un nodo visitato a un nodo non visitato. Se ci sono più archi con lo stesso peso minimo, ne viene scelto uno arbitrario. 
>	- Si aggiunge l'arco selezionato all'insieme di archi dell'MST. 
>	- Aggiungere il nodo non visitato connesso all'arco selezionato all'insieme dei nodi visitati.

^e8296c

> [!example] ***[[demo-prim.pdf|↪Esempio (PDF)]]***

> [!quote]- Codice
>```C
>Prim(G, s){
>	// Inserisci in una coda a PRIORITÀ MIN Q tutti i nodi in V;
>	for(ogni nodo v in V){
>		d[v] = infinito;
>		parent[v] = NULL;
>		Q <- v;
>	}
>
>	d[s] = 0;
>	Decrease_Key(Q, s, d[s]);
>
>	while(Q non è vuota){
>		u = Extract_Min(Q); // Rimuovi il nodo con d[...] minore
>		for(ogni nodo x adiacente ad u){
>			if(x in Q && costo(u, x) < d[x]){
>				d[x] = costo(u, x);
>				parent[x] = u;
>				Decrease_Key(Q, x, d[x]); // Cambia la priorità
>			}
>		}
>	}
>}
>```

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