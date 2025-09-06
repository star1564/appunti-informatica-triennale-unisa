---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Studio di Funzione
cssclasses:
  - 
---
> [!info] Nota (7/7) sullo [[058 Studio di una Funzione|Studio di una Funzione]]
>[[064 Studio della Derivata Prima|<- Passo precedente]]

Quest'ultimo passaggio è molto simile a quello precedente.

È altamente raro trovare il dominio della [[057 Derivata Seconda|derivata seconda]] diverso dalla derivata prima, quindi si considera sempre che:
$$Dom(f'')=\{x\in Dom(f')\ |\ f' \text{ è derivabile} \}$$

Si inizia con il **calcolo della derivata seconda** della funzione $y=f(x)$, ovvero la derivata della derivata prima: $$y=f''(x)=\frac{d}{dx}[f'(x)]$$
Poi si risolve la disequazione $f''(x)\geq 0$.

> [!note] Modo alternativo
> Oppure si risolvono i seguenti calcoli:
> - $f''(x)=0$ (Per vedere se ci sono punti in cui la retta tangente è parallela all'asse delle $x$)
> - $f''(x)>0$ (Per vedere dove è strettamente positiva la derivata)
> - $f''(x)<0$ (Per vedere dove è strettamente negativa la derivata)

Si ha che:
- Sugli intervalli *positivi*, $f(x)$ è **[[056 Funzioni Convesse e Concave#Funzione Convessa|convessa]]**
- Sugli intervalli *negativi*, $f(x)$ è **[[056 Funzioni Convesse e Concave#Funzione Concava|concava]]**

È consigliato disegnare una curva verso l'alto sotto i segni $+$, mentre una curva verso il basso sotto i segni $-$.

> [!example]+ <font color="orange">Esempio</font>
>Supponiamo di trovarci con questo grafico della disequazione.
>
>![[Pasted image 20220505113253.png|600]]
>
>Abbiamo che: 
>- $f''(x)>0$ per $x<a\ \lor\ b<x<c$
>- $f''(x)<0$ per $a<x<b\ \lor\ x>c$

Adesso bisogna cercare i candidati per i [[057 Derivata Seconda#Punti Di Flesso|punti di flesso]], ovvero si devono considerare i punti in cui la derivata seconda si annulla $f''(x_i)=0$.


---
> [!example]+ <font color="orange">Esempio Caso Studio</font>
> $$y=\frac{\ln(x+1)}{-2-\ln(x+1)}$$$$Dom(f)=(-1, e^{-2}-1)\ \cup\ (e^{-2}-1, +\infty)$$ $$f'(x)=\frac{-2}{(x+1)\cdot(-2-\ln(x+1))^2}$$
> 
> Prima di tutto abbiamo che la derivata prima è composizione e rapporto di funzioni derivabili in ogni punto del proprio dominio.
> $$Dom(f'')=Dom(f')=Dom(f)$$
> 
> Calcoliamo la derivata seconda:
> $$f''(x)=\frac{d}{dx}\left[\frac{-2}{(x+1)\cdot(-2-\ln(x+1))^2}\right]=\frac{2(\ln(x+1)+4)}{(x+1)^2\cdot (2+\ln(x+1))^3}$$
> 
> Osserviamo che il primo fattore del denominatore è sempre positivo in $Dom(f'')$, e vale la stessa cosa per parte del secondo fattore che consideriamo come 
> $$(2+\ln(x+1))^3=(2+\ln(x+1))^2\cdot(2+\ln(x+1))$$
> 
> Al posto dei quadrati, ovunque positivi sul dominio della derivata seconda, scriveremo per brevità $Den$.
> $$f''(x)=\frac{2(\ln(x+1)+4)}{Den\cdot (2+\ln(x+1))}$$
> 
> Consideriamo $f''(x)=0$ da cui ricaviamo l'unica soluzione $x=e^{-4}-1$.
> 
> Per sapere come cambia la convessità della funzione risolviamo la disequazione $f''(x)\geq0$.
> 
> In questo caso possiamo semplificare il denominatore $Den$ perché è sempre positivo su $Dom(f'')$, quindi otteniamo:
> $$\frac{2(\ln(x+1)+4)}{2+\ln(x+1)}\geq0\iff\begin{cases} x\geq e^{-4}-1 \\ x> e^{-2}-1 \end{cases}$$
> 
> Otteniamo il seguente grafico:
> ![[Pasted image 20220505115635.png|600]]
> 
> Poiché $x=e^{-4}-1$ annulla la derivata seconda e determina una variazione di convessità, concludiamo che è un punto di flesso discendente, in cui la funzione assume il valore:
> $$f(e^{-4}-1)=\frac{\ln((e^{-4}-1)+1)}{-2-\ln((e^{-4}-1)+1)}=\frac{-4}{2}= -2$$
> 
> Quindi il <font color="orange">punto di flesso</font> si trova alle coordinate $\color{orange}(e^{-4}-1, -2)$.
> 
> Adesso bisogna disegnare il grafico finale, ottenendo:
> 
> ![[Pasted image 20220505121014.png|800]]

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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/studio-di-funzioni-grafico/183-step-7-studio-della-derivata-seconda-convessita-e-punti-di-flesso.html)