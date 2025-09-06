---
aliases:
  - Linguaggio Riconosciuto da un DFA
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Automi Deterministici
cssclasses:
  - 
---
>Se $A$ è l'*insieme di tutte le stringhe che l'[[004 Automi Finiti Deterministici|automa]] $M$ accetta*, diciamo che $A$ è il **linguaggio della macchina $M$** e scriviamo che 
>$$\mathcal{L}(M)=A$$
>Si dice inoltre che $\color{#CC241D}M$ **riconosce** $\color{#CC241D}A$.

Una macchina *può accettare diverse stringhe*, ma riconosce sempre e soltanto *un solo [[001 Linguaggio|linguaggio]]*.

### Linguaggi Vuoti
Se la macchina non accetta alcuna stringa, essa riconosce ancora un linguaggio, ovvero il linguaggio vuoto $\emptyset$.

Quindi, se $\color{#61AFEF}|F|=0$, allora $\color{#61AFEF}\mathcal{L}(M)=\emptyset$.

![[Pasted image 20240310165805.png]]
### Linguaggi Alfabeto
Se una macchina accetta tutte le possibili stringhe, essa riconosce come linguaggio $\Sigma^*$.

Quindi, se $\color{#61AFEF}Q=F$, allora $\color{#61AFEF}\mathcal{L}(M)=\Sigma^*$.

![[Pasted image 20240310165941.png]]
### Linguaggi con solamente la stringa vuota
Se una macchina accetta la sola parola vuota, il diagramma è il seguente (dove l'alfabeto non è importante).

![[Pasted image 20240310170146.png]]

### Linguaggi finiti e infiniti
>L'*assenza di cicli* in un automa finito implica che $\mathcal{L}(M)$ è **finito**.

Ma la *presenza* di un ciclo *non* implica che sia infinito.

>Un linguaggio riconosciuto da un DFA è **infinito** se *esiste un ciclo su uno stato di accettazione*.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240310171855.png|500]]
>$$\mathcal{L}(M)=\{s\ |\ s\text{ inizia con } ab\}$$
>
><center>Questo linguaggio è infinito.</center>

### Linguaggi con un solo stato
Se la macchina ha solamente uno stato, il linguaggio riconosciuto dipende da come è fatto l'automa.

## Da un DFA ad un Linguaggio
>Dato un automa $M$, individuare il linguaggio $\mathcal{L}(M)$ *riconosciuto* dall'automa.

> [!summary] Tecnica per passare da un DFA a un Linguaggio
>1. *Analizzare i cammini di accettazione* attraverso l'utilizzo di stringhe diverse, che possono essere accettate e non accettate
>2. Creare un *elenco* di parole accettate e non accettate
>3. Cercare di descrivere la *proprietà in comune* tra le stringhe analizzando i due elenchi per descrivere $L$
>4. *Testare* che $\mathcal{L}(M)=L$ (ignorare il disegno dell'automa):
>	- Usando prima i due elenchi per trovare falsi positivi o negativi
>	- Controllare che tutte e sole le parole sono accettate

## Definizione Formale del Linguaggio Riconosciuto da un DFA
>Sia $A=(Q,\Sigma,\delta,q_0,F)$ un automa finito deterministico e sia $\hat\delta$ la [[006 Funzione di Transizione Estesa DFA|Funzione di Transizione ESTESA]].
>Il linguaggio **riconosciuto** da $A$ è 
>$$\mathcal{L}(A)=\{w\in\Sigma^*\ |\ \hat\delta(q_0,w)\in F\}$$

In altre parole, il linguaggio $A$ è l'insieme delle stringhe $w$ che *partano dallo stato iniziale* $q_0$ e *arrivano ad uno degli stati accettanti* all'interno di $F$.

# 

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