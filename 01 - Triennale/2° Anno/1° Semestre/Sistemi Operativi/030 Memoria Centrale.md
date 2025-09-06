---
aliases:
  - Memoria Principale
  - RAM
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Memoria
cssclasses:
  - 
---
La **Memoria Centrale** (anche conosciuta come [[070 Gerarchia di Memoria#ACCESSO CASUALE|RAM]] o Random Access Memory) è fondamentale nelle operazioni di un moderno sistema di calcolo.
Questa è una memoria *volatile* che consiste in un grande array di bytes, ciascuno con il *proprio indirizzo*.

La memoria centrale e i [[058 Registri|Registri incorporati]] nel processore sono le sole aree di memorizzazione a cui la CPU *può accedere direttamente*, pertanto tutti i dati necessari al funzionamento di un [[Processo (Job)|Processo]] *devono risiedere nella RAM*. Se sono presenti solamente sul disco, li si devono portare in memoria.

In genere i registri sono accessibili nell'arco di un [[057 Clock#CLOCK|ciclo di clock]], mentre la memoria ne può richiedere diversi. Per questo si trova tra i registri e la RAM una memoria [[070 Gerarchia di Memoria#MEMORIA CACHE|cache]] che tenta di conciliare le diverse velocità tra CPU e memoria.

### BASE E LIMITE
Per proteggere i dati in memoria di un processo e quindi assicurare una corretta modalità di funzionamento, si usano due registri: 
- **Registro Base**: contiene il più piccolo indirizzo legale della memoria fisica;
- **Registro Limite**: determina la dimensione dell'intervallo ammesso.

Si ha un esempio in figura.
![[Scan2022-11-24_150041.jpg|400]]

Per mettere in atto il meccanismo di protezione la CPU confronta ciascun indirizzo generato con i valori contenuti nei due registri. Qualsiasi tentativo da parte di un programma di accedere alle aree di memoria riservate al sistema operativo o a una qualsiasi area di memoria riservata ad altri utenti comporta l'invio di una eccezione che restituisce il controllo al SO.

![[Pasted image 20221124151311.png|640]]

---
### ASSOCIAZIONE DEGLI INDIRIZZI
In genere un programma eseguibile risiede in un disco sotto forma di un file binario. Per essere eseguito, il programma va caricato in memoria e inserito nel contenuto di un processo. 

Il collegamento delle istruzioni e dei dati agli **indirizzi di memoria** può essere effettuato in:
- *Fase di compilazione*: in fase di compilazione si alloca direttamente in memoria uno spazio di dimensione specificata e tutti gli indirizzi che si trovano nel codice si sommano all'indirizzo dell'inizio dello spazio;

- *Fase di caricamento*: il compilatore genera un codice rilocabile. Il caricatore associa indirizzi rilocabili ad indirizzi di memoria. Se l'indirizzo iniziale cambia è sufficiente ricaricare in memoria il codice;

- *Fase di esecuzione*: se il processo può essere spostato, durante l'esecuzione, da un segmento di memora ad un altro, allora il collegamento deve essere ritardato fino al momento dell'esecuzione. Necessita di supporto hardware.

![[Scan2022-11-24_152307.jpg|400]]

In fase di compilazione e caricamento si ha un [[Indirizzo Logico]] e [[Indirizzo Fisico|Fisico]] identici. Invece, in esecuzione, questi sono diversi.

L'associazione nella fase d'esecuzione dagli indirizzi logici a quelli fisici è svolta dal dispositivo chiamato **Unità di Gestione della Memoria**.

---
### CARICAMENTO DINAMICO (SWAPPING)
Prima era necessario che l'intero programma e i dati di un processo fossero presenti in memoria per far funzionare il tutto, limitando la dimensione del processo alla dimensione della memoria.

Per migliorare l'utilizzo della memoria si può ricorrere al **Caricamento Dinamico**, mediante il quale *si carica una procedura solo quando viene chiamata*.
Se una procedura non è chiamata, questa rimane sul disco in un formato rilocabile.

Questo avviene tramite lo **Swapping**, dove un processo può essere temporaneamente spostato e poi riportato in memoria centrale (*swap out*, *swap in*).
La maggior parte del tempo di swap è preso dal trasferimento che è direttamente proporzionale alla quantità di memoria interessata.

![[Pasted image 20221124155417.png|480]]

---
### ALLOCAZIONE CONTIGUA DI MEMORIA
Uno dei metodi più semplici per l'allocazione della memoria consiste nel suddividere la stessa in **partizioni** di dimensione variabile, dove ciascuna partizione può contenere esattamente un processo.
In questo sistema viene conservata una tabella in cui sono indicate le partizioni di memoria libere ed occupate.

![[Pasted image 20221124160407.png|600]]

Per soddisfare una richiesta di dimensione $n$ da una lista di blocchi liberi si possono usare tre soluzioni:
- *First-fit*: (questo è quello più veloce perché non deve esaminare l'intera tabella) assegna il primo blocco libero abbastanza grande per contenere lo spazio richiesto;
- *Best-fit*: assegna il più piccolo blocco libero abbastanza grande;
- *Worst-fit*: (meno efficiente) assegna il più grande blocco libero.

Questi però possono provocare [[012 Allocazione contigua di file#^0ef617|frammentazione esterna]].

___
[[000 Indice SO|↖ Ritorna all'indice ↖]]

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