---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Persistence API
cssclasses:
  - 
---
>**Java Persistence Query Language** (*JPQL*) è un linguaggio di query orientato agli oggetti che fa parte della specifica [[033 Java Persistence API|JPA]]. JPQL è progettato per consentire agli sviluppatori di eseguire query sui dati persistiti nel database utilizzando il modello di oggetti Java, piuttosto che il modello di tabelle relazionali.

JPQL *è indipendente dal database sottostante*. Ciò significa che le query JPQL possono essere scritte in modo da funzionare su diversi database relazionali senza la necessità di adattamenti specifici del database. In questo modo si può cambiare il database quando si vuole.

## Query
Le query JPQL vengono eseguite attraverso l'[[037 Entity Manager|Entity Manager]] di JPA. La sintassi di base di una query JPQL è simile a [[024 Comandi SQL|SQL]], ma invece di operare su tabelle e colonne, *opera su entità e attributi*.

```sql
SELECT b 
FROM Book b 
WHERE b.author = :author
```

In questa query:
- `Book` è l'[[034 Entità JPA|entità]] che stiamo interrogando.
- `b` è un alias per l'entità `Book`.
- `:author` è un parametro che sarà sostituito con il valore effettivo quando eseguiamo la query.
	- è possibile anche specificare la posizione del parametro attraverso `?numPar`, per esempio:

```SQL
SELECT b FROM Book b WHERE b.author = ?1
```

Inoltre `.author` si riferisce al nome dell'attributo di `Book`, non al nome di una colonna del database.

Una istruzione può essere eseguita con:
- query *dinamiche*, create dinamicamente e al run-time;
- query *statiche*, definite staticamente a tempo di compilazione.

> [!tip]- Metodi per creare ed eseguire Query attraverso l'Entity Manager
> 
>| Tipo Ritorno   | Metodo                                    | Descrizione                                                                                      |
>| -------------- | ----------------------------------------- | ------------------------------------------------------------------------------------------------ |
>| `Query`        | **`createQuery(String jpqlString)`**        | Crea e restituisce un oggetto `Query` basato sulla stringa di query **DINAMICA** specificata.        |
>| `Query`        | **`createNamedQuery(String name)`**       | Crea e restituisce un oggetto `Query` basato sul nome di una query nominata **STATICA** specificata. |
>| `Query`        | **`createNativeQuery(String sqlString)`** | Crea e restituisce un oggetto `Query` basato sulla stringa SQL specificata.                      |
>| `List<Object>` | **`getResultList()`**                     | Esegui la query ed ottieni una `List` con i risultati.                                            |

> [!example]- <font color="orange">Esempio Finale</font>
>```Java
>import java.io.Serializable;
>import javax.persistence.Entity;
>import javax.persistence.GeneratedValue;
>import javax.persistence.Id;
>import javax.persistence.NamedQuery;
>import javax.persistence.NamedQueries;
>
>@Entity
>@NamedQueries({
>	@NamedQuery(
>		name = "findAllBooks",
>		query = "SELECT b FROM Book b"
>	),
>	@NamedQuery(
>		name = "findBookH2G2",
>		query = "SELECT b FROM Book b WHERE b.title = 'H2G2'"
>	)
>})
>public class Book implements Serializable {
>	@Id
>	@GeneratedValue
>	private Long id;
>	private String title;
>	private Float price;
>	private String description;
>	private String isbn;
>	private Integer nbOfPage;
>	private Boolean illustrations;
>	
>	public Book(){}
>	// Constructor, getters, setters	
>}
>```
>
>```Java
>import javax.persistence.EntityManager;
>import javax.persistence.EntityManagerFactory;
>import javax.persistence.EntityTransaction;
>import javax.persistence.Persistence;
>
>public class Main {
>	public static void main(String[] args) {
>		Book book = new Book("H2G2", 12.5F,
>				"The Hitchhiker's Guide to the Galaxy",
>				"1-84023-742-2", 354, false);
>		
>		EntityManagerFactory emf
>				= Persistence.createEntityManagerFactory("chapter04PU");
>		EntityManager em = emf.createEntityManager();
>		EntityTransaction tx = em.getTransaction();
>		
>		tx.begin();
>		em.persist(book);
>		tx.commit();
>		
>		Book result = em.createNamedQuery("findBookH2G2", Book.class).getSingleResult();
>		System.out.println(result);
>		
>		em.close();
>		emf.close();
>	}
>}
>```
### Query Statiche
Le **query statiche** (*named queries*) sono definite usando l'annotazione **`@NamedQuery`** (per specificarne una) e **`@NamedQueries`** (per specificare una lista di `@NamedQuery`).
Vengono definite *direttamente nelle classi entità* e sono conosciute a livello di codice durante la compilazione.

```Java
@Entity
@NamedQueries({
	@NamedQuery(
		name = "findAllBooks",
		query = "SELECT b FROM Book b"
	),
	@NamedQuery(
		name = "findBooksByTitle",
		query = "SELECT b FROM Book b WHERE b.title = :title"
	)
})
public class Book implements Serializable {
    // ... altri dettagli della classe entità
}
```

```Java
public List<Book> findBooksByTitle(String title) {
    Query query = entityManager.createNamedQuery("findBooksByTitle", Book.class);
	query.setParameter("title", title);
    return query.getResultList();
}
```

### Query Dinamiche
Le **query dinamiche** sono delle query che vengono create e definite a livello di runtime, durante l'esecuzione dell'applicazione. Sono costruite in modo flessibile in base alle esigenze dell'applicazione.

```Java
public List<Book> findBooksByTitle(String title) {
    String jpql = "SELECT b FROM Book b WHERE b.title = :title";

    // Creazione dinamica di una query JPQL
    Query query = entityManager.createQuery(jpql, Book.class);
    query.setParameter("title", title);

    return query.getResultList();
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