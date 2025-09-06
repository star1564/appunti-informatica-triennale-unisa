---
tags:
  - corsi/informatica/programmazione_distribuita
cssclasses:
  - 
---
>L'annotazione **`@Default`** rappresenta il *[[041 Enterprise Java Beans|bean]] predefinito* o la selezione predefinita quando non è specificata alcuna qualificazione personalizzata per un'[[027 Context e Dependency Injection#^4b80c8|iniezione di dipendenza]]. 

In altre parole, se una dipendenza non è annotata con una qualificazione personalizzata, il CDI container utilizzerà il bean contrassegnato con `@Default` come valore predefinito.

![[Immagine 2023-10-22 154801.png]]

## Utilizzo
L'annotazione `@Default` può essere applicata a: 
- *Classi*;
- *Interfacce*;
- *Metodi*. 

> [!example]- <font color="orange">Esempio</font>
>IsbnGenerator.java:
>```java
>import javax.inject.Default;
>
>@Default
>public class IsbnGenerator implements INumberGenerator {
>    // ...
>}
>```
>La `@Inject` preferirà l'implementazione che ha `@Default`.


%%
[[000 Indice PD|↖ Ritorna all'indice ↖]]
%%