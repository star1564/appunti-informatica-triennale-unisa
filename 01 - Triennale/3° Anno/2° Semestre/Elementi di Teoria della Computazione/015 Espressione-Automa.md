---
aliases: 
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Espressioni Regolari
cssclasses:
  - 
---

## Dall'Espressione all'Automa

![[014 Espressioni Regolari#^308157]]

Supponiamo di avere un'espressione regolare $R$ che descrive un linguaggio $A$. Mostriamo come trasformare $R$ in un [[009 Automi Finiti Non Deterministici|NFA]] che riconosce $A$.

### Passo Base

Per il passo base ci possono essere tre possibili rappresentazioni:

1. $\color{#61AFEF}R=a$ per qualche $a\in\Sigma$. Allora $\mathcal{L}(R)=\{a\}$ e il seguente NFA riconosce $\mathcal{L}(R)$.

    ![[Pasted image 20240323144545.png]]

2. $\color{#61AFEF}R=\epsilon$. Allora $\mathcal{L}(R)=\{\epsilon\}$ e il seguente NFA riconosce $\mathcal{L}(R)$.

    ![[Pasted image 20240323144702.png]]

3. $\color{#61AFEF}R=\emptyset$. Allora $\mathcal{L}(R)=\emptyset$ e il seguente NFA riconosce $\mathcal{L}(R)$.

    ![[Pasted image 20240323144748.png]]

### Passo Ricorsivo

Per il passo ricorsivo ci possono essere tre possibili rappresentazioni:

1. L'unione di due linguaggi [[013 Dimostrazioni Chiusure con NFA#Unione|con rappresentazione in NFA]].

    ![[Pasted image 20240323145109.png]]

2. La concatenazione di due linguaggi [[013 Dimostrazioni Chiusure con NFA#Concatenazione|con rappresentazione in NFA]].

    ![[Pasted image 20240323145158.png]]
    O più semplicemente, se si hanno _solo due caratteri_ (o più) _concatenati tra di loro_ (ad esempio $10$):

    ![[Pasted image 20240323150300.png|400]]

3. La star di un linguaggio [[013 Dimostrazioni Chiusure con NFA#Star|con rappresentazione in NFA]].

    ![[Pasted image 20240323145235.png]]

    O più semplicemente, se si ha lo _star di un solo carattere_ (ad esempio $0^*$):

    ![[Pasted image 20240323150504.png|160]]

4. La [[003 Chiusura di Kleene#Chiusura Positiva di Kleene|Chiusura Positiva]] di un linguaggio:

    ![[Immagine 2024-03-23 151222.png]]

> [!example]- <font color="orange">Esempio (PDF)</font>
> ![[Esempio RE a NFA.pdf]]


## Dall'Automa all'Espressione

Esistono almeno due algoritmi per trasformare un [[004 Automi Finiti Deterministici|DFA]] in un'[[014 Espressioni Regolari|espressione regolare]] equivalente:

-   Metodo di Kleene
-   Metodo di Brozozowski e McCluskey (BMC)

### Star

Si ha una operazione star **di tutta l'espressione** se _lo stato iniziale è anche accettante_.

> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20240323152641.png]] 
> $$E(ab\cup aba)^*, \mathcal{L}(E)=\{ab,aba\}$$

Si ha una star **per un certo stato e l'input usato per arrivarci** se si ha un _ciclo su quello stato_.

> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20240323153103.png|350]] $$0^*1(0^*\cup 10^*1)^*$$
>
> ---
>
> ![[Pasted image 20240323153239.png|350]]
>
> $$(bb^*a)^*bb^*(a\cup b)b^*$$

---

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
