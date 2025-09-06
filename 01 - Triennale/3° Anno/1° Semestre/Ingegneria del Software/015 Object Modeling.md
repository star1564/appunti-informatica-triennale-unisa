---
aliases:
  - Object Modeling
  - Object Modeling Technique
  - OMT
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Analisi dei Requisiti - Analisi
cssclasses:
  - 
---
>L'**Object Modeling** ha lo scopo di costruire il [[014 Modello ad Oggetti|modello ad oggetti]] andando a *trovare le astrazioni più importanti*. Consente di trasformare i [[011 Scenari e Casi d_Uso#Caso d'Uso|Casi d'Uso]] in *[[014 Modello ad Oggetti#Entity, Boundary e Control object|Oggetti del Modello a Oggetti]]*. 

È diviso in diverse attività.

## 1 - Identificare gli Oggetti Entity
Per identificare gli **oggetti partecipanti** si possono osservare i casi d'uso. Ci sono varie tecniche, ma quella più usata è la Tecnica di Abbot.

### Tecnica di Abbott
Visto che il documento della [[012 Specifica dei Requisiti|specifica dei requisiti]] è scritto in un linguaggio naturale, è possibile utilizzare la **Tecnica di Abbott**, che analizza il testo per identificare oggetti, attributi ed associazioni.

Si usa la seguente tabella per individuare i tipi e le relazioni.

| Testo           | Tipo di Oggetto                                              | Esempio                                       |
| --------------- | ------------------------------------------------------------ | --------------------------------------------- |
| Nomi Propri     | *Istanza*                                                    | `Alice`                                       |
| Nomi Comuni     | *Classe*                                                     | `Agente`                                      |
| Verbi di Fare   | *Operazione*                                                 | `Crea`, `Invia`, `Seleziona`                  |
| Verbi di Essere | *[[015 Ereditarietà\|Ereditarietà]]*                         | `È un tipo di`, `Può essere uno dei seguenti` |
| Verbi di Avere  | *[[UML 5 - Diagramma di Classi#Aggregazione\|Aggregazione]]* | `Ha`, `Consiste di`, `Include`                |
| Verbi Modali    | *[[008 Requisiti#Vincoli\|Vincolo]]*                         | `Deve essere`                                 |
| Aggettivi       | *Attributi*                                                  | `Descrizione Incidente`, `Livello Emergenza`  |


> [!success] Vantaggi
> Questa tecnica si focalizza sul linguaggio dell'utente

> [!failure] Svantaggi
> - Il linguaggio naturale è spesso impreciso
> - La qualità del modello dipende fortemente dallo stile di scrittura dell'analista
> - Ci possono essere sinonimi sconvenienti

> [!tip] Euristiche - Entity
>È possibile usare, insieme alla tecnica di Abbott, le seguenti euristiche che aiutano a dare peso a certe caratteristiche:
>- Termini che gli sviluppatori o gli utenti devono chiarire per comprendere il caso d'uso.
>- Sostantivi ricorrenti nei casi d'uso (ad esempio, `Incidente`).
>- Entità del mondo reale che il sistema deve tracciare (ad esempio, `FieldOfficer`, `Dispatcher`, `Resource`).
>- Attività del mondo reale che il sistema deve tracciare (ad esempio, `EmergencyOperationsPlan`).
>- Sorgenti o pozzi di dati (ad esempio, `stampante`).

### Dare un nome alle Entity
Per ogni [[014 Modello ad Oggetti#^7cb8de|Oggetto Entity]] individuato (classe):
- Si assegna un **nome univoco** con una **breve descrizione**. Per gli oggetti entity, si consiglia di *utilizzare gli stessi nomi usati dagli utenti e dagli specialisti* del dominio applicativo.
- Si individuano gli **attributi** e le **responsabilità**.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240118134147.png]]

## 2 - Identificare gli Oggetti Boundary
In ogni caso d'uso, ogni attore dovrebbe interagire con almeno un [[014 Modello ad Oggetti#^fc2909|Oggetto Boundary]].

Questi *raccolgono informazioni dall'attore* e lo *traducono* in un formato che può essere usato dagli oggetti entity e control. 

> [!tip] Euristiche - Boundary
>Euristiche per identificare gli oggetti boundary:
>- Identificare i controlli dell'interfaccia utente di cui l'utente ha bisogno per *avviare il caso d'uso* (ad esempio, `ReportEmergencyButton`).
>- Identificare i moduli di cui gli utenti hanno bisogno per *inserire i dati nel sistema* (ad esempio, `EmergencyReportForm`).
>- Identificare gli avvisi e i messaggi che il sistema utilizza *per rispondere all'utente* (ad esempio, `AcknowledgmentNotice`).
>- Quando in un caso d'uso sono coinvolti *più attori*, identificare *i terminali degli attori* (ad esempio, `DispatcherStation`) per riferirsi all'interfaccia utente in esame.
>- *Non modellare gli aspetti visivi dell'interfaccia* con oggetti boundary (i [[004 Modelli di CVS#^920367|Mock-Up]] dell'utente sono più adatti a questo scopo).
>- Usare sempre i *termini dell'utente* finale per descrivere le interfacce; non usare termini del dominio delle soluzioni o delle implementazioni.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240118141015.png]]

## 3 - Identificare gli Oggetti Control
Un [[014 Modello ad Oggetti#^e6d6d5|Oggetto Control]] viene *creato all'inizio di un caso d'uso* e cessa di esistere al suo termine. 
È anche responsabile di *ottenere le informazioni dagli oggetti boundary* e *mandarle verso gli oggetti entity*.

> [!tip] Euristiche - Control
>Euristiche per identificare gli oggetti control:
>- Identificare un oggetto control *per ogni caso d'uso*.
>- Identificare un oggetto control *per ogni attore* del caso d'uso.
>- L'arco di vita di un oggetto di controllo deve coprire l'estensione del caso d'uso o l'estensione di una sessione utente. Se è difficile identificare l'inizio e la fine dell'attivazione di un oggetto di controllo, probabilmente il caso d'uso corrispondente non ha condizioni di ingresso e di uscita ben definite.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240118142101.png]]

## 4 - Mappare i Casi d'Uso ad Oggetti attraverso i Sequence Diagrams
Un [[UML 2 - Diagramma di Sequenza|Sequence Diagram]] mostra come il comportamento di un caso d'uso (o scenario) sia *distribuito tra tutti i suoi oggetti* partecipanti.

Questi diagrammi non dovrebbero essere usati per comunicare con gli utenti, ma *solamente tra gli sviluppatori*.

> [!tip] Alcune Euristiche - Sequence Diagrams
>Il diagramma deve avere *colonne* specifiche rappresentano gli **oggetti del modello**:
>- La prima colonna a sinistra rappresenta l'*attore* che inizia il caso d'uso;
>- La seconda colonna è l'*oggetto boundary*;
>- La terza colonna è l'*oggetto control*;
>- La quarta colonna è il *manager*, che esegue le operazioni con gli *oggetti entity*;
>- Gli oggetti control *creano altri oggetti boundary* e possono interagire con manager e altri oggetti control.
>
>Inoltre:
>- Oggetti creati durante l'interazione sono illustrati con il messaggio `<<create>>`.
>- Oggetti distrutti durante l'interazione sono evidenziati con una croce, e vengono distrutti attraverso il messaggio `<<destroy>>`.

Mediante i sequence diagram, è possibile trovare comportamenti o oggetti mancanti. *In caso manchi qualche entità*, è necessario ritornare ai casi d'uso, ridefinire le parti mancanti e tornare a questa fase per ricreare il sequence diagram.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240118144740.png]]

## 5 - Identificare le Associazioni
Da questo punto in poi si iniziano ad utilizzare i [[UML 5 - Diagramma di Classi|Class Diagram]], che permettono agli sviluppatori di descrivere le dipendenze e le relazioni tra gli oggetti.

> [!warning]- Per il progetto
> In questo class diagram (che andrà inserito nel [[017 Requirement Analysis Document|RAD]]) non si devono inserire metodi e le classi NON devono essere inerenti all'implementazione (per esempio, una classe entità che contiene le immagini non va inserito in questo diagramma, ma in quello dell'[[028 Object Design Document|ODD]]).


Una **Associazione** mostra la *relazione bilaterale tra due o più classi*. Deve avere:
- Un **nome** per descrivere l'associazione tra le classi. Non è necessario che queste siano uniche, infatti sono opzionali.
- Un **ruolo** ad ogni endpoint, una frase che identifica la *funzione della classe che partecipa*.
- La **molteplicità** ad ogni endpoint, per identificare il *numero di istanze che partecipano*.

![[Pasted image 20240118150535.png|600]]

> [!tip] Euristiche - Associazioni
>Euristiche per identificare le associazioni:
>- Esaminare le frasi dei verbi.
>- Nominare con precisione associazioni e ruoli.
>- Usare il più spesso possibile i qualificatori per identificare gli spazi dei nomi e gli attributi chiave.
>- Eliminare qualsiasi associazione che possa essere derivata da altre associazioni.
>- Non preoccupatevi della molteplicità finché l'insieme delle associazioni non è stabile.
>- Troppe associazioni rendono il modello illeggibile.

## 6 - Identificare le Aggregazioni
Le [[UML 5 - Diagramma di Classi#Aggregazione|Aggregazioni]] identificano che un oggetto *fa parte di un altro oggetto*.

Ci sono due tipi di aggregazioni:
- Una [[UML 5 - Diagramma di Classi#Composizione|composizione]];
- Una aggregazione condivisa.

![[Pasted image 20240118151238.png|750]]

## 7 - Identificare gli Attributi
Gli **attributi** sono proprietà individuali degli oggetti. Questi hanno:
- Un *nome*;
- Un *tipo*;
- Una breve descrizione.

![[Pasted image 20240118151554.png|400]]





___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

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