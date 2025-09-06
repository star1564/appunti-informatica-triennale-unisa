---
aliases: 
tags:
  - corsi/matematica/metodi_matematici_informatica
paragrafo: Logica Predicativa
cssclasses:
  - 
---
Tradurre una frase da un linguaggio naturale alla forma simbolica è una parte importante del corso.

## Per i [[018 Quantificatore Universale|Quantificatore Universali]]

> [!example] <font color="orange">Esempio</font>
>Tradurre la frase "Ogni studente in questo corso ha studiato MMI" usando predicati e quantificatori.

Questa soluzione è corretta solamente se il dominio è **composto solamente dagli studenti** del corso:
1. Per prima cosa, si *riscrive la frase* in modo da individuare chiaramente quale quantificatore e connettivi logici si devono usare. Otteniamo in questo caso: "Per ogni studente in questo corso, quello studente ha studiato MMI".

2. Poi introduciamo una *variabile* $x$ per far diventare la frase: "Per ogni studente $x$ in questo corso, $x$ ha studiato MMI".

3. Introduciamo $\color{#61AFEF}MMI(x)$ che è la frase "$x$ ha studiato MMI". Quindi, se il dominio di $x$ consiste solamente degli studenti del corso, otteniamo: $\color{#CC241D}\forall x\ MMI(x)$.
 
Se vogliamo considerare il dominio **di tutte le persone**, dobbiamo riscrivere la frase in questo modo: "Per ogni persona $x$, se $x$ è uno studente del corso allora $x$ ha studiato MMI".

- Se $STU(x)$ rappresenta la frase "$x$ è uno studente del corso", vediamo che la frase diventa: $\color{#CC241D}\forall x(STU(x)\to MMI(x))$.

Infatti, per il quantificatore universale, quando si prende in considerazione un dominio del genere, si *associa l'[[006 Implicazione|implicazione]]*.

> [!error] Soluzione Errata con AND
> La soluzione $\forall x(STU(x)\land MMI(x))$ è ERRATA perché questa dice "Tutte le persone sono studenti ed hanno studiato MMI".

## Per i [[019 Quantificatore Esistenziale|Quantificatori Esistenziali]]

> [!example] <font color="orange">Esempio</font>
Tradurre la frase "Qualche studente di questo corso ha è stato a Roma" usando predicati e quantificatori.

Questa soluzione è corretta solamente se il dominio è **composto solamente dagli studenti** del corso:
1. Questa frase significa che: "Esiste almeno uno studente che è stato a Roma".

2. Introduciamo una *variabile* $x$ per far diventare la frase: "Esiste almeno uno studente $x$ di questo corso che è stato a Roma".

3. Introduciamo $R(x)$ che è la frase "$x$ è stato a Roma". Quindi, se il dominio di $x$ consiste solamente degli studenti del corso, otteniamo: $\color{#CC241D}\exists x\ R(x)$.

Se vogliamo considerare il dominio **di tutte le persone**, dobbiamo riscrivere la frase in questo modo: "Esiste almeno una persona $x$ tale che $x$ è uno studente di questo corso ed $x$ è stato a Roma".

- Se $S(x)$ rappresenta la frase "$x$ è uno studente del corso", vediamo che la frase diventa: $\color{#CC241D}\exists x(S(x)\land R(x))$.

Infatti, per il quantificatore esistenziale, quando si prende in considerazione un dominio del genere, si *associa l'[[002 AND (MMI)|AND]]*.

> [!error] Soluzione Errata con l'implicazione
> La soluzione $\exists x(S(x)\to R(x))$ è ERRATA perché l'asserzione risulta Vera anche se una persona che non è uno studente ha visto Roma.

## Per le Asserzioni Composte
> [!example] <font color="orange">Esempio</font>
>Tradurre la frase "Tutti gli studenti del corso hanno visitato Milano oppure Roma" usando predicati e quantificatori.

Se il dominio è l'insieme degli **studenti del corso**:
- Traduciamo la frase in "Per ogni studente $x$ del corso, $x$ ha visitato Milano oppure $x$ ha visitato Roma".
- Diciamo che $M(x)$ è la frase "$x$ ha visitato Milano", $R(x)$ è la frase "$x$ ha visitato Roma".
- Allora la frase diventa: $\color{#CC241D}\forall x(M(x)\lor R(x))$.

Se il dominio è l'**insieme delle persone**:
- Traduciamo la frase in "Per ogni persona $x$, se $x$ è studente del corso, $x$ ha visitato Milano oppure $x$ ha visitato Roma".
- Diciamo che $M(x)$ è la frase "$x$ ha visitato Milano", $R(x)$ è la frase "$x$ ha visitato Roma".
- Allora la frase diventa: $\color{#CC241D}\forall x(S(x)\to (M(x)\lor R(x)))$.



___
[[000 Indice MMI|↖ Ritorna all'indice ↖]]

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
