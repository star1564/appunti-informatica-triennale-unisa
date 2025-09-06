---
aliases:
  - JEE Beans
  - Enterprise Java Beans
  - EJB
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Enterprise Java Bean
cssclasses:
  - 
---
## Introduzione
Le [[034 Entità JPA|Entità]] possono avere metodi per convalidare i loro attributi, ma *non sono fatte per rappresentare attività complesse*, che spesso richiedono un'interazione con altri [[020 Java Enterprise Edition#Componenti JEE|componenti]] (altri oggetti persistenti, servizi esterni, ecc.). Il livello di persistenza non è il livello appropriato per l'elaborazione della logica di business. Allo stesso modo, l'interfaccia utente non dovrebbe eseguire la logica aziendale, soprattutto quando ci sono più interfacce (Web, Swing, dispositivi portatili, ecc.).

Per separare il livello di persistenza da quello di presentazione, per implementare la logica di business e per aggiungere la gestione delle transazioni e la sicurezza, le applicazioni hanno bisogno di un *Livello di Business*. In Java EE, questo livello viene implementato utilizzando gli **Enterprise JavaBeans** (*EJB*).

![[Pasted image 20231115141654.png|500]]

Il Livello di Business ha anche il compito di:
- interagire con [[053 Web Service|servizi]] esterni;
- inviare messaggi asincroni (usando [[049 Java Message Service|JMS]]);
- orchestrare componenti del DB verso sistemi esterni;
- servire il layer di presentazione.

## Definizione
> Un **Enterprise Java Bean (EJB)** è un [[020 Java Enterprise Edition#Componenti JEE|componente di JEE]] server-side che *incapsula la [[Logica di business]]* di una applicazione e si occupa delle transazioni e della sicurezza. È un oggetto [[040 Ciclo di Vita di una Entità#^f8ef5d|managed]].

### Servizi del Container
Una volta fatto il deployment, il [[021 Container Java EE|container]] offre i servizi seguenti, mentre il programmatore si concentra solo sulla logica di business:
- *Comunicazione remota*: client EJB possono [[017 Java RMI|invocare metodi remoti]] per mezzo di protocolli standard;
- *[[024 Dipendenze e Tipi di Iniezioni|Iniezione di dipendenze]]*;
- *Gestione dello stato*;
- *Pooling*: creazione di un pool di istanze che possono essere condivise da client multipli;
- *[[044 Ciclo di vita di un Session Bean|Ciclo di Vita]]*;
- *Gestione dei messaggi JMS*;
- *[[046 Transazioni EJB|Transazioni]], Sicurezza, Concorrenza*;
- *Interceptor ai metodi*;
- *Invocazione asincrona*;

### Tipi di EJB
Gli EJB possono essere di due tipi:
- **[[041 Enterprise Java Beans|Session Bean]]**, si concentra sulle logiche di business specifiche dell'applicazione; ^a4706e
- *[[049 Java Message Service#Message-Driven Beans|Message-Driven]]*, agisce come listener per un particolare tipo di messaggistica, come il Java Message Service API.

I Session Bean si dividono a loro volta in:
- **[[042 Tipi di Session Beans#Stateless Beans|Stateless Beans]]**;
- **[[042 Tipi di Session Beans#Stateful Beans|Stateful Beans]]**;
- **[[042 Tipi di Session Beans#Singleton Beans|Singleton Beans]]**.

### Anatomia di un Session EJB
Un Session Bean è semplicemente un [[023 POJO|POJO]] annotato.

```Java
@Stateless // Tipologia di EJB
public class BookEJB {
	// Iniezione della dipendenza attraverso la Persistence Unit.
	@PersistenceContext(unitName = "chapter07PU")
	private EntityManager em;

	// Metodi che implementano la logica di business.
	
	public Book findBookById(Long id) {
		return em.find(Book.class, id);
	}
	
	public Book createBook(Book book) {
		em.persist(book);
		return book;
	}
}
```

Un EJB è composto dai seguenti elementi:
- *Una classe bean*: contiene l'implementazione dei metodi di business e può implementare da zero a più interfacce di business. Il session bean deve essere annotato con `@Stateless`, `@Stateful`, o `@Singleton` in base al suo tipo.
- *Interfacce di Business*: contengono una dichiarazione dei metodi di business che sono visibili al client ed implementati dalla classe bean. Un session bean può avere interfacce locali, remote o nessuna.

![[Pasted image 20231115145527.png|600]]

> [!note] Caratteristiche della classe Bean
>- La classe deve essere annotata con `@Stateless`, `@Stateful` o `@Singleton`;
>- Deve implementare i metodi delle sue interfacce, se esistono;
>- La classe deve essere `public` e non deve essere `final` o `abstract`;
>- Il costruttore deve essere `public` e senza parametri;
>- La classe non deve definire il metodo `finalize()`;
>- I metodi di business non devono cominciare per "`ejb`" e non devono essere `final` o `static`;
>- Gli argomenti e il valore di ritorno di un metodo remoto devono essere dei tipi legali per RMI.

### Interfacce Remote, Local e No-interface Views
A seconda di *dove un client invoca un session bean*, la classe del bean dovrà implementare interfacce remote o locali, oppure nessuna interfaccia (no-interface view). 

Se l'architettura prevede client che risiedono *al di fuori dell'istanza JVM del contenitore EJB*, devono utilizzare un'**interfaccia remota**. Questo vale per i client in esecuzione in una JVM separata, in un contenitore client di applicazioni o in un contenitore web o EJB esterno.  In questo caso, i client dovranno invocare i metodi dei bean di sessione tramite [[017 Java RMI|RMI]]. 

È possibile utilizzare l'invocazione locale attraverso l'**interfaccia locale** quando il bean e il client *sono in esecuzione nella stessa JVM*. Può trattarsi di un EJB che invoca un altro EJB o di un componente web ([[013 Servlet|Servlet]], JSF) in esecuzione in un contenitore web nella stessa JVM. 

![[Pasted image 20231115150708.png|700]]

È anche possibile che l'applicazione utilizzi chiamate sia remote che locali sullo stesso session bean.

La **no-interface view** è una variante dell'interfaccia locale che *espone tutti i metodi pubblici di business della classe bean localmente* senza l'utilizzo di un'interfaccia separata. ^2aad01

Una *interfaccia di business* è una semplice [[018 Interfacce|Interfaccia]] di Java che include una delle seguenti annotazioni:
- `@Remote`: denota una interfaccia di business *remota*. I parametri dei metodi sono passati per valore e devono essere [[025 Serializzazione|serializzabili]] come descritto dal protocollo RMI.
- `@Local`: denota una interfaccia di business *locale*. I parametri dei metodi sono passati per referenza dal client al bean.

> [!example]- <font color="orange">Esempio</font>
>```Java
>@Local
>public interface ItemLocal {
>	List<Book‎ > findBooks();
>	List<CD‎ > findCDs();
>}
>
>@Remote
>public interface ItemRemote {
>	List<Book‎ > findBooks();
>	List<CD‎ > findCDs();
>	Book createBook(Book book);
>	CD createCD(CD cd);
>}
>
>@Stateless
>public class ItemEJB implements ItemLocal, ItemRemote {
>	// ...
>}
>```

### Identificare un EJB con JNDI
Automaticamente, alla creazione di un EJB viene creato un nome [[025 Risorse e JNDI|JNDI]].

```markup
java:<scope>[/<app-name>]/<module-name>/<bean-name>[!<fully-qualified-interface-name>]
```

- `<scope>` definisce una serie di namespaces standard che mappano i vari scope dell'applicazione .
	- `global`, `app`, `module`, `comp`.
- `<app-name>` richiesto solo se il bean viene packaged in file `.ear` o `.war`.
- `<module-name>` nome del modulo in cui il session bean è packaged.
- `<fully-qualified-interface-name>` nome di ogni interfaccia definita.

> [!example]- <font color="orange">Esempio</font>
>Nomi Standard:
>```markup
>java:global/cdbookstore/ItemEJB!org.agoncal.book.javaee7.ItemRemote
>java:global/cdbookstore/ItemEJB!org.agoncal.book.javaee7.ItemLocal
>java:global/cdbookstore/ItemEJB!org.agoncal.book.javaee7.ItemEJB
>```
>
>Accesso via moduli e app:
>```markup
>java:app/cdbookstore/ItemEJB!org.agoncal.book.javaee7.ItemRemote
>java:app/cdbookstore/ItemEJB!org.agoncal.book.javaee7.ItemLocal
>java:app/cdbookstore/ItemEJB!org.agoncal.book.javaee7.ItemEJB
>java:module/ItemEJB!org.agoncal.book.javaee7.ItemRemote
>java:module/ItemEJB!org.agoncal.book.javaee7.ItemLocal
>java:module/ItemEJB!org.agoncal.book.javaee7.ItemEJB
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