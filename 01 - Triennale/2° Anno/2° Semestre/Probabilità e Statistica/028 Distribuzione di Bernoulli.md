---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Distribuzioni V.A. Discrete
cssclasses:
  - 
---
>Una [[023 Variabili Aleatorie|Variabile Aleatoria]] $X$ ha una [[024 Funzione di Distribuzione|distribuzione]] di **Bernoulli** se può avere *solo i due valori 0 e 1*, ovvero che
>$$\color{#CC241D}\mathbb{P}(X=1)=p,\quad \mathbb{P}(X=0)=1-p$$
>
>Dove $0\leq p\leq 1$ è una qualsiasi probabilità.

Questa è chiamata anche **variabile indicatrice**, un tipo di variabile aleatoria che assume il valore di 1 o 0 per *indicare il verificarsi o meno di un [[011 Evento|Evento]]* specifico all'interno di uno [[010 Spazio Campionario|Spazio Campionario]].
$$X=\begin{cases} 1 & \text{se si ha un successo} \\ 0 & \text{altrimenti} \end{cases}$$ ^cb35ca

Il valore atteso di una variabile indicatrice di un evento è uguale alla [[016 Definizione di Probabilità|probabilità]] dell'evento stesso.

#### Densità Discreta
>La sua [[025 Variabili Aleatorie Discrete#Densità Discreta|Densità Discreta]] può essere espressa in questo modo:
>$$\color{#CC241D}p(x)=p^x \cdot (1-p)^{1-x}=\begin{cases}1-p, & x=0 \\ p, & x=1 \end{cases}$$

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20240820113321.png|500]]

#### Funzione di Distribuzione
La sua [[024 Funzione di Distribuzione|Funzione di Distribuzione]] è la seguente:
$$F(x)=\displaystyle\sum_{k\leq x} p(k)=\displaystyle\sum_{k\leq x} p^k(1-p)^{1-k}= \begin{cases} 0, & x<0 \\ p(0)=1-p, & 0\leq x< 1 \\ p(0) + p(1) = 1, & x\geq1 \end{cases}$$

![[Pasted image 20240820111704.png|500]]

#### Valore Atteso
Il suo [[026 Valore Atteso di una VA Discreta|Valore Atteso]] è il seguente:
$$\color{#CC241D}E[X]=p$$

> [!quote] Dimostrazione
> Sapendo che $\color{#61AFEF}p(1)=p$:
> $$\begin{align}E[X] &= \sum_{x=0}^{1} x\cdot p(x)\\ &=0\cdot p(0) + 1\cdot p(1) \\ &= \textcolor{#61AFEF}{p(1)} \\ &= p\end{align}$$

#### Momento di Ordine $n$
Il suo [[026 Valore Atteso di una VA Discreta#Momento di Ordine $n$|Momento di Ordine n]] è il seguente:
$$\color{#CC241D}E[X^n]=p$$


> [!quote] Dimostrazione
> Sapendo che $\color{#61AFEF}p(1)=p$:
>$$\begin{align}E[X^n]&=\sum_{x=0}^{1} x^n\cdot p(x)\\ &= 0^n\cdot p(0) + 1^n\cdot p(1) \\ &= \textcolor{#61AFEF}{p(1)} \\ &=p\end{align}$$


#### Varianza
La sua [[027 Varianza di una VA Discreta|Varianza]] è la seguente:
$$\color{#CC241D}\text{Var}(X)=p\cdot(1-p)$$
> [!quote] Dimostrazione
> Sapendo che:
> - $\color{#61AFEF}E[X]=p$
> - $\color{#00C575}E[X^n]=p$
> 
> Si ha che:
> $$\begin{align}\text{Var}(X)&=\textcolor{#00C575}{E[X^2]}-(\textcolor{#61AFEF}{E[X]})^2\\ &= \textcolor{#00C575}p-\textcolor{#61AFEF}p^2 \\ &= p\cdot(1-p)\end{align}$$

Si nota che $X$ è una [[027 Varianza di una VA Discreta#Variabile Aleatoria Degenere|Variabile Aleatoria Degenere]], perché la varianza è $0$ con $p=0$ e $p=1$.

Inoltre, la varianza è massima per $p=1/2$.

![[Pasted image 20240820114017.png|500]]

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
- [Video (inglese), Statistics 110 - Bernoulli (Introduzione)](https://youtu.be/PNrqCdslGi4?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&t=2580)
- [Video (inglese), Statistics 110 - Bernoulli (Valore Atteso)](https://youtu.be/LX2q356N2rU?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&t=1452)
