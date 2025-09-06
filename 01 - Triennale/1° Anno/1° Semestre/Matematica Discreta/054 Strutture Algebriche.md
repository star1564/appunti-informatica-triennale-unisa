---
aliases:
  - Gruppo
  - Anello
  - Campo
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Aritmetica
cssclasses:
  - 
---
Un insieme $S$ munito di una o più operazioni è detto una struttura algebrica. In particolare può essere almeno una delle seguenti distinzioni.

Tenendo conto delle [[051 Congruenze con Classi di Equivalenza#PROPRIETÀ DELLE OPERAZIONI|proprietà delle operazioni]].

## GRUPPO $(G, + )$
Si ha un **Gruppo** se e solo se si soddisfano tutti questi parametri:
1. $+$ è Associativa: $\forall a, b, c\in G, \quad (a+b)+c=a+(b+c)$;
2. Esiste un Elemento Neutro rispetto a $+$: $\exists 0\in G: 0+a =a= a+0,\quad\forall a\in G$;
3. Ogni elemento possiede un "simmetrico" rispetto a $+$: $\forall a\in G, \exists -a\in G: (-a)+a=0=a+(-a)$;

#### GRUPPO ABELIANO
Un *Gruppo* si dice **Abeliano** se ha anche una quarta proprietà:

4. $+$ è Commutativa: $\forall a, b\in G, \quad a+b=b+a$.


## ANELLO $(A, +, \cdot)$
Si ha un **Anello** se e solo se si soddisfano tutti questi parametri:
1. $(A, +)$ è un *Gruppo Abeliano*;
2. $\cdot$ è Associativa: $\forall a, b, c\in A, \quad (a\cdot b)\cdot c=a\cdot(b\cdot c)$;
3. $\cdot$ è Distributiva rispetto a $+$: $\forall a, b, c\in A, \quad a\cdot (b+c) = a\cdot b + a \cdot c$;


#### ANELLO COMMUTATIVO
Un *Anello* si dice **Commutativo** se ha anche la seguente proprietà:

-  $\cdot$ è Commutativa: $\forall a, b\in A, \quad a\cdot b=b\cdot a$.

#### ANELLO UNITARIO
Un *Anello* si dice **Unitario** se ha anche la seguente proprietà:

-  Esiste un Elemento Neutro rispetto a $\cdot$: $\exists 1\in A: 1\cdot a =a= a\cdot 1,\quad\forall a\in A$.

#### ANELLO COMMUTATIVO UNITARIO
Un *Anello* si dice **Commutativo Unitario** se è sia Commutativo che Unitario.

## CAMPO $(A, +, \cdot)$
Si ha un **Campo** se e solo se si soddisfano tutti questi parametri:
1. La $m$ in $\mathbb{Z}_m$ (Insieme A) deve essere un numero primo;
2. $(A, +, \cdot)$ è un *Anello Commutativo Unitario*;
3. Ogni elemento di $A$ diverso da $\{0\}$ è invertibile rispetto alla seconda operazione: $\forall a \in A - \{0\}, \exists a^{-1} = \frac{1}{a} \in A: a\cdot a^{-1} = 1 = a^{-1} \cdot a$.


---
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
