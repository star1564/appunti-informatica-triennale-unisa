---
aliases:
  - Spring Beans
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Spring
cssclasses:
  - 
---
## Spring IoC Container
>Lo **Spring IoC Container** è una componente cruciale del [[063 Spring e Spring Boot#^8f128a|modulo core]]. È responsabile per *la creazione, la configurazione e la gestione* dell'intero [[044 Ciclo di vita di un Session Bean|ciclo di vita degli oggetti gestiti dal container]]. Questi oggetti vengono chiamati **Spring Beans** e sono gestiti tramite la [[024 Dipendenze e Tipi di Iniezioni#Dependency Injection|Dependency Injection]].

Il container riceve istruzioni su quali oggetti istanziare, configurare e gestire tramite i metadati di configurazione forniti.
I metadati di configurazione possono essere rappresentati tramite XML, annotazioni o codice.

## Spring Stereotype
Spring fornisce annotazioni speciali usate per *creare automaticamente i bean* nel contesto dell'applicazione.

L'annotazione ***`@Component`*** rappresenta lo [[005 UML#Stereotipi|stereotype]] principale e generico per *definire uno Spring Bean*.  ^5fda70

```Java
import org.springframework.stereotype.Component;

@Component
public class GreetingLogger {
	public void log() {
		System.out.println("Sata andagi!");
	}
}
```

Da questo, derivano altre annotazioni come: 
- `@Service`, classe che *contiene la [[Logica di business]]*; 
- `@Repository`, classe che si occupa di *operazioni CRUD*. Viene di solito usata con implementazioni di classi DAO;
- `@Controller`, classe responsabile di *gestire le richieste degli utenti e restituire una risposta appropriata*, può essere anche in [[006 Basi di HTML|HTML]];
- `@RestController`, classe che si occupa solo di *API backend*, inviando solamente risposte in formati come il [[036 JSON con AJAX#JSON|JSON]].

Le ultime due vengono usate in [[065 Spring Web MVC|Spring Web MVC]].

Una delle annotazioni più importanti in Spring è ***`@Configuration`***. Questa indica che la classe ha dei metodi annotati con **`@Bean`** e che quindi *definiscono come creare particolari Spring Beans* necessari all'applicazione.

Spring, al momento della creazione del bean della classe `@Configuration`, genererà i Spring Bean definiti dai metodi `@Bean`, che potranno essere usati tramite dependency injection.

### Scope dei Spring Bean
Di **default** i bean sono *gestiti come [[042 Tipi di Session Beans#Singleton Beans|Singleton]]*, ma possono essere configurati per far si che ne venga generata una nuova istanza per ogni richiesta oppure per ogni sessione.

Il modo in cui il container gestisce il ciclo di vita del bean, definisce lo **scope del bean**.



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