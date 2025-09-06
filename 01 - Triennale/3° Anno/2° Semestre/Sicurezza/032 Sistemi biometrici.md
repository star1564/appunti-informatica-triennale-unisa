---
aliases: 
tags:
  - corsi/informatica/sicurezza
paragrafo: Mezzi di Autenticazione
cssclasses:
  - 
---
>La **Biometria** consiste nell'utilizzo di strumenti *per riconoscere tratti* fisici e comportamentali *unici* di una persona. Sono usati per autenticare una singola persona, oppure per identificare da una a più persone.

> [!question] Per via della loro unicità, vengono usati anche per il controllo degli accessi in sedi governative



![[Pasted image 20240616101024.png]]


> [!question] Firma grafometrica
> È al pari della [[022 Firme Digitali|firma digitale]], ha la medesima efficacia probatoria della scrittura privata.


### Fasi di un Sistema Biometrico
![[Pasted image 20240617151921.png|800]]

1. **Enrollment** - È un *processo iterativo* di acquisizione ed elaborazione dei dati biometrici dell'utente *per l'utilizzo da parte del sistema nelle successive operazioni* di autenticazione/identificazione
   
   ![[Pasted image 20240617151956.png|700]]
2. **Identificazione/Autenticazione**: Acquisizione ed elaborazione dei dati biometrici degli utenti *per prendere una decisione di autenticazione*, basata sul risultato di un processo di matching tra il modello (template) memorizzato e quello corrente
	- *Identificazione*: Cerca una corrispondenza all'interno di un database di modelli
	- *Autenticazione*: Confronta un campione (sample) con un singolo modello archiviato

![[Pasted image 20240617152018.png|700]]



### Sistemi Biometrici Unimodali
I sistemi biometrici che utilizzano una *singola biometria* sono detti **Sistemi biometrici unimodali**. Tali sistemi possono avere delle limitazioni legate al fatto che viene utilizzata una singola biometria.

Una singola biometria, ad un certo punto, potrebbe *non soddisfare* una o più delle seguenti caratteristiche base:
- *Universalità*: ogni individuo deve possedere quella determinata caratteristica biometrica
- *Unicità*: non è possibile che due persone condividano la stessa identica caratteristica biometrica
- *Permanenza*: la caratteristica biometrica deve rimanere immutata nel tempo, stabilisce il grado di permanenza di una determinata biometria e può determinare la stabilità a breve o a lungo termine di un sistema biometrico
- *Catturabilità*: la caratteristica biometrica deve poter essere acquisita e quantitativamente misurata

### Sistemi Biometrici Multimodali
I sistemi biometrici che utilizzano *più di una singola biometria* sono detti **Sistemi biometrici multimodali**.

La fase di progettazione di un sistema biometrico multimodale deve tener conto di diversi aspetti:
- [[008 Requisiti#Requisiti Funzionali|Requisiti Funzionali]]
- Quali biometrie utilizzare
- Il livello architetturale dove effettuare la *combinazione* delle biometrie (**fusione**) 

Ci sono tre possibili scelte progettuali:
1. **Progettazione in Parallelo** - L'acquisizione e la valutazione delle biometrie vengono svolte in maniera *indipendente* per ciascuna biometria. Le valutazioni sono poi *combinate mediante la fusione*, che *può avvenire in diversi moduli* dell'architettura di tale sistema.

![[Pasted image 20240617144345.png]]

2. **Progettazione in Serie** - L'acquisizione e la valutazione delle biometrie vengono svolta in *sequenza*, in modo indipendente per ciascuna biometria. Il tempo di elaborazione è ridotto se viene presa una decisione prima di passare attraverso tutti i sottosistemi biometrici.

![[Pasted image 20240617144546.png]]

3. **Progettazione a Livello Gerarchico** - Combina i vantaggi delle architetture seriali e parallele. Un sottoinsieme delle biometrie acquisite può essere combinato *in parallelo*, mentre le restanti biometrie possono essere combinate *in modo seriale*. 
   L'architettura è determinata dinamicamente in base alla qualità dei singoli campioni biometrici ed alla possibilità di acquisire i dati biometrici mancanti.
   Può superare le limitazioni di un sistema biometrico unimodale.

![[Pasted image 20240617144702.png]]

___
[[000 Indice Sicurezza|↖ Ritorna all'indice ↖]]

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