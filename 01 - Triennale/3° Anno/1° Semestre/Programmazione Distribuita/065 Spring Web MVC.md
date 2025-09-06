---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Spring
cssclasses:
  - 
---
>**Spring Web [[020 Stili di Architettura#Model-View-Controller (MVC)|MVC]]** è il framework web costruito sulla [[013 Servlet|Servlet]] API di Java che *fornisce tutto il necessario per sviluppare applicazioni web*, come [[053 Web Service|REST API]], backend, ecc.



> [!tip]+ Dipendenze Maven in `pom.xml`
> Modulo: [spring-boot-starter-web](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web).
>```xml
><dependency>
>	<groupId>org.springframework.boot</groupId>
>	<artifactId>spring-boot-starter-web</artifactId>
></dependency>
>```

Spring Web Consente di creare bean controller per *gestire le richieste HTTP in ingresso*, tramite le annotazioni `@Controller` o `@RestController`.

Il *mapping dei controller alle richieste HTTP* avviene utilizzando l'annotazione `@RequestMapping`, che *indica il path della richiesta che il controller deve gestire* (es. `/users`).

I metodi dei controller vengono etichettati con annotazioni che permettono di specificare il path della richiesta HTTP che il singolo metodo deve gestire: 
- `@GetMapping`, utilizza il metodo HTTP **GET**;
- `@PostMapping`, utilizza il metodo HTTP **POST**; 
- `@DeleteMapping`, utilizza il metodo HTTP **DELETE**;
- ecc...
 
Permettono anche di specificare parti del path da usare come variabili.

> [!example]- <font color="orange">Esempio</font>
>```Java
>@RestController
>@RequestMapping("/users")
>public class UserController {
>	// "/users"
>	@GetMapping("/") 
>	public User getUsers( ) {
>		// ...
>	}
>	
>	// "/users"
>	@PostMapping("/")
>	public User saveUser(/*...*/) {
>		// ...
>	}
>	
>	// "/users/delete-users"
>	@DeleteMapping("/delete-users")
>	public void deleteUsers() {
>		// ...
>	}
>}
>```
>
>Il controller `UserController` gestirà le richieste all'URL `https://my-domain/users`.
>
>Nel dettaglio:
>- Il metodo `getUsers()` gestirà le richieste GET all'URL `https://my-domain/users`;
>- Il metodo `saveUser()` gestirà le richieste POST all'URL `https://my-domain/users`;
>- Il metodo `deleteUsers()` gestirà le richieste DELETE all'URL `https://my-domain/users/delete-users`.

### Elementi delle richieste HTTP
Spring Web fornisce nel contesto dell'applicazione dei bean `HttpMessageConverters` che si *occupano di convertire in tipi primitivi e oggetti* i seguenti elementi:
- Parametri nella [[002 URL#^97d608|query string]] della richiesta: `@RequestParam`;
- Parametri nel path della richiesta: `@PathVariable`;
- [[004 Richiesta e Risposta HTTP#^5613f3|Header]] della richiesta: `@RequestHeader`;
- [[004 Richiesta e Risposta HTTP#^02e64f|Body]] della richiesta: `@RequestBody`.

> [!example]- <font color="orange">Esempio</font>
>```Java
>@RestController
>@RequestMapping("/users")
>public class UserController {
>	@GetMapping("/{userId}")
>	public User getUser(@PathVariable UUID userId) {
>		// ...
>	}
>	
>	@PostMapping("/")
>	public User saveUser(
>		@RequestHeader("retires") Integer retires,
>		@RequestBody User user
>	 ) {
>		// ...
>	}
>	
>	@DeleteMapping("/{userId}")
>	public void deleteUser(@PathVariable UUID userId) {
>		// ...
>	}
>}
>```
>
>- Il metodo `getUser()` gestirà le richieste GET all'URL `https://my-domain/users/:userId`, dove `userId` è una variabile con valore diverso in ogni richiesta;
>- Il metodo `saveUser()` gestirà le richieste POST all'URL `https://my-domain/users` e riceverà un body da mappare ad un'istanza della classe `User` e un parametro `retries` nell'header HTTP indicante il numero massimo di tentativi per effettuare il salvataggio;
>- Il metodo `deleteUser()` gestirà le richieste DELETE all'URL `https://my-domain/users/:userId`, dove `userId` è una variabile con valore diverso in ogni richiesta.

### @Service e @Autowired
Buona pratica di sviluppo prevede che *i controller si occupino solo della gestione della richieste e risposte* e che *la logica di business dell'applicazione sia gestita da dei servizi*.

La **[[024 Dipendenze e Tipi di Iniezioni#Dependency Injection|Dependency Injection]]** in Spring è dichiarata tramite l'annotazione ***`@Autowired`***. Questa si può inserire su una variabile, metodo setter o costruttore.
I [[053 Web Service|Servizi]] vengono forniti ai controller tramite dependency injection. 

Implementare un servizio prevede la definizione di un'*interfaccia* e una *classe per implementare i metodi* definiti da annotare con l'annotazione ***`@Service`***.

> [!example]- <font color="orange">Esempio</font>
>*Interfaccia*:
>```Java
>public interface UserService {
>	User getUser(UUID userId);
>	void saveUser(User user);
>	void deleteUser(UUID userId);
>}
>```
>
>*Implementazione*:
>```Java
>@Service
>public class UserServiceImpl implements UserService {
>	@Override
>	public User getUser(UUID userId) {
>		//...
>	}
>	
>	@Override
>	public void saveUser(User user) {
>		//...
>	}
>	
>	@Override
>	public void deleteUser(UUID userId) {
>		//...
>	}
>}
>```
>
>*Controller*:
>```Java
>@RestController
>@RequestMapping("/users")
>public class UserController {
>	private UserService userService;
>	
>	// Costruttore che esegue la dependency injection
>	@Autowired
>	public UserController(UserService userService) {
>		this.userService = userService;
>	}
>	
>	@GetMapping("/{userId}")
>	public User getUser(@PathVariable UUID userId) {
>		userService.getUser(userId);
>	}
>	
>	@PostMapping("/")
>	public User saveUser(
>		@RequestHeader("retires") Integer retires,
>		@RequestBody User user
>	 ) {
>		userService.saveUser(user);
>	}
>	
>	@DeleteMapping("/{userId}")
>	public void deleteUser(@PathVariable UUID userId) {
>		userService.deleteUser(userId);
>	}
>}
>```


## Intercettare richieste HTTP
In Spring ci sono due componenti in grado di intercettare richieste HTTP:
- **[[020 Filtri Servlet|Filtri]]**: eseguiti *prima di individuare il controller* che gestirà la richiesta;
- **Handler Interceptor**: eseguiti prima, durante e dopo *la gestione della richiesta*.

![[Pasted image 20231223141426.png|800]]

Dove **Dispatcher Servlet** è la componente che si occupa di fare il forwarding delle richieste ai controller.

### Filtri in Spring
>I **Filtri** vengono *eseguiti prima della Dispatcher Servlet* e quindi prima che le richieste siano gestite dall'applicazione.

Possiamo utilizzare i filtri per *manipolare e bloccare le richieste in ingresso* prima che raggiungano i controller, o viceversa, *bloccare le risposte in uscita* prima che raggiungano il client.

Possiamo usare i filtri per innumerevoli funzionalità, tra cui:
- Autenticazione;
- Auditing;
- Compressione di immagini e dati;
- Qualsiasi funzionalità che vogliamo disaccoppiare da Spring MVC.

> [!example]- <font color="orange">Esempio</font>
>```Java
>@Component
>public class UserLoggingFilter extends OncePerRequestFilter {
>	@Override
>	protected boolean shouldNotFilter(HttpServletRequest request) {
>		String path = request.getServletPath();
>		if (path.startsWith("/users"))
>			return false; // Deve essere filtrato
>		else 
>			return true; // Non deve essere filtrato
>	}
>	
>	@Override
>	protected void doFilterInternal(
>		HttpServletRequest request,
>		HttpServletResponse response,
>		FilterChain filterChain,
>	) throws ServletException, IOException {
>		try {
>			String method = request.getMethod();
>			
>			if (method.equalsIgnoreCase("POST") ) {
>				Integer retries = Integer.parseInt(request.getHeader("retries"));
>				System.out.println("Numer of retries: " + retries);
>			}
>		} catch (NumberFormatException e) {
>			System.err.println(e.getMessage());
>		}
>		
>		filterChain.doFilter(request, response); // Prosegui con la richiesta
>	}
>}
>```
>
>Il filtro intercetta solo le richieste per il path `/users` come definito dal metodo `shouldNotFilter()`.
>
>Per ogni richiesta:
>- Ottiene il metodo HTTP utilizzato;
>- Se POST allora inserisce un log indicante il numero di tentativi per il salvataggio dell'utente;
>- Prima di terminare, indica che la richiesta può proseguire con il metodo `doFilter()`.
>
>La richiesta sarà poi gestita dal metodo `saveUser()` del controller nell'esempio precedente.

### Handler Interceptor
>Gli **Handler Interceptor** vengono *eseguiti dopo la Dispatcher Servlet* e forniscono metodi per eseguire logica:
>- Prima che le richieste siano gestite dai controller;
>- Dopo che sono state gestite dai controller ma prima che vengano reinderizzate eventuali pagine;
>- Dopo il completamento della richiesta.

Possono essere usati per funzionalità come:
- Modificare una pagina prima che sia reinderizzata;
- Controlli specifici legati all'autorizzazione;
- Manipolazione del contesto dell'applicazione.

Ci sono tre metodi principali:
- **`preHandle()`** è eseguito *prima che le richieste siano gestite* dai controller.
- **`postHandle()`** è eseguito *dopo che sono state gestite dai controller* ma *prima che vengano reindirizzate le pagine*.
	- Il parametro di tipo `ModelAndView` fornisce accesso alle pagine e al loro contenuto.
- **`afterCompletion()`** è eseguito *dopo il completamento della richiesta*.
	- Il parametro di tipo `Exception` permette di eseguire logiche condizionate dalla presenza di un errore.

> [!example]- <font color="orange">Esempio</font>
>```Java
>@Component
>public class LogInterceptor implements HandlerInterceptor {
>
>	@Override
>	public boolean preHandle(
>		HttpServletRequest request, 
>		HttpServletResponse response,
>		Object handler
>	) throws Exception {
>		System.out.println("preHandle invoked");
>		return true;
>	}
>	
>	@Override
>	public void postHandle(
>		HttpServletRequest request, 
>		HttpServletResponse response,
>		Object handler, 
>		ModelAndView modelAndView
>	) throws Exception {
>		System.out.println("postHandle invoked");
>	}
>
>	@Override
>	public void afterCompletion(
>		HttpServletRequest request, 
>		HttpServletResponse response,
>		Object handler, 
>		Exception ex
>	) throws Exception {
>		System.out.println("afterCompletion invoked");
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