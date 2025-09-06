---
aliases:
  - Requirements Specification
  - Modello Funzionale
  - Modello dei Casi d'Uso
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Analisi dei Requisiti - Estrazione
cssclasses:
  - 
---
>Una **Specifica dei Requisiti** definisce un sistema che risolve un problema individuato dal cliente, dagli sviluppatori e dagli utenti. Serve da *contratto tra il cliente e gli sviluppatori* e si focalizza sulla descrizione del sistema da sviluppare. È scritto in un *linguaggio naturale*.

I seguenti procedimenti *mappano* un [[010 Problem Statement|Problem Statement]] in una Specifica dei Requisiti.

> [!info] Formalizzazione della Specifica dei Requisiti
> Quello che si otterrà dalla Specifica dei Requisiti verrà riscritto (attraverso una formalizzazione) all'interno del [[017 Requirement Analysis Document|Requirement Analysis Document]], il documento più importante dell'intero progetto.

### Modello Funzionale (Modello dei Casi d'Uso)
Seguendo i passaggi, si va anche a creare un **Modello Funzionale**, che *descrive le funzionalità che un sistema software deve fornire* ([[008 Requisiti#Requisiti Funzionali|Requisiti Funzionali]]). Fornisce una descrizione dettagliata delle azioni che il sistema può eseguire e dei risultati che queste azioni produrranno. 

È tipicamente *rappresentato* in diversi modi, tra cui i *casi d'uso*.

## 1 - Identificare gli Attori
>Il primo passo dell'estrazione dei requisiti è quello di **identificare gli [[005 UML#Attori|Attori]]**, con cui si possono *definire dei confini del sistema* e far trovare agli sviluppatori tutte le *prospettive che devono considerare*.

Durante le fasi iniziali dell'identificazione degli attori, è difficile distinguere gli attori dagli oggetti. Ad esempio, un database può a volte essere un attore, mentre in altri casi può essere parte del sistema. 

Una volta definito il **confine del sistema**, non è più difficile distinguere tra attori e componenti del sistema come oggetti o sottosistemi: 
- *gli attori sono al di fuori del confine del sistema*, sono esterni; 
- i sottosistemi e *gli oggetti sono all'interno del confine del sistema*; sono interni. 

Pertanto, qualsiasi sistema software esterno che utilizza il sistema da sviluppare è un attore. 

> [!question]- Come identificare gli attori
>Nell'identificare gli attori, gli sviluppatori possono porsi le seguenti domande:
>- Quali gruppi di utenti sono supportati dal sistema per svolgere il loro lavoro?
>- Quali gruppi di utenti eseguono le funzioni principali del sistema?
>- Quali gruppi di utenti svolgono funzioni secondarie, come la manutenzione e l'amministrazione?
>- Con quale sistema hardware o software esterno interagirà il sistema?

## 2 - Identificare gli Scenari
>Il secondo passo dell'estrazione dei requisiti è quello di **identificare gli [[011 Scenari e Casi d_Uso#Scenario|Scenari]]**, utilizzati per determinare le funzionalità che saranno accessibili *a ciascun attore*. 

Nell'estrazione dei requisiti, gli sviluppatori e gli utenti scrivono e perfezionano una serie di scenari per ottenere una *comprensione condivisa* di ciò che il sistema dovrebbe essere. Inizialmente, ogni scenario può essere di alto livello e incompleto. 

> [!question]- Come identificare gli scenari
> Per identificare gli scenari si possono utilizzare le seguenti domande:
>- Quali sono i compiti che l'attore vuole che il sistema svolga?
>- A quali informazioni può accedere l'attore? Chi crea questi dati? Possono essere modificati o rimossi? Da chi?
>- In quali situazioni l'attore deve informare il sistema di un cambiamento esterno (come il cambiamento del prezzo di un prodotto)? Con quale frequenza? Quando?
>- Di quali eventi il sistema deve informare l'attore? Con quale latenza?

Per rispondere a queste domande, gli sviluppatori spesso utilizzano documenti esistenti sul [[002 Sistemi, Modelli e Viste#^ae7aa9|dominio applicativo]], come manuali d'uso di sistemi precedenti, manuali di procedure e altro. Infatti, l'identificazione degli attori e degli scenari serve per far capire meglio il dominio applicativo agli sviluppatori.

Disegnare *[[004 Modelli di CVS#^920367|mock-up dell'interfaccia utente]]* spesso aiuta a trovare omissioni nelle specifiche e a costruire un'immagine più concreta del sistema.

## 3 - Identificare e Raffinare i Casi d'Uso
>Il terzo passo dell'estrazione dei requisiti è quello di **identificare e raffinare i [[011 Scenari e Casi d_Uso#Caso d'Uso|Casi d'Uso]]**, attraverso *l'espansione e la formalizzazione degli scenari* individuati.

*Associare i casi d'uso agli attori che lo hanno inizializzato* consente agli sviluppatori di chiarire i **ruoli** dei diversi utenti. Spesso, concentrandosi su chi avvia ciascun caso d'uso, gli sviluppatori identificano nuovi attori precedentemente trascurati.

> [!tip]- Guida alla scrittura di semplici casi d'uso
>- I casi d'uso devono essere denominati con frasi verbali. Il nome del caso d'uso deve indicare ciò che l'utente sta cercando di ottenere (ad esempio, `ReportEmergency`, `OpenIncident`).
>- Gli attori devono essere denominati con frasi sostantive (ad esempio, `FieldOfficer`, `Dispatcher`, `Victim`).
>- Il confine del sistema deve essere chiaro. I passi compiuti dall'attore e quelli compiuti dal sistema devono essere distinti (ad esempio, le azioni del sistema sono indentate a destra, mentre quelle dell'utente a sinistra).
>- I passi del caso d'uso nel flusso degli eventi devono essere formulati in voce attiva. Questo rende esplicito chi ha compiuto il passo.
>- La relazione tra i passi successivi deve essere chiara.
>- Un caso d'uso deve descrivere una transazione completa dell'utente (ad esempio, il caso d'uso `ReportEmergency` descrive tutti i passaggi tra l'avvio della segnalazione di emergenza e la ricezione di un riscontro).
>- Un caso d'uso non deve descrivere l'interfaccia utente del sistema. Ciò distoglie l'attenzione dai passi effettivi compiuti dall'utente ed è meglio affrontato con simulazioni visive (ad esempio, il caso d'uso `ReportEmergency` si riferisce solo alla funzione "Report Emergency", non al menu, al pulsante o al comando effettivo che corrisponde a questa funzione).
>- Specifica la Entry Condition (ad esempio, "Questo caso d'uso comincia quando...").
>- Specifica la Exit Condition (ad esempio, "Questo caso d'uso termina quando...").
>- Descrivi le [[023 Gestione delle Eccezioni|eccezioni]].
>- Lista tutti i requisiti non funzionali e vincoli.
>- Un caso d'uso non dovrebbe superare le due o tre pagine.

## 4 - Identificare le Relazioni tra Attori e Casi d'Uso
>Il quarto passo dell'estrazione dei requisiti è quello di **identificare le [[UML 1 - Diagramma Caso d_Uso#Relazioni tra Casi d'Uso|Relazioni tra i casi d'uso]] e gli attori**, permettendo agli sviluppatori di ridurre la complessità del modello.

Si usa l'[[UML 1 - Diagramma Caso d_Uso|Use Case Diagram]] per rappresentare queste relazioni.

| Relazione | Utilizzo |
| ---- | ---- |
| *Associazione* | Rappresentare il flusso delle informazioni durante il caso d'uso. |
| *Extend* | Separare gli eventi straordinari da quelli normali.<br>Usarla per comportamenti eccezionali, opzionali o rari. |
| *Include* | Ridurre la ridondanza nei casi d'uso. <br>Usarla per un comportamento che è condiviso tra due o più casi d'uso. |

## 5 - Creare un Glossario di Termini (oggetti partecipanti)
>Per stabilire una terminologia chiara, gli sviluppatori e gli utenti *identificano una serie di termini utilizzati* (inerenti al dominio dell'applicazione) all'interno dei casi d'uso. Questi termini vengono inseriti all'interno di un **glossario**.

La costruzione di questo glossario costituisce il primo passo verso l'analisi dei requisiti.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240117173936.png]]

## 6 - Identificare i Requisiti Non Funzionali
>L'ultimo passo dell'estrazione dei requisiti è quello di **identificare i [[008 Requisiti#Requisiti Non Funzionali|Requisiti Non Funzionali]]**.

> [!question]- Domande da farsi per trovare i requisiti non funzionali
> ![[Pasted image 20240117174537.png]]



___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

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