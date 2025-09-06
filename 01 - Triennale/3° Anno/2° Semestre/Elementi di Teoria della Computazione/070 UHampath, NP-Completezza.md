---
aliases:
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: NP-completezza
cssclasses:
    - 
---

È possibile definire una “versione [[012 Grafi#Grafi Non Diretti (Non Orientati)|non orientata]]” del [[069 Hampath, NP-Completezza|problema del cammino Hamiltoniano]].
$$UHAMPATH = \{\langle G,s,t \rangle\ |\ G\text{ è un grafo NON orientato e ha un cammino Hamiltoniano da }s \text{ a }t\}$$

Per mostrare che $UHAMPATH$ è NP-completo, definiamo una riduzione di tempo polinomiale da $HAMPATH$ a $UHAMPATH$.


## Teorema $UHAMPATH \in NP$

Un algoritmo $N$ che verifica $UHAMPATH$ in tempo polinomiale è il seguente.

> [!note] Descrizione di $N$
> Sull'input $\langle \langle G,s,t \rangle, c \rangle$, dove $G=(V,E)$ è un grafo non orientato:
>
> 1. Verifica se $c = (u_1, \dots , u_{|V|})$ è una sequenza di $|V|$ vertici di $G$, altrimenti rifiuta.
> 2. Verifica le seguenti condizioni, accettando in caso affermativo e rifiutadno altrimenti:
>     - I nodi della sequenza sono distinti
>     - $u_1 = s, u_{|V|} = t$
>     - Per ogni $i$ con $2 \le i \le n$, se $(u_{i−1}, u_i) \in E$
>
> $$\exists c : \langle \langle G,s,t \rangle, c \rangle \in \mathcal{L}(N) \iff \langle G,s,t \rangle \in UHAMPATH$$


## Teorema NP-completezza di $UHAMPATH$

![[063 NP-Completezza#^68fbfb]]

Abbiamo provato che $UHAMPATH\in NP$. Per concludere la prova, dimostriamo che $$HAMPATH\leq_p UHAMPATH$$

La riduzione di tempo polinomiale associa a un grafo orientato $G = (V, E)$ con vertici $s$ e $t$ un grafo non orientato $G' = (V', E')$ con vertici $s'$ e $t'$.

Il grafo G ha un cammino Hamiltoniano da $s$ verso $t$ se e solo se $G'$ ha un cammino Hamiltoniano da $s'$ verso $t'$. Inoltre, $G'$ può essere costruito a partire da $G$ in tempo
polinomiale.

![[Pasted image 20240607135302.png]]

Costruzione di $G'$:

-   Ogni vertice $u$ di $G$, diverso da $s$ e $t$ è rimpiazzato da tre vertici $u^{in}$, $u^{mid}$ e $u^{out}$ in $G'$.
-   I vertici $s$ e $t$ sono sostituiti con i vertici $s^{out}$ e $t^{in}$ in $G'$
-   $\forall u \in V - \{s, t\}$, si ha che $(u^{in}, u^{mid} ), (u^{mid} , u^{out} ) \in E'$
-   Se $(u, v) \in E$ allora $(u^{out} , v^{in}) \in E'$

1. Dimostriamo che $G$ ha un cammino Hamiltoniano da $s$ a $t$ se e solo se $G'$ ha un cammino Hamiltoniano da $s^{out}$ a $t^{in}$.

Se $G$ ha un cammino Hamiltoniano $P$ da $s$ a $t$
$$P=s,u_1,u_2,\dots,u_k,t$$

Allora $P'$ è un cammino Hamiltoniano in $G'$ da $s^{out}$ a $t^{in}$
$$P'=s^{out}, u^{in}_1,u^{mid}_1,u^{out}_1,\quad u^{in}_2,u^{mid}_2,u^{out}_2,\quad \dots\quad u^{in}_k,u^{mid}_k,u^{out}_k, t^{in}$$

2. Viceversa, se $G'$ ha un cammino Hamiltoniano $P'$ da $s^{out}$ a $t^{in}$, è facile vedere che $P'$ deve essere della forma

$$P'=s^{out}, u^{in}_1,u^{mid}_1,u^{out}_1,\quad u^{in}_2,u^{mid}_2,u^{out}_2,\quad \dots\quad u^{in}_k,u^{mid}_k,u^{out}_k, t^{in}$$

La prova è per [[046 Principio di Induzione Matematica|induzione]] su $k$.
Infatti $P'$ ha come primo vertice $s^{out}$ il quale è connesso solo a vertici della forma $u^{in}_i$ . Quindi il secondo vertice è $u^{in}_i$ per qualche $i$ .

I vertici successivi devono essere $u^{mid}_i, u^{out}_i$ perché $u^{mid}_i$ è connesso solo a $u^{in}_i$ e $u^{out}_i$.

Ma se $P'$ ha la forma suddetta, allora
$$P=s,u_1,u_2,\dots,u_k,t$$
è un cammino Hamiltoniano da $s$ a $t$.

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
