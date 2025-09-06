---
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: GlassFish Help
cssclasses:
  - 
---

## SETUP PROGETTI
**Creare due progetti**:
- *EJBModule*: è il progetto che contiene uno o più [[041 Enterprise Java Beans|EJB]] e tutte le risorse associate; ^87d269
	- `New Project ➡️ Java with Ant ➡️ Java Enterprise ➡️ EJB Module`
	  
	  ![[Pasted image 20231119165819.png]]
- *Client*: è il progetto responsabile (con una main class) di interagire con il modulo EJB e di utilizzare i vari metodi offerti dagli EJB.
	- `New Project ➡️ Java with Ant ➡️ Java Application`

## EJBMODULE
**Creare il file `beans.xml`**:
- `(Tasto destro sul Progetto) ➡️ New ➡️ Contexts and Dependency Injection ➡️ beans.xml (CDI Configuration File)`
- Modificare l'attributo `bean-discovery-mode` in modalità `"all"`

**Creare un nuovo pacchetto**: 
- `Source Packages (Tasto destro) ➡️ New ➡️ Java Package...` (dargli un nome)

> [!warning] Se devi fare una Persistence Unit con entità nel bean vai prima [[#CREARE ED USARE LA PERSISTENCE UNIT ALL'INTERNO DI UN EJBMODULE|QUI]]

Creare *MANUALMENTE* un nuovo EJB e la sua Interfaccia Remota con `New ➡️ Java Class... / Java Interface...`

> [!note]- Creare EJB "Automaticamente"
> Su NetBeans è possibile creare un EJB (attraverso `New ➡️ Stateless Bean...`) linkandogli il progetto Client ed una Interfaccia Remota/Locale.
> Però inserisce l'interfaccia all'interno della classe Client (all'interno di un nuovo package), quindi bisogna spostarla nel progetto EJBModule e cancellare il package inutile.


![[Pasted image 20231119172911.png]]

> [!tip]- HelloBean
>```Java
>package mybeans;
>
>import javax.ejb.LocalBean;
>import javax.ejb.Stateless;
>
>@Stateless
>@LocalBean
>public class HelloBean implements HelloBeanRemote {
>	@Override
>	public String sayHello(String name) {
>		return "Hello " + name + "!";
>	}
>}
>```

> [!tip]- HelloBeanRemote
>```Java
>package mybeans;
>
>import javax.ejb.Remote;
>
>@Remote
>public interface HelloBeanRemote {
>	public String sayHello(String name);
>}
>```

**Fare il deploy del bean**: ^0ddf22
- `(Tasto destro sul Progetto) ➡️ Deploy` ^3483e1
- Nella console di output segnare il [[041 Enterprise Java Beans#Identificare un EJB con JNDI|percorso JNDI dell'interfaccia remota]]
  
  ![[Pasted image 20231119174800.png]] ^ff1828

> [!bug] NameAlreadyBoundException
> Se si ha questa eccezione nei log di GlassFish, riavviare il server. Si consiglia di fare l'undeploy prima di effettuare modifiche per evitare il problema.


## CLIENT
**Specificare le dipendenze necessarie**:
- `(Tasto destro sul progetto "Client") ➡️ Properties ➡️ Libraries ➡️ (Il pulsante "+" vicino a "Classpath") ➡️ Add Project... ➡️ (Aggiungere il progetto "EJBModule")`
- `(Tasto destro sul progetto "Client") ➡️ Properties ➡️ Libraries ➡️ (Il pulsante "+" vicino a "Classpath") ➡️ Add JAR/Folder ➡️ (Aggiungere "[USER_HOME]/GlassFish_Server/glassfish/lib/gf-client.jar")`

Il pacchetto `[USER_HOME]/GlassFish_Server/glassfish/lib/gf-client.jar` carica configurazioni per l'`InitialContext` di default e si trova nella cartella di installazione di GlassFish.

![[Pasted image 20231119173728.png|700]]

> [!tip]- Main in Client
>```Java
>package client;
>
>import javax.naming.Context;
>import javax.naming.InitialContext;
>import javax.naming.NamingException;
>import myBeans.HelloBeanRemote;
>
>public class Main {
>	public static void main(String[] args) throws NamingException {
>		Context ctx = new InitialContext();
>		HelloBeanRemote ejb = (HelloBeanRemote) ctx.lookup(
>			// Inserire qui l'indirizzo dell'interfaccia remota vista nella
>			// console di output di GlassFish dopo il deploy dell'EJB
>			"java:global/EJBModule/HelloBean!myBeans.HelloBeanRemote"
>		);
>		
>		System.out.println(ejb.sayHello("My Name"));
>	}
>}
>```

**Eseguire il file Main**:
- `(Tasto destro sulla classe "Main" del Progetto "Client") ➡️ Run File`

![[Pasted image 20231119175428.png]]

> [!info]- Problemi con la lookup dei bean
> Se ci sono problemi con la lookup dei bean, verificare che si stia usando il dominio corretto con i parametri di default nel file `[USER_HOME]/GlassFish_Server/glassfish/domains/domain1/config/domain.xml`.


**Output** (lento...):
![[Pasted image 20231119175452.png]]

> [!note] Aggiornamento automatico delle variabili non presente nel client
> In una applicazione client l'aggiornamento automatico di JPA con i metodi di set non funziona, quindi bisogna implementare un metodo di aggiornamento nell'EJB:
> 
>```Java
>@Override
>public void updateSong(Song song) {
>	em.merge(song);
>}
>```

## CREARE ED USARE LA PERSISTENCE UNIT ALL'INTERNO DI UN EJBMODULE
Se si ha bisogno di creare una entità in un progetto che utilizza gli EJB si devono seguire vari passi.

**Creare un nuovo JDBC Connection Pool**: quando si crea una nuova entità ed una nuova persistence unit all'interno di un modulo ejb ([[3 GlassFish Help - JPA#^69e25d|con lo stesso metodo di prima]]), JPA forza l'utilizzo di un *Data Source* (invece di un link di connessione al database). Quindi si deve creare una nuova Connection Pool dalla console di admin di GlassFish:
- `(Andare su localhost:4848) ➡️ (Sulla sidebar) Resources ➡️ (Sulla sidebar) JDBC ➡️ JDBC Connection Pools ➡️ New... ➡️ (imposta i seguenti parametri) ➡️ Next`
	- Pool Name: `(dare un nome al pool)`
	- Resource Type: `java.sql.Driver`
	- Database Driver Vendor: `Derby`

![[Pasted image 20231126105423.png]]

- `(Nel secondo step della creazione, inserire le seguenti proprietà in fondo alla pagina, creando proprietà se non esistono) ➡️ Finish`
	- dataBase: `(nome del database, si può prendere dal link di connessione al DB)`
	- portNumber: `1527 (si può prendere dal link)`
	- password: `(inserire la password settata durante la creazione del DB)`
	- user: `(inserire l'user settato durante la creazione del DB)`
	- URL: `(inserire il link)`

![[Pasted image 20231126110041.png]]

**Verifica la Connection Pool con un Ping**:

![[Pasted image 20231126110429.png]]

**Creare un nuovo Data Source**:
- `(Andare su localhost:4848) ➡️ (Sulla sidebar) Resources ➡️ (Sulla sidebar) JDBC ➡️ JDBC Resources ➡️ New... ➡️ (imposta i seguenti parametri) ➡️ OK`
	- JNDI Name: `(dare un nome alla risorsa, come "jdbc/nomeRisorsa")`
	- Pool Name: `(specificare il pool creato in precedenza)`

![[Pasted image 20231126110858.png]]

**Creare una Persistence Unit ed una Entità**:
- `(Tasto destro sul Progetto) ➡️ New... ➡️ Entity Class... (oppure Persistence Unit...) ➡️ (specificare come Data Source quella appena creata)`

![[Pasted image 20231126111156.png]]

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.1" xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_1.xsd">
  <persistence-unit name="MusicPU" transaction-type="JTA">
    <jta-data-source>jdbc/MusicLibraryDataSource</jta-data-source>
    <exclude-unlisted-classes>false</exclude-unlisted-classes>
    <properties>
      <property name="javax.persistence.schema-generation.database.action" value="drop-and-create"/>
    </properties>
  </persistence-unit>
</persistence>
```

> [!warning] Questo progetto *NON* deve includere le seguenti librerie
> - EclipseLink
> - Java EE 7 API Library
> 
> Per qualche motivo non permettono di fare il deploy (Java DB Driver è ok, ma *non sembra essere necessario*).

**Crea l'entità, il bean, l'interfaccia ed esegui il [[#^0ddf22|deploy]] del progetto EJBMODULE**:
> [!example]- <font color="orange">Esempio</font>
>```Java
>package it.pd2023.musiclibrary;
>import javax.ejb.LocalBean;
>import javax.ejb.Stateless;
>import javax.persistence.EntityManager;
>import javax.persistence.PersistenceContext;
>
>@Stateless
>@LocalBean
>public class MyBean implements MyBeanRemote {
>	@PersistenceContext(unitName = "MusicPU")
>	private EntityManager em;
>
>	@Override
>	public void saveSong(Song song) {
>		em.persist(song);
>	}
>	
>}
>```
>
>```Java
>package it.pd2023.musiclibrary;
>
>import javax.ejb.Remote;
>
>@Remote
>public interface MyBeanRemote {
>	public void saveSong(Song song);
>}
>```
>
>```Java
>package it.pd2023.musiclibrary;
>
>import java.io.Serializable;
>import javax.persistence.Entity;
>import javax.persistence.GeneratedValue;
>import javax.persistence.GenerationType;
>import javax.persistence.Id;
>
>
>@Entity
>public class Song implements Serializable {
>
>	private static final long serialVersionUID = 1L;
>	@Id
>     @GeneratedValue(strategy = GenerationType.AUTO)
>	private Long id;
>	private String authors;
>
>	public Song() {
>	}
>	
>	public Song(String authors) {
>		this.authors = authors;
>	}
>	
>	// Getter e setter ...
>}
>```


%%
[[000 Indice PD|↖ Ritorna all'indice ↖]]
%%