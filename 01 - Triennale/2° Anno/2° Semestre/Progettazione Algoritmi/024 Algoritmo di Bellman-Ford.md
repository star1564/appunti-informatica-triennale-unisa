---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Alberi e Grafi
cssclasses:
  - 
---
>L'**Algoritmo di Bellman-Ford** è un algoritmo di [[010 Programmazione Dinamica|programmazione dinamica]] utilizzato per trovare i cammini più brevi da un nodo di partenza a tutti gli altri nodi in un [[012 Grafi|Grafo]] diretto o non diretto con archi di peso positivo o [[023 Archi con costo Negativo|negativo]]. 
>
>A differenza dell'algoritmo di Dijkstra, l'algoritmo di Bellman-Ford è in grado di gestire grafi che contengono cicli negativi.

Sia $\color{#CC241D}OPT(i, v)$ il minimo costo di un cammino che *parte da $v$*, arriva a $t$  e che *usa al massimo $i$ archi*. Possiamo avere due casi:
1. Il cammino minimo $P$ da $v$ a $t$ usa al massimo $i-1$ archi;
2. Il cammino minimo $P$ da $v$ a $t$ usa esattamente $i$ archi.

Se $P$ usa l'arco $(v,w)$ come primo arco, allora il costo di $P$ è pari a: $costo\_ arco+\text{costo del cammino minimo da }w\text{ a }t$, che usa al massimo $i-1$ archi, quindi: $$OPT(i,v)=costo\_ arco(v,w)+OPT(i-1, w)$$
![[Pasted image 20230613180813.png|300]]

Normalmente non conosciamo il primo arco $(v,w)$, quindi bisogna calcolarlo, andando a trovare il minimo costo tra gli archi uscenti da $v$: $$min_{w\in V}\{costo\_ arco(v,w)+OPT(i-1,w)\}$$
Questo perché dobbiamo vedere quale primo arco prendere.

> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20230613180842.png]]
> Qui prendiamo $w_3$, perché se $OPT(i-1, w)$ ha un certo costo $x$, abbiamo che: 
> - $w_1$: $1+x$
> - $w_2$: $3+x$
> - $w_3$: $-2+x$
> 
> Quindi $w_3$ comporta il minor costo.

Vogliamo quindi trovare il minimo valore trai due casi possibili (quello in cui si usano $i-1$ archi partendo da $v$, oppure quello in cui si usano $i$ archi calcolando il minimo man mano): $$OPT(i,v)=\begin{cases} 0\quad\quad\text{se }v=t \\ \infty\quad\quad\text{se }i=0,v\neq t \\ min\Big\{OPT(i-1,v),\ min_{w\in V}(costo(v,w) + OPT(i-1,w))\Big\} \quad\quad\text{se }i>0,v\neq t \end{cases}$$

> [!summary] Algoritmo di Bellman-Ford
>```C
>BellmanFord(G, s, t){
>	for(ogni nodo v in V){
>		M[0, v] = infinito;
>	}
>
>	M[0, t] = 0;
>	
>	for(|V|-1 volte){
>		for(ogni nodo v in V){
>			M[i, v] = M[i-1, v];
>			for(ogni arco uscente da v, (v,w) in E){
>				if(M[i, v] > M[i-1, w]+costo(v,w)){
>					M[i, v] = M[i-1, w]+costo(v,w)
>				}
>			}
>		}
>	}
>	return M[n-1, s];
>}
>```

L'algoritmo di Bellman-Ford ha una complessità temporale di $O(|V| \cdot |E|)$, dove $|V|$ è il numero dei nodi e $|E|$ è il numero degli archi nel grafo. Non è l'algoritmo più efficiente per trovare i cammini più brevi in grafi senza cicli negativi, ma è utile quando si devono gestire archi con peso negativo o quando si desidera identificare la presenza di cicli negativi.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20230613183445.png]]
>Abbiamo $|V|=5$, quindi avverranno $|V|-1=4$ iterazioni.
>
>- Iterazione 0:
>	- $OPT(0,v)=\infty,\quad \forall v\in V$
>	- $OPT(0,t)=0$
>- Iterazione 1:
>	- $OPT(1, s)=min(\infty, min(10+0,\ 5+\infty))=10$
>	- $OPT(1, y)=min(\infty, min(3+0,\ 9+\infty,\ 2+\infty))=3$
>	- $OPT(1, z)=min(\infty, min(6+\infty,\ 7+\infty))=\infty$
>	- $OPT(1, x)=min(\infty, min(4+\infty))=\infty$
>- Iterazione 2:
>	- $OPT(2, s)=min(10, min(5+3))=8$
>	- $OPT(2, y)=min(3, min(9+\infty, 2+\infty))=3$
>	- $OPT(2, z)=min(\infty, min(6+\infty, 7+10))=17$
>	- $OPT(2, x)=min(\infty, min(4+\infty))=\infty$
>- Iterazione 3:
>	- $OPT(3, s)=min(8, min(5+3))=8$
>	- $OPT(3, y)=min(3, min(9+\infty, 2+17))=3$
>	- $OPT(3, z)=min(17, min(4+\infty, 7+8))=15$
>	- $OPT(3, x)=min(\infty, min(4+17))=21$
>- Iterazione 4:
>	- $OPT(4, s)=min(8, min(5+3))=8$
>	- $OPT(4, y)=min(3, min(9+17, 2+17))=3$
>	- $OPT(4, z)=min(15, min(4+17, 7+8))=15$
>	- $OPT(4, x)=min(17, min(4+15))=19$
>
>Risultato: $OPT(4, s)=8$.

___
[[000 Indice PA|↖ Ritorna all'indice ↖]]

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
- [Michael Sambol (TEORIA)](https://www.youtube.com/watch?v=9PHkk0UavIM)
- [Michael Sambol (ESEMPIO)](https://www.youtube.com/watch?v=obWXjtg0L64)