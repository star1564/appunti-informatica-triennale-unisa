---
aliases:
  - Sintattica,
  - Semantica
cssclasses:
  - 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Programmazione Modulare
---

Lo sviluppo di un programma è diviso in 4 fasi.
![[Pasted image 20220303105252.png|850]]

> [!warning] Versione poco precisa
> Queste note sono state create prima del corso di [[000 Indice IS|Ingegneria del Software]], dove una parte di questi argomenti verranno introdotti e spiegati in modo migliore.


## ANALISI
Si specifica cosa fa il programma, individuando:
- I *dati in ingresso* e i loro vincoli;
- I *dati in uscita* e i loro vincoli.

### VINCOLI
I vincoli sono definiti in:
- *Precondizione*: condizione definita sui dati di ingresso che deve essere soddisfatta affinché la funzione sia applicabile;
 ^bda15e
- *Postcondizione*: condizione definita sui dati di uscita e dati di ingresso e che deve essere soddisfatta al termine dell'esecuzione del programma, ovvero definisce cosa sono i dati di output in funzione di quelli di input. ^23536b

### SINTATTICA E SEMANTICA
**Sintattica:**
- Si riferisce alle regole ed alle regolazioni per scrivere codice in un linguaggio di programmazione.
- Non ha nulla a che fare con il significato del codice.
- Riguarda la grammatica e la struttura del linguaggio.
- Un codice è valido sintatticamente se segue tutte le regole di sintattica.

**Semantica:** ^a1bf25
- Si riferisce al significato associato al codice in un linguaggio di programmazione.
- Riguarda il significato del codice che fa capire facilmente un programma.
- Gli errori sono gestiti al runtime.

> [!example]+ <font color="orange">Esempio 1</font>
> *Dati in ingresso*: Sequenza di interi $v$.
> - Precondizione: `dim` > 0.
>
> *Dati in uscita*: Sequenza di interi $v1$.
> - Postcondizione: $v1$ è una permutazione di $v$ dove per ogni `i` in $v$, `v[i] < v[i+1]` (ovvero una sequenza ordinata).

> [!example]- <font color="orange">Esempio 2</font>
> ![[Pasted image 20220317113522.png]]


### DIZIONARIO DEI DATI
Si utilizza di solito un *dizionario dei dati*, una tabella con tre colonne, definite da: "Identificatore", "Tipo", "Descrizione".

<font color="orange">Esempio</font>: Ordinamento di una sequenza di interi.
![[Pasted image 20220303110052.png]]

## PROGETTAZIONE
Si specifica come il programma effettua la trasformazione specificata.

La progettazione dell'algoritmo sarà basata su una metodologia chiamata *Per Raffinamenti Successivi*. 

Avviene definendo degli step ed in base a questi step si effettua una decomposizione funzionale, ovvero decomporre il programma principale in subroutine.

<font color="orange">Esempio</font>: Ordinamento di una sequenza di interi.
1. Input sequenza $s$ in un array $a$ di dimensione $n$;
2. Ordina array $a$ di dimensione $n$ (array unico per input e output);
3. Output sequenza $s1$ contenuta in array $a$ di dimensione $n$.

Raffinamento del programma principale: definiamo delle funzioni corrispondenti agli step individuati.
- ``input_array(a,n)``
- ``ordina_array(a,n)``
- ``output_array(a,n)``

Per ognuna delle funzioni si esegue l'analisi, progettazione, codifica e verifica. 

## TESTING
**Testing** significa esercitare il programma con dati di test per verificare se il suo comportamento sia conforme a quello atteso.

- Viene detto *Oracolo* l'output atteso, quello che ci si aspetta il programma produca;
- Viene detto *Malfunzionamento* un comportamento del programma diverso da quello atteso.

L'obbiettivo del testing è quello di trovare i malfunzionamenti in un programma.

### TIPICI ERRORI DEL PROGRAMMA
Se il testing riporta un malfunzionamento, questo viene generato tipicamente da diverse fonti.  Per individuare e correggere il difetto che ha causato il malfunzionamento, eseguiamo il **Debugging**.

Queste sono le solite cause da controllare:
- Individuare i punti dove si può trovare il difetto ed inserire *punti di controllo* dello stato delle variabili attraverso delle `printf`, nel main e/o nelle funzioni;
- Vedere se tutte le dichiarazioni di variabili sono *inizializzate* (e se ne hanno bisogno). Questo lo si può notare se certe variabili hanno valori quasi casuali;
- Vedere se ci sono *errori ortografici* non riportati dal compilatore (come un `;` dopo un `if` o un ciclo, se non è necessario);
- *Riordinare* le chiamate delle funzioni;
- Sperare che questo problema non ti faccia perdere troppo tempo...

### TIPI DI CASI
Visto che i dati in input possono essere infiniti, individueremo delle classi di dati di test, selezioneremo un caso di test da ogni classe ed eviteremo test ridondanti. 
Quindi scegliamo *casi generali* e *casi particolari*.

### TIPI DI TESTING
Test suite: un insieme di casi di test per un programma.

Per testare programmi con più funzioni e file, si possono usare due strategie:

- *Big-bang* (per piccoli programmi): integra il programma con tutti i sottoprogrammi e lo verifica nel suo insieme.
- *Incrementali*: testare e integrare un sottoprogramma alla volta, considerando la struttura delle chiamate tra i sottoprogrammi.

Tra gli incrementali vediamo la strategia *Bottom-up*, che verifica prima i programmi terminali (quelli che non chiamano nessun altro sottoprogramma), poi quelli che li chiamano, ed infine il "main".

Per ogni sottoprogramma da verificare è necessario costruire un programma main (detto **Driver**) che:
- Acquisisce i dati di ingresso necessari al sottoprogramma;
- Invoca il sottoprogramma passandogli i dati di ingresso e ottenendo i dati di uscita;
- Visualizzare i dati di uscita del sottoprogramma.

### AUTOMATIZZARE IL TESTING
Si possono utilizzare i [[002 File in C|File]] e l'[[008 Oracolo e Driver|Oracolo]] per automatizzare il testing.

Lo stream indica una sorgente di input (come la tastiera) o una destinazione per l'output (come lo schermo).

Usiamo 2 file per dati di input:

- "**input.txt**" contenente gli elementi in input;
- "**oracle.txt**" contenenti gli output che ci aspettiamo;

```C
//Ordinamento di un Array

//input.txt
5
1 2 3 4 5
5 4 3 2 1
5 8 2 9 1

//oracle.txt
5
1 2 3 4 5
1 2 3 4 5
1 2 5 8 9
```

Per l'output:

- "**output.txt**" risultante dall'esecuzione del programma (output effettivo), oltre ad un' indicazione dell'esito del test (*PASS/FAIL*).

```C
//Ordinamento di un Array

//output.txt
5
1 2 3 4 5
5 4 3 2 1
5 8 2 9 1
```


___
[[000 Indice PSD|↖ Ritorna all'indice ↖]]

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