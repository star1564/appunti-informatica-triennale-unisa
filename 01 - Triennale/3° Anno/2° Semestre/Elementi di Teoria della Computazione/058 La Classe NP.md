---
aliases:
    - CLIQUE
    - SUBSET-SUM
    - HAMPATH
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Verificabilità in Tempo Polinomiale
cssclasses:
    - 
---

> La **Classe $NP$** è l'insieme dei _linguaggi [[057 Verificatore Polinomiale#Definizione|verificabili]] in tempo polinomiale_. Quindi è la classe di tutti i problemi per i quali, data una soluzione corretta, è possibile verificare che sia tale, in un tempo "ragionevole" (polinomiale).

> [!warning] La classe $NP$ **_NON È_** abbreviazione di "tempo non polinomiale"
> $NP$ è abbreviazione di "tempo polinomiale _non deterministico_". Deriva da una caratterizzazione equivalente di $NP$ che usa le [[029 Macchina di Turing Non Deterministica|macchine di Truing non deterministiche]] di [[054 Tempo Polinomiale ed Esponenziale|tempo polinomiale]].

> [!quote] Teorema 7.20
> Un linguaggio $L$ è in $NP$ se e solo se esiste una Macchina di Turing non deterministica che [[024 Macchine Decisionali|decide]] $L$ in tempo polinomiale.

^2de491

> [!quote] Classe di complessità di tempo non deterministico
> Sia $t:\mathbb{N}\to \mathbb{R}^+$ una funzione. La [[052 Classi complessità TIME|classe di complessità]] in **tempo non deterministico $NTIME$** è $$NTIME(t(n))=\{L\ |\ \text{esiste una macchina di Turing non deterministica che decide }L\text{ in tempo }O(t(n))\}$$

> [!quote] Corollario 7.22
> $$NP=\bigcup_{k\geq1}NTIME(n^k)$$
> Questa è una diretta conseguenza del [[#^2de491|Teorema 7.20]].


## Esempi di problemi in NP

> [!summary]+ A cosa appartiene ad N o a NP?
> **Informalmente**:
>
> -   In $P$ si trovano tutti quei problemi che sono _veloci da risolvere e da verificare_
>     -   Ad esempio, il problema della moltiplicazione $100\times5$, è veloce da risolvere, ed è veloce verificare che una soluzione che posso dare sia corretta o meno ($500$: si, $2$: no)
> -   In $NP$ si trovano tutti quei problemi che sono _difficili da risolvere_ ma _veloci da verificare_
>     -   Ad esempio, un sudoku con un miliardo di celle è difficile da risolvere anche per un computer, ma data una soluzione completa (anche se è da un miliardo di celle) è possibile verificare se questa sia corretta o meno in un tempo (relativamente) "ragionevole"

### CLIQUE

> Una **Clique** (cricca) in un [[012 Grafi#Grafi Non Diretti (Non Orientati)|grafo non orientato]] $G$ è un _sottografo_ di $G$ in cui _ogni coppia di vertici è connessa_ da un arco.
> Una **k-clique** è una clique che contiene $k$ vertici.

![[Pasted image 20240601155750.png]]

Il problema di stabilire se $G$ contiene una k-clique si può formulare come un problema di decisione, il cui linguaggio associato è $$CLIQUE = \{\langle G,k \rangle\ |\ G\text{ è un grafo non orientato in cui esiste una k-clique}\}$$

> [!quote]+ Teorema CLIQUE in NP
> $$CLIQUE\in NP$$
>
> Un algoritmo $V$ che verifica $CLIQUE$ in tempo polinomiale è il seguente.
>
> > [!note] Descrizione di $V$
> > Sull'input $\langle \langle G,k \rangle, c \rangle$:
> >
> > 1. Verifica se $c$ è un insieme di $k$ nodi di $G$, altrimenti rifiuta.
> > 2. Verifica se per ogni coppia di nodi in $c$, esiste un arco in $G$ che li connette. Se esistono per ogni arco accetta, altrimenti rifiuta.
>
> $$\exists c: \langle \langle G,k \rangle, c \rangle \in \mathcal{L}(V)\iff \langle  G,k \rangle\in CLIQUE$$

^d1928b

### SUBSET-SUM

> Dato un insieme finito $S$ di numeri interi e un numero intero $t$, il problema del **subset-sum** dice se esiste un sottoinsieme $S'$ di $S$ tale che la somma dei suoi numeri sia uguale a $t$.

> [!note] Questo problema è simile al problema dello zaino

![[Pasted image 20240601155917.png|500]]

Il problema di stabilire se esiste questo sottoinsieme $S'$ dato $t$ si può formulare come un problema di decisione, il cui linguaggio associato è $$SUBSET\text{-}SUM = \{\langle S,t \rangle\ |\ S=\{x_1,\dots,x_k\}\text{ ed esiste }S'\subseteq S\text{ tale che }\displaystyle(\sum_{s\in S'} s)=t\}$$

> [!quote]+ Teorema SUBSET-SUM in NP
> $$SUBSET\text{-}SUM\in NP$$
>
> Un algoritmo $V$ che verifica $SUBSET\text{-}SUM$ in tempo polinomiale è il seguente.
>
> > [!note] Descrizione di $V$
> > Sull'input $\langle \langle S,t \rangle, c \rangle$:
> >
> > 1. Verifica se $c$ è un insieme di numeri la cui somma è $t$, altrimenti rifiuta.
> > 2. Verifica se $S$ contiene tutti i numeri in $c$. Se li contiene accetta, altrimenti rifiuta.
>
> $$\exists c: \langle \langle S,t \rangle, c \rangle \in \mathcal{L}(V) \iff \langle  S,t \rangle\in SUBSET\text{-}SUM$$

^481d51

### HAMPATH

> [!tip]- Come funziona $HAMPATH$
> ![[057 Verificatore Polinomiale#HAMPATH]]

> [!quote]+ Teorema HAMPATH in NP
> $$HAMPATH\in NP$$
>
> Un algoritmo $V$ che verifica $HAMPATH$ in tempo polinomiale è il seguente.
>
> > [!note] Descrizione di $V$
> > Sull'input $\langle \langle G,s,t \rangle, c \rangle$, dove $G=(V,E)$ è un grafo orientato:
> >
> > 1. Verifica se $c=(u_1,\dots,u_{|V|})$ è una sequenza di $|V|$ vertici di $G$, altrimenti rifiuta.
> > 2. Verifica se:
> >
> >     - i nodi della sequenza $c$ sono distinti
> >     - $u_1=s$
> >     - $u_{|V|}=t$
> >     - per ogni $i$ con $2\leq i\leq n$, $(u_{i-1}, u_i)\in E$
> >
> >     Accetta in caso affermativo, altrimenti rifiuta.
>
> $$\exists c: \langle \langle G,s,t \rangle, c \rangle \in \mathcal{L}(V) \iff \langle  G,s,t \rangle\in HAMPATH$$

^9730bd

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
