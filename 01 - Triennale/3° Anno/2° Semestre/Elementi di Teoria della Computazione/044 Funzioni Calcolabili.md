---
aliases:
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Riducibilità
cssclasses:
    - 
---

> Una [[011 Funzioni|funzione]] $f : \Sigma^* \to \Sigma^*$ è chiamata **Funzione Calcolabile** se esiste una Macchina di Turing $M$ tale che, per ogni input $w$, $M$ [[024 Macchine Decisionali|si arresta]] _lasciando sul suo nastro esattamente la stringa $f(w)$_.

Questo implica che la macchina di Turing può determinare il risultato della funzione $f$ per ogni possibile input in un tempo finito.

> [!example]- <font color="orange">Esempio</font>
> Le seguenti funzioni aritmetiche sono calcolabili (dove $n,m\in \mathbb{N}$):
>
> -   $incr(n)=n+1$
> -   $decr(n)=\begin{cases} n-1,\text{ se } n>0 \\ 0,\text{ se }n =0 \end{cases}$
> -   $(m,n)\to m+n$
> -   $(m,n)\to m-n$
> -   $(m,n)\to m\cdot n$

Le funzioni possono essere anche trasformazioni di [[036 Codifiche#Codifica di una Macchina di Turing|codifiche di Macchine di Turing]].
Stabilire se funzioni di questo tipo sono calcolabili può non essere facile, soprattutto se sono trasformazioni di codifiche di macchine di Turing descritte ad alto livello.

> [!example]+ <font color="orange">Esempio</font>
> Data una Macchina di Turing $M=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$, in questo esempio denotiamo con $M'$ la Macchina di Turing che:
>
> -   Accetta le stringhe rifiutate da $M$
> -   Rifiuta le stringhe accettate da $M$
>
> In generale, $M'$ _non riconosce il complemento_ di $\mathcal{L}(M)$.
>
> > [!note] Descrizione di $M'$
> > Sull'input $x$:
> >
> > 1. Simula $M$ su $x$
> > 2. ➜ Se $M$ accetta, rifiuta
> >    ➜ Se $M$ rifiuta, accetta
>
> Consideriamo la funzione $f:\Sigma^*\to\Sigma^*$ così definita:
> $$f(y)=\begin{cases} \epsilon,\quad \quad  \text{ se } y\neq \langle M \rangle \\ \langle M' \rangle,\quad  \text{se } y= \langle M \rangle \end{cases}$$
>
> Consideriamo la Macchina di Turing $F$.
>
> > [!note] Descrizione di $F$
> > Sull'input $y$:
> >
> > 1. Se $y\neq \langle M \rangle$, restituisce $\epsilon$
> > 2. Se $y= \langle M \rangle$, "costruisce" la Macchina di Turing $M'$ definita come sopra
> > 3. Fornisce in output $\langle M' \rangle$
>
> Chiamiamo $\delta$ la [[019 Funzione di Transizione delle Macchine di Turing|funzione di transizione]] di $M$ e $\delta'$ quella della macchina $M'$ che sarà costruita.
>
> $F$ inizia la sua computazione scorrendo l'input per verificare se è "legale", ovvero se è la codifica di una Macchina di Turing. Se non lo è, cancella l'input e si ferma.
>
> Se l'input è la codifica di una Macchina di Turing, $F$ scorre nuovamente l'input ed effettua le seguenti operazioni:
>
> 1.  Cerca nella codifica di $M$ la codifica delle transizioni della forma $\delta(q,a)=(q_{accept},a',D)$, dove $D\in\{L,R\}$, $a,a'\in\Gamma$, $q\in Q$
> 2.  Cambia ognuna di queste codifiche di transizioni con la codifica di $\delta'(q,a)=(q_{reject},a',D)$, ovvero trasforma tutte le transizioni che portano ad uno stato di accettazione facendole portare ad uno stato di rifiuto
> 3.  Fa un'azione analoga sulla codifica delle transizioni della forma $\delta(q,a)=(q_{reject},a',D)$, che cambia con la codifica di $\delta'(q,a)=(q_{accept},a',D)$, ovvero trasforma tutte le transizioni che portano ad uno stato di rifiuto facendole portare ad uno stato di accettazione
> 4.  Lascia le altre transizioni inalterate e si ferma
>
> La Macchina di Turing $F$ esiste, perché deve solo scorrere l'input, verificare se questo è "legale" e cambiare dei caratteri in esso contenuti.
> La stringa in output di $F$ è la codifica $\langle M' \rangle$ della Macchina di Turing $M'$. Tutto questo _senza eseguire o simulare $M$_.

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
