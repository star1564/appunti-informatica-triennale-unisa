---
aliases:
  - MdT
  - Macchina di Turing
  - Macchina di Turing Deterministica
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Macchine di Turing
cssclasses:
  - 
---
>Una **Macchina di Turing** (MdT) è simile ad un [[004 Automi Finiti Deterministici#Automi Finiti|automa finito]], ma con una *memoria semi-illimitata e senza restrizioni*. È un modello molto più preciso di un computer, perché può fare tutto quello che può fare un computer reale.
>
>Attraverso le Macchine di Turing è anche possibile *rappresentare [[001 Introduzione a PA#^Algoritmo|algoritmi]] per calcolare [[011 Funzioni|funzioni]]*.

Questo tipo di macchina viene anche chiamata *deterministica*.

## Elementi di una Macchina di Turing
Una Macchina di Turing è composta principalmente da tre elementi.

### Nastro per l'input
>Questo è un **nastro semi-infinito** (*infinito a destra*) che rappresenta la memoria della macchina. 

Il nastro è diviso in *celle*, dove ciascuna cella contiene un carattere.
- Nell'*alfabeto dei simboli del nastro* $(\Gamma)$ viene aggiunto un nuovo carattere, il **blank $\sqcup$**, che indica la *non presenza di simboli* nella cella
- Se la stringa finisce, verranno piazzati un numero infinito di blank dopo l'ultimo carattere

![[Pasted image 20240422113656.png|800]]


### Testina Lettura/Scrittura
>La **testina** è in grado di ***leggere*** e ***scrivere*** simboli nella cella corrente ed è libera di muoversi lungo il nastro *in entrambe le direzioni* (ma non può oltrepassare il limite a sinistra del nastro).

Se la testina si trova su una cella specifica, allora si trova in uno *stato $q$*.

In base allo stato in cui si trova e del simbolo input letto, la macchina utilizza una [[019 Funzione di Transizione delle Macchine di Turing|funzione di transizione]] per: 
1. *Cambiare stato* 
2. *Scrivere un simbolo* nella cella su cui era posizionata
3. *Spostare la testina* di una posizione a destra o a sinistra

![[Pasted image 20240422104612.png|800]]


### Stati di Arresto
La macchina continua a computare *finché non raggiunge uno stato di arresto*. Gli stati di arresto possono essere:
- Di **accettazione** ($q_{accept}$), dove l'input viene accettato
- Di **rifiuto** ($q_{reject}$), dove l'input viene rifiutato

Quindi, se la macchina *raggiunge* uno di questi stati, la computazione *termina*. Se, invece, *non raggiunge* nessuno di questi stati, la macchina andrà avanti *per sempre*, senza mai fermarsi.

> [!info] Il numero di passi di computazione non è limitato dalla lunghezza dell'input

## 

> [!example]- <font color="orange">Esempio</font>
>Descrizione ad alto livello di una Macchina di Turing che accetta il linguaggio $A=\{0^n1^n:n>0\}$:
>1. Se legge $0$ lo sostituisce con $X$, se legge $1$ rifiuta, se legge $Y$ va al passo 4.
>2. Scorre il nastro verso destra fino a incontrare $1$, se trova $1$ lo sostituisce con $Y$, altrimenti rifiuta.
>3. Scorre il nastro a sinistra fino a incontrare $X$, si sposta a destra e ripete dal passo 1.
>4. Scorre il nastro a destra. Se legge solo delle $Y$ e poi il carattere $\sqcup$, accetta. Altrimenti rifiuta.
>
>$000111\sqcup\to \textcolor{#00C575}X00111\sqcup\to X00\textcolor{#00C575}Y11\sqcup \to X\textcolor{#00C575}X0Y11\sqcup \to XX0Y\textcolor{#00C575}Y1\sqcup \to XX\textcolor{#00C575}XYY1\sqcup \to XXXYY\textcolor{#00C575}Y\sqcup \to XXX\textcolor{#00C575}YYY\sqcup \to XXXY\textcolor{#00C575}YY\sqcup\to$ 
>$\to XXXYY\textcolor{#00C575}Y\sqcup \to XXXYYY\textcolor{#00C575}\sqcup\to \textcolor{#00C575}{q_{accept}}$

## Definizione Formale
>Una **Macchina di Turing Deterministica** è una settupla $\color{#61AFEF}(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$, dove:
>1. $\color{#61AFEF}Q$ è un insieme finito chiamato l'**insieme degli stati**
>2. $\color{#61AFEF}\Sigma$ è un insieme finito chiamato l'**alfabeto dei simboli input** 
>	- $\sqcup\not\in\Sigma$
>3. $\color{#61AFEF}\Gamma$ è un insieme finito chiamato l'**alfabeto dei simboli del nastro**
> 	- $\sqcup\in\Gamma$
> 	- $\Sigma\subset\Gamma$, perché $\sqcup\in\Gamma$ ma $\sqcup\not\in\Sigma$
> 	- $L,R\not\in\Gamma$
>4. $\color{#61AFEF}\delta:(Q-\{q_{accept},q_{reject}\})\times\Gamma\to Q\times\Gamma\times\{L,R\}$ è la **[[019 Funzione di Transizione delle Macchine di Turing|funzione di transizione]]**, con l'insieme dei possibili stati successori
>5. $\color{#61AFEF}q_0\in Q$ è lo **stato iniziale**
>6. $\color{#61AFEF}q_{accept}$ è lo stato di **accettazione**
>7. $\color{#61AFEF}q_{reject}$ è lo stato di **rifiuto**
>	- $q_{accept} \textcolor{#61AFEF}{\neq} q_{reject}$

> [!warning] Spesso l'aggettivo "deterministica" è sottinteso


___
[[000 Indice ETC|↖ Ritorna all'indice ↖]]

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
- [turingmachine.io - Simulazione di una MdT](http://turingmachine.io)