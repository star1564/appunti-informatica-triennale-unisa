---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Introduzione
cssclasses:
  - 
---
Dopo aver visto i [[004 Architettura di Rete|livelli di rete]] in senso astratto, osserviamo ora il modello di riferimento **ISO-OSI** (International Standards Organization - Open System Interconnection), anche abbreviato a OSI.

Il modello OSI ha 7 livelli. 
![[Pasted image 20230916080134.png|500]]

## Livelli di Data Flow
Questi livelli sono **responsabili per la trasmissione dei dati tra i dispositivi** di rete. Si concentrano sull'invio, la ricezione, la frammentazione e l'assemblaggio dei dati per garantire che possano essere trasmessi in modo affidabile attraverso la rete. I livelli sono i seguenti:

| #   | Nome                                             | Riceve                            | Invia                           | Descrizione                                                                                                                                                                                                     |
| --- | ------------------------------------------------ | --------------------------------- | ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **[[006 Livello Fisico\|Livello Fisico]]**       | <font color="FF6611">Frame</font> | <font color="00C575">Bit</font> | <ul><li>Si occupa della *trasmissione di bit grezzi sul canale di comunicazione*</li><li>Specifica voltaggi, velocità su cavo ed altro riguardante i *componenti fisici* che compongono questo livello</li></ul> |
| 2   | **[[008 Livello Data Link\|Livello Data Link]]**       | <font color="FF6611">Pacchetto</font> | <font color="00C575">Frame</font> | <ul><li>Trasforma la linea fisica in una linea in cui si possono *segnalare errori di trasmissione*</li><li>Divide le informazioni in frame e li trasmette attraverso il mezzo fisico, attendendo un segnale di "avvenuta ricezione" detto anche *ack*</li><li>Gestisce l'eventuale duplicazione dei frame ricevuti, causata dalla perdita dell'ack</li></ul> |
| 3   | **[[022 Livello di Rete\|Livello di Rete]]**       | <font color="FF6611">Datagramma</font> | <font color="00C575">Pacchetto</font> | <ul><li>Controlla il funzionamento della *sottorete*</li><li>Gestisce l'*indirizzamento* universale dei nodi in rete e l'*instradamento* dei pacchetti</li><li>Può gestire la *congestione* (problemi infrastrutturali) e controlla il *flusso* (quantità di dati)</li><li>Implementa *interfacce per la comunicazione* tra reti di tipo diverso</li></ul> |
| 4   | **[[029 Livello di Trasporto\|Livello di Trasporto]]**       | <font color="FF6611">Dati</font> | <font color="00C575">Datagramma</font> | <ul><li>Accetta dati dal livello superiore, per dividerli in unità più piccole quando necessario, passarle al livello di rete e assicurarsi che tutti i pezzi *arrivino correttamente a destinazione*</li><li>Assicura un servizio *privo di errori* end to end con l'*ordine corretto di ricomposizione*</li><li>Fornisce il servizio di recapito dei messaggi senza garanzia di arrivo</li></ul> |


## Livelli di Processo o Applicazione
Questi livelli sono **responsabili dell'interazione con le applicazioni** software degli utenti. Sono *spesso riuniti in un singolo livello* (nel modello *TCP/IP*), chiamato **[[035 Livello di Applicazione|Livello di Applicazione]]**.

|  #    |  Nome                                              |  Descrizione                                                                                                                                                                                                                        |
|:------|:---------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  5    |  **Livello di Sessione**                           |  <ul><li>Permette a utenti su computer diversi di stabilire tra loro una sessione</li><li>Controlla il dialogo tra due macchine</li><li>Gestisce il controllo dei token e la sincronizzazione del trasferimento dei dati</li></ul>  |
|  6    |  **Livello di Presentazione**                      |  <ul><li>Si occupa della sintassi e della semantica dell'informazione trasmessa</li><li>Queste informazioni vengono poi riconvertite nel formato proprietario della macchina destinataria</li></ul>                                 |
|  7    |  **Livello Applicazione**                          |  <ul><li>Implementa specifici servizi applicativi che interfacciano direttamente l'utente.</li><li>Un protocollo applicativo largamente usato è [[003 HTTP\|HTTP]]</li></ul>                                                        |  

___
[[000 Indice RC|↖ Ritorna all'indice ↖]]
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