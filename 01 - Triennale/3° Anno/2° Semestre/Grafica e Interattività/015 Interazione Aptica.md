---
aliases: 
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Teoria Grafica
cssclasses:
  - 
---
>L'**Aptica** è la scienza che studia e acquisisce informazioni sul senso del *tatto* e delle interazioni con l'ambiente tramite esso. 
>Ha possibili applicazioni in [[013 VR e AR|VR e AR]].

Essenzialmente, il tatto è una *perturbazione meccanica* della pelle, prodotta dal contatto fisico con un oggetto. Questa dipende dalle proprietà fisiche dell'oggetto e da come si esplora l'oggetto.

![[Pasted image 20240602110659.png]]

È un canale bidirezionale, in quanto vi sono dei *feedback* che consentono di stabilire delle informazioni sulle svariate sensazioni che possono manifestarsi al tocco di un oggetto: 
- Dimensione e forma
- Proprietà materiali come: texture, temperatura, peso e altre

Ci sono due livelli attraverso i quali il senso del tatto fornisce informazioni:
- Il *livello cinestetico* - Si riferisce al senso di posizione e movimento di parti del corpo in associazione alle forze esercitate da un oggetto. È la percezione del movimento e della posizione degli arti.
- Il *livello tattile* - Permette di ottenere informazioni sul tipo di contatto e sulle proprietà fisiche degli oggetti.

## Interfacce Aptiche
Le **Interfacce Aptiche** (Haptic Interface, HI) servono ad orientare l'utente sulla *posizione e sulla natura degli oggetti* nello spazio virtuale.
Ci sono tre tipi di interfacce:
1. Sistemi robotici che restituiscono all'utente *sensazioni cinestetiche di forza*
2. Dispositivi in grado di trasmettere all'utente *sensazioni di forza*
3. Dispositivi che riproducono informazioni sensoriali tattili (*tactile feedback*) e/o cinestetiche (*force feedback*)

Un dispositivo di interfaccia aptica è un apparecchio elettromeccanico *collegato permanentemente all'arto dell'utente*, come un guanto con sensori, che interagisce con un sistema di attuatori. I movimenti dell'utente sono rilevati da un *sistema di controllo* che attiva gli attuatori per fornire un feedback di forza basato sulla posizione dell'oggetto con cui sta interagendo. 

![[Pasted image 20240602110426.png]]

Esistono due tipi principali di interfacce aptiche:
- Ad impedenza, che *generano forza* in base alla posizione e velocità
- Ad ammettenza, che utilizzano misure di forza per *determinare* posizione o velocità, sebbene siano meno comuni

## Haptic Rendering
>L'**haptic rendering** è il processo mediante il quale si *forniscono forze all'utente* basate sulla sua interazione con un ambiente virtuale, permettendogli di toccare, sentire e manipolare oggetti virtuali attraverso un'interfaccia aptica. Questo include la gestione del refresh delle informazioni aptiche trasmesse. 

![[Pasted image 20240602112615.png]]

Poiché il senso del tatto è *molto più sensibile* rispetto alla vista, è difficile riprodurre in modo convincente la sensazione aptica. Mentre una scena 3D deve essere aggiornata 25-30 volte al secondo (25/30Hz) per essere visualizzata correttamente, il tatto richiede che una sensazione sia aggiornata almeno 1000 volte al secondo (1000Hz) per offrire un'esperienza tattile realistica.


### Azioni dell'Interazione Aptica

- Dall'utente verso il sistema aptico:
	1. **Leggere la posizione del dispositivo**: L'utente interagisce con il dispositivo aptico e i sensori rilevano la posizione.
	2. **Verificare collisione con mondo virtuale**: Il sistema verifica se c'è una [[U_012 Fisica di un GameObject|collisione]] con l'ambiente virtuale.
- Dal sistema aptico all'utente:
	1. **Calcolare la forza $F$ di reazione**: Se c'è una collisione, il sistema calcola la forza di reazione:
		- Se non vi è contatto con l'oggetto, l'oggetto non produce alcuna forza di ritorno;
		- Se si penetra l'oggetto, anche se di poco, l'oggetto produce una forza generalmente proporzionale alla distanza di penetrazione.
	2. **Inviare $F$ a dispositivo aptico**: La forza calcolata viene inviata al dispositivo aptico.
	3. **Modificare mondo virtuale**: Il mondo virtuale viene aggiornato in base alle azioni dell'utente.


___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

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