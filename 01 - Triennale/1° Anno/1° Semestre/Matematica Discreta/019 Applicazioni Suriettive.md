---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Relazioni - Applicazioni
cssclasses:
  - 
---
> Una **Applicazione Suriettiva** è un tipo di [[015 Applicazioni|applicazione]] dove ogni elemento nel codominio ha *almeno* un elemento nel dominio che lo mappa su di esso.

Ovvero *quando tutti gli elementi del secondo insieme vengono raggiunti almeno una volta*.

> [!quote] Definizione Formale
> 
>Sia $f:S\rightarrow T$ una applicazione. 
>Si dice che $f$ è **suriettiva** se $f(S)=T$. 
> $$\color{#CC241D}f(S)=T \iff \forall y\in T: \exists x\in S: f(x)=y$$

Dove $f$ non è suriettiva se:
$f(S)\subset T \iff \exists y\in T: y\notin f \iff \exists y\in T: f(x)\neq y,\quad\forall x\in S$

> [!tip] Riconoscere subito una applicazione suriettiva
> Data una applicazione con *elementi finiti ed elencati*, si nota se è suriettiva quando tutti gli elementi di $T$ si trovano alle seconde coordinate delle coppie.

> [!example]- <font color="orange">Esempio</font>
>A = {a, b, c, d}, B = {1, 2, 3}.
>$f$ = {(a, 1), (b, 2), (c, 3), (d, 3)}
>
>![[Pasted image 20211025114328.png|400]]
>
>$f$ è Suriettiva.
>Tutti gli elementi di $T$ sono stati raggiunti almeno una volta.

## VERIFICARE LA SURIETTIVITÀ
1. *Porre $f(x) = y$*;
2. *Risolvere l'uguaglianza in funzione di y*;
3. *Se* alla fine i risultati, con $y\in$ codominio di $f$ e $x\in$ dominio di $f$, si trovano nei rispettivi domini *allora $f$ è suriettiva*. Altrimenti non lo è.

### Suriettività del grafico
Si può verificare la suriettività di una funzione dal grafico.

Si deve osservare se la proiezione dell'intero grafico sull'asse delle $y$ copre interamente l'asse delle $y$.

Se accade, la funzione è suriettiva, altrimenti non lo è.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20220311110740.png|200]]
>Questa funzione non è suriettiva, perché se tracciamo una linea verticale su tutto il grafico notiamo che al di sotto di $-7$ non ci sono corrispondenze.
>
>![[Pasted image 20220311110856.png|200]]
>Questa, invece, è suriettiva.


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
- Pagina Libro: 66