---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Spring
cssclasses:
  - 
---
>**Spring Security** è un framework potente e altamente personalizzabile per *l'autenticazione e il controllo degli accessi*. È lo standard de facto per la sicurezza delle applicazioni basate su Spring.

> [!warning] Nuova versione post-2022
> Dal 2022 questo modulo è stato rinnovato, rimuovendo molte funzionalità precedentemente note. Pertanto molti tutorial su internet sono diventati obsoleti.
> Qui cercherò di riassumere il modo con cui si può utilizzare. Nella parte di esempio si va nei dettagli.


Come tutti i moduli Spring, la vera potenza di Spring Security si trova nella facilità con cui può essere esteso per soddisfare requisiti personalizzati.

### Utilizzo
> [!tip]+ Dipendenze Maven in `pom.xml`
> Modulo: [spring-boot-starter-security](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-security).
>```xml
><dependency>
>	<groupId>org.springframework.boot</groupId>
>	<artifactId>spring-boot-starter-security</artifactId>
></dependency>
>```

Abilitare Spring Security creando una classe con le annotazione `@Configuration` e `@EnableWebSecurity`:

```Java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig {
	
	@Bean
	public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
		http.authorizeHttpRequests((requests) -> requests
					.requestMatchers("/", "/home").permitAll() // Permetti a tutti di accedere a questi path
					.anyRequest().authenticated()
			)
			.formLogin((form) -> form
					.loginPage("/login").permitAll() // Specifica una pagina di login personalizzata
			)
			.logout((logout) -> logout.permitAll()); // Permetti a tutti di fare il logout
			
		return http.build();
	}
}
```

Il bean `SecurityFilterChain` definisce quali path URL devono richiedere un autenticazione e quali no:
- `/` e `/home` sono configurati per non richiedere una autenticazione;
- altri path invece sono protetti e richiedono una autenticazione.

È specificata una pagina personalizzata per fare il login (`/login`), a cui tutti possono accedere. Se non viene specificata, Spring Security ne fornisce una di default.

Quando un utente esegue il login, sono reindirizzati verso la pagina che richiedeva la loro autenticazione.

![[Pasted image 20231223163358.png|800]]

### Disabilitare la pagina di login di default
Talvolta le applicazioni non possono richiedere autenticazione manuale.

Si disabilita l'autenticazione manuale configurata di default e si implementano meccanismi “*automatici*” supportati da Spring Security, ad esempio usando *token JWT* (JSON Web Token).

```Java
@Configuration
@EnableWebSecurity
public class DefaultLoginSecurityConfig extends WebSecurityConfigurerAdapter {
	
	@Override
	protected void configure(HttpSecurity security) throws Exception {
		security.httpBasic().disable();
	}
}
```

---
> [!example]- <font color="orange">Esempio dettagliato versione post-2022</font>
>Si utilizzano alcune [[070 Librerie e Moduli utili per Spring|librerie descritte più avanti]], quindi vedere prima quelle.
>
>- **Entity: User** (attenzione, nel modulo security esiste già una classe chiamata `User`, si consiglia di rinominare la classe utente personale in qualcos'altro)
>
>```Java
>import jakarta.persistence.*;  
>import lombok.Getter;  
>import lombok.NoArgsConstructor;  
>import lombok.Setter;  
>import org.springframework.security.core.GrantedAuthority;  
>import org.springframework.security.core.authority.SimpleGrantedAuthority;  
>import org.springframework.security.core.userdetails.UserDetails;  
>  
>import java.util.Collection;  
>import java.util.Collections;  
>import java.util.UUID;  
>  
>@Getter  
>@Setter  
>@Entity  
>@NoArgsConstructor  
>@Table(name = "users")  
>public class User {  
>  
>    @Id  
>    @GeneratedValue(strategy = GenerationType.UUID)  
>    private UUID id;  
>  
>    @Column(nullable = false, unique = true, length = 45)  
>    private String email;  
>  
>    @Column(nullable = false, length = 64)  
>    private String password;  
>  
>    @Column(nullable = false, length = 20)  
>    private String firstName;  
>  
>    @Column(nullable = false, length = 20)  
>    private String lastName;  
>  
>    @Column(nullable = false)  
>    private String role;
>    // Ruolo dell'account (es. UTENTE, GESTORE, ...)
>	// ATTENZIONE!!!! I ruoli che vengono salvati nell'entità che va nel DB
>	// devono essere preceduti da "ROLE_" (es. "ROLE_UTENTE", "ROLE_UTENTE")
>
>	// In questo caso un solo utente può avere un solo ruolo
>	// Se si ha una Molti a Molti utilizzare le annotazioni appropriate
>  
>    public User(String email, String password, String firstName, String lastName, String role) {  
>       this.email = email;  
>       this.password = password;  
>       this.firstName = firstName;  
>       this.lastName = lastName;  
>       this.role = role;  
>    }  
>  
>}
>```
>
>- **UserRepository**
>```Java
>import org.springframework.data.jpa.repository.JpaRepository;  
>import org.springframework.data.jpa.repository.Query;  
>import org.springframework.stereotype.Repository;  
>  
>import java.util.UUID;  
>  
>@Repository  
>public interface UserRepository extends JpaRepository<User, UUID> {  
>    User findByEmail(String email);  
>}
>```
>
>- **UserService**
>```Java
>  
>import java.util.List;  
>  
>public interface UserService {  
>    void saveUser(User user);  
>  
>    User findByEmail(String email);  
>  
>    List<User> findAllUsers();  
>}
>```
>
>- **UserServiceImpl**
>```Java
>import org.springframework.beans.factory.annotation.Autowired;  
>import org.springframework.security.crypto.password.PasswordEncoder;  
>import org.springframework.stereotype.Service;  
>  
>import java.util.List;  
>  
>@Service  
>public class UserServiceImpl implements UserService {  
>    private UserRepository userRepository;  
>    private PasswordEncoder passwordEncoder;  // Encoder per le password da inserire nel database
>  
>    @Autowired  
>    public UserServiceImpl(UserRepository userRepository, PasswordEncoder passwordEncoder) {  
>       this.userRepository = userRepository;  
>       this.passwordEncoder = passwordEncoder;  
>    }  
>  
>    @Override  
>    public void saveUser(User user) {  
>       user.setPassword(passwordEncoder.encode(user.getPassword()));  
>       userRepository.save(user);  
>    }  
>  
>    @Override  
>    public User findByEmail(String email) {  
>       return userRepository.findByEmail(email);  
>    }  
>  
>    @Override  
>    public List<User> findAllUsers() {  
>       return userRepository.findAll();  
>    }  
>}
>```
>
>- **CustomUserDetailsService**
>```Java
>import org.springframework.beans.factory.annotation.Autowired;  
>import org.springframework.security.core.authority.SimpleGrantedAuthority;  
>import org.springframework.security.core.userdetails.UserDetails;  
>import org.springframework.security.core.userdetails.UserDetailsService;  
>import org.springframework.security.core.userdetails.UsernameNotFoundException;  
>import org.springframework.stereotype.Service;  
>  
>import java.util.Collections;  
>  
>@Service  
>public class CustomUserDetailsService implements UserDetailsService {  
>    private UserRepository userRepository;  
>  
>    @Autowired  
>    public CustomUserDetailsService(UserRepository userRepository) {  
>       this.userRepository = userRepository;  
>    }  
>  
>    @Override  
>    public UserDetails loadUserByUsername(String email) throws UsernameNotFoundException {  
>       User user = userRepository.findByEmail(email);  
>  
>       if (user != null) {  
>          return new org.springframework.security.core.userdetails.User(  
>             user.getEmail(),  
>             user.getPassword(),  
>             Collections.singleton(new SimpleGrantedAuthority(user.getRole()))  
>          );  
>       } else{  
>          throw new UsernameNotFoundException("Invalid username or password.");  
>       }  
>    }  
>}
>```
>
>- **WebSecurityConfig**
>
>```Java
>import org.springframework.beans.factory.annotation.Autowired;  
>import org.springframework.context.annotation.Bean;  
>import org.springframework.context.annotation.Configuration;  
>import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;  
>import org.springframework.security.config.annotation.web.builders.HttpSecurity;  
>import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;  
>import org.springframework.security.core.userdetails.UserDetailsService;  
>import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;  
>import org.springframework.security.crypto.password.PasswordEncoder;  
>import org.springframework.security.web.SecurityFilterChain;  
>import org.springframework.security.web.util.matcher.AntPathRequestMatcher;  
>  
>@Configuration  
>@EnableWebSecurity  
>public class WebSecurityConfig {  
>  
>    private UserDetailsService userDetailsService;  
>  
>    @Autowired  
>    public WebSecurityConfig(UserDetailsService userDetailsService) {  
>       this.userDetailsService = userDetailsService;  
>    }  
>  
>    @Bean  
>    public static PasswordEncoder passwordEncoder() {  
>       return new BCryptPasswordEncoder();  
>    }  
>  
>    @Bean  
>    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {  
>       // In questo caso, solo gli utenti possono andare sulla pagina dei prodotti
>       // ma non possono andare su quella per visualizzare gli utenti
>       // Gli admin possono accedere ai prodotti e alla visualizzazione degli utenti
>       http.csrf().disable() // disabilitato perché dà alcuni problemi di CSRF token
>          .authorizeHttpRequests((authorize) -> authorize  
>             .requestMatchers("/").permitAll()  
>             .requestMatchers("/register/**").permitAll()  
>             .requestMatchers("/users").hasRole("ADMIN")  
>             .requestMatchers("/products").hasAnyRole("USER", "ADMIN")  
>             .anyRequest().permitAll()  
>          ).formLogin((form) -> form  
>                .loginPage("/login")  
>                .loginProcessingUrl("/login")  
>                .defaultSuccessUrl("/users")  
>                .permitAll()  
>          ).logout((logout) -> logout  
>                .logoutRequestMatcher(new AntPathRequestMatcher("/logout"))  
>                .permitAll()  
>          );  
>       return http.build();  
>    }  
>  
>    @Autowired  
>    public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {  
>       auth.userDetailsService(userDetailsService)  
>          .passwordEncoder(passwordEncoder());  
>    }   
>}
>```
>
>- **AuthController**
>```Java
>import org.springframework.beans.factory.annotation.Autowired;  
>import org.springframework.stereotype.Controller;  
>import org.springframework.ui.Model;  
>import org.springframework.web.bind.annotation.GetMapping;  
>import org.springframework.web.bind.annotation.ModelAttribute;  
>import org.springframework.web.bind.annotation.PostMapping;  
>  
>import java.util.List;  
>  
>@Controller  
>public class AuthController {  
>    private UserService userService;  
>  
>    @Autowired  
>    public AuthController(UserService userService) {  
>       this.userService = userService;  
>    }  
>  
>    @GetMapping("/login")  
>    public String loginForm() {  
>       return "login";  
>    }  
>  
>    @GetMapping("/logout")  
>    public String logout() {  
>       return "logout";  
>    }  
>  
>    // handler method to handle user registration request  
>    @GetMapping("/register")  
>    public String showRegistrationForm(Model model){  
>       User user = new User();  
>       model.addAttribute("user", user);  
>       return "register";  
>    }  
>  
>    // handler method to handle register user form submit request  
>    @PostMapping("/register/save")  
>    public String registration(@ModelAttribute("user") User user, Model model){  
>       User existing = userService.findByEmail(user.getEmail());  
>  
>       if (existing != null) {  
>          model.addAttribute("error", "Email already exists");  
>          model.addAttribute("user", user);  
>          return "register";  
>       }  
>  
>       user.setRole("ROLE_USER");  
>       userService.saveUser(user);  
>       return "register-success";  
>    }  
>  
>    @GetMapping("/users")  
>    public String listRegisteredUsers(Model model){  
>       List<User> users = userService.findAllUsers();  
>       model.addAttribute("users", users);  
>       return "users";  
>    }  
>}
>```
>
>- **register.html**
>```html
><!DOCTYPE html>  
><html lang="en">  
><head>  
>    <meta charset="UTF-8">  
>    <title>Registrati</title>  
>  
>    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css"  
>          rel="stylesheet"  
>          integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN"  
>          crossorigin="anonymous">  
>  
>    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"  
>            integrity="sha384-C6RzsynM9kWDrMNeT87bh95OGNyZPhcTNXj1NW7RuBCsyN/o0jlpcV8Qyq46cDfL"  
>            crossorigin="anonymous"></script>  
></head>  
><body data-bs-theme="dark">  
><div class="container text-center">  
>    <div class="container mt-5">  
>        <h2 class="mb-4">Registrazione</h2>  
>        <form method="post" role="form" th:action="@{/register/save}" th:object="${user}">  
>            <div class="mb-3">  
>                <label for="firstName" class="form-label">Nome</label>  
>                <input type="text" class="form-control" id="firstName" placeholder="Inserisci il tuo nome" th:field="*{firstName}">  
>                <p th:errors="*{firstName}" class="text-danger" th:if="${#fields.hasErrors('firstName')}"></p>  
>            </div>  
>            <div class="mb-3">  
>                <label for="lastName" class="form-label">Cognome</label>  
>                <input type="text" class="form-control" id="lastName" placeholder="Inserisci il tuo cognome" th:field="*{lastName}">  
>                <p th:errors="*{lastName}" class="text-danger" th:if="${#fields.hasErrors('firstName')}"></p>  
>            </div>  
>            <div class="mb-3">  
>                <label for="email" class="form-label">Email</label>  
>                <input type="email" class="form-control" id="email" placeholder="Inserisci la tua email" th:field="*{email}">  
>                <p th:errors="*{email}" class="text-danger" th:if="${#fields.hasErrors('email')}"></p>  
>            </div>  
>            <div class="mb-3">  
>                <label for="password" class="form-label">Password</label>  
>                <input type="password" class="form-control" id="password" placeholder="Inserisci la tua password" th:field="*{password}">  
>                <p th:errors="*{password}" class="text-danger"  
>                   th:if="${#fields.hasErrors('password')}">  
>                </p>  
>            </div>  
>            <button type="submit" class="btn btn-primary">Registrati</button>  
>        </form>  
>    </div>  
></div>  
></body>  
></html>
>```
>
>- **login.html**
>```html
><!DOCTYPE html>  
><html lang="en">  
><head>  
>    <meta charset="UTF-8">  
>    <title>Login</title>  
>  
>    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css"  
>          rel="stylesheet"  
>          integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN"  
>          crossorigin="anonymous">  
>  
>    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"  
>            integrity="sha384-C6RzsynM9kWDrMNeT87bh95OGNyZPhcTNXj1NW7RuBCsyN/o0jlpcV8Qyq46cDfL"  
>            crossorigin="anonymous"></script>  
></head>  
><body data-bs-theme="dark">  
><div class="container text-center">  
>    <div th:if="${param.error}">  
>        <div class="alert alert-danger">Invalid Email and Password.</div>  
>    </div>  
>    <div class="container mt-5">  
>        <h2 class="mb-4">Login</h2>  
>        <form method="post" th:action="@{/login}">  
>            <div class="mb-3">  
>                <label for="username" class="form-label">Email</label>  
>                <input type="email" class="form-control" id="username" name="username" placeholder="Inserisci la tua email">  
>            </div>  
>            <div class="mb-3">  
>                <label for="password" class="form-label">Password</label>  
>                <input type="password" class="form-control" id="password" name="password" placeholder="Inserisci la tua password">  
>            </div>  
>            <button type="submit" class="btn btn-primary">Accedi</button>  
>        </form>  
>    </div>  
></div>  
></body>   
></html>
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
- [Playlist con codice outdated, ma i primi due video sono buoni per capire i concetti di security](https://www.youtube.com/playlist?list=PLqq-6Pq4lTTYTEooakHchTGglSvkZAjnE)