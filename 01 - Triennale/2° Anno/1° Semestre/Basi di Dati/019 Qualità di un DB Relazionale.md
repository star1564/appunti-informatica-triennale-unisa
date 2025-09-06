---
aliases: 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: Modello Relazionale
cssclasses:
  - 
---
Finora, nella [[013 Mapping DB Relazionale|traduzione verso il modello logico]], non si è sviluppato alcuna misura di adeguatezza per misurare la *qualità* della progettazione, se non l'intuizione dello sviluppatore.

Ci sono due livelli ai quali possiamo esaminare se uno schema relazionale è da considerarsi migliore di un altro:
- **Livello Logico**:
	- Specifica come gli utenti interpretano gli schemi di relazione e il significato dei loro attributi.
	- Avere buoni schemi di relazione a questo livello consente agli utenti di capire chiaramente il significato dei dati nelle relazioni, e quindi di formulare correttamente le loro [[Query]].
- **Livello di Implementazione**:
	- Specifica come le [[010 Modello Relazionale#^Righe|tuple]] in una relazione di base sono memorizzate e aggiornate (solo per le relazioni di base).

## LINEE GUIDA INFORMALI PER LA PROGETTAZIONE DI SCHEMI DI RELAZIONE
Queste possono essere usate come **Misure Informali** per determinare la qualità della progettazione di un DB relazionale:
1. Assicurarsi che la semantica degli attributi sia chiara nello schema;
2. Riduzione dei valori ridondanti nelle tuple;
3. Riduzione del numero di valori nulli nelle tuple;
4. Non consentire tuple spurie.

Queste misure non sono sempre indipendenti tra loro.

---
### 1. SEMANTICA ADATTA PER ATTRIBUTI E RELAZIONI
Ogni volta che si raggruppano [[ER3_Attributo|attributi]] per formare uno schema di relazione, si presuppone che a questi venga associato un significato nel mondo reale.

La **Semantica** specifica come interpretare i valori degli attributi di una relazione. Più è semplice spiegare la semantica della relazione, migliore è il disegno dello schema relazionale.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20230103095833.png]]
><center>f.k. = Chiave Esterna; p.k. = Chiave Primaria</center>
>Tutte le relazioni possono essere considerate buone dal punto di vista di avere una semantica chiara.

> [!summary] Linea Guida 1
> Si progetti uno schema di relazione in modo tale che sia semplice spiegarne il significato.
>
> Non si uniscano attributi provenienti da più tipi di entità e tipi di relazione in un'unica relazione. Intuitivamente, se uno schema di relazione corrisponde ad *un* tipo di entità o ad *un* tipo di relazione, il significato tende ad essere chiaro.

> [!example]- <font color="orange">Esempio violazione della Linea Guida 1</font>
> ![[Pasted image 20230103100932.png]]
> `EMP_DEPT`: ogni entità rappresenta un impiegato ma include informazioni sul dipartimento in cui lavora;
> `EMP_PROJ`: ogni tupla correla un impiegato a un progetto ma richiede anche il nome dell'impiegato `ENAME` ed il nome e la locazione del progetto, `PNAME` e `PLOCATION`.
>
> Tali schemi di relazione hanno anch'essi una semantica chiara, però entrambe contravvengono la linea guida , contendendo attributi di entità distinte.

---
### 2. RIDUZIONE DEI VALORI RIDONDANTI NELLE TUPLE
Uno degli scopi della progettazione di schemi è ridurre al minimo lo *spazio di memoria* occupato dalle relazioni di base.

> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20230103104000.png]]
> <center>SCHEMA 1</center>
>
>![[Pasted image 20230103104020.png]]
><center>SCHEMA 2</center>
>
> Si confronti lo spazio occupato dalle due relazioni di base `EMPLOYEE` e `DEPARTMENT` dello SCHEMA 1 con lo spazio della relazione di base `EMP_DEPT` (che è una [[018 Operazioni di Join|JOIN]] a `EMPLOYEE` di `DEPARTMENT`).
> In `EMP_DEPT`, i valori degli attributi che riguardano uno specifico dipartimento sono ripetuti per ogni impiegato che lavora per quel dipartimento.

La memorizzazione di join applicati a relazioni di base porta a un ulteriore problema noto come **Anomalie di Aggiornamento**. Esse possono essere classificate in tre tipi.
Useremo come esempio la relazione `EMP_DEPT`.
![[Pasted image 20230103104650.png]]

#### ANOMALIE DI INSERIMENTO
Possono essere distinte in due tipi:
1. Per inserire una nuova tupla impiegato, dobbiamo anche richiedere i valori degli attributi del dipartimento per cui questo lavora. Inoltre tali valori devono essere consistenti.
2. È difficile inserire un nuovo dipartimento che non ha ancora impiegati nella relazione `EMP_DEPT`. L'unico modo è di inserirlo ponendo a null gli attributi per l'impiegato, ma ciò crea un problema poiché l'SSN è la chiave primaria. Inoltre inserendo il primo impiegato per quel dipartimento non servirebbe più la tupla con valori null.

#### ANOMALIE DI CANCELLAZIONE
Questo problema è legato alla seconda situazione delle anomalie di inserimento.

Se si cancella da `EMP_DEPT` una tupla impiegato che rappresenta l'ultimo impiegato che lavora per un particolare dipartimento, l'informazione sul dipartimento è persa sul database.

#### ANOMALIE DI MODIFICA
Se cambiamo il valore di uno degli attributi di un particolare dipartimento, dobbiamo aggiornare tutte le tuple di tutti gli impiegati che lavorano per quel dipartimento. Altrimenti il database diventa inconsistente.

> [!summary] Linea Guida 2
> Si progettino gli schemi di relazione di base in modo che nelle relazioni non siano presenti anomalie di inserimento, cancellazione o modifica.
>
> Se sono presenti anomalie, le si rilevi chiaramente e ci si assicuri che i programmi che aggiornano la base di dati operino correttamente.

---
### 3. RIDUZIONE DEI VALORI NULL NELLE TUPLE

> [!info]+ Interpretazioni di null
> Il valore null può avere diverse interpretazioni:
> - L'attributo non si applica a questa tupla;
> - Il valore dell'attributo per questa tupla non è noto;
> - Il valore dell'attributo è noto ma assente, cioè non è stato ancora registrato.

È possibile che nei progetti di alcuni schemi vengano raggruppati numerosi attributi a formare una "grossa" relazione. 
Se molti di questi attributi non riguardano tutte le tuple della relazione, quelle tuple finiscono con l'avere molti valori nulli. 

Ciò può dar luogo a uno spreco di spazio di memoria e può portare problemi con le funzioni di aggregazione COUNT e SUM.

> [!summary] Linea Guida 3
> Per quanto possibile, si eviti di porre in una relazione di base degli attributi i cui valori possono essere frequentemente null.
>
> Se i valori null sono inevitabili, assicurarsi che essi ricorrano in casi eccezionali e non per la maggioranza delle tuple.

---
### 4. GENERAZIONE DI TUPLE SPURIE
Si considerino i due schemi di relazione seguenti.

![[Pasted image 20230103110114.png]]
Le tuple rappresentano:
- `EMP_LOCS`: l'impiegato di nome `ENAME` lavora per qualche progetto la cui locazione è `PLOCATION`;
- `EMP_PROJ1`: l'impiegato con codice `SSN` lavora per `HOURS` ore alla settimana sul progetto avente il numero `PNUMBER`, nome `PNAME` e locazione `PLOCATION`.

Questa suddivisione non corrisponde ad un buon disegno di DB in quanto, volendo riottenere le informazioni di `EMP_PROJ` mediante un natural join su due schemi, si ottengono **Tuple Spurie**, rappresentanti informazioni errate.

Tale problema sorge perché `PLOCATION` è l'attributo che correla `EMP_LOCS` e `EMP_PROJ1`, ma non è né chiave primaria, né chiave esterna in nessuna delle due relazioni.

> [!summary] Linea Guida 4
> Progettare gli schemi di relazione in modo da poter effettuare JOIN con condizioni di uguaglianza su attributi che sono o chiave primaria o chiave esterna, in modo da non generare tuple spurie.


___
[[000 Indice BD|↖ Ritorna all'indice ↖]]

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