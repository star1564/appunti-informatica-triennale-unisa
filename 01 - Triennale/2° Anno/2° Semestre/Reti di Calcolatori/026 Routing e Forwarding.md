---
aliases: ARP
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Rete
cssclasses:
  - 
---
>Il **Routing** è il processo mediante il quale i pacchetti di dati vengono *instradati* da una sorgente a una destinazione attraverso una rete di computer o dispositivi di rete interconnessi. In altre parole, il routing *determina il percorso ottimale* che un pacchetto deve seguire per raggiungere la sua destinazione.

Durante il routing, i pacchetti vengono inviati da un dispositivo chiamato "router" o "gateway" lungo una serie di *hop* (salti) da un nodo di rete all'altro. Ogni router *prende decisioni* sul percorso da seguire basandosi su informazioni contenute nella loro **tabella di routing**, che elenca le possibili destinazioni di rete e le relative interfacce di uscita.

![[Immagine 2023-05-29 101444.png]]

Durante il routing:
- Viene *estratto* il campo *destinazione*;
- Si cerca la destinazione nella *routing table*;
- Si trova la prossima destinazione $X$ (*hop*), la quale può essere il dispositivo destinatario o il prossimo router;
- Il pacchetto viene *spedito* a $X$.

## Avvenimento del Forwarding
>Il [[022 Livello di Rete#Inoltro (Forwarding)|Forwarding]] si verifica quando un dispositivo di rete riceve un pacchetto sulla sua interfaccia di ingresso e *determina l'interfaccia di uscita appropriata* a cui inoltrare il pacchetto. Il compito principale del forwarding è quello di prendere la decisione di inoltro basandosi sulle informazioni contenute nell'header del pacchetto, come l'indirizzo di destinazione.

## Instradamento Diretto e Indiretto
Riguardo il routing [[025 IPv6|IPv6]], questo può essere diretto o indiretto:

**Instradamento diretto**: la trasmissione di un pacchetto avviene tra due stazioni connesse nella stessa sottorete *senza coinvolgere alcun router intermedio*. 

![[Pasted image 20230529101842.png|500]]
**Instradamento indiretto**: la trasmissione di un pacchetto avviene tra *due stazioni situate in sottoreti diverse*, coinvolgendo *router intermedi*: 
- Se l'host di destinazione non si trova in una sottorete a cui il router è direttamente connesso, inoltrerà il pacchetto al router successivo, il cui ripeterà tali controlli. 
- Se invece il destinatario si trova nella stessa sottorete del router che sta esaminando il pacchetto, si individua l'[[MAC (Indirizzo e Sottolivello)|indirizzo MAC]] del destinatario tramite procedura *ARP*.

![[Pasted image 20230529102103.png|600]]

## Protocollo ARP
Il problema principale è quello di trovare l'indirizzo [[MAC (Indirizzo e Sottolivello)#Indirizzo MAC|MAC]] corretto *a cui inviare il pacchetto*, contando che l'host *conosce solo l'indirizzo IP* del destinatario. 
Quindi si ci appoggia ad un protocollo chiamato **ARP** (*Address Resolution Protocol*).

Poniamo di avere un host con indirizzo IP $A_1$ e con indirizzo MAC $MA_1$ il quale deve inviare un pacchetto IP ad un host con indirizzo IP $A_2$ sulla stessa rete. ARP si procura le informazioni necessarie nel seguente modo:
- Viene *costruito un pacchetto data-link* (chiamato *ARP Request*) contenente $A_1, MA_1, A_2$ e $MA_2$, quest'ultimo contrassegnato da una serie di $0$;
- Tale pacchetto viene inviato in broadcast sulla rete locale;
- Tutti ricevono tale pacchetto ARP, ma solo l'host con MAC $MA_2$ lo processerà;
- L'host di destinazione creerà un pacchetto data-link (chiamato *ARP Response*) nella quale inserirà il campo mancante. Tale pacchetto verrà trasmesso in maniera diretta e non in broadcast;
- Viene quindi acquisito il MAC $MA_2$ rilegato all'indirizzo IP $A_2$.

![[Pasted image 20230529103233.png|600]]

Esiste anche l'*ARP Reverse*, che serve a trovare l'indirizzo IP associato ad un indirizzo MAC.

> [!info]- Come funziona (template)
> ![[Pasted image 20230602103002.png]]


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