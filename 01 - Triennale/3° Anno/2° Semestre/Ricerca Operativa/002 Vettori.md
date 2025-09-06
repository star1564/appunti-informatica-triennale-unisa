---
aliases:
  - Vettore Colonna
  - Vettore Riga
  - Vettore Trasposto
  - Vettore Nullo
  - i-esimo vettore fondamentale
  - Scalare
  - Modulo di un Vettore
  - Verso di un Vettore
  - Direzione di un Vettore
tags:
  - corsi/matematica/ricerca_operativa
paragrafo: Vettori
cssclasses:
  - 
---
> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

>Un **Vettore** ad *$n$ componenti reali* è una [[026 Sequenza|sequenza]] di $n$ numeri reali.

Esistono due tipi di vettori:
- **Vettore Colonna** (***default per questo corso***) - È un vettore in cui le componenti sono disposte lungo una *linea verticale*. $$\color{#CC241D}\underline{x}=\begin{bmatrix} x_1 \\ x_2 \\ x_3  \end{bmatrix}$$
- **Vettore Trasposto** (*Vettore Riga*) - È un vettore in cui le componenti sono disposte lungo una *linea orizzontale*. È ottenuto dalla **trasposizione**, un'operazione unaria che trasforma un vettore colonna in un vettore riga. $$\color{#CC241D}\underline{x}^T= [x_1\ x_2\ x_3]$$ ^461f62

> [!example]- <font color="orange">Esempio</font>
>Questo è un vettore colonna in $\mathbb{R}^4$, quindi di dimensione $n=4$ $$\underline{x}=\begin{bmatrix} 3 \\ -1 \\ 5 \\ 7  \end{bmatrix}$$

### Vettore Nullo
Un Vettore si dice **Nullo** quando le sue componenti sono *tutte nulle*. Si indica con $$\underline{0}^T=[0\ 0\ \dots\ 0]$$

### i-esimo Vettore Fondamentale
Un vettore si dice **$i$-esimo Vettore Fondamentale** se tutte le sue componenti sono nulle *tranne la $i$-esima componente che sarà uguale a 1*. Si indica con $$\underline{e_i}^T = [0\ 0\ \dots\ \underbrace{1}_{\text{i-esima componente}}\ \dots 0\ 0]$$
### Scalare
Uno **Scalare** è un qualsiasi numero reale utilizzato nelle [[003 Operazioni con Vettori|operazioni con vettori]].

### Dimensione di un Vettore
Un vettore può avere *$m$ dimensioni*, in questo caso si può denotare *l'insieme di tutti i vettori $m$-dimensionali* attraverso $$\color{#CC241D}\mathbb{R}^m$$


## Rappresentazione di Vettori
Dato un sistema di *assi cartesiani*, ogni vettore può essere rappresentato nel sistema tramite un **punto** oppure da una **segmento** che connette l'origine degli assi al punto (in questo corso il punto di applicazione di un vettore sarà sempre l'origine degli assi $[0\ 0]$).

![[Pasted image 20240323163420.png|700]]

### Caratteristiche di un Vettore
Ogni vettore è caratterizzato da:
- **Modulo** (o intensità) - La *lunghezza* del vettore
- **Direzione** - La *retta* sulla quale giace il vettore
- **Verso** - Uno dei due alternativi sulla retta che dà la direzione del vettore, dettato dalla posizione della *freccia*

![[Pasted image 20240323163937.png|700]]


___
[[000 Indice RO|↖ Ritorna all'indice ↖]]

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
- https://math.libretexts.org/Bookshelves/Linear_Algebra/Understanding_Linear_Algebra_(Austin)/02%3A_Vectors_matrices_and_linear_combinations/2.01%3A_Vectors_and_linear_combinations