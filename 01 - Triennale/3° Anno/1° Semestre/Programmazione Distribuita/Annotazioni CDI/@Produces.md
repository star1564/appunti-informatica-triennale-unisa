---
tags:
  - corsi/informatica/programmazione_distribuita
cssclasses:
  - 
---
È possibile far iniettare un [[026 Managed Beans - CDI Bean|CDI Bean]] in un altro CDI Bean, dato che sono entrambi gestiti dal [[021 Container Java EE|container]]. Però non è possibile far iniettare una classe di tipo `java.util.Date`, `java.lang.String` o `java.util.logging.Logger`, perché queste classi si trovano nel file *non modificabile* `rt.jar` (java runtime environment classes) che *non contiene* il file `beans.xml`.

> Un **Producer** serve per *produrre una qualsiasi classe* da far iniettare.

![[Immagine 2023-10-22 165752.png]]

Le risorse create da un `@Producer` da chiudere possono essere chiuse attraverso [[@Disposes]].
## Utilizzo

L'annotazione **`@Produces`** è utilizzata per dichiarare un metodo come un produttore di oggetti gestiti CDI. Questo significa che il metodo annotato con `@Produces` è responsabile della creazione e della fornitura di istanze di un bean specifico.

L'annotazione `@Produces` viene applicata a un metodo che restituisce un'istanza di un tipo bean CDI specifico. Questo metodo agisce come un "factory method" per la creazione del bean.

```java
import javax.enterprise.inject.Produces;

public class NonCDIBeanProducer {
    @Produces @Qualifier
    public NonCDIBean createBean() {
        // Creazione e configurazione di un'istanza di MyBean
        return new NonCDIBean();
    }
}
```

Il metodo `createMyBean` è annotato con `@Produces` e restituisce un'istanza di `MyBean`. Questo indica al CDI container che questo metodo è responsabile della creazione di un'istanza di `MyBean` quando è richiesta una dipendenza di tipo `MyBean`.

> [!example]- <font color="orange">Esempio con i [[@Qualifiers]]</font>
>```Java
>public class MyClass {
>	@Produces @MyString
>	private String str = "this is a string"; 
>	// Questa stringa può essere iniettata con @Inject @MyString
>
>	@Produces @MyInteger
>	private int editorNumber = 1234; 
>	// Questo intero può essere iniettato con @Inject @MyInteger
>
>	@Produces @MyRandomMethod
>	public double random() {
>		// Il risultato del metodo può essere iniettato con @Inject @MyRandomMethod
>		return Math.abs(new Random().nextInt()); 
>	}
>}
>```
>
>Utilizzo:
>```Java
>@Inject @MyString
>private String str;
>
>@Inject @MyInteger
>private int i;
>
>@Inject @MyRandomMethod
>private double rnd;
>```

> [!example]- <font color="orange">Esempio solo con @Produces</font>
>```Java
>import java.util.logging.Logger;
>import javax.enterprise.inject.Produces;
>import javax.enterprise.inject.spi.InjectionPoint;
>
>public class LoggerProducer {
>    @Produces
>    public Logger produceLogger(InjectionPoint injectionPoint) {
>        // Il logger creato sarà associato alla classe che contiene 
>        // il punto in cui viene iniettato.
>        return Logger.getLogger(
>			injectionPoint.getMember().getDeclaringClass().getName()
>	   );
>    }
>}
>```
>Utilizzo:
>```Java
>@Inject 
>private Logger logger;
>```

%%
[[000 Indice PD|↖ Ritorna all'indice ↖]]
%%