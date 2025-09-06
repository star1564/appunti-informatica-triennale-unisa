---
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: GlassFish Help
cssclasses:
  - 
---
## SETUP DEL DATABASE
**Creare il database**:
- `Services ➡️ (Tasto destro su "Java DB") ➡️ Create Database...`
- Specificare il nome del DB, un username ed una password (entrambe non vuote), ad esempio
	- User Name: `username`
	- Password: `password`

![[Pasted image 20231114150606.png|900]]

**Creare la connessione al DB**:
- `(Tasto destro sul DB) ➡️ Connect...` 
- L'icona cambia se si è connesso

![[Pasted image 20231114150810.png|900]]

È possibile visualizzare le tabelle memorizzate.

![[Pasted image 20231114150916.png|600]]

Per vedere i dati che si trovano nelle tabelle:
- `(Tasto destro sulla tabella) ➡️ View Data...`

![[Pasted image 20250120133606.png|1300]]

## SETUP PROGETTO [[033 Java Persistence API|JPA]]
**Creare un nuovo Progetto**:
- `New Project... ➡️ Java with Ant ➡️ Java Application`

![[Pasted image 20231119165910.png]]

**Creare l'Entity Class**:
- `(Tasto destro sul Progetto) ➡️ New... ➡️ Entity Class...` ^69e25d
- Specificare il nome, package, database connection e `Drop and Create`

![[Pasted image 20231114151840.png|650]]

> [!note] Questo crea anche il package `META-INF` con `persistence.xml` se non esistono!

**Struttura del progetto**:
![[Pasted image 20231114152624.png|300]]

## AGGIUNGERE LE LIBRERIE
- `Tasto destro su "Libraries"` 
- Aggiungere al progetto le librerie: 
	- `EclipseLink (JPA 2.1)`
	- `Java DB Driver`
	- `Java EE 7 API Library`

![[Pasted image 20231114154259.png]]

## MODIFICARE L'XML
Aggiungere all'interno di `persistence.xml`: 
- `;create=true` alla fine dell'URL per poter creare il database se non esiste.
- (*opzionale*) `<property name="javax.persistence.sql-load-script-source" value="insert.sql"/>` per inserire uno script chiamato `insert.sql` che carica dati nel database.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.1" xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_1.xsd">
  <persistence-unit name="JPA_LabPU" transaction-type="RESOURCE_LOCAL">
    <provider>org.eclipse.persistence.jpa.PersistenceProvider</provider>
    <class>jpa_lab.Book</class>
    <properties>
      <property name="javax.persistence.jdbc.url" value="jdbc:derby://localhost:1527/JPA_Lab;create=true"/>
      <property name="javax.persistence.jdbc.driver" value="org.apache.derby.jdbc.ClientDriver"/>
      <property name="javax.persistence.jdbc.user" value="APP"/>
      <property name="javax.persistence.jdbc.password" value="APP"/>
      <property name="javax.persistence.schema-generation.database.action" value="drop-and-create"/>
	  <property name="javax.persistence.sql-load-script-source" value="insert.sql"/>
    </properties>
  </persistence-unit>
</persistence>
```

Script in SQL (`insert.sql` nel package di default) per aggiungere dati iniziali al database (ogni istruzione su una sola riga...):
```SQL
INSERT INTO BOOK(ID, TITLE, DESCRIPTION, ILLUSTRATIONS, ISBN, NBOFPAGE, PRICE) VALUES (1000, 'Beginning Java EE 6', 'Best Java EE book ever', 1, '1234-5678', 450, 49)

INSERT INTO BOOK(ID, TITLE, DESCRIPTION, ILLUSTRATIONS, ISBN, NBOFPAGE, PRICE) VALUES (1001, 'Beginning Java EE 7', 'No, this is the best ', 1, '5678-9012', 550, 53)

INSERT INTO BOOK(ID, TITLE, DESCRIPTION, ILLUSTRATIONS, ISBN, NBOFPAGE, PRICE) VALUES (1010, 'The Lord of the Rings', 'One ring to rule them all', 0, '9012-3456', 222, 23)
```

**Struttura del progetto**:
![[Pasted image 20231114154050.png|300]]

Nota che dopo ogni esecuzione il DB viene resettato.

## SCRIVERE LE CLASSI
> [!tip]- Main
>```Java
>import java.util.List;
>import javax.persistence.EntityManager;
>import javax.persistence.EntityManagerFactory;
>import javax.persistence.EntityTransaction;
>import javax.persistence.Persistence;
>import javax.persistence.Query;
>
>public class Main {
>	public static void main(String[] args) {
>		Book book = new Book("H2G2", 12.5F,
>				"The Hitchhiker's Guide to the Galaxy",
>				"1-84023-742-2", 354, false);
>		
>		EntityManagerFactory emf
>				= Persistence.createEntityManagerFactory("JPA_LabPU");
>		EntityManager em = emf.createEntityManager();
>		EntityTransaction tx = em.getTransaction();
>		
>		tx.begin();
>		em.persist(book);
>		tx.commit();
>		
>		book = em.createNamedQuery("findBookH2G2", Book.class).getSingleResult();
>		System.out.println("\n####### Query per H2G2:");
>		System.out.println(book.getTitle() + " - (" + book.getPrice() + " €) - " + book.getDescription());
>		
>		Query all = em.createNamedQuery("findAllBooks", Book.class);
>		
>		List<Book‎ > results = all.getResultList();
>		System.out.println("\n####### Query per tutti i libri:");
>		for (Book b: results) {
>			System.out.println(b.getTitle() + " - (" + b.getPrice() + " €) - " + b.getDescription());
>		}
>		
>		em.close();
>		emf.close();
>	}	
>}
>```

> [!tip]- Book
>```Java
>import java.io.Serializable;
>import javax.persistence.Entity;
>import javax.persistence.GeneratedValue;
>import javax.persistence.Id;
>import javax.persistence.NamedQueries;
>import javax.persistence.NamedQuery;
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
>	// Costruttore obbligatorio
>	public Book() {}
>
>	// Costruttore normale
>	public Book(String title, Float price, String description, String isbn, Integer nbOfPage, Boolean illustrations) {
>		this.title = title;
>		this.price = price;
>		this.description = description;
>		this.isbn = isbn;
>		this.nbOfPage = nbOfPage;
>		this.illustrations = illustrations;
>	}
>	// Getters e Setters
>}
>```

## OUTPUT
![[Pasted image 20231114162252.png]]

È possibile visualizzare le righe nel database andando in:
`Services ➡️ (Espandere il database fino a "Tables") ➡️ (Tasto destro sulla Tabella) ➡️ View Data...`

![[Pasted image 20231114162515.png]]

%%
[[000 Indice PD|↖ Ritorna all'indice ↖]]
%%