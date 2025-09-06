---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Leggi Congiunte delle V.A.
cssclasses:
  - 
---
>Date due [[025 Variabili Aleatorie Discrete|Variabili Aleatorie Discrete]] $X$ e $Y$, si definisce la **[[025 Variabili Aleatorie Discrete#Densità Discreta|Densità Discreta]] [[019 Probabilità Condizionata|Condizionata]]** di $X$ dato $Y=y$, come
>$$\color{#CC241D}p_{X|Y}(x|y)=\mathbb{P}(X=x|Y=y)=\frac{\mathbb{P}(X=x,Y=y)}{\mathbb{P}(Y=y)}=\frac{p(x,y)}{p_Y(y)}$$
>per tutti i valori di $y$ per i quali $p_Y(y)>0$. Si utilizza la [[040 Funzioni di Distribuzione Congiunte#Variabili Aleatorie Discrete|densità discreta congiunta]].

### Funzione di Distribuzione
Per ogni $y$ tale che $p_Y(y)>0$ si definisce la *[[024 Funzione di Distribuzione|funzione di distribuzione]] condizionata* di $X$ dato $Y=y$:
$$\color{#61AFEF}F_{X|Y}(x|y)=\mathbb{P}\left(X\leq x\mid Y=y\right)=\sum_{a\leq x}p_{X|Y}(a|y)$$

### Variabili Indipendenti
Se $X$ è [[041 Variabili Aleatorie Indipendenti|indipendente]] da $Y$, le funzioni condizionate $p_{X|Y}(x|y)$ e $F_{X|Y}(x|y)$ coincidono con le versioni non condizionate. Infatti, se $X$ e $Y$ sono indipendenti, per ogni $x,y$ si ha che

$$
\begin{align*}
p_{X|Y}(x|y) &= \mathbb{P}(X=x|Y=y)\\
&=\frac{\mathbb{P}(X=x,Y=y)}{\mathbb{P}(Y=y)} \\
&= \frac{\mathbb{P}(X=x)\cdot \cancel{ \mathbb{P}(Y=y) }}{\cancel{ \mathbb{P}(Y=y) }} \\
&= \mathbb{P}(X=x) \\
&= p_X(x) \\
\end{align*}
$$

Viceversa, se $p_{Y|X}(y|x)=p_Y(y)$ per ogni $x,y$, allora $X$ e $Y$ sono indipendenti.

> [!example]- <font color="orange">Esempio</font>
>>Nell'esperimento che consiste nell'effettuare 5 estrazioni a caso da un'urna contenente 2 biglie nere e 3 biglie bianche, denotiamo con $X$ l'indice di posizione in cui fuoriesce la prima biglia nera, e con $Y$ il numero di biglie nere fuoriuscite nelle prime 3 posizioni.
>>Calcolare le densità discrete condizionate di $X$, dato $Y = y$, per $y = 0, 1, 2$.
>
>Si hanno ${5\choose 2}=10$ sequenze di estrazioni possibili. Calcoliamo la [[040 Funzioni di Distribuzione Congiunte#Densità Discreta Congiunta|Densità Discreta Congiunta]] di $(X,Y)$, da cui si ricava la densità discreta di $Y$.
>
>![[Pasted image 20240822122426.png]]
>
>Si nota che $X$ e $Y$ *non sono indipendenti* in quanto $$p(1,0)=0 \textcolor{#61AFEF}\neq p_X(1)\cdot p_Y(0) = 0,04$$
>
>Ricaviamo le densità discrete condizionate di $X$, dato $Y=y$ tenendo presente che $$p_{X|Y}(x|y)=\frac{p(x,y)}{p_Y(y)},\quad y=0,1,2\quad x=1,2,3,4$$
>
>Per i valori si va a vedere la corrispondenza sulla colonna $y$ della seconda tabella. Se questa corrispondenza è $\neq 0$, la si divide per la $p_Y(y)$ corrispondente considerando anche la $x$.
>
>Per $y=0$:
>$$p_{X|Y}(x|0)=\frac{p(x,0)}{p_Y(0)}=\begin{cases} 0, & x=1,2,3 \\ \frac{1/10}{1/10}=1, & x=4 \end{cases}$$
>
>
>Per $y=1$:
>$$p_{X|Y}(x|1)=\frac{p(x,1)}{p_Y(1)}=\begin{cases} \frac{2/10}{6/10}=\frac{1}{3}, & x=1,2,3 \\ 0, & x=4 \end{cases}$$
>
>Per $y=2$:
>$$p_{X|Y}(x|2)=\frac{p(x,2)}{p_Y(2)}=\begin{cases} \frac{2/10}{3/10}=\frac{2}{3}, & x=1 \\ \frac{1/10}{3/10}=\frac{1}{3}, & x=2 \\ 0, & x=3,4 \end{cases}$$

### Valore Atteso
Il *[[026 Valore Atteso di una VA Discreta|Valore Atteso]] Condizionato* di $X$ dato $Y=y$ e di $Y$ dato $X=x$ sono:
$$E[X|Y=y]=\sum_{x}x\cdot p_{X|Y}(x|y)$$ $$E[Y|X=x]=\sum_{y}y\cdot p_{Y|X}(y|x)$$

### Varianza
La *[[027 Varianza di una VA Discreta|Varianza]] Condizionata* di $X$ dato $Y=y$ e di $Y$ dato $X=x$ sono:
$$\begin{align} \\
\text{Var}(X|Y=y)&=\sum_{x}(x-E[X|Y=y])^{2}\,p_{X|Y}(x|y) \\  \\
&=E[X^{2}|Y=y]-(E[X|Y=y])^{2}
\end{align}$$

con $E[X^{2}|Y=y]=\sum_{x}x^{2}\,p_{X|Y}(x|y)$

$$\begin{align}
\text{Var}(Y|X=x)&=\sum_{y}(y-E[Y|X=x])^{2}\,p_{Y|X}(y|x) \\
&=E[Y^{2}|X=x]-(E[Y|X=x])^{2}
\end{align}$$
con $E[Y^{2}|X=x]=\sum_{y}y^{2}\,p_{Y|X}(y|x)$

___
[[000 Indice PS|↖ Ritorna all'indice ↖]]

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