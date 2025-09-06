---
aliases:
  - Divisione tra Polinomi
tags:
  - corsi/matematica/analisi
paragrafo: Integrali
cssclasses:
  - 
---
>Un [[066 Integrali|integrale]] con un [[F04 Funzione Razionale-Frazione|razionale fratto]] è del tipo $$\int \frac{N(x)}{D(x)}\, dx$$
>Per *poterlo risolvere*, le seguenti condizioni devono essere vere:
>- $\color{#CC241D}\text{grado}(D(x)) = 2$
>- $\color{#CC241D}\text{grado}(N(x)) \leq 1$, quindi, solamente con un numero oppure di grado 1

## Risoluzione


> [!important] Prima di risolvere
> Si esegue la [[#Divisione tra Polinomi|divisione tra numeratore e denominatore]] se: 
> $$\color{#CC241D}\text{grado}(N(x))>\text{grado}(D(x))$$
> 
> In questo caso, si separa l'integrale in questo modo:
> $$\int \frac{N(x)}{D(x)}\, dx = \textcolor{#61AFEF}{\int \text{Quoziente}(x) + \frac{\text{Resto}(x)}{D(x)}\, dx} = \int \text{Quoziente}(x)\, dx + \int \frac{\text{Resto}(x)}{D(x)}\, dx$$

>Controllare il [[MB - 08 Equazioni di Secondo Grado#Formula quadratica|Delta]] del polinomio $D(x)$. Ci possono essere tre casi:
>1. $\Delta>0$
>2. $\Delta=0$
>3. $\Delta <0$

### Caso 1: $\Delta>0$
1. [[MB - 08 Equazioni di Secondo Grado#Scomposizione in Fattori|Scomporre il polinomio del denominatore]] per ottenere una moltiplicazione tra due polinomi di grado 1
	$$ax^2+bx+c\iff a\textcolor{#FF6611}{(x-x_1)}\textcolor{#00C575}{(x-x_2)}$$
	- Se è presente anche un numero $a\neq1$, considerarlo come costante e [[070 Proprietà degli Integrali#Omogeneità dell'integrale|spostarlo fuori dall'integrale]]
	- Se il polinomio ha la $a$ negativa, cambiare di segno l'intero polinomio, ricordando di inserire fuori dall'integrale un $-$.
	- Se si ha solamente $ax^2+bx$ allora la scomposizione è semplicemente $x(ax+b)$
  

2. Tenere conto di due frazioni: $$\frac{A}{\textcolor{#FF6611}{(x-x_1)}} + \frac{B}{\textcolor{#00C575}{(x-x_2)}}$$
3. Unire le due frazioni con il [[045 Massimo Comun Divisore|MCD]]: $$\frac{A\cdot \textcolor{#00C575}{(x-x_2)}+B\cdot\textcolor{#FF6611}{(x-x_1)}}{\textcolor{#FF6611}{(x-x_1)}\cdot \textcolor{#00C575}{(x-x_2)}}$$
4. Calcolare le due moltiplicazioni per ottenere: $$\frac{Ax -Ax_2+Bx-Bx_1}{\textcolor{#FF6611}{(x-x_1)}\cdot \textcolor{#00C575}{(x-x_2)}}$$
5. Unire i termini $x$ simili: $$\frac{x(A+B)-Ax_2-Bx_1}{\textcolor{#FF6611}{(x-x_1)}\cdot \textcolor{#00C575}{(x-x_2)}}$$
6. Andare a vedere nell'integrale di partenza $\int \frac{N(x)}{D(x)}\, dx$ quali saranno i valori in $N(x)$ del termine della $x$ e del termine noto.
7. Creare e risolvere questo [[MB - 06 Sistemi di Equazioni Lineari|sistema]] per $A$ e per $B$: $$\begin{cases} A+B=\text{termine della }x\text{ in } N(x) \\ -Ax_2-Bx_1 = \text{termine noto in }N(x) \end{cases}$$
	- Se la $x$ non è presente in $N(x)$, allora questo termine è $0$, quindi si avrà $A+B=0$
8. Una volta ottenuti $\color{#61AFEF}A$ e $\color{#61AFEF}B$, sostituirli in $$\frac{\color{#61AFEF}A}{\textcolor{#FF6611}{(x-x_1)}} + \frac{\color{#61AFEF}B}{\textcolor{#00C575}{(x-x_2)}}$$
9. Inserire queste due frazioni nell'integrale originale, sostituendo la frazione $\frac{N(x)}{D(x)}$ $$\int\frac{\color{#61AFEF}A}{\textcolor{#FF6611}{(x-x_1)}} + \frac{\color{#61AFEF}B}{\textcolor{#00C575}{(x-x_2)}}\,dx=\int\frac{\color{#61AFEF}A}{\textcolor{#FF6611}{(x-x_1)}}\, dx + \int \frac{\color{#61AFEF}B}{\textcolor{#00C575}{(x-x_2)}}\, dx$$
10. Risolvere questi due integrali attraverso gli [[069 Primitive Fondamentali|integrali immediati]] o per altri mezzi.

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20250129171852.png]]


### Caso 2: $\Delta=0$
- Si eseguono gli stessi passaggi del [[#Caso 1 $ Delta>0$|caso 1]], ma questa volta la scomposizione sarà il quadrato di un polinomio di primo grado. Infatti: $$ax^2+bx+c,\text{ con }x_1=x_2 \iff a(x-x_1)(x-x_1)\iff a(x-x_1)^2$$
Inoltre, quando si creano le due frazioni con $A$ e $B$, i loro denominatori saranno con *l'esponente crescente a partire da 1*:
$$\frac{A}{x-x_1} + \frac{B}{(x-x_1)^\textcolor{#00C575}{2}}$$

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20250129172231.png]]


### Caso 3: $\Delta < 0$
In questo caso **bisogna ricondurre la forma dell'integrale** all'integrale immediato: $$\int \frac{\textcolor{#00C575}{f^{\prime}(x)}}{1+[\textcolor{#fdaa39}{f(x)}]^2}\, dx= \text{arctg}(\textcolor{#fdaa39}{f(x)} )+c$$

Si cerca di modificare algebricamente il polinomio di secondo grado al denominatore alla formula $$(a+b)^2=a^2+2ab+b^2$$ e sommando questa ad $1$.

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20250129174426.png]]



## Divisione tra Polinomi
La divisione lunga dei polinomi è simile alla divisione lunga dei numeri interi. 

Si utilizzano queste terminologie:
- <font color="#FF6611">Dividendo</font> - il polinomio da dividere
- <font color="#00C575">Divisore</font> - il polinomio usato per la divisione
- <font color="orange">Quoziente</font> - il risultato della divisione
- <font color="violet">Resto</font> - il resto della divisione


>Questo è il **processo**:
>1. Scrivere i polinomi in *ordine decrescente* rispetto agli esponenti della variabile $x$.
>2. Prendere il primo termine del <font color="#FF6611">dividendo</font> e dividerlo per il primo termine del <font color="#00C575">divisore</font>.
>3. Scrivere questo risultato nel <font color="orange">quoziente</font>.
>4. Moltiplicare il risultato ottenuto per l'intero <font color="#00C575">divisore</font> e sottrarre questo dal <font color="#FF6611">dividendo</font>.
>5. 🔁 Ripetere dal punto 2 con il nuovo polinomio ottenuto dopo la sottrazione fino a quando il grado del <font color="violet">resto</font> è *inferiore* al grado del <font color="#00C575">divisore</font>.


> [!example]- <font color="orange">Esempi Divisione</font>
>![[Pasted image 20250129155643.png|1000]]
>
>---
>$$x^4+3x: x^2-1$$
>![[Pasted image 20241006175748.png]]

> [!example]- <font color="orange">Esempio integrale con grado del numeratore più grande di quello del denominatore</font>
> Calcolare:
> $$\int \frac{x^3-3x-1}{x^2-x-2}\, dx$$
> 
> 1. Si nota che il denominatore ha grado minore del denominatore, quindi si esegue la divisione e si ottiene:
>    ![[Pasted image 20241006175748.png]]
>    $$(x+1)+\frac{1}{x^2-x-2}$$
> 2. $$x^2-x-2=(x-2)(x+1)$$
> 3. $$\begin{align*}\frac{1}{x^2-x-2} &=\frac{A}{x-2} + \frac{B}{x+1} \\ &=\frac{Ax + A + Bx -2B }{(x-2)(x+1)} \\ & =\frac{(A+B)x + A-2B}{(x-2)(x+1)}\end{align*}$$
>    $$\begin{cases} A+B=0 \\ A-2B=1 \end{cases} = \begin{cases} A=-B \\ -B-2B=1 \end{cases}=\begin{cases} A=\frac{1}{3} \\ B=-\frac{1}{3} \end{cases}$$
>    $$\frac{1}{(x-2)(x+1)}=\frac{1}{3}\cdot \frac{1}{x-2} - \frac{1}{3}\cdot \frac{1}{x+1}$$
> 
> 4. $$\begin{align*}\int \frac{x^3-3x-1}{x^2-x-2}\, dx &= \int \left[ x+1+\frac{1}{3}\cdot \frac{1}{x-2} - \frac{1}{3}\cdot \frac{1}{x+1}\right]\, dx \\ &= \int x\, dx + \int 1\, dx + \frac{1}{3} \int \frac{1}{x-2}\, dx - \frac{1}{3} \int \frac{1}{x+1}\, dx \\ &= \frac{x^2}{2} + x + \frac{1}{3}\ln(|x-2|) - \frac{1}{3}\ln(|x+1|) + c \end{align*}$$



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
- [Elia Bombardelli](https://www.youtube.com/watch?v=2D2-g93Kljo)
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/integrali/570-integrazione-di-funzioni-fratte-e-metodo-dei-fratti-semplici.html)