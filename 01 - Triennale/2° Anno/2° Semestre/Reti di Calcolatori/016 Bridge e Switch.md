---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Data Link
cssclasses:
  - 
---
## Bridge
> Un **Bridge** è un dispositivo che collega due o più segmenti di rete (solitamente [[LAN]]), consentendo ai dati di *passare da un segmento all'altro*. 
> Opera a [[008 Livello Data Link|livello data link]]. 

Quando un pacchetto di dati arriva ad un bridge, il bridge controlla l'[[MAC (Indirizzo e Sottolivello)#Indirizzo MAC|indirizzo MAC]] di destinazione del pacchetto e decide se inoltrarlo al segmento di rete appropriato o meno. In questo modo, il bridge è in grado di isolare il traffico in modo da evitare che si propaghi in segmenti della rete non necessari, migliorando la sicurezza e la prestazione della rete.

![[Pasted image 20230425153852.png]]

Quindi il bridge "*unisce*" le reti che collega, formando una LAN più grande con un unico dominio di [[Interferenza e Collisione#Collisione|collisione]].

Osserva in modo promiscuo il traffico delle LAN a cui è connesso, costruendo una tabella chiamata **MAC Address Table** che *associa ogni indirizzo MAC alla porta corrispondente del bridge* ([[#Backward learning|backward learning]]).

![[Pasted image 20230425154348.png|900]]

## Switch
> Uno **Switch** è un dispositivo che connette due o più segmenti di rete e funziona allo stesso modo del [[#Bridge|bridge]]. Uno switch è un dispositivo di rete più sofisticato rispetto al bridge, poiché è in grado di suddividere e indirizzare il traffico di rete in modo *molto più efficiente*. 
> Opera a [[008 Livello Data Link|livello data link]]. 

Essenzialmente, uno switch permette la comunicazione tra reti diverse tenendole *costantemente separate*, con domini di collisione separati e diversi, confinando i malfunzionamenti dovuti a stazioni difettose.

![[Immagine 2023-04-25 160202 1.png]]

Quando un pacchetto di dati arriva ad uno switch, lo switch controlla l'indirizzo MAC (Media Access Control) di destinazione del pacchetto e decide a quale porta di uscita inoltrare il pacchetto.

Anche gli switch usano la MAC Address Table per capire chi è il *reale destinatario* ([[#Backward learning|backward learning]]).

Uno switch è composto da:
- **Memoria Condivisa**: nella quale vengono memorizzati i pacchetti, i quali verranno poi mandati alla porta di destinazione;
- **Fabric**: dispositivo che recapita le trame in input verso una porta di output;
- **Porte**: apertura fisica in cui è possibile collegare un cavo che connette fisicamente le LAN allo switch;
- **Architettura Bus**: ha un [[Bus]] interno condiviso ad alta velocità che utilizza [[012 Protocolli a suddivisione del canale#TDMA|TDMA]].

> [!error] Quando vengono collegati due o più switch a vicenda, si potrebbero verificare [[017 Switching Loop|loop]]
> In caso di loop si devono fisicamente staccare e riattaccare i cavi.

## Backward learning
Procedimento per il **Backward Learning**:
1. Al boot le tabelle sono *vuote*;
2. Viene usato l'indirizzo di provenienza per definire la *posizione (porta) del mittente* nella tabella;
3. Se un pacchetto ha una *destinazione sconosciuta*, viene emesso su *tutte le porte* eccetto quella di provenienza;
4. Si registra nella tabella la porta da cui si è ricevuto il riscontro positivo della ricezione del pacchetto.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20230425161230.png|(1)]]
>1. Inizializzazione, la tabella è vuota.
>
>![[Pasted image 20230425161334.png|(2)]]
>
>2. La stazione A invia una trama alla stazione C:
>	- Lo switch memorizza l'associazione fra il MAC address della stazione A e la porta E0 osservando l'inizio della sorgente delle trame.
>	- La trama della stazione A alla stazione C è inviata su tutte le porte tranne la porta E0 (flooding verso gli indirizzi unicast non noti).
>
>![[Pasted image 20230425161615.png|(3)]]
>
>3. La stazione D invia una trama alla stazione C:
>	- Lo switch memorizza l'associazione fra il MAC address della stazione D e la porta E3 osservando l'inizio della sorgente delle trame.
>	- La trama della stazione D alla stazione C è inviata su tutte le porte tranne la porta E3 (flooding verso gli indirizzi unicast non noti).
>
>4. Eventualmente si scopriranno tutte le associazioni nella MAC address table.
>
>![[Pasted image 20230425161850.png|(5)]]
>
>5. La stazione D invia una trama alla stazione C:
>	- La destinazione è nota, la trama non viene inviata a tutti (nessun flooding).

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
- [Switching (Professor Messer)](https://youtu.be/ND_wGVid51M)