---
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: GlassFish Help
cssclasses:
  - 
---
> [!info] Gli import e il package non vanno inseriti all'esame, solo nel progetto

![[Pasted image 20231217122946.png|1100]]

%%
https://www.planttext.com/

@startuml

package "EJB Module" {
  entity MyEntity {
    @Entity
    @XmlRootElement
    -id: Long
    -attributi: String
    
    +getters(): Object
    +setters(): void
  }
  
  class DBProducer {
    @Produces
    -em: EntityManager
  }
  
  class MyEJB {
    @Stateless 
    @LocalBean
    @WebService
    
    +eseguiOperazione(): void
    +createMyEntity(entity: MyEntity): void
    +removeMyEntity(entity: MyEntity): void
    +updateMyEntity(entity: MyEntity): void
    +findAll(): List<MyEntity>
    +findById(id: Long): MyEntity
  }
  
  interface MyEJBRemote {
    @Remote
    @WebService
    
    +eseguiOperazione(): void
  }
  
  class DBPopulator {
    @Singleton
    @Startup
    -populateDB(): void
  }
  
  interface InterceptThis {
    @InterceptorBinding
    @InterceptThis
  }
  
  class MyInterceptor {
    @InterceptThis
    -interceptMethod(): Object
  }
  
  class MyMDB {
    @MessageDriven
    +onMessage(message: Message): void
  }
  
  interface MyEvent {
    @Qualifier
  }
  
  class MyEventDoesSomething {
    @Observes
    @MyEvent
    +doSomething(param: MyParam): void
  }

}

package "WSClients" {
  class WSClient {
    -ottieniWS(): MyEJBRemote
  }
}

package "Clients" {
  class StandardClient {
    -ottieniBean(): MyEJBRemote
  }
  
  class MessageClient {
    -inviaMessaggio(): void
  }
}


MyEJB --> MyEntity : Utilizza
DBPopulator --> MyEntity : Utilizza
MyEJBRemote --|> MyEJB : Implementato da
DBPopulator --> MyEJB : Utilizza
DBProducer <-- MyEJB : Ottiene l'EM da
MyEJB ..> InterceptThis : Metodi intercettati da
StandardClient --> MyEJBRemote : Utilizza
WSClient --> MyEJBRemote : Utilizza
MyMDB ..> MyEJB : Può utilizzare
MessageClient --> MyMDB : Invia messaggio con MessageWrapper
InterceptThis ..|> MyInterceptor : Interceptor binder di
MyMDB ..> MyEvent : Può lanciare
MyEJB ..> MyEvent : Può lanciare
MyEventDoesSomething <|.. MyEvent : Qualifier di



@enduml



%%

## 1 - Creare una Entità

```Java
@Entity
@NamedQueries({
	// Named query di base
	@NamedQuery(name = MyEntity.FIND_ALL, query = "SELECT e FROM MyEntity e"),
	@NamedQuery(name = MyEntity.FIND_BY_ID, query = "SELECT e FROM MyEntity e WHERE e.id = ?1"),
	// Altre named query richieste dalla traccia
	@NamedQuery(name = MyEntity.FIND_BY_ATTRIBUTI, query = "SELECT e FROM MyEntity e WHERE e.attributi = ?1"),
	// ...
	// Evitare di scrivere query fisse (es. "WHERE e.isBoolean = true"), 
	// ma renderle dinamiche (es. "WHERE e.isBoolean = ?1")
})
@XmlRootElement // <-- Inserire solamente se la traccia richiede un Web Service
public class MyEntity implements Serializable {
	
	// Costanti per i nomi per le query
	public static final String FIND_ALL = "MyEntity.findAll";
	public static final String FIND_BY_ID = "MyEntity.findById";
	public static final String FIND_BY_ATTRIBUTI = "MyEntity.findByAttributi";
	
	@Id
    @GeneratedValue
	private int id;
	// Attributi dell'entità
	private String attributi;
	
	// Attributo con proprietà unico
	@Column(unique=true)
	private String attributoUnico;
	// ...
	
	// Costruttore vuoto
	public MyEntity() {}
	
	// Costruttore completo, getter e setter
	
}
```

## 2 - Creare un Database Producer
```Java
// Uguale per tutte le tracce d'esame
public class DBProducer {
	
	@Produces
	@PersistenceContext(unitName = "EsamePU")
	private EntityManager em;
	
}
```

## 3 - Creare l'interfaccia remota dell'EJB
```Java
@Remote
@WebService // <-- Inserire solamente se la traccia richiede un Web Service
public interface MyEJBRemote {
	
	// Metodi CRUD
	public void createMyEntity(MyEntity entity);
	public void removeMyEntity(MyEntity entity);
	public void updateMyEntity(MyEntity entity);
	public List<MyEntity> findAll();
	public MyEntity findById(int id);
	
	// Altri metodi inerenti alla traccia
	
}
```

## 4 - Creare l'EJB
```Java
@Stateless
@LocalBean
@WebService // <-- Inserire solamente se la traccia richiede un Web Service
public class MyEJB implements MyEJBRemote {

	@Inject
	private EntityManager em;
	
	// Metodi CRUD
	
	@Override
	public void createMyEntity(MyEntity entity) {
		em.persist(entity);
	}
	
	@Override
	public void removeMyEntity(MyEntity entity) {
		em.remove(entity);
	}
	
	@Override
	public void updateMyEntity(MyEntity entity) {
		em.merge(entity);
	}
	
	@Override
	public List<MyEntity> findAll() {
		TypedQuery query = em.createNamedQuery(MyEntity.FIND_ALL, MyEntity.class);
        return query.getResultList();
	}
	
	@Override
	public MyEntity findById(int id) {
		TypedQuery query = em.createNamedQuery(MyEntity.FIND_BY_ID, MyEntity.class);
		query.setParameter(1, id);
        return (MyEntity) query.getSingleResult();
	}
	
	// Altri metodi inerenti alla traccia
	
}
```

## 5 - Creare il Database Populator (Singleton)
```Java
@Singleton
@Startup
@DataSourceDefinition(
		className = "org.apache.derby.jdbc.EmbeddedDataSource", 
		name = "jdbc/EsameDataSource",
		databaseName = "EsameDB",
		user = "username",
		password = "password"
)
public class DBPopulator {
    
    @EJB
    private MyEJB ejb;
    
    @PostConstruct
    public void populateDB() {
        MyEntity e1 = new MyEntity("Attributi1");
        MyEntity e2 = new MyEntity("Attributi2");
        MyEntity e3 = new MyEntity("Attributi3");
        ejb.createMyEntity(e1);
        ejb.createMyEntity(e2);
        ejb.createMyEntity(e3);
    }
    
}
```

## 6 - Creare gli Interceptor
**Per ogni interceptor** richiesto, creare due classi.
### Interceptor Binding
```Java
@InterceptorBinding
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD, ElementType.TYPE})
public @interface InterceptThis {}
```

### Interceptor
```Java
@Interceptor
@InterceptThis
public class MyInterceptor {
	// Solamente se richiede di salvare per ogni metodo DIVERSO intercettato un valore
	private Map<String, Integer> map = new HashMap<>();
	
	// Solamente se richiede di contare un valore per QUALSIASI metodo intercettato
	private Integer counter = 0;
	
	@AroundInvoke
	public Object interceptMethod(InvocationContext ic) throws Exception {
		// Ottieni l'ejb SE NECESSARIO
		MyEJBRemote ejb = (MyEJBRemote) new InitialContext().lookup(
				"java:global/Esame/MyEJB!esame.MyEJBRemote"
		);
		
		// Ottiene il nome della classe e del metodo SE RICHIESTO
		String className = ic.getMethod().getDeclaringClass().getSimpleName();
		String methodName = ic.getMethod().getName();
		
		// Ottiene i parametri passati al metodo SE RICHIESTO
		Object[] args = ic.getParameters();
		MyParameter arg0 = (MyParameter) args[0];
		MyParameter arg1 = (MyParameter) args[1];
		//...
		
		// Logica dell'interceptor
		// return null; se si deve interrompere la chiamata
		
		Object result = ic.proceed();
		return result;
	}
}
```

Inserire l'annotazione `@InterceptThis` su ogni metodo richiesto dalla traccia. Se la traccia richiede un interceptor su tutti i metodi, inserire l'annotazione `@InterceptThis` sulla classe.

## 7 - Creare Message Driven Beans
### Creare un Message Wrapper
```Java
public class MessageWrapper implements Serializable {
	
	private int id;
	private String valore;
	// Costruttori, getter, setter, toString
	
}
```

### MDB
```Java
@MessageDriven(mappedName = "jms/javaee7/Topic")
public class MyMDB implements MessageListener {
	
	@EJB
	private MyEJB ejb;
	
	@Override
	public void onMessage(Message message) {
		try {
			MessageWrapper mw = message.getBody(MessageWrapper.class);
			// Logica MDB
		} catch (Exception e) {
			System.err.println(e);
		}
	}
	
}
```

## 8 - Creare un evento
**Per ogni evento** richiesto, creare due classi.
### Annotazione Qualifier Evento

```Java
@Qualifier
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD, ElementType.TYPE, ElementType.METHOD, ElementType.PARAMETER})
public @interface MyEvent {}
```

### Logica Evento
```Java
public class MyEventDoesSomething {
	
	public void doSomething(@Observes @MyEvent MyParameter param) {
		// Logica Evento
	}
	
}
```

### Lanciare un evento
```Java
public class SomeClass {
	@Inject
	@MyEvent
	private Event<MyParameter> myEvent;

	public void someMethod() {
		MyParameter param;
		...
		myEvent.fire(param)
	}
}
```

## 9 - Creare i Client
### Client Normale
```Java
public class Client {
    public static void main(String[] args) throws Exception {
        Context ctx = new InitialContext();
		
		// CLIENT BASE
		MyEJBRemote ejb = (MyEJBRemote) ctx.lookup(
                "java:global/Esame/MyEJB!esame.MyEJBRemote"
        );
		
		// Logica client richiesta
    }
}
```

### Client Messaggi
```Java
public class MessageClient {
	public static void main(String[] args) throws Exception {
		Context ctx = new InitialContext();
		ConnectionFactory cf = (ConnectionFactory) ctx.lookup("jms/javaee7/ConnectionFactory");
		Destination topic = (Destination) ctx.lookup("jms/javaee7/Topic");
		
		try (JMSContext jmsContext = cf.createContext()) {
			MessageWrapper m = new MessageWrapper(1, "valore");
			jmsContext.createProducer().send(topic, m);
			System.out.println("Messaggio inviato: " + m);
		}
	}
}
```

### Client Web Services
```Java
public class WSClient {
	public static void main(String[] args) throws Exception {
		MyEJB ejb = new MyEJBService().getMyEJBPort();

		// Logica client richiesta
		// Attenzione, non esiste il toString nel service
	}
}
```

## Progetto da Consegnare
Ci sono da fare quattro progetti.

### Setup (progetto da consegnare)
1. Fare il [[3 GlassFish Help - JPA#SETUP DEL DATABASE|Setup del Database]]
2. Creare [[4 GlassFish Help - EJB#CREARE ED USARE LA PERSISTENCE UNIT ALL'INTERNO DI UN EJBMODULE|Connection Pool, Data Source e Persistence Unit]]
3. Fare il [[6 GlassFish Help - JMS#Setup JMS|Setup di JMS]]

### Modulo EJB (progetto da consegnare)
1. Creare il [[4 GlassFish Help - EJB#^87d269|Progetto del Modulo EJB]] (chiamarlo `COGNOMENOME_EJB`)
2. Creare i packages con `(Tasto destro su 'Source Packages' del modulo EJB) ➡️ New ➡️ Java Package... ➡️ (dare i seguenti nomi)`
	- `database` - Conterrà:
		- [[#1 - Creare una Entità|Le classi Entità]]
		- [[#2 - Creare un Database Producer|Il Database Producer]]
		- [[#5 - Creare il Database Populator (Singleton)|Il Database Populator]]
	- `beans` - Conterrà:
		- [[#3 - Creare l'interfaccia remota dell'EJB|L'Interfaccia Remota degli EJB]]
		- [[#4 - Creare l'EJB|L'Implementazione degli EJB]]
	- `events` - Conterrà:
		- [[#Annotazione Qualifier Evento|Le Annotazioni Qualifier degli Eventi]]
		- [[#Logica Evento|Le Classi della Logica degli Eventi]]
	- `interceptors` - Conterrà:
		- [[#Interceptor Binding|Le Annotazioni Binding degli Interceptor]]
		- [[#Interceptor|Le Classi della Logica degli Interceptor]]
	- `messages` - Conterrà:
		- [[#7 - Creare Message Driven Beans|I Message Driven Beans]]
		- [[#Creare un Message Wrapper|Le Classi dei Message Wrappers]]
4. Dopo aver inserito tutti i file necessari, fare il [[4 GlassFish Help - EJB#^3483e1|deploy del progetto Modulo EJB]] e [[4 GlassFish Help - EJB#^ff1828|segnare il percorso JNDI]] del bean appropriato

### Client Normale (progetto da consegnare)
1. Creare il [[4 GlassFish Help - EJB#CLIENT|Progetto del Client Normale e importare le librerie necessarie]] (chiamarlo `COGNOMENOME_CLIENT`)
2. Creare la [[#Client normale|Classe Client con un main]]
3. Inserire il percorso JNDI del bean come stringa nel `ctx.lookup()`

### Client Messaggi (progetto da consegnare)
1. Creare il [[6 GlassFish Help - JMS#Client|Progetto del Client Messaggi e importare le librerie necessarie]] (chiamarlo `COGNOMENOME_MESSAGE_CLIENT`)
2. Creare la [[#Client Messaggi|Classe Client Messaggi con un main]]
3. Inserire il dati necessari di JMS configurati in precedenza

### Client Web Services (progetto da consegnare)
1. Creare i [[7 GlassFish Help - WS#Creare il WS|Web Services nel progetto Modulo EJB]]
2. Fare il [[7 GlassFish Help - WS#Testare il WS con NetBeans|Test dei WS]]
3. Creare il [[7 GlassFish Help - WS#SETUP PROGETTO WS CLIENT|Progetto del Client Web Services]] (chiamarlo `COGNOMENOME_WS_CLIENT`)
4. Creare la [[#Client Web Services|Classe Client Web Services con un main]]

%%
[[000 Indice PD|↖ Ritorna all'indice ↖]]
%%