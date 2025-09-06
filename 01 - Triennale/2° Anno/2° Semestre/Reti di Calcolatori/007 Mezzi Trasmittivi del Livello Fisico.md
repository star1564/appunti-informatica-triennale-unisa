---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Fisico
cssclasses:
  - 
---
>I **Mezzi Trasmissivi** sono i *mezzi con cui vengono trasportati dati ed informazioni*. 

Ogni mezzo trasmissivo è caratterizzato dalla *banda*, dal *delay*, dal *costo* e dalla *facilità di installazione e manutenzione*.  

Purtroppo, un mezzo trasmissivo ideale, cioè che non riceva il minimo disturbo o distorsione, non esiste. 
Piuttosto esistono i mezzi trasmissivi ideali, i quali sono caratterizzati da bassa dissipazione e bassa esposizione ai disturbi: con tali mezzi, quasi tutta la potenza trasferita viene ricevuta dal ricevitore ed il segnale non viene distorto.

## Trasmissione Wired
I mezzi **Trasmissivi Wired** (elettrici) sono attualmente quelli più utilizzati, in particolare nelle [[LAN]]: questo perché tali mezzi rendono *sicura* la trasmissione dell'energia da un estremo all'altro con il minimo disturbo. 

Però, tali mezzi sono soggetti anche a disturbi elettromagnetici, ai quali ultimamente si sta prestando particolare attenzione. L'utilizzo delle schermature ed una corretta messa a terra riduce drasticamente i disturbi elettromagnetici, migliorando notevolmente le caratteristiche del cavo. 

Le schermature più utilizzate nelle LAN sono fogli di alluminio e trecciole di fili di rame che avvolgono il cavo.
### Doppino
Il **doppino** è il classico mezzo trasmissivo della telefonia e consiste in *due fili di rame attorcigliati tra loro ricoperti da una guaina isolante*. 
Tale guaina, detta *binatura*, riduce i disturbi elettromagnetici, in particolare quando si utilizzano cavi con più coppie.

![[Pasted image 20230316125034.png|300]]

Nati come mezzo trasmissivo con banda molto ridotta, attualmente i doppini hanno raggiunto elevate prestazioni grazie ai nuovi materiali isolanti utilizzati.
### Cavo Coassiale
Il **Cavo Coassiale**, oggigiorno, è stato sostituito dai [[#DOPPINO|doppini]] e dalla [[#FIBRA OTTICA|fibra ottica]]. Viene utilizzato solamente nelle reti geografiche ([[WAN]]). Il cavo coassiale a banda base consiste di *un filo di rame rigido circondato da una garza metallica che funge da schermo*. 

![[Pasted image 20230316125501.png|650]]

La banda di tale cavo dipende dalla lunghezza del cavo: più è corto, più è possibile aumentare la velocità di trasmissione.
Il cavo coassiale a banda larga, invece, usa la trasmissione analogica, quindi molto simile alla trasmissione televisiva.

### Trasmissione Power-Line
La **Trasmissione Power-Line** è una tecnologia di trasmissione dati che *utilizza la rete di alimentazione elettrica come mezzo trasmissivo*: si sovrappone alla corrente elettrica una frequenza più elevata su cui è stata modulata l'informazione da trasmettere.

![[Pasted image 20230316125526.png|650]]

### Fibra Ottica
La **Fibra Ottica** è un cavo composto da un'anima trasparente di silicio puro (*core*) avvolta in un rivestimento di silicio puro con indice di rifrazione diverso (*cladding*). 

![[Pasted image 20230316125554.png|600]]

La parte in silicio è ricoperta da una guaina di plastica nera. È importante sapere che core e cladding devono avere indici di rifrazione diversi, in particolare nel cladding dovrà essere minore rispetto all'indice nel core.

Il **Core** è il nucleo centrale *in cui viaggia la luce*, mentre il **Cladding** è il suo rivestimento.

La luce entra nel core ad un certo angolo e si propaga mediante una serie di riflessioni generate nell'impatto con il cladding. Normalmente, molteplici fibre ottiche sono raggruppate insieme intorno ad un filo di metallo che facilita la posa del cavo. 

> [!success] Vantaggi
> La fibra presenta molteplici vantaggi, quali sono l'immunità ai disturbi, banda decisamente alta (quindi velocità trasmissiva notevolmente elevata) e costo relativamente basso. 

> [!failure] Svantaggi
> Gli svantaggi, invece, riguardano la dispersione del segnale e la difficoltà di interfacciamento.


![[Pasted image 20230316125822.png|550]]

La trasmissione all'interno del core (propagazione della luce) può avvenire in due modalità diverse: 
- *Fibra monomodale*: è una fibra nella quale la luce viaggia in maniera diretta senza riflessioni, quindi non è presente dispersione modale.  
- *Fibra multimodale*: è una fibra il cui nucleo è abbastanza ampio da poter permettere diversi angoli di rimbalzo della luce trasmessa. Essa è divisa in due tipologie:
	- Fibra multimodale *step-index*: la variazione dell'indice di rifrazione tra core e cladding è talmente elevata da causare molta dispersione modale (ritardo della luce);
	- Fibra multimodale *graded-index*: la variazione dell'indice di rifrazione tra core e cladding rallenta i raggi più centrali;  

Parte della luce che si propaga lungo la fibra viene assorbita dal materiale o si diffonde in esso, costituendo una perdita del segnale trasmesso. Minore è l'attenuazione, maggiore è la distanza di trasmissione. Riguardo, quindi, i segnali ottici, si utilizza la [[006 Livello Fisico#WDM (Wave Division Multiplexing)|multiplazione WDM]]. 

Se le distanze coperte dal segnale multiplato sono notevolmente grandi, può essere necessario rigenerare e risincronizzare il segnale. 
Bisogna sapere che quando si trasmette su fibra è necessario effettuare due conversioni: 
- in trasmissione, da elettrico a luminoso; 
- in ricezione, da luminoso ad elettrico.

## Trasmissione Wireless
L'aria è un buon mezzo di trasmissione siccome risulta semplice generare le onde radio: possono viaggiare per lunghe distanze e penetrano facilmente negli edifici. Ogni trasmissione via etere (aria) deve poter utilizzare due stazioni, quali sono trasmittente e ricevente. La trasmissione è resa possibile grazie alle antenne.
### Radiodiffuzione
La **Radiodiffusione** viene generalmente utilizzata per la trasmissione analogica in broadcast ed utilizza due tecniche trasmissive in base alla regione di frequenze:
- Nella regione fino al MHz, il segnale segue la curvatura terrestre superando bene gli ostacoli;
- Nella regione dal MHz fino al GHz, il segnale viene assorbito dalla superficie della terra ma viene riflesso molto bene dalla ionosfera.

### Trasmissione via Ponte Radio
La **Trasmissione via ponte radio** si utilizza per grandi frequenze (1-40 GHz) ed instaura una comunicazione ottica rettilinea punto a punto tra sorgente e destinazione: ciò significa che sorgente e destinazione devono essere allineati e ben visibili tra loro. 
Utilizzando diverse stazioni ripetitrici, è possibile raggiungere elevate distanze. 
Di solito, le connessioni a breve distanza utilizzano frequenze più alte, antenne più piccole e sono soggette a minori interferenze.
### Trasmissioni Satellitari
Il **Satellite** si comporta come una normale stazione ripetitrice. Il segnale viene mandato dalla stazione terrestre al satellite, il quale lo rimanda alla stazione di destinazione sulla terra. 
Il satellite opera su molteplici bande di frequenza, utilizzando la tecnologia [[006 Livello Fisico#FDM (Frequency Division Multiplexing)|FDM]], gestendo molteplici comunicazioni contemporaneamente. Le bande utilizzate si aggirano tra 1 e 10 GHz.


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