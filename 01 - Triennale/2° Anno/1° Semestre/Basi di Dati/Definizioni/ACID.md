---
tags:
  - corsi/informatica/basi_di_dati
---
>**ACID** si riferisce alle quattro proprietà che definiscono una [[046 Transazioni EJB#^991e46|transazione]] affidabile: Atomicità, Consistenza, Isolamento e Durabilità. 
>
>| Proprietà      | Descrizione                                                                                                                                                                                                                                                                                        |
>| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
>| **Atomicità**  | Una transazione è composta da una o più operazioni raggruppate in un'unità di lavoro. Alla conclusione della transazione, queste operazioni vengono eseguite tutte con successo (commit) o nessuna di esse viene eseguita affatto (rollback) se si verifica qualcosa di inatteso o irrecuperabile. |
>| **Coerenza**   | Alla conclusione della transazione, i dati vengono lasciati in uno stato coerente.                                                                                                                                                                                                                 |
>| **Isolamento** | Lo stato intermedio di una transazione non è visibile alle applicazioni esterne.                                                                                                                                                                                                                   |
>| **Durabilità** | Una volta che la transazione è stata confermata, le modifiche apportate ai dati sono visibili alle altre applicazioni.                                                                                                                                                                             |


> [!example]- <font color="orange">Esempio</font>
>Considerare un trasferimento bancario: è necessario addebitare il conto di risparmio per accreditare il conto corrente.
>
>Quando si trasferisce denaro da un conto all'altro, si può immaginare una sequenza di accessi al database: il conto di risparmio viene addebitato con un'istruzione [[024 Comandi SQL#UPDATE|SQL update]], il conto corrente viene accreditato con un'altra istruzione di aggiornamento e viene creato un registro in un'altra tabella per tenere traccia del trasferimento. 
>
>Queste operazioni devono essere eseguite nella stessa unità di lavoro (*Atomicità*), perché non si vuole che si verifichi l'addebito ma non l'accredito. Dal punto di vista di un'applicazione esterna che interroga i conti, solo quando entrambe le operazioni sono state eseguite con successo sono visibili (*Isolamento*).
>
>Con l'isolamento, l'applicazione esterna non può vedere lo stato intermedio quando un conto è stato addebitato e l'altro non è ancora accreditato (se potesse, penserebbe che l'utente ha meno soldi di quanti ne abbia in realtà). 
>
>La *Coerenza* si ha quando le operazioni di transazione (con un commit o un rollback) vengono eseguite all'interno dei vincoli del database (come [[ER5_ Attributi Chiave|Chiavi Primarie]], [[ER7_Tipi Relazioni|Relazioni]] o [[ER3_Attributo|Attributi]]). 
>
>Una volta completato il trasferimento, i dati possono essere consultati da altre applicazioni (*Durabilità*).


%%[[000 Indice BD|↖ Ritorna all'indice ↖]]%%