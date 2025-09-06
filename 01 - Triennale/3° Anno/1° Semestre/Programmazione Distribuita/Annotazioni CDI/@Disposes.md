---
tags:
  - corsi/informatica/programmazione_distribuita
---
L'annotazione **`@Disposes`**  è utilizzata per dichiarare un metodo che svolge il ruolo di "*disposer method*". Un disposer method è responsabile del *rilascio o della pulizia delle risorse create* da un [[@Produces|metodo produttore]] annotato con `@Produces`.

Viene chiamato quando lo [[029 Scope di un Managed Bean|scope del bean]] termina.

## Utilizzo
Un disposer method è dichiarato utilizzando l'annotazione `@Disposes` e viene utilizzato per specificare come una risorsa dovrebbe essere rilasciata o pulita quando non è più necessaria. Questo è particolarmente utile per risorse come connessioni al database, oggetti gestiti, o qualsiasi altra risorsa che richiede la liberazione esplicita delle risorse quando non sono più in uso.

> [!example]- <font color="orange">Esempio</font>
>```java
>import javax.enterprise.inject.Disposes;
>import javax.enterprise.inject.Produces;
>
>public class MyResourceProducer {
>    @Produces
>    public MyResource createMyResource() {
>        // Creazione e configurazione di una risorsa, ad esempio, una connessione al database
>        return new MyResource();
>    }
>
>    public void disposeMyResource(@Disposes MyResource resource) {
>        // Rilascio o pulizia della risorsa quando non è più necessaria
>        resource.close(); // Esempio di metodo per chiudere la connessione al database
>    }
>}
>```
>
>Il metodo `createMyResource` è annotato con `@Produces` ed è responsabile della creazione di una risorsa `MyResource`. Il metodo `disposeMyResource`, annotato con `@Disposes`, è chiamato quando la risorsa non è più necessaria e si occupa di rilasciarla o pulirla in modo appropriato.

%%
[[000 Indice PD|↖ Ritorna all'indice ↖]]
%%