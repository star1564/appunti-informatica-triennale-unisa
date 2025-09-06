---
tags:
  - corsi/informatica/programmazione_distribuita
cssclasses:
  - 
---

## Basilari

| Link               | Annotazione          | Descrizione                                                                                                  | Dove può essere applicata         |
| ------------------ | -------------------- | ------------------------------------------------------------------------------------------------------------ | --------------------------------- |
| [[@Inject\|→]]     | `@Inject`            | Utilizzata per l'iniezione delle dipendenze. Marca il punto in cui un componente deve essere iniettato.      | Campi, costruttori, metodi setter |
| [[@Qualifiers\|→]] | `@Qualifiers`        | Utilizzata per definire criteri di qualificazione personalizzati per l'iniezione di dipendenze.              | Annotazioni personalizzate        |
| [[@Default\|→]]    | `@Default`           | Indica il bean predefinito quando non è specificata una qualificazione personalizzata.                       | Classi                            |
| [[@Produces\|→]]            | `@Produces`          | Utilizzata per dichiarare un metodo (o un oggetto) come produttore di un oggetto gestito CDI.                               | Metodi, campi                            |
| [[@Disposes\|→]] | `@Disposes`          | Utilizzata per dichiarare un metodo che rilascia una risorsa precedentemente prodotta da `@Produces`.        | Parametri di metodo       |


## Interceptor

| Link                                           | Annotazione           | Descrizione                                                                                                  | Dove può essere applicata |
| ---------------------------------------------- | --------------------- | ------------------------------------------------------------------------------------------------------------ | ------------------------- |
| [[030 Interceptor#Creare un Interceptor\|→]]   | `@Interceptor`        | Utilizzata per dichiarare un interceptor CDI, che consente di applicare logica trasversale a metodi di bean. | Classi                    |
| [[030 Interceptor#Abbinare un Interceptor\|→]] | `@Interceptors`       | Permette di far utilizzare ad un'intera classe o metodo un particolare interceptor.                          | Classi, Metodi            |
| [[030 Interceptor#Abbinare un Interceptor\|→]] | `@InterceptorBinding` | Alternativa ad `@Interceptors` che permette di specificare una annotazione personalizzata attraverso il binding.                   | Classi                    |
| [[@AroundInvoke\|→]]                           | `@AroundInvoke`       | Utilizzata per intercettare un metodo durante la sua esecuzione.                                             | Metodi                    |
| x                                              | `@AroundConstruct`    | Utilizzata per intercettare la costruzione di un'istanza di un bean.                                         | Costruttori               |
| [[030 Interceptor#Abbinare un Interceptor\|→]] | `@Priority`    | Utilizzata per indicare la priorità di un interceptor se si utilizza il binding. Minore è il numero, maggiore è la priorità.                                         | Classi               |

## Decorator ed Eventi
| Link             | Annotazione          | Descrizione                                                                                                  | Dove può essere applicata |
| ---------------- | -------------------- | ------------------------------------------------------------------------------------------------------------ | ------------------------- |
| [[031 Decorators\|→]]          | `@Decorator`         | Utilizzata per dichiarare un decorator CDI, che può decorare un bean esistente con funzionalità aggiuntive.  | Classi                    |
| [[032 Eventi (CDI)\|→]]          | `@Observes`       | Agisce come osservatore di eventi, reagendo alle notifiche di eventi pubblicati nel sistema.              | Metodi, Parametri di metodo                    |

## Scope

| Scope    | Annotazione          | Descrizione                                                                            | Dove può essere applicata |
| ------- | -------------------- | -------------------------------------------------------------------------------------- | ------------------------- |
| Request | `@RequestScoped`     | Definisce il ciclo di vita del componente come lo scope della singola richiesta HTTP.          | Classi                    |
| Session | `@SessionScoped`     | Definisce il ciclo di vita del componente come lo scope della sessione HTTP.           | Classi                    |
| Application | `@ApplicationScoped` | Definisce il ciclo di vita del componente come lo scope dell'applicazione.             | Classi                    |
| Dependent | `@Dependent`         | Definisce il ciclo di vita del componente come dipendente dalla durata dell'iniezione. | Classi, campi             |
| Conversation | `@ConversationScoped` | Definisce il ciclo di vita del componente come lo scope di conversazione, che si estende all'interno di sessioni con punti di partenza e fine identificate dall'applicazione. | Classi |

## Callback Ciclo di Vita di un'Entità
Si possono applicare solamente sui metodi.

|  Annotation         |  Descrizione                                                                                                                                                                                                                                                                              |
|:--------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  `@PrePersist`      |  Contrassegna un metodo da invocare prima che venga eseguito `EntityManager.persist()`.                                                                                                                                                                                                   |
|  `@PostPersist`     |  Contrassegna un metodo da invocare dopo che l'entità è stata persistita. Se l'entità genera automaticamente la sua chiave primaria (con `@GeneratedValue`), il valore è disponibile nel metodo.                                                                                          |
|  `@PreUpdate`       |  Contrassegna un metodo da invocare prima che venga eseguita un'operazione di aggiornamento del database (chiamando i setter dell'entità o il metodo `EntityManager.merge()`).                                                                                                            |
|  `@PostUpdate`      |  Contrassegna un metodo da invocare dopo che è stata eseguita un'operazione di aggiornamento del database.                                                                                                                                                                                |
|  `@PreRemove`       |  Contrassegna un metodo da invocare prima che venga eseguito `EntityManager.remove()`.                                                                                                                                                                                                    |
|  `@PostRemove`      |  Contrassegna un metodo da invocare dopo che l'entità è stata rimossa.                                                                                                                                                                                                                    |
|  `@PostLoad`        |  Contrassegna un metodo da invocare dopo che un'entità è stata caricata (con una query JPQL o un `EntityManager.find()`) o ricaricata dal database sottostante. Non esiste l'annotazione `@PreLoad`, poiché non ha senso precaricare dati su un'entità che non è ancora stata costruita.  |  

## Altri

| Annotazione         | Descrizione                                                                                               | Dove può essere applicata |
| ------------------- | --------------------------------------------------------------------------------------------------------- | ------------------------- |
| `@RequestParameter` | Utilizzata per l'iniezione di parametri di richiesta HTTP in un bean CDI.                                 | Parametri di metodo       |
| `@Alternative`      | Utilizzata per dichiarare un bean alternativo, che può essere utilizzato al posto di un bean predefinito. | Classi                    |
| `@Specializes`      | Utilizzata per dichiarare un bean specializzato che eredita dal comportamento di un altro bean.           | Classi                    |

