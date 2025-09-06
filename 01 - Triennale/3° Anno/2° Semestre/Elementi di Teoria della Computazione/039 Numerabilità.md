---
aliases:
  - Insieme Numerabile
  - Insieme Contabile
  - Insieme Enumerabile
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Indecidibilità
cssclasses:
  - 
---

> [!summary] Insieme Numerabile
> Un insieme $X$ è **Numerabile** se ha la stessa [[038 Metodo della Diagonalizzazione|cardinalità]] di $\mathbb{N}$, ovvero se esiste una *funzione [[021 Applicazioni Biettive|biettiva]]* $$f:\mathbb{N}\to X$$

^4f7a3f

> [!quote]+ Teorema
> Se ogni insieme $S_n$ è numerabile, allora sarà numerabile anche anche la loro unione $$S=\bigcup_{n\in\mathbb{N}}S_n$$

^006c31


> [!summary] Insieme Contabile
> Un insieme $X$ è **Contabile** se è *finito o numerabile*.

> [!summary] Insieme Enumerabile
> Un insieme $X$ è **Enumerabile** se ha la stessa [[038 Metodo della Diagonalizzazione|cardinalità]] di $\mathbb{N}$ (è *numerabile*) ed è *calcolabile*.
> Infatti, un enumeratore è una macchina che stampa la lista (finita) di tutte le stringhe che accetta il suo linguaggio e si ferma quando questa lista termina.

^a70d8d

> [!example]- <font color="orange">Esempio - $\mathbb{N}_p$</font>
>Sia $\mathbb{N}$ l'insieme dei numeri interi positivi e sia $\mathbb{N}_p=\{2n\ |\ n\in\mathbb{N}\}$ l'insieme dei numeri interi positivi pari.
>
>Utilizzando la [[038 Metodo della Diagonalizzazione#Cardinalità come una Funzione|definizione di cardinalità]] si può notare che $\mathbb{N}$ e $\mathbb{N}_p$ hanno la stessa cardinalità, infatti esiste $$f:\mathbb{N}\to\{2n\ |\ n\in\mathbb{N}\}$$
>dove $f(n)=2n, \forall n\in\mathbb{N}$ è biettiva.
>
>![[Pasted image 20240511142737.png]]
>
>Quindi $\mathbb{N}_p$ è numerabile.


> [!example]- <font color="orange">Esempio - $\mathbb{Q}^+$</font>
>Si consideri l'insieme $\mathbb{Q}^+=\left\{ \frac{x}{y}\ |\ x,y\in\mathbb{N}, x>0,y>0 \right\}$ dei numeri razionali positivi.
>
>Utilizzando la definizione di cardinalità possiamo vedere che $\mathbb{N}$ e $\mathbb{Q}^+$ hanno la stessa cardinalità.
>Bisogna definire una funzione biettiva tra i due insiemi.
>
>Creiamo una [[058 Matrici|Matrice]] infinita contenente tutti i numeri razionali positivi:
>- La riga $i$-esima contiene tutti i numeri con il numeratore $i$ 
>- La colonna $j$-esima ha tutti i numeri con denominatore $j$
>
>Quindi il numero $\frac{i}{j}$ occupa la $i$-esima riga e la $j$-esima colonna.
>
>Per definire una funzione biettiva tra $\mathbb{N}$ e $\mathbb{Q}^+$ dobbiamo definire una corrispondenza biunivoca tra gli elementi della matrice e quelli di $\mathbb{N}$. 
>
>![[Pasted image 20240511143851.png|500]]
>
>Quindi $\mathbb{Q}$ è numerabile.

## Non Numerabilità

>Per alcuni insiemi infiniti non c'é alcuna biezione con $\mathbb{N}$. Questi insiemi sono semplicemente *troppo grandi*. Un tale insieme è detto **Non Numerabile**.

L'insieme dei [[003 Insieme dei numeri Reali|numeri reali]] è un esempio di insieme non numerabile. 

> [!quote]+ Dimostrazione
> Per dimostrare che $\mathbb{R}$ non è numerabile, mostriamo che non esiste una biezione tra $\mathbb{N}$ e $\mathbb{R}$. La dimostrazione è per assurdo
> Supponiamo che esista una biezione $f$ tra $\mathbb{N}$ e $\mathbb{R}$. Dobbiamo dimostrare che $f$ non funziona come dovrebbe. 
> 
> Per essere una biezione, $f$ deve associare tutti gli elementi di $\mathbb{N}$ con tutti gli elementi di $\mathbb{R}$. Mostriamo che esiste un numero $x\in\mathbb{R}$ che non è [[016 Immagine di un Elemento|immagine]] di alcun elemento in $\mathbb{N}$, il che rappresenterà la contraddizione.
> 
> Questo numero sarà compreso tra 0 e 1. Supponiamo che la biezione $f$ esista e che i suoi valori siano i seguenti.
> 
> ![[Pasted image 20240511152129.png]]
> 
> Scegliamo $x$ in modo che sia diverso dagli elementi della tabella procedendo "in diagonale". Dove per ogni $n$ scegliamo una qualsiasi cifra diversa dalla $n$-esima cifra dopo la virgola. Continuando in questo modo otteniamo tutte le cifre di $x$.
> 
> ![[Pasted image 20240511152609.png]]
> 
> Sappiamo che $x$ non è $f(n)$ per ogni $n$ perché differisce da $f(n)$ nell'$n$-esima cifra frazionaria.
> 
> #### Attraverso il [[038 Metodo della Diagonalizzazione#Metodo della diagonalizzazione per la Cardinalità|Metodo della Diagonalizzazione]]
> Supponiamo per assurdo che esista una funzione biettiva $f$ di $\mathbb{N}$ su $\mathbb{R}$. Allora possiamo creare la seguente tabella.
> 
> ![[Pasted image 20240511153058.png]]
> 
> Dove $r_i$ è la parte intera e $d_{i,1}, d_{i,2}, \dots$ è la parte decimale del numero reale $f(i)$, quindi $f(i)=r_i,d_{i,1},d_{i,2},\dots$
> 
> Le cifre sulla [[059 Matrici Quadrate|diagonale]] di questa matrice $d_{1,1},d_{2,2},\dots$ definiscono un numero reale $r=0,d_{1,1},d_{2,2},\dots$
> 
> Il numero reale $x=0,d_{1,1}',d_{2,2}',\dots$ che si ottiene scegliendo in ogni posizione della parte decimale una cifra diversa dalla corrispondente cifra in $r$ (e diversa da 0 e 9) non è immagine di nessun intero positivo. In generale $x\neq f(i)$ per un qualsiasi $i$, perché $d_{i,i}'\neq d_{i,i}$.

Possiamo però dedurre che $(\textcolor{#CC241D}{|\mathbb{N}|< |\mathbb{R}|})$, questo perché consideriamo le [[038 Metodo della Diagonalizzazione#^8e6455|seguenti condizioni]]: 
- Non esiste alcuna funzione biettiva di $\mathbb{N}$ su $\mathbb{R}$ $(\textcolor{#61AFEF}{|\mathbb{N}|\ne |\mathbb{R}|})$ 
- Esiste una funzione iniettiva $n\in\mathbb{N}\to n\in\mathbb{R}$ $(\textcolor{#61AFEF}{|\mathbb{N}|\le |\mathbb{R}|})$

___
[[000 Indice ETC|↖ Ritorna all'indice ↖]]

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