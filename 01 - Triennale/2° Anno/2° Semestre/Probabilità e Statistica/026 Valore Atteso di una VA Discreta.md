---
aliases:
  - Momento di Ordine n
  - Proprietà di Linearità (VA Discreta)
tags:
  - corsi/matematica/probabilità_statistica
inglese:
  - Expected Value
paragrafo: V.A. Discrete
cssclasses:
  - 
---
### Definizione Valore Atteso
>Il **Valore Atteso** di una [[023 Variabili Aleatorie|Variabile Aleatoria]] $X$ è un *indice di tendenza centrale* dei suoi valori. Essendo una [[Media Ponderata|Media Pesata]] di tutti i possibili valori che $X$ può assumere, il valore atteso è *un valore a cui tende un [[009 Esperimento|esperimento]] dopo molte ripetizioni*.

### Valore Atteso per Variabili Aleatorie Discrete
>Se $X$ è una [[025 Variabili Aleatorie Discrete|variabile aleatoria discreta]] con [[025 Variabili Aleatorie Discrete#Densità Discreta|densità discreta]] $p(x)$, il **Valore Atteso** (o valore medio) di $X$ è definito da 
>$$\color{#CC241D}E[X]=\displaystyle\sum_{x} x\cdot p(x)$$con $p(x)>0$ per ogni $x$.
>
>Viene spesso rappresentato con $\color{#CC241D}\mu$.

^2c4c32

> [!example]- <font color="orange">Esempi</font>
>Se $p(0)=p(1)=\frac{1}{2}$ ed $x=0,1$ abbiamo che $$E[X]=0\cdot p(0)+1\cdot p(1)=0\cdot \frac{1}{2} + 1\cdot \frac{1}{2} = \frac{1}{2}$$
>Se $p(0)=\frac{1}{3}, p(1)=\frac{2}{3}$ ed $x=0,1$ abbiamo che $$E[X]=0\cdot p(0)+1\cdot p(1)=0\cdot \frac{1}{3} + 1\cdot \frac{2}{3} = \frac{2}{3}$$
>
>---
>> Calcolare il valore atteso nel lancio di un dado non truccato.
>
>Abbiamo che $$p(k)=\textcolor{#61AFEF}{\frac{1}{6}},\quad k=1,2,3,4,5,6$$
>
>Quindi
>$$E[X]=\displaystyle\sum_{k=1}^{6} k\cdot p(k)=\textcolor{#61AFEF}{\frac{1}{6}} \textcolor{#e86162}{\displaystyle\sum_{k=1}^{6} k}=\textcolor{#61AFEF}{\frac{1}{6}} \cdot \textcolor{#e86162}{\frac{6\cdot7}{2}}=\frac{7}{2}=3,5$$
>
>Avendo usato
>$$\color{#e86162}\displaystyle\sum_{k=1}^{n} k=\frac{n(n+1)}{2}$$
>
>---
>![[Pasted image 20250313124619.png]]

### Valore Atteso per una funzione di una Variabile Aleatoria
>Se $X$ è una variabile aleatoria discreta che assume i valori $x_i, i\geq 1$, con probabilità $p(x_i)$, allora per ogni *funzione a valori reali $g(x)$* risulta che 
>$$\color{#61AFEF}E[g(X)]=\displaystyle\sum_{i}g(x_i)\cdot p(x_i)$$

> [!example]- <font color="orange">Esempio</font>
>>Sia $X$ una variabile aleatoria che assume i valori $\{-1,0,1\}$ con probabilità $p(-1)=0,2$, $p(0)=0,5$, $p(1) = 0,3$.
>>Calcolare $E[X^2]$.
>
>$$E[X^2]=\displaystyle\sum_{i} x_i^2\cdot p(x_i)=(-1)^2\cdot 0,2 + 0^2\cdot 0,5 + 1^2\cdot 0,3=0,2+0,3=0,5$$

### Proprietà di Linearità
>Sia $X$ una variabile aleatoria discreta. Se $a$ e $b$ sono costanti reali, allora
>$$\color{#CC241D}E[a\cdot X + b]=a\cdot E[X] + b$$

Con con $X,Y$ variabili aleatorie discrete, possiamo derivare che:
- $E[X+Y]=E[X]+E[Y]$
- $E[c\cdot X] = c\cdot E[X]$

> [!quote]+ Dimostrazione
> Si ha che: 
> - $E[g(X)]=\displaystyle\sum_{i}g(x_i)\cdot p(x_i)$
> - $g(x)=a\cdot x+b$
>
>Quindi:
>$$\begin{align}E[g(X)]&=\sum_{i}\bigg((a\cdot x_i+b)\cdot p(x_i)\bigg)\\ &=\sum_{i}\bigg(\big(a\cdot x_i\cdot p(x_i)\big)+\big(b\cdot p(x_i)\big)\bigg) \\ &= \text{[proprietà della sommatoria: somma]} \\ &=\sum_{i}\bigg(a\cdot x_i\cdot p(x_i)\bigg) + \sum_{i}\bigg(b\cdot p(x_i)\bigg)\\ &= \text{[proprietà della sommatoria: moltiplicazione per costanti]}  \\ &=a\cdot\textcolor{#61AFEF}{\left(\displaystyle\sum_{i}x_i\cdot p(x_i)\right)} + b\cdot \textcolor{#e86162}{\left(\displaystyle\sum_{i}p(x_i)\right)}\\ &= \text{[\textcolor{#61AFEF}{formula del Valore Atteso}\quad\vert\quad \textcolor{#e86162}{somma della funzione di densità = 1}]} \\ &=a\cdot \textcolor{#61AFEF}{E[X]}+b\cdot \textcolor{#e86162}1\\ &=a\cdot E[X]+b\end{align}$$


### Momento di Ordine $n$

>Sia $X$ una variabile aleatoria discreta. La quantità $\color{#61AFEF}E[X^n]$ con $n\geq 1$ è detta **Momento di ordine $n$** di $X$. E risulta che 
>$$\color{#61AFEF}E[X^n]=\displaystyle\sum_{x} x^n\cdot p(x)$$con $p(x)>0$ per ogni $x$.

Si ha che:
- $E[X^0]=E(1)=\displaystyle\sum_{x} 1\cdot p(x)=1$
- $E[X^1]=E[X]=\displaystyle\sum_{x} x\cdot p(x)$

> [!example]- <font color="orange">Esempio</font>
>>Sia $X$ una variabile aleatoria che assume i valori $\{-1,0,1\}$ con probabilità $$\begin{array}\ p(-1)=0,2 & p(0)=0,5 & p(1) = 0,3\end{array}$$
>>Calcolare $E[X^n]$.
>
>$$\begin{align}E[X^n]&=\sum_{x=-1}^1 x^n\cdot p(x)\\ &=(-1)^n\cdot 0,2 + 0^n\cdot 0,5 + 1^n\cdot 0,3\\ &=(-1)^n\cdot0,2+0,3\end{align}$$
>Quindi $E[X^n]$:
>- $=0,5$ con $n$ pari
>- $=0,1$ con $n$ dispari

### Valori compresi tra due numeri
Se $X$ assume valori compresi tra $a$ e $b$, allora il valore atteso è compreso tra $a$ e $b$. Ovvero che 
$$\mathbb{P}(a\leq X\leq b)= 1\ \Longrightarrow a\leq E[X]\leq b$$ ^1ddd5c

> [!quote] Dimostrazione
>Poiché $p(x)=0$ se $x$ non appartiene ad $[a,b]$, nel caso discreto si ha che 
>
>$$\begin{align}E[X]&=\sum_{x\, :\, p(x)> 0}x\cdot p(x)\\ &=\sum_{a\leq x\leq b}x\cdot p(x)\geq\sum_{a\leq x\leq b}a\cdot p(x)\\ &=a\left(\sum_{a\leq x\leq b}p(x)\right)\\ &=a\end{align}$$

^0f1f1c

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
- [Video (inglese), Statistics 110 - Valore Atteso](https://youtu.be/LX2q356N2rU?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&t=1328)