---
aliases: 
  - Proposizione
  - Problema di Decisione
  - Variabile Proposizionale
  - Variabile Booleana
tags:
  - corsi/matematica/metodi_matematici_informatica
paragrafo: Logica Proposizionale
cssclasses:
  - 
---
>Una **Proposizione** (o *problema di decisione*) è una frase che ha come soluzione *vero o falso*, ma non può essere entrambe.
>Le proposizioni fanno parte della **Logica Proposizionale**. Ad ogni proposizione è associata una [[Tabella di Verità|Tabella di Verità]].

^proposizione

In generale, se una "affermazione" dipende da incognite o variabili (come $x$), allora questa *non è una proposizione*, dato che non si può dedurre se l'affermazione sia vera o falsa.

> [!example]+ <font color="orange">Esempio</font>
>
>| Frase                          | Proposizione? | Vera | Falsa |
>| ------------------------------ | ----- | ---- | ----- |
>| Bologna è la capitale d'Italia | SI    | NO   | SI    |
>| $x+1=2$                        | NO    | -    | -     |
>| $1+1=2$                        | SI    | SI   | NO    |
>| Che ore sono?                  | NO    | -    | -     |

### Variabile Proposizionale (Booleana)
>Ad ogni proposizione viene assegnata una **Variabile Proposizionale** (o *Booleana*) che la rappresenta. Il valore di questa variabile proposizionale può essere: 
>- **`Vero`** (*True*, *`T`*, *`1`*) se la proposizione è vera
>- **`Falso`** (*False*, *`F`*, *`0`*) se la proposizione è falsa

^dda1ee

Ogni proposizione rientra in una delle seguenti quattro forme linguistiche:
1. L'oggetto $\color{#61AFEF}x$ gode della proprietà $\color{#61AFEF}P$;
2. Ogni oggetto dell'[[001 Insieme|insieme]] o del tipo $\color{#61AFEF}T$ gode della proprietà $\color{#61AFEF}P$;
3. Esiste *almeno un oggetto* dell'insieme o del tipo $\color{#61AFEF}T$ che gode della proprietà $\color{#61AFEF}P$;
4. Se *proposizione* $\color{#61AFEF}A$ è vera allora *proposizione* $\color{#61AFEF}B$ è vera.

## Proposizioni Composte e Connettivi Logici
>Le proposizioni possono essere *combinate* tra di loro, creane alcune più complesse, ovvero delle **Proposizioni Composte**, unite attraverso i *connettivi logici*.

I connettivi logici sono i seguenti:
- [[002 AND (MMI)|AND]]
- [[003 OR (MMI)|OR]]
- [[004 NOT (MMI)|NOT]]
- [[005 XOR (MMI)|XOR]]
- [[006 Implicazione|Implicazione]]
- [[007 Equivalenza Logica|Equivalenza Logica]]
- [[008 Equivalenza Bicondizionale|Equivalenza (Bicondizione)]]

> [!example]- <font color="orange">Esempio</font>
>- Proposizione $A$: "Fuori piove";
>- Proposizione $B$: "Fuori nevica";
>- Proposizione $C$: "Esco con l'ombrello";
>- Proposizione Composta: Se $A$ oppure $B$ allora $C$.
>
>Forma simbolica: $(A\lor B)\Rightarrow C$.

> [!success] [[Alcune Proprietà di MMI|Alcune proprietà dei connettivi logici]]

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