---
aliases:
  - Entity Manager
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Persistence API
cssclasses:
  - 
---
[[033 Java Persistence API|JPA]] consente di [[036 Mapping di Relazioni ORM|mappare le entità nei database]] e di eseguire query con criteri diversi. 

La forza di JPA sta nella possibilità di *eseguire query sulle entità* e le loro relazioni in modo object oriented, senza dover utilizzare le chiavi esterne o le colonne del database sottostante. 

>Il componente principale per questo tipo di operazioni è l'**Entity Manager**. Il suo ruolo è quello di gestire le entità, *leggere* da (e *scrivere* su) un determinato database e consentire semplici operazioni *CRUD* (create, read, update, delete) sulle entità, nonché query complesse utilizzando [[039 JPQL|JPQL]]. 

In senso tecnico, l'Entity Manager è solo un'interfaccia la cui implementazione è affidata al fornitore di persistenza (ad esempio, EclipseLink). 

![[Pasted image 20231114134559.png|600]]
### Tipi di Entity Manager
Un Entity Manager può essere:
- **Gestito dalla applicazione**: l'applicazione è responsabile per l'istanza specifica di Entity Manager e per gestirne il ciclo di vita (il metodo `createEntityManagerFactory()` ha bisogno del nome di una [[038 Persistence Unit|Persistence Unit]]):

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("NomePU");
EntityManager em = emf.createEntityManager();
EntityTransaction tx = em.getTransaction();
```

- **Gestito dal container**: quando si ci affida a risorse iniettate.

```Java
@PersistenceContext(unitName = "NomePU")
private EntityManager em;
```


## Manipolare le Entità con un Entity Manager
Quando si devono manipolare delle entità, l'`EntityManager` si può vedere come una classe *Data Access Object* (DAO) che permette operazioni CRUD su una entità.

### Operazioni CRUD
| Tipo Ritorno | Metodo                                                      | Descrizione                                                                                                                                                                          |
| ------------ | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `void`       | **`persist(Object entity)`**                                | Gestisce l'*inserimento* di un'istanza nell'unità di persistenza.                                                                                                                    |
| `T`          | **`merge(T entity)`**                                       | Gestisce l'*aggiornamento* di un'istanza nell'unità di persistenza.                                                                                                                  |
| `void`       | **`remove(Object entity)`**                                 | Gestisce la *rimozione* di un'istanza dall'unità di persistenza.                                                                                                                     |
| `T`          | **`find(Class<T> entityClass, Object primaryKey)`**         | *Restituisce l'istanza* di un'entità con la chiave primaria specificata.                                                                                                             |
| `T`          | **`getReference(Class<T> entityClass, Object primaryKey)`** | *Restituisce una referenza all'istanza* di un'entità con la chiave primaria specificata. I dati vengono caricati solo quando si accede effettivamente ai loro valori (*Lazy Fetch*). |

> [!example]- <font color="orange">Esempio</font>
>```Java
>import javax.persistence.EntityManager;
>import javax.persistence.EntityManagerFactory;
>import javax.persistence.EntityTransaction;
>import javax.persistence.Persistence;
>
>public class Main {
>
>	public static void main(String[] args) {
>		// (1)
>		Book book = new Book("H2G2", 12.5F,
>				"The Hitchhiker's Guide to the Galaxy",
>				"1-84023-742-2", 354, false);
>
>		// (2)
>		EntityManagerFactory emf
>				= Persistence.createEntityManagerFactory("JPA_LabPU");
>		EntityManager em = emf.createEntityManager();
>		EntityTransaction tx = em.getTransaction();
>
>		// (3)
>		// Create: Faccio persistere il libro.
>		tx.begin();
>		em.persist(book);
>		tx.commit();
>		
>		// Read: Cerco il libro con la classe e l'id (Long).
>		// Se non lo trova ritorna null.
>		Book found = em.find(Book.class, 1L);
>		System.out.println("=> (Create) Risultato Query find(): " + found.getTitle() 
>				+ " - " + found.getDescription());
>
>		// (4)
>		// Supponiamo che dobbiamo aggiornare il libro con questo POJO.
>		Book newBook = new Book("H3G3", 1.99F,
>				"The non existent sequel to H2G2",
>				"1-99923-792-9", 150, true);
>
>		// Update: Aggiorno lo stato del libro nella PU.
>		tx.begin();
>		em.merge(newBook);
>		tx.commit();
>
>		found = em.find(Book.class, 1L);
>		System.out.println("=> (Update) Risultato Query find(): " + found.getTitle() 
>				+ " - " + found.getDescription());
>
>		// Delete: Rimuovo il libro dalla PU.
>		// Il POJO esiste ancora dopo il commit!
>		tx.begin();
>		em.remove(book);
>		tx.commit();
>		
>		found = em.find(Book.class, 1L);
>		if (found != null) {
>			System.out.println("=> Remove non riuscita");
>		} else {
>			System.out.println("=> (Remove) Libro non trovato");
>		}
>		
>		// (5)
>		em.close();
>		emf.close();
>	}
>}
>```
>![[Pasted image 20231115114927.png|400]]
>1. *Crea un'istanza dell'entità `Book`*: Le entità sono POJO annotati, gestiti dal fornitore di persistenza. Dal punto di vista di Java, un'istanza di una classe deve essere creata tramite la parola chiave `new`, come per qualsiasi POJO. È importante sottolineare che, fino a questo punto del codice, il persistence provider non è a conoscenza dell'oggetto `book`.
>
>2. *Ottiene un Entity Manager e una Transazione*: Questa è la parte importante del codice, poiché un Entity Manager è necessario per manipolare le entità. Innanzitutto, viene creato una factory di entity manager per la [[038 Persistence Unit|Persistence Unit]] "JPA_LabPU". Questa factory viene poi utilizzata per ottenere un Entity Manager (la variabile `em`), utilizzato in tutto il codice per ottenere una transazione (variabile `tx`) e per far persistere e recuperare un `Book`.
>
>3. *Persiste il libro nel database*: Il codice avvia una transazione (`tx.begin()`) e utilizza il metodo `EntityManager.persist()` per inserire un'istanza di `Book`. Quando la transazione viene eseguita (`tx.commit()`), i dati vengono effettivamente trasferiti al database.
>4. Ho utilizzato una nuova istanza di `Book` per dimostrare l'utilizzo della `merge()`, ma è possibile usare i metodi set per modificare gli attributi di una entità. Quando un'entità è gestita da un Entity Manager, quest'ultimo sincronizzerà automaticamente i dati dei suoi attributi se verranno usati i suoi metodi set. Per esempio se utilizziamo i metodi set invece di creare una nuova istanza di `Book`, questi cambiamenti verranno salvati automaticamente.
>
>```Java
>book.setTitle("H3G3");
>book.setDescription("The non existent sequel to H2G2");
>book.setPrice(1.99F);
>// A questo punto, book è stato aggiornato automaticamente
>```
>5. *Chiude l'Entity Manager e la factory*.


### Controllo Entità

| Tipo Ritorno | Metodo                        | Descrizione                                                                         |
| ------------ | ----------------------------- | ----------------------------------------------------------------------------------- |
| `void`       | **`flush()`**                 | *Sincronizza il contesto* di persistenza con il database. Viene chiamata automaticamente al commit time.                         |
| `void`       | **`clear()`**                 | *Rimuove tutti gli oggetti* gestiti dal contesto di persistenza dalla RAM.          |
| `void`       | **`refresh(Object entity)`**  | *Ricarica* lo stato dell'istanza dall'unità di persistenza.                         |
| `void`       | **`detach(Object entity)`**   | *Scollega* un'istanza dal contesto di persistenza, in modo che non sia più gestita. |
| `boolean`    | **`contains(Object entity)`** | *Controlla* se l'istanza di un oggetto è una entità gestita dall'`EntityManager` attuale.                                          |
| `void`       | **`close()`**                 | *Chiude* l'istanza di `EntityManager`.                                              |

> [!example]- <font color="orange">Esempio</font>
>```Java
>tx.begin();
>em.persist(book); // Diventa un'entità
>tx.commit();
>
>System.out.println("PERSIST => contains(book): " + em.contains(book));
>printAllBooks(em);
>
>tx.begin();
>em.detach(book); // Diventa un'entità detached, ma persiste nel database
>tx.commit();
>
>System.out.println("\nDETACH => contains(book): " + em.contains(book));
>printAllBooks(em);
>
>tx.begin();
>book = em.merge(book); // Lo faccio diventare di nuovo una entità normale
>tx.commit();
>
>System.out.println("\nRIAGGANCIAMENTO => contains(book): " + em.contains(book));
>printAllBooks(em);
>```
>![[Pasted image 20231115121839.png|300]]

^f70afc


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
- [[3 GlassFish Help - JPA|Come impostare JPA su NetBeans]]