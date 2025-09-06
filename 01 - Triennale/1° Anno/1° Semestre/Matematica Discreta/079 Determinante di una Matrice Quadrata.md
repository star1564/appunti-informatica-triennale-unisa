---
aliases: Determinante
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Matrici - Terminologie
cssclasses:
  - 
---
>Se $A$ è una [[059 Matrici Quadrate|Matrice Quadrata]] di ordine $n\geq 1$, si chiama **determinante** di $A$ il numero reale che viene definito per induzione su $n$ come segue:
>1. Viene precisato per $n = 1$ che: 
>$$det(a_{1,1})=a_{1,1}$$
>2. Supponiamo di avere definito il determinante di una matrice quadrata di ordine $t$. Supponiamo allora di definirlo anche per $t+1$:
>$$A=\begin{pmatrix}
a_{1,1}&a_{1,2}&...&a_{1,n}\\
a_{2,1}&a_{2,2}&...&a_{2,n}\\
\vdots&\vdots&\vdots&\vdots\\
a_{m,1}&a_{m,2}&...&a_{m,n}\\
\end{pmatrix}$$
*$$detA=a_{1,1}\cdot A_{1, 1} - a_{1,2}\cdot A_{1, 2} + ... - \space a_{1,n}\cdot A_{1, n}$$*

Dove: **i segni + e - si alternano**; $A_{m, n}$ è la [[064 Matrici Complementari|matrice complementare]].

Quindi il determinante è la somma dei prodotti dei termini della prima riga per i loro [[081 Complemento Algebrico|complementi algebrici]].

Il determinante si può calcolare anche con il [[082 Teorema di Laplace|Teorema di Laplace]].

#### DETERMINANTE DI UNA MATRICE QUADRATA DI ORDINE 2
Il determinante di una *matrice quadrata di ordine 2* si può facilmente calcolare in questo modo:
$$A_{2\times 2}=
\begin{pmatrix}
a_{1,1} & a_{1,2}\\
a_{2,1} & a_{2,2}\\
\end{pmatrix}$$
**$$detA = a_{1,1}\cdot a_{2,2} - a_{1,2}\cdot a_{2,1}$$**

#### REGOLA DI SARRUS (ORDINE 3)
Il determinante di una *matrice quadrata di ordine 3* si può facilmente ricavare attraverso la [[080 Regola di Sarrus|Regola di Sarrus]].


## PROPRIETÀ DEL DETERMINANTE
Sia $A$ una matrice quadrata, allora gode delle seguenti proprietà:
1. Se $A$ possiede una *linea nulla* (una riga o colonna con tutti gli elementi pari a 0), allora il $detA=0$;
2. Il determinante di $A$ è uguale a quello della sua *trasposta*: $detA=detA^t$;
3. Se $B$ si ottiene da $A$ *scambiando* due righe o due colonne, allora $detB=-detA$;
4. Se $B$ si ottiene da $A$ *moltiplicando* tutti i termini di una riga o coonna per un numero reale $\lambda\in\mathbb{R}$, allora $detB = \lambda detA$;
5. Se $A$ possiede due righe o due colonne *uguali*, allora $detA = 0$;
6. Se $A$ è una [[065 Matrici Triangolari|matrice triangolare]], allora $detA=a_{1,1}\cdot a_{2,2}\cdot ... \cdot a_{n,m}$;
7. Se B si ottiene da A *sommando* ad una riga (o ad una colonna) il prodotto di un'altra riga (o colonna) per un numero reale, allora $detA = detB$.

#### TEOREMA DI BINÈT
$det(AB) = det A\cdot detB$
$AB \neq BA$ però $det(AB) = det (BA)$
$det(AB) = det A\cdot detB = detB \cdot A = det(BA)$

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
- Pagina Libro: 273