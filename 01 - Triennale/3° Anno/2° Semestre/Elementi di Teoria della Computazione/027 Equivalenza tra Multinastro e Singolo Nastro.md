---
aliases:
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Varianti delle Macchine di Turing
cssclasses:
    - 
---

> Per ogni Macchina di Turing [[026 Macchina di Turing Multinastro|Multinastro]] $M=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ esiste una Macchina di Turing [[018 Macchine di Turing|a singolo nastro]] $S$ **equivalente** ad $M$, cioè tale che entrambe le macchine [[023 Linguaggio Turing-Riconoscibile|riconoscano lo stesso linguaggio]].
> $$\mathcal{L}(M)=\mathcal{L}(S)$$

> [!tip]- Macchine equivalenti
> ![[025 Varianti delle Macchine di Turing#^7ce7c4]]


## Dimostrazione

Proveremo che esiste una Macchina di Turing a nastro singolo $S=(Q',\Sigma, \Gamma',\delta',q_0,q_{accept}, q_{reject})$ equivalente ad $M$.

> [!note] Contenuto del nastro
> È la stringa dei simboli che restano sul nastro dopo aver eliminato tutti i simboli $\sqcup$ che seguono l'ultimo carattere del nastro diverso da $\sqcup$. Ovvero la "parte finita" del nastro.

### Idea

L'idea chiave è quella di mostrare come **simulare** $M$ con $S$. Supponiamo che $M$ abbia $k$ nastri.

Per ogni [[021 Configurazione di una Macchina di Turing|configurazione]] di $M$ $$(u_1\ q\ v_1, u_2\ q\ v_2, \dots, u_k\ q\ v_k)$$
la macchina $S$ deve simulare l'effetto di $k$ nastri memorizzando le loro informazioni sul suo singolo nastro.

### Conservare le informazioni in $S$

> Il _contenuto_ $u_1v_1,\dots,u_kv_k$ dei $k$ nastri verrà conservato dalla macchina $S$ attraverso il nuovo simbolo $\#$, utilizzato come **delimitatore** per _separare i contenuti dei diversi nastri_.

Ogni blocco corrisponderà ad un nastro di $M$ e avrà una lunghezza variabile che dipende dal contenuto del nastro corrispondente.

Il simbolo $\#$ viene aggiunto all'alfabeto del nastro.

> Invece, la _posizione di ciascuna testina_ su ogni nastro verrà tracciata attraverso un **punto al di sopra del simbolo** attuale per quel nastro. Ovviamente deve esistere uno ed un solo punto per ogni blocco di $S$.

I simboli del nastro "puntati" (o _testine virtuali_) sono aggiunti all'alfabeto del nastro.

![[Immagine 2024-05-06 102457.png|600]]

Quindi, ad una configurazione di $M$ $(u_1\ q\ v_1, u_2\ q\ v_2, \dots, u_k\ q\ v_k)$ corrisponderà la configurazione di $S$
$$\color{#CC241D}q'\ \#\ u_1\dot{a_1}v_1\ \#\ u_2\dot{a_2}v_2\ \#\ \dots\ \#\ u_k\dot{a_k}v_k\ \#$$

Inoltre, l'alfabeto dei simboli del nastro $S$ sarà
$$\color{#61AFEF}\Gamma\cup\{\dot{\gamma}\ |\ \gamma\in\Gamma\}\cup\{\#,\dot{\#}\}$$

### Simulare le Computazioni

Sia $w=w_1\dots w_n$ una stringa input e $w_t\in\Sigma^*$ con $t\in\{1,\dots,n\}$.

1. Inizialmente, $S$ **genera la configurazione iniziale** di $M$, passando dalla configurazione iniziale $q_0 w_1\dots w_n$ alla configurazione $$q'\ \#\ \dot{w_1}w_2\dots w_n\ \#\ \dot{\sqcup}\ \#\ \dots\ \#\ \dot{\sqcup}\ \#$$

2. Per simulare un [[021 Configurazione di una Macchina di Turing#Passo di Computazione|Passo di Computazione]] di $M$, per effetto della [[026 Macchina di Turing Multinastro#Funzione di transizione|funzione di transizione per le macchine multinastro]] $\delta(q, a_1,\dots, a_k)=(s,\ b_1,\dots,b_k,\ d_1,\dots,d_k)$, la macchina $S$ _scansiona_ il suo nastro dal primo $\#$, che segna _l'estremità a sinistra_, al $(k+1)$-esimo $\#$, che segna _l'estremità destra_, per **determinare i simboli puntati dalle testine virtuali**. Questo avviene _due volte_:
    - Da sinistra a destra - La prima volta $S$ determina quali sono i simboli correnti di $M$, ovvero $\dot{a_t}$, memorizzando nello stato i simboli marcati sui singoli nastri.
    - Da destra a sinistra - La seconda volta $S$ esegue su ogni blocco, demarcato da $\#$, le azioni che simulano quelle di $M$, ovvero la scrittura dei caratteri ed un eventuale spostamento delle testine.
3. Se in qualsiasi momento $S$ sposta una delle testine virtuali a destra su un $\#$ (e quindi scrive $\dot{\#}$), questa azione significa che $M$ ha spostato la testina del corrispondente nastro sulla parte di nastro vuota (contenente solamente degli infiniti $\sqcup$). In questo caso:
    - $S$ deve spostare di una posizione verso destra tutto il suffisso del suo nastro a partire da $\dot{\#}$ (che viene sostituito da un $\#$) fino all'ultimo $\#$ e scrivere $\dot{\sqcup}$ nella cella vuota creata (al posto di $\dot\#$).
    - Poi la Macchina di Turing entra nello stato che ricorda il nuovo stato di $M$ e _riposiziona la testina all'inizio del nastro_.

![[Pasted image 20240506113239.png|500]]

4. Se $M$ si ferma, anche $S$ si ferma, eventualmente rimuovendo tutti i separatori $\#$ e sostituendo i caratteri $\dot{a_t}$ con $a_t$.


## Linguaggio Turing-Riconoscibile per le Multinastro

![[023 Linguaggio Turing-Riconoscibile#^5b96ec|Linguaggio accettato da una MdT]]

Questo è vero anche per le Macchine di Turing Multinastro.

> [!quote] Dimostrazione
> Se $L$ è Turing-Riconoscibile, allora esiste una Macchina di Turing $M$ tale che $\mathcal{L}(M) = L$.
>
> -   Poiché $M$ è una Macchina di Turing a $k$ nastri con $k=1$, esiste una Macchina di Turing Multinastro $N$ tale che $\mathcal{L}(N)=L$.
>
> -   Viceversa, se esiste una Macchina di Turing multinastro $N$ tale che $\mathcal{L}(N)=L$, per il teorema precedente esiste una Macchina di Turing a singolo nastro $S$ tale che $\mathcal{L}(S)=\mathcal{L}(N)=L$.
>
> Quindi $L$ è Turing-Riconoscibile.

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
