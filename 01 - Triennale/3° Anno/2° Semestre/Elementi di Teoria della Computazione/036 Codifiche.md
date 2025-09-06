---
aliases:
  - Codifica di una Macchina di Turing
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Problemi di decisione
cssclasses:
  - 
---
Dato che si usa una [[035 Macchine di Turing e Algoritmi#^5f99d5|descrizione ad alto livello]] delle Macchine di Turing, l'input di queste devono essere *sempre delle stringhe*. Se vogliamo dare in input alla macchina *un tipo di oggetto diverso da una stringa* ([[033 Istanze di un Problema di Decisione|istanze]] di un problema), si deve prima **rappresentare tale oggetto come una stringa** su un alfabeto.

Le stringhe possono rappresentare facilmente polinomi, [[012 Grafi|grafi]], grammatiche, [[004 Automi Finiti Deterministici|automi]] e qualsiasi combinazione di questi oggetti.

## Codifica
>Per denotare la *stringa univoca* che **Codifica** l'oggetto $O$ si usa $$\color{#CC241D}\langle O \rangle$$
>Invece, per denotare la *singola stringa univoca* che codifica *vari oggetti* $O_1,\dots,O_k$ si usa $$\color{#CC241D}\langle O_1,\dots,O_k \rangle$$

> [!example]- <font color="orange">Esempio</font>
>Possiamo scegliere come codifica di un numero intero non negativo la sua rappresentazione binaria.
>$$\langle 4 \rangle = 100,\quad \langle 7 \rangle=111$$
>
>---
>>Dato un grafo $G$, questo è connesso?
>
>In questo problema le istanze sono i grafi, quindi $\langle G \rangle$ rappresenta una codifica di un grafo $G$.
>
>Consideriamo il grafo $G = (\{1, 2, 3\}, \{(1, 2), (2, 3), (3, 1)\})$.
>
>Con $\Sigma=\{0,1,(,),\#\}$ associamo a $G$ la stringa $$\langle G \rangle=(1\#10\#11)\#((1\#10)\#(10\#11)\#(11\#1))$$

## Codifica di una Macchina di Turing
È possibile codificare una Macchina di Turing $M$ sotto forma di una stringa. È anche possibile codificare *sia $M$ che una stringa $w$* attraverso una stringa.

Utilizziamo la funzione di **encoding $e$** per codificare un oggetto in una stringa.

Una possibile codifica di una Macchina di Turing $M=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ mediante una stringa sull'alfabeto $\{0,1\}$ è la seguente:
- Fissiamo un *alfabeto infinito universale* $\Sigma_U=\{a_1,a_2,\dots\}$ e assumiamo che tutti i simboli input e di nastro siano estratti da $\Sigma_U$
- Fissiamo un *insieme finito universale di stati* $Q_U=\{q_0,q_1,\dots\}$ e assumiamo che tutte le Macchine di Turing usano nomi di stati scelti da $Q_U$
- Codifichiamo un *elemento* $a_i\in\Sigma_U$ con la stringa $e(a_i)=0^{i+1}$, dove $e(\sqcup)=0$
- Codifichiamo un alfabeto $\Delta=\{b_1,b_2,\dots,b_r\}$ mediante la stringa $$e(\Delta)=111e(b_1)\ 1\ e(b_2)\ 1\ \dots\ 1\ e(b_r)111$$In questo modo $e(\Sigma)$ ed $e(\Gamma)$ sono definiti.
- Codifichiamo un elemento $q_i\in Q_U$ con la stringa $e(q_i)=0^{i+1}$
- Codifichiamo i *movimenti* della testina con $$e(L)=0,e(R)=00,e(S)=000$$
- Codifichiamo una *transizione* $m$ di $M$, ad esempio $\delta(q,a)=(p,b,D)$, con la stringa $$e(m)=11e(q)\ 1\ e(a)\ 1\ e(p)\ 1\ e(D)11$$
- Codifichiamo *l'intera Macchina di Turing* $M$ con transizione $m_1,\dots,m_t$ mediante la stringa $$\color{#CC241D}\langle M \rangle=11111e(q_0)\ 1\ e(q_{accept})\ 1\ e(q_{reject})\ 1\ e(\Sigma)\ 1\ e(\Gamma)\ 1\ e(m_1)\ 1\ \dots\ 1\ e(m_t)11111$$
- Codifichiamo una *stringa* $w=b_1b_2\dots b_r$ mediante $$\color{#CC241D}e(w)=11e(b_1)\ 1\ e(b_2)\ 1\ \dots\ 1\ e(b_r)11$$
- Codifichiamo una *Macchina di Turing* $M$ ed una *stringa di input* $w$ con la stringa $$\color{#00C575}\langle M,w \rangle=e(M)e(w)$$





___
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