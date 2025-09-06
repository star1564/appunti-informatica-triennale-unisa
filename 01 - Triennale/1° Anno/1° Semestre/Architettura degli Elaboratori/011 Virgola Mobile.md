---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: Rappresentazione Numeri - Con Virgola
cssclasses:
  - 
---

> [!note] Con virgola

## RAPPRESENTAZIONE

La rappresentazione in virgola mobile, anche detta Floating Point (FP), estende l'intervallo dei numeri rappresentabili a parità di cifre, ed utilizza la [[010 Notazione Scientifica#NOTAZIONE SCIENTIFICA NORMALIZZATA|Notazione Scientifica Normalizzata]].

I numeri binari in forma normalizzata sono rappresentati da una tripla <<font color="yellow">s</font>, <font color="green">E</font>, <font color="orange">M</font>>, dove:
- <font color="yellow">s = segno</font> (0 per i positivi +, 1 per i negativi -), questo, come nel [[007 Modulo e Segno|MS]], **non ha un peso**
- <font color="green">E = esponente</font> (la parte intera)
- <font color="orange">M = mantissa</font> (la parte frazionaria)


Il numero corrispondente è:
$$N=(-1)^{\textcolor{yellow}S}\times(1+\textcolor{orange}M)\times 2^{\color{green}E}$$

### STANDARD IEEE 754
Per poter stabilire il numero di bit da assegnare ad ogni parte per la rappresentazione, si è creato lo Standard IEEE 754, che specifica il *formato*, le operazioni aritmetiche e di confronto per i numeri FP.

Propone due formati:
- **Precisione Singola (32 bit: 1 parola macchina)**
<font color="yellow">1 bit per il segno s</font>
<font color="green">8 bit per rappresentare l'esponente E</font>
<font color="orange">23 bit per rappresentare la mantissa M</font>

  ![[65dd7411-ecd9-41ea-a320-583e4ea4a6ff.png|550]]


- **Precisione Doppia (64 bit: 2 parole macchina)**
<font color="yellow">1 bit per il segno s</font>
<font color="green">11 bit per rappresentare l'esponente E</font>
<font color="orange">52 bit per rappresentare la mantissa M</font>

### RAPPRESENTAZIONE IN VIRGOLA MOBILE CON 32 BIT
Per poter rappresentare l'<font color="green">esponente</font> con 8 bit, si usa la rappresentazione in [[002 Interi#Numero di Bit in una Sequenza a Binario Puro|Binario Puro]].
L'intervallo rappresentabile è [0, 2<sup>n</sup>-1] = [0, +255], di cui:
- 0 = 00000000 è riservato allo *zero*;
- 255 = 11111111 è riservato all'*infinito* e *NaN* (Not a Number).

L'intervallo restante ([1, 254]) è messo in corrispondenza con i 254 numeri appartenenti all'intervallo *[-126, 127]*.

Lo scopo di questa procedura è quello di rappresentare l'esponente più **negativo** con 00...00<sub>2</sub> e quello più *positivo* con 11...11<sub>2</sub>.

Questa notazione viene chiamata **Notazione Polarizzata** e la **Polarizzazione** sara 127 addizionato al numero esponente del 10 (10<sup><font color="green">E</font></sup>):
$\textcolor{green}E=e-127$, ovvero
$$e=127+\color{green}E$$

> [!example]- <font color="orange">Esempio</font>
00000000 = 0 RISERVATO (per lo zero)
00000001 = 1 rappresenta 1 - 127 = -126
00000010 = 2 rappresenta 2 - 127 = -125
00000011 = 3 rappresenta 3 - 127 = -124
...
11111101 = 253 rappresenta 253 - 127 = +126
11111110 = 254 rappresenta 254 - 127 = +127
11111111 = 255 RISERVATO (per l'infinito e NaN)

Quindi il valore di un numero in notazione polarizzata è:
$$N=(-1)^{\color{yellow}S}\times(1+\textcolor{orange}M)\times 2^{\textcolor{green}e-127}$$

## CONVERSIONE

### DECIMALE → BINARIO IN VIRGOLA MOBILE

1. Convertire la [[003 Base Qualunque e Decimale#DECIMALE → BASE QUALUNQUE|parte decimale]] e la [[009 Virgola Fissa#DECIMALE → BINARIO|parte frazionaria]] la parte frazionaria in **binario**, senza considerare il segno
2. Esprimere il numero in [[010 Notazione Scientifica#NOTAZIONE SCIENTIFICA NORMALIZZATA|Notazione Scientifica Normalizzata]], tenere conto dell'esponente E del 10 (10<sup>E</sup>)
3. Si specifica il <font color="yellow">segno</font> del numero (0 oppure 1)
4. Si prende la <font color="orange">parte frazionaria</font> del numero in notazione scientifica normalizzata e la si specifica come <font color="orange">mantissa</font>
5. Si calcola l'<font color="lime">esponente polarizzato</font> (<font color="lime">e</font> = 127 + E)
6. Si converte l'esponente polarizzato in binario e lo si specifica come <font color="green">esponente</font>
7. Si riunisce tutto in ordine (<<font color="yellow">s</font>, <font color="green">E</font>, <font color="orange">M</font>>)

> [!example]- <font color="orange">Esempio</font>
>-0,25<sub>10</sub> in FP su 32 bit.
>1. 0,25<sub>10</sub> = 0,01<sub>2</sub> $\times$ 10<sup>0</sup>;
>2. 0,01<sub>2</sub> $\times$ 10<sup>0</sup> = 00,1<sub>2</sub> $\times$ 10<sup>-1</sup> = **1,0<sub>2</sub> $\times$ 10<sup>*-2*</sup>**, parte frazionaria: <font color="orange">0</font>;
>3. Il segno è -, quindi sarà <font color="yellow">1</font>;
>4. Mantissa: <font color="orange">00..00<sub>2</sub></font>;
>5. Esponente polarizzato: <font color="lime">e</font> = 127 *- 2* = <font color="lime">125</font>;
>6. Esponente: 125 = <font color="green">01111101<sub>2</sub></font>;
>7. <font color="yellow">1</font> <font color="green">01111101</font> <font color="orange">0000000000000000000000</font>.

### BINARIO IN VIRGOLA MOBILE → DECIMALE

1. Verificare il <font color="yellow">segno</font> (0 = +, 1 = -)
2. Convertire l'<font color="green">esponente</font> in decimale
3. Convertire la <font color="orange">mantissa</font> in decimale
4. Il numero è rappresentato da: $N=(-1)^{\color{yellow}S}\times(1+\textcolor{orange}M)\times 2^{\textcolor{green}e-127}$

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

