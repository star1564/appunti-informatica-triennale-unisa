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
> -   $E_{TM}=\{\langle M \rangle\ |\ M \text{ è una MdT e } \mathcal{L}(M)=\emptyset\}$
>     -   L'insieme di tutte le descrizioni di macchine di Turing che hanno linguaggi vuoti
>     -   Indecidibile.
> -   $EQ_{TM}=\{\langle M_1,M_2 \rangle\ |\ M_1,M_2 \text{ sono MdT e } \mathcal{L}(M_1)=\mathcal{L}(M_2)\}$
>     -   L'insieme di tutte le descrizioni di due Macchina di Turing che hanno lo stesso linguaggio


## Dimostrazione Riducibilità con $E_{TM}$

> [!quote] Teorema
> $$E_{TM}\leq_m EQ_{TM}$$
> Ovvero che $E_{TM}$ è [[045 Riducibilità|riducibile]] in $EQ_{TM}$.

Sia $M_1$ una Macchina di Turing tale che $\mathcal{L}(M_1)=\emptyset$. Quindi, data una Macchina di Turing $M$, avremo $\mathcal{L}(M)=\mathcal{L}(M_1)$ se e solo se $\mathcal{L}(M)=\emptyset$.

La funzione $f$, che associa a $\langle M \rangle$ la stringa $\langle M,M_1 \rangle$, è una riduzione da $E_{TM}$ in $EQ_{TM}$.
Infatti, $f$ è calcolabile: possiamo costruire una Macchina di Turing $F$ che sull'input $\langle M \rangle$ fornisce in output $\langle M,M_1 \rangle$.

Inoltre: $$\textcolor{#61AFEF}{\langle M \rangle\in E_{TM}} \Leftrightarrow \mathcal{L}(M)= \emptyset \Leftrightarrow \mathcal{L}(M)=\mathcal{L}(M_1)\Leftrightarrow \textcolor{#61AFEF}{\langle M,M_1 \rangle \in EQ_{TM}}$$

### Teorema Indecidibilità

> [!quote] Teorema
> $EQ_{TM}=\{\langle M_1,M_2 \rangle\ |\ M_1,M_2 \text{ sono MdT e } \mathcal{L}(M_1)=\mathcal{L}(M_2)\}$ è [[042 Linguaggi Indecidibili|indecidibile]].


## $EQ_{TM}$ non è né Turing-Riconoscibile né Co-Turing-Riconoscibile

Le riduzioni seguenti utilizzano l'osservazione che per ogni linguaggio $X$, il linguaggio $X^*$ è sempre diverso dall'insieme vuoto. In particolare, per ogni alfabeto $\Sigma$, l'insieme $\Sigma^*$ delle stringhe su $\Sigma$ è sempre diverso dall'insieme vuoto.

### $A_{TM} \leq_m EQ_{TM}$

Data $\langle M,w \rangle$, considerare una Macchina di Turing $M_1$ che riconosce $\Sigma^*$ e una macchina $M_2$ che riconosce $\Sigma^*$ se $M$ accetta $w$.

Per ogni input $x$:

-   $M_1$ accetta $x$
-   $M_2$ simula $M$ su $w$. Se $M$ accetta, $M_2$ accetta

Quindi $\mathcal{L}(M_1)=\Sigma^*$ e $$\mathcal{L}(M_2)=\begin{cases} \Sigma^*,\quad\text{ se }\langle M,w \rangle\in A_{TM} \\ \emptyset,\quad\quad \text{altrimenti} \end{cases}$$
Di conseguenza, $\mathcal{L}(M_1)=\mathcal{L}(M_2)$ se e solo se $M$ accetta $w$.

Consideriamo la seguente Macchina di Turing $G$.

> [!note] Descrizione di $G$
> Sull'input $\langle M,w \rangle$:
>
> 1. Costruisce le seguenti due macchine $M_1$ e $M_2$:
>    $M_1=$ Su ogni input:
>
>     1. Accetta
>
>     $M_2=$ Su ogni input:
>
>     1. Esegue $M$ su $w$
>     2. Se $M$ accetta, accetta
>
> 2. Restituisce $\langle M_1,M_2 \rangle$

$G$ calcola una funzione $g$ che associa a $\langle M,w \rangle$ la stringa $\langle M_1,M_2 \rangle$. Questa è una riduzione da $A_{TM}$ in $EQ_{TM}$.
Infatti, $g$ è [[044 Funzioni Calcolabili|calcolabile]], inoltre $$\textcolor{#61AFEF}{\langle M, w \rangle\in A_{TM}}\Leftrightarrow M \text{ accetta }w \Leftrightarrow \mathcal{L}(M_1)=\Sigma^*=\mathcal{L}(M_2)\Leftrightarrow\color{#61AFEF}\langle M_1,M_2 \rangle\in EQ_{TM}$$

### $A_{TM} \leq_m \overline{EQ_{TM}}$

Data $\langle M,w \rangle$, considerare una Macchina di Turing $M_1$ che _riconosce l'insieme vuoto_ e una macchina $M_2$ che riconosce $\Sigma^*$ se $M$ accetta $w$.

Per ogni input $x$:

-   $M_1$ rifiuta $x$
-   $M_2$ simula $M$ su $w$. Se $M$ accetta, $M_2$ accetta

Quindi $\mathcal{L}(M_1)=\emptyset$ e $$\mathcal{L}(M_2)=\begin{cases} \Sigma^*,\quad\text{ se }\langle M,w \rangle\in A_{TM} \\ \emptyset,\quad\quad \text{altrimenti} \end{cases}$$
Di conseguenza, $\mathcal{L}(M_1)\neq \mathcal{L}(M_2)$ se e solo se $M$ accetta $w$.

Consideriamo la seguente Macchina di Turing $F$.

> [!note] Descrizione di $F$
> Sull'input $\langle M,w \rangle$:
>
> 1. Costruisce le seguenti due macchine $M_1$ e $M_2$:
>    $M_1=$ Su ogni input:
>
>     1. Rifiuta
>
>     $M_2=$ Su ogni input:
>
>     1. Esegue $M$ su $w$
>     2. Se $M$ accetta, accetta
>
> 2. Restituisce $\langle M_1,M_2 \rangle$

Da notare che $F$ _non deve eseguire_ le azioni che definiscono le Macchine di Turing $M_1,M_2$.

$F$ calcola una funzione $f$ che associa a $\langle M,w \rangle$ la stringa $\langle M_1,M_2 \rangle$. Questa è una riduzione da $A_{TM}$ in $\overline{EQ_{TM}}$.
Infatti, $f$ è [[044 Funzioni Calcolabili|calcolabile]], inoltre $$\textcolor{#61AFEF}{\langle M, w \rangle\in A_{TM}}\Leftrightarrow M \text{ accetta }w \Leftrightarrow \mathcal{L}(M_2)=\Sigma^*\neq \emptyset= \mathcal{L}(M_1)\Leftrightarrow\color{#61AFEF}\langle M_1,M_2 \rangle\in \overline{EQ_{TM}}$$

### Conclusione

> [!summary]- Teoremi e corollari necessari
> ![[045 Riducibilità#^55b02f]]
>
> ![[043 Linguaggi Co-Turing-Riconoscibili#$ overline{A_{TM}}$ Non è Turing-Riconoscibile (4.23)]]
>
> ![[045 Riducibilità#^a8c89d]]

> [!quote] Teorema
> $EQ_{TM}$ non è né [[023 Linguaggio Turing-Riconoscibile|Turing-Riconoscibile]] né [[043 Linguaggi Co-Turing-Riconoscibili|Co-Turing-Riconoscibile]].

Supponiamo per assurdo che $EQ_{TM}$ sia _Turing-Riconoscibile_:

-   Siccome $A_{TM}\leq_m \overline{EQ_{TM}}$, allora $\overline{A_{TM}}\leq_m EQ_{TM}$ (Teorema 1).
-   Quindi, per il Teorema 5.28, se $EQ_{TM}$ fosse Truing-Riconoscibile, allora $\overline{A_{TM}}$ sarebbe Turing-Riconoscibile, in contraddizione con il Corollario 4.23.

Supponiamo per assurdo che $EQ_{TM}$ sia _Co-Turing-Riconoscibile_, cioè che $\overline{EQ_{TM}}$ sia Turing-Riconoscibile:

-   Siccome $A_{TM}\leq_m EQ_{TM}$, allora $\overline{A_{TM}}\leq_m \overline{EQ_{TM}}$ (Teorema 1).
-   Quindi, per il Teorema 5.28, se $\overline{EQ_{TM}}$ fosse Truing-Riconoscibile, allora $\overline{A_{TM}}$ sarebbe Turing-Riconoscibile, in contraddizione con il Corollario 4.23.

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
