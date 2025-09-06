---
aliases:
  - RDT
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Data Link
cssclasses:
  - 
---
Può capitare che il mittente sia in grado di trasmettere *più velocemente* rispetto alla capacità di ricezione del destinatario. Nel caso in cui il destinatario faccia da collo di bottiglia, si inizierebbero a perdere frame. Il [[008 Livello Data Link|protocollo]] dovrà poter gestire questa situazione. 

## Reliable Data Transfer (RDT)
Alcune tecniche per la gestione del [[Flusso Trasmissivo]] sono denominate **RDT** (*Reliable Data Transfer*).

### RDT 1.0
Questa tecnica prevede una semplice trasmissione dei dati *senza alcuna tecnica di rilevazione o correzione degli errori*.  È utilizzata solo in condizioni in cui il canale di comunicazione è *perfetto e senza errori*.

### RDT 2.0-2.1
>Questa tecnica prevede l'invio di un **ACK** (*Acknowledgment*) o di un **NAK** (*Negative Acknowledgment*) dal destinatario al mittente.
>- L'ACK è una *notifica positiva* utilizzata per confermare la ricezione corretta dei dati. 
>- Il NAK è una *notifica negativa* utilizzata per comunicare al mittente di aver ricevuto un pacchetto contentente degli errori.

Sostanzialmente, tale procedura viene chiamata protocollo **stop-and-wait**. Tale protocollo prevede che il mittente, dopo aver trasmesso un pacchetto, debba aspettare un riscontro dal destinatario, quale è l'ACK o il NAK. Si nota che il traffico dati è [[Flusso Trasmissivo#SIMPLEX|simplex]], ma il canale è [[Flusso Trasmissivo#HALF-DUPLEX|half-duplex]] o [[Flusso Trasmissivo#FULL-DUPLEX|full-duplex]] perché i dati viaggiano comunque in entrambe le direzioni. 

Può, però, presentarsi un grave problema: il feedback da parte del destinatario potrebbe arrivare *corrotto*: in tal caso, il mittente non sa cosa sia accaduto al destinatario. 
Una soluzione sarebbe quella di aggiungere il [[009 Rilevazione Errori nel Data Link#CHECKSUM|checksum]] al feedback, in modo tale che nel caso arrivi alterato *si rinvii il pacchetto*. 

Tale metodo produce *pacchetti duplicati* tra mittente e destinatario. Inoltre, il destinatario non sa se il pacchetto ricevuto sia nuovo o duplicato: si aggiunge, quindi, un nuovo campo al pacchetto, il **numero di sequenza**.

### RDT 3.0 (Sliding Window)
Questa tecnica utilizza una **finestra scorrevole** (*sliding window*) per la gestione della trasmissione dei dati su canali fisici in cui si può perdere *sia il pacchetto che il feedback*. 

La finestra scorrevole permette al mittente di inviare più di un pacchetto alla volta e al destinatario di confermare la ricezione dei pacchetti ricevuti con un ACK *per un determinato tempo*. Se il mittente non riceve l'ACK o NAK entro un certo tempo, allora ritrasmette i dati. Viene quindi introdotto un **timer**.

![[Pasted image 20230324174829.png|750]]

## Tecniche con Pipeline
RDT è poco efficiente dato che utilizza il protocollo *stop-and-wait*. 

Un protocollo **con [[067 Pipeline#^150a30|pipeline]]** migliorerebbe decisamente le prestazioni, siccome il mittente trasmetterebbe anche senza feedback. 

>In pratica, il mittente trasmette senza attendere l'ACK, *mandando in buffering molteplici pacchetti* presso il ricevente che, una volta elaborato il pacchetto, manderà il feedback. 

![[Pasted image 20230425095937.png|700]]

Tale tecnica con pipeline è implementata dai protocolli a [[#RDT 3.0|finestra scorrevole]]: permettono di inviare $K$ frame prima di fermarsi ad aspettare un riscontro ($K$ stabilito e fisso, con $K \leq 2^n – 1$, dove $n$ è il numero di bit che rappresenta il massimo numero di sequenza). 

Poiché in ricezione arrivano molteplici frame, essi devono essere numerati in modo da capire quali vengano persi. In trasmissione devono essere memorizzati i frame inviati in attesa del feedback, in modo da poterli rinviare in caso di necessità. 

Ad ogni feedback ricevuto, viene liberato il buffer associato al frame a cui il feedback fa riferimento. Anche in ricezione si deve disporre di un buffer, in modo da memorizzare i frame fuori sequenza. I frame di cui è stato mandato il feedback passano al livello di rete e il relativo buffer viene liberato.

![[Pasted image 20230412112602.png|600]]

Precisamente, si usa una "finestra" *per contenere i frame da inviare*. La dimensione iniziale della finestra è pari a $K$.

- **Mittente**: 
	- Per ogni *frame inviato*, la dimensione della finestra diminuisce di uno (spostando il limite sinistro verso destra), fino a quando $K$ diventa 0. Una volta chiusa, cioè quando sono stati inviati tutti i $K$ frame, la trasmissione si ferma.
	- Per ogni *frame riscontrato* (che ha quindi ricevuto l'ACK dal destinatario), la dimensione della finestra aumenta di uno (spostando il limite destro verso destra), permettendo la trasmissione di nuovi frame.
	- Conserva tutti i frame di cui non si è ricevuto l'ACK, in caso sia necessario re-inviarlo.
- **Destinatario** (procedimento simile al mittente):
	- Per ogni *frame ricevuto*, la dimensione della finestra diminuisce di uno (spostando il limite sinistro verso destra).
	- Per ogni *frame per cui è stato mandato un riscontro*, la dimensione della finestra aumenta di uno (spostando il limite destro verso destra).

Esistono due protocolli che utilizzano la tecnica a finestra scorrevole: Go-Back-N e selective reject.

### Go-Back-N
> Nel **Go-Back-N**, il mittente invia dei pacchetti numerati in sequenza e attende l'acknowledgement (ACK) da parte del destinatario *per ogni pacchetto inviato*. Se il mittente non riceve l'ACK entro un certo intervallo di tempo, re-invia tutti i pacchetti successivi a partire da quello non confermato.

Il destinatario riceve i pacchetti in ordine e risponde con un ACK per ogni pacchetto ricevuto correttamente. Se il destinatario riceve un pacchetto non corretto, lo scarta e non invia l'ACK sia per il pacchetto errato sia per i successivi dopo l'errore. Quindi c'è un time out e *tutti i pacchetti di cui non si è ricevuto l'ack* vengono re-inviati.

> [!success] ‎Video consigliati
> [Video Consigliato](https://youtu.be/QD3oCelHJ20?t=383)
> [Video Consigliato (esempio pacchetti mancanti)](https://www.youtube.com/watch?v=9BuaeEjIeQI)

Il vantaggio principale riguarda il buffering, il quale non viene utilizzato, siccome non c'è bisogno di memorizzare alcun pacchetto fuori ordine. L'unica cosa da conservare è il numero di sequenza del prossimo pacchetto da spedire.

### Selective Reject
> Nel **Selective Reject** (o Selective Repeat), il destinatario *invia riscontri specifici per tutti i pacchetti ricevuti correttamente*, quindi il mittente ritrasmette solamente i pacchetti per il quale *ha ricevuto un NAK*. 

Anche qui è presente un timer per i pacchetti non riscontrati. In questo modo si riduce drasticamente il numero di pacchetti ritrasmessi.

> [!success] ‎[Video Consigliato](https://youtu.be/WfIhQ3o2xow?t=149)

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