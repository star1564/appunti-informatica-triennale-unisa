---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Spring
cssclasses:
  - 
---
## Spring
><b>[Spring](https://spring.io/)</b> è considerato il più popolare tra i *framework* per lo sviluppo di applicazioni Java Enterprise. Ha l'obiettivo di *gestire la complessità* relativa allo sviluppo di applicazioni enterprise in Java.

Il framework è progettato con una **struttura modulare**.

![[Pasted image 20231223124010.png|600]]

- Il modulo **Core** fornisce le funzionalità fondamentali, come: [[024 Dipendenze e Tipi di Iniezioni#Inversion of Control|Inversion of Control]], [[024 Dipendenze e Tipi di Iniezioni#Dependency Injection|Dependency Injection]], application context (come un registro [[025 Risorse e JNDI|JNDI]]), ecc. ^8f128a
	- Questo modulo *è sempre necessario*, invece gli altri sono opzionali.
- Il modulo di **Data Access** fornisce interazione con sorgenti dati (database, cache, ecc.), astraendo tecnologie come [[026 JDBC|JDBC]], [[049 Java Message Service|JMS]], ecc.
- Il modulo **Web** fornisce le funzionalità necessarie alle applicazioni Web, come gestione [[004 Richiesta e Risposta HTTP|richieste e risposte]].
- I moduli **AOP** e **Instrumentation** offrono funzionalità trasversali, come [[030 Interceptor|interceptor]], logging, ecc.
- Il modulo **Test** fornisce funzionalità di [[035 CVS - Testing|Testing]] integrate con gli altri moduli.

Tutte le dipendenze necessarie ad un'app (come moduli, librerie, ecc.) vengono gestite tramite software di build automation e dependency management, come Maven.

### Apache Maven
>**Apache Maven** è uno *strumento di gestione* dei progetti software. Basato sul concetto di un **Project Object Model** (*POM*), Maven può gestire la build e documentazione di un progetto in maniera centralizzata.

Un POM è *l'unità fondamentale* di lavoro in Maven. È un file [[034 XML|XML]] chiamato `pom.xml` che *contiene informazioni sul progetto* e dettagli di configurazione utilizzati da Maven per il processo di build. Contiene ad esempio le dipendenze del progetto, reperibili da [Maven Central Repository](https://mvnrepository.com/repos/central).

> [!warning] Vulnerabilità delle dipendenze vecchie
> Bisogna fare attenzione alle *versioni* di ogni dipendenza presa dalla central repository, perché in versioni vecchie possono essere trovate delle vulnerabilità, risolte dalle versioni nuove.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <version>0.0.1</version>
    <name>project-name</name>
    <description>project-description</description>

    <dependencies>
        <!-- Aggiungi le dipendenze del tuo progetto qui -->
        <!-- Esempio: -->
        <dependency>
	        <groupId>org.springframework.boot</groupId>
	        <artifactId>spring-boot-starter-web</artifactId>
	        <version>3.2.0</version>
        </dependency>
    </dependencies>
</project>
```

## Spring Boot
**Spring Boot** semplifica la creazione di applicazioni Spring-based, richiedendo una configurazione minima. Vantaggi:
- Riduzione del tempo necessario per la configurazione, ad esempio *eliminando le configurazioni XML*;
- Una soluzione opinionata ma che possa essere facilmente estesa e personalizzata;
- Funzionalità comuni a tutte le app, come sicurezza, health-check, configurazione esternalizzata, ecc.

Spring Boot fornisce anche un **[[011 Web Dinamico|Web Server]]** integrato, in questo modo non è necessario dover configurare un web server in cui deployare l'app, ma basterà avviare
l'app e il web server sarà avviato automaticamente con l'applicazione deployata al suo interno.

Di default, il web server è [[012 Apache Tomcat|Tomcat]].

### Creazione Progetto
Tramite <b>[Spring Initializer](https://start.spring.io)</b> è possibile generare un progetto Spring Boot con pochi click. È integrato nella maggior parte degli IDE, ma è anche disponibile online.

Le dipendenze si gestiscono come per Spring, ad esempio con un file `pom.xml` nel caso di Maven. È possibile selezionare un pool di dipendenze di partenza al momento della creazione del progetto.

![[Pasted image 20231223131422.png|900]]

### Struttura Progetto
Un progetto Spring Boot ha sempre:
- Un file `pom.xml`;
- Una directory `src/main/java` in cui scrivere il codice dell'applicazione;
- Una directory `src/main/resources` in cui aggiungere risorse statiche o configurazioni;
- Una directory `src/test` in cui scrivere i test per il codice dell'applicazione;
- Una classe `<Nome>Application.java` contente lo starting point dell'applicazione.

![[Pasted image 20231223131632.png|300]]

```Java
@SpringBootApplication
public class PdtifyApplication {
	public static void main(String[] args){
		SpringApplication.run(PdtifyApplication.class, args);
	}
}
```

### Configurare proprietà dell'applicazione
Un'applicazione necessita spesso di proprietà il cui valore non può essere configurato automaticamente da Spring Boot, come ad esempio la stringa di connessione per un database.

Queste proprietà possono essere specificate all'interno del file `application.properties` o del file `application.yml`.

*Properties*:
```properties
server.port = 8080
logging.level.root = INFO
database.url = jdbc:mysql://mysql.db.server:3306/my_database
```

*YAML*:
```yaml
server:
	port: 8080
logging:
	level:
		root: INFO
database:
	url: "jdbc:mysql://mysql.db.server:3306/my_database"
```

> [!info] [Stringhe in Yaml](https://stackoverflow.com/a/22235064/23087941)

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
- https://github.com/tizianocitro/pdtify