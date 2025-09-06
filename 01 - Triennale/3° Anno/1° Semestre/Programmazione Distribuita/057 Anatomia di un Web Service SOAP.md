---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Web Services
cssclasses:
  - 
---
> [!note] Approcci alla scrittura di un WS
>Ci sono due approcci per scrivere un Web Service con Java.
>- **Top-down Approach** (*contract first*) comincia con la scrittura del contratto [[054 WSDL|WSDL]], definendo le operazioni, i messaggi, ecc. Quando entrambi i consumer ed il provider si mettono d'accordo sul contratto, si possono implementare le classi Java basandosi su quel contratto.
>- **Bottom-up Approach**, l'implementazione delle classi Java esiste già, tutto quello che si deve fare è creare il contratto WSDL basandosi su di esse.

Come le [[034 Entità JPA|entità]] o gli [[041 Enterprise Java Beans|EJB]], un servizio Web [[055 SOAP|SOAP]] utilizza un [[023 POJO|POJO]] annotato con la politica [[035 Object Relational Mapping#Configuration by Exception|Configuration by Exception]] e che deve essere deployato in un [[011 Web Dinamico|Web Container]].

Il runtime **JAX-WS** genererà tutte le operazioni che trasformeranno l'invocazione di un metodo Java in una chiamata XML su [[003 HTTP|HTTP]]. 

La classe del WS può essere sia una classe Java normale che un EJB.

> [!info] I requisiti di un Web Service SOAP
> - La classe deve essere annotata con ***`@javax.jws.WebService`***
> - La classe (ovvero il bean dell'implementazione) può implementare da zero a più interfacce che devono essere annotate con `@WebService`
> - La classe deve essere `public` e non deve essere `final` o `abstract`
> - La classe deve avere un costruttore `public` di default
> - La classe NON deve definire il metodo `finalize()`
> - Un servizio deve essere un oggetto [[042 Tipi di Session Beans#Stateless Beans|stateless]] e non deve salvare stati inerenti a client specifici

### POJO o EJB
JAX-WS consente di esporre come servizi Web *sia le normali classi Java che gli EJB*. Queste sono chiamate **endpoint interface**. 

Entrambi gli endpoint hanno un comportamento quasi identico, ma l'uso degli *endpoint EJB offre alcuni vantaggi in più*. Poiché il servizio Web è anche un EJB, i vantaggi della [[046 Transazioni EJB|transazione]] e della sicurezza gestiti dal [[021 Container Java EE|container]] sono automatici e si possono usare gli [[030 Interceptor|Interceptor]], cosa che non è possibile con gli endpoint delle [[013 Servlet|Servlet]]. 

Il codice business può essere esposto come servizio web e come EJB allo stesso tempo, il che significa che la logica di business può essere esposta tramite SOAP e anche tramite [[017 Java RMI|RMI]], aggiungendo un'interfaccia remota.

### @WebService
L'annotazione ***`@javax.jws.WebService`*** contrassegna una classe o un'interfaccia Java come servizio web. Se usata direttamente sulla classe, il processore di annotazioni del contenitore genererà l'interfaccia, per cui i seguenti frammenti di codice sono equivalenti. 

Questa è l'annotazione sulla classe:
```Java
@WebService
public class CardValidator {...}
```

Qui l'annotazione è su un'interfaccia implementata da una classe. Come si può vedere, l'implementazione deve definire il nome completo dell'interfaccia nell'attributo `endpointInterface`:
```Java
@WebService
public interface Validator {...}

// -------------------------------------

@WebService(endpointInterface = "package.path.to.Validator")
public class CardValidator implements Validator {...}
```


L'annotazione `@WebService` ha una serie di attributi che *consentono di personalizzare i suoi vari aspetti che vengono rispecchiati nel file XML*, come il nome del servizio web nel file WSDL (l'elemento `<wsdl:portType>` o `<wsdl:service>`) e nel suo spazio dei nomi, nonché di modificare la posizione del WSDL stesso (l'attributo `wsdlLocation`).

```Java
@WebService(portName = "CreditCardValidator", serviceName = "ValidatorService")
public class CardValidator {...}
```

Il codice di sopra viene rispecchiato nell'XML:
```xml
<service name="ValidatorService">
	<port name="CreditCardValidator" binding="tns:CreditCardValidatorBinding">
		<soap:address location="http://localhost:8080/chapter14/ValidatorService"/>
	</port>
</service>
```

> [!example]- <font color="orange">Esempio WS</font>
>```Java
>@WebService
>@Stateless
>public class CardValidator {
>	public boolean validate(CreditCard creditCard) {
>		Character lastDigit = creditCard.getNumber().charAt(
>		creditCard.getNumber().length() - 1);
>		if (Integer.parseInt(lastDigit.toString()) % 2 != 0) {
>			return true;
>		} else {
>			return false;
>		}
>	}
>}
>```
>Questo servizio Web SOAP CardValidator ha un metodo per convalidare una carta di credito. Prende una carta di credito come parametro e restituisce true o false a seconda che la carta sia valida o meno. In questo caso, si assume che le carte di credito con un numero pari siano valide e quelle con un numero dispari no.

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