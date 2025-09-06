---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Spring
cssclasses:
  - 
---
>**Spring Cloud** fornisce strumenti per consentire agli sviluppatori di *sviluppare e gestire agilmente alcuni dei pattern comuni nei sistemi distribuiti*.

Per esempio: service discovery, API gateway, circuit breaker, lock globali, sessioni distribuite, ecc.

La coordinazione dei sistemi distribuiti può portare a implementazioni ripetitive.
Con Spring Cloud è possibile ottenere soluzioni a problematiche comuni con effort minore.

### API Gateway
>L'**API Gateway** è il *singolo punto d'ingresso* per tutti i client ai dati o ai servizi back-end. Gestisce tutte le *attività di accettazione ed elaborazione* relative alle chiamate API.

![[Pasted image 20231223164026.png|600]]



### Service Discovery
Un'applicazione distribuita è eseguita in ambienti dove *il numero di istanze dei servizi e la loro locazione cambiano dinamicamente*. Risulta quindi necessario un meccanismo che permetta ai client di un servizio di inviare richieste ad un insieme di istanze di servizi che cambia dinamicamente.

La **Service Discovery** permette a client, API gateway e altri servizi di *scoprire la locazione delle istanze di un servizio*.

Distinguiamo:
- *client-side* service discovery
- *server-side* service discovery

#### Service Registry
Il **Service Registry** è un *database dei servizi*, delle loro *istanze* e delle loro *locazioni*.

Viene interrogato per individuare le locazioni delle istanze dei servizi, che *si registrano* durante la fase di avvio e *rimuovono la propria registrazione* durante la fase di
spegnimento.

#### Client-side Service Discovery
Quando il client effettua una chiamata ad un servizio, interroga il service registry per *ottenere le locazioni delle istanze del servizio*.

**Il client esegue load balancing** ed inoltra la richiesta ad un'*istanza disponibile* del servizio.

![[Pasted image 20231223164648.png|600]]

#### Server-side Service Discovery
Quando il client effettua una chiamata ad un servizio, *invia una richiesta ad un load balancer* che è in esecuzione ad una locazione ben nota.

**Il load balancer interroga il service registry** e inoltra la richiesta ad un'*istanza disponibile* del servizio.

![[Pasted image 20231223164818.png|600]]

## Spring Cloud Netflix
In Spring Cloud Netflix, tramite annotazioni è possibile *abilitare e configurare rapidamente pattern comuni* all'interno delle applicazioni in *ambiente distribuito*.
Particolarmente utile per lo sviluppo di applicazioni basate su architetture orientate ai micro-servizi.

Si possono trovare i seguenti sistemi:
- **Zuul**: un *API gateway* a livello 7 che offre funzionalità per routing dinamico, monitoraggio, resilienza, sicurezza, ecc.
- **Eureka**: un servizio utilizzato per *service discovery*, bilanciamento del carico e il failover.
- **Hystrix**: è una soluzione di *circuit breaking* progettata per isolare punti di accesso in caso di fallimento *per fermare il fallimento a cascata di servizi dipendenti* dal punto di accesso.

Ad esempio, permette di creare un Server Eureka che gestisca il service registry e poi far si che le applicazioni si registrino a questo server.

### Creare un server Eureka

> [!tip]+ Dipendenze Maven in `pom.xml`
> Modulo: [spring-cloud-starter-netflix-eureka-server](https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-eureka-server).
>```xml
><dependency>
>	<groupId>org.springframework.cloud</groupId>
>	<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
>	<version></version>
></dependency>
>```

Configurare le proprietà necessarie nel file `application.yml` (alternativa ad `application.properties`):

```yml
server:
	port: 8761
eureka:
	client:
		registerWithEureka: false
		fetchRegistry: false
```

Annotare la classe di partenza del progetto Spring Boot con `@EnableEurekaServer`:
```Java
@SpringBootApplication
@EnableEurekaServer
public class MyEurekaServerApplication {
	public static void main(String[] args){
		SpringApplication.run(MyEurekaServerApplication.class, args);
	}
}
```

Accedere alla dashboard di Eureka:

![[Pasted image 20231223165544.png|800]]

### Creare un client Eureka

> [!tip]+ Dipendenze Maven in `pom.xml`
> Modulo: [spring-cloud-starter-netflix-eureka-client](https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-eureka-client).
>```xml
><dependency>
>	<groupId>org.springframework.cloud</groupId>
>	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
>	<version></version>
></dependency>
>```

![[Pasted image 20231223165831.png|600]]

Configurare le proprietà necessarie nel file `application.yml`:

```yml
spring:
	application:
		name: spring-cloud-eureka-client
eureka:
	client:
		serviceUrl:
			defaultZone: ${EUREKA_URI:http://localhost:8761/eureka}
```

Annotare la classe di partenza del progetto Spring Boot con `@EnableEurekaClient`:
```Java
@SpringBootApplication
@EnableEurekaClient
public class EurekaClientApplication {
	public static void main(String[] args){
		SpringApplication.run(EurekaClientApplication.class, args);
	}
}
```

Visualizzare i client registrati ad Eureka, andando nuovamente sulla dashboard:

![[Pasted image 20231223170107.png|900]]

### Utilizzare Eureka per la service discovery
Un bean `EurekaClient` permette di accedere al service registry per ottenere le informazioni sulle applicazioni connesse.

Si utilizza il metodo `getApplication()` per ottenere il riferimento all'applicazione.

Con il metodo `getInstances()` si accede alle istanze dell'applicazione.

Dalle istanze possiamo ottenere host e porta per le chiamate HTTP, il tutto conoscendo inizialmente solo il nome dell'applicazione da chiamare, ovvero `spring-cloud-eureka-client`.

```Java
@Autowired
private EurekaClient eurekaClient;

public String getSpringCloudEurekaClient() {
	InstanceInfo service = eurekaClient
		.getApplication("spring-cloud-eureka-client")
		.getInstances()
		.get(0);

	String hostName = service.getHostName();
	int port = service.getPort();
	//...
}
```

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