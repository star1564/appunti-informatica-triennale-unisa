---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Matrici - Terminologie
cssclasses:
  - 
---
## MATRICI SIMILI
Sia $F$ un [[054 Strutture Algebriche|Campo]], dove le matrici $A, B\in M_n(F)$ sono sullo stesso campo e dello stesso ordine.

>$A$ e $B$ sono **Simili** se e solo se:
>$\color{#61AFEF}\exists P\in M_n(F): det\ P\neq 0$ *e* $\color{#61AFEF}B=P^{-1}\cdot A\cdot P$.
>Dove $\cdot$ è il [[072 Prodotto Righe per Colonne di una Matrice|Prodotto di righe per colonne]] e $P^{-1}$ è la matrice inversa. 

Se $A$ è simile a $B$, allora $det\ A=det\ B$.

<font color="green">Dimostrazione</font>:
$det\ B=det(P^{-1}\cdot A\cdot P)=det\ (P^{-1})\ det\ A\ det\ P$
$=det(P^{-1}P)det\ A=det\ (In)\ det\ A=1\cdot det\ A= det\ A$.


## DIAGONALIZZAZIONE
Data una matrice $A^{n\times n}$, per capire se è diagonalizzabile, dobbiamo trovare gli *Autovalori*.
$$P\lambda(A)=det(A-\lambda I)=0\quad \Longrightarrow\quad \color{#61AFEF}\lambda_i\ autovalori$$
Dove $\lambda I$ è una *matrice identità* di $A$ ovvero, per esempio: ^db861a
$$\lambda\begin{pmatrix} 1&0&0\\ 0&1&0\\ 0&0&1\\ \end{pmatrix}=
\begin{pmatrix} \color{#61AFEF}\lambda_1&0&0\\ 0&\color{#61AFEF}\lambda_2&0\\ 0&0&\color{#61AFEF}\lambda_3\\ \end{pmatrix}$$

- Quindi si esegue $A-\lambda I$, ottenendo per esempio:
$$\begin{pmatrix} 2 & 1\\ 0 & 3\\ \end{pmatrix}-\begin{pmatrix} \lambda & 0\\ 0 & \lambda\\ \end{pmatrix}=\color{#61AFEF}\begin{pmatrix} 2-\lambda & 1\\ 0 & 3-\lambda\\ \end{pmatrix}$$

- Ora si calcola il suo [[079 Determinante di una Matrice Quadrata|Determinante]] ottenendo il *polinomio caratteristico*, per esempio:
$$det\begin{pmatrix} 2-\lambda & 1\\ 0 & 3-\lambda\\ \end{pmatrix}=\color{#61AFEF}(2-\lambda)\cdot(3-\lambda)$$

Adesso dobbiamo calcolare la Molteplicità Algebrica e Geometrica del polinomio caratteristico. È importante notare che:
$$\color{#CC241D}1\leq m_g\leq m_a$$

- *Molteplicità Algebrica ($\color{#61AFEF}m_a$)*: quante volte un autovalore è soluzione del polinomio caratteristico, esempio:
$(2-\lambda_1)\cdot(3-\lambda_2)$
$m_a(\lambda_1)=m_a(2)=1$
$m_a(\lambda_2)=m_a(3)=1$

- *Molteplicità Geometrica ($\color{#61AFEF}m_g$)*: la dimensione dell'autospazio associato ad un certo autovalore, si usa la regola descritta prima, se la $m_a=1$ allora $m_g=1$, ma se è diversa da $1$ si devono sostituire i parametri nella [[#^db861a|equazione di partenza]] esempio:
Matrice $3\times 3$
$\lambda_1=5$
$m_a(\lambda_1)=2$
$1\leq m_g(\lambda_2)\leq 2=m_g(\lambda 1)=n-\rho(A-\lambda_1I)$
$m_g(5)=3-\rho(A-5I)$
$=\textcolor{#61AFEF}{3-(1\lor 2)}=\begin{cases} 3-1=2\Rightarrow A\ diagonalizzabile \\ 3-2=1\Rightarrow\ A\ NON\ diagonalizzabile \end{cases}$

Una Matrice $A^{n\times n}$ è **diagonalizzabile** se e solo se:
1) $\color{#61AFEF}m_a(\lambda_1)+...+m_a(\lambda_n)=n$
2) $\textcolor{#61AFEF}{m_a(\lambda_i)=m_g(\lambda i)},\quad \forall i=1,...,n$

Se $A$ è diagonalizzabile, la matrice $P$ si dice che diagonalizza $A$.

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
- Pagina Libro: 