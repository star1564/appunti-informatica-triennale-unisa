---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Derivate
cssclasses:
  - 
---
La [[046 Derivata di una Funzione|derivata]] di una [[011 Funzioni|funzione]] elementare si ottiene sfruttando la definizione di derivata ed applicandola al caso specifico.

---
### [[F01 Funzione Lineare e Costante#^759638\|Funzione Costante]]

$$f'\left( \textcolor{#61AFEF}{k} \right)=\textcolor{#CC241D}{0}, \quad k\in \mathbb{R}$$



> [!success]- Dimostrazione
> $$
> \begin{align*}
> f'(x) &=\lim\limits_{h \to 0}\frac{f(x+h)-f(x)}{h} \\
> &= \lim\limits_{h \to 0}\frac{\color{#fdaa39}k-k}{h} \\
> &= \lim\limits_{h \to 0} 0 \\
> &= 0
> \end{align*}
> $$
> 
> 
> La funzione $f(x)$ sarà sempre $\color{#fdaa39}k$.
>
>>[!warning] Attenzione
>>Non è la [[038 Forme Indeterminate dei Limiti|forma indeterminata]] $\left[ \frac{0}{0} \right]$, in quanto il numeratore è esattamente zero e non una quantità che tende a zero, dunque il limite è zero.


---
### Potenza [[F02 Funzione Potenza con Esponente Pari|Pari]] e [[F03 Funzione Potenza con Esponente Dispari|Dispari]]

$$f'\left( \textcolor{#61AFEF}{x^s} \right)=\textcolor{#CC241D}{s\cdot x^{s-1}}, \quad s\in \mathbb{R}$$


> [!success]- Dimostrazione
> Prendere per esempio la funzione $f(x)=x^2$.
> 
> $$
> \begin{align*}
> f'(x) &=\lim\limits_{h \to 0}\frac{f(x+h)-f(x)}{h} \\
> &= \lim\limits_{h \to 0} \frac{(x+h)^2-x^2}{h} \\
> &= \lim\limits_{h \to 0} \frac{x^2+2xh+h^2-x^2}{h}  \\
> &= \lim\limits_{h \to 0} \frac{h(2x+h)}{h} \\
> &= \lim\limits_{h \to 0} (2x+h) \\
> &=2x
> \end{align*}
> $$
> 
> 
> 
> Stessa cosa per $f(x)=x^3$.
> 
> $$
> \begin{align*}
> f'(x) &=\lim\limits_{h \to 0}\frac{f(x+h)-f(x)}{h} \\
> &= \lim\limits_{h \to 0} \frac{(x+h)^3-x^3}{h} \\
> &= \lim\limits_{h \to 0} \frac{x^3+3x^2h+3xh^2+h^3-x^3}{h}  \\
> &= \lim\limits_{h \to 0} \frac{h(3x^2+3xh+h^2)}{h} \\
> &= \lim\limits_{h \to 0} 3x^2+3xh+h^2 \\
> &= 3x^2
> \end{align*}
> $$
> 
> 
> Ovvero che, $\forall n \in \mathbb{N}$:
> 
> $$
> \begin{align*}
> f'(x) &=\lim\limits_{h \to 0}\frac{f(x+h)-f(x)}{h} \\
> &= \lim\limits_{h \to 0} \frac{\textcolor{#fdaa39}{(x+h)^n}-x^n}{h} \\
> &= \lim\limits_{h \to 0} \frac{\textcolor{#fdaa39}{x^n+nx^{n-1}h + \left( \frac{n}{2} \right) x^{n-2}h^2+\dots+ h^n}- x^n}{h}  \\
> &= \lim\limits_{h \to 0} \frac{h\left[ nx^{n-1}+\left( \frac{n}{2} \right)x^{n-2}h+\dots + h^{n-1} \right]}{h} \\
> &= n\cdot x^{n-1}
> \end{align*}
> $$
> 
> Il caso per $n\in \mathbb{R}$ la dimostrazione è analoga ma leggermente diversa.




---
### Funzione Identità

$$f'\left( \textcolor{#61AFEF}{x} \right)=\textcolor{#CC241D}{1}$$


> [!success]- Dimostrazione
> Basta riscrivere la $x$ in potenza elevata ad $1$
>$$f(x)=x^1$$ 
>e quindi, grazie alla [[#Potenza F02 Funzione Potenza con Esponente Pari Pari e F03 Funzione Potenza con Esponente Dispari Dispari|derivata della potenza]] si ha che 
>$$f'(x)=1\cdot x^0 = 1\cdot 1 = 1$$




---
### Radice [[F05 Funzione Radice con Indice Pari|Pari]] e [[F06 Funzione Radice con Indice Dispari|Dispari]]

$$f'\left( \textcolor{#61AFEF}{\sqrt[m]{x^n}} \right)=\textcolor{#CC241D}{\frac{n}{m}x^{\frac{n-m}{m}}}$$

> [!success]- Dimostrazione
>Basta riscrivere la radice in potenza secondo la definizione di radicale 
>$$f(x)=x^{\frac{n}{m}}$$ 
>e quindi, grazie alla [[#Potenza F02 Funzione Potenza con Esponente Pari Pari e F03 Funzione Potenza con Esponente Dispari Dispari|derivata della potenza]] si ha che 
>$$f'(x)=\frac{n}{m}x^{\frac{n}{m}-1}=\frac{n}{m}x^{\frac{n-m}{m}}$$


---
### [[FT1 Funzione Seno|Funzione Seno]]


$$f'\left( \textcolor{#61AFEF}{\sin(x)} \right)=\textcolor{#CC241D}{\cos(x)}$$



> [!success]- Dimostrazione
> $$
 \begin{align*}
 f'(x) &=\lim\limits_{h \to 0}\frac{f(x+h)-f(x)}{h} \\
 &= \lim\limits_{h \to 0} \frac{\sin(x+h) - \sin(x)}{h} \\
 &= [\sin(\alpha+\beta)=\sin(\alpha)\cos(\beta)+ \sin(\beta)\cos(\alpha)]  \\
 &= \lim\limits_{h \to 0} \frac{\sin(x)\cos(h) + \sin(h)\cos(x) - \sin(x)}{h} \\
 &= \lim\limits_{h \to 0} \frac{\sin(x)(\cos(x)-1) + \sin(h)\cos(x)}{h} \\
 &= \lim\limits_{h \to 0} \left[\sin(x) \textcolor{#B35DB0}{\frac{(\cos(h)-1)}{h}} + \textcolor{#B35DB0}{\frac{\sin(h)}{h}}\cos(x) \right] \\
 &= [\sin(x)\cdot \textcolor{#B35DB0}0 + \textcolor{#B35DB0}1\cdot \cos(x)] \\
 &= \cos(x)
 \end{align*}
> $$
> 
> Si utilizzano le [[041 Calcolo dei Limiti di una Funzione|regole dei calcoli dei limiti]] insieme al [[040 Limiti Notevoli|limite notevole del seno]], dove il primo limite è conseguenza del limite notevole del seno.


---
### [[FT2 Funzione Coseno|Funzione Coseno]]

$$f'\left( \textcolor{#61AFEF}{\cos(x)} \right)=\textcolor{#CC241D}{-\sin(x)}$$

> [!success]- Dimostrazione
> La situazione è analoga a quella per il [[#FT1 Funzione Seno Funzione Seno|seno]].
> 
> $$
> \begin{align*}
> f'(x) &=\lim\limits_{h \to 0}\frac{f(x+h)-f(x)}{h} \\
> &= \lim\limits_{h \to 0} \frac{\cos(x+h) - \cos(x)}{h} \\
> &= [\cos(\alpha+\beta)=\cos(\alpha)\cos(\beta)- \sin(\alpha)\sin(\beta)]  \\
> &= \lim\limits_{h \to 0} \frac{\cos(x)\cos(h) - \sin(x)\sin(x) - \cos(x)}{h} \\
> &= \lim\limits_{h \to 0} \frac{\cos(x)(\cos(x)-1) - \sin(x)\sin(h)}{h} \\
> &= \lim\limits_{h \to 0} \left[\cos(x) \textcolor{#B35DB0}{\frac{(\cos(h)-1)}{h}} - \textcolor{#B35DB0}{\frac{\sin(h)}{h}}\sin(x) \right] \\
> &= [\cos(x)\cdot \textcolor{#B35DB0}0 - \textcolor{#B35DB0}1\cdot \sin(x)] \\
> &= -\sin(x)
> \end{align*}
> $$
> 
> Si utilizzano le [[041 Calcolo dei Limiti di una Funzione|regole dei calcoli dei limiti]] insieme al [[040 Limiti Notevoli|limite notevole del seno]], dove il primo limite è conseguenza del limite notevole del seno.


---
### Funzione con [[Numero di Nepero]]

$$f'\left( \textcolor{#61AFEF}{e^x} \right)=\textcolor{#CC241D}{e^x}$$


> [!success]- Dimostrazione
> $$
> \begin{align*}
> f'(x) &=\lim\limits_{h \to 0}\frac{f(x+h)-f(x)}{h} \\
> &= \lim\limits_{h \to 0}\frac{e^{x+h}-e^x}{h} \\
> &= \lim\limits_{h \to 0} \frac{e^{x}\cdot e^h-e^x}{h}  \\
> &= \lim\limits_{h \to 0} e^{x}\color{#B35DB0}\frac{e^h-1}{h} \\
> &= e^x\cdot 1 \\
> &= e^x
> \end{align*}
> $$
> 
> Si è usato un [[040 Limiti Notevoli|limite notevole.]]

---
### [[MB - 15 Logaritmi|Funzione Logaritmo Naturale]]

$$f'\left( \textcolor{#61AFEF}{\ln(x)} \right)=\textcolor{#CC241D}{\frac{1}{x}}$$

> [!success]- Dimostrazione
> $$
> \begin{align*}
> f'(x) &=\lim\limits_{h \to 0}\frac{f(x+h)-f(x)}{h} \\
> &= \lim\limits_{h \to 0} \frac{\color{#fdaa39}\ln(x+h)- \ln(x)}{h} \\
> &= \lim\limits_{h \to 0} \frac{\color{#fdaa39}\ln\left( \frac{x+h}{x} \right)}{h} \\
> &= \lim\limits_{h \to 0} \frac{\ln\left( 1+ \frac{h}{x} \right)}{h} \\
> &= \lim\limits_{h \to 0} \color{#B35DB0}\frac{\ln\left( 1+ \frac{h}{x} \right)}{\frac{h}{x}\textcolor{cyan}{\cdot x}} \\
> &= \textcolor{#B35DB0}{1} \cdot \textcolor{cyan}{\frac{1}{x}} \\
> &= \frac{1}{x}
> \end{align*}
> $$
> 
> Si è usata <font color="#fdaa39">qui</font> la [[MB - 15 Logaritmi#^28ff0a|proprietà dei logaritmi del rapporto]].
> Si è usato un [[040 Limiti Notevoli|limite notevole]].


---
### [[F09 Funzione Logaritmica|Funzione Logaritmo]] 

$$f'\left( \textcolor{#61AFEF}{\log_a(x)} \right)=\textcolor{#CC241D}{\frac{1}{x\cdot \ln(a)}}$$


> [!success]- [Dimostrazione](https://www.youmath.it/domande-a-risposte/view/5886-derivata-logaritmo.html)



---
### [[F07 Funzione Esponenziale|Funzione Esponenziale]]

$$f'\left( \textcolor{#61AFEF}{a^x} \right)=\textcolor{#CC241D}{a^x\cdot \ln(a)}$$



> [!success] [Dimostrazione](https://www.youmath.it/domande-a-risposte/view/5921-derivata-esponenziale.html)



---
### [[F10 Funzione Valore Assoluto|Funzione Valore Assoluto]]

$$f'\left( \textcolor{#61AFEF}{|x|} \right)=\textcolor{#CC241D}{\frac{|x|}{x}}$$


> [!warning] Non esiste la derivata prima nel punto 0


> [!success] [Dimostrazione](https://www.youmath.it/domande-a-risposte/view/5914-derivata-del-modulo.html)



---
### [[FT3 Funzione Tangente|Funzione Tangente]]


$$f'\left( \textcolor{#61AFEF}{\tan(x)} \right)=\textcolor{#CC241D}{\frac{1}{\cos^2(x)}}$$


> [!success] [Dimostrazione](https://www.youmath.it/domande-a-risposte/view/5880-derivata-tangente.html)


---
### [[FT4 Funzione Cotangente|Funzione Cotangente]]

$$f'\left( \textcolor{#61AFEF}{\cot(x)} \right)=\textcolor{#CC241D}{-\frac{1}{\sin^2(x)}}$$

> [!success] [Dimostrazione](https://www.youmath.it/domande-a-risposte/view/5906-derivata-cotangente.html)

---
### [[FT5 Funzione Arcoseno|Funzione Arcoseno]]

$$f'\left( \textcolor{#61AFEF}{\arcsin(x)} \right)=\textcolor{#CC241D}{\frac{1}{\sqrt{1-x^2}}}$$

> [!success] [Dimostrazione](https://www.youmath.it/domande-a-risposte/view/5956-derivata-arcoseno.html)


---
### [[FT6 Funzione Arcocoseno|Funzione Arcocoseno]]

$$f'\left( \textcolor{#61AFEF}{\arccos(x)} \right)=\textcolor{#CC241D}{-\frac{1}{\sqrt{1-x^2}}}$$


> [!success]- Dimostrazione analoga a quella dell'Arcoseno


---
### [[FT7 Funzione Arcotangente|Funzione Arcotangente]]

$$f'\left( \textcolor{#61AFEF}{\arctan(x)} \right)=\textcolor{#CC241D}{\frac{1}{1+x^2}}$$


> [!success] [Dimostrazione](https://www.youmath.it/domande-a-risposte/view/5942-derivata-arcotangente.html)

---
### [[FT8 Funzione Arcocotangente|Funzione Arcocotangente]]

$$f'\left( \textcolor{#61AFEF}{\text{arccot}(x)} \right)=\textcolor{#CC241D}{-\frac{1}{1-x^2}}$$



> [!success]- Dimostrazione analoga a quella dell'Arcotangente


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
- [Tabella YouMath con Dimostrazioni](https://www.youmath.it/lezioni/analisi-matematica/derivate/212-derivate-di-funzioni-elementari.html)

- Video dimostrazioni di Elia Bombardelli:
	- [Video 1 (Costanti, Potenze, Radici)](https://www.youtube.com/watch?v=kx8FhlZmglY&list=PLpkXLf6Zhdx3cfwlkGty-daBIHvPj4OuW&index=2)
	- [Video 2 (Seno, Coseno, Esponenziale, Logaritmo Naturale)](https://www.youtube.com/watch?v=UnxHYSngRvk&list=PLpkXLf6Zhdx3cfwlkGty-daBIHvPj4OuW&index=3)


