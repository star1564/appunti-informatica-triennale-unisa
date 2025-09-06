---
aliases: 
tags:
  - corsi/informatica/machine_learning
paragrafo: Model Development and Training
cssclasses:
  - 
---
In base a diversi aspetti strategici si possono identificare i *migliori algoritmi* da adottare per la risoluzione di uno specifico task. 
Infatti, quando bisogna selezionare un modello per un problema, non si sceglie tra tutti i modelli possibili, ma ci si concentra su **un insieme di modelli adatti al problema**.

La conoscenza dei task comuni di ML e degli approcci tipici per risolverli è essenziale in questo processo.
I diversi tipi di algoritmi richiedono un numero diverso di *etichette* e una diversa *potenza di calcolo*. Alcuni richiedono un *addestramento più lungo* di altri, mentre altri impiegano *più tempo per fare previsioni*.

> [!example]+ <font color="orange">Esempio</font>
>Se si devono filtrare dei tweet tossici, si ha un problema di **classificazione del testo**, quindi i modelli comuni per questo problema sono:
>- [[031 Naive Bayes|Naive Bayes]]
>- [[008 Task di Regressione|Regressione Logistica]]
>- Reti Neurali Ricorrenti
>- ...
>
>Se si devono rilevare delle transazioni fraudolenti, si ha un problema di **rilevamento delle anomalie**, quindi i modelli comuni per questo problema sono:
>- K-Nearest Neighbors
>- Reti Neurali
>- [[024 Discretizzazione#^707338|Clustering]]
>- ...

Gli algoritmi classici di ML tendono a essere più **interpretabili** (ad esempio, quali feature hanno contribuito maggiormente a classificare un'email come spam) rispetto alle reti neurali.

Quando si valuta quale modello utilizzare, è importante considerare non solo le performance, misurate tramite metriche come l'[[019 Gestire Dataset Sbilanciati#^363748|Accuracy]] e l'[[019 Gestire Dataset Sbilanciati#^7c5678|F1 Score]], ma anche altre proprietà, come:
- **La quantità di dati, il calcolo e il tempo necessari per l'addestramento**;
- **La latenza dell'interferenza**;
- **L'interpretabilità**.

> [!example]- <font color="orange">Esempio</font>
>Un semplice modello di regressione logistica potrebbe avere un'accuratezza inferiore rispetto a una rete neurale complessa, ma richiede meno dati etichettati, è molto più veloce da addestrare, è molto più facile da implementare ed è anche molto più facile spiegare perché sta facendo certe predizioni

## Consigli per la selezione dei modelli
### Evitare la trappola dello stato dell'arte
I ricercatori spesso valutano i modelli solo in ambito accademico, per cui un modello migliore allo stato dell'arte **spesso** significa che ha prestazioni migliori rispetto ai modelli esistenti su alcuni set di dati statici.

Ma ciò **non implica** che il modello è *abbastanza veloce o economico* da poter essere implementato. Inoltre, non è detto che il modello avrà prestazioni migliori rispetto ad altri modelli sui *nostri dati specifici*.

La cosa più importante da fare quando si risolve un problema è *trovare le soluzioni che possono risolverlo*. Se esiste un modello che è molto più economico e semplice di modelli più avanzati, utilizzare la soluzione più semplice

### Iniziare con i modelli più semplici
La semplicità ha tre scopi:
1. I modelli più semplici **sono più facili da distribuire**, quindi consente *il prima possibile* di convalidare che le predizioni siano coerenti con i dati di addestramento.
2. Iniziare con qualcosa di semplice e aggiungere componenti più complessi passo dopo passo **rende più facile la comprensione** del modello e il suo debug.
3. Il modello più semplice **serve da riferimento** per confrontare i modelli più complessi.

### Evitare i pregiudizi nella valutazione dei modelli
Ci sono **molti pregiudizi umani** nella valutazione dei modelli. Parte del processo di valutazione di un'architettura di ML consiste nello sperimentare diverse feature e diversi set di [[Iper-parametro|iper-parametri]] per trovare il modello migliore per tale architettura. 

Se una persona è più entusiasta di un'architettura, probabilmente passerà molto più tempo a sperimentarla, il che *potrebbe portare a modelli più performanti per quell'architettura*.

Quando si confrontano architetture diverse, è *importante confrontarle con configurazioni simili*. 

> [!example] <font color="orange">Esempio</font>
> Se si eseguono 100 esperimenti per un'architettura, non è corretto eseguire solo un paio di esperimenti per un'altra architettura. Potrebbe essere necessario eseguire 100 esperimenti su di essa.

Poiché le prestazioni di un'architettura dipendono fortemente dal contesto in cui viene valutata, l'affermazione potrebbe essere vera in un contesto, ma improbabile per tutti i contesti possibili.

### Valutare le performance attuali rispetto a quelle future
Il modello migliore per la situazione attuale *non è detto che sia il modello migliore tra due mesi*. Un modo semplice per stimare come le performance di un modello
variano è quello di utilizzare le **curve di apprendimento**.

![[Pasted image 20240105103009.png|700]]

Una curva di apprendimento illustra graficamente *come variano le performance di un modello* all'aumentare del numero di campioni di addestramento utilizzati.

### Valutare i compromessi
Quando si scelgono i modelli, si devono fare molti compromessi. Capire *cosa è più importante* per le performance del sistema di ML aiuterà a scegliere il modello più adatto. Esempi classici di compromessi sono:
- **Falsi positivi e falsi negativi**: ridurre il numero di falsi positivi potrebbe aumentare il numero di falsi negativi e viceversa. 
	- In un task in cui i falsi positivi sono più pericolosi dei falsi negativi (ad esempio l'accesso a strutture tramite impronte digitali, le persone non autorizzate non dovrebbero essere classificate come autorizzate e avere accesso), si potrebbe preferire un modello che produce meno falsi positivi.

- **Requisiti di calcolo e accuratezza**: un modello più complesso potrebbe offrire un'accuratezza più elevata, ma potrebbe richiedere una macchina più potente per generare previsioni con una latenza di inferenza accettabile.

- **Interpretabilità e performance**: un modello più complesso può fornire performance migliori, ma i suoi risultati sono meno interpretabili.

### Comprendere le ipotesi di un modello
Il mondo reale è estremamente complesso e i modelli possono solo approssimarlo utilizzando delle **ipotesi**. *Ogni modello ha le sue ipotesi*. 
Capire quali sono le ipotesi di un modello e se i dati correnti le soddisfano può aiutare a valutare quale sia il modello migliore per il caso d'uso corrente. 

Ipotesi comuni sono: 
- **IID**: le reti neurali presuppongono che *gli esempi siano indipendenti e identicamente distribuiti*, il che significa che tutti gli esempi sono tratti in modo indipendente dalla stessa distribuzione congiunta.
- **Confini**: un classificatore lineare presuppone che i confini decisionali siano lineari.

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