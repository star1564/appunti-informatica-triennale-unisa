---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Tipi di Algoritmi
cssclasses:
  - 
---
Gli algoritmi basati sulla tecnica **Divide-et-Impera** hanno, generalmente, la seguente struttura [[008 Algoritmi Ricorsivi|ricorsiva]].

```C
Algoritmo_DeI(x){
	if(l`input x è sufficientemente piccolo o semplice){
		return risolvi x direttamente;
	}
	
	//	Decomoni l`istanza di input x in k istanze più piccole x_1,x_2,...,x_k
	s1 = Algoritmo_DeI(x_1);
	s2 = Algoritmo_DeI(x_2);
	...
	sk = Algoritmo_DeI(x_k);

	// Componi le sottosoluzioni s1, s2, ..., sk alle istanze x_i per ottenere 
	// una soluzione globale s alla istanza completa x
	return s;
}
```

Quando analizziamo un generico algoritmo $A$ progettato con la tecnica Divide-et-Impera, il numero di operazioni elementari eseguite sarà in generale $$T(n)=\begin{cases} c_0\quad\quad\quad\quad\quad\quad\quad\quad se\ n=n_0 \\ k\cdot T(f(n)) + g(n)\ \ \ \ \  altrimenti \end{cases}$$
Dove:
- $c_0$ è una costante che conta il numero di operazioni elementari eseguite dall'algoritmo $A$ quando l'input è di dimensione "piccola", ovvero di dimensione $\leq n_0$;
- $k$ è il numero di chiamate ricorsive di $A$;
- $f(n)$ è la dimensione dell'input di ogni chiamata ricorsiva di $A$;
- $g(n)$ è il numero di operazioni elementari eseguite al di fuori della ricorsione.

Ricordare il [[008 Algoritmi Ricorsivi#^0d0fd1|teorema della soluzione alla ricorrenza]].

> [!example]- <font color="orange">Esempio</font>
>L'esempio più classico del divide-et-impera è la ricerca binaria in un array ordinato.
>In **input** alla funzione abbiamo:
>- $a$: l'*array ordinato*, quindi $a[0]\leq a[1]\leq\dots\leq a[n-1]$.
>- $k$: un *numero arbitrario da ricercare*.
>- $s$: l'indice *dove iniziare la ricerca* nella chiamata ricorsiva (alla primissima chiamata è $0$).
>- $d$: l'indice *dove terminare la ricerca* nella chiamata ricorsiva (alla primissima chiamata è $n-1$).
>
>In **output** vogliamo ricevere: 
>- Un valore $\textcolor{#61AFEF}i\in\{0,1,\dots,n-1\}$ se $\color{#61AFEF}k=a[i]$. Ovvero vogliamo l'indice dell'elemento $k$.
>- Altrimenti (se l'elemento $k$ non si trova in $a$) la stringa *"non c'è"*.
>
>```C
>RicercaBinaria(a, k, s, d){
>	if(s == d){         // c_0 se n <= 1
>		if(k == a[s]){
>			return s;
>		}
>		else {
>			return "non c'è";
>		}
>	}
>
>	// Trova la metà dell'array
>	c = (s+d)/2;       // c
>	
>	if(k <= a[c]){ // Viene eseguita solo una chiamata
>		return RicercaBinaria(a, k, s, c); // T(n/2)
>	}
>	else{
>		return RicercaBinaria(a, k, c+1, d); // T(n/2)
>	}
>}
>```
>
>Abbiamo la seguente complessità $$T(n)=\begin{cases} c_0\quad\quad\quad\quad\quad se\ n\leq1 \\ T(n/2)+c\quad altrimenti \end{cases}$$
>E quindi la sua risoluzione è $T(n)=\color{#61AFEF}O(log\ n)$, perché:
>$T(n)=1\cdot T(n/2)+c\cdot n^0$, allora $1=\textcolor{#61AFEF}{a = c^0}=2^0=1$. Quindi $O(n^k\ log\ n)=O(n^0\ log\ n)=O(log\ n)$.


Ci sono moltissime applicazioni per il divide-et-impera, un esempio sarebbe quello di trovare il numero di moltiplicazioni usate per calcolare la potenza di un numero $a$ per un intero positivo $n$: $$a^n=a^{\lfloor n/2 \rfloor}\cdot a^{\lceil n/2 \rceil}$$

Questo perché sis sfruttano le [[040 Proprietà Operazioni in N0#POTENZE|proprietà delle potenze]].
Inoltre, $a^{\lceil n/2 \rceil} = a^{\lfloor n/2 \rfloor}$ se $n$ è pari, e $a^{\lceil n/2 \rceil} = a\cdot a^{\lfloor n/2 \rfloor}$ se $n$ è dispari.

> [!tip]- Algoritmo e analisi complessità
>```C
>FastPower(a, n){
>	if (n==1) // Moltiplicazioni: 0
>		return a;
>	else {
>		// Viene mandata la parte bassa di n/2 
>		x = FastPower(a, n/2); // T(n/2)
>
>		// Si prende il caso peggiore
>		if(n è pari)
>			return x*x; // Moltiplicazioni: 1
>		else
>			return x*x*a; // Moltiplicazioni: 2
>	}
>}
>```
>
>Abbiamo la seguente complessità $$T(n)\leq\begin{cases} 0\quad\quad\quad\quad\quad se\ n=1 \\ T(n/2)+2\quad altrimenti \end{cases}$$
>E quindi la sua risoluzione è $T(n)=\color{#61AFEF}O(log\ n)$.

Con il divide-et-impera è anche possibile riordinare un array (vedere il [[014 Merge Sort|Merge Sort]]).

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