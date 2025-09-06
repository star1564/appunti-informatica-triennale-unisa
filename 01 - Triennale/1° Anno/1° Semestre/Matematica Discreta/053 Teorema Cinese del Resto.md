---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Aritmetica
cssclasses:
---
> Il **Teorema Cinese del Resto** fornisce un metodo per risolvere sistemi di [[052 Equazioni Congruenziali|equazioni congruenziali]] simultanee.

Siano $m_1, ..., m_k$ interi positivi a due a due coprimi ($k\geq 2$) e siano $b_1, ..., b_k$ interi. Allora il sistema
$$\begin{cases}
x\equiv b_1(mod\space m_1)\\
...\\
x\equiv b_k(mod\space m_k)\\
\end{cases}
$$
ha soluzione *$s$* e l'insieme delle soluzioni del sistema coincide con la [[027 Classi di Equivalenza|classe]] $[s]_{m_1...m_k}$.

<font color="green">Dimostrazione (193, 5.7.7)</font>.

## RISOLUZIONE DEL SISTEMA
1. Si *ordina il sistema* spostando le equazioni con i moduli più grandi come primi elementi;
2. Dato un sistema di equazioni congruenziali si vede prima se ci sono soluzioni, quindi se i moduli sono *coprimi tra di loro*. Se lo sono si passa al punto 3, altrimenti non ci sono soluzioni;
3. Si *moltiplicano i moduli* delle equazioni ($m_1\cdot ...\cdot m_k$). Il risultato sarà il *modulo della classe delle soluzioni* ($[s]_{m_1\cdot ...\cdot m_k}$);
4. Si risolve come da esempi.

> [!example]- <font color="orange">Esempio</font>
>#### SISTEMA CON DUE EQUAZIONI
>$$\begin{cases}
>x\equiv 4(mod\space 5) \\
>x\equiv 3(mod\space 4) \\
>\end{cases}
>$$
>$MCD(5,4)=1$, quindi $\exists s\in \mathbb{Z}: [s]_{5\cdot 4}=[s]_{\color{green}20}=S$
>
>Si prende la *prima equazione* e la si mette nella seconda.
>$[4]_5=\{\textcolor{#61AFEF}{4+5t}|t\in\mathbb{Z}\}$
>
>$4+5t\equiv 3(mod\space 4) \iff 5t\equiv -1(mod\space 4) \iff 5t\equiv 3$[^1]$(mod\space 4)$
>
>Si [[052 Equazioni Congruenziali#CALCOLO DELL'EQUAZIONE - METODO 2|Calcola l'Equazione Congruenziale]] (in questo caso utilizziamo il secondo metodo), ottenendo come risultato $\color{red}3$.
>
>La soluzione del sistema deriva dalla *prima equazione*:
>$\color{#61AFEF}4+5t$
>
>Si sostituisce $t$ con il **risultato** di prima, ottenendo la soluzione del sistema:
>$4+5\cdot\textcolor{red}3= \color{#61AFEF}19$
>
>L'insieme delle soluzioni è:
>$[\textcolor{#61AFEF}{19}]_{\color{green}20}=\{\textcolor{#61AFEF}{19}+\textcolor{green}{20}z:z\in \mathbb{Z}\}$
>
>---
>#### SISTEMA CON TRE EQUAZIONI
>$$\begin{cases}
>x\equiv 3(mod\space 5) \\
>x\equiv 3(mod\space 7) \\
>x\equiv 9(mod\space 11) \\
>\end{cases} \iff 
>\begin{cases}
>x\equiv 9(mod\space 11) \\
>x\equiv 3(mod\space 7) \\
>x\equiv 3(mod\space 5) \\
>\end{cases} 
>$$
>- $MCD(11,7)=1, MCD(11,5)=1, MCD(7,5)=1$, quindi $\exists s\in \mathbb{Z}: [s]_{11\cdot 7\cdot 5}=[s]_{\color{green}385}=S$
>
>- Si prende la *prima equazione* e la si mette nella seconda.
>$[9]_{11}=\{\textcolor{#61AFEF}{9+11t}|t\in\mathbb{Z}\}$
>$9+11t\equiv 3(mod\space 7) \iff 11t\equiv -6(mod\space 7) \iff 11t\equiv 1$[^2]$(mod\space 7)$
>
>- Si [[052 Equazioni Congruenziali#CALCOLO DELL'EQUAZIONE - METODO 2|Calcola l'Equazione Congruenziale]] (in questo caso utilizziamo il secondo metodo), ottenendo come risultato $\color{red}2$.
>
>- La soluzione delle prime due equazioni (derivante da $\color{#61AFEF}9+11t$) sarà:
>$9+11\cdot\textcolor{red}2 = \color{#61AFEF}31$
>
>- L'insieme delle prime due soluzioni è:
>$[31]_{\color{green}11\cdot 7}=[31]_{\color{green}77}=\{31+\textcolor{green}{77}z:z\in \mathbb{Z}\}$
>
>- Si sposta *questa equazione nella terza*.
>$31+77z\equiv 3(mod\space 5) \iff 77z\equiv -28(mod\space 5)$ 
>$\iff 77z\equiv 2(mod\space 5)$
>
>- Si [[052 Equazioni Congruenziali#CALCOLO DELL'EQUAZIONE - METODO 2|Calcola l'Equazione Congruenziale]] (in questo caso utilizziamo il secondo metodo), ottenendo come risultato $\color{red}1$.
>
>- La soluzione (derivante da $31+77t$) sarà:
>$31+77\cdot\textcolor{red}1= \color{#61AFEF}108$
>
>- L'insieme delle prime due soluzioni è:
>$[108]_{\color{green}77\cdot 5}=[108]_{\color{green}385}=\{108+\textcolor{green}{385}z:z\in \mathbb{Z}\}$
>
>
>[^1]: Per farlo rientrare nel modulo $4$ si esegue: $4+(-1) = 3$
>[^2]: Per farlo rientrare nel modulo $7$ si esegue: $7+(-6) = 1$

## RISOLUZIONE DEI RESTI
Dato il resto seguente:
$$\text{rest}(a, m) = b$$
lo si può convertire nella seguente equazione congruenziale:
$$a\equiv b(mod\space m)$$

Si possono anche unire più equazioni per creare un sistema.

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