---
aliases: 
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Riducibilità
cssclasses:
  - 
---
Una **proprietà $\mathcal{P}$ di un linguaggio** è essenzialmente *un insieme di linguaggi*. Diciamo che il linguaggio $A$ soddisfa la proprietà $\mathcal{P}$ se $A\in \mathcal{P}$.



> Sia $X=\{\langle M \rangle\ |\ M \text{ è una MdT che verifica una proprietà } \mathcal{P}\}$ un linguaggio che soddisfa le seguenti tre condizioni:
>
> 1.  Prese comunque due Macchine di Turing $M_1,M_2$ tali che _hanno lo stesso linguaggio_, ovvero che $\color{#61AFEF}\mathcal{L}(M_1)=\mathcal{L}(M_2)$, risulta che
>     $$\color{#CC241D}\langle M_1 \rangle \in X \Leftrightarrow \langle M_2 \rangle \in X$$
> 2.  Esiste una Macchina di Turing $M_1$ tale che $\color{#CC241D}\langle M_1 \rangle\in X$
> 3.  Esiste una Macchina di Turing $M_2$ tale che $\color{#CC241D}\langle M_2 \rangle\notin X$
>
> Allora $X$ è [[042 Linguaggi Indecidibili|indecidibile]].


> [!tip] Determinare se il teorema può essere applicato
>1. Verificare se il linguaggio descrive una ***proprietà*** della Macchina di Turing:
>	- Se la proprietà riguarda il *comportamento (semantica)* della Macchina di Turing, allora il Teorema di Rice si può applicare ✔️
>		- Ad esempio: "la Macchina di Turing accetta un certo linguaggio e ne rifiuta un altro", ...
>	- Se la proprietà riguarda la *forma o la struttura (sintassi)* della Macchina di Turing, allora il Teorema di Rice si NON può applicare ❌
>		- Ad esempio: "la Macchina di Turing ha 100 stati", "la Macchina di Turing ha un numero pari di stati", ...
>
>2. Controlla se la proprietà è ***non banale***: 
>	- Una proprietà è considerata **non banale** se *è vera per alcune Macchina di Turing e falsa per altre*
>		- Ad esempio: "la Macchina di Turing accetta almeno 37 stringhe"
>	- Una proprietà si dice **banale** se *è soddisfatta da tutte le Macchina di Turing o da nessuna*
>		- Se una proprietà *è banale, allora è sempre decidibile*, in quanto si sa già in anticipo la decisione da prendere [^1]
>	- Se è *non banale*, il Teorema di Rice si si può applicare ✔️
>	- Altrimenti, non si può applicare ❌
>
>Se la proprietà **soddisfa tutti questi criteri**, allora si può concludere che il Teorema di Rice è applicabile.

[^1]: [Gate CSE](https://gatecse.in/rices-theorem/#Part_1_.28For_some_undecidable_languages.29)

> [!example]- <font color="orange">Esempio</font> 
>$$X=\{\langle M \rangle\ |\ M \text{ è una MdT che rifiuta } ab \text{ ed accetta le stringhe che terminano con } a\}$$
>
> Il teorema di Rice _non è applicabile_ a $X$:
>
> -   Sia $D_{rab}$ un decider che: 
> 	- Rifiuta l'input $ab$ 
> 	- Accetta le stringhe che terminano con $a$, ovvero $\{wa\ |\ w\in\Sigma^*\}$
> -   Sia $M_{cab}$ una Macchina di Turing che:
> 	- Cicla all'infinito con input $ab$
> 	- Accetta le stringhe che terminano con $a$, ovvero $\{wa\ |\ w\in\Sigma^*\}$
>
> Risulta che $\mathcal{L}(M_{cab})=\mathcal{L}(D_{rab})=\{wa\ |\ w\in\Sigma^*\}$, però $\langle M_{cab} \rangle\notin X$, mentre $\langle D_{rab} \rangle\in X$. Questo perché $M_{cab}$ non rifiuta l'input $ab$, in quanto va in un ciclo infinito.
>
> ---
>
> $$Y=\{\langle M \rangle\ |\ M \text{ è una MdT che accetta un linguaggio finito}\}$$
>
> È possibile utilizzare il teorema di Rice per mostrare che $Y$ è indecidibile.
>
> 1. Siano $M_1,M_2$ Macchine di Turing tali che $\mathcal{L}(M_1)=\mathcal{L}(M_2)$. Allora
> $$\langle M_1 \rangle\in Y\iff \mathcal{L}(M_1) \text{ è un insieme finito} \iff \mathcal{L}(M_2) \text{ è un insieme finito} \iff \langle M_2 \rangle\in Y$$
>
> 2.  Sia $M_a$ la Macchina di Turing che decide il linguaggio $\{a\}$ (insieme finito). Risulta che $\langle M_a \rangle\in Y$.
> 3.  Sia $M_{all}$ la Macchina di Turing che decide $\{a,b\}^*$ (insieme infinito). Risulta che $\langle M_{all} \rangle\notin Y$.
>
> Quindi, per il teorema di Rice, $Y$ è indecidibile.
>
> ---
>
> $$Z=\{\langle M \rangle\ |\ M \text{ è una MdT che ha un solo stato}\}$$
>
> Il teorema di Rice _non è applicabile_ a $Z$.
>
> Infatti, ogni macchina di Turing ha almeno due stati, poiché il suo insieme di stati contiene sempre $q_{accept}, q_{reject}$ con $q_{accept}\neq q_{reject}$.
>
> Risulta che $\mathcal{L}(Z)=\emptyset$.

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
