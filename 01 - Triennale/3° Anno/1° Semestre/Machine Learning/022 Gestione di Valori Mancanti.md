---
aliases:
  - Cancellazione
  - Imputazione
tags:
  - corsi/informatica/machine_learning
paragrafo: Features Engineering
cssclasses:
  - 
---
>Nel processo di *preparazione dei dati* per l'apprendimento automatico, la presenza di **valori mancanti**, noti anche come *missing values*, rappresenta una delle sfide più comuni. Questi valori possono derivare da errori umani, interruzioni nel flusso dei dati, questioni di privacy e altri fattori. È fondamentale affrontare efficacemente i valori mancanti, poiché possono *compromettere le prestazioni dei modelli* di machine learning.

![[Pasted image 20231216101438.png|600]]

## Tipi di valori mancanti
I valori mancanti possono essere distinti in tre categorie principali:
1. **Mancanti del tutto casuali (MCAR)**: questi sono valori mancanti in modo *completamente casuale*, senza alcuna connessione con altre variabili o con i dati mancanti stessi. La mancanza di questi valori è *indipendente da qualsiasi fattore*, rendendoli "del tutto casuali".

![[Pasted image 20231216102322.png|150]]
<center>Il progettista potrebbe aver dimenticato di inserire il valore</center>

2. **Mancanti casuali (MAR)**: in questo caso, la probabilità che un valore manchi *dipende da altre variabili nel dataset*, e non dalla variabile mancante stessa.

![[Pasted image 20231216102107.png|150]]
<center>Le persone di genere A potrebbero non amare rilevare la propria età</center>

3. **Mancanti non casuali (MNAR)**: questi valori mancanti sono legati alla *natura della variabile stessa*.

![[Pasted image 20231216101800.png|250]]
<center>Le persone che non hanno specificato il reddito potrebbero avere un reddito molto alto</center>

---
Quando si incontrano valori mancanti, si possono generalmente applicare due strategie.

## Cancellazione
>La **Cancellazione** è una tecnica nella gestione dei valori mancanti che comporta la rimozione delle righe o delle colonne che *contengono valori mancanti dal dataset*. 

Si divide in:
- Cancellazione di *Colonne*;
- Cancellazione di *Righe*.

> [!warning] Si potrebbero cancellare dati importanti
> ![[Pasted image 20231219095928.png]]
> Applicando il criterio di cancellazione delle righe (o anche delle colonne) si ottiene una *distorsione dei dati*, Per esempio, non avremo più esempi di persone che NON hanno intenzione di partecipare.
> ![[Pasted image 20231219095749.png]]

### Cancellazione delle Colonne
>La strategia più usata è quella di **Eliminare la Colonna** che contiene il *maggior numero di valori mancanti*.

![[Pasted image 20231216102924.png|550]]


### Cancellazione delle Righe
>La strategia consiste nell'**Eliminare i Record** (*righe*) nella quale sono presenti il *maggior numero di valori mancanti*.

Questo metodo può funzionare quando:
- I valori mancanti sono *Completamente Casuali (MCAR)*;
- Il *numero di record con valori mancanti è piccolo* (ad esempio, inferiore allo 0,1%).

![[Pasted image 20231216103430.png|800]]

## Imputazione
>L'**Imputazione** è una tecnica nella gestione dei valori mancanti che *prevede l'assegnazione di valori sostitutivi* al fine di preservare la completezza dei dati senza dover eliminare intere righe o colonne. 

Questa strategia è spesso preferita quando la cancellazione dei dati mancanti potrebbe comportare una perdita significativa di informazioni.

![[Pasted image 20231216103951.png|700]]

Tutti i metodi di imputazione per i valori mancanti (ad eccezione dei metodi deduttivi) si basano implicitamente o esplicitamente sull'assunzione che *i dati siano di tipologia MAR (Missing at Random)*.

Si divide in:
- Imputazione Numerica (*valori numerici*):
	- Metodi Deduttivi;
	- Metodi Deterministici e Stocastici.
-  Imputazione Categorica (*valori categorici*).

> [!failure] Svantaggi
> Si rischia di iniettare dei pregiudizi e rumore ai dati.

### Imputazione Numerica - Metodi Deduttivi
>Il **Metodo Deduttivo** di imputazione si avvale delle informazioni presenti nel dataset per *dedurre il valore da assegnare* a un dato mancante, *utilizzando una o più variabili ausiliarie*. Questa strategia si basa sulla possibilità di fare inferenze o *deduzioni logiche* in relazione al fenomeno studiato.

Per una corretta applicazione di questi metodi è necessario avere un buon grado di conoscenza dei dati e delle relazioni esistenti al loro interno.

> [!example]- <font color="orange">Esempio</font>
>Deduzione per sottrazione:
>![[Pasted image 20231216105104.png|800]]
>
>Deduzione utilizzando l'età dell'utente attraverso il codice fiscale oppure la data di nascita:
>![[Pasted image 20231216105157.png|800]]

### Imputazione Numerica - Metodi Deterministici e Stocastici
>Nel **Metodo Deterministico e Stocastico** di imputazione, i valori mancanti sono sostituiti *con un unico valore*, ottenuto attraverso la *[[Media]]*, *[[Mediana]]* o *[[Moda]]* della variabile corrispondente. 

I dati che prendono parte al processo di imputazione devono avere le *stesse caratteristiche*, quindi non possono essere diversi tra loro.

> [!example]- <font color="orange">Esempio</font>
>[[Media]]: $\frac{5000+2000+1275+2000+750}{5}=\color{#CC241D}2205$
>
>![[Pasted image 20231219100055.png]]
>
>[[Moda]]: $\textcolor{#CC241D}{2000}:\text{2 volte}$
>[[Mediana]]: $750<1275<\textcolor{#CC241D}{2000}<2000<5000$
>
>![[Pasted image 20231219100226.png]]

### Imputazione Categorica
>L'**Imputazione Categorica** è una tecnica utilizzata quando ci sono dati mancanti in *variabili categoriche* di un dataset. Questo approccio mira a sostituire i valori mancanti con *categorie stimate o dedotte* in base alle informazioni disponibili.

> [!example]- <font color="orange">Esempio</font>
> Il missing value è sostituito con il valore con la *maggiore frequenza* oppure un valore di *default*.
> 
> ![[Pasted image 20231216110536.png|500]]

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