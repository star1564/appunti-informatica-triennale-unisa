---
aliases:
  - Problema di Ottimizzazione
  - Programmazione Matematica
  - Programmazione Lineare Intera
  - Programmazione Lineare Continua
tags:
  - corsi/matematica/ricerca_operativa
paragrafo: Programmazione Lineare
cssclasses:
  - 
---
> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

>La **Programmazione Lineare** (PL) si occupa di trovare la *soluzione ottimale* (minimizzazione o massimizzazione) di una [[F01 Funzione Lineare e Costante|funzione lineare]] soddisfacendo allo stesso tempo un insieme di *vincoli o restrizioni*.

I componenti principali di un problema di programmazione lineare sono:
- **Variabili Decisionali** - Variabili che si *vogliono determinare* per ottenere la soluzione ottimale
- **Funzione Obiettivo** - Equazione matematica che rappresenta *l'obiettivo che si vuole raggiungere* (come la minimizzazione o la massimizzazione)
- **Vincoli** (constraint, *inequality*) - Limitazioni o restrizioni che le variabili decisionali *devono rispettare*. Si dice che la funzione obbiettivo deve essere soggetta a questi vincoli (*subject to*, s.t.)
- **Restrizioni di non negatività** - In alcuni scenari reali, le variabili decisionali *non possono essere negative*


## Tipi di Problemi di Programmazione Lineare
### Problema di Ottimizzazione
Data una funzione $f:\mathbb{R}^n\to\mathbb{R}$ (ovvero che si prende un [[002 Vettori|vettore]] ad $n$ componenti e restituisce uno [[002 Vettori#Scalare|Scalare]]) e $X\subseteq\mathbb{R}^n$, un **Problema di Ottimizzazione** (PO) può essere formulato come:

$$\text{min } f(\underline{x})$$$$\text{s.t.}$$$$\underline{x}\in X$$

Dove:
1. **Funzione obiettivo**:
	- $\underline{x}$​ è un *vettore* di $n$ variabili decisionali che rappresenta la soluzione del problema
	- $f(\underline{x}​)$ rappresenta la funzione che vogliamo minimizzare, in questo caso è un problema di minimizzazione
2. $\color{#CC241D}\text{s.t.}=\text{subject to}$: la funzione obbiettivo è soggetta ai vincoli
3. **Vincoli**:
	- $X$ è l'insieme di tutte le soluzioni ammissibili che rispettano i vincoli
	- $\underline{x}​$ deve soddisfare tutti i vincoli per essere una soluzione valida, si quindi deve trovare nell'insieme $X$

Quindi, cerchiamo il *valore minimo* della funzione $f$ tra tutte le soluzioni che *soddisfano i vincoli*.

### Programmazione Matematica
Quando l'insieme delle soluzioni ammissibili di un problema di ottimizzazione *viene espresso attraverso un sistema di equazioni e disequazioni*, tale problema prende il nome di problema di **Programmazione Matematica** (PM).

$$\text{min } f(\underline{x})$$$$\text{s.t.}$$$$g_i(\underline{x})\geq b_i\quad i=1,\dots,m$$

Dove:
- $g_i$ è l'i-esimo vincolo del sistema
- $b_i$ è l'i-esima componente del vettore dei termini noti

## Definizione di Programmazione Lineare
Un problema di Programmazione Matematica è **Lineare** quando:
- la *funzione obbiettivo è lineare*: $f(\underline{x})=\underline{c}^T\underline{x}$
- l'insieme $X$ è espresso in termini di *relazioni* (uguaglianze e disuguaglianze) *lineari*

### Forme Esplicite e Compatte
Il seguente problema di Programmazione Matematica con la funzione obbiettivo lineare 
$$\text{min } f(\underline{x})$$$$\text{s.t.}$$$$g_i(\underline{x})\geq b_i,\quad i=1,\dots,m$$

si può esprimere in due forme.

- <font color="#00C575">Forma Esplicita</font>: $$\color{#00C575}\text{min } c_1x_1+c_2x_2+\dots+c_nx_n$$$$\color{#00C575}\text{s.t.}$$$$\color{#00C575}a_{11}x_1+a_{12}x_2+\dots+a_{1n}x_n\geq b_1$$$$\color{#00C575}a_{21}x_1+a_{22}x_2+\dots+a_{2n}x_n\geq b_2$$$$\color{#00C575}\dots$$$$\color{#00C575}a_{m1}x_1+a_{m2}x_2+\dots+a_{mn}x_n\geq b_m$$
- <font color="#FF6611">Forma Compatta</font>: $$\color{#FF6611}\text{min } \underline{c}^T\underline{x}$$$$\color{#FF6611}\text{s.t. }A\underline{x}\geq\underline{b}$$

### Programmazione Lineare Continua e Intera
Se l'insieme di tutte le possibili soluzioni $X$ è composto da vettori con valori:
- [[003 Insieme dei numeri Reali|reali]], ovvero che $X=\{\underline{x}\in\mathbb{R}^n:A\underline{x}\geq \underline{b}\}$, allora si ha un problema di **Programmazione Lineare Continua (PL)**
- interi, ovvero che $X=\{\underline{x}\in\mathbb{Z}^n:A\underline{x}\geq \underline{b}\}$, allora si ha un problema di **Programmazione Lineare Intera (PLI)**

### Problemi di Programmazione Lineare
Dato il seguente problema di programmazione lineare 
$$\text{min } f(\underline{x})$$$$\text{s.t.}$$$$g_i(\underline{x})\geq b_i\quad i=1,\dots,m$$
Si ha che un vettore $\underline{x}'$ di $\mathbb{R}^n$:
- **Soddisfa** il vincolo $g_i(\underline{x})\geq b_i$ se $\color{#61AFEF}g_i(\underline{x}')\geq b_i$
- **Viola** il vincolo $g_i(\underline{x})\geq b_i$ se $\color{#61AFEF}g_i(\underline{x}')< b_i$
- **Satura** il vincolo $g_i(\underline{x})\geq b_i$ se $\color{#61AFEF}g_i(\underline{x}')= b_i$

>Un vettore $\underline{x}$ di $\mathbb{R}^n$ è una **Soluzione Ammissibile** per un problema di programmazione lineare *se e solo se soddisfa tutti i vincoli del problema*.

Un problema di programmazione lineare risulta:
- *Inammissibile* se la regione ammissibile è vuota ossia $X=\emptyset$.
- *Illimitato* inferiormente (perché il problema è di minimo) se scelto un qualsiasi [[002 Vettori#Scalare|Scalare]] $k$, esiste sempre un punto $\underline{x}\in X$ tale che $f(\underline{x}) < k$.
- *Ammettere soluzione ottima finita* se esiste un punto $\underline{x}^*\in X$ tale che $f(\underline{x}^*) \leq f(\underline{x}), \forall \underline{x}\in X$.

> [!quote] Ottimo Globale
> Un punto $\underline{x}^*\in X$ è un **Ottimo Globale** per la funzione di minimo $f(\underline{x})$ se e solo se $$f(\underline{x}^*)\le f(\underline{x}), \forall \underline{x}\in X$$


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
- [Intro to Linear Programming - Video, Trefort Bazett](https://www.youtube.com/watch?v=K7TL5NMlKIk)
- [geeksforgeeks](https://www.geeksforgeeks.org/linear-programming/)