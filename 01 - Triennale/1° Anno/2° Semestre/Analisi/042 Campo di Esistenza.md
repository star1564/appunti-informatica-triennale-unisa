---
aliases:
  - Condizioni di Esistenza
tags:
  - corsi/matematica/analisi
paragrafo: Limiti di Funzioni
cssclasses:
  - 
---

>Il **Campo (o Condizioni) di Esistenza** di una [[011 Funzioni|funzione]] è l'insieme su cui la funzione è definita, o equivalentemente l'insieme di partenza su cui è possibile valutare punto per punto la funzione.

## Calcolo del Dominio
Prima di tutto bisogna dare delle proprietà per *calcolare il dominio di una funzione elementare*:

|                Denominatore                |                   Condizione di Esistenza                   |                  Tipo                   |
| :----------------------------------------: | :---------------------------------------------------------: | :-------------------------------------: |
|            $\frac{N(x)}{D(x)}$             |                        $D(x) \neq 0$                        |      [[MB - 10 Equazioni Fratte]]       |
|                   $ax+b$                   |                         $ax+b\neq0$                         |  [[MB - 04 Equazioni di Primo Grado]]   |
|                $ax^2+bx+c$                 |                     $ax^2+bx+c \neq 0$                      | [[MB - 08 Equazioni di Secondo Grado]]  |
|      $\sqrt[n]{A(x)}, n\text{ pari}$       |                        $A(x)\geq 0$                         | [[F05 Funzione Radice con Indice Pari]] |
|               $\ln_a(B(x))$                |                          $B(x)>0$                           |      [[F09 Funzione Logaritmica]]       |
|            $\log_{A(x)}(B(x))$             | $\begin{cases} A(x)>0 \\ A(x) \neq 1 \\ B(x)>0 \end{cases}$ |      [[F09 Funzione Logaritmica]]       |
|               $A(x)^{B(x)}$                |                          $A(x)>0$                           |      [[F07 Funzione Esponenziale]]      |
| $\tan(A(x))=\frac{\sin(A(x))}{\cos(A(x))}$ |        $A(x)\neq \frac{\pi}{2}+k\pi, k\in\mathbb{Z}$        |        [[FT3 Funzione Tangente]]        |
| $\cot(A(x))=\frac{\cos(A(x))}{\sin(A(x))}$ |             $A(x)\neq \pi+k\pi, k\in\mathbb{Z}$             |       [[FT4 Funzione Cotangente]]       |
|     $\sec(A(x))=\frac{1}{\cos(A(x))}$      |       $A(x)\neq (2k+1) \frac{\pi}{2},k\in\mathbb{Z}$        |                 Secante                 |
|     $\csc(A(x))= \frac{1}{\sin(A(x))}$     |               $A(x)\neq k\pi,k\in\mathbb{Z}$                |                Cosecante                |
|              $\arcsin(A(x))$               |                     $-1\leq A(x)\leq 1$                     |        [[FT5 Funzione Arcoseno]]        |
|              $\arccos(A(x))$               |                     $-1\leq A(x)\leq 1$                     |       [[FT6 Funzione Arcocoseno]]       |
|           $\text{arcsec}(A(x))$            |                $A(x)\leq -1 \lor A(x)\geq 1$                |               Arcosecante               |
|           $\text{arccsc}(A(x))$            |                $A(x)\leq -1 \lor A(x)\geq 1$                |              Arcocosecante              |

Se non compare nessuna delle situazioni descritte, il dominio è tutto $\mathbb{R}$.

## Determinare Il Campo Di Esistenza Di Una Funzione Qualsiasi
Nella maggior parte dei casi potremmo avere delle funzioni che presentano diverse condizioni da imporre contemporaneamente.

> [!example]+ <font color="orange">Esempio</font>
> Trovare il dominio della funzione $$y=\sqrt{\frac{ln(x+2)}{x}}$$
> *Primo Passaggio*: Innanzitutto osserviamo l'espressione analitica della funzione e ci accorgiamo che ci sono tre casi del calcolo del dominio:
> - [[042 Campo di Esistenza#^26fc3a|La radice quadrata]];
> - [[042 Campo di Esistenza#^03c457|Il logaritmo]];
> - [[042 Campo di Esistenza#^3647d7|La frazione]].
> 
> Troviamo il loro dominio:
> - L'argomento della radice deve essere maggiore-uguale a zero $$\frac{ln(x+2)}{x}\geq0$$
> - L'argomento del logaritmo deve essere strettamente maggiore di zero  $$x+2>0$$
> - Il denominatore della frazione deve essere diverso da zero $$x\neq0$$
> 
> *Secondo Passaggio*: Dopo aver applicato le proprietà per trovare i domini, scriviamo quello che abbiamo ottenuto in un sistema: $$\begin{cases} \frac{ln(x+2)}{x}\geq0 \\ x+2>0 \\ x\neq0  \end{cases}$$
> *Terzo Passaggio*: [[MB - 07 Sistemi di Disequazioni|Risolvere il sistema]].
> Prendiamo in considerazione la prima condizione $\frac{ln(x+2)}{x}\geq0$.
> 
> Studiamo separatamente il segno di numeratore e denominatore. Nel caso del numeratore dobbiamo risolvere una disequazione logaritmica.
> 
> Numeratore $\geq 0$: $\ ln(x+2)\geq0\iff(x+2)\geq1\iff x\geq-1$.
> Denominatore $>0$: $\ x>0$.
> 
> Notiamo che la disequazione ammette soluzioni se l'argomento del logaritmo è maggiore di zero. In questo caso $x+2>0\iff x>-2$.
> 
> Tracciamo il grafico della disequazione (con tratteggi necessari) e si prendono le parti con il segno $+$.
> 
> ![[Pasted image 20220408120553.png]]
> 
> Soluzione della prima condizione: $-2<x\leq-1\lor x>0$.
> 
> Per la prima e per la seconda condizione abbiamo che $x+2>0\iff x>-2$ e $x\neq0$.
> 
> Tracciamo il grafico.
> ![[Pasted image 20220408104847.png]]
> 
> Otteniamo il dominio della funzione: $$Dom(f)=\{x\in\mathbb{R}:-2<x\leq-1\ \lor\ x>0\}$$
> 
> Ovvero: $$Dom(f)=(-2,-1]\cup(0,+\infty)$$


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
- [YouMath Calcolo del Dominio](https://www.youmath.it/lezioni/analisi-matematica/le-funzioni-da-r-a-r-in-generale/15-dominio-di-una-funzione-da-r-a-r-cose-come-si-trova-i-parte.html)
- [YouMath Calcolo della Condizione di Esistenza](https://www.youmath.it/lezioni/analisi-matematica/le-funzioni-da-r-a-r-in-generale/16-dominio-di-una-funzione-da-r-a-r-cose-come-si-trova-ii-parte.html)
