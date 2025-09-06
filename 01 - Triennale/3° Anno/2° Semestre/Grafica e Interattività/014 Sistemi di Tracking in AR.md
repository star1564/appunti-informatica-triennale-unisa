---
aliases: 
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Teoria Grafica
cssclasses:
  - 
---
## L'importanza del tracciamento
Perché il paradigma operativo della [[013 VR e AR#Realtà Aumentata|AR]] funzioni, è necessario che immagini reali e contenuti virtuali siano **co-registrati** tra loro. Ovvero che bisogna *allineare con precisione le immagini reali e i contenuti virtuali* in modo che appaiano integrati e coerenti.

Anche il più piccolo degli errori del sistema può portare a problemi notevoli.

![[Pasted image 20240602095238.png]]

Una delle maggiori difficoltà nelle applicazioni di AR è, appunto, il calcolo in real-time del punto di vista dell'utente. Questo risultato si raggiunge grazie ad un buon **tracciamento dei movimenti** dell'utente tramite *sistemi di Motion Tracking* (anche noti come di Motion Capture, 6DOF Mouse, etc.). Il tracciamento dei movimenti permette anche la corretta gestione della telecamera del punto di vista dell'utente, aumentando la co-registrazione.

I dispositivi di motion tracking 3D possono essere usati per capire *cosa sta facendo l'utente* e con cosa stanno interagendo in un ambiente [[013 VR e AR#Realtà Aumentata|AR]]. Sono classificati in base alla tecnologia usata per la misurazione.


## Tipologie Classiche
### Sistemi meccanici
Sono sistemi a bracci che utilizzano potenziometri o encoder ottici per *misurare la rotazione dei perni* di vincolo delle aste di collegamento. Noti gli angoli di ciascun giunto e le lunghezze delle aste della catena cinematica, è possibile *calcolare con facilità la posizione dell'utente* tracciato. 

![[Pasted image 20240601122751.png]]

Un altro tipo di tracciamento meccanico misura le forze esercitate mediante un misuratore di deformazione. Quest'ultima tecnica è spesso utilizzata in joystick sensibili alla forza applicata.

> [!success] Vantaggi
> - La semplicità costruttiva e la mancanza di unità di trasmissione-ricezione ne riducono il costo e la latenza. 
> - Sono meno sensibili a interferenze dell'ambiente esterno come, ad esempio, i campi elettromagnetici.

> [!failure] Svantaggi
> - Solitamente il volume di lavoro è piccolo e le parti in movimento sono soggette ad usura, inoltre l'utente è vincolato nei movimenti.

### Sistemi elettromagnetici
Sono costituiti da un trasmettitore e da un ricevitore. Il trasmettitore genera un *campo magnetico fluttuante* mediante tre spire tra loro ortogonali, che è rilevato da tre simili spire nel ricevitore. 

La variazione del segnale è tradotta in una variazione di posizione e di orientazione a 6 DOF (degree of freedom).

> [!success] Vantaggi
>  - Sono dispositivi molto comodi grazie alla piccola dimensione dei ricevitori (meno di 2 cm), il che permette di attaccarli con facilità alla testa, alle mani o ad una penna. 
>  - È possibile estendere l'area di lavoro combinando più dispositivi tra loro.

> [!failure] Svantaggi
> - Molto sensibile alle interferenze elettromagnetiche (EMI) di apparecchi elettronici come radio o monitor, così come materiali ferromagnetici nelle vicinanze possono deformare il campo elettrico e quindi compromettere la precisione della misura. 
> - Lentezza della risposta, che è di circa 100ms, anche se, nei dispositivi più recenti, tale valore è stato ridotto.

### Sistemi Acustici
Un emettitore genera un *segnale sonoro* e un microfono lo raccoglie misurando il tempo impiegato dal suono per percorrere il cammino (chiamato tempo di volo o TOF). Un elaboratore raccoglie i dati di più dispositivi (ne servono tre coppie per misurare sei gradi di libertà) e calcola la posizione e l'orientamento.

![[Pasted image 20240601123443.png|600]]

> [!success] Vantaggi
> - Economici e di facile reperibilità.

> [!failure] Svantaggi
> - La velocità del suono varia con le condizioni ambientali, quindi sono sistemi poco precisi se non si tiene conto di pressione, temperatura e umidità dell'aria. 
> - Inutilizzabili in ambienti dotati di pareti che riflettono le onde sonore, che creano immagini fantasma delle sorgenti.
> - Volume di funzionamento limitato.

### Sistemi Inerziali
Questo tipo di tecnologia, nata nei sistemi di guida di missili e aerei utilizza i *giroscopi* per misurare cambiamenti di rotazione attorno a uno o più assi.

> [!success] Vantaggi
> Non sono necessari sistemi di trasmissione-ricezione.

> [!failure] Svantaggi
> - Costo alto.
> - Necessità di ricalibrazione e la sensibilità alla temperatura.

### Sistemi ottici
Il sistema è costituito da sorgenti luminose disposte sull'oggetto da tracciare e da *telecamere che ne rilevano la posizione*. Solitamente si utilizzano sorgenti a infrarossi perché sono invisibili e quindi non creano confusione all'utente.

![[Pasted image 20240601123832.png|500]]

> [!success] Vantaggi
> - Accuratezza.
> - Capaci di gestire un'area di lavoro molto grande (fino a 100m con il laser). 

> [!failure] Svantaggi
> - Richiedono che sia mantenuta la continuità del cammino ottico tra proiettore e sensore.
> - Grande complessità e costo. 

## Approccio Image-Based
Questo tipo di approccio cambia radicalmente il concetto di tracking dell'utente. Infatti, non sarà più legato all'intercettazione dei segnali trasmittente/ricevente ma sul *riconoscimento di speciali figure* dette **marker** (marker-based). Da essi si ricava un *punto di riferimento* per la scena inquadrata, sulla base del quale inserire i contenuti augmentati.

![[Pasted image 20240602100339.png|500]]

> [!success] Vantaggi
> - Il costo è praticamente nullo della stampa dei marker su carta e quello di una camera per l'acquisizione della scena osservata.

> [!failure] Svantaggi
>- Il tracciamento è possibile fintanto che il marker risulta inquadrato dalla camera. Per questa ragione è richiesto che la focale della camera sia ampia.
>- Il flusso video della camera deve essere privo di rumore.
>- La frequenza di campionamento della camera deve essere elevata (almeno 30 [[Frames per Second|FPS]]).

> [!info] ARToolkit
> Una libreria open-source scritta in C/C++ orientata allo sviluppo di applicazioni di realtà aumentata marker-based.

### Marker
I marker sono delle speciali figure che ARToolkit intercetta e traccia all'interno di un flusso video proveniente da una camera. Un marker ha dei vincoli precisi:
- Deve essere perfettamente *quadrato* con delle proporzioni ben definite;
- I bordi esterni devono essere *ben delineati* e privi di interruzioni;
- L'immagine interna al marker deve essere completamente *asimmetrica*. Ciò è essenziale affinché la detection sul suo orientamento non sia ambigua;
- Le immagini interne ai marker devono essere abbastanza *diverse tra di loro* per evitare ambiguità.

![[Pasted image 20240602100831.png|400]]


### Approccio Multimarker
La detection non dipende unicamente da un marker bensì da una *costellazione di questi*, di cui sono noti gli spiazzamenti (offset) degli uni rispetto agli altri. Questo permette alla camera di non perdere il tracciamento di un marker quando non è più in vista.

![[Pasted image 20240602101535.png]]

Contando sulla presenza di più marker, il sistema è in grado di assicurare un più fedele tracciamento in quanto con maggiore probabilità uno o più marker della costellazione verranno inquadrati dalla camera.

Inoltre, se il sistema commette un errore di detection su un marker (ombre, occlusioni, noise, ecc.) la presenza di altri marker può compensare l'errore, aumentando di gran lunga la precisione del tracciamento.

## Approccio Markerless
Ci sono circostanze in cui la presenza di un marker potrebbe creare problemi (questioni di estetica, impossibilità di incollarli sulla superficie, ecc.) L'impatto che una soluzione **markerless** ha sull'ambiente di lavoro è praticamente nullo sebbene il riconoscimento degli oggetti diventa molto più complicato.

Delle speciali *depth-camera* riconoscono e tracciano, con una considerevole precisione, i movimenti dell'utente in uno spazio di lavoro limitato dalla capacità di acquisizione delle medesime camere.

![[Pasted image 20240602102620.png]]

Questo approccio consente di utilizzare *qualsiasi parte dell'ambiente fisico* come base per il posizionamento dei contenuti 3D, attraverso la *scansione e il tracciamento dell'ambiente reale*.

Questo è particolarmente utile quando si vuole vedere l'aspetto di un oggetto in ambienti e impostazioni diverse. Un esempio è il posizionamento di un mobile virtuale in una stanza per vedere come si adatta. Alcuni dispositivi sono in grado di riconoscere le superfici piane e di identificarle come “terreno”, in modo che l'oggetto AR si posizioni lì invece di fluttuare nell'aria.

Richiede una fase di **feature extraction**, ovvero l'individuazione delle caratteristiche salienti degli oggetti presenti in un frame. Il tracking diventa principalmente *matching delle caratteristiche*, svolto anche attraverso algoritmi di [[001 Machine Learning|machine learning]].

![[Pasted image 20240602103356.png|600]]



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
- https://www.assemblrworld.com/blog/3-different-types-of-marker