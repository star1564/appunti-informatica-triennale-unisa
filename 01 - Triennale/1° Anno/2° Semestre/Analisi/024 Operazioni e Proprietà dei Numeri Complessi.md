---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Numeri Complessi
cssclasses:
  - 
---
## Proprietà
### Esponente del Numero Immaginario
A partire dalla definizione di [[022 Numeri Complessi#Unità Immaginaria|Unità Immaginaria]] $i=\sqrt{-1}$ si hanno le seguenti identità con l'esponente di una [[MB - 13 Potenze|potenza]]:
- $\color{#61AFEF}i^2=-1$ ^e2df33
	- $\sqrt{-1}\sqrt{-1}=-1$
- $\color{#61AFEF}i^3=-\sqrt{-1}=-i$
	- $\sqrt{-1}\sqrt{-1}\sqrt{-1}=-1\sqrt{-1}=-\sqrt{-1}$
- $\color{#61AFEF}i^4=+1$
	- $\sqrt{-1}\sqrt{-1}\sqrt{-1}\sqrt{-1}=(-1)\cdot (-1)=+1$

Più in generale:
$$i^n=\begin{cases} i & \text{se rest}(n,4)=1  \\ -1 & \text{se rest}(n,4)=2 \\ -i & \text{se rest}(n,4)=3 \\ +1 & \text{se rest}(n,4)=0 \end{cases}\quad \forall n\in\mathbb{N}$$


### Complesso Coniugato
Dato un numero complesso $z=a+ib$ si definisce *Complesso Coniugato* di $z$ e si indica con $\overline{z}$ il numero complesso $$\color{#CC241D}\overline{z}=a-ib$$
Quindi la parte reale è positiva, mentre quella immaginaria è negativa.

> [!warning] Questo è vero solo se $a=a$ e $b=b$, ovvero se i due numeri hanno la stessa parte reale e immaginaria


Inoltre: $$\color{#61AFEF}z\cdot \bar z=a^2+b^2$$
> [!tip] Questo lo si dimostra con [[#Prodotto|l'operazione di prodotto]]


### Opposto
L'*Opposto* del numero complesso $(a,b)$ è $$\color{#CC241D}(-a,-b)$$

### Reciproco
Il *Reciproco* (o inverso moltiplicativo) di $z=(a,b)$ con $a\neq0$ e $b\neq0$ è 	$$\color{#CC241D}z^{-1}=\left(\frac{a}{a^2+b^2}, \frac{-b}{a^2+b^2}\right)$$
### Valore Assoluto
Il *[[MB - 16 Valore Assoluto|Valore Assoluto]]* di un numero complesso $z=(a,b)$ con $a\neq0$ e $b\neq0$ è 	$$\color{#CC241D}|z|=\sqrt{a^2+b^2}$$
Questo è uguale al [[025 Forma Trigonometrica di un Numero Complesso#^d6ccd5|rho della forma trigonometrica]].

## Operazioni
Si considerano due numeri reali in [[023 Forma Algebrica di un Numero Complesso|forma algebrica]]:
- $z=a+ib$
- $w=c+id$
### Somma
>La **Somma tra due numeri complessi** $z$ e $w$ è il numero complesso che ha:
>- Come parte reale, la *somma delle parti reali*
>- Come parte immaginaria, la *somma delle parti immaginarie*
>
>Ovvero:
>$$\textcolor{#CC241D}{z+ w}=(a+ib)+(c+id)=\color{#CC241D}(a+ c)+ i(b+ d)$$

### Differenza
>La **Differenza tra due numeri complessi** $z$ e $w$ è il *caso analogo alla somma $z+(-w)$*, infatti è il numero complesso che ha:
>- Come parte reale, la *differenza tra le parti reali*
>- Come parte immaginaria, la *differenza tra le parti immaginarie*
>
>Ovvero:
>$$\textcolor{#CC241D}{z- w}=(a+ib)-(c+id)=(a+ib)+(-c-id)=\color{#CC241D}(a- c)+ i(b- d)$$

### Prodotto
>Il **Prodotto tra due numeri complessi** è definita come:
>$$\textcolor{#CC241D}{z\cdot w}=(a+ib)\cdot(c+id)=\color{#CC241D}(ac-bd)+i(ad+bc)$$

Dove si è usata [[#^e2df33|questa proprietà]].

> [!tip]+ Metodo per ricordare meglio
>![[Pasted image 20250211194124.png|500]]




### Elementi Neutri della Somma e del Prodotto
L'elemento neutro rispetto alla *somma* è $$\color{#CC241D}(0,0)$$

L'Elemento Neutro rispetto al *prodotto* è $$\color{#CC241D}(1,0)$$

### Divisione
>La **Divisione tra due numeri complessi** è definita come:
>$$\textcolor{#CC241D}{\frac{z}{w}}=\frac{z}{w}\cdot\frac{\overline{w}}{\overline{w}}=\color{#CC241D}\frac{ac+bd}{c^2+d^2}+i \frac{bc-ad}{c^2+d^2}$$

Dove si è usato il [[024 Operazioni e Proprietà dei Numeri Complessi#COMPLESSO CONIUGATO|complesso coniugato]].

### Radice
Se dobbiamo calcolare la radice di un numero complesso in [[023 Forma Algebrica di un Numero Complesso|forma algebrica]] si procede in questo modo, dove si vuole trovare 
$$\color{#CC241D}\sqrt{-8i}$$
1. Trovare il numero complesso $a+ib$ che è uguale a $\sqrt{-8i}$, dati i numeri reali $a$ e $b$: $$a+ib=\sqrt{-8i}$$
2. Eleva al quadrato entrambi i membri tenendo conto che $i^2=-1$: $$a^2+2abi-b^2=-8i$$
3. Due numeri complessi sono **uguali** se le loro *parti reali sono uguali* e le loro *parti immaginarie sono uguali*, quindi si mettono a [[MB - 06 Sistemi di Equazioni Lineari|sistema]] i numeri simili (parti reali con parti reali e parti immaginarie con parti immaginarie): $$\begin{cases} a^2-b^2=0 & \text{reali} \\ 2ab=-8 & \text{immaginari} \end{cases}$$
4. Risolvi l'equazione in $b$: $$\begin{cases} a^2-b^2=0 \\ b=-\frac{4}{a} \end{cases}$$
5. Sostituisci il valore di $b$ nell'equazione $a^2-b^2=0$: $$a^2-\Big(-\frac{4}{a}\Big)^2=0$$
6. Risolvi l'equazione in $a$: $$a=\pm2$$
7. Sostituisci i valori di $a$ nell'equazione $2ab=-8$: $$2(-2)b=-8$$ $$2(2)b=-8$$
8. Risolvi l'equazione in $b$: $$b=\pm2$$
9. Le soluzioni sono le coppie $(-2,2)$ e $(2,-2)$. Si sceglie una delle due, come $(-2,2)$.
10. Otteniamo la radice del numero complesso iniziale: $$z=-2+
2i$$



## Equazioni
Si possono anche eseguire delle equazioni di [[MB - 04 Equazioni di Primo Grado|primo grado]] e di [[MB - 08 Equazioni di Secondo Grado|secondo grado]].

Questo significa che si può anche usare la [[MB - 08 Equazioni di Secondo Grado#Formula quadratica|formula quadratica]] per quelle di secondo grado.

> [!example]+ <font color="orange">Esempio</font>
> - $z^2+z+2i=0$ 
> Quindi $a=1, b=1, c=2i$. Si sostituiscono alla formula.
>
> - $2z^2+4i=0$
> Quindi $a=2, b=0, c=4i$. Si sostituiscono alla formula.
>
> - $z^2+2iz-3=0$
> Quindi $a=1, b=2i, c=-3$. Si sostituiscono alla formula.


### Razionalizzazione Delle Frazioni
Quando si ha *al denominatore di una frazione un numero complesso* lo si cerca di **eliminare**, moltiplicando il numeratore ed il denominatore per il complesso coniugato del numero in questione.

> [!example]+ <font color="orange">Esempio</font>
> $$\frac{i}{1+2i}$$
> 
> Si risolve in questo modo:
>$$
\begin{align*}
{\frac{i}{1+2i}}\cdot{\frac{1-2i}{1-2i}} &= {\frac{i(1-2i)}{(1+2i)(1-2i)}} \\
&= {\frac{i-2i^2}{1-4i^2}} \\
&= [\text{utilizzando } i^2=-1] \\
&= {\frac{i-2(-1)}{1-4(-1)}} \\
&= {\frac{i+2}{5}} \\
&= \frac{i}{5}+\frac{2}{5}
\end{align*}
>$$






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
- [YouMath - Operazioni](https://www.youmath.it/lezioni/analisi-matematica/numeri-complessi/773-operazioni-con-i-numeri-complessi.html)