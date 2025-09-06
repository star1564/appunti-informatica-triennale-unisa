---
aliases: 
tags:
  - corsi/matematica/metodi_matematici_informatica
paragrafo: Ricorsione - Alberi
cssclasses:
  - 
---
[[023 Alberi Binari|Alberi e grafi]] sono tra le principali strutture dati gerarchiche.

## Alberi Radicati (MMI)
>Un **Albero Radicato** $T$ è una coppia costituita da un *insieme* $V$ di <font color="FF6611">Vertici (o Nodi)</font> contenente un vertice speciale chiamato Radice, ed un *insieme* $E\subseteq V\times V$ di <font color="#00C575">Archi che collegano tali Vertici</font>.
>$$T=(\textcolor{#FF6611}{V}, \textcolor{#00C575}{E})$$

Gli alberi radicati possono essere definiti ricorsivamente mediante una definizione che costruisce alberi più grandi a partire da alberi più piccoli.

Inoltre vengono spesso [[013 Alberi#^f4688c|rappresentati graficamente]].

> [!example]- <font color="orange">Esempio</font>
> $\textcolor{#FF6611}{V}=\{1,2,3,4,5,6,7\}$
>$\textcolor{#00C575}{E}=\{(1, 2), (1, 3), (1, 4), (2, 5), (2, 6), (4, 7)\}$ 
>
>![[Pasted image 20220513120440.png|500]]


> [!quote]+ Definizione Ricorsiva di un Albero
> **Passo Base**: La coppia $(\{\textcolor{#FFC000}{r}\}, \textcolor{#00C575}{\emptyset})$ è un albero radicato (solo con la radice $r$ e nessun arco).
> 
> **Passo Ricorsivo**: Supponiamo che abbiamo $n$ alberi $$T_1=(\textcolor{#FF6611}{V_1}, \textcolor{#00C575}{E_1}),\ T_2=(\textcolor{#FF6611}{V_2}, \textcolor{#00C575}{E_2}),...,\ T_n=(\textcolor{#FF6611}{V_n}, \textcolor{#00C575}{E_n})$$
>questi sono radicati e disgiunti, ovvero devono essere diversi tra di loro $$V_i\ \cap\ V_j=\emptyset,\quad 1\leq i<j\leq n$$
>e che hanno radici $$r_1,\ r_2, ..., r_n,\quad r_1\in V_1, r_2\in V_2,...,\ r_n\in V_n$$
>Allora la coppia $T=(\textcolor{#CC241D}{V}, \textcolor{#61AFEF}{E})$, ottenuta ponendo come radice di $T$ un vertice $\textcolor{#FFC000}r$ che non appaia in nessuno di questi alberi e aggiungendo un arco da $\textcolor{#FFC000}r$ a ciascuno dei vertici $\textcolor{#FF6611}{r_1}, \textcolor{#FF6611}{r_2}, \textcolor{#FF6611}{...}, \textcolor{#FF6611}{r_n}$, cioè $$\textcolor{#CC241D}{V}=\{\textcolor{#FFC000}{r}\}\ \cup\ \textcolor{#FF6611}{V_1}\ \cup\ \textcolor{#FF6611}{...}\ \cup\ \textcolor{#FF6611}{V_n},\quad \textcolor{#FFC000}{r}\not\in \textcolor{#FF6611}{V_1}\ \cup\ \textcolor{#FF6611}{...}\ \cup\ \textcolor{#FF6611}{V_n}$$ $$\textcolor{#61AFEF}{E} = \{(\textcolor{#FFC000}r, \textcolor{#FF6611}{r_1}), ..., (\textcolor{#FFC000}r, \textcolor{#FF6611}{r_n}))\}\ \cup\ \textcolor{#00C575}{E_1}\ \cup\ \textcolor{#00C575}{...}\ \cup\ \textcolor{#00C575}{E_n}$$
>è un albero radicato.
>
>![[Pasted image 20220513130135.png|600]]


Il passo ricorsivo costruisce un nuovo albero radicato a partire da uno o più alberi $T_1, ..., T_n$, tali che nessun vertice appaia più di una volta nei $T_i$, e da un "nuovo" vertice $r$, collegando $r$ alle radici di $T_1, ..., T_n$.

## Proprietà degli Alberi
Certe proprietà degli alberi hanno le loro definizioni ricorsive. Queste sfruttano il fatto che un albero radicato *si riduce a un singolo vertice oppure è ottenuto a partire da $n$ alberi $T_1, ..., T_n$*.

- [[038 Numero di Vertici in un Albero|Numero di Vertici in un Albero]]
- [[039 Numero di Archi in un Albero|Numero di Archi in un Albero]]
- [[040 Numero di Foglie in un Albero|Numero di Foglie in un Albero]]
- [[041 Numero di Nodi Interni in un Albero|Numero di Nodi Interni in un Albero]]
- [[042 Altezza di un Nodo in un Albero|Altezza di un Nodo in un Albero]]
- [[043 Profondità di un Nodo in un Albero|Profondità di un Nodo in un Albero]]
- [[044 Livello di un Nodo in un Albero|Livello di un Nodo in un Albero]]

---

> [!note]+ Definizione Ricorsiva di un Albero Binario Pieno [Def. 4, cap. 5.3]
> **Passo Base**: $(\{r\}, \emptyset)$ è un albero binario pieno (solo la radice $r$).
> 
> **Passo Ricorsivo**: Supponiamo che $T_1(V_1, E_1),\ T_2(V_2, E_2)$ siano alberi binari pieni disgiunti, cioè $$V_1\ \cap\ V_2=\emptyset$$ con radici $r_1, r_2$, dove $r_1\in V_1,\ r_2\in V_2$.
>Allora la coppia $T=(V,E)$ ottenuta ponendo come radice di un $T$ un nodo $r$ che non appaia né in $T_1$ e ne in $T_2$ e aggiungendo un arco da $r$ a ciascuno dei vertici $r_1, r_2$, cioè $$V=\{r\}\ \cup\ V_1\ \cup\ V_2,\quad r\not\in V_1\ \cup V_2$$ $$E=\{(r, r_1), (r, r_2)\}\ \cup\ E_1\ \cup E_2$$
>è un albero binario pieno.
>
> ![[Pasted image 20220521093307.png|300]]



___
[[000 Indice MMI|↖ Ritorna all'indice ↖]]

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
