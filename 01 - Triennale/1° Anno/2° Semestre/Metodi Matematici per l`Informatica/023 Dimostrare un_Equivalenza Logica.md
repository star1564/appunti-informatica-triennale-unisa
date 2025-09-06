---
aliases: 
tags:
  - corsi/matematica/metodi_matematici_informatica
paragrafo: Logica Predicativa
cssclasses:
  - 
---
## Dimostrare l'Equivalenza Logica di due Proposizioni Composte
Per dimostrare l'[[007 Equivalenza Logica|equivalenza logica]] tra due [[001 Logica Proposizionale#^proposizione|proposizioni]] ci sono due metodi:
- Utilizzare le [[Tabella di Verità|tavole di verità]] delle due proposizioni composte e far vedere che esse sono uguali.
- Utilizzare le [[Alcune Proprietà di MMI|proprietà]] per riscrivere la prima proposizione in modo da farla diventare la seconda proposizione.
- Dedurre i valori delle preposizioni attraverso le tabelle di verità dei connettivi logici, [[012 Tautologia|tautologie]] e [[013 Contraddizione|contraddizioni]].

### Utilizzo delle Tabelle di Verità
Le due espressioni **sono logicamente equivalenti**, poiché hanno le *stesse tavole di verità*. Bisogna dimostrare quindi che $P1\equiv P2$.

> [!example]+ <font color="orange">Esempio</font>
Provare che $p\oplus q\equiv (p\land\neg q)\lor(\neg p\land q)$ sono logicamente equivalenti.
>
>| $p$ | $q$ | $\color{#CC241D}p\oplus q$ | $\color{#CC241D}(p\land\neg q)\lor(\neg p\land q)$ | $\neg p$ | $\neg q$ | $p\land\neg q$ | $\neg p\land q$ |
>| --- | --- | -------------------------- | -------------------------------------------------- | -------- | -------- | -------------- | --------------- |
>| T   | T   | **F**                      | **F**                                              | F        | F        | F              | F               |
>| T   | F   | **T**                      | **T**                                              | F        | T        | T              | F               |
>| F   | T   | **T**                      | **T**                                              | T        | F        | F              | T               |
>| F   | F   | **F**                      | **F**                                              | T        | T        | F              | F               |

### Utilizzo delle Proprietà
Si possono anche usare delle **proprietà** per dimostrare l'equivalenza logica.

> [!example]+ <font color="orange">Esempio</font>
>Provare che $\neg(p\Rightarrow q)$ e $p\land\neg q$ sono logicamente equivalenti.
>
>Proveremo che $P1\equiv P2$ sviluppando una serie di equivalenze logiche, iniziando da $P1$ fino ad arrivare a $P2$, spiegando quali proprietà si usano.
>
>$\neg(p\Rightarrow q)\equiv\neg(\neg p\lor q)\quad\quad$   Equivalenza condizionale disgiuntiva
>$\equiv\neg(\neg p)\land\neg q\quad\quad\quad\quad\quad\quad$ Legge di De Morgan
>$\equiv p\land \neg q\quad\quad\quad\quad\quad\quad\quad\quad$  Doppia negazione
>
>Si è dimostrato che $P1\equiv P2$.

### Non Logicamente Equivalenti (Falsa)
Le due espressioni $P1$ e $P2$ **non sono logicamente equivalenti**, poiché hanno *diverse tavole di verità*. Bisogna dimostrare quindi che $P1 \not\equiv P2$.

> [!example]+ <font color="orange">Esempio</font>
>Ad esempio se il valore di $p$ è `Vero`/`Falso` ed il valore di $q$ è `Vero`/`Falso`, allora il valore di $P1$ è `Vero`/`Falso` mentre il valore di $P2$ è `Vero`/`Falso`. Questi sono valori diversi, pertanto non hanno le stesse tavole di verità.

## Dimostrare l'Equivalenza Logica di due Espressioni
Per dimostrare che due espressioni sono logicamente equivalenti, dobbiamo provare che *danno sempre gli stessi valori*, senza considerare cosa sono i predicati $P$ e $Q$, e senza considerare i domini usati.

Si seguono due passaggi:
1. Si dimostra che se $P1$ è vera, allora anche $P2$ è vera
2. Si dimostra che che se $P2$ è vera, allora anche $P1$ è vera

> [!example]+ <font color="orange">Esempio</font>
>Provare che $\forall x(P(x)\land Q(x))$ e $\forall x P(x)\land \forall x Q(x)$.
>
>1. Supponiamo che $\forall x(P(x)\land Q(x))$ sia vera. Questo significa che se $a$ si trova nel dominio, allora $P(a)\land Q(a)$ è vera. Pertanto, $P(a)$ è vera e $Q(a)$ è vera.
>Dato che $P(a)$ e $Q(a)$ sono vere per ogni elemento $a$ nel dominio, possiamo concludere che $\forall x\ P(x)$  e $\forall x\ Q(x)$ sono entrambe vere. Questo significa che $\forall xP(x)\land \forall xQ(x)$ è vera.
>
>2. Supponiamo che $\forall xP(x)\land \forall xQ(x)$ è vera. Segue che $\forall xP(x)$ è vera e $\forall xQ(x)$ è vera. Pertanto, se $a$ è nel dominio, allora $P(a)$ e $Q(a)$ sono vere. Segue che per tutte le le $a$, $P(a)\land Q(a)$ è vera. Quindi $\forall x(P(x)\land Q(x))$ è vera.
>
>Possiamo concludere che $\forall x(P(x)\land Q(x))\equiv\forall x P(x)\land \forall x Q(x)$.


___
[[000 Indice MMI|↖ Ritorna all'indice ↖]]

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
