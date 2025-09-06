---
aliases:
tags: 
  - corsi/informatica/elementi_teoria_computazione
paragrafo: NP-completezza
cssclasses:
  - 
---
Un progresso importante sulla questione $\color{#B35DB0}P = NP\ ?$ ci fu quando si scoprirono vari linguaggi appartenenti ad $NP$ la cui complessità individuale è **correlata** a quella dell'intera classe $NP$. In pratica, se esistesse un algoritmo di [[054 Tempo Polinomiale ed Esponenziale|Tempo Polinomiale]] per uno qualsiasi di essi, *tutti* i linguaggi in $NP$ diventerebbero decidibili in tempo polinomiale attraverso le [[062 Riducibilità in tempo polinomiale|riduzioni polinomiali]]. Questi linguaggi vengono detti **NP-Completi**.

Il primo linguaggio NP-Completo che fu scoperto è legato al [[064 SAT e 3SAT|problema della soddisfacibilità di un'espressione booleana]].

## Definizione
>Un linguaggio $B$ è **NP-completo** se soddisfa le seguenti due definizioni:
>1. $B\in NP$
>2. Ogni $A\in NP$  è [[062 Riducibilità in tempo polinomiale|riducibile in tempo polinomiale]] in $B$

> [!summary] Strategia per provare che un linguaggio $B$ è NP-completo
> 1. Mostrare che $B\in NP$
> 2. Scegliere un linguaggio $A$ che sia NP-completo
> 3. Definire una [[062 Riducibilità in tempo polinomiale|riduzione di tempo polinomiale]] da $A$ in $B$ (molto spesso $A=3SAT$)

^68fbfb

## Teoremi 

> [!quote]+ Teorema 1
> >Se $B$ è NP-completo e $B$ è in $P$, allora $\color{#B35DB0}P = NP$.
>
>Siccome $B$ è NP-completo, per ogni $A\in NP$, risulta che $A\leq_p B$. Ma si è [[062 Riducibilità in tempo polinomiale#^ad2b8c|provato]] che se Se $A\leq_p B$ e $B\in P$ , allora $A\in P$.
>Quindi $NP\subseteq P$ e siccome $P\subseteq NP$ risulta che $\color{#B35DB0}P=NP$.

> [!quote]+ Teorema 2
> >Se $B$ è NP-completo e $B\leq_p C$, con $C\in NP$, allora $C$ è NP-completo.
>
> Per ipotesi abbiamo che:
> - $C\in NP$
> - Per ogni $A\in NP$, $A\leq_p B$ (ovvero che $B$ è NP-completo)
> - $B\leq_p C$
> 
> Allora, utilizzando la [[062 Riducibilità in tempo polinomiale#^6ba29e|proprietà transitiva]] di $\leq_p$ abbiamo che:
> - $C\in NP$
> - Per ogni $A\in NP$, $A\leq_p C$
>
> Quindi $C$ è NP-completo.

### Teoremi per problemi
- [[066 Sat e Clique, NP-Completezza#Teorema Cook-Levin $(SAT)$|SAT]]
- [[066 Sat e Clique, NP-Completezza#Teorema $SAT_{CNF}$|SAT CNF]]
- [[066 Sat e Clique, NP-Completezza#Teorema $3SAT$|3SAT]]
- [[066 Sat e Clique, NP-Completezza#Teorema $CLIQUE$|CLIQUE]]
- [[067 Vertex Cover, NP-Completezza|VERTEX-COVER]]
- [[068 Subset-Sum, NP-Completezza|SUBSET-SUM]]
- [[069 Hampath, NP-Completezza|HAMPATH]]
- [[070 UHampath, NP-Completezza|UHAMPATH]]



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