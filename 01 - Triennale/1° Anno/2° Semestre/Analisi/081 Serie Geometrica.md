---
aliases:
  - Serie con Progressione Geometrica
tags:
  - corsi/matematica/analisi
paragrafo: Serie Numeriche
cssclasses:
  - 
---
## Progressione Geometrica
>Una [[079 Carattere di una Serie Numerica|Serie Numerica]] si dice che ha una una **Progressione Geometrica** se, per ogni elemento della [[029 Successioni|successione]] $a_n$, si ha che $$\color{#CC241D}\frac{a_{n+1}}{a_n}=\rho$$
>
>Il numero *unico* $\color{#CC241D}\rho$ si dice **Ragione**.

> [!example]- <font color="orange">Esempio</font>
> 
> Si supponga di avere una successione $\{a_n\}_{n\in\mathbb{N}}$ di potenze di 2 (con $a_n=2^n, \forall n \in \mathbb{N}$):
> - $a_1=2$
> - $a_2=4$,
> - $a_3=8$
> - $a_4=16$
> - $a_5 = 32$
> - $a_6 = 64$
> - $\dots$
> 
> Si ha che:
> - $\frac{a_2}{a_1}=\frac{4}{2}=\color{#CC241D}2$
> - $\frac{a_3}{a_2}=\frac{8}{4}=\color{#CC241D}2$
> - $\frac{a_4}{a_3}=\frac{16}{8}=\color{#CC241D}2$
> - $\frac{a_5}{a_4}=\frac{32}{16}=\color{#CC241D}2$
> - $\frac{a_6}{a_5}=\frac{64}{32}=\color{#CC241D}2$
> - $\dots$
> 
> Pertanto, $\{a_n\}_{n\in\mathbb{N}}$ è una Serie Numerica con *Progressione Geometrica* con ragione $\color{#CC241D}2$.

## Serie Geometrica

> Una **Serie Geometrica** è caratterizzata dalla seguente forma:
> $$\textcolor{#CC241D}{\displaystyle\sum_{n=0}^{+\infty} \rho^n}=1+\rho+\rho^2+\rho^3+\dots+\rho^n, \quad\quad\color{#61AFEF}\rho\in\mathbb{R}$$
> Quindi, la sua [[078 Serie Numeriche#Successione di Somme Parziali|Successione di Somme Parziali]] si calcola in questo modo:
> $$\color{#CC241D}S_n=\begin{cases} \frac{1-\rho^n}{1-\rho} & \text{se }q\neq1 \\ n+1& \text{se }q=1 \end{cases}$$

> [!warning] $\rho$ non può essere una funzione
> Ad esempio:
> - $2, 3, 6, 225 \rightarrow \text{OK}$ 
> - $2+5n \rightarrow \text{NO}$ 

Quindi, per determinare il [[079 Carattere di una Serie Numerica|carattere di una serie numerica]] geometrica, basta fare il limite:
$$\lim\limits_{n \to +\infty}S_n=\begin{cases} \lim\limits_{n \to +\infty}\frac{1-\rho^n}{1-\rho} & \text{se }\rho\neq 1 \\ \lim\limits_{n \to +\infty}n+1& \text{se }\rho=1 \end{cases}$$

Si possono avere tre casi:
$$\color{#61AFEF}\lim\limits_{n \to +\infty}S_n=\begin{cases} \frac{1}{1-\rho} & \text{se }|\rho|< 1 \iff -1<\rho<1 \\ +\infty & \text{se }\rho\geq 1 \\ \text{non esiste} & \text{se }\rho\leq -1 \end{cases}$$

Pertanto, la serie:
$$\displaystyle\sum_{n=0}^{+\infty} \rho^n=\begin{cases} \text{converge con somma } \frac{1}{1-\rho} & \text{se }|\rho|< 1 \iff -1<\rho<1 \\ \text{diverge} & \text{se }\rho\geq 1 \\ \text{indeterminata} & \text{se }\rho\leq -1 \end{cases}$$


> [!example]- <font color="orange">Esempi</font>
> Tornando all'esempio di $\displaystyle\sum_{n=1}^{+\infty} 2^n$, si può notare che ha la forma di una serie geometrica, dove la ragione è $2$, quindi:
> $$\displaystyle\sum_{n=1}^{+\infty} \rho^n=\displaystyle\sum_{n=1}^{+\infty} 2^n$$
> 
> In questo caso $2=\rho\geq1$, pertanto questa diverge a $+\infty$.
> 
> ---
> $$\displaystyle\sum_{n=0}^{+\infty} \left( \frac{1}{2} \right)^n$$
> Per verificare la condizione necessaria, si nota che: 
> $$\lim\limits_{n \to +\infty}\left( \frac{1}{2} \right)^n=\left( \frac{1}{2} \right)^{+\infty}=\left( \frac{1^{+\infty}}{2^{+\infty}} \right)=\left( \frac{1}{+\infty} \right)=0$$
> 
> ![[Pasted image 20250208152515.png]]
> 
> Per la condizione sufficiente, si nota che ha la forma di una serie geometrica, dove la ragione è $\frac{1}{2}$, quindi
> $$\displaystyle\sum_{n=0}^{+\infty} \rho^n=\displaystyle\sum_{n=0}^{+\infty} \left( \frac{1}{2} \right)^n$$
> In questo caso $-1<\frac{1}{2}<1$, pertanto converge con la somma $$\frac{1}{1-\frac{1}{2}}=2$$



___
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

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
- [[079 Carattere di una Serie Numerica#Serie Numerica Convergente|Serie Numerica Convergente]]
- [[079 Carattere di una Serie Numerica#Serie Numerica Divergente|Serie Numerica Divergente]]
- [[079 Carattere di una Serie Numerica#Serie Numerica Irregolare (o Indeterminata)|Serie Numerica Indeterminata]]
- [Elia Bombardelli](https://youtu.be/NJ4O1n3zcmg?si=4fqK4lK7C4nz9og1)