---
aliases: 
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Teoria Grafica
cssclasses:
  - 
---
## Definizione
>La **Stereoscopia** è una tecnica di realizzazione e [[010 Visione, Percezione e l'Occhio#Visione|visione]] di immagini, disegni, fotografie e filmati, atta a trasmettere una *illusione di tridimensionalità* (quindi anche di profondità), analoga a quella generata dalla visione binoculare dell'occhio umano.

In pratica funziona con l'utilizzo di due immagini, prese da *angolazioni differenti*, che vengono presentate *separatamente* ai due occhi su un piano di proiezione, in modo che ogni occhio veda solo l'immagine di competenza.

![[Pasted image 20240601104907.png|300]]

Il cervello attua un processo di *fusione delle due immagini* elaborate dalla vista che dà all'osservatore una illusione di tridimensionalità.

### Parallasse
I due occhi dell'essere umano proiettano su un piano di proiezione immaginario le immagini elaborate dal senso della vista. L'angolo formato dai piani di proiezioni normali alla direzione di osservazione di ciascun occhio costituisce la **parallasse**:
- Maggiore è la parallasse $\alpha$, maggiore è la *vicinanza del punto osservato* e di conseguenza lo sforzo per la vista di mettere a fuoco quel determinato punto. 
- Minore è la parallasse $\alpha$, minore sarà lo sforzo di messa a fuoco.

![[Pasted image 20240601104823.png|700]]

Però, la parallasse è limitata. Questo pone dei vincoli anche alla riproduzione della visione stereoscopica.

Non si può pensare di allargare enormemente la parallasse così da illudere l'osservatore che l'oggetto osservato sia troppo vicino, altrimenti si percepirebbe uno *sdoppiamento del punto di osservazione* (ad esempio quando si cerca di focalizzare la propria visione sulla punta del proprio naso).

### Stereoscopia Moderna
Il funzionamento di base è medesimo, ma è cambiato il modo in cui le immagini vengono inviate in maniera distinta a ciascun occhio dell'osservatore.

Ci sono due tipi di stereoscopia.

## Stereoscopia Passiva
Nella **stereoscopia passiva** due immagini vengono proiettate *contemporaneamente* sullo stesso schermo.
L'osservatore indossa degli *occhiali* che filtrano lo stream video, in modo che su ciascuna lente venga impressa *solo* l'immagine che compete a quell'occhio.

Gli occhiali possono essere diversi a seconda del tipo di stereoscopia passiva utilizzata. Infatti esistono diverse tecniche di stereografia passiva.

### Anaglifi
Un **anaglifo** è una immagine speciale ottenuta per *sovrapposizione* di due fotogrammi di uno stereogramma che subiscono un processo di colorazione distinto. In particolare si usa il Rosso per l'immagine sinistra e il Ciano per l'immagine di destra.

![[Pasted image 20240601105833.png|550]]

> [!success] Vantaggi
> - Estremamente economica
> - Facilmente fruibile

> [!failure] Svantaggi
> La predominanza di rosso/ciano impatta sulla resa cromatica dell'immagine risultante

### Polarizzazione
La **Polarizzazione** utilizza un *doppio proiettore* le cui lenti sono dotate di filtri polarizzatori, orientati *ortogonalmente* uno rispetto all'altro, così da proiettare due immagini polarizzate in modo differente l'una dall'altra.

Lo spettatore viene dotato di un paio di occhiali montanti anch'essi due lenti polarizzate, in modo tale che ogni occhio visualizzi solamente l'immagine ad esso destinata.
È il sistema adottato per la proiezione cinematografica.

![[Pasted image 20240601110036.png]]


> [!success] Vantaggi
> - Non sono richieste costose schede grafiche
> - Costi contenuti per l'hardware di proiezione

> [!failure] Svantaggi
> - Può essere usato solo in ambienti in cui sono richiesti pochi movimenti della testa (come il cinema)
> - Lo schermo di proiezione deve essere di uno speciale materiale polarizzante

### Gamma di Colore
Viene impiegata una *ruota di colori alternati* integrata nel proiettore. Questa ruota di colori contiene un set di filtri rossi, verdi e blu in più rispetto a una ruota tipica. 

Il set supplementare di tre filtri produce la stessa [[004 Illuminazione#Definizione di Luce|gamma di colore]] della ruota tipica, ma è in grado di trasmettere la luce a lunghezze d'onda diverse. Le lenti degli occhialini filtrano alternativamente l'uno o l'altro set di filtri trasmettendo a ciascun occhio l'immagine giusta.

![[Pasted image 20240601110539.png|300]]

> [!success] Vantaggi
> - Scarsi fenomeni di ghosting
> - Indipendenza dalla rotazione della testa
> - Migliore separazione delle immagini
> - Qualsiasi superficie può fungere da schermo

> [!failure] Svantaggi
> - Costi elevati
> - Le immagini subiscono un'alterazione cromatica che va corretta via software

## Stereoscopia Attiva
Nella **stereoscopia attiva** due immagini vengono proiettate in maniera *alternata* sullo stesso schermo. Degli speciali occhiali detti *shutter glasses*, sincronizzati
con il proiettore, *oscurano in maniera alternata* le lenti cosicché ciascuna immagine sia indirizzata all'occhio di competenza. 

In questo modo, su ciascuna lente è impressa *solo una delle due immagini* proiettate a video. Il cervello umano elabora i flussi video catturati dai due occhi e riproduce il senso della tridimensionalità.

Affinché questo funzioni, è richiesta una **frequenza di aggiornamento** (refresh rate) per ciascun occhio di *60 Hz*, quindi il proiettore deve averne il doppio (120 Hz).

![[Pasted image 20240601111242.png|500]]

> [!success] Vantaggi
> - Effetto stereoscopico molto realistico
> - Sfarfallii ed effetti di sdoppiamento quasi del tutto assenti

> [!failure] Svantaggi
> - Proiettore costoso
> - Gli occhiali sono spesso molto delicati
> - Non adatto ad un elevato numero di osservatori, in quanto tutti dovrebbero ricadere nell'area di copertura di sincronizzazione proiettore/occhiali

## Libera visione stereoscopica
La **libera visione stereoscopica**, non definisce una tecnologia, ma la *capacità innata di osservare* in rilievo uno stereogramma. Non serve null'altro che il senso della vista ed uno stereogramma.

Ci sono due tecniche diverse per poter vedere in stereoscopia una immagine 2D contando solo sull'uso della vista.

### Visione stereoscopica Incrociata
Ogni occhio guarda l'immagine opposta:
- L'occhio **destro** guarda l'immagine *sinistra*
- L'occhio **sinistro** guarda l'immagine *destra*

![[Pasted image 20240601111752.png|200]]

È relativamente semplice da imparare, in quanto richiede una posizione naturale degli occhi che avvicinano il piano di osservazione.

Per provare è sufficiente avere uno stereogramma ed osservarlo *incrociando gli occhi*. Inizialmente le due immagini appariranno sdoppiate ma lentamente, per un processo di compensazione attuato dal cervello, lentamente le due immagini si sovrappongono perfettamente dando come risultato una immagine nitida con la percezione della profondità.

### Visione stereoscopica Parallela
Ogni occhio guarda l'immagine pertinente:
- L'occhio **destro** guarda l'immagine *destra*
- L'occhio **sinistro** guarda l'immagine *sinistra*

È la più difficile da imparare perché richiede una posizione meno naturale degli occhi che allontanano il piano di osservazione fino a volte oltre l'infinito (occhi divergenti).
Infatti, è necessario avvicinare molto gli occhi a quest'ultime, rilassarli e muoversi lentamente indietro finché le due confuse immagini che si vedono diventino una sola.

![[Pasted image 20240601112104.png|200]]

## Autostereoscopia
A differenza della stereoscopia classica, nell'**Autostereoscopia** l'immagine tridimensionale può essere osservata in rilievo *senza richiedere l'uso di altri dispositivi* (come lo stereoscopio o gli occhiali), in quanto il supporto (stampa su carta o monitor) è *munito di una tecnologia apposita* che provvede già da sé a nascondere ad ogni occhio l'immagine destinata all'altro.

Esistono diverse tecniche di visualizzazione autostereoscopia.

### Barriera di Parallasse
Un filtro chiamato **Barriera di Parallasse** distribuisce *in alternanza* i punti di vista destinati all'uno o all'altro dei due occhi.

![[Pasted image 20240601113023.png|400]]

Le posizioni laterali per vedere bene l'immagine sono tutte alla stessa distanza dal piano dell'immagine.

### Rete Lenticolare
L'impressione di rilievo è ottenuta grazie a una **rete lenticolare** (*rete di microlenti*) collocata sulla superficie dell'immagine, costituita da *immagini ad incastro*  rappresentanti ognuna un punto di vista preso da un angolo differente.

La rete permette di indirizzare a ciascun occhio un'immagine differente che il cervello dell'osservatore ricostruirà come unica ed in rilievo.

![[Pasted image 20240601113338.png]]

Una delle caratteristiche di questi schermi è quella di restituire *più punti di vista* dei due strettamente necessari in modo da permettere un più libero posizionamento degli spettatori: questi schermi sono detti "multiscopici".

### Olografia
Un **Elemento Ottico Olografico** (HOE - Holographical Optical Element) è posto davanti allo schermo di visualizzazione. Le immagini per i due occhi sono ognuna proiettata da un proiettore LCD e *riflesse da uno specchio su uno schermo convesso*.

Le condizioni di *illuminazione ambientale* giocano un ruolo fondamentale sul successo o meno della illusione di tridimensionalità.

![[Pasted image 20240601113443.png]]


### Autostereogramma
L'**Autostereogramma**, è uno stereogramma *a singola immagine*, realizzato per creare una illusione ottica tridimensionale da un'immagine bidimensionale.
Al fine di percepire una forma tridimensionale in un autostereogramma, gli occhi devono superare il normale coordinamento automatico tra messa a fuoco e [[010 Visione, Percezione e l'Occhio#^aa2cbb|convergenza]]. 

Con un autostereogramma il cervello riceve *pattern ripetuti* da entrambi gli occhi ma non riesce a combinarli correttamente, infatti *li combina in due differenti pattern* adiacenti che formano un oggetto virtuale basato su angoli di parallasse errati, ponendo così l'oggetto virtuale ad una profondità diversa da quella dell'autostereogramma.

![[Pasted image 20240601113718.png|800]]



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