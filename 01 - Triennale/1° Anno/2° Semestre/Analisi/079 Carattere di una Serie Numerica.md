---
aliases: 
  - Serie Numerica Divergente
  - Serie Numerica Convergente
  - Serie Numerica Irregolare
  - Serie Numerica Indeterminata
tags:
  - corsi/matematica/analisi
paragrafo: Serie Numeriche
cssclasses:
  - 
---
>Data la [[029 Successioni|Successione]] $a_n$ di una [[078 Serie Numeriche|Serie Numerica]] e la sua corrispondente [[078 Serie Numeriche#Successione di Somme Parziali|Successione di Somme Parziali]] $S_n$, si può osservare il comportamento della successione $S_n$ con $\color{#61AFEF}n\to+\infty$.  Questo è detto **carattere** della serie e si individua con:
>$$\color{#CC241D}\lim\limits_{n \to +\infty}S_n=\displaystyle\sum_{n=1}^{+\infty} a_n$$

> [!warning] Calcolare il limite della successione corretta
> Bisogna calcolare il limite della successione delle somme parziali $\{s_n\}_{n\in\mathbb{N}}$ associata alla successione $\{a_n\}_{n\in\mathbb{N}}$ e non il limite della successione della serie stessa.


Si possono avere tre casi.

### Serie Numerica Divergente
>Si dice che una Serie Numerica è <font color="#FF6611"><b>Divergente</b></font> se: $$\color{#FF6611}\lim\limits_{n \to +\infty}S_n=\pm\infty$$

- Se tende a $+\infty$, si dice che *Diverge Positivamente*.
- Se tende a $-\infty$, si dice che *Diverge Negativamente*.


> [!example]- <font color="orange">Esempio</font>
> Sia una successione $\{a_n\}_n$ creata da elementi con $a_n= n, \forall n \in \mathbb{N}$, ovvero $$(a_1=1, a_2=2, a_3=3, \dots, a_{100}=100,\dots)$$
> 
> Si costruisce la successione delle somme parziali: $$S_n=1+2+3+\dots+(n-1)+n$$
> In questo caso particolare, si può osservare che è possibile riscrivere l'intera successione delle somme parziali $S_n$ al contrario, senza cambiare il risultato:
> $$S_n=\textcolor{#CC241D}1+\textcolor{#61AFEF}2+\textcolor{#FF6611}3+\dots+\textcolor{#00C575}{(n-2)}+\textcolor{orange}{(n-1)}+\textcolor{pink}n$$
> $$S_n=\textcolor{#CC241D}n+\textcolor{#61AFEF}{(n-1)}+\textcolor{#FF6611}{(n-2)}+\dots+\textcolor{#00C575}{3}+\textcolor{orange}{2}+\textcolor{pink}1$$
> 
> Sommando tutti i termini di queste due sommatorie e la somma stessa otteniamo questa equazione:
>$$\begin{align*}2S_n&=\textcolor{#CC241D}{(1+n)}+\textcolor{#61AFEF}{(2+n-1)}+\textcolor{#FF6611}{(3+n-2)}+\dots+\textcolor{#00C575}{(n-2+3)}+\textcolor{orange}{(n-1+2)}+\textcolor{pink}{(n+1)}\\ 2S_n&=\underbracket{\textcolor{#CC241D}{(n+1)}+\textcolor{#61AFEF}{(n+1)}+\textcolor{#FF6611}{(n+1)}+\dots+\textcolor{#00C575}{(n+1)}+\textcolor{orange}{(n+1)}+\textcolor{pink}{(n+1)}}_n \end{align*}$$
> 
> Quindi:
> $$\begin{align*}2S_n&=n(n+1) \\ S_n&=\frac{n(n+1)}{2}\end{align*}$$
> 
> 
> Calcolando il limite, andando a sostituire $n$ a $+\infty$, si nota che:
> $$\lim\limits_{n \to +\infty}S_n=\lim\limits_{n \to +\infty}\frac{n(n+1)}{2}=\frac{+\infty(+\infty+1)}{2}=+\infty$$
> 
> Pertanto, la serie numerica $\displaystyle\sum_{n=1}^{+\infty} a_n = \displaystyle\sum_{n=1}^{+\infty} n$ è divergente positivamente.
> 
> Lo si può notare anche dal grafico di $S_n=\frac{n(n+1)}{2}$, dove:
> - $S_1=\frac{1(1+1)}{2}=1$
> - $S_2=\frac{2(2+1)}{2}=3$
> - $S_3=\frac{3(3+1)}{2}=6$
> - $S_4=\frac{4(4+1)}{2}=10$
> - $S_5=\frac{5(5+1)}{2}=15$
> - $\dots$
> ![[Pasted image 20250208123131.png]]

### Serie Numerica Irregolare (o Indeterminata)
>Si dice che una Serie Numerica è <font color="#e86162"><b>Irregolare (o Indeterminata)</b></font> se: $$\color{#e86162}\lim\limits_{n \to +\infty}S_n\text{ non esiste}$$


> [!example]- <font color="orange">Esempio</font>
> Sia una successione $\{a_n\}_n$ creata da elementi con $a_n= (-2)^n, \forall n \in \mathbb{N}$, ovvero $$(a_1=-2, a_2=4, a_3=-8, a_4=16,\dots)$$
> 
> Si può notare che il limite "oscilla" tra valori positivi e negativi crescenti, in base alla parità della $n$. Quindi:
> $$\lim\limits_{n \to +\infty}(-2)^n\text{ non esiste}$$
> 
> Pertanto, la Serie Numerica è Irregolare.
> 
> Infatti, dovrebbe avere due limiti, ma per il [[034 Teoremi per i Limiti di Successioni#Teorema dell'unicità del Limite|Teorema dell'unicità del Limite]] questo non può accadere.
>
>![[Pasted image 20250208135035.png]]

### Serie Numerica Convergente
>Si dice che una Serie Numerica è <font color="orange"><b>Convergente</b></font> se: $$\color{orange}\lim\limits_{n \to +\infty}S_n=l, \quad l\in\mathbb{R}$$

Si dirà quindi che $\color{orange}l$ è la *somma* della serie.



> [!quote] Condizione **NECESSARIA** affinché una Serie Numerica Converga
> Per sapere se la Serie Numerica $a_n$ *POSSA* convergere ad un numero finito reale, si deve considerare se il suo limite è $\color{#61AFEF}0$:
> $$\lim\limits_{n \to +\infty}a_n=0$$
> 
> Se questo limite è effettivamente $0$, allora NON È ANCORA DETTO CHE $a_n$ CONVERGA, ma in questo caso sappiamo che POTREBBE. 
> Per la Condizione **SUFFICIENTE**, *bisogna calcolare anche il limite della successione delle somme parziali $S_n$*:
> $$\lim\limits_{n \to +\infty}S_n$$
> 
> - Se il limite è un numero reale finito $l$, allora converge ad $l$.
> - Se il limite è $\pm\infty$, allora diverge.
> - Altrimenti è irregolare.



> [!example]- <font color="orange">Esempio</font>
> Sia una successione $\{a_n\}_n$ creata da elementi con $a_n= 0, \forall n \in \mathbb{N}$, ovvero $$(a_1=0, a_2=0, a_3=0, \dots, a_{100}=0,\dots)$$
> Ovviamente si ha che $$S_n=0+0+\dots+0+0=0$$
> Quindi la serie numerica converge a $0$, perché $$\lim\limits_{n \to +\infty}a_n=\lim\limits_{n \to +\infty}S_n=\lim\limits_{n \to +\infty}0=0$$
>
>![[Pasted image 20250208121028.png]]
>
>---
>
> Sia una successione $\{a_n\}_n$ creata da elementi con $$a_n= \frac{1}{n(n+1)},\quad \forall n \in \mathbb{N}$$
> Ovvero $$\left( a_1=\frac{1}{2}, a_2=\frac{1}{6}, a_3=\frac{1}{12}, a_4=\frac{1}{20}, \dots\right)$$
>
> Si vuole studiare il carattere di questa serie numerica (Serie di Mengoli, Telescopica) e se converge:
> $$\displaystyle\sum_{n=1}^{+\infty} a_n = \displaystyle\sum_{n=1}^{+\infty} \frac{1}{n(n+1)}$$
> 
> Per prima cosa, si calcola il limite della successione $a_n$:
> $$\begin{align*}\lim\limits_{n \to +\infty}a_n &= \lim\limits_{n \to +\infty} \frac{1}{n(n+1)}\\ &= \frac{1}{+\infty}\\ &= \left[ \text{il limite a }+\infty \text{ di } \frac{1}{x}\text{ è }0 \right] \\ &= 0\end{align*}$$
> 
> ![[Pasted image 20250208124225.png]]
> 
> Allora POTREBBE ESSERE convergente.
> 
> Quindi si trova la successione delle somme parziali (in questo caso attraverso [[075 Integrazione di Razionali Fratti#Caso 1 $ Delta>0$|fratti semplici]]):
> $$\begin{align*}\frac{1}{n(n+1)}&=\frac{A}{n}+ \frac{B}{n+1}\\ &=\frac{A(n+1)+Bn}{n(n+1)}\\ &=\frac{An+A+Bn}{n(n+1)}\\ &=\frac{n(A+B)+A}{n(n+1)}\\ &\begin{cases} A+B=0 \\ A=1 \end{cases}\\ &\begin{cases} B=-1 \\ A=1 \end{cases}\\ S_n&=\frac{1}{n}-\frac{1}{n+1}\end{align*}$$
> 
> Si nota che:
> $$\begin{align*}S_n&=\left( \frac{1}{1} - \frac{1}{1+1} \right)+\left( \frac{1}{2} - \frac{1}{2+1} \right)+\left( \frac{1}{3} - \frac{1}{3+1} \right)+\dots+\left( \frac{1}{n} - \frac{1}{n+1} \right)\\ &=1 -\frac{1}{2}+\frac{1}{2} - \frac{1}{3} + \frac{1}{3} - \frac{1}{4} +\dots+ \frac{1}{n} - \frac{1}{n+1}\\ &=1 \textcolor{#CC241D}{\cancel{-\frac{1}{2}} \cancel{+\frac{1}{2}}} \textcolor{#61AFEF}{\cancel{-\frac{1}{3}} \cancel{+\frac{1}{3}}}\textcolor{#FF6611}{\cancel{-\frac{1}{4}}\underbracket{\cancel{+\frac{1}{4}}}_{\text{si suppone}}} +\dots+ \textcolor{#00C575}{\underbracket{\cancel{-\frac{1}{n}}}_{\text{si suppone}}\cancel{+\frac{1}{n}}} - \frac{1}{n+1}\\ &= 1- \frac{1}{n+1}\end{align*}$$
> 
> Si calcola il limite di questa successione:
> $$\begin{align*}\textcolor{orange}{\lim\limits_{n \to +\infty}\left(1- \frac{1}{n+1}\right)}&=\left( 1-\frac{1}{+\infty} \right)\\ &=\left[ \text{il limite a }+\infty \text{ di } \frac{1}{x}\text{ è }0 \right]\\ &=(1-0)=\color{orange}1\end{align*}$$
> 
> Pertanto, la Serie Numerica $\displaystyle\sum_{n=1}^{+\infty} \frac{1}{n(n+1)}$ converge e la somma è $\color{orange}1$.
> 
> Lo si può notare anche dal grafico:
> 
> ![[Pasted image 20250208133157.png]]


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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/serie-numeriche/728-carattere-di-una-serie.html)
- [Elia Bombardelli](https://youtu.be/zrHz138eyv0?t=7)