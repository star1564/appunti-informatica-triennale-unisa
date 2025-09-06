---
tags:
  - corsi/informatica/programmazione_distribuita
cssclasses:
  - 
---
>L'annotazione **`@Inject`** è uno strumento fondamentale che consente di *dichiarare un punto in cui una dipendenza dovrebbe essere [[027 Context e Dependency Injection#^4b80c8|iniettata]]* all'interno di un'applicazione [[027 Context e Dependency Injection|CDI]]. 

È utilizzata per semplificare la *gestione delle dipendenze* e migliorare la *modularità* delle applicazioni Java EE.

![[Immagine 2023-10-22 152917.png]]

## Utilizzo
L'annotazione `@Inject` può essere applicata ai seguenti punti:
- *Campi*: Un campo di una classe può essere marcato con `@Inject` per indicare che una dipendenza dovrebbe essere iniettata direttamente nel campo.

- *Costruttori*: Un costruttore di una classe può essere marcato con `@Inject` per specificare il punto in cui una dipendenza dovrebbe essere iniettata durante la creazione dell'oggetto. Il costruttore deve avere come parametro l'oggetto da iniettare.

- *Metodi setter*: Un metodo setter di una classe può essere marcato con `@Inject` per indicare che una dipendenza dovrebbe essere iniettata tramite il metodo setter.

> [!tip] Regola generale per `@Inject`
> Per ottenere l'istanza di una classe `X` che utilizza la `@Inject` (o qualsiasi altra annotazione CDI) al suo interno, bisogna fare l'`@Inject` della classe `X`.

In particolare, non è possibile utilizzare una classe iniettata con `@Inject` all'interno di metodi [[Keyword static|statici]] come il `main`. Quindi è molto utile notare il funzionamento di una `@Inject` all'interno di una [[013 Servlet|Servlet]].

> [!example]- <font color="orange">Esempio</font>
>```java
>import javax.inject.Inject;
>
>public class OrderService {
>	@Inject
>	private PaymentProcessor paymentProcessor;
>
>	public void processOrder(Order order) {
>		// Utilizza il paymentProcessor iniettato per elaborare l'ordine
>		paymentProcessor.processPayment(order.getTotalAmount());
>		
>		// ...
>	}
>}
>```
>
>In questo esempio, l'annotazione `@Inject` viene utilizzata per iniettare una dipendenza di tipo `PaymentProcessor`. Questo semplifica notevolmente la gestione delle dipendenze all'interno della classe `OrderService` e consente di scambiare facilmente implementazioni di `PaymentProcessor` senza dover apportare modifiche significative al codice di `OrderService`.
>
>Per ottenere `OrderService` si deve fare la sua `@Inject`.
>
>```Java
>@WebServlet(name = "NewServlet", urlPatterns = {"/NewServlet"})
>public class NewServlet extends HttpServlet {
>
>	// Inject della classe
>	@Inject
>	OrderService os;
>	
>	protected void doGet(HttpServletRequest request, HttpServletResponse response)
>			throws ServletException, IOException {
>
>		// Chiamate ai metodi
>		os.processOrder(...); 
>		// ...
>	}
>// ...
>}
>```

%%
[[000 Indice PD|↖ Ritorna all'indice ↖]]
%%