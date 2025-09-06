---
aliases:
  - Funzione Derivabile
  - Derivata Prima
tags:
  - corsi/matematica/analisi
paragrafo: Derivate
cssclasses:
  - 
---
>La **Derivata** è il [[037 Limiti di Funzioni|limite]] del [[045 Rapporto Incrementale|rapporto incrementale]] della [[011 Funzioni|funzione]] nel punto al tendere dell'incremento a zero.
>
>È essa stessa una funzione che *rappresenta la sensitività del cambiamento* della funzione originale.

Considerare una funzione $f:\mathbb{R}\to\mathbb{R}$, dove $y=f(x)$, e fissare un certo punto $x_0$ appartenente al dominio della funzione.

Si vuole trovare un modo che consente di calcolare l'equazione della *retta tangente* al grafico della funzione $f(x)$ nel punto di ascissa $x_0$. Ovvero la retta in <font color="61AFEF">blu</font>.


![[Pasted image 20220416124500.png]]

Quindi si deve trovare il coefficiente angolare $\color{yellow}m$ nella seguente equazione della retta: $y-f(x_0)=\textcolor{yellow}{m}(x-x_0)$.

Si introduce un *secondo punto sull'asse delle ascisse*, $x_0+h$, dove $h$ rappresenta la distanza tra i due punti.
Si vuole trovare il rapporto incrementale della nuova retta secante che otteniamo unendo i due punti.

![[Pasted image 20220416124938.png]]

Questo lo si trova con la formula del [[045 Rapporto Incrementale|rapporto incrementale]]: $$m_{secante}=\frac{\Delta y}{\Delta x}=\frac{f(x_0+h)-f(x_0)}{h},\quad h\neq0$$
Si nota ora che più il secondo punto si avvicina al primo, più la retta secante tende a diventare la retta tangente, finché idealmente le due *coincidono* quando i due punti vanno a sovrapporsi. 

> [!tip]+ Questo lo si nota nella seguente animazione
>![[secante-a-tangente.gif|700]]

### Funzione Derivabile
>Per far avvicinare i due punti e formare una retta tangente alla funzione, si fa *tendere $h$ a $0$*. In formula: $$\textcolor{yellow}{m_{tangente}}=\lim\limits_{\Delta x \to 0}\frac{\Delta y}{\Delta x}=\color{#CC241D}\lim\limits_{h \to 0}\frac{f(x_0+h)-f(x_0)}{h}$$
>Se questo limite *esiste ed è finito*, la funzione $y=f(x)$ si dice **Derivabile** nel punto di ascissa $x_0$. 
>
>Mentre il valore che il limite assume prende il nome di **Derivata di $\color{#CC241D}f$ in $\color{#CC241D}x_0$**.


La derivata si indica in vari modi: $$y';\quad f'(x_0);\quad D(f(x_0));\quad \frac{d}{dx}(x_0)$$

Pertanto, il coefficiente angolare dell'equazione della retta tangente equivale alla derivata di $x_0$, ovvero  $$y-f(x_0)=\textcolor{yellow}{m}(x-x_0)=y-f(x_0)=\textcolor{yellow}{f'(x_0)}(x-x_0)$$ 

> [!example]+ <font color="orange">Esempio</font>
> Considerare la seguente funzione e notare come la derivata (in rosso) diventa zero quando il coefficiente angolare $\textcolor{yellow}{m}=0$.
> 
> ![[Pasted image 20241004185319.png|600]]
> 
> Si può notare la crescita e la decrescita della derivata attraverso questa animazione.
> 
> ![[Tangent_function_animation.gif]]


### Derivata Prima
Se $f(x)$ è derivabile per ogni $x_0\in A$, la funzione si dice *derivabile in $A$*. 

>Allora si può introdurre una nuova funzione, che ad ogni $x_0\in A$ fa corrispondere $f'(x_0)$, con il nome di **Derivata Prima**. 
>$$y = f'(x)$$
>$$x_0\mapsto f'(x_0)$$

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
- [Elia Bombardelli](https://www.youtube.com/watch?v=yHyPJ0_ENdk)
- [Wikipedia EN](https://en.wikipedia.org/wiki/Derivative)