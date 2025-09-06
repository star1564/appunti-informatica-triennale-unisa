---
tags:
  - corsi/matematica/probabilità_statistica
paragrafo: Calcolo Combinatorio
inglese:
  - Permutation
cssclasses:
  - 
---
# %% BEGIN PAGE %%
>Una **Permutazione** è una *[[026 Sequenza|sequenza ordinata]]* di $n$ oggetti ottenibile *scambiando gli $n$ oggetti in tutti i possibili modi*. Quindi, [[Considerare l'ordine in Combinatoria|l'ordine viene considerato]].
>
>Si ha che:
>- Una sequenza deve avere tutti gli $n$ elementi
>- Ciascuna sequenza deve differire dalle altre *per l'ordine* con cui gli elementi si susseguono

^bb1733

> [!example]+ <font color="orange">Esempio</font>
> Le possibili permutazioni degli elementi dell'insieme $A=\{a,b,c\}$ sono:
> $$abc, acb, bac, bca, cab, cba$$
> in quanto si differenziano per l'ordine con cui i tre elementi si presentano.
> 
> ![[Pasted image 20250315162125.png|600]]

## Tipi di Permutazioni
Ci sono tre tipi di permutazioni.

### Permutazioni Semplici
>Si dicono **permutazioni semplici** i possibili modi per ordinare totalmente *$n$ elementi distinti tra di loro*. 
>
>Il numero di permutazioni semplici si calcola con il fattoriale di $n$.
>$$\textcolor{#CC241D}{n!}=\begin{cases} 1 \quad\quad\quad\quad\quad\quad\quad\quad\quad\ \text{se }n=0 \\ n\cdot(n-1)\cdot\ldots\cdot2\cdot1 \quad\text{se } n\in \mathbb{N}, n\ne0 \end{cases}$$

![[Pasted image 20240801160457.png|500]]

> [!example]- <font color="orange">Esempio</font>
>Calcolare il numero di permutazioni degli elementi dell'insieme $E=\{a,b,c,d\}$, quindi con $n=4$.
>
>$$4! = 1\cdot2\cdot3\cdot4=24$$

### Permutazioni con Ripetizione
>Si dicono **permutazioni con ripetizione** i possibili modi per ordinare totalmente $n$ elementi *di cui alcuni sono ripetuti due o più volte*.
>
>Nello specifico, supponiamo di avere $n$ elementi, divisi in diverse *categorie*. Si hanno $n_1$ di un tipo, $n_2$ di un altro tipo, e così via fino a $n_k$. Alla fine abbiamo che $$n=n_1+n_2+\ldots+n_k$$
>
>Il numero di permutazioni con ripetizione degli $n$ elementi è uguale al rapporto tra il fattoriale di $n$ e il prodotto dei fattoriali di $n_1,n_2,\ldots,n_k$.
>$$\color{#CC241D}\frac{n!}{n_1!\cdot n_2!\cdot \dotsc \cdot n_k!}$$ 


> [!example]- <font color="orange">Esempio</font>
>Calcolare il numero di permutazioni degli elementi della sequenza $(a,b,a,b,a)$.
>
>La successione è formata da 5 elementi, di cui 3 uguali alla lettera $a$ e 2 uguali alla lettera $b$.
>$$\frac{5!}{3!\cdot2!}=\frac{120}{12}=10$$
>
>---
>Quanti sono gli anagrammi di "statistica"?
>
>In questo caso le lettere di "statistica" *non sono tutte distinte*, abbiamo quindi $\{S,T,A,I\}$.
>Allora si conta il numero di lettere ripetute:
>- S → 2;
>- T → 3;
>- A → 2;
>- I → 2.
>
>Il numero di permutazioni possibili sarà:
>$$\frac{10!}{2!\cdot3!\cdot2!\cdot2!}=75\ 600$$
>
>---
>Si eseguono simultaneamente 10 programmi, di cui 4 sono scritti in C e 6 in Java. Viene poi stilato l'elenco dei programmi nell'ordine di terminazione delle esecuzioni.
>Quanti sono i possibili elenchi dei programmi se è indicato solo il loro linguaggio?
>
>Trattandosi di permutazioni di 10 oggetti non tutti distinti, appartenenti a 2 categorie (con 4 casi nella prima categoria e 6 casi nella seconda categoria, quindi $\{\text{C}, \text{JAVA}\}$), gli elenchi possibili sono: $$\frac{10!}{4!\cdot6!}=210$$


### Permutazioni Circolari
>Si dicono **permutazioni circolari** tutti i possibili modi per ordinare $n$ elementi distinti *da ordinare in modo circolare* e in cui non si può distinguere la prima dall'ultima posizione.
>
>Il numero di permutazioni circolari è uguale al fattoriale di $n-1$.
>$$\frac{n!}{n}=\frac{\cancel{ n }(n-1)!}{\cancel{ n }}=\color{#CC241D}(n-1)!$$

![[Immagine 2023-03-04 165547.png]]


> [!example]- <font color="orange">Esempio</font>
>In quanti modi quattro persone possono sedersi attorno ad una tavola rotonda con sei sedie non numerate?
>
>L'insieme da cui partire è formato da 4 elementi. Poiché il tavolo è rotondo e i posti a sedere non sono numerati, possiamo usare la formula.
>
>$$(4-1)! = 6$$


# %% END PAGE %%
___

[[000 Indice PS|↖ Ritorna all'indice ↖]]
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
- [YouMath - Permutazioni](https://www.youmath.it/lezioni/probabilita/calcolo-combinatorio/5154-permutazione.html)
- [YouMath- P. Semplici](https://www.youmath.it/lezioni/probabilita/calcolo-combinatorio/1212-permutazione-semplice.html)
- [YouMath- P. Ripetizione](https://www.youmath.it/lezioni/probabilita/calcolo-combinatorio/1213-permutazione-con-ripetizione.html)