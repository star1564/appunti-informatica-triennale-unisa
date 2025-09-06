---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Calcolo Combinatorio
cssclasses:
  - 
---
>Una **Combinazione** è una sequenza di $k$ oggetti *presi in qualsiasi ordine*, **estratti** da un insieme che ne contiene $n$ distinti. Quindi, [[Considerare l'ordine in Combinatoria|l'ordine NON viene considerato]].
>
>Si ha che:
>- In ogni sequenza ci sono esattamente $k$ elementi
>- Ciascuna sequenza deve differire dalle altre *per almeno un elemento*, ma *NON per l'ordine*.

![[Pasted image 20230930145803.png|400]]

> [!example]+ <font color="orange">Esempio</font>
>Consideriamo l'insieme $A=\{a,b,c\}$ formato da $n=3$ elementi, e sia $k=2$.
>
>Ogni combinazione con $k=2$ è un raggruppamento formato da 2 elementi di $A$. Inoltre ogni combinazione si distingue da un'altra se cambia almeno un elemento:
>$$ab, ac, bc$$
>$$aa, bb, cc$$
>
>Infatti, l'ordine non viene contato. Dato che esiste già $ab$ non può esistere $ba$.

## Tipi di Combinazioni
Ci sono due tipi di combinazioni.

### Combinazioni Semplici
>Una **combinazione semplice** è una sequenza di *$k$ elementi distinti* (dove *non* sono ammesse ripetizioni) estratti tra $n$ elementi distinti, indipendentemente dall'ordine, con $n\geq k$.
>
>Il numero di combinazioni semplici si indica con $C_{n,k}$ ed è dato dal [[006 Coefficiente Binomiale|Coefficiente Binomiale]] $n$ su $k$.
>$$C_{n,k}={n\choose k}=\frac{(n)_k}{k!}=\color{#CC241D}\frac{n!}{k!(n-k)!}$$


> [!example]- <font color="orange">Esempio</font>
>Consideriamo l'insieme $A=\{a,b,c,d\}$ formato da $n=4$ elementi, e sia $k=2$.
>
>Abbiamo che il numero delle combinazioni semplici è 
>$${n\choose k}={4\choose 2}=\frac{4!}{2!(4-2)!}=\frac{24}{4}=6$$
>
>Per prima cosa scriviamo tutte le [[005 Disposizioni#Disposizioni Semplici|disposizioni semplici]] formati da 2 elementi. Quindi abbiamo:
>$$ab, ac, ad$$
>$$ba, bc, bd$$
>$$ca, cb, cd$$
>$$da, db, dc$$
>
>Tra queste, quelle che cambiano solo per l'ordine degli elementi sono:
>$$ab \text{ e } ba,\quad ac \text{ e } ca$$
>$$ad \text{ e } da,\quad bc \text{ e } cb$$
>$$bd \text{ e } db,\quad cd \text{ e } dc$$
>
>Prendiamo solo una per ogni scelta, e quindi abbiamo le seguenti combinazioni:
>$$ab,ac,ad,bc,bd,cd$$
>
>---
> Supponiamo di voler prendere gli elementi $\{1,2,3\}$ e creare delle sequenze di 2 elementi.
> 
> ![[Pasted image 20230926183904.png|700]]
> 
> L'albero si costruisce un ramo alla volta per vedere se la sequenza che si potrebbe andare a creare esiste già.

### Combinazioni con Ripetizione
>Una **combinazione con ripetizione** è una sequenza di $k$ elementi estratti tra $n$ elementi distinti, con la possibilità che ogni elemento possa ripetersi fino a $k$ volte nella stessa sequenza e indipendentemente dall'ordine, con $n\geq k$.
>
>Il numero di combinazioni con ripetizione si indica con $C'_{n,k}$ ed è dato dal [[006 Coefficiente Binomiale|Coefficiente Binomiale]] $n+k-1$ su $k$.
>$$C'_{n,k}=\textcolor{#CC241D}{{n+k-1\choose k}}=\frac{(n+k-1)_k}{k!}=\frac{(n+k-1)!}{k!(n-1)!}$$



> [!example]- <font color="orange">Esempio</font>
>Consideriamo l'insieme $A=\{a,b,c,d\}$ formato da $n=4$ elementi, e sia $k=2$.
>
>Abbiamo che il numero delle combinazioni semplici è 
>$${n\choose k}={4+2-1\choose 2}={5\choose 2}=\frac{5!}{2!(5-2)!}=\frac{120}{12}=10$$
>
>Per prima cosa scriviamo tutte le [[005 Disposizioni#Disposizioni con Ripetizione|disposizioni con ripetizione]] formate da 2 elementi. Quindi abbiamo:
>$$aa, ab, ac, ad$$
>$$ba, bb,  bc, bd$$
>$$ca, cb, cc, cd$$
>$$da, db, dc, dd$$
>
>Tra queste, quelle che cambiano solo per l'ordine degli elementi sono:
>$$ab \text{ e } ba,\quad ac \text{ e } ca$$
>$$ad \text{ e } da,\quad bc \text{ e } cb$$
>$$bd \text{ e } db,\quad cd \text{ e } dc$$
>
>Prendiamo solo una per ogni scelta, aggiungiamo quelle ripetute e otteniamo le seguenti combinazioni:
>$$ab,ac,ad,bc,bd,cd,aa,bb,cc,dd$$
> 
> ---
> 
> Supponiamo di voler prendere gli elementi $\{1,2,3\}$ e creare delle sequenze di 2 elementi.
> 
> ![[Pasted image 20230926183941.png|700]]


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
- [YouMath - Combinazioni](https://www.youmath.it/lezioni/probabilita/calcolo-combinatorio/5162-combinazione.html)
- [YouMath - C. Semplici](https://www.youmath.it/lezioni/probabilita/calcolo-combinatorio/1216-combinazione-semplice.html)
- [YouMath - C. Ripetizioni](https://www.youmath.it/lezioni/probabilita/calcolo-combinatorio/1217-combinazione-con-ripetizione.html)