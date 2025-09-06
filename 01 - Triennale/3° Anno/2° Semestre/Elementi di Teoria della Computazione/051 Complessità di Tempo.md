---
aliases:
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Complessità di Tempo
cssclasses:
    - 
---

Anche quando un [[037 Linguaggio associato ad un Problema di Decisione#^3c83c4|problema è decidibile]], e quindi computazionalmente risolvibile, _può non essere risolvibile in pratica_ se la soluzione richiede una _quantità eccessiva di tempo o memoria_.

Si studieranno [[024 Macchine Decisionali#Linguaggio Decidibile|linguaggi decidibili]] e [[024 Macchine Decisionali#Definizione|decider]] dal punto di vista della _quantità di risorse di calcolo utilizzate nella computazione_. In questo caso consideriamo il **tempo** utilizzato.

-   Il **numero di passi** che utilizza un algoritmo su un particolare input _può dipendere da diversi parametri_.
-   Il **tempo di esecuzione** dell'algoritmo sarà calcolato in funzione della _lunghezza della stringa che rappresenta l'input_ senza considerare eventuali altri parametri.

> [!example]- <font color="orange">Esempio</font>
> Se l'input è un [[012 Grafi|grafo]] $G$:
>
> -   il numero di passi può dipendere dal numero di nodi, dal numero di archi e dal grado massimo del grafo, o da una combinazione di questi e/o altri parametri.
> -   il tempo di esecuzione di un algoritmo sui grafi sarà calcolato in funzione della lunghezza della codifica $\langle G \rangle$ di $G$.

Si considera sempre l'analisi del [[007 Caso Peggiore|caso peggiore]], valutando il tempo di esecuzione massimo tra tutti gli input di una determinata lunghezza.


## Definizione

Sia $M=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ un decisore, ovvero una Macchina di Turing a nastro singolo che si arresta su ogni input.

> La **Complessità di Tempo** (_tempo di esecuzione_) di $M$ è la [[011 Funzioni|funzione]] $f:\mathbb{N}\to\mathbb{N}$, dove $f(n)$ è il _massimo numero di [[021 Configurazione di una Macchina di Turing#Passo di Computazione|passi di computazione]] eseguiti_ da $M$ su un input di lunghezza $n\in\mathbb{N}$.
>
> Se $M$ ha complessità di tempo $f(n)$, si dirà che _$M$ decide $\mathcal{L}(M)$ in tempo deterministico $f(n)$_.

> [!tip]+ Complessità con le Configurazioni
> Se $C_1,C_2,\dots,C_{k+1}$ con $k\ge 1$ sono [[021 Configurazione di una Macchina di Turing|configurazioni]] di $M$, tali che:
>
> 1.  $C_1=q_0\ w$ è la [[021 Configurazione di una Macchina di Turing#Tipi di Configurazioni|Configurazione Iniziale]] di $M$ con input $w$
> 2.  $C_i\to C_{i+1}, \forall i\in\{1,\dots,k\}$
> 3.  $C_{k+1}$ è una [[021 Configurazione di una Macchina di Turing#Tipi di Configurazioni|Configurazione di Arresto]]
>
> Allora il numero di passi eseguiti da $M$ su $w$ è $k$.
>
> Quindi, se $f$ è la complessità di tempo di $M$, $f(n)$ è il massimo numero di passi in $$q_0\ w\to^* u\ q\ v$$con $q\in\{q_{accept},q_{reject}\}$, al variare di $w$ in $\Sigma^n$.

Non si valuterà esattamente la complessità di tempo $f(n)$ di $M$, ma si stabilirà una [[002 Complessità Computazionale#COMPORTAMENTO ASINTOTICO|limite asintotico]] superiore per $f(n)$ attraverso la notazione [[003 Notazione O|O-grande]].

> [!example]- <font color="orange">Esempio</font>
> La complessità di tempo della macchina di Turing $M$ che:
>
> 1.  Rifiuta se l'input è la parola vuota
> 2.  Cancella il primo carattere dell'input e accetta, se l'input è una stringa non vuota.
>
> è $O(1)$, ovvero tempo costante.

> [!example]- <font color="orange">Esempio</font> 
> $$L=\{0^k1^k\ |\ k\ge 0\}$$
>
> ![[Pasted image 20240525121452.png|700]]
>
> > [!note] Descrizione di $M$
> > Sull'input $w$, la Macchina di Turing $M$:
> >
> > 1. Verifica che $w\in \mathcal{L}(0^*1^*)$
> > 2. Partendo dallo $0$ più a sinistra, sostituisce $0$ con $\sqcup$ poi va a destra, ignorando $0$ e $1$, fino ad incontrare $\sqcup$.
> > 3. Verifica che immediatamente a sinistra di $\sqcup$ ci sia $1$:
> >     - Se non è così, $w$ viene rifiutata
> >     - Altrimenti, sostituisce $1$ con $\sqcup$ e va a sinistra fino ad incontrare $\sqcup$
> > 4. Guarda il simbolo a destra di $\sqcup$:
> >     - Se è un altro $\sqcup$, allora $w$ viene accettata.
> >     - Se è $1$, allora $w$ viene rifiutata.
> >     - Se è $0$, allora ripete a partire dal passo 2.
>
> Sia $n=|w|$, ovvero utilizziamo $n$ per rappresentare la lunghezza dell'input.
> Per analizzare $M$, consideriamo ciascuna delle sue quattro fasi separatamente.
>
> -   Nella prima fase, la macchina scansiona il nastro per verificare che l'input è in $\mathcal{L}(0^*1^*)$, ovvero del tipo $0^t1^q$, con $t,q\in\mathbb{N}$. Tale operazione di scansione usa $n$ passi. Per riportare la testina all'estremità sinistra del nastro utilizza ulteriori $n$ passi. Per cui, il totale di passi utilizzati in questa fase è $2n$ passi, ovvero $O(n)$ passi.
> -   Le fasi 2 e 3 richiedono ciascuna $O(n)$ passi e sono eseguite al massimo $\frac{n}{2}$ volte. Infatti, la macchina esegue ripetutamente la scansione del nastro e cancella uno $0$ e un $1$ ad ogni scansione. Ogni scansione utilizza $O(n)$ passi. Poiché ogni scansione elimina due simboli, possono verificarsi al massimo $\frac{n}{2}$ scansioni. Poi la macchina accetta o rifiuta.
>
> Quindi $M$ decide $L$ in tempo $$O(n)+\frac{n}{2}O(n)=O(n)+O(n^2)=O(n^2)$$

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
