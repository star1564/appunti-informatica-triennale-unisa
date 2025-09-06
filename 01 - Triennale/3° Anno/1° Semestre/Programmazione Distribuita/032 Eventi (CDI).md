---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Enterprise Edition
cssclasses:
  - 
---
>Gli **Eventi** in [[027 Context e Dependency Injection|CDI]] permettono la comunicazione [[Asincrono|asincrona]] tra i beans di una applicazione, senza alcuna dipendenza a tempo di compilazione.

Un bean può definire un evento, un altro lo lancia (*fire*) e un altro ancora lo gestisce. Inoltre possono essere in separati package o layer. Segue il design pattern *Observer-Observable*.

Gli **Event Producer** generano eventi utilizzando l'interfaccia `javax.enterprise.event.Event`, chiamando il metodo `fire()`, passa l'oggetto evento e non dipende dall'osservatore. 

> [!example]- <font color="orange">Esempio</font>
>Supponiamo di avere un caso in cui vogliamo notificare altri componenti dell'applicazione quando un utente si registra. Creiamo un evento per questo scopo.
>
>1. Definiamo un evento CDI:
>
>```java
>import java.io.Serializable;
>
>public class RegistrationEvent implements Serializable {
>    private static final long serialVersionUID = 1L;
>
>    private String username;
>
>    public RegistrationEvent(String username) {
>        this.username = username;
>    }
>
>    public String getUsername() {
>        return username;
>    }
>}
>```
>
>2. Creiamo un produttore di eventi CDI che pubblicherà l'evento quando si verifica un determinato evento (ad esempio, quando un utente si registra):
>
>```java
>import javax.enterprise.context.RequestScoped;
>import javax.enterprise.event.Event;
>import javax.inject.Inject;
>import javax.inject.Named;
>
>@Named
>@RequestScoped
>public class RegistrationController {
>
>    @Inject
>    private Event<RegistrationEvent> registrationEvent;
>
>    private String username;
>
>    public String register() {
>        // Effettua la registrazione dell'utente...
>
>        // Pubblica l'evento di registrazione
>        registrationEvent.fire(new RegistrationEvent(username));
>
>        // Altri passaggi di registrazione...
>        
>        return "success"; // o la pagina successiva
>    }
>
>    // Getter e setter per 'username'
>}
>```
>
>In questo esempio, la classe `RegistrationController` è un componente CDI che gestisce la registrazione degli utenti. Quando l'utente si registra tramite il metodo `register()`, viene pubblicato un evento `RegistrationEvent` con il nome utente associato.
>
>3. Ora, creiamo un osservatore che ascolterà l'evento e reagirà di conseguenza:
>
>```java
>import javax.enterprise.event.Observes;
>
>public class RegistrationObserver {
>
>    public void onRegistration(@Observes RegistrationEvent event) {
>        // Gestisce l'evento di registrazione
>        String username = event.getUsername();
>        System.out.println("New user registered: " + username);
>
>        // Altri passaggi relativi alla gestione dell'evento...
>    }
>}
>```
>
>La classe `RegistrationObserver` è un componente CDI che contiene un metodo annotato con `@Observes`. Questo metodo sarà chiamato quando un evento `RegistrationEvent` viene pubblicato.
>
>Con questo esempio, quando un utente si registra chiamando il metodo `register()` su `RegistrationController`, verrà pubblicato un evento `RegistrationEvent`. L'evento sarà ascoltato dal metodo `onRegistration` in `RegistrationObserver`, che gestirà l'evento di registrazione.

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