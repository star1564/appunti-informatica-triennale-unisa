---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Web Services
cssclasses:
  - 
---
A livello di servizio, il sistema è definito in termini di [[034 XML|XML]], operazioni [[054 WSDL|WSDL]] e messaggi [[055 SOAP|SOAP]]. Invece, al livello di Java, ci sono oggetti, interfacce e metodi.
Quindi occorre un modo per *convertire gli oggetti Java in XML e viceversa*.

>**JAXB** (*Java Architecture for XML Binding*) fornisce delle annotazioni per *eseguire il parsing da un oggetto ad XML e viceversa* .

L'oggetto Java che deve essere inviato o ricevuto va annotato con `@javax.xml.bind.annotation.XmlRootElement` e qualche altra annotazione di mappatura (ad esempio `@XmlAttribute`) se è necessario personalizzare la mappatura. JAXB lo trasformerà da XML a Java e viceversa. 

> [!example]- <font color="orange">Esempio Mapping</font>
>```Java
>@XmlRootElement
>public class CreditCard {
>	@XmlAttribute(required = true)
>	private String number;
>	
>	@XmlAttribute(name = "expiry_date", required = true)
>	private String expiryDate;
>	
>	@XmlAttribute(name = "control_number", required = true)
>	private Integer controlNumber;
>	
>	@XmlAttribute(required = true)
>	private String type;
>	
>	// Constructors, getters, setters
>}
>```

Le specifiche di JAX-WS definiscono due tipi di annotazioni:
- Mapping WSDL: `@WebMethod`, `@WebResult`, `@WebParam`, `@OneWay`.
- Binding SOAP: `@SOAPBinding`.

## Annotazioni di Mapping WSDL
Queste annotazioni si trovano in `javax.jws` e *permettono di cambiare il mapping WSDL/Java* e di *personalizzare la firma dei metodi esposti*.

### @WebMethod
Per impostazione predefinita, *tutti i metodi pubblici di un servizio Web SOAP sono esposti nel WSDL* e utilizzano tutte le regole di mappatura *predefinite*. 

Per personalizzare alcuni elementi di questa mappatura, è possibile applicare l'annotazione ***`@javax.jws.WebMethod`*** ai metodi. L'API di questa annotazione è abbastanza semplice e consente di **rinominare un metodo** o di **escluderlo dal WSDL**.

Equivale a specificare i campi [[054 WSDL#^641900|message nell'XML del WSDL]].

> [!example]- <font color="orange">Esempio</font>
>```Java
>@WebService
>public class CardValidator {
>	@WebMethod(operationName = "ValidateCreditCard")
>	public boolean validate(CreditCard creditCard) {
>		// Business logic 
>		// Metodo PUBBLICO con nome "ValidateCreditCard"
>	}
>	
>	@WebMethod(operationName = "ValidateCreditCardNumber")
>	public void validate(String creditCardNumber) {
>		// Business logic 
>		// Metodo PUBBLICO con nome "ValidateCreditCardNumber"
>	}
>	
>	@WebMethod(exclude = true)
>	public void validate(Long creditCardNumber) {
>		// Business logic 
>		// Metodo NON INCLUSO NEL WS
>	}
>}
>```
>
>Il servizio web `CardValidator` rinomina i primi due metodi ed esclude l'ultimo.
>
>Equivale al seguente XML:
>```xml
><message name="ValidateCreditCard">
>	<part name="parameters" element="tns:ValidateCreditCard"/>
></message>
><!-- Non considerarlo adesso
><message name="ValidateCreditCardResponse">
>	<part name="parameters" element="tns:ValidateCreditCardResponse"/>
></message>
>-->
><message name="ValidateCreditCardNumber">
>	<part name="parameters" element="tns:ValidateCreditCardNumber"/>
></message>
><message name="ValidateCreditCardNumberResponse">
>	<part name="parameters" element="tns:ValidateCreditCardNumberResponse"/>
></message>
><portType name="CardValidator">
>	<operation name="ValidateCreditCard">
>		<input message="tns:ValidateCreditCard"/>
>		<!-- Non considerarlo adesso
>		<output message="tns:ValidateCreditCardResponse"/>
>		-->
>	</operation>
>	<operation name="ValidateCreditCardNumber">
>		<input message="tns:ValidateCreditCardNumber"/>
>		<!-- Non considerarlo adesso
>		<output message="tns:ValidateCreditCardNumberResponse"/>
>		-->
>	</operation>
></portType>
>```

### @WebResult
L'annotazione ***`@javax.jws.WebResult`*** controlla il **nome del valore restituito** dal messaggio nel WSDL.

Normalmente il nome del valore restituito è *`return`*.

> [!example]- <font color="orange">Esempio</font>
>```Java
>@WebService
>public class CardValidator {
>	@WebResult(name = "IsValid")
>	public boolean validate(CreditCard creditCard) {
>		// Business logic
>	}
>}
>```
>
>```xml
><!-- Default -->
><xs:element name="return" type="xs:boolean"/>
><!-- Renamed to IsValid -->
><xs:element name="IsValid" type="xs:boolean"/>
>```
>
>Il risultato restituito dal metodo `validate()` è rinominato in `IsValid`.

### @WebParam
L'annotazione ***`@javax.jws.WebParam`*** è simile a `@WebResult`, in quanto personalizza i **parametri dei metodi** del servizio web. La sua API consente di modificare il nome del parametro nel WSDL, lo spazio dei nomi e il tipo. 

I tipi validi sono `IN`, `OUT` o `INOUT` (entrambi), che determinano il modo in cui il parametro scorre (l'impostazione predefinita è `IN`).

> [!example]- <font color="orange">Esempio</font>
>```Java
>@WebService
>public class CardValidator {
>	public boolean validate(@WebParam(name= "Credit-Card", mode = IN) CreditCard creditCard) {
>		// Business logic
>	}
>}
>```
>
>```xml
><!-- Default -->
><xs:element name="arg0" type="tns:creditCard" minOccurs="0"/>
><!-- Renamed to Credit-Card -->
><xs:element name="Credit-Card" type="tns:creditCard" minOccurs="0"/>
>```

### @OneWay
L'annotazione ***`@OneWay`*** può essere utilizzata per i *metodi che non hanno un valore di ritorno*, come i metodi che restituiscono `void`.
Questa annotazione non ha elementi e può essere vista come un'interfaccia di markup che informa il contenitore che l'invocazione può essere ottimizzata, dato che non c'è un ritorno (usando un'invocazione asincrona, per esempio).

## Annotazione di Binding SOAP - @SOAPBinding
Questa annotazioni si trovana in `javax.jws.soap` e permettone la personalizzazione del binding SOAP.

Il **Binding** descrive come il Web Service è limitato ad un protocollo di messaggistica (SOAP). Esistono due tipi di stili di programmazione che definiscono il SOAP Binding e che differiscono solamente per il contenuto del ***`<soap:Body>`***:
- **Document** (default): il messaggio contiene un documento *senza regole di formattazioni* ulteriori;
- **RPC**: il SOAP *contiene un elemento con il nome del metodo* (o la [[006 Remote Procedure Calls|procedura remota]]) che è stato *invocato*.

È inoltre possibile scegliere fra due tipi di [[025 Serializzazione|Serializzazione/Deserializzazione]]:
- **Literal**: i dati vengono serializzati in accordo allo schema XML;
- **Encoded**: la codifica del SOAP specifica come gli oggetti o collection devono essere serializzati.

Questo ci porta ad avere 4 possibili tipologie di configurazioni:
- *Document/Literal* (default);
- Document/Encoded;
- RPC/Literal;
- RPC/Encoded.

> [!example]- <font color="orange">Esempio</font>
>```Java
>@WebService
>@SOAPBinding(style = RPC, use = LITERAL)
>public class CardValidator {
>	public boolean validate(CreditCard creditCard) {
>		// Business logic
>	}
>}
>```
>
>Questo va a sovrascrivere il binding default nell'XML del WSDL.
>```xml
><!-- Document style -->
><message name="validate">
>	<part name="parameters" element="tns:validate"/>
></message>
><message name="validateResponse">
>	<part name="parameters" element="tns:validateResponse"/>
></message>
>...
><soap:binding transport="http://schemas.xmlsoap.org/soap/http" style="document"/>
>
><!-- RPC Style -->
><message name="validate">
>	<part name="arg0" type="tns:creditCard"/>
></message>
><message name="validateResponse">
>	<part name="return" type="xsd:boolean"/>
></message>
>...
><soap:binding transport="http://schemas.xmlsoap.org/soap/http" style="rpc"/>
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