---
aliases:
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Indecidibilità
cssclasses:
    - 
---

> Un linguaggio $L$ è **Co-Turing-Riconoscibile** se il suo [[013 Complemento di un Insieme|complemento]] $\overline{L}$ è [[023 Linguaggio Turing-Riconoscibile|Turing-Riconoscibile]]. Dove il complemento di $L$ sull'alfabeto $\Sigma$ è $$\overline{L}=\{w\in\Sigma^*\ |\ w\not\in L\}$$


## Proprietà

### La classe dei linguaggi decidibili è chiusa rispetto al complemento

> [!tip] [[Chiusura di un_opearzione|Chiusura di un'operazione]]

Sia $A$ un linguaggio decidibile, sia $M_A$ una Macchina di Turing che [[024 Macchine Decisionali#Linguaggio Deciso|decide]] $A$.

Definiamo la Macchina di Turing $M_{\overline{A}}$ che, sull'input $w$, simula $M_A$ e _accetta $w$ se e solo se $M_A$ rifiuta $w$_.

-   Poiché $M_A$ si arresta su ogni input, anche *$M_{\overline{A}}$ si arresta su ogni input*.
-   Inoltre, *il linguaggio $M_{\overline{A}}$ è $\overline{A}$*, perché $M_{\overline{A}}$ accetta $w$ se e solo se $M_A$ rifiuta $w$ e quindi se e solo se $w\not\in A$.

Quindi $M_{\overline{A}}$ è una Macchina di Turing che decide $\overline{A}$, che è decidibile.

### Un linguaggio $L$ è decidibile se e solo se $L$ è Turing-Riconoscibile e Co-Turing-Riconoscibile

Dobbiamo provare l'[[008 Equivalenza Bicondizionale|equivalenza]] $$L \text{ è decidibile (1)} \Leftrightarrow L \text{ e } \overline{L} \text{ sono entrambi Turing-Riconoscibili (2)}$$

#### 1. $L$ è decidibile

Se $L$ è decidibile, allora esiste un [[024 Macchine Decisionali#Definizione|decisore]], ovvero una Macchina di Turing $M$ con due possibili risultati di una computazione (accettazione e rifiuto), tale che $M$ accetta $w$ se e solo se $w\in L$. In particolare, $M$ riconosce $L$ e quindi $L$ è [[023 Linguaggio Turing-Riconoscibile|Turing-Riconoscibile]]. Inoltre abbiamo provato che [[#^ebcbb6|il complemento del linguaggio è decidibile]]. Con lo stesso ragionamento concludiamo che $\overline{L}$ è Turing-Riconoscibile.

#### 2. $L$ e $\overline{L}$ sono Turing-Riconoscibili

Supponiamo che $L$ e il suo complemento siano entrambi Turing-Riconoscibili. Sia $M_1$ una Macchina di Turing che riconosce $L$, e sia $M_2$ una Macchina di Turing che riconosce $\overline{L}$. Definiamo una Macchina di Turing $M$.

> [!note] Descrizione di $M$
> Su input $w$:
>
> 1. Esegue sia $M_1$ che $M_2$ su input $w$ in parallelo
> 2. ➜ Se $M_1$ accetta, allora accetta
>    ➜ Se $M_2$ accetta, allora rifiuta

Questa è una Macchina di Turing a [[026 Macchina di Turing Multinastro|due nastri]] che, dopo aver copiato $w$ sul secondo nastro, simula $M_1$ sul primo nastro ed $M_2$ sul secondo nastro.
Quindi, $M$ _alterna_ la simulazione di un passo di $M_1$ con un passo di $M_2$ e _continua finché una delle due accetta_.

Vogliamo provare che $M$ decide $L$, per fare questo dobbiamo provare due cose:

-   _$M$ è un decisore_. Infatti, per ogni stringa $w$ abbiamo due casi: $w\in L$ oppure $w\in\overline{L}$. Pertanto, una tra $M_1$ ed $M_2$ deve accettare $w$. Poiché $M$ si ferma ogni volta che $M_1$ accetta oppure $M_2$ accetta, allora _$M$ si ferma sempre_, quindi è un decisore.
-   $\color{#61AFEF}\mathcal{L}(M)=L$:

    1.  $w\in L$ se e solo se $M_1$ accetta $w$. Quindi $M$ accetta $w$. $$w\in L\Rightarrow M\text{ accetta }w$$
    2.  $w\not\in L$ se e solo se $M_2$ accetta $w$. Quindi $M$ rifiuta $w$. $$w\not\in L\Rightarrow M\text{ non accetta }w$$

    Siccome $M$ accetta $w$ se e solo se $w\in L$, possiamo concludere che $\mathcal{L}(M)=L$.

> [!note] Nota
> L'implicazione $w\not\in L\Rightarrow M\text{ non accetta }w$ è logicamente equivalente a $M\text{ accetta }w \Rightarrow w\in L$.

### $\overline{A_{TM}}$ Non è Turing-Riconoscibile (4.23)

Supponiamo per assurdo che $\overline{A_{TM}}$ sia Turing-Riconoscibile.
Quindi $\overline{A_{TM}}$ sarebbe Turing-Riconoscibile e Co-Turing-Riconoscibile.

Per il [[042 Linguaggi Indecidibili#Dimostrazione che $A_{TM}$ è Turing-Riconoscibile|precedente teorema]], $\overline{A_{TM}}$ sarebbe decidibile. Il che è assurdo, poiché abbiamo dimostrato che $\overline{A_{TM}}$ [[042 Linguaggi Indecidibili#Dimostrazione che $A_{TM}$ non è decidibile|è indecidibile]].

#

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
