---
aliases: 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: Modello Relazionale
cssclasses:
  - 
---
### DA SCHEMA CONCETTUALE A LOGICO
Terminata la fase di analisi concettuale del database e creato un [[002 Modelli di Dati#MODELLI CONCETTUALI (AD ALTO LIVELLO)|modello di alto livello]] che descrive il [[Mini-Mondo]], passiamo alla definizione di uno [[002 Modelli di Dati#MODELLI LOGICI (RAPPRESENTATIVI / IMPLEMENTATIVI)|schema logico]], più vicino al [[DBMS]] ma meno comprensibile alle persone normali.

![[Pasted image 20221126120300.png|500]]

È utile articolare la progettazione logica in due fasi:
- [[#FASI DI RISTRUTTURAZIONE ER|Ristrutturazione dello schema ER]].
- [[013 Mapping DB Relazionale|Traduzione verso il modello logico (Mapping)]].

I dati in ingresso sono lo schema concettuale ed il carico applicativo (dimensione dei dati e caratteristiche delle operazioni).

![[Pasted image 20221213094933.png|600]]

## FASI DI RISTRUTTURAZIONE ER
Prima di passare allo schema logico è necessario ristrutturare lo [[007 Modello ER|schema ER]] per:
- *Semplificare la traduzione*: non tutti i costrutti del modello ER hanno una traduzione naturale nei modelli logici;
- *Ottimizzare il progetto*: rendere più efficiente l'esecuzione delle operazioni.

Lo schema ER ristrutturato non è uno schema concettuale in senso stretto, tiene conto degli aspetti realizzativi per *rendere più efficienti le operazioni previste*.

### ANALISI DELLE PRESTAZIONI
Uno schema ER va modificato per ottimizzare alcuni **indici di prestazione** del progetto.

Le prestazioni di una base di dati non sono valutabili in maniera precisa in sede di progettazione logica. Ma è comunque possibile, facendo uso degli indici di prestazione, effettuare studi di massima dei due parametri che generalmente regolano le prestazioni dei sistemi software:
- *Costo di una operazione*: numero di occorrenze di entità e relazioni visitate per rispondere a una operazione;
- *Occupazione di memoria*: misurato in numero di byte.

Per studiare questi parametri abbiamo bisogno di conoscere, oltre allo schema, le seguenti informazioni:
- **Volume dei Dati**, ovvero:
	- il numero di occorrenze di ogni entità e relazione dello schema;
	- le dimensioni di ciascun attributo (di entità o relazioni).
- **Caratteristiche delle Operazioni**, ovvero:
	- il tipo dell'operazione (*interattiva*: richiede l'input da parte dell'utente; *batch*: è possibile eseguire uno script automatico);
	- la frequenza (il numero medio di esecuzioni in un certo intervallo di tempo);
	- i dati coinvolti (entità e/o relazioni).

Entrambe (a cui viene aggiunta quella degli accessi) hanno le proprie "*tavole*".

- *Tavola dei Volumi*: vengono riportati tutti i concetti dello schema (entità e relazioni) con il volume previsto a regime.
- *Tavola delle Operazioni*: viene riportata per ogni operazione la frequenza prevista ed il tipo.
- *Tavola degli Accessi*: viene riportata, per ogni operazione, il numero di accessi ai concetti coinvolti ed il tipo di accesso:
	- Le operazioni in *Lettura (L)* hanno valore $1$;
	- Le operazioni in *Scrittura (S)* hanno valore $2$.


> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20221213102804.png|Da notare che "lavora per" = "afferisce"|700]]
>
>*Tavola dei volumi*:
>
>| Concetto     | Tipo | Volume |
>| ------------ | ---- | ------ |
>| Sede         | E    | 10     |
>| Dipartimento | E    | 80     |
>| Impiegato    | E    | 2000   |
>| Progetto     | E    | 500    |
>| Composto da  | R    | 80     |
>| Afferisce    | R    | 1900   |
>| Dirige       | R    | 80     |
>| Partecipa    | R    | 6000   | 
>
>*Numero delle occorrenze delle associazioni*:
>Dipende da due parametri:
>- Il numero delle occorrenze delle entità coinvolte nelle associazioni;
>- Il numero medio di partecipazioni di un'occorrenza di entità alle occorrenze di associazioni.
>
>Per esempio supponiamo che un impiegato lavori in media a tre progetti.
>
>*Operazioni da eseguire*:
>1. Assegna un impiegato ad un progetto.
>2. Trova i dati di un impiegato, in quale dipartimento lavora ed i progetti a cui partecipa.
>3. Trova i dati di tutti gli impiegati di un certo dipartimento.
>4. Per ogni sede, trova i suoi dipartimenti con il cognome del direttore e l'elenco degli impiegati del dipartimento.
>
>*Tavola delle operazioni*:
>
>| Operazione | Tipo | Frequenza     |
>| ---------- | ---- | ------------- |
>| Op. 1      | I    | 50 al giorno  |
>| Op. 2      | I    | 100 al giorno |
>| Op. 3      | I    | 10 al giorno  |
>| Op. 4      | B    | 2 a settimana | 
>
>*Schema di operazione* (OPERAZIONE 2):
>Per ogni operazione possiamo descrivere graficamente i dati coinvolti attraverso uno **schema di operazione**.
>Descrive il cammino logico da percorrere per accedere alle informazioni di interesse.
>
>![[Pasted image 20221213110609.png|450]]
>
>*Tavola degli Accessi* e *Costo di un'operazione* (OPERAZIONE 2):
>Si stima il costo di un'operazione contando il numero degli accessi alle occorrenze di entità e di relazioni.
>(Diciamo che ogni impiegato partecipa in media a 3 progetti)
>
>| Concetto     | Costrutto | Accessi | Tipo |
>| ------------ | --------- | ------- | ---- |
>| Impiegato    | E         | 1       | L    |
>| Afferisce    | R         | 1       | L    |
>| Dipartimento | E         | 1       | L    |
>| Partecipa    | R         | 3       | L    |
>| Progetto     | E         | 3       | L    | 
>
> Costo operazione 2: $9L \cdot 100/giorno = (9\cdot 1) \cdot 100/giorno = 900/giorno$

---
### RISTRUTTURAZIONE
La fase di ristrutturazione di uno schema ER è suddiviso in:
1. Analisi delle ridondanze.
2. Eliminazione delle generalizzazioni.
3. Partizionamento/Accorpamento di entità e associazioni.
4. Scelta degli identificatori primari.

#### ANALISI DELLE RIDONDANZE
Bisogna decidere se inserire una [[Ridondanza|ridondanza]], confrontando il costo delle operazioni che coinvolgono il dato ridondante e relativa occupazione di memoria nei casi di *presenza/assenza* di ridondanza.

#### ELIMINAZIONE DELLE GENERALIZZAZIONI
I sistemi tradizionali per la gestione di basi di dati non consentono di rappresentare direttamente una generalizzazione.
È necessario tradurre le generalizzazioni usando altri costrutti del modello ER.

![[Pasted image 20221213114146.png|700]]

Considerando lo schema iniziale qui sopra, ci sono tre metodi per eliminare le generalizzazioni:
- *Accorpamento delle figlie della generalizzazione nel padre*: 
	- conviene quando le operazioni non fanno molta distinzione tra le occorrenze tra gli attributi di $E_0$, $E_1$ ed $E_2$. Introduciamo valori nulli, ma abbiamo un minor numero di accessi; 
	  ![[Pasted image 20221213114213.png|600]]
	  <center>Le entità E1 ed E2 vengono eliminate e le loro proprietà vengono aggiunte al padre E0</center>

- *Accorpamento del padre della generalizzazione nelle figlie*:
	- è applicabile quando la generalizzazione è totale. Altrimenti le occorrenze di $E_0$, che non sono occorrenze né di $E_1$ né di $E_2$, non sarebbero rappresentate; !
	  ![[Pasted image 20221213114300.png|720]]
	  <center>Si rimuove il padre E0 e si inseriscono i suoi attributi nelle figlie E1 ed E2</center>

- *Sostituzione della generalizzazione con associazioni*:
	- è applicabile quando la generalizzazione NON è totale e ci sono operazioni che fanno distinzione tra entità padre ed entità figlie. assenza di valori nulli e incremento degli accessi. 
	  ![[Pasted image 20221213114547.png|700]]
	  <center>Le entità E1 ed E2 sono identificate esternamente dall'entità E0</center>

La ristrutturazione delle generalizzazioni è un tipico caso per cui il conteggio delle istanze e degli accessi *non permette sempre* di scegliere la migliore alternativa.

#### PARTIZIONAMENTO / ACCORPAMENTO DI CONCETTI
Gli accessi si riducono separando attributi di uno stesso concetto che vengono acceduti da operazioni diverse e raggruppando attributi di concetti diversi che vengono acceduti dalle medesime operazioni.

1. Il **partizionamento di entità** avviene attraverso:
	- La *Decomposizione Verticale* (Attributi): 
		- Suddivisione del concetto operando sulla struttura;
		- È conveniente quando lo stesso insieme di operazioni accede ad un sottoinsieme di attributi.
		- ![[Pasted image 20221213115427.png|650]]
	- La *Decomposizione Orizzontale* (Entità):
		- Agisce sulle occorrenze delle entità, lasciando la struttura invariata.
		- Occorre duplicare tutte le associazioni a cui l'entità originaria partecipava.

2. **Eliminazione di attributi multivalore**: Il [[010 Modello Relazionale|modello relazionale]] *non consente la rappresentazione di attributi multivalore*.

3. **Accorpamenti di entità**: 
	- Si accorpano i concetti operando sulla struttura e conviene quando le operazioni accedono a dati presenti su entrambe le entità.
	- Contiene valori nulli e si effettua generalmente su associazioni di tipo 1:1.
![[Pasted image 20221213120023.png|300]]![[Pasted image 20221213120044.png|300]]

#### SCELTA DEGLI IDENTIFICATORI PRIMARI
Qui avviene la scelta della [[ER5_ Attributi Chiave|chiave primaria]] per la costruzione degli indici per il recupero efficiente dei dati.

I *criteri generali* sono:
- Escludere attributi con valori nulli;
- Numero minimo di attributi (dimensione ridotte degli indici);
- Identificatore coinvolto in molte operazioni;
- Velocità di accesso all'indice.

Se nessuno degli identificatori candidati soddisfa tali criteri, si introduce un ulteriore attributo all'entità. Questo contiene i valore speciali (**Codici**) generati appositamente per identificare le occorrenze delle entità.

> [!example]- <font color="orange">Esempio</font>
> Si ha una entità Studente con due possibili attributi chiave:
> 1. Matricola (10 caratteri numerici);
> 2. Codice Fiscale (16 caratteri alfanumerici).
>
> È opportuno scegliere l'attributo Matricola perché la velocità di accesso all'indice è ridotta (meno caratteri da confrontare).

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