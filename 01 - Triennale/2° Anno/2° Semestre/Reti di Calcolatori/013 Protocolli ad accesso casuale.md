---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Data Link
cssclasses:
  - 
---
In questo tipo di protocollo, un nodo trasmette sempre alla velocità massima consentita dal canale ($K$ b/s). Non essendoci coordinazione tra i nodi, c'è sempre il rischio di collisione. ^49a907

## ALOHA puro
> Nell'**ALOHA puro**, ogni dispositivo ha la possibilità di *trasmettere in qualsiasi momento*, senza una sincronizzazione con gli altri dispositivi sulla rete. Quando un dispositivo ha un pacchetto di dati da inviare, lo trasmette *immediatamente* sulla rete.

Se la trasmissione del pacchetto si sovrappone a quella di un altro dispositivo sulla stessa frequenza, si verifica una collisione tra i pacchetti e entrambi i dispositivi ricevono un segnale di errore. In tal caso, i dispositivi riproveranno a trasmettere il pacchetto in un momento successivo, in modo casuale.

![[Pasted image 20230425115740.png|400]]

### Slotted ALOHA
> Nello **Slotted ALOHA**, il canale di comunicazione viene *diviso in intervalli di tempo*, chiamati *slot*, di lunghezza fissa. Ogni dispositivo ha la possibilità di inviare un pacchetto dati in uno qualsiasi degli slot disponibili, ma può farlo solo all'inizio di un determinato slot.

Se un dispositivo trasmette in un determinato slot, gli altri dispositivi che desiderano trasmettere devono *attendere il prossimo slot disponibile*, in modo da evitare collisioni tra i pacchetti. In caso di collisioni, i dispositivi che hanno trasmesso rilevano l'errore e ritrasmettono il pacchetto in un altro slot successivo.

![[Pasted image 20230425115341.png]]

Slotted ALOHA ha il vantaggio di ridurre le collisioni tra i pacchetti, rispetto ad altri protocolli di accesso casuale come il semplice Aloha, dove i dispositivi possono trasmettere in qualsiasi momento senza un'organizzazione predefinita. Tuttavia, Slotted Aloha richiede un sincronismo tra i dispositivi per coordinare l'invio dei pacchetti all'inizio degli slot, e non è adatto per reti ad alta densità di traffico o con dispositivi a basso costo e bassa potenza.

## CSMA
> Nel **CSMA** (*Carrier Sense Multiple Access*, Accesso multiplo a rilevazione della portante) ogni dispositivo *controlla il canale di comunicazione prima di trasmettere* i dati. 

Se il canale è occupato (collisione), il dispositivo *attende* finché il canale non diventa libero prima di iniziare a trasmettere i dati. Questo processo viene definito *ascolto del canale* (carrier sense), in quanto il dispositivo controlla se il canale è occupato dal segnale di un altro dispositivo.

![[Pasted image 20230425120430.png]]

Può capitare, però, che agli altri nodi sul canale non venga notificata in tempo la trasmissione di un certo nodo, facendo risultare libero il canale (*ritardo di propagazione*), risultando in una collisione. In questo caso verranno bloccate le trasmissioni, facendo attendere per un tempo casuale i nodi.

### CSMA persistente
> Nel **CSMA persistente**, un nodo ascolta e *controlla continuamente il canale*, attendendo che si liberi. In caso il canale fosse occupato (collisione), *attende un tempo casuale e ripete i controlli*.

![[Pasted image 20230425120854.png]]

### CSMA non persistente
> Il **CSMA non persistente** si differenzia dal persistente dal fatto che quando un nodo vuole trasmettere ma trova il canale occupato, non resta ad ascoltare in continuazione, *ma attende un tempo casuale e riprova*.

![[Pasted image 20230425123026.png]]

Tale meccanismo riduce sensibilmente le collisioni, siccome ogni stazione attende un tempo randomico. 
Il problema di tale tecnica è che, nel caso ci siano molteplici nodi connessi alla rete, il tempo di attesa può crescere enormemente.

### CSMA p-persistente
> Nel **CSMA p-persistente**, chi vuole trasmettere ascolta continuamente il canale e, se nel caso questo fosse libero, *trasmette con probabilità $P$ nello slot attuale*.

Nel caso non trasmetta, quindi con probabilità $1-P$, attende il prossimo slot.

### CSMA/CD
> Nel **CSMA/CD** (Collision Detection), ogni nodo ascolta continuamente il canale e trasmette solo se è libero. 

![[Pasted image 20230425123947.png]]

Se si verifica una collisione, i nodi bloccano la trasmissione. A seguito della collisione, il nodo mittente trasmette una serie di *jamming* (interferenze) per comunicare a tutti i nodi della collisione. Il nodo mittente riproverà poi la trasmissione dopo un tempo casuale.

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