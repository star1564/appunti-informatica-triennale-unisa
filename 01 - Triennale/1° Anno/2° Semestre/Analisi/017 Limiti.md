---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Limiti
cssclasses:
  - 
---
> [!info] Argomento di introduzione
> Questo è un argomento per introdurre il concetto di limite. Esistono note per i vari tipi di applicazioni dei limiti:
> - [[032 Limiti di Successioni]]
> - [[037 Limiti di Funzioni]]


>Il **Limite** è un operatore che permette di studiare il comportamento di una [[011 Funzioni|funzione]] o di una [[029 Successioni|successione]] nell'[[009 Intorno di un Punto|intorno di un punto]], e grazie al quale possiamo stabilire *a quale valore tende* la funzione man mano che i valori della variabile indipendente si approssimano a quel punto.
>$$\color{#CC241D}\lim\limits_{x \to x_0}f(x)$$
>Dove:
>- $f(x)$ è la funzione (o successione)
>- $x_0$ è il punto in prossimità del quale si vuole studiare il comportamento della funzione, può essere:
>	- un valore reale 
>	- $+\infty$
>	- $-\infty$

Se questa operazione risulterà lecita restituisce un valore finito o infinito come risultato. 

È utile per capire come si comporta una funzione in punti in cui essa *non è definita*.


> [!example]+ <font color="orange">Esempio con grafico</font>
> > [!warning] Queste sono approssimazioni con i grafici
> 
>
> 
>$$\lim\limits_{x \to 1}(x+1)$$
>![[Pasted image 20240930115806.png|500]]
>
>Arrivando da $1^-$ e da $1^+$ si ha lo stesso risultato, quindi:
>$$\lim\limits_{x \to 1}(x+1)=\lim\limits_{x \to 1^-}(x+1)=\lim\limits_{x \to 1^+}(x+1)=2$$
>---
>$$\lim\limits_{x \to x_0}\left( \frac{-3x-2}{x-2} \right)$$
>
>Si ha che $x\neq 2$. Quindi un [[044 Asintoto di una Funzione|asintoto]].
>
>![[Pasted image 20240930121814.png|500]]
>
>In base al valore di $x_0$ possiamo vedere i limiti dal grafico:
>- $x_0=2$, da notare che non è definita per questo valore: $$\lim\limits_{x \to 2^-}\left( \frac{-3x-2}{x-2} \right)=+\infty$$ $$\lim\limits_{x \to 2^+}\left( \frac{-3x-2}{x-2} \right)=-\infty$$
>- $x_0=-\infty$: $$\lim\limits_{x \to -\infty}\left( \frac{-3x-2}{x-2} \right)=0$$Infatti, non raggiunge mai lo zero.
>- $x_0=+\infty$: $$\lim\limits_{x \to +\infty}\left( \frac{-3x-2}{x-2} \right)=0$$Infatti, non raggiunge mai lo zero.


### Tipi di Limiti
Esistono quattro tipi di limiti.
1. Limite per $x$ che *tende a un valore finito* e che dà un *risultato finito* $$\lim\limits_{x \to x_0}f(x)=c$$
2. Limite per $x$ che *tende a un valore finito* e che dà un risultato *infinito* $$\lim\limits_{x \to x_0}f(x)=\pm\infty$$
3. Limite per $x$ che *tende a infinito* e che dà un *risultato finito* $$\lim\limits_{x \to \pm\infty}f(x)=c$$
   ![[Pasted image 20241001160949.png|250]]
4. Limite per $x$ che *tende a infinito* e che dà un *risultato infinito* $$\lim\limits_{x \to \pm\infty}f(x)=\pm\infty$$
   ![[Pasted image 20241001160900.png|500]]

5. Se non rientra in nessuno dei casi precedenti, allora il limite *non esiste*.
   
   ![[Pasted image 20241001161045.png]]
   


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


- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/limiti-continuita-e-asintoti.html)
- [Elia Bombardelli - Introduzione](https://www.youtube.com/watch?v=kDqCKm40mr8&list=PLpkXLf6Zhdx00lP1ul6ez4F5C_VCCRjos)
- [Elia Bombardelli - Introduzione Infinito](https://www.youtube.com/watch?v=U00z7X380TQ)
