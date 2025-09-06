---
aliases:
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Web Services
cssclasses:
  -
---
>**WSDL** è il *linguaggio di definizione delle interfacce* che definisce le interazioni tra i consumatori e i [[053 Web Service|servizi web]]. È fondamentale per un servizio web, poiché *descrive i vari dati necessari alla comunicazione*, come il tipo di messaggio, la [[032 Porte|Porta]], il [[Protocollo]] di comunicazione, le *operazioni supportate*, la posizione e ciò che il consumatore deve aspettarsi in ritorno. 

Si può pensare al WSDL come ad un'[[018 Interfacce|interfaccia Java]], ma scritta in [[034 XML|XML]].

![[Pasted image 20231123181511.png|600]]


| Elemento    | Descrizione                                                                                                                                                                                            |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `definitions` | È l'elemento radice di WSDL e specifica le dichiarazioni globali dei namespace visibili in tutto il documento.                                                                                         |
| `types`       | Definisce i tipi di dati da utilizzare nei messaggi. In questo esempio, è uno schema XML che descrive i parametri passati alla richiesta del servizio Web e la risposta. |
| `message`     | Definisce il formato dei dati trasmessi tra un consumatore di servizi Web e il servizio stesso. Qui hai la richiesta (il metodo `SimpleRequest`) e la risposta (`SimpleResponse`).                                                                                  |
| `portType`    | Specifica le operazioni del servizio Web (il metodo `SimpleRequest`). Ogni operazione si riferisce a un messaggio di input e output.                                                                             |
| `binding`     | Descrive il protocollo concreto (qui `SOAP`) e i formati dati per le operazioni e i messaggi definiti per un determinato tipo di porta.                                                                                |
| `service`     | Contiene una raccolta di elementi `<port>`, dove ogni porta è associata a un endpoint (un indirizzo di rete o URL).                                                                                    |
| `port`        | Specifica un indirizzo per un binding, definendo così un singolo punto di comunicazione.                                                                                                               |

^641900

È possibile anche inserire l'elemento `<xsd:import namespace>` all'interno di `types` per riferirsi ad uno schema XML che descrive i tipi. Deve essere disponibile in rete per i consumatori del WSDL.


> [!tip]- XML WSDL
> > [!warning] Non so se questo XML è corretto
> 
>```xml
><?xml version="1.0" encoding="UTF-8"?>
><definitions
>    name="SimpleService"
>    targetNamespace="http://www.example.com/SimpleService"
>    xmlns="http://schemas.xmlsoap.org/wsdl/"
>    xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/"
>    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
>    xmlns:tns="http://www.example.com/SimpleService">
>
>    <!-- Definizione di tipi di dati -->
>    <types>
>        <xsd:schema targetNamespace="http://www.example.com/SimpleService">
>            <xsd:element name="InputParameter" type="xsd:string"/>
>            <xsd:element name="OutputParameter" type="xsd:string"/>
>        </xsd:schema>
>    </types>
>
>    <!-- Definizione di un messaggio -->
>    <message name="SimpleRequest">
>        <part name="input" element="tns:InputParameter"/>
>    </message>
>
>    <message name="SimpleResponse">
>        <part name="output" element="tns:OutputParameter"/>
>    </message>
>
>    <!-- Definizione di un portType (l'interfaccia del servizio) -->
>    <portType name="SimplePortType">
>        <operation name="SimpleOperation">
>            <input message="tns:SimpleRequest"/>
>            <output message="tns:SimpleResponse"/>
>        </operation>
>    </portType>
>
>    <!-- Definizione di un binding (come il servizio è accessibile) -->
>    <binding name="SimpleBinding" type="tns:SimplePortType">
>        <soap:binding style="document" transport="http://schemas.xmlsoap.org/soap/http"/>
>        <operation name="SimpleOperation">
>            <soap:operation soapAction=""/>
>            <input>
>                <soap:body use="literal"/>
>            </input>
>            <output>
>                <soap:body use="literal"/>
>            </output>
>        </operation>
>    </binding>
>
>    <!-- Definizione di un servizio -->
>    <service name="SimpleService">
>        <port name="SimplePort" binding="tns:SimpleBinding">
>            <soap:address location="http://www.example.com/SimpleService"/>
>        </port>
>    </service>
></definitions>
>
>```



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