---
aliases:
  - Albero decisionale
  - Albero di decisione
  - Alberi di decisione
  - Alberi decisionale
  - Decision Tree
tags:
  - corsi/informatica/machine_learning
paragrafo: Alberi di Decisione
cssclasses:
  - 
---
>Un **Albero Decisionale** è un [[012 Grafi|Grafo]] ad [[013 Alberi|Albero]], cioè un diagramma sequenziale che *illustra tutte le possibili alternative decisionali* ed i risultati corrispondenti.

I decision tree vengono spesso usati per problemi di **[[007 Task di Classificazione|Classificazione]]** e **[[008 Task di Regressione|Regressione]]**.

![[Pasted image 20231216134340.png|600]]

Partendo dalla [[013 Alberi#^0c339a|radice]] dell'albero, ogni [[013 Alberi#^3090b6|nodo interno]] rappresenta la base su cui viene presa una decisione. Ogni ramo di un nodo rappresenta il *modo in cui una scelta può portare ai nodi successivi*. Infine, ogni [[013 Alberi#^d91f38|foglia]] rappresenta il risultato prodotto.

In *ogni nodo* viene posta una *domanda* relativa ai valori e alle caratteristiche di una caratteristica. A seconda della risposta alla domanda, le osservazioni vengono suddivise in sottoinsiemi.

Vengono condotti test sequenziali finché non si giunge a una conclusione sull'etichetta di destinazione delle osservazioni. I percorsi dalla radice alle foglie rappresentano il **processo decisionale** e le regole di classificazione.

> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20231216135744.png]]

## Costruzione di un Albero di Decisione
Un albero decisionale viene creato *dividendo i campioni di addestramento in sottoinsiemi*. Questo processo si ripete in modo ricorsivo per ogni sottoinsieme. 

In ogni nodo, viene eseguito un test basato sul valore di una caratteristica del sottoinsieme. Il processo continua fino a quando il sottoinsieme ha la *stessa etichetta* di classe o quando ulteriori divisioni *non migliorano* la purezza della classe. A questo punto, il partizionamento ricorsivo si interrompe.

Esistono due tipi di divisioni (**Split**):
- *Splitting Binario*, avviene quando ci sono solo due soluzioni alla domanda nel nodo di decisione, in pratica si ha un [[023 Alberi Binari|Albero Binario]];
- *Splitting Multiway*, avviene quando ci sono più di due soluzioni per nodo.

![[Pasted image 20231216140239.png|1200]]

### Algoritmi per Alberi di Decisione
Sono stati sviluppati molti algoritmi per costruire in modo efficiente una decisione accurata.

L'idea di base di questi algoritmi è quella di far *crescere l'albero in modo [[011 Algoritmi Greedy|greedy]]* effettuando una serie di ottimizzazioni locali quando si sceglie la caratteristica più significativa da utilizzare per partizionare i dati.

> [!note]- Iterative Dichotomiser3 (ID3)
>L'algoritmo **Iterative Dichotomiser3** (*ID3*) utilizza una strategia "*greedy top-down*", selezionando l'attributo ottimale per dividere il dataset ad ogni iterazione senza effettuare [[025 Tecnica Backtracking|backtracking]].
>
>![[Pasted image 20231216140646.png|850]]

> [!note]- C4.5
>L'algoritmo **C4.5** è una versione migliorata di ID3 che introduce il backtracking, attraversando l'albero costruito e sostituendo i rami con i nodi foglia se la purezza è migliorata.
>
>![[Pasted image 20231216140853.png|450]]

> [!note]- Chi-squared Automatic Interaction Detector (CHAID)
>L'algoritmo **Chi-squared Automatic Interaction Detector** (*CHAID*) è comunemente impiegato nel *marketing diretto* per selezionare gruppi di consumatori, prevedendo come le loro risposte a alcune variabili influenzino altre variabili. 
>
>Sebbene le sue prime applicazioni siano state nei campi della ricerca medica e psichiatrica, il CHAID utilizza *concetti statistici complessi* per determinare il modo ottimale di *combinare le variabili predittive* al fine di spiegare al meglio il risultato.

>L'algoritmo più importante è il **Classification and Regression Tree** (*CART*). 
>Costruisce *solamente alberi binari* che crescono attraverso regole `if else` e applicando split binari. 

In modo greedy, cerca la *combinazione più significativa tra una caratteristica e il suo valore*, sperimentando tutte le possibili combinazioni e testandole con una funzione di misurazione. 

![[Pasted image 20231216141348.png|600]]

Il dataset viene diviso in modo che i campioni con il **valore** della caratteristica (per caratteristiche categoriche) o un valore superiore (per caratteristiche numeriche) diventino il *figlio destro*, mentre i **campioni rimanenti** diventano il *figlio sinistro*. Questo processo di suddivisione si ripete in modo ricorsivo fino a ottenere sottogruppi.

#### Criteri di Arresto
Il processo di suddivisione in un albero di decisione si interrompe quando uno dei seguenti criteri è soddisfatto:
1. **Numero minimo di campioni**: l'albero smette di crescere quando il numero di campioni in un nodo *non è sufficiente per una suddivisione ulteriore*. Questo evita che l'albero si adatti eccessivamente ai dati di addestramento.

2. **Profondità massima dell'albero**: il processo di crescita si ferma quando la profondità di un nodo raggiunge il *limite massimo prestabilito*. La profondità è il numero di suddivisioni dal nodo radice al nodo corrente. Limitare la profondità aiuta a prevenire l'[[Overfitting]].

Un nodo *senza ulteriori suddivisioni diventa una foglia* e rappresenta la classe dominante tra i campioni del nodo, che sarà quindi la previsione. Una volta completate tutte le suddivisioni, l'albero è costruito con etichette nei nodi terminali e i punti di divisione (caratteristica + valore) nei nodi interni superiori.


___
[[000 Indice ML|↖ Ritorna all'indice ↖]]

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