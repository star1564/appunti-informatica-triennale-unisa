---
aliases:
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Riducibilità
cssclasses:
    - 
---

> [!note] Definizioni
>
> -   $A_{TM}=\{\langle M,w \rangle\ |\ M \text{ è una MdT e } w\in \mathcal{L}(M)\}$
>     -   L'insieme di tutte le descrizioni di macchine di Turing con un input che viene accettato dalla macchina.
>     -   Indecidibile.
> -   $REGULAR_{TM}=\{\langle M \rangle\ |\ M \text{ è una MdT e } \mathcal{L}(M) \text{ è regolare}\}$
>     -   L'insieme di tutte le descrizioni di macchine di Turing che hanno [[007 Linguaggio Regolare|linguaggi regolari]]


## Dimostrazione Riducibilità

> [!quote] Teorema
> $$A_{TM}\leq_m REGULAR_{TM}$$
> Ovvero che $A_{TM}$ è [[045 Riducibilità|riducibile]] in $REGULAR_{TM}$.

Per provare che $A_{TM} \leq_m REGULAR_{TM}$ definiamo una [[044 Funzioni Calcolabili|funzione calcolabile]] $f : \Sigma^* \to \Sigma^*$ tale che, per ogni stringa $\langle M,w \rangle$, con $M$ Macchine di Turing e $w$ stringa
$$f(\langle M,w \rangle)=\langle R \rangle$$

Dove $R$ è una Macchina di Turing descritta in questo modo.

> [!note] Descrizione di $R$
> Sull'input $x$:
>
> 1. Se $x\in\{0^n1^n\ |\ n\in\mathbb{N}\}$, allora $R$ si ferma e accetta $x$
> 2. Se $x\notin\{0^n1^n\ |\ n\in\mathbb{N}\}$, allora $R$ simula $M$ su $w$ e accetta $x$ se $M$ accetta $w$
>
> Quindi $$\mathcal{L}(R)=\begin{cases} \Sigma^*,\quad\quad\quad\quad\quad\quad\quad\text{ se }\langle M,w \rangle\in A_{TM} \\ \{0^n1^n\ |\ n\in\mathbb{N}\},\quad\quad \text{altrimenti} \end{cases}$$

Inoltre $$\langle M,w \rangle\in A_{TM} \Leftrightarrow \langle R \rangle\in REGULAR_{TM}$$
La funzione $f$, che associa a $\langle M,w \rangle$ la stringa $\langle R \rangle$, è una riduzione da $A_{TM}$ in $REGULAR_{TM}$.
Infatti, $f$ è calcolabile: possiamo costruire una Macchina di Turing $F$ che sull'input $\langle M,w \rangle$ fornisce in output $\langle R \rangle$.

Quindi: $$\textcolor{#61AFEF}{\langle M,w \rangle\in A_{TM}} \Rightarrow M \text{ accetta }w \Rightarrow \mathcal{L}(R)=\Sigma^* \text{ è regolare} \Rightarrow \textcolor{#61AFEF}{\langle R \rangle \in REGULAR_{TM}}$$

Inoltre: $$\textcolor{#61AFEF}{\langle M,w \rangle\notin A_{TM}} \Rightarrow M \text{ non accetta }w \Rightarrow \mathcal{L}(R)=\{0^n1^n\ |\ n\in\mathbb{N}\} \text{ non è regolare} \Rightarrow \textcolor{#61AFEF}{\langle R \rangle \notin REGULAR_{TM}}$$

In conclusione: $$\textcolor{#61AFEF}{\langle M,w \rangle\in A_{TM}} \Leftrightarrow M \text{ accetta }w \Rightarrow \mathcal{L}(R) \text{ è regolare} \Leftrightarrow \textcolor{#61AFEF}{\langle R \rangle \in REGULAR_{TM}}$$


## Teorema Indecidibilità

> [!quote] Teorema
> $REGULAR_{TM}=\{\langle M \rangle\ |\ M \text{ è una MdT e } \mathcal{L}(M) \text{ è regolare}\}$ è [[042 Linguaggi Indecidibili|indecidibile]].

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
