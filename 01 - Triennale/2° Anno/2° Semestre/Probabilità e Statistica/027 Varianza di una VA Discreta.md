---
aliases:
  - Variabile Aleatoria Degenere
  - Derivazione Standard
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: V.A. Discrete
cssclasses:
  - 
---
## Dispersione e Derivazione Standard
Sebbene il [[026 Valore Atteso di una VA Discreta|Valore Atteso]] fornisca una [[Media Ponderata|Media Pesata]] dei possibili valori di una [[023 Variabili Aleatorie|Variabile Aleatoria]], esso non dà alcuna informazione riguardo alla *dispersione di questi valori*. 

>La **Dispersione** si riferisce a quanto i dati *si allontanano dalla media o dal valore centrale*. È una misura della variabilità o della differenza tra i valori all'interno di un insieme di dati. Se i dati sono molto dispersi, significa che ci sono grandi differenze tra i valori, mentre se sono poco dispersi, i valori sono più vicini tra loro.

>Al contrario, la **Derivazione Standard** si riferisce a quanto i dati *si avvicinano alla media* o dal valore centrale.

^1f00a4

> [!example]- <font color="orange">Esempio</font>
>- $W=0$ con probabilità $1$
>	- La variabile $W$ ha un solo valore possibile, 0, quindi non c'è dispersione, ma un'alta derivazione standard. Il valore atteso è 0.
>- $Y = \begin{cases} -1 \text{ con probabilità } 1/2 \\ +1 \text{ con probabilità } 1/2 \end{cases}$
>	- La variabile $Y$ può assumere due valori, -1 e +1, entrambi con la stessa probabilità. Anche in questo caso, il valore atteso è 0, ma c'è una dispersione maggiore rispetto a $W$.
>- $Z = \begin{cases} -100 \text{ con probabilità } 1/2 \\ +100 \text{ con probabilità } 1/2 \end{cases}$ 
>	- La variabile $Z$ ha una dispersione molto alta, con valori estremi di -100 e +100. Anche qui, il valore atteso è 0, ma la dispersione è significativamente maggiore rispetto a $Y$.
>
>![[Pasted image 20240819151103.png|700]]

## Definizione Varianza
>Sia $X$ una [[025 Variabili Aleatorie Discrete|variabile aleatoria discreta]] con valore atteso $\color{#61AFEF}\mu=E[X]$, la **Varianza** di $X$ è così definita 
>$$\color{#CC241D}\begin{align}\text{Var}(X) &= E[(X-\mu)^2] \\ &=\sum_{x} \Big((x-\mu)^2\cdot p(x)\Big)\end{align}$$con $p(x)>0$ per ogni $x$.
>
>Si indica spesso con $\color{#CC241D}\sigma^2$.

> [!note] Formula Derivazione Standard
> Si indica spesso con $\color{#CC241D}\sigma$:
> $$\text{DerivazioneStandard}(X)=\sqrt[]{\text{Var}(X)}$$


La varianza è una misura specifica della dispersione. Calcola la media dei quadrati delle differenze tra ogni valore e la media dei valori, fornendo un'indicazione precisa di quanto i dati si discostano dalla media complessiva.

> [!quote] Formula Alternativa
>$$\begin{align}\color{#CC241D}\text{Var}(X)&=\color{#CC241D}E[X^2]-\mu^2\\ &= E[X^2]-[E(X)]^2 \\ & =\sum_{x} \Big(x^2\cdot p(x)\Big)-\left(\sum_{x} x\cdot p(x)\right)^2\end{align}$$
>
>Si ricava dalla formula originale, sapendo che $\color{#61AFEF}\mu=E[X]$: 
>
>$$\begin{align}\text{Var}(X) &= E[(X-\mu)^2] \\ &= E[X^2-2\mu X +\mu^2]\\ &= \text{[proprietà della linearità]} \\ &= E[X^2-2\mu X] +\mu^2 \\ &= E[X^2]-E[2 \mu X] +\mu^2 \\ &= E[X^2]-(2\cdot \mu\cdot \textcolor{#61AFEF}{E[X]}) +\mu^2 \\ &=E[X^2]-(2\cdot \mu \cdot \textcolor{#61AFEF}{\mu})+\mu^2\\ &=E[X^2]-2\mu^2+\mu^2\\ &=E[X^2]-\mu^2\end{align}$$

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20250313130629.png]]

### Proprietà della Linearità
Anche la varianza gode della [[026 Valore Atteso di una VA Discreta#Proprietà di Linearità|proprietà di linearità]]. 

>Sia $X$ una variabile aleatoria discreta. Se $a$ e $b$ sono costanti reali, allora $$\color{#CC241D}\text{Var}(a\cdot X+b)=a^2\cdot \text{Var}(X)$$

### Variabile Aleatoria Degenere
>Una Variabile Aleatoria $X$ si dice **Degenere** se esiste un reale $x_0$ tale che $\color{#61AFEF}p(x_0)=1$. Quindi si ha una funzione di distribuzione $$F(x)=\mathbb{P}(X\leq x)=\begin{cases} 0, & x<x_0 \\ 1, & x\geq x_0 \end{cases}$$
>![[Pasted image 20240819152827.png]]

In questo caso si ha che:
$$E[X]=x_0,\quad \text{Var}(X)=0$$




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