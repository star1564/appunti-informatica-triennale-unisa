---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Studio di Funzione
cssclasses:
  - 
---

> [!info] Nota (5/7) sullo [[058 Studio di una Funzione|Studio di una Funzione]]
> [[062 Studio del Segno di una Funzione|<- Passo precedente]] | [[064 Studio della Derivata Prima|Prossimo passo ->]]

>Si vogliono calcolare i [[037 Limiti di Funzioni|limiti]] agli **Estremi del Dominio**, ovvero i "confini" dell'[[059 Studio del Dominio di una Funzione|insieme di definizione]] della funzione $y=f(x)$ considerata.

Grazie a questi, è possibile individuare gli [[044 Asintoto di una Funzione#Asintoto Verticale|asintoti verticali]], [[044 Asintoto di una Funzione#Asintoto Orizzontale|orizzontali]] e [[044 Asintoto di una Funzione#Asintoto Obliquo|obliqui]].

> [!example]+ <font color="orange">Esempio</font>
> $$Dom(f)=(-\infty, a)\ \cup\ (a, b)\ \cup\ [c,d]\ \cup\ (e, +\infty)$$
> 
> In tal caso gli estremi del dominio sono $-\infty, a, b, c, d, e, +\infty$.
> 
> Ora dobbiamo capire come la funzione si comporta in corrispondenza degli estremi del dominio *in cui non è definita*.
> 
> Bisogna quindi calcolare i limiti di $-\infty, a, b, e, +\infty$.
> 
> Dato che non possiamo valutare la funzione in tali punti, dobbiamo considerare il loro [[009 Intorno di un Punto|intorno]] (sinistro o destro). Se uno dei due lati si trova fuori dalle condizioni di esistenza, è inutile calcolarlo.
> 
> Quando si deve calcolare il limite di un punto non definito si considera $elemento^+$ oppure $elemento^-$ in base a dove si trova nell'intervallo.
> 
> Quindi si calcolano i seguenti limiti:
> - $\lim\limits_{x \to -\infty}f(x)$
> - $\lim\limits_{x \to a^-}f(x)$
> - $\lim\limits_{x \to a^+}f(x)$
> - $\lim\limits_{x \to b^-}f(x)$
> - $\lim\limits_{x \to e^+}f(x)$
> - $\lim\limits_{x \to +\infty}f(x)$




> [!example]+ <font color="orange">Esempio Caso Studio</font>
> $$y=\frac{ln(x+1)}{-2-ln(x+1)}$$$$Dom(f)=(-1, e^{-2}-1)\ \cup\ (e^{-2}-1, +\infty)$$
> Dobbiamo calcolare i seguenti limiti (si usa [[055 Teorema di de l_Hôpital|De L'Hôpital]] per alcuni):
> 
> 1. $$
> \begin{align*}
> \textcolor{#CC241D}{\lim\limits_{x \to (-1)^+}}\frac{\ln(x+1)}{-2-\ln(x+1)} &= \left[\frac{\infty}{\infty}\right] \text{ si applica De L'Hopital} \\ 
> &= \lim\limits_{x \to (-1)^+}\frac{\frac{d}{dx}[\ln(x+1)]}{\frac{d}{dx}[-2-\ln(x+1)]} \\
> &= \lim\limits_{x \to (-1)^+}\frac{\frac{1}{x+1}}{-\frac{1}{x+1}} \\
> &= \lim\limits_{x \to (-1)^+}-1 \\
> &= \color{#CC241D} -1
> \end{align*}
> $$
> 
> 
> 
> 
> 2. $$
> \begin{align*}
> \textcolor{#61AFEF}{\lim\limits_{x \to (e^{-2}-1)^-}}\frac{\ln(x+1)}{-2-\ln(x+1)} &= \lim\limits_{x \to (e^{-2}-1)^-} (\ln(x+1)) \cdot \lim\limits_{x \to (e^{-2}-1)^-} \left(\frac{1}{-2-\ln(x+1)}\right) \\
> &= \ln(e^{-2}-1+1) \cdot +\infty \\
> &= \ln(e^{-2}) \cdot +\infty \\
> &= -2\cdot +\infty \\
> &= \color{#61AFEF} -\infty
> \end{align*}
> $$
> 
> 
> 3. $$
> \begin{align*}
> \textcolor{#61AFEF}{\lim\limits_{x \to (e^{-2}-1)^+}}\frac{\ln(x+1)}{-2-\ln(x+1)} &= \lim\limits_{x \to (e^{-2}-1)^+} (\ln(x+1)) \cdot \lim\limits_{x \to (e^{-2}-1)^+} \left(\frac{1}{-2-\ln(x+1)}\right) \\
> &= \ln(e^{-2}-1+1) \cdot -\infty \\
> &= \ln(e^{-2}) \cdot -\infty \\
> &= -2\cdot -\infty \\
> &= \color{#61AFEF} +\infty
> \end{align*}
> $$
> 
> 
>> [!note]- Nota sul limite $\lim\limits_{x \to (e^{-2}-1)} \left(\frac{1}{-2-\ln(x+1)}\right)$
>> La funzione può essere approssimata a $\frac{1}{-x}$, infatti hanno lo stesso comportamento quando il denominatore tende a $0$. Ovvero che tende a:
>> - $+\infty$ se tende a $0^-$
>> - $-\infty$ se tende a $0^+$
>> 
>> ![[Pasted image 20241005123537.png]]
> 
> 
> 4. $$
> \begin{align*}
> \textcolor{green}{\lim\limits_{x \to +\infty}}\frac{\ln(x+1)}{-2-\ln(x+1)} &= \left[\frac{\infty}{\infty}\right] \text{ si applica De L'Hopital} \\ 
> &= \lim\limits_{x \to +\infty}\frac{\frac{d}{dx}[\ln(x+1)]}{\frac{d}{dx}[-2-\ln(x+1)]} \\
> &= \lim\limits_{x \to +\infty}\frac{\frac{1}{x+1}}{-\frac{1}{x+1}} \\
> &= \lim\limits_{x \to +\infty}-1 \\
> &= \color{green} -1
> \end{align*}
> $$
> 
> Dato che $\color{green}\lim\limits_{x \to +\infty}f(x)=-1$ possiamo dire che $y=-1$ è un <font color="green">asintoto orizzontale</font> per la funzione.
> 
> Dato che $\color{#61AFEF}\lim\limits_{x \to (e^{-2}-1)^-}f(x)=-\infty$ e che $\color{#61AFEF}\lim\limits_{x \to (e^{-2}-1)^+}f(x)=+\infty$ possiamo dire che $x=e^{-2}-1$ è un <font color="#61AFEF">asintoto verticale</font> per la funzione.
> 
> Da $\color{#CC241D}\lim\limits_{x \to (-1)^+}=-1$ posiamo notare che la funzione si avvicina al punto $(-1,-1)$ ma non lo riuscirà mai a raggiungerlo.


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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/studio-di-funzioni-grafico/181-step-5-limiti-agli-estremi-del-dominio-ed-eventuali-asintoti.html)