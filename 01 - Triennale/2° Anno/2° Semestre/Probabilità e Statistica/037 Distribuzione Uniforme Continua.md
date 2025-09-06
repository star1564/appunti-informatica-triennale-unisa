---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Distribuzioni V.A. Continue
cssclasses:
  - 
---
>La [[024 Funzione di Distribuzione|distribuzione]] **Uniforme Continua** attribuisce *la stessa probabilità a tutti i punti* appartenenti ad un dato [[004 Intervalli|intervallo]] $(a,b)$.

#### Funzione di Densità
>La sua [[034 Variabili Aleatorie Continue#Funzione di Densità|Funzione di Densità]] può essere espressa in questo modo con un intervallo $(0,1)$:
>$$\color{#61AFEF}f(x)=\begin{cases} 1 & 0<x<1 \\ 0& \text{altrimenti} \end{cases}$$

Risulta che: 
- $f(x)\geq 0$
- $\int_{-\infty}^{\infty} f(x)dx=1$

Inoltre, per ogni $0<a<b<1$ si ha che: $$\mathbb{P}(a\leq X\leq b) = \int_a^b f(x)dx=\int_a^b dx = b-a$$

>Più in generale, $X$ è una variabile aleatoria uniforme sull'intervallo $(\alpha, \beta)$ se ha la seguente densità:
>$$\color{#CC241D}f(x)=\begin{cases} \frac{1}{\beta-\alpha} & \alpha<x<\beta \\ 0& \text{altrimenti} \end{cases}$$


![[Pasted image 20240821112547.png|400]]


#### Funzione di Distribuzione
La sua [[024 Funzione di Distribuzione|Funzione di Distribuzione]] è la seguente con intervallo $(\alpha, \beta)$:
$$F(x)=\begin{cases} 0, & x<\alpha \\ \frac{x-\alpha}{\beta-\alpha}, & \alpha\leq x< \beta \\ 1 & x\geq \beta\end{cases}$$

![[Pasted image 20240821112854.png|400]]

Infatti, per $a<x<\beta$ si ha $f(x)=\frac{1}{\beta-\alpha}$, quindi:
$$F(x)=\int_{-\infty}^{x} f(u)du = \int_a^x \frac{1}{\beta-\alpha}du=\frac{1}{\beta-\alpha}\int_a^x du=\frac{x-\alpha}{\beta-\alpha}$$



#### Valore Atteso
Il suo [[035 Valore Atteso di una VA Continua|Valore Atteso]] è il seguente, per $n=1,2$:
$$\textcolor{#CC241D}{E[X]=} \frac{\beta^2-\alpha^2}{2(\beta-\alpha)}=\frac{(\beta-\alpha)(\beta+\alpha)}{2(\beta-\alpha)}=\color{#CC241D}\frac{\alpha+\beta}{2}$$

$$\textcolor{#CC241D}{E[X^2]=} \frac{\beta^3-\alpha^3}{3(\beta-\alpha)}=\frac{(\beta-\alpha)(\beta^2+\alpha\beta+\alpha^2)}{3(\beta-\alpha)}=\color{#CC241D}\frac{\beta^2+\alpha\beta+\alpha^2}{3}$$


#### Varianza
La sua [[036 Varianza di una VA Continua|Varianza]] è la seguente:
$$\textcolor{#CC241D}{\text{Var}(X)=}\frac{\beta^2+\alpha\beta+\alpha^2}{3}-\frac{(\alpha+\beta)^2}{4}=\frac{4(\beta^2+\alpha\beta+\alpha^2)-3(\beta^2+2\alpha\beta+\alpha^2)}{12}=\frac{\beta^2-2\alpha\beta+\alpha^2}{12}=\color{#CC241D}\frac{(\beta-\alpha)^2}{12}$$

Si ha che:
- Se $\alpha$ e $\beta$ sono distanti, allora la varianza di $X$ è grande e quindi il valore atteso di $X$ è poco significativo
- Se $\alpha$ e $\beta$ sono vicini, allora la varianza di $X$ è piccola e quindi il valore atteso di $X$ è molto significativo


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
- [[033 Distribuzione Uniforme Discreta]]