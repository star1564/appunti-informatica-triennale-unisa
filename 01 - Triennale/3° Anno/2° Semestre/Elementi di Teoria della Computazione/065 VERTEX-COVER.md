---
aliases:
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: NP-completezza
cssclasses:
    - 
---

## Definizione

Sia $G=(V,E)$ un [[012 Grafi#Grafi Non Diretti (Non Orientati)|Grafo non orientato]], sia $V'\subseteq V$ e sia $(u,v)\in E$.

> Se $V'$ _contiene ALMENO UNO dei due vertici_ dell'arco $(u,v)$, diremo che $V'$ **copre** l'arco $(u,v)$.

> [!example]+ <font color="orange">Esempio</font>
> ![[Pasted image 20240606143454.png|600]]

> Un **Vertex Cover $V'$** di $G$ è un sottoinsieme di $V$ tale che per ogni $(u,v)\in E$ risulta che $$\{u,v\}\cap V' \neq \emptyset$$
> Ovvero che _$V'$ copre ogni arco $(u,v)$_ nel grafo.
>
> Il _numero di vertici_ in $V'$ si chiama **cardinalità** del Vertex Cover.

> [!example]+ <font color="orange">Esempio</font>
> ![[Pasted image 20240606144828.png|500]]
>
> ![[Pasted image 20240606144932.png|500]]

Il problema di _stabilire_ se un grafo non orientato $G=(V,E)$ ha un vertex cover di cardinalità $k$ si può formulare come un [[001 Logica Proposizionale#^proposizione|Problema di Decisione]], il cui linguaggio associato è
$$VERTEX\text{-}COVER=\{\langle G,k \rangle\ |\ G\text{ è un grafo non orientato che ha un Vertex Cover di cardinalità }k\}$$

> [!quote] Teorema VERTEX-COVER in NP
>
> > $VERTEX\text{-}COVER\in NP$
>
> Un algoritmo $V$ che [[057 Verificatore Polinomiale|verificatore]] $VERTEX\text{-}COVER$ in [[054 Tempo Polinomiale ed Esponenziale|tempo polinomiale]] è il seguente.
>
> > [!note] Descrizione di $V$
> > Sull'input $\langle \langle G,k \rangle, c \rangle$:
> >
> > 1. Verifica se $c$ è un sottoinsieme $V'$ con $k$ nodi di $G$, altrimenti rifiuta
> > 2. Verifica se $V'$ copre ogni arco in $G$, accetta in caso affermativo, altrimenti rifiuta
> >
> > $$\exists c: \langle \langle G,k \rangle, c \rangle\in \mathcal{L}(V)\iff \langle G,k \rangle\in VERTEX\text{-}COVER$$

^f64b0b

---

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
