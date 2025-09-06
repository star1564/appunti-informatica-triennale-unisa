---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Aritmetica
cssclasses:
  - 
---
>Siano $a, b, m\in \mathbb{Z}$ e sia $m>1$.
>Si chiama **Equazione Congruenziale Lineare** in $x$ modulo $m$:
>$$\color{#CC241D}a\cdot x\equiv b(mod\space m)$$

Studiarla significa: 
1. *Individuare se esistono soluzioni* $t\in \mathbb{Z}$ tali che $a\cdot t\equiv b(mod\space m)$;
2. Capire come le soluzioni sono *legate* tra loro.

Se $a$ ed $m$ sono [[047 Numeri Coprimi|coprimi]] allora esiste $s\in \mathbb{Z}$ tale che $as\equiv b(mod\space m)$;
inoltre $\{z\in \mathbb{Z}: az\equiv b(mod\space m)\} = [s]_m$.

<font color="green">Dimostrazione (pL 191, 5.7.1)</font>.

## CALCOLO DELL'EQUAZIONE - METODO 1
1. Trovare il [[045 Massimo Comun Divisore|MCD]]$(a, m)$. Se questo è *$1$* procedere al punto 3, altrimenti vai al punto 2;
2. Se il MCD è diverso da 1 si vede se *$MCD(a, m)|b$*. Se questo è vero allora si dividono $a,b,m$ per $MCD(a, m)$ e si passa al punto 3, altrimenti **non ci sono soluzioni**;
3. Si esegue il calcolo dell'equazione come da <font color="orange">esempio</font> riportato al di sotto.

<font color="orange">Esempio</font>: $12x\equiv 8(mod\space 35)$
$a = 12, b = 8, m = 35$

Si nota che $MCD(12, 35) = 1$, quindi ha soluzioni, che si ottengono in questo modo:

$1 = 12\textcolor{#61AFEF}u+35v$

Soluzione: **$s$** $=$ *$u$*$\cdot b =$ *$u$*$\cdot 8$
Dobbiamo trovare la *$u$* che risolve l'equazione:
$35=12\cdot 2+11 \quad \Longrightarrow \quad 11=m-2a$
$12=11\cdot 1+1 \quad \Longrightarrow \quad 1=a-(m-2a)=$*$3$*$a-m$

*$u = 3$*

Soluzione: **$s$** $=$ *$u$*$\cdot b =$ *$3$*$\cdot 8 =$ **$24$**

4. Si può *Verificare il risultato* eseguire $(a\cdot s)-|b|$ e vedere se è divisibile per $m$.

5. *SE LA SOLUZIONE SI RITROVA FUORI DAL MODULO* si risolve in questo modo: 
- Riportare il numero della classe "nell'intervallo della base" sommando o sottraendo il numero della "base".
- Se il numero della classe è negativo si somma il numero della "base".


$[-15]_{14} = [-15+14]_{14} = [-1]_{14} = [14-1]_{14} = [13]_{14}$

## CALCOLO DELL'EQUAZIONE - METODO 2
Questo metodo non è molto efficace se c'è una grande differenza tra $a$ ed $m$, per esempio $7x\equiv5(mod\ 256)$.

Data l'equazione congruenziale $259x\equiv16(mod\ 11)$.
1. Trovare il $MCD(a, m)$. Se questo è *$1$* procedere al punto 3, altrimenti vai al punto 2;

2. Se il MCD è diverso da 1 si vede se *$MCD(a, m)|b$*. Se questo è vero allora si dividono $a,b,m$ per $MCD(a, m)$ e si passa al punto 3, altrimenti **non ci sono soluzioni**;

3. Trovare un numero tale che $a\equiv x(mod\ m)$;
	$259\equiv \textcolor{#61AFEF}x(mod\ 11)\Longrightarrow 259=11\cdot 23+\textcolor{#61AFEF}6\Longrightarrow259\equiv \textcolor{#61AFEF}6(mod\ 11)$
4. Sostituire la *nuova* $\color{#61AFEF}b$ con $a$ nella equazione originale;
	$\textcolor{#61AFEF}6x\equiv16(mod\ 11)$
	
5. Trovare un numero tale che $(\textcolor{green}n\cdot a)\equiv 1(mod\ m)$
	$\textcolor{green}2\cdot 6=12\equiv1(mod\ 11)$

6. Moltiplicare gli $a, b$ originali per il <font color="green">numero</font>.
	$\textcolor{green}2\cdot6x\equiv\textcolor{green}2\cdot16(mod\ 11)$
	$12x\equiv32(mod\ 11)\Longrightarrow x\equiv\textcolor{#CC241D}{32}(mod\ 11)$

7. La nuova $\color{#CC241D}b$ sarà il risultato (scalare se necessario).
	$[\textcolor{#CC241D}{32}]_{11}=[32-11]_{11}=[21-11]_{11}=[\textcolor{#CC241D}{10}]_{11}$
	$s=\{\textcolor{#CC241D}{10}+11k:k\in \mathbb{Z}\}$
	
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