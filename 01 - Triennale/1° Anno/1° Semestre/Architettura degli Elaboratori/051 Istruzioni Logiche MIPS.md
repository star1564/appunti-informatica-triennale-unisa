---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: MIPS - Assembly
cssclasses:
  - 
---
## OPERAZIONI DI SHIFT

### SHIFT LOGICO A SINISTRA

[[048 Formati delle istruzioni MIPS#FORMATO R|Formato R]].
La `sll` consente di inserire un certo numero $n$ di zeri a partire dalla posizione meno significativa di una word, *traslandola a sinistra* di $n$ posti. 
In questa operazione gli $n$ [[Bit Più Significativo|bit più significativi]] **vengono persi**.

Un effetto collaterale di questa istruzione è che il risultato corrisponde a **moltiplicare l'operando per $2^n$**.

Questo numero deve rientrare nell'[[008 Complemento a 2#^5f9c5a|intervallo di rappresentazione del C2]], altrimenti l'effetto collaterale non si verifica.

<font color="orange">Esempio</font>:
$0000\space0000\space0000\space0000\space0000\space0000\space0000\space1001 = 9_{10}$

**`sll $t2, $s0, 4`**

<font color="red">~~0000~~</font>$\textcolor{#61AFEF}{\ 0000\ 0000\ 0000\ 0000\ 0000\ 0000\ 1001 \leftarrow 0000}$
Che corrisponde a $\textcolor{#61AFEF}{144_{10}}=9_{10}\times 2^4=9_{10}\times 16_{10}$

### SHIFT LOGICO A DESTRA

[[048 Formati delle istruzioni MIPS#FORMATO R|Formato R]].
La `srl` consente di inserire un certo numero $n$ di zeri a partire dalla posizione più significativa di una word, *traslandola a destra* di $n$ posti. 
In questa operazione gli $n$ [[Bit Meno Significativo|bit meno significativi]] **vengono persi**.

Un effetto collaterale di questa istruzione è che il risultato corrisponde a **dividere l'operando per $2^n$**.

L'effetto collaterale non si verifica se un numero è negativo. Per risolvere questo problema si usa lo [[047 Assembly nel MIPS#SHIFT ARITMETICO A DESTRA|Shift Aritmetico a Destra]].

<font color="orange">Esempio</font>:
$0000\space0000\space0000\space0000\space0000\space0000\space0000\space1001 = 9_{10}$

**`srl $t2, $s0, 1`**

$\textcolor{#61AFEF}{1\rightarrow0000\ 0000\ 0000\ 0000\ 0000\ 0000\ 0000\ 100}$ <font color="red">~~1~~</font>
Che corrisponde a $\textcolor{#61AFEF}{4_{10}}=9_{10}/ 2$ (divisione intera)

### SHIFT LOGICO A SINISTRA

[[048 Formati delle istruzioni MIPS#FORMATO R|Formato R]].
La `sll` ha come effetto collaterale quello di **moltiplicare l'operando per $2^n$**.

### SHIFT ARITMETICO A DESTRA

[[048 Formati delle istruzioni MIPS#FORMATO R|Formato R]].
La `sra` consente di inserire un certo numero $n$ di bit, **tutti uguali al bit di segno** a partire dalla posizione più significativa di una word, *traslandola a destra* di $n$ posti.
Lo shift aritmetico a sinistra non ha senso.

## OPERAZIONI BIT A BIT
### OPERAZIONI COMUNI

Il MIPS offre operazioni logiche che operano su ciascun bit:
- [[026 AND (AdE)|AND]];
- [[027 OR (AdE)|OR]];
- [[031 NOR|NOR]] (anche per il [[028 NOT (AdE)|NOT]], `nor $t0, $s1, $zero` $\equiv \overline{\$s1}$);
- [[029 XOR (AdE)|XOR]].

Il formato è sempre [[048 Formati delle istruzioni MIPS#FORMATO R|R]].

### OPERAZIONI BIT A BIT IMMEDIATE

[[048 Formati delle istruzioni MIPS#FORMATO I|Formato I]].
Le *costanti* sono molto utili nelle operazioni logiche.
Abbiamo:
- AND immediato (`andi`);
- OR immediato (`ori`);

Infatti, si può ottenere un registro di destinazione completamente azzerato oppure con tutti 1.

___
[[000 Indice AdE|↖ Ritorna all'indice ↖]]

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