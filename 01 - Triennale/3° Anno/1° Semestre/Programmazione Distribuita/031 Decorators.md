---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Enterprise Edition
cssclasses:
  - 
---
>Un **Decorator** consente di *aggiungere nuove funzionalità a un oggetto esistente* in modo dinamico, senza alterare la sua struttura di base.

L'idea è quella di prendere una classe e *avvolgerne un'altra classe intorno a essa* (cioè, decorarla). In questo modo, quando si chiama una classe decorata, si passa sempre attraverso il decoratore circostante prima di raggiungere la classe di destinazione. 

I decoratori hanno lo scopo di aggiungere logica aggiuntiva a un metodo [[Logica di business|business]]. Non sono in grado di risolvere problemi tecnici che riguardano molti tipi diversi. Gli [[030 Interceptor|interceptor]] e i decorator, sebbene simili sotto molti aspetti, *sono opposti*.

I decorator devono avere un *punto di [[024 Dipendenze e Tipi di Iniezioni#Dependency Injection|iniezione]] delegato* (annotato con **`@Delegate`**), con lo stesso tipo dei bean che decorano. Ciò consente al decorator di invocare l'oggetto delegato (cioè il bean di destinazione) e quindi di invocare qualsiasi metodo di business su di esso. 

Se un'applicazione ha sia interceptor che decorator, *gli interceptor vengono invocati per primi*.

> [!warning] Aggiornare `beans.xml`
> Per impostazione predefinita, tutti i decorator sono disabilitati. È necessario abilitarli nel `beans.xml` nella cartella `WEB-INF`.
>```xml
><beans xmlns="http://xmlns.jcp.org/xml/ns/javaee"
>       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
>       xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/beans_1_1.xsd"
>       version="1.1" bean-discovery-mode="all">
>
>    <decorators>
>        <class>percorso.del.package.nomeClasseDecorator</class>
>    </decorators>
></beans>
>```

> [!example]- <font color="orange">Esempio</font>
>Supponiamo di avere un servizio che fornisce un saluto e vogliamo creare un decoratore per aggiungere una formattazione specifica al saluto.
>
>1. Definiamo l'interfaccia del servizio:
>```java
>public interface GreetingService {
>    String greet(String name);
>}
>```
>
>2. Implementiamo una classe di servizio di base:
>```java
>public class BasicGreetingService implements GreetingService {
>    @Override
>    public String greet(String name) {
>        return "Hello, " + name + "!";
>    }
>}
>```
>
>3. Creiamo un decoratore che aggiunge la formattazione:
>```java
>import javax.decorator.Decorator;
>import javax.decorator.Delegate;
>import javax.inject.Inject;
>
>@Decorator
>public class FormattedGreetingService implements GreetingService {
>
>    @Inject
>    @Delegate
>    private GreetingService greetingService;
>
>    @Override
>    public String greet(String name) {
>        // Aggiungi la formattazione al saluto di base
>        String formattedName = name.toUpperCase();
>        return greetingService.greet(formattedName);
>    }
>}
>```
>
>In questo esempio, `FormattedGreetingService` è un decoratore che estende l'interfaccia `GreetingService` e annota la classe con `@Decorator`. Il campo `@Inject @Delegate` viene utilizzato per iniettare l'implementazione di base (`BasicGreetingService`). Il metodo `greet` nel decoratore aggiunge la formattazione specifica (in questo caso, converte il nome in maiuscolo) al saluto di base.
>
>![[Pasted image 20231114114746.jpg|400]]

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