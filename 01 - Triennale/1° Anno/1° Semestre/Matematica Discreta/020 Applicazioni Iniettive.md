---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Relazioni - Applicazioni
cssclasses:
  - 
---
> Una **Applicazione Iniettiva** è un tipo di [[015 Applicazioni|applicazione]] dove *non ci sono due elementi distinti* nel dominio che vengono associati allo stesso elemento nel codominio.

Ovvero solo *quando ALCUNI elementi del secondo insieme vengono raggiunti SOLO una volta*.

> [!quote] Definizione Formale
> 
>Sia $f:S\rightarrow T$ una applicazione. 
>Si dice che $f$ è **suriettiva** se è tale che elementi distinti di $S$ hanno immagini distinte in $T$.
> $$\color{#CC241D}x_1, x_2\in S,x_1 \neq x_2 \Longrightarrow f(x_1)\neq f(x_2)$$

Dove $f$ non è iniettiva se:
$f(x_1)=f(x_2)\space con \space x_1, x_2 \in S \Longrightarrow x_1 = x_2$

> [!tip] Riconoscere subito una applicazione iniettiva
> Data una applicazione con *elementi finiti ed elencati*, si nota se è iniettiva quando tutte le seconde coordinate delle coppie sono diverse tra di loro.

> [!example]- <font color="orange">Esempio</font>
>$A = \{a, b, c, d\}, B = \{1, 3, 5, 7, 9, 11\}$
>$f$ = {(a, 3), (b, 7), (c, 5), (d, 1)}
>
>![[Pasted image 20211027170229.png|400]]
>
>$f$ è Iniettiva.
>Gli elementi di $T$ sono stati raggiunti una  sola volta.


## VERIFICARE L'INIETTIVITÀ
1. *Porre $f(x_1)=f(x_2)$*, con $x_1$ e $x_2$ generici;
2. *Risolvere l'uguaglianza*, portando tutti gli $x_1$ a sinistra dell'uguale e tutti gli $x_2$ a destra dell'uguale;
3. *Se* alla fine troviamo come unica possibile soluzione *$x_1 = x_2$* *allora $f$ è iniettiva*. Altrimenti non lo è.

### Iniettività del grafico
Si può verificare la iniettività di una funzione dal grafico.

Si creano delle linee orizzontali parallele all'asse delle $x$, se riusciamo a trovare almeno una sola retta orizzontale che interseca il grafico della funzione in due o più punti, allora la funzione non è iniettiva.

Invece se tutte le rette orizzontali hanno al massimo una sola intersezione con il grafico allora la funzione è iniettiva.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20220311111100.png|300]]
>Questa funzione non è iniettiva, perché se tracciamo una linea orizzontale su tutto il grafico notiamo che almeno una di queste interseca il grafico della funzione in due punti.
>
>![[Pasted image 20220311111519.png|300]]
>Questa, invece, è iniettiva.


___
[[000 Indice MD|↖ Ritorna all'indice ↖]]

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
- Pagina Libro: 65