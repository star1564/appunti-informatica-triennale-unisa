---
aliases:
  - Parola Accettata da una MdT
  - Parola Riconosciuta da una MdT
  - Parola Rifiutata da una MdT
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Macchine di Turing
cssclasses:
  - 
---
> [!summary]- Computazione su un input
> ![[021 Configurazione di una Macchina di Turing#Computazione su un input]]

Sia $M=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ una Macchina di Turing e sia $C$ una configurazione di $M$. Ci sono tre possibili casi.

## Accettazione
>Una Macchina di Turing **accetta** una parola $w\in\Sigma^*$ se esiste una computazione $$C\to^*C'$$ 
>Dove: 
>1. $\color{#61AFEF}C=q_0\ w$ è la [[021 Configurazione di una Macchina di Turing#Tipi di Configurazioni|Configurazione Iniziale]] di $M$ con input $w$
>2. $\color{#61AFEF}C'=u\ q_{accept}\ v$ è una [[021 Configurazione di una Macchina di Turing#Tipi di Configurazioni|Configurazione di Accettazione]]


Quindi, $M$ accetta l'input $w$ se esiste una sequenza di configurazioni $C_1,C_2,\dots,C_k$ tale che:
1. $C_1=C$, ovvero che $C_1$ è la *configurazione iniziale* di $M$ su input $w$
2. $C_i\to C_{i+1}, \forall i\in\{1,\dots,k-1\}$, ovvero che *ogni $C_i$ produce $C_{i+1}$*
3. $C_k=C'$, ovvero che $C_k$ sia una **configurazione di accettazione**

Pertanto, esistono $u,v\in\Gamma^*$ tali che $$\color{#61AFEF}q_0\ w\to^* u\ q_{accept}\ v$$




## Rifiuto
>Una Macchina di Turing **rifiuta** una parola $w\in\Sigma^*$ se esiste una computazione $$C\to^*C'$$ 
>Dove: 
>1. $\color{#61AFEF}C=q_0\ w$ è la [[021 Configurazione di una Macchina di Turing#Tipi di Configurazioni|Configurazione Iniziale]] di $M$ con input $w$
>2. $\color{#61AFEF}C'=u\ q_{reject}\ v$ è una [[021 Configurazione di una Macchina di Turing#Tipi di Configurazioni|Configurazione di Rifiuto]]


Quindi, $M$ rifiuta l'input $w$ se esiste una sequenza di configurazioni $C_1,C_2,\dots,C_k$ tale che:
1. $C_1=C$, ovvero che $C_1$ è la *configurazione iniziale* di $M$ su input $w$
2. $C_i\to C_{i+1}, \forall i\in\{1,\dots,k-1\}$, ovvero che *ogni $C_i$ produce $C_{i+1}$*
3. $C_k=C'$, ovvero che $C_k$ sia una **configurazione di rifiuto**

![[019 Funzione di Transizione delle Macchine di Turing#^d05899]]

## Non Terminazione
>Una Macchina di Turing **non termina la computazione** su una parola $w\in\Sigma^*$ se *per ogni* configurazione $C'$ tale che $C\to^*C'$ esiste una computazione $C''$ tale che $$C\to^*C'\to^*C''$$

Ovvero che non raggiunge mai una [[021 Configurazione di una Macchina di Turing#Tipi di Configurazioni|Configurazione di Arresto]].

> [!summary]+ Terminazione
> Quindi, $M$ si ferma su $w\in\Sigma^*$ se $M$ accetta **OPPURE** rifiuta $w$.
> 
> Se $M$ non accetta $w$, allora $M$ rifiuta $w$ **OPPURE** la computazione non si arresta su $w$.
>
>|                 | La computazione termina | La computazione non termina |
>| --------------- | ----------------------- | --------------------------- |
>| $M$ accetta $w$ |  ✔️         |   ❌                          |
>| $M$ rifiuta $w$ | ✔️          | ✔️                       |


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