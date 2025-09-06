---
aliases: 
tags:
  - corsi/informatica/machine_learning
paragrafo: Model Development and Training
cssclasses:
  - 
---
Idealmente, i metodi di valutazione dovrebbero essere gli stessi sia in fase di sviluppo che di produzione. In molti casi, però, l'ideale è impossibile, perché *durante lo sviluppo si dispone di etichette di verità*, ma *in produzione no*.

Per alcuni task, è possibile dedurre o approssimare le etichette in produzione in base al feedback degli utenti. Ad esempio, per un task di raccomandazione, è possible dedurre se una raccomandazione è buona dal fatto che gli utenti la considerano. Tuttavia, ci sono molte distorsioni associate a questo metodo.

Per altri task, non si potrebbe essere in grado di valutare direttamente le performance di un modello in produzione e bisogna affidarsi ad un monitoraggio approfondito per rilevare i cambiamenti e i guasti nelle performance del sistema.

### Baseline
Quando si valuta un modello, è essenziale conoscere la base di riferimento (**baseline**) rispetto alla quale lo si sta valutando. Le baseline dovrebbero variare da un caso d'uso all'altro.

- **Random baseline**: le predizioni sono generate a caso seguendo una distribuzione specifica, che può essere la distribuzione uniforme o la distribuzione delle etichette del task.

- **Euristica semplice**: tale soluzione si limita a fare previsioni basate su semplici.

- **Zero rule baseline**: questo è un caso speciale di euristica semplice in cui il modello predice sempre la classe più comune.

- **Human baseline**: in molti casi, l'obiettivo del ML è automatizzare ciò che altrimenti sarebbe stato fatto dall'uomo, quindi è utile sapere come si comporta il vostro modello rispetto agli esperti umani.

- **Soluzioni esistenti**: in molti casi, i sistemi di ML sono progettati per sostituire le soluzioni esistenti. È fondamentale confrontare il nuovo modello con queste soluzioni.
	- Il modello *non deve sempre essere migliore* delle soluzioni esistenti per essere utile. Un modello le cui performance sono leggermente inferiori *può comunque essere utile se è molto più semplice o più economico da usare*.


### Test di perturbazione
Idealmente, gli input utilizzati per sviluppare un modello dovrebbero essere simili a quelli con cui il modello dovrà lavorare in produzione, ma in molti casi non è
possibile.

Questo è particolarmente vero quando la raccolta dei dati è costosa o difficile e i migliori dati disponibili per l'addestramento sono ancora molto diversi dai dati reali.

Gli input con cui i modelli devono lavorare in produzione sono spesso rumorosi rispetto a quelli utilizzati durante lo sviluppo.
Il modello che funziona meglio sui dati di addestramento non è necessariamente il modello che funziona meglio sui dati che presentano rumore.

Per avere un'idea di come il modello potrebbe funzionare con dati che presentano rumore, si possono apportare piccole modifiche agli split di test per vedere come queste modifiche influenzano le performance del modello. Pertanto, si potrebbe scegliere il modello che funziona meglio sui dati perturbati invece di quello che funziona meglio sui dati puliti.

*Più il modello è sensibile al rumore, più sarà difficile manutenerlo*, poiché se i comportamenti degli utenti cambiano di poco le performance del modello potrebbero degradarsi. Inoltre, rende il modello suscettibile ad attacchi di avversari.

### Test di invarianza
Alcune modifiche agli input non dovrebbero portare a cambiamenti nell'output Se ciò accade, il modello presenta delle *distorsioni* che potrebbero renderlo inutilizzabile, indipendentemente dalle sue performance.

Per evitare queste distorsioni, una soluzione non consiste nel *mantenere gli input invariati ma cambiare le informazioni sensibili* per vedere se i risultati cambiano.
Oppure si dovrebbero escludere le informazioni sensibili dalle feature utilizzate per addestrare il modello.

### Test di aspettativa direzionale

Alcune modifiche agli input, tuttavia, dovrebbero causare cambiamenti prevedibili negli output.

Ad esempio, quando si sviluppa un modello per prevedere i prezzi delle abitazioni, mantenendo tutte le feature invariate ma aumentando le dimensioni del lotto non dovrebbe diminuire il prezzo previsto, mentre diminuendo la metratura non dovrebbe aumentarlo.

Se i risultati cambiano nella direzione opposta a quella prevista, è possibile che il modello non stia imparando la cosa giusta e che sia necessario indagare ulteriormente prima di distribuirlo.

## Calibrazione del modello
La **calibrazione del modello** è un concetto sottile ma cruciale da comprendere.

Immaginiamo che qualcuno faccia una previsione che qualcosa accadrà con una probabilità del 70%. Questa previsione significa che, tra tutte le volte che viene fatta, il risultato previsto corrisponde a quello effettivo: il 70% delle volte.

Se un modello prevede che la squadra A batterà la squadra B con una probabilità del 70% e su 1000 volte che queste due squadre giocano contro, la squadra A vince solo il 60% delle volte, allora diciamo che questo modello *non è calibrato*.

Un modello calibrato dovrebbe prevedere che la squadra A vinca con una probabilità del 60%.

Per *misurare la calibrazione* di un modello, un metodo semplice è il **conteggio**: si conta il numero di volte in cui il modello emette la probabilità X e la frequenza Y che la predizione di avvera e si traccia il grafico di X rispetto a Y.

Il grafico di un modello perfettamente calibrato avrà X uguale a Y in tutti i punti dati.

![[Pasted image 20240105123457.png|500]]

### Misura della confidenza
La **misura della confidenza** può essere vista come un modo per *considerare la soglia di utilità* di ogni singola predizione.

Mostrare indiscriminatamente agli utenti tutte le predizioni di un modello, anche quelle su cui il modello non è sicuro, può, nel migliore dei casi, causare fastidio e *far perdere agli utenti la fiducia nel sistema*, come nel caso di un sistema di rilevamento dell'attività sullo smartwatch che pensa che si stia correndo anche se si sta solo camminando un po' velocemente.

Nel peggiore dei casi, può causare conseguenze catastrofiche, come nel caso di un algoritmo che segnala alla polizia una persona innocente come potenziale criminale.

Mentre la maggior parte delle altre metriche misurano le performance del sistema in media, la misurazione della confidenza è una metrica applicata ad ogni singolo campione.

La misurazione a livello di sistema è utile per avere un'idea delle performance complessive, ma le metriche a livello di campione sono fondamentali quando ci si preoccupa delle performance del sistema su ogni singolo campione.

### Valutazione Slice-based
In tale valutazione i dati vengono separati in sottoinsiemi e si esaminano le performance del modello su ciascun sottoinsieme separatamente.

Un errore comune è quello di concentrarsi su metriche a grana grossa, come la F1 complessiva o l'accuratezza sull'insieme dei dati, e non abbastanza su metriche slice based.

Questo può portare a due problemi:
1. Il modello potrebbe *comportarsi in modo diverso* su diversi sottoinsiemi di dati quando invece dovrebbe agire allo stesso modo.
2. Il modello potrebbe *comportarsi allo stesso modo* su diversi sottogruppi di dati, quando invece dovrebbe agire in modo diverso.

Un motivo affascinante e apparentemente controintuitivo per cui la valutazione slice based è fondamentale è **il paradosso di Simpson**, un fenomeno in cui una tendenza appare in diversi gruppi di dati ma scompare o si inverte quando i gruppi vengono combinati.

Per monitorare le performance del modello sui sottogruppi critici, occorre innanzitutto sapere quali sono i sottogruppi critici nei dati.

Ecco i tre approcci principali:
- **Basato sull'euristica**: suddividere i dati in base alla conoscenza che avete dei dati e del task da svolgere;
- **Analisi degli errori**: esaminare manualmente gli esempi classificati erroneamente e trovare pattern significativi;
- **Slice finder**: il processo inizia generalmente con la generazione di sottogruppi candidati tramite algoritmi come il beam search o il clustering, e poi procede eliminando i candidati sbagliati nei sottogruppi e riassegnandoli.


___
[[000 Indice ML|↖ Ritorna all'indice ↖]]

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