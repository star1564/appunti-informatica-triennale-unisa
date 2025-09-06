---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Aritmetica
cssclasses:
  - 
---
Sia $S$ uno [[055 Spazi Vettoriali|Spazio Vettoriale]] su F [[054 Strutture Algebriche#CAMPO $(A, +, cdot)$|Campo]] e $\color{#61AFEF}V \subseteq S$. 
$V$ è un **sottospazio** di $S$ se e solo se (dove $\underline{0}$ è un vettore nullo): ^3a6327
1. $\color{#61AFEF}\underline{0}\in V$;
2. Se $\underline{u}, \underline{v}\in V$ allora $\color{#61AFEF}\underline{u}+\underline{v}\in V$;
3. Se $\alpha\in F$ e $\underline{v}\in V$ allora $\color{#61AFEF}\alpha\underline{v}\in V$.

In ogni spazio vettoriale ci sono sempre due sottospazi, che vengono chiamati *sottospazi banali*:
- Il sottospazio nullo $\{\underline{0}\}=\{(0,0)\}$;
- L'insieme $S$.

> [!example]- <font color="orange">Esempio</font>
>Spazio Vettoriale: $\mathbb{R}^2=\{(x,y)|x,y\in \mathbb{R}\}$
>$V=\{(x, x)|x\in \mathbb{R}\}$ è un sottospazio
>$T=\{(x, 1)|x\in \mathbb{R}\}$ NON è un sottospazio perché $\underline{0}=(0,0)\not\in T$
>$W=\{(0, y)|y\in \mathbb{R}\}$ è un sottospazio

## CRITERIO DI RICONOSCIMENTO DEI SOTTOSPAZI

Serve per riconoscere se un insieme è un sottospazio.
Se $V\subseteq S$ spazio vettoriale, allora $V$ è un sottospazio se e solo se si verifica la seguente *combinazione lineare*:

> $$\textcolor{red}{\underline{0}\in V};\quad \forall\alpha,\beta\in F,\ \forall \underline{u},\underline{v}\in V: \color{red}\alpha\underline{u} + \beta\underline{v}\in V$$


> [!example]- <font color="orange">Esempio</font>
>Utilizziamo il Criterio di Riconoscimento dei Sottospazi per determinare se i seguenti sono dei Sottospazi.
>
>Spazio Vettoriale $\mathbb{Q}^2=\{(x,y)|x,y\in \mathbb{Q}\}$
>___
>$\color{#CC241D}W_1=\{(x,y)\in \mathbb{Q}^2|x=y\}$
>Si nota subito che questo è un sottospazio, perché abbiamo una coppia uguale:
>$W_1=\{(x,y)\in \mathbb{Q}^2|x=y\} = \{\textcolor{#61AFEF}{(t,t)}|t\in \mathbb{Q}\}$
>
>Dobbiamo arrivare a: $\color{#61AFEF}(y,y)$.
>
>- $(0,0)\in W_1$, perché $\color{#61AFEF}0=0$;
>- $\forall \alpha, \beta\in \mathbb{Q}, \forall (t,t),(k,k)\in W_1$
>	$\alpha(t,t)+\beta(k,k)=(\alpha t, \alpha t)+(\beta k, \beta k)$
>	$=(\alpha t+\beta k,\alpha t+\beta k) = \color{#61AFEF}(y,y)$.
>___
>$\color{#CC241D}W_2=\{(x,y)\in \mathbb{Q}^2|x=2y\}$
>Si nota che questo è un sottospazio, perché abbiamo il risultato corretto:
>$W_2=\{(x,y)\in \mathbb{Q}^2|x=2y\} = \{\textcolor{#61AFEF}{(2t,t)}|t\in \mathbb{Q}\}$
>
>Dobbiamo arrivare al risultato: $\color{#61AFEF}(2y,y)$.
>
>- $(0,0)\in W_2$, perché $\color{#61AFEF}0=2\cdot0$, quindi $\color{#61AFEF}(2\cdot0,0)\in W_2$;
>- $\forall \alpha, \beta\in \mathbb{Q}, \forall (2t,t),(2k,k)\in W_2$
>	$\alpha(2t,t)+\beta(2k,k)=(2\alpha t, \alpha t)+(2\beta k, \beta k)$
>	$=(2\alpha t+2\beta k,\alpha t+\beta k) = (2(\alpha t+\beta k),\alpha t+\beta k) = \color{#61AFEF}(2y,y)$.
>---
>$\color{#CC241D}W_3=\{(x,y)\in \mathbb{Q}^2|x+y=1\}$
>Si nota subito che questo NON è un sottospazio, perché:
>- $(0,0)\in W_3$, ma $\color{#61AFEF}0+0\neq 1$.
>---
>$\color{#CC241D}W_4=\{(x,y)\in \mathbb{Q}^2|x=0\}$
>Si nota subito che questo è un sottospazio, perché:
>$W_4=\{(x,y)\in \mathbb{Q}^2|x=0\}=\{\textcolor{#61AFEF}{(0,t)}|t\in\mathbb{Q}\}$
>
>Dobbiamo arrivare al risultato: $\color{#61AFEF}(0,y)$.
>
>- $(0,0)\in W_2$, perché $x$ è sempre $0$ ed $y$ può essere $0$;
>- $\forall \alpha, \beta\in \mathbb{Q}, \forall (0,t),(0,k)\in W_1$
>	$\alpha(0,t)+\beta(0,k)=(\alpha 0, \alpha t)+(\beta 0, \beta k)=(0, \alpha t)+(0, \beta k)$
>	$=(0+0,\alpha t+\beta k) = (0,\alpha t+\beta k) = \color{#61AFEF}(0,y)$.
>___
>$\color{#CC241D}W_5=\{(x,y)\in \mathbb{Q}^2|x^2-y^2=0\}$
>Si nota che questo NON è un sottospazio, perché:
>$x^2-y^2=0 \iff \textcolor{#61AFEF}{x^2=y^2} \iff \begin{cases} x=y \\ x=-y \end{cases}$
>
>Dobbiamo arrivare al risultato: $\color{#61AFEF}(y\land-y,y)$.
>
>- $(0,0)\in W_5$, perché $\color{#61AFEF}0^2=0^2 \iff 0=0$;
>- $\forall \alpha, \beta\in \mathbb{Q}, \forall (t,t),(k,k)\in W_5$
>	$\alpha(t,t)+\beta(k,k)=(\alpha t, \alpha t)+(\beta k, \beta k)$
>	$=(\alpha t+\beta k,\alpha t+\beta k) = \color{#61AFEF}(y,y)$.
>
>Ma il $-y$ non si può trovare, un esempio sono le coppie:
>$(1,1),(1,-1)\in W_5$
>$(1,1)+(1,-1)=(2,0)\not\in W_5$


## COMBINAZIONE LINEARE E SOTTOSPAZIO GENERATO
Sia $S$ uno spazio vettoriale su un campo $F$. 
Consideriamo i vettori $\underline{v}_1, \underline{v}_2, ..., \underline{v}_n\in S$ e gli scalari $\alpha_1, \alpha_2, ..., \alpha_n\in F$.
Si chiama **Combinazione Lineare** dei vettori considerati secondo gli scalari considerati, il seguente vettore:
$$\color{#61AFEF}\alpha_1\cdot\underline{v}_1+\alpha_2\cdot\underline{v}_2+...+\alpha_n\cdot\underline{v}_n$$

Fissati $n$ vettori $\underline{u}_1, ..., \underline{u}_n\in S$ posso considerare l'insieme di tutte le combinazioni lineari di questi vettori
$$\textcolor{#61AFEF}{<\underline{u}_1, ..., \underline{u}_n>} = \{\alpha_1\cdot\underline{u}_1+...+\alpha_n\cdot\underline{u}_n|\alpha_1, ..., \alpha_n\in F\}$$

Questo è un sottospazio detto **sottospazio generato da** $\underline{u}_1, ... \underline{u}_n$.

È sempre vero che: $\underline{0}\in <\underline{u}_1, ...\underline{u}_n>$.

<font color="orange">Esempio</font>:
$<(-3,1), (5,0)> =\{\alpha(-3,1)+\beta(5,0)|\alpha,\beta\in \mathbb{R}\}=\{(-3\alpha,\alpha1)+(5\beta,\alpha0)|\alpha,\beta\in \mathbb{R}\}$$=\color{#61AFEF}\{(-3\alpha+5\beta,\alpha)|\alpha,\beta\in \mathbb{R}\}$.
*Sottospazio di* $\color{#61AFEF}\mathbb{R}^2$ *generato da* $\color{#61AFEF}(-3,1),(5,0)$.

## SISTEMA DI GENERATORI
Sia $W$ un sottospazio di $S$, se esistono $\underline{w}_1, ...\underline{w}_t$ tali che $W=<\underline{w}_1, ...\underline{w}_t>$ si dice che $X=\{\underline{w}_1, ...\underline{w}_t\}$ è un **Sistema di Generatori** per $W$.

> Ogni sottospazio possiede un sistema finito di generatori.

### TROVARE IL SISTEMA DI GENERATORI
Data: $W=\{(x,y,z)\in\mathbb{R}^3|x-2y+z=0\}=\{(x,y,z)\in\mathbb{R}^3|x=2y-z\}$ $= \{(2y-z,y,z)^3|y,z\in\mathbb{R}\}$.
Abbiamo notato che questo è un sottospazio. Dobbiamo trovarne il sistema di generatori.

Prendiamo il generico vettore di $W$ e *scomponiamolo*:
$(2y-z,y,z)=(2y,y,0)+(-z,0,z)=\color{#61AFEF}y(2,1,0)+z(-1,0,1)$.

È combinazione lineare di:
$\underline{u}_1=(2,1,0),\ \underline{u}_2=(-1,0,1)$
$\forall \underline{w}\in W,\quad \underline{w}\in <\underline{u}_1,\underline{u}_2>,\quad \color{#61AFEF}W\subseteq <\underline{u}_1,\underline{u}_2>$.

Ma $\underline{u}_1, \underline{u}_2\in W;\quad \forall \alpha,\beta\in \mathbb{R}, \alpha\underline{u}_1+\beta\underline{u}_2\in W,\quad \color{#61AFEF}<\underline{u}_1,\underline{u}_2>\subseteq W$

Quindi $W = <\underline{u}_1,\underline{u}_2>,\quad \color{#CC241D}X=\{\underline{u}_1,\underline{u}_2\}$.
___
[[000 Indice MD|↖ Ritorna all'indice ↖]]

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
- [[004 Combinazione Lineare, Conica e Convessa|In Ricerca Operativa]]