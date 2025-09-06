---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Analisi Algoritmi
cssclasses:
  - 
---
>L'**Analisi degli Algoritmi Ricorsivi** permette di esprimere la [[002 Complessità Computazionale|Complessità Computazionale]] di algoritmi [[028 Ricorsione (Programmazione)|ricorsivi]] mediante [[045 Relazioni di Ricorrenza|Relazioni di Ricorrenza]].

Per derivare le relazioni di ricorrenza che descrivono il tempo di esecuzione $T(n)$ di un algoritmo ricorsivo occorre:
1. Determinare la [[006 Analisi di Algoritmi#TAGLIA DELL'INPUT|taglia]] dell'input $n$;
2. Determinare quale valore $n_0$ di $n$ è usato per la base della ricorsione;
3. Determinare il valore la complessità computazionale sulla base della ricorsione $T(n_0)$.

Una tipica relazione di ricorrenza sarà: $$T(n)=\begin{cases} c\quad\quad\quad\quad\quad\quad\quad\quad se\ n=n_0 \\ a\cdot T(f(n)) + g(n)\ \ \  altrimenti \end{cases}$$
Dove:
- $n_0$ è la base della ricorsione;
- $c$ è il tempo di esecuzione per la base;
- $a$ è il numero di chiamate ricorsive da effettuare;
- $f(n)$ è la taglia dei problemi risolti nelle chiamate ricorsive;
- $g(n)$ è tutto il tempo di calcolo non incluso nelle chiamate ricorsive.

> [!example]- <font color="orange">Esempi</font>
>```C
>Algoritmo1(n){
>	if(n==1)
>		fà qualcosa // c
>	else {
>		Algoritmo1(n-1); //T(n-1)
>		Algoritmo1(n-2); //T(n-2)
>	}
>
>	for(i = 1; i < n+1; i++) // Eseguito n volte
>		fà qualcos`altro // d
>}
>```
>
>L'equazione di ricorrenza che descrive la complessità $T(n)$ dell'`Algoritmo1(n)` sarà: $$T(n)=\begin{cases} c,\quad se\ n=1 \\ T(n-1)+T(n-2)+dn,\quad altrimenti \end{cases}$$
>
>---
>```C
>Algoritmo2(n){
>	if(n==1)
>		fà qualcosa // c
>	else {
>		for(i = 1; i < n+1; i++) // Eseguito n volte
>			Algoritmo2(i)
>	}
>	fà qualcos`altro // d
>}
>```
>
>L'equazione di ricorrenza che descrive la complessità $T(n)$ dell'`Algoritmo2(n)` sarà: $$T(n)=\begin{cases} c,\quad se\ n=1 \\ \sum_{i=1}^{n} T(i)+d,\quad altrimenti \end{cases}$$
>
>---
>```C
>Algoritmo3(n){
>	if ((n==1) || (n==2)){
>		fà qualcosa // c
>	}
>	else { // Due chiamate ricorsive
>		Algoritmo3(n/2); // T(n/2)
>		Algoritmo3(n/2); // T(n/2)
>	}
>	for(i = 1; i < n+1; i++){ // Eseguito per n volte
>		fà qualcos`altro // d
>	}	
>}
>```
>
>L'equazione di ricorrenza che descrive la complessità $T(n)$ dell'`Algoritmo3(n)` sarà: $$T(n)=\begin{cases} c,\quad se\ 1\leq n\leq2 \\ 2\cdot T(n/2)+n\cdot d,\quad altrimenti \end{cases}$$

---
> [!quote] Teorema - Soluzione alla Ricorrenza
>Per arbitrarie costanti $c,a,d$ la soluzione alla ricorrenza $$T(n)=\begin{cases} d,\quad se\ n\leq1 \\ a\cdot T(n/c)+bn^k,\quad altrimenti \end{cases}$$ è data da $$T(n)=\begin{cases} O(n^k),\quad\quad\quad\ \ \ se\ a<c^k \\ O(n^k\ log\ n),\quad\ se\ a=c^k \\ O(n^{log_ca}), \quad\quad\ \ se\ a>c^k \end{cases}$$

^0d0fd1

> [!example]- <font color="orange">Esempio</font>
> Supponendo che $T(1)=1$:
>- Se $T(n)=2\cdot T(n/3)+d\cdot n$, allora $2=\textcolor{#61AFEF}{a < c}=3$. Quindi è $\color{#61AFEF}O(n)$.
>- Se $T(n)=2\cdot T(n/2)+d\cdot n$, allora $2=\textcolor{#61AFEF}{a = c}=2$. Quindi è $\color{#61AFEF}O(n\ log\ n)$.
>- Se $T(n)=4\cdot T(n/2)+d\cdot n$, allora $4=\textcolor{#61AFEF}{a > c}=2$. Quindi è $O(n^{log_ca})=O(n^{log_24})=\color{#61AFEF}O(n^2)$.
>- Se $T(n)=T(9n/2)+ n$, allora $1=\textcolor{#61AFEF}{a < c}=10$. Quindi è $\color{#61AFEF}O(n)$.
>- Se $T(n)=2\cdot T(n/2)+n^3$, allora $2=\textcolor{#61AFEF}{a < c^3}=2^3=8$. Quindi è $\color{#61AFEF}O(n^3)$.
>- Se $T(n)=16\cdot T(n/4)+n^2$, allora $16=\textcolor{#61AFEF}{a = c^2}=4^2=16$. Quindi è $\color{#61AFEF}O(n^2\ log\ n)$.


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