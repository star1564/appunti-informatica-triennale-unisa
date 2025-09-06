---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
paragrafo: Probabilità
cssclasses:
  - 
---
>La **Formula di Bayes** serve per calcolare la [[019 Probabilità Condizionata|probabilità condizionata]] di un [[011 Evento|Evento]] $A$ rispetto a un altro evento $B$, a patto di conoscere, oppure di saper calcolare, *alcune informazioni in più*: 
>- Le probabilità dei due eventi
>- La probabilità condizionata di $B$ rispetto a $A$
>$$\color{#CC241D}\mathbb{P}(A|B)=\frac{\mathbb{P}(B|A)\cdot \mathbb{P}(A)}{\mathbb{P}(B)}$$

^014a04

> [!quote] Dimostrazione
> Si parte dalla formula della probabilità condizionata $$\mathbb{P}(A|B)=\frac{\mathbb{P}(A\cap B)}{\mathbb{P}(B)}$$
> L'unica cosa da cambiare è il numeratore.
> 
> Si prende adesso l'opposto, ovvero $$\mathbb{P}(\textcolor{#61AFEF}{B|A})=\frac{\mathbb{P}(B\cap A)}{\mathbb{P}(A)}=\frac{\mathbb{P}(A\cap B)}{\mathbb{P}(A)}$$
> 
> Si trova l'intersezione tra $A$ e $B$, ovvero la [[020 Probabilità Composta|probabilità composta]] $$\begin{align}\mathbb{P}(B|A)=\frac{\mathbb{P}(A\cap B)}{\mathbb{P}(A)}&\iff\mathbb{P}(A)\cdot\mathbb{P}(B|A)=\frac{\mathbb{P}(A\cap B)}{\cancel{ \mathbb{P}(A) }}\cdot\cancel{ \mathbb{P}(A) }\\ &\iff \color{#00C575}\mathbb{P}(B|A)\cdot\mathbb{P}(A)=\mathbb{P}(A\cap B) \end{align}$$
> 
> Si va a sostituire il numeratore della formula originale $$\mathbb{P}(A|B)=\frac{\mathbb{P}(A\cap B)}{\mathbb{P}(B)}\iff \mathbb{P}(A|B)=\frac{\color{#00C575}\mathbb{P}(B|A)\cdot\mathbb{P}(A)}{\mathbb{P}(B)}$$
> 
> da cui la tesi.


Da questa possiamo ricavare anche la formula per $n$ alternative ($j=1,2,\dots,n$): 
$$\mathbb{P}(F_j|E)=\frac{\mathbb{P}(E\cap F_j)}{\mathbb{P}(E)}=\frac{\mathbb{P}(E| F_j)\cdot \mathbb{P}(F_j)}{\mathbb{P}(E)}=\color{#61AFEF}\frac{\mathbb{P}(E| F_j)\cdot \mathbb{P}(F_j)}{\sum_{i=1}^{n} \mathbb{P}(E|F_{i})\cdot \mathbb{P}(F_{i})}$$

> [!example]- <font color="orange">Esempio 1</font>
>>Date due monete
>>- una è ingiusta, con il 90% dei lanci che ottiene testa e il 10% croce,
>>- mentre l'altra è equa.
>>
>>Si sceglie a caso una moneta e la si lancia. 
>>
>>Qual è la probabilità che questa moneta sia quella ingiusta, se si ottiene un testa?
>
>Abbiamo tre eventi:
>- $U$ l'evento che abbiamo una moneta ingiusta (unfair);
>- $H$ l'evento che abbiamo ottenuto testa al lancio (head);
>- $F$ l'evento che abbiamo una moneta equa (fair).
>
>Sappiamo che:
>- $\mathbb{P}(H|U) = 90\%$
>- $\mathbb{P}(U) = 50\%$
>- $\mathbb{P}(F) = 50\%$
>
>Per ricavare la probabilità di ottenere una testa $P(H)$ utilizziamo la [[019 Probabilità Condizionata#Formula delle alternative|formula delle alternative]], poiché la probabilità che si ottenga una testa dipende da due eventi indipendenti:
>$$\mathbb{P}(H) = \mathbb{P}(H|U)\cdot \mathbb{P}(U) + \mathbb{P}(H|\overline{F})\cdot \mathbb{P}(\overline{F})$$
>Dove $\mathbb{P}(\overline{F})=\mathbb{P}(F)$.
>
>Quindi $\mathbb{P}(U|H)$ può essere calcolata come segue
>
>$$\mathbb{P}(U|H) = \frac{\mathbb{P}(H|U)\ \mathbb{P}(U)}{\mathbb{P}(H)}=\frac{\mathbb{P}(H|U)\ \mathbb{P}(U)}{\mathbb{P}(H|U)\ \mathbb{P}(U) + \mathbb{P}(H|F)\ \mathbb{P}(F)} = \frac{0,9\cdot0,5}{0,9\cdot0,5 + 0,5\cdot0,5} = 0,64 = 64\%$$

> [!example]- <font color="orange">Esempio 2</font>
>>Supponiamo che un medico abbia riportato il seguente test di screening del cancro su 10.000 persone.
>>
>>![[Pasted image 20231216163420.png]]
>>
>>Ciò indica che:
>>- 80 pazienti su 100 sono diagnosticati correttamente, mentre gli altri 20 non lo sono.
>>- Il cancro viene erroneamente rilevato in 900 persone sane su 9.900.
>>
>>Se il risultato di questo test di screening su una persona è positivo, qual è la probabilità che abbia effettivamente il cancro?
>
>Assegniamo:
>- l'evento di avere un cancro come $C$
>- l'evento di avere risultati positivi risultati positivi $Pos$
>
>Quindi abbiamo $$\mathbb{P}(C|Pos)=\frac{\mathbb{P}(Pos|C)\cdot \mathbb{P}(C)}{\mathbb{P}(Pos)}$$
>
>Dove:
>- $\mathbb{P}(Pos|C) = \frac{80}{100} = 0,8$ 
>- $\mathbb{P}(C) = \frac{100}{10000} = 0,01$
>- $\mathbb{P}(Pos) = \frac{980}{10000} = 0,098$
>
>$$\mathbb{P}(C|Pos)=\frac{0,8\cdot 0,01}{0,098} = 0,0816 = 8,16\%$$

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
- https://www.youtube.com/watch?v=cqTwHnNbc8g