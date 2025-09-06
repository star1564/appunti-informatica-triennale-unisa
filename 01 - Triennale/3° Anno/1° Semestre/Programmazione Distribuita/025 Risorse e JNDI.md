---
aliases: JNDI
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Enterprise Edition
cssclasses:
  - 
---
In un'applicazione distribuita, i [[020 Java Enterprise Edition#Componenti JEE|componenti]] devono accedere a componenti e/o risorse, compresi i database. 

>La piattaforma Java EE utilizza il servizio di denominazione **Java Naming and Directory Interface** (*JNDI*) per aiutare i componenti a trovarsi l'un l'altro e le risorse all'interno di un application server. Infatti JNDI *può restituire un riferimento* ad un oggetto già esistente o ne *può creare uno nuovo* se non esiste.

>Le **Risorse** sono dei program object che *forniscono connessioni a sistemi* come i server di database e i sistemi di messaggistica. Sono identificate da *nomi unici* e facili da usare, chiamati *nomi JNDI*. 

Ad esempio, il nome JNDI della risorsa [[026 JDBC|JDBC]] preconfigurata associata al database Java DB in [[1 GlassFish Help - Setup|GlassFish Server]] è `java:comp/DefaultDataSource`.

Gli amministratori stabiliscono queste risorse all'interno di uno *spazio dei nomi JNDI*.

Quando una applicazione accede ad una risorsa, *il server gestisce l'invocazione dell'API JNDI*, sollevando l'applicazione da questa responsabilità. Tuttavia, le applicazioni possono anche individuare direttamente le risorse invocando l'API JNDI.

## Resource Injection
La **Resource Injection** consente di *[[024 Dipendenze e Tipi di Iniezioni#Dependency Injection|iniettare]] qualsiasi risorsa disponibile* nello spazio dei nomi JNDI in qualsiasi oggetto gestito dal contenitore, come una [[013 Servlet|Servlet]], un [[041 Enterprise Java Beans|Enterprise Java Bean]] o un [[026 Managed Beans - CDI Bean|Managed Bean]].

Ad esempio, è possibile utilizzare l'iniezione di risorse per iniettare data sources, connettori o risorse personalizzate disponibili nello spazio dei nomi JNDI.

Una applicazione può accedere ad una risorsa con il Resource Injection attraverso una [[Annotazioni|Annotazione]]. Può "decorare" campi, metodi e classi.

> [!example]+ <font color="orange">Esempio</font>
>Questo codice inietta in `myDb` un oggetto data source che fornisce connessioni al database di default di Java con GlassFish Server:
>
>![[Pasted image 20231021170725.png|850]]


___
[[000 Indice PD|↖ Ritorna all'indice ↖]]

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