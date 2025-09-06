---
tags:
  - corsi/informatica/programmazione_distribuita
cssclasses:
  - 
---
>Le annotazioni di **qualificazione** (**`@Qualifiers`**) sono uno strumento *per risolvere il problema dell'ambiguità* quando si iniettano dipendenze dello stesso tipo. Queste annotazioni permettono di distinguere tra diverse *implementazioni dello stesso tipo di bean* e specificare quale implementazione deve essere iniettata in un punto di iniezione.

![[Immagine 2023-10-22 154444.png]]

## Utilizzo
Le annotazioni di qualificazione sono definite come *annotazioni personalizzate* che vengono applicate ai bean per contraddistinguerli in base a criteri specifici.

È possibile avere anche dei **Qualificatori con Parametri**.

> [!example]- <font color="orange">Esempio</font>
>È possibile creare annotazioni di qualificazione personalizzate come per marcare le implementazioni dei bean in modo diverso.
>
>```java
>import java.lang.annotation.Retention;
>import java.lang.annotation.Target;
>import javax.inject.Qualifier;
>import static java.lang.annotation.ElementType.*;
>import static java.lang.annotation.RetentionPolicy.RUNTIME;
>
>@Qualifier
>@Retention(RUNTIME)              // Le annotazioni sono nel file .class e vengono lette al runtime
>@Target({FIELD, TYPE, METHOD})   // A cosa si può applicare
>public @interface ThirteenDigits {} // Nome dell'interfaccia
>```
>
>```java
>import java.lang.annotation.Retention;
>import java.lang.annotation.Target;
>import javax.inject.Qualifier;
>import static java.lang.annotation.ElementType.*;
>import static java.lang.annotation.RetentionPolicy.RUNTIME;
>
>@Qualifier
>@Retention(RUNTIME)              // Le annotazioni sono nel file .class e vengono lette al runtime
>@Target({FIELD, TYPE, METHOD})   // A cosa si può applicare
>public @interface EightDigits {} // Nome dell'interfaccia
>```
>
>Una volta definita, un'annotazione di qualificazione, può essere applicata a implementazioni specifiche dei bean per indicare la loro distinzione:
>
>```java
>@ThirteenDigits
>public class IsbnGenerator implements NumberGenerator {
>    // Implementazione del bean
>}
>```
>
>```java
>@EightDigits
>public class IssnGenerator implements NumberGenerator {
>    // Implementazione del bean
>}
>```
>
>Ora è possibile iniettare il bean con l'annotazione di qualificazione in un punto di iniezione appropriato.
>
>```java
>import javax.inject.Inject;
>
>public class BookService {
>	@Inject @ThirteenDigits
>	private NumberGenerator ng;
>
>	public Book createBook(/*...*/){
>		// ...
>	}
>}
>```
>
>```java
>import javax.inject.Inject;
>
>public class LegacyBookService {
>	@Inject @EightDigits
>	private NumberGenerator ng;
>
>	public Book createBook(/*...*/){
>		// ...
>	}
>}
>```
>
>Per il `BookService` farà l'iniezione di `IsbnGenerator`, mentre per `LegacyBookService` farà l'iniezione di `IssnGenerator`.

%%
[[000 Indice PD|↖ Ritorna all'indice ↖]]
%%