---
tags:
  - corsi/informatica/programmazione_distribuita
---
È utilizzata per definire un metodo di [[030 Interceptor|Interceptor]] che circonda l'esecuzione di un metodo business. 

Quando questo tipo di interceptor è associato a un metodo business all'interno di un [[026 Managed Beans - CDI Bean|managed bean]], viene eseguito prima e dopo l'invocazione del metodo business, consentendo l'aggiunta di comportamenti o funzionalità extra senza modificare direttamente il codice del metodo business stesso.

```Java
import java.util.Date;
import javax.interceptor.AroundInvoke;
import javax.interceptor.Interceptor;
import javax.interceptor.InvocationContext;

@Interceptor
public class MyInterceptor {
	@AroundInvoke
	public Object intercettaMetodo(InvocationContext ctx) throws Exception {
		// Ottiene il nome della classe e del metodo
		String className = ctx.getMethod().getDeclaringClass().getSimpleName();
		String methodName = ctx.getMethod().getName();
		// Ottiene i parametri passati alla funzione
		MyParameter[] args = (MyParameter[]) ctx.getParameters();
		MyParameter arg0 = args[0];
		MyParameter arg1 = args[1];
		//...
		
		System.out.println("==> [" + className + "." + methodName + "] Inizio intercettazione");
		
		long start = new Date().getTime();	// Prende l'orario iniziale
		
		Object result = ctx.proceed();		// Permette al metodo di iniziare e finire
											
		long end = new Date().getTime();	// Prende l'orario finale
		
		System.out.println("==> [" + className + "." + methodName + "] Eseguito in: " + (end-start));
		
		System.out.println("-----------------------------------------------");
		
		return result; // Ritorna il risultato del metodo intercettato
	}
}
```

```markup
Output:   
==> [MyClass.metodo1] Inizio intercettazione
Dentro il metodo1
==> [MyClass.metodo1] Eseguito in: 1
-----------------------------------------------
==> [MyClass.metodo2] Inizio intercettazione
Dentro il metodo2
==> [MyClass.metodo2] Eseguito in: 2
-----------------------------------------------
```

> [!bug] Serializzazione fallita durante l'ottenimento di parametri con `InvocationContext`
> L'interceptor lancia un'eccezione quando l'oggetto parametro contiene alcuni tipi di oggetti (come `java.net.URL`). Non capisco perché.
> Una soluzione potrebbe essere quella di utilizzare come parametro un tipo primitivo (o [[008 Boxing, unboxing e autoboxing|Boxato]]) ed ottenere con [[033 Java Persistence API|JPA]] l'[[034 Entità JPA|entità]] associata.
 


Da notare che lo [[029 Scope di un Managed Bean|scope]] di questo interceptor è limitato al bean stesso (la classe target). Quindi è possibile istanziare un contatore nella classe che rimarrà nello stesso stato.

```Java
@Interceptor
public class PlayInterceptor {
	private int counter = 0;
	
	@AroundInvoke
	Object countCalls(InvocationContext ic) throws Exception {
		//...
		counter++;
		//...
	}
}
```

Invece di stampare con `sysout`, è anche possibile fare l'inject di un `Logger` da una classe `LoggerProducer` che utilizza un [[@Produces|Producer]].

> [!example]- <font color="orange">Esempio</font>
>```Java
>import java.util.logging.Logger;
>import javax.enterprise.inject.Produces;
>import javax.enterprise.inject.spi.InjectionPoint;
>
>public class LoggerProducer {
>    @Produces
>    public Logger produceLogger(InjectionPoint injectionPoint) {
>        // Il logger creato sarà associato alla classe che contiene 
>        // il punto in cui viene iniettato con:
>	   // @Inject private Logger logger;
>        return Logger.getLogger(
>			injectionPoint.getMember().getDeclaringClass().getName()
>		);
>    }
>}
>```


%%
[[000 Indice PD|↖ Ritorna all'indice ↖]]
%%