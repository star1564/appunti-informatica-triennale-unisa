---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Spring
cssclasses:
  - 
---
## Thymeleaf
<b>[Thymeleaf](https://www.thymeleaf.org/)</b> è un *template engine* per Java per elaborare e gestire [[006 Basi di HTML|HTML]], [[034 XML|XML]], [[028 Basi per JavaScript|JavaScript]] e [[024 CSS|CSS]].

Per aggiungerlo ad un progetto Spring Boot usare la dipendenza:

> [!tip]+ Dipendenze Maven in `pom.xml`
> Modulo: [spring-boot-starter-thymeleaf](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-thymeleaf).
>```xml
><dependency>
>	<groupId>org.springframework.boot</groupId>
>	<artifactId>spring-boot-starter-thymeleaf</artifactId>
></dependency>
>```

Permette anche di *far ritornare documenti HTML* ad un [[065 Spring Web MVC|controller]].
Creare una directory `templates` nella directory `src/main/resources` del progetto, dove aggiungere le pagine HTML create.

Thymeleaf fornisce degli *attributi identici* a quelli dei tag HTML ma che **hanno il prefisso `th:`** e che permettono di mostrare valori “*dinamici*”.

Possiamo fornire i parametri a questi tag utilizzando un oggetto `Model` iniettato nel metodo del controller. I parametri vengono passati tramite un sistema di *coppie-valore*.

> [!example]- <font color="orange">Esempio</font>
>Creiamo un file `userDetails.html` nella directory `src/main/resources/templates`.
>
>```html
><!-- Pagina: .../user-details -->
><!-- File: userDetails.html -->
>
><!DOCTYPE html>
><html lang="en">
>	<head>
>		<meta charset="UTF-8">
>		<title>User Details</title>
>	</head>
>	<body>
>		<h1 th:text="'Welcome ' + ${user.name}"/>
>	</body>
></html>
>```
>
>`th:text` è usato per parametrizzare il testo da mostrare nell'elemento `<h1>`.
>
>Creiamo un `@Controller` per fornire dati alla pagina.
>La stringa restituita dal controller *deve corrispondere al nome del file HTML*. Usiamo un oggetto `Model` iniettato nel metodo del controller per passare i parametri alla pagina tramite coppie chiave-valore.
>
>```Java
>@Controller
>public class UserDetailsController {
>	private final UserDetailsService userDetailsService;
>	
>	@Autowired
>	public UserDetailsController(UserDetailsService userDetailsService) {
>		this.userDetailsService = userDetailsService;
>	}
>	
>	@GetMapping("/user-details")
>	public String getUserDetails(@RequestParam("id") UUID id, Model model)
>		User user = userDetailsService.findById(id);
>		model.addAttribute("user", user);
>		return "userDetails";
>	}
>}
>```

> [!success] [Cheat Sheet](http://engma.github.io/thymeleaf-cheat-sheet/)

## Lombok

> [!quote] Lo raccomando assolutamente per progetti con Maven, rende scrivere le classi più semplice!


<b>[Lombok](https://projectlombok.org/)</b> è una libreria per l'*autogenerazione di boilerplate tramite annotazioni*. Ad esempio `@Getter` per generare metodi getter per tutti i campi di una classe, oppure `@Slf4j` (simple logging facade for java) che ottiene una interfaccia di logging senza averla configurata esplicitamente.

> [!tip]+ Dipendenze Maven in `pom.xml`
> Modulo: [lombok](https://mvnrepository.com/artifact/org.projectlombok/lombok).
>```xml
><dependency>
>	<groupId>org.projectlombok</groupId>
>	<artifactId>lombok</artifactId>
></dependency>
>```

> [!example]- <font color="orange">Esempio</font>
>```Java
>@NoArgsConstructor    /* <- Costruttore senza argomenti (vuoto) */
>@AllArgsConstructor   /* <- Costruttore con tutti gli argomenti (pieno) */
>@Getter               /* <- Getter per tutte le variabili */
>@Setter               /* <- Setter per tutte le variabili */
>@ToString             /* <- toString */
>@Entity               
>public class User {
>	@Id
>	@GeneratedValue(strategy = GenerationType.UUID)
>	private UUID ID;
>	private String name;
>	private String username;
>	private String username;
>}
>```
>
>Si possono anche inserire sopra i campi specifici, invece sopra la classe.

## ModelMapper
<b>[ModelMapper](http://modelmapper.org/)</b> è una libreria per la *conversione tra oggetti di due classi Java* per ridurre il boilerplate.

> [!tip]+ Dipendenze Maven in `pom.xml`
> Modulo: [modelmapper](https://mvnrepository.com/artifact/org.modelmapper/modelmapper).
>```xml
><dependency>
>	<groupId>org.modelmapper</groupId>
>	<artifactId>modelmapper</artifactId>
></dependency>
>```

> [!example]- <font color="orange">Esempio</font>
>Sviluppiamo una classe `UserMapper` che usa `ModelMapper` per convertire le istanze di una classe `UserEntity` in delle istanze di una classe `UserDTO`.
>
>```Java
>public class UserMapper {
>	public UserDTO map(UserEntity user) {
>		Model mapper = new ModelMapper();
>		return mapper.map(user, UserDTO.class);
>	}
>}
>```

## Jackson
<b>[Jackson](https://github.com/FasterXML/jackson)</b> è una libreria per *serializzare e deserializzare JSON* in Java (esiste anche per XML).

> [!tip]+ Dipendenze Maven in `pom.xml`
> Modulo: [jackson-databind](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind).
>```xml
><dependency>
>	<groupId>com.fasterxml.jackson.core</groupId>
>	<artifactId>jackson-databind</artifactId>
></dependency>
>```

> [!example]- <font color="orange">Esempio</font>
>Sviluppiamo una classe `UserSerializer` che usa Jackson per serializzare in JSON le istanze di una classe `UserDTO`.
>
>```Java
>public class UserSerializer {
>	public String serialize(UserDTO user) {
>		ObjectMapper objectMapper = new ObjectMapper();
>		return objectMapper.writeValueAsString(user);
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