---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Integrali
cssclasses:
  - 
---
>L'**Integrazione per Sostituzione** utilizza il *metodo di sostituzione* e la [[048 Calcolo delle Derivate di una Funzione#Teorema per la Derivata della Funzione Composta|derivazione di una funzione composta]] per ottenere:
>$$\textcolor{#61AFEF}{\int_a^b f(g(x))\cdot g'(x)\ dx} = \color{#CC241D}\int_{g(a)}^{g(b)} f(y)\, dy$$

### Metodo "Ufficiale"

I passaggi da seguire sono i seguenti:
1. *Sostituire* la funzione contenuta all'interno dell'altra $(g(x))$ ad $y$, ovvero: $$\color{#00C575}y=g(x)$$
2. Il *nuovo [[066 Integrali#^7b8a91|differenziale]] $dy$* sarà la [[046 Derivata di una Funzione|derivata]] della funzione che si trova in $y$ in $dx$, ovvero: $$\color{#FF6611}dy=g'(x)\,dx$$
3. Se si deve calcolare un [[066 Integrali#Funzione Qualsiasi e Integrale Definito|integrale definito]], allora anche gli estremi di integrazione si trasformeranno, quindi: 
   $$\color{#61AFEF}a \text{ diventa } g(a)$$
   $$\color{#61AFEF}b \text{ diventa } g(b)$$

![[Pasted image 20241006173658.png|600]]



### Metodo "Pratico"

1. Imposta la $y$, con $$\text{DaSostituire}(x) = y$$
2. Trova la $x$ al primo membro $$x=\text{Risultato}(y)$$
3. Calcola la derivata ad entrambi i membri $$D(x)=D(\text{Risultato}(y))$$
4. Inserisci $dx$ e $dy$ $$D(x)\, dx=D(\text{Risultato}(y))\,dy$$
5. Sostituire: 
	- $\text{DaSostituire}(x)$ con $y$ 
	- $D(x)\,dx$ con $D(\text{Risultato}(y))\,dy$

6. Risolvere l'integrale

> [!warning] Quando si lavora con $y$ e $dy$ non inserire la $c$ alla risoluzione dell'integrale!

7. Sostituire $y$ con $\text{DaSostituire}(x)$
8. Aggiungere la $c$

> [!note] La $c$ assorbe tutti i termini noti
> Ad esempio, $\sin(x)+1+c$ diventa solo $\sin(x)+c$.



> [!example]+ <font color="orange">Esempio 1</font>
>Calcolare il seguente integrale.
> > $$\int \sin(e^x)\cdot e^x\, dx$$
> 
> Poniamo:
> - $\color{#00C575}e^x=y$
> - $\ln(e^x)=\ln(y) \iff \color{#61AFEF}x = \ln(y)$
> - $D\left[\textcolor{#61AFEF}x \right]\, dx = D\left[\textcolor{#61AFEF}{\ln(y)} \right]\, dy \iff\color{#FF6611} dx=\frac{1}{y}\, dy$
>
> Si sostituisce, con $c\in \mathbb{R}$:
> $$\begin{align*}\int \sin(\textcolor{#00C575}{e^x})\cdot \textcolor{#00C575}{e^x}\textcolor{#FF6611}{\, dx} &= \int \sin(\textcolor{#00C575}{y})\cdot\textcolor{#00C575}{y}\cdot \, \textcolor{#FF6611}{\frac{1}{y}dy} \\ &=\int \sin(\textcolor{#00C575}{y})\textcolor{#FF6611}{\, dy} \\ &= -\cos(\textcolor{#00C575}{y}) \\ &= [\text{ripristinare la }x \text{ con }\textcolor{#00C575}{e^x=y}] \\ &= -\cos(\textcolor{#00C575}{e^x}) + c \end{align*}$$

> [!example]+ <font color="orange">Esempio 2</font>
Calcolare il seguente integrale.
> $$\int \sin(\sin(x))\cdot \cos(x)\, dx$$
>
> Poniamo:
> - $\color{#00C575}\sin(x)=y$
> - $\color{#61AFEF}\text{Non posso trovare la }x$
> - $D\left[\textcolor{#61AFEF}{\sin(x)} \right]\, dx= D\left[\textcolor{#61AFEF}y \right]\, dy \iff \color{#FF6611} \cos(x)\,dx = dy$
> 
> Si sostituisce, con $c\in \mathbb{R}$:
> $$\begin{align*}\int \sin(\textcolor{#00C575}{\sin(x)})\cdot \textcolor{#FF6611}{\cos(x)\, dx} &= \int \sin(\textcolor{#00C575}{y})\, \textcolor{#FF6611}{dy} \\ &= -\cos(\textcolor{#00C575}{y}) \\ &= [\text{ripristinare la }x \text{ con } \textcolor{#00C575}{\sin(x)=y}] \\ &= -\cos(\textcolor{#00C575}{\sin(x)}) + c \end{align*}$$

> [!example]+ <font color="orange">Esempio 3</font>
>Calcolare il seguente integrale.
> > $$\int \frac{e^x}{1+e^{2x}}\, dx = \int \frac{e^x}{1+(e^x)^2}\, dx$$
> 
> > [!note] Qui si potrebbe usare [[069 Primitive Fondamentali#^0570b7|questo]] integrale immediato, dato che $D(e^x)=e^x$
>
>
> Poniamo:
> - $\color{#00C575}e^x=y$
> - $\ln(e^x)=\ln(y) \iff \color{#61AFEF}x = \ln(y)$
> - $D\left[\textcolor{#61AFEF}x \right]\, dx = D\left[\textcolor{#61AFEF}{\ln(y)} \right]\, dy \iff\color{#FF6611} dx=\frac{1}{y}\, dy$
> 
> Si sostituisce, con $c\in \mathbb{R}$:
> $$\begin{align*}\int \frac{\textcolor{#00C575}{e^x}}{1+(\textcolor{#00C575}{e^x})^2} \textcolor{#FF6611}{\, dx} &= \int \frac{\textcolor{#00C575}{y}}{1+\textcolor{#00C575}{y}^2} \cdot \textcolor{#FF6611}{\frac{1}{y}\, dy} \\ &= \int\frac{1}{1+\textcolor{#00C575}{y}^2}\, dy \\ &= \arctan(\textcolor{#00C575}y) \\ &= [\text{ripristinare la }x \text{ con }\textcolor{#00C575}{e^x=y}] \\ &= \arctan(\textcolor{#00C575}{e^x}) + c \end{align*}$$


> [!example]+ <font color="orange">Esempio Definito</font>
> Calcolare il seguente integrale.
> >$$\int_0^{\sqrt[]{\pi}} \cos(x^2)\cdot x\, dx$$
> 
> Poniamo:
> - $\color{#00C575}y=x^2$
> - $\color{#FF6611}dy=\frac{d}{dx}\left[y \right]\, dx= 2x\, dx$
> - $\color{#61AFEF}0 \text{ diventa } g(0)=0^2=0$
> - $\color{#61AFEF}0 \text{ diventa } g(\sqrt[]{\pi})=(\sqrt[]{\pi})^2=\pi$
> 
> Si sostituisce, con $c\in \mathbb{R}$:
> $$\begin{align*}\int_{\color{#61AFEF}0}^{\color{#61AFEF}\sqrt[]{\pi}} \cos(\textcolor{#00C575}{x^2})\cdot \textcolor{#FF6611}{x\, dx}  &= [\star]= \int_{\color{#61AFEF}0}^{\color{#61AFEF}\pi} \cos(\textcolor{#00C575}{y})\,  \textcolor{#FF6611}{\frac{dy}{2}} \\ &= \textcolor{#FF6611}{\frac{1}{2}}\cdot \int_{\color{#61AFEF}0}^{\color{#61AFEF}\pi} \cos(\textcolor{#00C575}{y})\,  \textcolor{#FF6611}{dy} \\ &= \frac{1}{2}\cdot \left[ \sin(y)\right]_0^\pi \\ &= \frac{1}{2}\cdot (\sin(\pi)-\sin(0)) \\ &=\frac{1}{2}(0-0) \\ &=0 \end{align*}$$
> 
> $[\star]$: Da notare che la $x\,dx$ è diversa da $\color{#FF6611}dy= 2x\, dx$, quindi si è diviso $dy$ per $2$ per compensare.

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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/integrali/598-metodo-di-integrazione-per-sostituzione.html)
- [Elia Bombardelli](https://www.youtube.com/watch?v=2D2-g93Kljo)
- [Andrea Minini](https://www.andreaminini.org/matematica/integrale/integrazione-per-sostituzione)