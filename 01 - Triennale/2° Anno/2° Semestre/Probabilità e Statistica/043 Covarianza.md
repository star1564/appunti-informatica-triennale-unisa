---
aliases:
  - Variabili Aleatorie Scambiabili
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Somme di V.A.
cssclasses:
  - 
---
> [!note]+ Valore Atteso di una moltiplicazione tra due funzioni
>Siano $X$ e $Y$ due variabili aleatorie [[041 Variabili Aleatorie Indipendenti|indipendenti]] e siano $g,h$ due funzioni, allora $$E[g(X)\cdot h(Y)]=E[g(X)]\cdot E[h(Y)]$$
>
>#### Dimostrazione
>Supponiamo che $X$ e $Y$ siano variabili [[025 Variabili Aleatorie Discrete|discrete]] con [[040 Funzioni di Distribuzione Congiunte#Densità Discreta Congiunta|distribuzione congiunta]] $p(x,y)$. Allora
>
>$$\begin{align*} E[g(X)\cdot h(Y)] &=\sum_{x}\sum_{y}g(x)\cdot h(y)\cdot p(x,y) \\ &= \sum_{x}\sum_{y}g(x)\cdot h(y)\cdot p_{X}(x)\cdot p_{Y}(y) \\ &= \sum_{x}(g(x)\cdot p_{X}(x))\cdot\sum_{y}(h(y)\cdot p_{Y}(y)) \\ &= E[g(X)]\cdot E[h(Y)] \end{align*}$$
>
>Avendo sfruttato la relazione d'indipendenza:
>![[041 Variabili Aleatorie Indipendenti#^e48a86]]

## Definizione Covarianza
>La **Covarianza** tra due variabili aleatorie $X$ e $Y$, misura la *relazione lineare tra le due variabili*, ovvero un'informazione su quanto $X$ e $Y$ siano relazionate.
>
>Essa è definita come:
>$$\color{#CC241D}\text{Cov}(X,Y)=E\big[(X-E[X])\cdot (Y-E[Y])\big]$$

Se sviluppiamo i termini e utilizziamo la proprietà di linearità, si ricava che:
$$\begin{align*} \text{Cov}(X,Y) &= E\big[X\cdot Y-E[X]\cdot Y-X\cdot E[Y]+E[X]\cdot E[Y]\big] \\ &= E[X\cdot Y]-E[X]\cdot E[Y]-E[X]\cdot E[Y]+E[X]\cdot E[Y] \\ &= \color{#61AFEF}E[X\cdot Y]-E[X]\cdot E[Y] \\ \end{align*}$$

> [!example]- <font color="orange">Esempio</font>
> Calcolare la covarianza data la densità discreta di $X$ e $Y$:
> 
> ![[Pasted image 20250321120507.png]]
> 
> Il valore atteso per $X$ e $Y$ è:
> $$\begin{align}E[X]&=\left[ 1\cdot \frac{7}{16} \right] + \left[ 2\cdot \frac{5}{16} \right] + \left[ 3\cdot \frac{1}{16} \right]\\ &= \frac{7}{16} + \frac{10}{16} +\frac{3}{16} \\ &=\frac{20}{16}\\ &=\frac{5}{4}\end{align}$$
> 
> $$\begin{align}E[Y]&=\left[ 1\cdot \frac{1}{2} \right] + \left[ 2\cdot \frac{1}{4} \right]\\ &= \frac{1}{2}+\frac{1}{2}\\ &= 1\end{align}$$
> 
> Queste due variabili aleatorie *non sono indipendenti*, in quanto (ad esempio):
> $$p(0, 1) = 0 \textcolor{#CC241D}{\neq} \frac{3}{32} = \frac{3}{16}\cdot \frac{1}{2}= p_{X}(0)\cdot p_{Y}(1)$$
> 
> ![[Pasted image 20250321122123.png|500]]
> 
> Si vuole prima di tutto calcolare questo (*si ignorano indici o densità pari a $0$*):
> $$\begin{align}
> E[X\cdot Y] &= \textcolor{#00C575}{\left[ (1\cdot 1)\cdot \frac{4}{16} \right]} + \textcolor{#FF6611}{\left[ (1\cdot 2)\cdot \frac{3}{16} \right]} +\\ &+ \textcolor{#e86162}{\left[ (2\cdot 1)\cdot \frac{4}{16} \right]} + \textcolor{#fdaa39}{\left[ (2\cdot 2)\cdot \frac{1}{16} \right]} \\ &= \frac{4}{16}+\frac{6}{16}+\frac{8}{16}+\frac{4}{16} \\ &= \frac{11}{8}
> \end{align}$$
> 
> Si calcola adesso la covarianza:
> $$\begin{align}\text{Cov}(X,Y)&=E[X\cdot Y]-E[X]\cdot E[Y]\\ &=\frac{11}{8}-\left( \frac{5}{4}\cdot 1 \right)\\ &=\frac{1}{8}\end{align}$$
> 

### Interpretazione della Covarianza
La covarianza tra $X$ e $Y$ indica come i valori di $X$ e $Y$ si muovono in relazione tra di essi:
- Se $\text{Cov}(X,Y)\color{#CC241D}>0$, le variabili aleatorie $X$ e $Y$ sono **positivamente correlate**. Questo significa che se una delle variabili *tende ad assumere valori superiori* alla propria media, *anche l'altra variabile tende a fare lo stesso*. Quindi c'è una tendenza comune: entrambe tendono a "muoversi" nella *stessa direzione*.
  ![[Pasted image 20240822172152.png]]

- Se $\text{Cov}(X,Y)\color{#CC241D}<0$, le variabili aleatorie $X$ e $Y$ sono **negativamente correlate**. In questo caso, se una delle variabili *tende ad assumere valori superiori* alla propria media, l'altra variabile *tende ad assumere valori inferiori* alla propria media. Qui, le variabili tendono a "muoversi" in *direzioni opposte*.
  ![[Pasted image 20240822172208.png]]

- Se $\text{Cov}(X,Y)\color{#CC241D}=0$, le variabili aleatorie $X$ e $Y$ **non sono correlate** (scorrelate). Questo indica che *non esiste una relazione lineare* evidente tra le due variabili. Tuttavia, ciò *non significa necessariamente che siano indipendenti*, ma solo che non c'è una tendenza lineare comune.
  ![[Pasted image 20240822172220.png]]
  

Se $X$ e $Y$ sono variabili aleatorie indipendenti, $\text{Cov}(X,Y)=0$. Quindi:
$$X, Y \text{ indipendenti} \Longrightarrow \text{Cov}(X,Y)=0$$
$$\text{Cov}(X,Y)= 0 \not\Longrightarrow X, Y \text{ indipendenti}$$
$$\text{Cov}(X,Y)\neq 0 \Longrightarrow X, Y \text{ non sono indipendenti}$$

> [!quote] Dimostrazione
> Usando $g(x) = x$ e $h(y)=y$ e avendo che $X$ e $Y$ sono *indipendenti*, si ha che $$E[X\cdot Y]=E[X]\cdot E[Y]$$
> Quindi $$\text{Cov}(X,Y)=E[X\cdot Y] - E[X]\cdot E[Y] = 0$$

### Variabili Aleatorie Scambiabili
Due variabili aleatorie $X$ e $Y$ si dicono **Scambiabili** se $$p(x,y)=p(y,x),\quad \forall x,y$$

Se $X$ e $Y$ sono scambiabili, allora sono identicamente distribuite.

### Proprietà della Covarianza
1. $\text{Cov}(X,Y) = \text{Cov}(Y,X)$
2. $\text{Cov}(X,X)=\text{Var}(X)$
3. $\text{Cov}(a\cdot X,Y)=a\cdot \text{Cov}(X,Y), \forall a\in\mathbb{R}$
4. $\text{Cov}\left(\displaystyle\sum_{i=1}^{n} X_i, \displaystyle\sum_{j=1}^{m} Y_j\right)=\displaystyle\sum_{i=1}^{n} \displaystyle\sum_{j=1}^{m} \text{Cov}(X_i,Y_j)$

> [!quote] Dimostrazione
> 1. Per via della definizione di covarianza
> 2. Per via della definizione di covarianza
> 3. Per via della linearità della covarianza, si ha che $$\text{Cov}(a\cdot X,Y)=E\Big[(a\cdot X - E[a\cdot X])\cdot (Y-E[Y])\Big]=a\cdot \text{Cov}(X,Y)$$
> 4. Si pone $\mu_i=E[X_i]$ e $v_j=E[Y_j]$, quindi $$E\left[\displaystyle\sum_{i=1}^{n} X_i\right]=\displaystyle\sum_{i=1}^{n} \mu_i,\quad E\left[\displaystyle\sum_{j=1}^{m} Y_j\right]=\displaystyle\sum_{j=1}^{m} v_j$$Inoltre
>$$\begin{align*}\text{Cov}\left(\sum_{i=1}^{n}X_{i},\sum_{j=1}^{m}Y_{j}\right) &= E\left[\left(\sum_{i=1}^{n}X_{i}-\sum_{i=1}^{n}\mu_{i}\right)\left(\sum_{j=1}^{m}Y_{j}-\sum_{j=1}^{m}v_{j}\right)\right] \\&= E\left[\sum_{i=1}^{n}(X_{i}-\mu_{i})\sum_{j=1}^{m}(Y_{j}-v_{j})\right] \\&= E\left[\sum_{i=1}^{n}\sum_{j=1}^{m}(X_{i}-\mu_{i})(Y_{j}-v_{j})\right] \\&= \sum_{i=1}^{n}\sum_{j=1}^{m} E\left[(X_{i}-\mu_{i})(Y_{j}-v_{j})\right] \end{align*}$$dove l'ultima uguaglianza segue dalla proprietà di linearità (in quanto il valore atteso di una somma di variabili aleatorie è uguale alla somma dei valori attesi).

### Coefficiente di Correlazione
>Il **Coefficiente di Correlazione** è un valore compreso tra $-1$ e $1$ che *misura il grado di linearità* tra due variabili aleatorie $X$ e $Y$. 
>È così definito:
>$$\color{#CC241D}\rho=\frac{\text{Cov}(X,Y)}{\sqrt{\text{Var}(X)\text{Var}(Y)}}=\frac{\text{Cov}(X,Y)}{\sigma_X\sigma_Y}$$
>Dove:
>- $\sigma_X=\sqrt{\text{Var}(X)}$ ([[027 Varianza di una VA Discreta#^1f00a4|derivazione standard]] di $X$)
>- $\sigma_Y=\sqrt{\text{Var}(Y)}$ (derivazione standard di $Y$)
>- $-1\le \rho(X,Y)\leq 1$


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