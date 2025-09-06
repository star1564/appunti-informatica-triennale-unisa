---
aliases: 
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Testing
cssclasses:
  - 
---
>L'**Integration Testing** rileva le [[035 CVS - Testing#^1cc62a|Fault]] che non sono state rilevate durante gli [[040 Unit Testing|Unit Test]] *concentrandosi su piccoli gruppi di componenti*. 
>Due o più componenti vengono *integrati e testati* e quando non vengono rilevati nuovi guasti, ulteriori componenti vengono *aggiunti* al gruppo.

Questa procedura consente di testare parti sempre più complesse del sistema *mantenendo relativamente piccola la posizione delle potenziali Fault*. Infatti, il componente aggiunto più recentemente è solitamente quello che innesca le Fault scoperte più recentemente.

Creare molti [[037 Concetti di Testing#Test Stub e Drivers|Test Stub e Drivers]] è un processo che *richiede tempo*. Tuttavia, l'**ordine in cui i componenti vengono testati** può influenzare lo sforzo totale richiesto dal test di integrazione. Un attento ordine dei componenti può *ridurre le risorse necessarie* per il testing di integrazione.

Le strategie per scegliere l'ordine sono descritte dall'Horizontal e Vertical Integration Testing. Ognuna di queste categorie è stata concepita per una *decomposizione gerarchica del sistema*, dove ogni componente appartiene a vari *layer* ordinati in base all'ordine di chiamata.

## Strategie per l'Horizontal Integration Testing
### Big Bang Integration Testing
Nella strategia del **Big Bang Testing**, ogni componente del sistema *viene prima testato individualmente*, per poi venir *testate insieme*.

![[Pasted image 20240126101113.png]]

| <font color="green">Vantaggi<font> | <font color="red">Svantaggi<font> |
| ---- | ---- |
| Non si devono creare test stub e drivers aggiuntivi | È difficile individuare lo specifico componente che ha causato un fallimento |

### Bottom-Up Integration Testing
Nella strategia del **Bottom-Up Testing**, ogni componente del *livello Inferiore è testato singolarmente* e poi *integrato con i componenti del livello successivo*. 

Questa operazione viene ripetuta fino a quando tutti i componenti di tutti i livelli sono stati combinati. I test drivers vengono utilizzati per simulare i componenti dei livelli superiori che non sono ancora stati integrati.

![[Pasted image 20240126101138.png|400]]

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240126103100.png|800]]
>
>Dopo il test unitario dei sottosistemi E, F e G, il test di integrazione bottom-up procede con il test triplo B-E-F e il test doppio D-G.

| <font color="green">Vantaggi<font> | <font color="red">Svantaggi<font> |
| ---- | ---- |
| Le Fault delle interfacce utenti sono facili da individuare | Testa l'intefaccia utente alla fine |
|  | La correzione delle Fault trovate in cima alla gerarchia potrebbero cambiare la decomposizione o le interfacce dell'intero sistema, rendendo inutili i test precedenti |

### Top-Down Integration Testing
Nella strategia del **Top-Down Testing**, ogni componente del *livello Superiore è testato singolarmente* e poi *integrato con i componenti del livello sottostante*. 

Anche in questo caso, i test aggiungono in modo incrementale *un componente alla volta*. Questa operazione viene ripetuta fino a quando tutti i livelli sono combinati e coinvolti nel test. Gli stub di test vengono utilizzati per simulare i componenti dei livelli inferiori non ancora integrati.

![[Pasted image 20240126102107.png|400]]

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240126103148.png|800]]
>
>Dopo il test unitario del sottosistema A, il test di integrazione procede con i test doppi A-B, A-C e A-D, seguiti dal test quadruplo A-B-C-D.

| <font color="green">Vantaggi<font> | <font color="red">Svantaggi<font> |
| ---- | ---- |
| Comincia il testing con l'interfaccia utente | Lo sviluppo dei test stubs richiede tempo e potrebbe produrre errori |
| Lo stesso set di test, derivato dai requisiti, può essere utilizzato per testare un insieme sempre più complesso di sottosistemi. | Richiede un grande numero di stubs per testare anche componenti non fondamentali |

### Sandwich Testing
La strategia del **Sandwich Testing** *combina le strategie top-down e bottom-up*, cercando di sfruttare il meglio di entrambe. 

Il tester divide la decomposizione del sottosistema in tre layer: 
- un layer *target*;
- un layer *sopra* il layer target;
- un layer *sotto* il layer target. 

![[Pasted image 20240126103809.png|400]]

Utilizzando il layer target come fulcro dell'attenzione, i test top-down e quelli bottom-up possono ora essere *eseguiti in parallelo*. 


| <font color="green">Vantaggi<font> | <font color="red">Svantaggi<font> |
| ---- | ---- |
| Non è necessario scrivere stub e driver di test per i livelli superiore e inferiore, <br>perché utilizzano i componenti effettivi del livello target | Le componenti vengono testate separatamente prima di integrarle |

Esiste anche la strategia **Modificata del Sandwich Testing**, che *controlla i tre layer individualmente* prima di combinarli con altri layer ^2ba926

## Strategie per il Vertical Integration Testing
Le strategie di **Vertical Integration Testing**, si concentrano sull'*integrazione anticipata*. 

Per un determinato caso d'uso, le parti necessarie di ciascun componente, come l'interfaccia utente, la [[Logica di business]], il [[005 Middleware|Middleware]] e lo storage, vengono identificate e sviluppate in parallelo, per poi essere testate a con l'integration testing. 

Un sistema costruito con questa strategia produce *candidati al rilascio*.


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