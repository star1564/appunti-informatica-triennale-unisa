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
> -   $HALT_{TM}=\{\langle M,w \rangle\ |\ M \text{ è una MdT ed } M \text{ si arresta su } w\}$
>     -   L'insieme di tutte le descrizioni di macchine di Turing con un input che si arrestano

$HALT_{TM}$ è sia riducibile che indecidibile.


## Dimostrazione Riducibilità

> [!quote] Teorema 5.24 [Sipser] 
> $$A_{TM}\leq_m HALT_{TM}$$
> Ovvero che $A_{TM}$ è [[045 Riducibilità|riducibile]] in $HALT_{TM}$.

Per provare che $A_{TM} \leq_m HALT_{TM}$ definiamo una [[044 Funzioni Calcolabili|funzione calcolabile]] $f : \Sigma^* \to \Sigma^*$ tale che, per ogni stringa $\langle M,w \rangle$, con $M$ Macchina di Turing e $w$ stringa
$$f(\langle M,w \rangle)=\langle M',w \rangle$$

Dove $M'$ è una Macchina di Turing così descritta.

> [!note] Descrizione di $M'$
> Sull'input $x$:
>
> 1. Simula $M$ su $x$
> 2. ➜ Se $M$ accetta, accetta
>    ➜ Se $M$ rifiuta, cicla
>
> Ovvero che $M'$ si ferma su un input $x$ se e solo se $M$ accetta $x$. Altrimenti andrà in un ciclo infinito.

Inoltre $$\langle M,w \rangle\in A_{TM} \Leftrightarrow \langle M',w \rangle\in HALT_{TM}$$
Consideriamo la Macchina di Turing $F$.

> [!note] Descrizione di $F$
> Sull'input $\langle M,w \rangle$:
>
> 1. Costruisce la macchina $M'$
> 2. Fornisce in output $\langle M',w \rangle$

La funzione $f$ calcolata da $F$, che associa ad $\langle M,w \rangle$ la stringa $\langle M',w \rangle$, è una riduzione da $A_{TM}$ in $HALT_{TM}$.
Infatti, $f$ è [[044 Funzioni Calcolabili|calcolabile]], in quanto è possibile definire una Macchina di Turing $F$ che ha il comportamento descritto prima.

Inoltre:
$$\textcolor{#61AFEF}{\langle M,w \rangle\in A_{TM}} \Leftrightarrow M \text{ accetta }w \Leftrightarrow M'\text{ si arresta su }w \Leftrightarrow \textcolor{#61AFEF}{\langle M',w \rangle \in HALT_{TM}}$$


## Dimostrazione Indecidibilità

> [!quote] Teorema 5.1 [Sipser] 
> $HALT_{TM}=\{\langle M,w \rangle\ |\ M \text{ è una MdT ed } M \text{ si arresta su } w\}$ è [[042 Linguaggi Indecidibili|indecidibile]].

Questa dimostrazione è per assurdo. Assumiamo che $HALT_{TM}$ è decidibile ed utilizziamo questa assunzione per dimostrare che $A_{TM}$ è decidibile, contraddicendo il [[042 Linguaggi Indecidibili#^19746f|Teorema 4.11]]. L'idea chiave è quella di mostrare che $A_{TM}$ è riducibile ad $HALT_{TM}$, il che è stato fatto sopra.

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

-   [Tom Scott - Indecidibilità](https://www.youtube.com/watch?v=eqvBaj8UYz4)
-   [lydia - Indecidibilità](https://www.youtube.com/watch?v=VyHbd6sx5Po)
