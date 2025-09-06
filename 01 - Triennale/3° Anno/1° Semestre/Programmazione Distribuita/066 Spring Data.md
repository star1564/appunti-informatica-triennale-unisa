---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Spring
cssclasses:
  - 
---
>**Spring Data** fornisce un modello di programmazione Spring-based per *l'accesso ai dati*. È una famiglia di moduli che contiene numerosi sotto-moduli specifici per ogni singola tecnologia di archiviazione. Ci focalizzeremo su **Spring Data JPA**.

Semplifica l'uso di tecnologie di storage, come [[010 Modello Relazionale|database relazionali]] e [[011 Database NoSQL|non relazionali]], framework di map-reduce, servizi di dati basati su cloud, ecc.

Un modulo di base *Spring Data Commons* funge da building block per tutti gli altri moduli.

## Spring Data JPA
Semplifica l'implementazione di repository basate su [[033 Java Persistence API|JPA]] per l'accesso a database relazionali. 

Gestire l'accesso ai dati per un'applicazione può essere oneroso. Tanto codice [boilerplate](https://en.wikipedia.org/wiki/Boilerplate_code) può dover essere scritto per eseguire anche le query più semplici e se si aggiungono funzionalità come la paginazione, l'auditing e altre opzioni spesso necessarie, il boilerplate e la complessità aumentano.

Spring Data JPA mira a *migliorare l'implementazione di funzionalità tipiche per l'accesso ai dati* riducendo lo sforzo di sviluppo. Tipicamente si definisce una interfaccia per repository a cui *Spring fornirà una implementazione*.

*Spring Data JPA è un'astrazione al di sopra di Hibernate*, un [[035 Object Relational Mapping|Object Relational Mapping]] che permette di semplificare la gestione dello strato di persistenza delle applicazioni.

### Importare Spring Data JPA

Bisogna: 
1. Aggiungere il modulo `spring-boot-starter-data-jpa`.
2. Aggiungere il connector Java dello specifico database a cui ci si connetterà, per esempio MySQL.

> [!tip]+ Dipendenze Maven in `pom.xml`
> Modulo: [mysql-connector-java](https://mvnrepository.com/artifact/mysql/mysql-connector-java).
>```xml
><dependency>
>	<groupId>mysql</groupId>
>	<artifactId>mysql-connector-java</artifactId>
>	<version>8.0.33</version>
></dependency>
>```


3. Indicare il database a cui connettersi. Si utilizzano delle proprietà definite da Spring Data JPA nel file `application.properties`.

```properties
spring.datasource.url = jdbc:mysql://localhost:3306/mydb?createDatabaseIfNotExist=true
spring.datasource.username = root
spring.datasource.password = password
spring.datasource.driverClassName = com.mysql.cj.jdbc.Driver
```

### Entità
Le **Entità** in Spring Data JPA funzionano allo stesso modo delle [[034 Entità JPA|Entità in JPA]].

```Java
@Entity
public class User {
	@Id @GeneratedValue(strategy = GenerationType.UUID)
	private UUID ID;
	private String name;
	private String username;
}
```

### Repository
>Una **repository** è una [[064 Spring Stereotype#^5fda70|componente]] che *si occupa di operazioni CRUD* verso una soluzione di storage.

È, in pratica, una interfaccia contenente metodi che andranno a comunicare con il database. Spring ne fornirà l'implementazione.
La sola definizione di questa interfaccia fornirà *metodi comuni*, come:
- `findById()`;
- `findAll()`;
- `save()`;
- `saveAll()`;
- `count()`;
- ...

Si possono anche *aggiungere metodi personalizzati*.

> [!example]- <font color="orange">Esempio</font>
>```Java
>@Repository
>public interface UserRepository extends JpaRepository<User, UUID> {
>	User findByUsername(String username); // Metodo personalizzato
>}
>```
>
>Dichiarando solamente il metodo ci permette di indicare a Spring Data di fornire un'implementazione.
>
>```Java
>@Service
>public class UserServiceImpl implements UserService {
>	private UserRepository userRepository;
>	
>	@Autowired
>	public UserServiceImpl(UserRepository userRepository) {
>		this.userRepository = userRepository;
>	}
>	
>	@Override
>	public User getUser(UUID userId) {
>		userRepository.findById(userId);
>	}
>	
>	@Override
>	public void saveUser(User user) {
>		userRepository.save(user);
>	}
>	
>	@Override
>	public void deleteUser(UUID userId) {
>		userRepository.deleteById(userId);
>	}
>}
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