---
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: GlassFish Help
cssclasses:
  - 
---
## Setup JMS

**Aggiungere la JMS Connection Factory su GlassFish**:
- `(Andare su localhost:4848) ➡️ (Sulla sidebar) Resources ➡️ (Sulla sidebar) JMS Resources ➡️ Connection Factories ➡️ New... ➡️ (Dopo aver impostato i seguenti parametri, premere OK)`
	- JNDI Name: `jms/javaee7/ConnectionFactory`
	- Resource Type: `javax.jms.ConnectionFactory`

![[Pasted image 20231120111248.png|700]]

**Aggiungere le JMS Destination Resources per la Queue e la Topic**:
- `(Andare su http://localhost:4848) ➡️ (Sulla sidebar) Resources ➡️ (Sulla sidebar) JMS Resources ➡️ Destination Resources ➡️ New... ➡️ (Dopo aver impostato i seguenti parametri su una destinazione, premere OK e ripetere per la prossima destinazione)`
	- *Queue*
		- JNDI Name: `jms/javaee7/Queue`
		- Physical Destination Name: `Queue`
		- Resource Type: `javax.jms.Queue`
	- *Topic*
		- JNDI Name: `jms/javaee7/Topic`
		- Physical Destination Name: `Topic`
		- Resource Type: `javax.jms.Topic`

![[Pasted image 20231120111203.png|700]]

![[Pasted image 20231124191147.png|700]]

![[Pasted image 20231124191216.png]]

## Client
**Creare un nuovo progetto**:
- `New Project ➡️ Java with Ant ➡️ Java Application ➡️ (Creare un nuovo Package)`

**Aggiungere le dipendenze**:
- `Progetto ➡️ (Tasto destro su Libraries) ➡️ Add Library... ➡️ (Aggiungere la libreria "Java EE7 API Library")`
- `Progetto ➡️ (Tasto destro su Libraries) ➡️ Add Jar/Folder... ➡️ (Aggiungere "[USER_HOME]/GlassFish_Server/glassfish/lib/gf-client.jar")`

### Progetto con Due Main
> [!error] Quello che viene chiesto all'esame è quello con un [[049 Java Message Service#Message-Driven Beans|Message-Driven Bean]]

Creare due classi Java, entrambe con un main:
- `Consumer`
- `Producer`

**Avviare prima il Consumer (andrà in un loop infinito) e poi il Producer**:
- `(Tasto destro su i file delle classi con main) ➡️ Run File`

> [!tip]- Codice Message Wrapper
>```Java
>public class OrderDTO implements Serializable {
>	private Long orderId;
>	private Date creationDate;
>	private String customerName;
>	private Float totalAmount;
>
>	public OrderDTO() {
>	}
>
>	public OrderDTO(Long orderId, Date creationDate, String customerName, Float totalAmount) {
>		this.orderId = orderId;
>		this.creationDate = creationDate;
>		this.customerName = customerName;
>		this.totalAmount = totalAmount;
>	}
>
>	// Getter, setter, toString...
>}
>```


> [!tip]- Codice Producer
>```Java
>import java.util.Date;
>import javax.jms.ConnectionFactory;
>import javax.jms.Destination;
>import javax.jms.JMSContext;
>import javax.naming.Context;
>import javax.naming.InitialContext;
>
>public class OrderProducer {
>
>	public static void main(String[] args) throws Exception {
>		Float totalAmount = 10F;
>		// Crea un ordine
>		OrderDTO order = new OrderDTO(1234L, new Date(), "Serge Gainsbourg", totalAmount);
>
>		// Prendi il contesto JNDI
>		Context jndiContext = new InitialContext();
>
>		// Cerca l'oggetto amministrato
>		ConnectionFactory connectionFactory = (ConnectionFactory) jndiContext.lookup("jms/javaee7/ConnectionFactory");
>		Destination topic = (Destination) jndiContext.lookup("jms/javaee7/Topic");
>
>		// Invia l'oggetto alla topic
>		try (JMSContext jmsContext = connectionFactory.createContext()) {
>			jmsContext.createProducer()
>					.setProperty("orderAmount", totalAmount)
>					.send(topic, order);
>
>			System.out.println("\nOrder sent: " + order);
>		}
>	}
>}
>```


> [!tip]- Codice Consumer
>```Java
>import javax.jms.ConnectionFactory;
>import javax.jms.Destination;
>import javax.jms.JMSContext;
>import javax.naming.Context;
>import javax.naming.InitialContext;
>
>public class OrderConsumer {
>
>	public static void main(String[] args) throws Exception {
>		// Prendi il contesto JNDI
>		Context jndiContext = new InitialContext();
>
>		// Cerca l'oggetto amministrato
>		ConnectionFactory connectionFactory = (ConnectionFactory) jndiContext
>				.lookup("jms/javaee7/ConnectionFactory");
>		Destination topic = (Destination) jndiContext.lookup("jms/javaee7/Topic");
>
>		// Ciclo infinito per ottenere i messaggi
>		System.out.println("\nWaiting...");
>		try (JMSContext jmsContext = connectionFactory.createContext()) {
>			while (true) {
>				// Ottieni il messaggio dal topic
>				OrderDTO order = jmsContext.createConsumer(topic)
>						.receiveBody(OrderDTO.class);
>				
>				System.out.println("Order received: " + order);
>			}
>		}
>	}
>}
>```


> [!warning] Fare attenzione ai consumer ancora in vita
> ![[Pasted image 20231124191903.png]]


### Progetto con Message Driven Bean
1. Nel [[4 GlassFish Help - EJB#EJBMODULE|progetto Modulo EJB]], creare un MDB
2. Nel progetto client di JMS, creare una classe Java con un main che invierà un messaggio al topic.

> [!tip]- Codice Client
> ```Java
> public class MessageClient {
> 	public static void main(String[] args) throws Exception {
> 		Context ctx = new InitialContext();
> 		ConnectionFactory cf = (ConnectionFactory) ctx.lookup("jms/javaee7/ConnectionFactory");
> 		Destination topic = (Destination) ctx.lookup("jms/javaee7/Topic");
> 		
> 		try (JMSContext jsmContext = cf.createContext()) {
> 			MessageWrapper m = new MessageWrapper(1, "valore");
> 			jmsContext.createProducer().send(topic, m);
> 			System.out.println("Messaggio inviato: " + m);
> 		}
> 	}
> }
> ```

> [!tip]- Codice MDB
> ```Java
> @MessageDriven(mappedName = "jms/javaee7/Topic")
> public class MyMDB implements MessageListener {
> 	
> 	@EJB
> 	private MyEJB ejb;
> 	
> 	@Override
> 	public void onMessage(Message message) {
> 		try {
> 			MessageWrapper mw = message.getBody(MessageWrapper.class);
> 			// Logica MDB
> 		} catch (Exception e) {
> 			System.err.println(e);
> 		}
> 	}
> 	
> }
> ```

%%
[[000 Indice PD|↖ Ritorna all'indice ↖]]
%%