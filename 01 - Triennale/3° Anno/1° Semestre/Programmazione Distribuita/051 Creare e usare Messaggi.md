---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Message Service
cssclasses:
  - 
---
> [!warning] API JMS classica outdated
> Quest'API è "outdated" ma basilare. Esiste una API semplificata (sempre in `javax.jms`) che include `JMSContext`, `JMSProducer` e `JMSConsumer`.
> 

L'API [[049 Java Message Service|JMS]] classica fornisce classi e interfacce per le applicazioni che richiedono un sistema di messaggistica. Consente la comunicazione [[Asincrono|asincrona]] tra client, fornendo una connessione al provider e una sessione in cui è possibile creare e inviare o ricevere messaggi. Questi messaggi possono contenere testo o altri tipi di oggetti.

> [!tip]- Codice Producer
>```Java
>import java.util.Date;
>import javax.jms.Connection;
>import javax.jms.ConnectionFactory;
>import javax.jms.Destination;
>import javax.jms.JMSException;
>import javax.jms.MessageProducer;
>import javax.jms.Session;
>import javax.jms.TextMessage;
>import javax.naming.Context;
>import javax.naming.InitialContext;
>import javax.naming.NamingException;
>
>public class Producer {
>	public static void main(String[] args) {
>		try {
>			// Prendi il contesto JNDI
>			Context jndiContext = new InitialContext();
>			
>			// Cerca l'oggetto amministrato
>			ConnectionFactory connectionFactory = (ConnectionFactory)
>					jndiContext.lookup("jms/javaee7/ConnectionFactory");
>			Destination queue = (Destination) jndiContext.lookup("jms/javaee7/Queue");
>
>			// Crea la connessione per la coda
>			try (Connection connection = connectionFactory.createConnection()) {
>				Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
>				MessageProducer producer = session.createProducer(queue);
>				
>				// Invia il messaggio alla coda
>				TextMessage message = session.createTextMessage("Text message sent at " + new Date());
>				producer.send(message);
>			}
>		} catch (NamingException | JMSException e) {
>			e.printStackTrace();
>		}
>	}
>}
>```

> [!tip]- Codice Consumer
>```Java
>import javax.jms.Connection;
>import javax.jms.ConnectionFactory;
>import javax.jms.Destination;
>import javax.jms.JMSException;
>import javax.jms.MessageConsumer;
>import javax.jms.Session;
>import javax.jms.TextMessage;
>import javax.naming.Context;
>import javax.naming.InitialContext;
>import javax.naming.NamingException;
>
>public class Consumer {
>	public static void main(String[] args) {
>		try {
>			// Prendi il contesto JNDI
>			Context jndiContext = new InitialContext();
>			
>			// Cerca l'oggetto amministrato
>			ConnectionFactory connectionFactory = (ConnectionFactory)
>					jndiContext.lookup("jms/javaee7/ConnectionFactory");
>			Destination queue = (Destination) jndiContext.lookup("jms/javaee7/Queue");
>
>			// Crea la connessione per la coda
>			Connection connection = connectionFactory.createConnection();
>			Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
>			MessageConsumer consumer = session.createConsumer(queue);
>
>			connection.start();
>			
>			// Ricevi messaggi in loop
>			while (true) {
>				TextMessage message = (TextMessage) consumer.receive();
>				System.out.println("Message received: " + message.getText());
>			}
>		} catch (NamingException | JMSException e) {
>			e.printStackTrace();
>		}
>	}
>}
>```

### Connection Factory
Le **Connection Factory** sono [[049 Java Message Service#Oggetti Amministrati|Oggetti Amministrati]] che consentono a un'applicazione di connettersi a un provider creando un oggetto `Connection` in modo programmatico. Una `javax.jms.ConnectionFactory` è un'interfaccia che incapsula i parametri di configurazione *definiti da un amministratore*. 

Per utilizzare un oggetto amministrato come una `ConnectionFactory`, il client deve eseguire una ricerca [[025 Risorse e JNDI|JNDI]] (o utilizzare l'iniezione).

```Java
Context ctx = new InitialContext();
ConnectionFactory ConnectionFactory =
		(ConnectionFactory) ctx.lookup("jms/javaee7/ConnectionFactory");
```

### Destination
Una **Destination** è un oggetto amministrato che contiene informazioni di configurazione specifiche del provider, come l'indirizzo di destinazione. Ma questa configurazione è nascosta al client JMS, utilizzando l'interfaccia standard `javax.jms.Destination`. 

Come per la Connection Factory, per restituire tali oggetti è necessaria una ricerca JNDI.
```Java
Context ctx = new InitialContext();
Destination queue = (Destination) ctx.lookup("jms/javaee7/Queue");
```

### Connection
Un oggetto **Connection**, creato con il metodo `createConnection()` di `ConnectionFactory`, incapsula la connessione al provider JMS.

```Java
Connection connection = connectionFactory.createConnection();

connection.start(); // Comincia a ricevere messaggi
// ...
connection.stop(); // Smetti di ricevere messaggi

connection.close(); // Alla fine, chiudi la connessione
```

### Session
Un oggetto **Session** è l'alternativa a `Connection`, perché  fornisce un contesto transazionale in cui un insieme di messaggi da inviare (o ricevere) *sono raggruppati in un'unità di lavoro atomica*. Quindi, se si inviano diversi messaggi durante la stessa sessione, JMS farà in modo che vengano inviati tutti o nessuno.

```Java
Session session = connection.createSession(true, Session.AUTO_ACKNOWLEDGE);
```

## API Semplificata
### Connettersi alla destinazione
Questa parte è uguale sia per il consumer che per il producer ed è la stessa usata nell'API di sopra.

```Java
// Prendi il contesto JNDI
Context jndiContext = new InitialContext();

// Cerca l'oggetto amministrato
ConnectionFactory connectionFactory = (ConnectionFactory) jndiContext
		.lookup("jms/javaee7/ConnectionFactory");

// Ottieni la destinazione
Destination topic = (Destination) jndiContext.lookup("jms/javaee7/Topic");
Destination queue = (Destination) jndiContext.lookup("jms/javaee7/Queue");

// Crea il Contesto per JMS
try (JMSContext jmsContext = connectionFactory.createContext()) {
	// Operazioni producer/consumer
}
```

### Creare ed usare un Consumer
Un consumer di tipo **`JMSConsumer`** è ottenuto attraverso il metodo ***`createConsumer(destination)`*** di `JMSContext`. Si possono prendere dal messaggio due cose:
- *Un wrapper di un messaggio*, è ottenuto attraverso il metodo ***`receiveBody(.class)`*** di `JMSConsumer` oppure ***`getBody(.class)`*** di `Message`;
- *Una proprietà*, è ottenuta con uno dei metodi ***`getXXXProperty(name)`*** di `Message` specificando il nome della proprietà.

```Java
import javax.jms.Message;
import javax.jms.JMSConsumer;

// ...

try (JMSContext jmsContext = connectionFactory.createContext()) {
	// Crea il consumer specificando la destinazione
	JMSConsumer consumer = jmsContext.createConsumer(topic);
	
	// Ciclo infinito per ottenere i messaggi
	System.out.println("\nWaiting...");
	while (true) {
		// Ottieni il messaggio dalla destinazione.
		// Questa chiamata si blocca indefinitamente finché non 
		// viene prodotto un messaggio o finché questo JMSConsumer non viene chiuso
		Message message = consumer.receive();
		
		// Ottieni l'oggetto dal messaggio
		OrderDTO order = message.getBody(OrderDTO.class);
		
		// Ottieni una proprietà dal messaggio
		Float orderAmount = message.getFloatProperty("orderAmount");
		
		System.out.println("[" + orderAmount + "] Order received: " + order);
	}
}
```

### Creare un Producer
Un producer di tipo **`JMSProducer`** è ottenuto attraverso il metodo ***`createProducer()`*** di `JMSContext`. Si possono inviare due cose:
- *Una proprietà*, inserita nel messaggio attraverso il metodo ***`setProperty(name, value)`*** di `JMSProducer`;
- *Un wrapper di un messaggio*, è inviato nel body del messaggio attraverso il metodo ***`send(destination, object)`*** di `JMSProducer`, va messo alla fine della catena di metodi.

```Java
try (JMSContext jmsContext = connectionFactory.createContext()) {
	jmsContext.createProducer()
		.setProperty("orderAmount", totalAmount) // Imposta una proprietà
		.send(topic, order); // Invia il messaggio con l'oggetto
	System.out.println("\nOrder sent: " + order);
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