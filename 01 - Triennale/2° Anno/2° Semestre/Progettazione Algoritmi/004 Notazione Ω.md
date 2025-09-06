---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Notazioni Asintotiche
cssclasses:
  - 
---
Siano $f$ e $g$ due [[011 Funzioni|funzioni]] dai naturali ai reali positivi, ovvero $f:n\in\mathbb{N}\to f(n)\in\mathbb{R}_+$, $g:n\in\mathbb{N}\to g(n)\in\mathbb{R}_+$.

Diremo che **$\color{#CC241D}f(n)=\Omega(g(n))$**, ovvero che la funzione $f(n)$ è $\Omega(g(n))$, se e solo se esistono due costanti positive $c>0$ e $n_0\in \mathbb{N}$ tale che $$\color{#61AFEF}f(n)\geq c\cdot g(n),\quad\forall n\geq n_0$$
> [!tip] Definizione informale
> È vero che $f(n) = \Omega(g(n))$ se la funzione $f(n)$ cresce tanto velocemente almeno quanto cresce la funzione $g(n)$.
>
> La notazione $\Omega$ ne [[006 Insieme Illimitato e Limitato#Insieme Limitato Inferiormente|limita inferiormente]] la crescita e pertanto fornisce una indicazione della *velocità minima* dell'algoritmo. Si dice quindi che la notazione $\Omega$ è il *limite inferiore asintotico* della funzione $f(n)$.
>
>Graficamente, abbiamo che il grafico della funzione $f(n)$, dal punto $n_0$ in poi, si trova sempre *sopra* il grafico della funzione $c\cdot g(n)$, per qualche costante $c>0$.
>
>![[Pasted image 20230311093016.png|700]]

---
### COME CALCOLARE LA COMPLESSITÀ Ω
Osserviamo che valgono le seguenti relazioni:

> [!quote] Dimostrazione
>$f(n)=\Omega(g(n))\iff \exists c, n_0: f(n)\geq c\cdot g(n), \forall n\geq n_0$
>$\quad\quad\quad\quad\quad\quad\quad\iff \exists c, n_0: g(n)\leq \frac{1}{c}f(n), \forall n\geq n_0$
>$\quad\quad\quad\quad\quad\quad\quad\iff g(n)= O(f(n))$

Pertanto, per provare che $f(n)=\Omega(g(n))$ basterà provare che $\color{#CC241D}g(n) = O(f(n))$.
Sulla base, quindi, di quanto già provato per la [[003 Notazione O|Notazione Asintotica O]], possiamo dire che 

|         Relazione         |
| :-----------------------: |
| $\sqrt{n}=\Omega(log\ n)$ |
|   $n=\Omega(\sqrt{n})$    |
|   $n^{k+1}=\Omega(n^k)$   |
|     $2^n=\Omega(n^k)$     |
|     $n!=\Omega(2^n)$      |
|     $n^n=\Omega(2^n)$     |

> [!example]- <font color="orange">Esempi</font>
>Sia $f(n)=n^2-2n, g(n)=n^2$ e vogliamo provare che $f(n)=\Omega(g(n))$. Quindi dobbiamo trovare una costante $c>0$ ed un numero intero $n_0$ tale che $f(n)\geq c\cdot g(n)$ per ogni $n\geq n_0$. Osserviamo che
>
>$n^2-2n\geq c\cdot n^2\iff n^2-c\cdot n^2\geq2n\iff n^2(1-c)\geq 2n\iff n(1-c)\geq2\iff n\geq\frac{2}{1-c}$
>
>per cui basterà scegliere $c$ come un qualsiasi valore $0<c<1$. Ad esempio $c=\frac{1}{2}$ ed avremmo quindi che $n^2-2n\geq c\cdot n^2$ per ogni valore di $n\geq4$.
>
>---
>Vogliamo provare che $n^2-\sqrt{n}\ log\ n=\Omega(n^2)$. Dobbiamo provare che $\exists c, n_0$ tale che $n^2-\sqrt{n}log\ n\geq c\cdot n^2, \forall n\geq n_0$.
>Osserviamo che:
>
>$n^2-\sqrt{n}\ \textcolor{#FF6611}{log\ n}\geq n^2-\sqrt{n}\textcolor{#FF6611}{\sqrt{n}}=n^2-n\geq n^2-\frac{1}{2}n^2=\frac{1}{2}n^2, \forall n\geq 2$
>
><font color="#FF6611">Questo</font> perché $log\ n = O(\sqrt{n})$.

___
[[000 Indice PA|↖ Ritorna all'indice ↖]]

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