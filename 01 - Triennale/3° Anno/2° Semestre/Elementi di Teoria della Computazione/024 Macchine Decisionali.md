---
aliases:
    - Decider
    - Decisore
    - Linguaggio Decidibile
    - Linguaggio Deciso
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Macchine di Turing
cssclasses:
    - 
---

Data una [[021 Configurazione di una Macchina di Turing|configurazione]] di una Macchina di Turing che non è di arresto, _non possiamo sapere_ se, a partire da tale configurazione, la computazione terminerà in qualche istante futuro o meno. Infatti, distinguere una macchina che è entrata in un loop da una che sta semplicemente impiegando molto tempo risulta difficile.


## Definizione

> Si utilizzano spesso i **decisori** (_decider_), ovvero delle Macchina di Turing che _terminano la computazione su ogni singolo input_, andando ad accettare o rifiutare l'input.

> [!quote] Definizione Formale
> Una Macchina di Turing $M=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ è un **decisore** se, per ogni $w\in\Sigma^*$, esistono $u,v\in\Gamma^*$ e $q\in\{q_{accept},q_{reject}\}$ tali che $$q_0\ w\to^* u\ q\ v$$
> Ovvero che terminano la propria computazione con una configurazione di arresto.

^63f682

Siano $\mathcal{L}(M)=\{w\in\Sigma^*\ |\ M \text{ accetta } w\}$ e $R(M)=\{w\in\Sigma^*\ |\ M \text{ rifiuta } w\}$.

Di solito, $\mathcal{L}(M)\cup R(M)$ **non coincide** con $\Sigma^*$, perché ci sono anche le parole per cui la computazione non termina (_$R$ contiene solo quelle che sono andati in uno stato di rifiuto_).

![[019 Funzione di Transizione delle Macchine di Turing#^d05899]]

![[Pasted image 20240429163445.png|600]]

Invece, se $\mathcal{L}(M)\cup R(M)$ **coincide** con $\Sigma^*$, allora $M$ _si arresta su ogni input_ ed è quindi un decisore.

![[Pasted image 20240429164011.png|600]]

> [!note]+ Nota
> Dato un decisore $S$ ed una stringa $w$, $S(w)$ rappresenta il risultato della computazione di $S$ sull'input $w$ (accettato o rifiutato).

^8e9c30


## Linguaggio Decidibile

> Un linguaggio $L$ si dice **Decidibile** se esiste una Macchina di Turing $M$ tale che:
>
> 1.  _$M$ riconosce $L$_, ovvero che $L$ è _[[023 Linguaggio Turing-Riconoscibile#^5b96ec|Turing-Riconoscibile]]_
> 2.  _$M$ si arresta **su ogni** input_, ovvero che $M$ è un _[[#Definizione|Decisore]]_

^37591d

Come conseguenza delle definizioni, un linguaggio $L$ è Turing-riconoscibile ma non decidibile se:

1. Esiste una Macchina di Turing che riconosce $L$ (quindi accetta tutte e sole le stringhe di $L$)
2. Non esiste nessuna Macchina di Turing $M$ tale che $M$ _accetta_ tutte le stringhe in $L$ e _rifiuta_ tutte le stringhe che appartengono al complemento $\overline{L}$ di $L$.

### Linguaggio Deciso

Se $L$ è decidibile, allora $L$ è **Deciso** da una macchina $M$. ^ba5ab0

### Proprietà

> [!note]- P1
> ![[043 Linguaggi Co-Turing-Riconoscibili#La classe dei linguaggi decidibili è chiusa rispetto al complemento]]

> [!note]- P2
> ![[043 Linguaggi Co-Turing-Riconoscibili#Un linguaggio $L$ è decidibile se e solo se $L$ è Turing-Riconoscibile e Co-Turing-Riconoscibile]]

---

> [!question]- Ogni linguaggio regolare è Turing riconoscibile?
> Testo.

> [!question]- Ogni linguaggio regolare è decidibile?
> Ogni linguaggio regolare è Turing-decidibile e quindi Turing riconoscibile.
>
> Si supponga di avere un [[004 Automi Finiti Deterministici|DFA]] $D$ tale che $L = \mathcal{L}(D)$. Si può costruire una Macchina di Turing $M$ che simuli $D$. Gli stati di $M$ saranno simili a quelli di $D$. Al termine dell'input, se $M$ si trova in uno stato che corrisponde a uno stato finale di $D$, $M$ si ferma e accetta; altrimenti si ferma e rifiuta. [^1]

> [!question]- Ogni linguaggio decidibile è regolare?
> Testo.

> [!question]- Ogni linguaggio Turing riconoscibile è regolare?
> Testo.

[^1]: [Stack Exchange](https://cs.stackexchange.com/questions/10971/is-every-regular-language-turing-decidable-and-how-can-we-prove-this)

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

-   [[Terminologie Linguaggi MdT.canvas|Canvas dei linguaggi delle MdT]]
