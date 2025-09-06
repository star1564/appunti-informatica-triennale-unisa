**Cosa significa la linea tratteggiata (in generale e nei class diagram)?**

In generale la linea tratteggiata indica una Dipendenza.

La linea tratteggiata può indicare in un class diagram due cose:
- Una [[UML 5 - Diagramma di Classi#Dipendenza|Dipendenza]]
- Una [[UML 5 - Diagramma di Classi#Realizzazione (Interfaccia)|Realizzazione di una Interfaccia]]

In un sequence diagram indica un [[UML 2 - Diagramma di Sequenza#^d6bd1d|messaggio asincrono]].

In un use case diagram indica le dipendenze [[UML 1 - Diagramma Caso d_Uso#Include|Include]] ed [[UML 1 - Diagramma Caso d_Uso#Extend|Extend]].

---
**Differenza tra `<<include>>` ed `<<extends>>`? Perché cambia la direzione della dipendenza? Cosa viene incluso?**

1. [[UML 1 - Diagramma Caso d_Uso#Include|Include]] - Un caso d'uso utilizza un altro caso d'uso. Viene usato per decomporre un caso d'uso complesso in molti più semplici o per riutilizzare funzionalità esistenti.
2. [[UML 1 - Diagramma Caso d_Uso#Extend|Extend]] - Un caso d'uso può utilizzare un altro caso d'uso, estendendone il suo comportamento.
3. La direzione cambia perché nella include viene incluso il caso d'uso, mentre nella extend viene esteso il caso d'uso.
4. Viene incluso il flusso di eventi

---
**Cosa sono i sequence diagram? Come sono strutturati?**
I [[UML 2 - Diagramma di Sequenza|Sequence Diagram]] sono dei diagrammi che rappresentano le interazioni tra gli oggetti in un caso d'uso. 
In esso gli attori mandano messaggi ad un [[014 Modello ad Oggetti#^fc2909|boundary object]], questi poi mandano messaggi verso i [[014 Modello ad Oggetti#^e6d6d5|control object]] che gestiscono il flusso di eventi e accedono agli [[014 Modello ad Oggetti#^7cb8de|entity object]].
I Controller e i boundary accedo agli entity (che rappresentano l'informazione persistente) ma non accade il viceversa.

La prima colonna corrisponde con l'attore che ha iniziato il caso d'uso
La seconda ai boundary Object (rappresentano l'interazione tra l'utente e il sistema)
La terza ai control object (rappresentano le operazioni eseguite dal sistema)

La barra di attivazione rappresenta il periodo di tempo durante il quale gli oggetti sono attivi o stanno eseguendo un'azione o partecipando a una sequenza di eventi, la barra di attivazione di un attore è sempre attiva.

In sintesi, la differenza fondamentale tra messaggi sincroni e asincroni sta nel fatto che i messaggi sincroni richiedono che il mittente attenda una risposta prima di continuare, mentre i messaggi asincroni consentono al mittente di continuare senza attendere una risposta immediata.

---
**Cosa sono i class diagram? Quali relazioni possono esistere tra le classi? Cosa è una associazione?**
I [[UML 5 - Diagramma di Classi|Class Diagram]] rappresentano la struttura del sistema e viene usato per modellare sia i concetti del [[002 Sistemi, Modelli e Viste#^ae7aa9|Dominio di Applicazione]] che quello delle [[002 Sistemi, Modelli e Viste#^0f8d39|Soluzioni]].
Ogni classe è composta da attributi e operazioni. Ogni attributo ha un [[Tipi, Signatures e Visibilità (IS)#^a3165d|Tipo]], mentre ogni operazione ha una [[Tipi, Signatures e Visibilità (IS)#^ecb02b|Firma]].

Le classi possono avere varie [[UML 5 - Diagramma di Classi#Relazioni tra Classi|Relazioni]] tra loro:
- L'[[UML 5 - Diagramma di Classi#Associazione|Associazione]] indica che due classi interagiscono tra di loro e si può rappresentare su di essa la molteplicità, ovvero il numero di oggetti che possono prendere parte alla relazione tra le due classi.
- L'[[UML 5 - Diagramma di Classi#Aggregazione|Aggregazione]] è una relazione che mette insieme diverse classi in una sola grande classe, dove ogni elemento può funzionare indipendentemente.
- La [[UML 5 - Diagramma di Classi#Composizione|Composizione]] è una relazione che mette insieme diverse classi in una sola grande classe, dove ogni elemento NON può funzionare indipendentemente.

---
**Cos'è la requirement elicitation? Cosa c'è nella matrice degli accessi? Legame della requirement elicitation con la matrice degli accessi?**
1. La [[009 Estrazione dei Requisiti|Requirements Elicitation]] è una fase di raccolta dei requisiti dai clienti e consiste della definizione del nuovo sistema in termini comprensibili all'utente.

2. La [[021 Attività del System Design#Controllo degli Accessi|Matrice degli Accessi]] è uno strumento utilizzato in ambienti multi-utente per definire a quali operazioni può accedere un determinato attore. Dove:
- Le righe rappresentano gli attori
- Le colonne rappresentano le classi
- Le celle rappresentano quali operazioni possono essere eseguite da un determinato attore in una determinata classe

3. Il legame tra raccolta dei requisiti e la matrice degli accessi può essere visto nel contesto della definizione dei requisiti di accesso per un sistema. Durante raccolta dei requisiti, si potrebbe chiedere al cliente, quali operazioni devono essere in grado di eseguire gli utenti, e queste informazioni potrebbero poi essere utilizzate per popolare la matrice degli accessi. Inoltre, si potrebbe anche discutere di come i requisiti di accesso possono cambiare nel tempo o in risposta a determinati eventi, il che potrebbe implicare modifiche alla matrice degli accessi.

**Come la rappresentiamo in un db? Rappresentazione e memorizzazione di una matrice degli accessi?**
Si possono avere varie implementazioni della Matrice degli Accessi:
- Tabella globale degli accessi: rappresenta esplicitamente ogni cella nella matrice come una tupla `(attore,classe,operazione)`
- Lista di controllo degli accessi: associa una lista di coppie `(attore,operazione)` a ogni classe che può essere acceduta.
- Capability: associa una coppia `(classe,operazione)` a un attore.

---
**Decomposizione sistema in sottosistemi? Quali sono i principi della decomposizione?**
La [[021 Attività del System Design#2 - Identificare i sottosistemi (decomposizione in sottosistemi)|Decomposizione in sottosistemi]] consiste nel ridurre la complessità di un sistema andando a decomporlo in parti piu piccole ([[019 System Design#Sottosistema|sottosistemi]], un'insieme di classi, associazioni e operazioni in relazione tra di loro).

Questi offrono [[027 Specifica delle Interfacce#^1c797f|Servizi]], ovvero gruppi di operazioni che trovano origine dai casi d'uso e sono specificate dalle interfacce dei sottosistemi.

**Definizione coesione, accoppiamento?**
1. La [[019 System Design#Coesione|Coesione]] è il numero di dipendenze tra le classi di un sottosistema. Alta Coesione significa che le classi nel sottosistema eseguono task simili e sono in relazione con molte classi, Bassa Coesione è il viceversa.
2. L'[[019 System Design#Coupling|Accoppiamento]] invece è il numero di dipendenze tra i sottosistemi. Alto Accoppiamento significa che i cambiamenti su un sottosistema avranno alto impatto sugli altri, Basso Accoppiamento è il viceversa.

I sottosistemi dovrebbero avere la massima coesione e il minimo accoppiamento. 

**Come organizzo la decomposizione? Tecnicamente come ottengo il layering? Come fa ad andare contro la coesione? Qual è la struttura di ogni layer? Coesione e partitioning?**

1. Il partizionamento e la stratificazione sono tecniche usate per ottenere un basso accoppiamento.
2. Uno [[019 System Design#Stratificazione|strato]] è un raggruppamento di sottosistemi gerarchico in cui ogni sottosistema in un certo livello offre dei servizi ai livelli superiori e riceve servizi dai livelli inferiori.
	- In un'[[019 System Design#^5952da|Architettura chiusa]] ogni strato può accedere solo allo strato immediatamente sottostante. 
	- In un'[[019 System Design#^a75bda|Architettura aperta]], uno strato può anche accedere a strati a livelli più profondi.
3. Le [[019 System Design#Partizionamento|partizioni]] dividono verticalmente un sistema in svariati sottosistemi indipendenti.

---
**Component e deployment diagram?** **Le componenti del component sono fisiche o logiche?**

1. Il [[UML 7 - Diagramma dei Componenti|Component Diagram]] è un grafo di componenti connesse mediante relazioni di dipendenza, e mostra le dipendenze tra componenti **fisiche** come: codice sorgente, librerie, eseguibili. Le dipendenze sono mostrare con frecce tratteggiate dal componente client al componente fornitore.

2. Il [[UML 8 - Diagramma di Deployment|Deployment Diagram]] è un grafo di nodi connessi mediante associazioni di comunicazione e mostra come le componenti di un sistema sono distribuiti su diversi dispositivi fisici (nodi). I nodi vengono mostrati come box 3D e possono contenere istanze di componenti.

---
**[[025 Design Pattern|Design Pattern]]**

---
**Cosa sono i contratti?**
I [[Contratti]] sono vincoli su una classe che consentono agli sviluppatori di condividere le stesse ipotesi sulla classe.

Esistono tre tipi di contratti:
- [[Contratti#^fba292|Invariante]]: un predicato che è sempre vero per tutte le istanze di una classe. Sono vincoli associati a classi o interfacce e vengono utilizzati per specificare vincoli di coerenza tra gli attributi della classe.

- [[Contratti#^e78e08|Precondizione]]: un predicato che deve essere vero prima che venga richiamata un'operazione. Sono associate a un'operazione specifica e vengono utilizzate per specificare i vincoli che un chiamante deve soddisfare prima di chiamare un'operazione.

- [[Contratti#^438049|Postcondizione]]: un predicato che deve essere vero dopo che è stata invocata un'operazione. Sono associate a un'operazione specifica e vengono utilizzate per specificare i vincoli che l'oggetto deve garantire dopo l'invocazione dell'operazione.

**Cosa è l'OCL?**
L'[[026 Object Constraint Language|OCL]] è un linguaggio non procedurale che consente di specificare formalmente i vincoli su singoli elementi del modello (contratti).
Non può essere usato per denotare il flusso di controllo.

Un vincolo viene espresso con un'espressione OCL che ritorna vero o falso. Può essere rappresentato come una nota allegata ai modelli UML.

---
**Testing random e testing sistematico? Perche devo scegliere il primo e non il secondo?**
1. Il test random si basa su:
	- Distribuzione uniforme dello spazio di input
	- Considerare tutti gli input ugualmente importanti
	- Non va bene siccome la distribuzione dei difetti non è uniforme
2. Il test sistematico si basa su:
	- Seleziona l'input piu importante
	- Di solito si selezionano rappresentanti di classi di input
	- Lo spazio di input è suddiviso in classi
3. Perche i dati sono uniformi e quindi scelgo randomicamente

**Testing BlackBox e WhiteBox? In cosa differiscono le tecniche del primo dal secondo?**
Entrambi sono tipi di testing ottenuti mediante analisi dinamica:
- [[037 Concetti di Testing#Blackbox|Black-box]] (funzionale), la selezione dei test è basata sulla specifica (scalabile a qualsiasi livello di test)
	- Il test funzionale utilizza la specifica per suddividere lo spazio di input in classi.
	- Dividere i dati di input in classi di equivalenza e scegliere test case per ciascuna classe di equivalenza
	- Testare ogni classe e i confini tra le classi.
- [[037 Concetti di Testing#Whitebox|White-box]] (strutturale), la selezione dei test è basata sulla logica interna del programma (NON scalabile, solo a livello di unità)
	- Si focalizza sulla completezza (copertura).
	- Ogni stato nel modello dinamico dell'oggetto e ogni interazione tra gli oggetti viene testata.

**WhiteBox Testing**
- Statement testing: testare singoli statement
- Loop Testing:
	- Causa di saltare completamente l'esecuzione del ciclo. (Eccezione:Ripeti i cicli)
	- Ciclo da eseguire esattamente una volta
	- Ciclo da eseguire più di una volta

- Path Testing: Assicurarsi che tutti i percorsi nel programma vengano eseguiti
- Branch Testing (Test condizionale): assicurarsi che ogni possibile risultato di una condizione venga testato almeno una volta

**Una volta decisi i cammini da eseguire, cosa devo decidere?**
Gli input per percorrere i cammini.

**Black-Box cosa utilizza per selezionare i casi di test?**
La specifica del software siccome non si conosce il codice.

**Cosa mi da l'oracolo nel BB e nel WB?**
Nel BB lo da la specifica del software mentre nel WB la specifica del codice

**Cosa mi permette di ottenere WB che non mi dà il BB?**
Il White-Box Testing spesso testa ciò che è stato fatto, invece di ciò che dovrebbe essere fatto. Tuttavia, non individua i casi d'uso mancanti. Il Black-Box Testing ha la peculiarità di essere una potenziale esplosione combinatoria di casi di test. Spesso, però, non è chiaro se i casi di test selezionati scoprono particolari errori o meno e non scopre casi d'uso estranei.

---
**Equivalence class testing?**
La tecnica dell'[[040 Unit Testing#Equivalence Testing|Equivalence Testing]] di test blackbox riduce al minimo il numero di casi di test. I possibili input sono suddivisi in classi di equivalenza e per ciascuna classe viene selezionato un caso di test.

Il presupposto del test di equivalenza è che i sistemi solitamente si comportano in modo simile per tutti i membri di una classe. Per testare il comportamento associato a una classe di equivalenza, dobbiamo testarne solo un membro della classe. Il test di equivalenza consiste di due fasi:
- identificazione delle classi di equivalenza
- selezione degli input del test

I seguenti criteri vengono utilizzati per determinare le classi di equivalenza.

- Copertura: Ogni possibile input appartiene a una delle classi di equivalenza.
- Disgiunzione: Nessun input appartiene a più di una classe di equivalenza.
- Rappresentanza: Se l'esecuzione dimostra uno stato errato quando un particolare membro di una classe di equivalenza viene utilizzato come input, lo stesso stato errato può essere rilevato utilizzando qualsiasi altro membro della classe come input.

Per ciascuna classe di equivalenza vengono selezionati almeno due dati: 
- un input tipico, che esercita il caso comune 
- un input non valido, che esercita la capacità di gestione delle eccezioni del componente. 

Dopo che tutte le classi di equivalenza sono state identificate, è necessario identificare un input di test per ciascuna classe che copra la classe di equivalenza.

Se c'è la possibilità che non tutti gli elementi della classe di equivalenza sono coperti dall'input del test, la classe di equivalenza deve essere suddivisi in classi di equivalenza più piccole e gli input di test devono essere identificati per ciascuno delle nuove classi.

**Per cosa uso le precondizioni?**
Individuo le classi valide, le non valide comunque le uso per il test di robustezza

**Cosa uso per partizionare il dominio delle classi di equivalenza?**
La postcondizione

**Cosa partizioni in classe di equivalenza?**
Per ogni parametro seleziono degli elementi

**Weak e Strong equivalence testing?**
Weak: Scegliere un valore variabile da ciascuna classe di equivalenza
Strong: Prodotto cartesiano dei sottoinsiemi di partizione

**Da cosa mi è dato il numero di casi di test minimo?**
In weak è uguale al numero di classi di test, in strong è uguale alla combinazione delle classi di test

**Problemi equivalence class testing?**
ECT è appropriato quando i dati di input sono definiti in termini di intervalli e insiemi di valori discreti, e siccome parte dal presupposto che il comportamento del programma sia “simile”, non si concentra su alcuni tipici errori che si trovano al limite tra classi diversi

**Boundary Testing**
Il [[040 Unit Testing#Boundary Testing|Boundary Testing]] è caso speciale di test di equivalenza che si concentra sulle condizioni al confine delle classi di equivalenza. Invece di selezionare qualsiasi elemento nella classe di equivalenza, il testing dei confini richiede che gli elementi siano selezionati dai “margini” della classe di equivalenza.

Quindi il minimo, un po piu del minimo, un valore nominale, un po meno del massimo, il massimo.

Il test set si ottiene fissando una variabile al suo valore nominale e facendo variare le altre per tutti i valori possibili. Questo per ogni variabile. Per una funzione con n variabili richiede 4n+1 test case.

**Worst Case Testing**
Prodotto cartesiano di {min,min+,nom,max-.max}, è una tecnica piu completa dell'analisi dei Boundary Value, ma molto piu costosa, 5^n casi di test.

**Category Partititon: Steps**
Il sistema è suddiviso in singole funzioni che possono essere testate in modo indipendente

Il metodo individua i *parametri* di ciascuna “funzione” e, per ciascun parametro, identifica *categorie* distinte

Oltre ai parametri si possono considerare anche gli *oggetti dell'ambiente*

Le categorie sono proprietà o caratteristiche principali

Le categorie vengono ulteriormente suddivise in *scelte* analogamente a come viene applicato il partizionamento per equivalenza (possibili “valori”)

Vengono poi individuati i *vincoli* che operano tra le scelte, cioè, come il verificarsi di una scelta può influenzare l'esistenza di un'altra

Vengono generati i *Test Frame* che consistono nelle combinazioni consentite di scelte nelle categorie

I *Test Frame* vengono quindi convertiti in dati di prova

---
**Testing d'integrazione e strategie?**
Il [[041 Integration Testing|test di integrazione]] rileva bug che non sono stati determinati durante il Test di Unità, focalizzando l'attenzione su un insieme di componenti che vengono integrate. 

Le strategie di test di integrazione si dividono in:
- orizzontale: in cui i componenti sono integrati in base ai livelli
- verticale: in cui i componenti sono integrati in base alle funzioni.


Sono stati ideati diversi approcci per implementare una strategia di test di integrazione orizzontale: test big bang, test bottom-up, test top-down, test sandwich, test sandwich modificata. 

Ciascuna di queste strategie sono state originariamente ideate partendo dal presupposto che la scomposizione del sistema sia gerarchica e che ciascuno dei componenti appartenga a strati gerarchici ordinati rispetto all'associazione “Call”.

**BIG-BANG**
La strategia del test [[041 Integration Testing#Big Bang Integration Testing|big bang]] presuppone che tutti i componenti vengano prima testati individualmente e poi testati insieme come un unico sistema. Il vantaggio è che non sono necessari stub o driver di test aggiuntivi. Anche se questa strategia sembra semplice, il big bang testing è costoso: se un test scopre un guasto, è impossibile distinguere i guasti nell'interfaccia dai guasti all'interno di un componente.

**BOTTOM-UP**
La strategia di test [[041 Integration Testing#Bottom-Up Integration Testing|bottom-up]] testa innanzitutto ciascun componente dello strato inferiore individualmente, quindi li integra con i componenti dello strato successivo. Questo viene ripetuto finché tutti i componenti di tutti gli strati non vengono combinati. I test driver vengono utilizzati per simulare i componenti degli strati superiori che non sono ancora stati integrati. Si noti che non sono necessari [[037 Concetti di Testing#Test Stub|stub di test]] durante i test bottom-up.
- **vantaggi:** Il vantaggio del test bottom-up è che i guasti dell'interfaccia possono essere individuati più facilmente
- **svantaggi:** Lo svantaggio del test bottom-up è che testa i sottosistemi più importanti, vale a dire i componenti dell'interfaccia utente, per ultimi.


**TOP-DOWN**
[[041 Integration Testing#Top-Down Integration Testing|Top-down]] testa prima i componenti dello strato superiore, quindi integra i componenti dello strato successivo. Quando tutti i componenti del nuovo livello sono stati testati insieme, viene selezionato il livello successivo. Test stub vengono utilizzati per simulare i componenti degli strati inferiori che non sono ancora stati integrati. Nota che i [[037 Concetti di Testing#Test Driver|test driver]] non sono necessari durante i test top-down.
- **vantaggi:** Il vantaggio del test top-down è che inizia con i componenti dell'interfaccia utente.
- **svantaggi:** Lo svantaggio dei test top-down è che lo sviluppo degli stub di test richiede molto tempo ed è soggetto a errori.

**SANDWICH**

La [[041 Integration Testing#Sandwich Testing|Sandwich]] combina l'uso di strategie top-down e bottom-up. Il sistema è visto come se avesse 3 strati:
- un livello target nel mezzo
- uno sopra il target
- uno sotto il target

I test top-down e bottom-up possono ora essere eseguiti in parallelo.

Il test di integrazione top-down viene eseguito testando il livello superiore in modo incrementale con i componenti del livello target, mentre il test bottom-up viene utilizzato per testare il livello inferiore in modo incrementale con i componenti del livello target.

Consente il test anticipato dei componenti dell'interfaccia utente.

- **PROBLEMA** C'è un problema con il test sandwich: non testa a fondo i singoli componenti dello strato target prima dell'integrazione.

**SANDWICH MODIFICATA**
La strategia di [[041 Integration Testing#^2ba926|test Sandwich Modificata]] testa i tre strati individualmente prima di combinarli tra loro in test incrementali.

I test dei singoli livelli consistono in un gruppo di tre prove:
- un test del livello superiore con stub per il livello target.
- un test del livello target con driver e stub che sostituiscono i livelli superiore e inferiore
- un test dello strato inferiore con un driver per lo strato target.

I test a strati combinati consistono in due test:
- Il livello superiore accede al livello target. Sostituendo i driver con componenti del livello superiore.
- Al livello inferiore si accede dal livello target. Sostituendo lo stub con i componenti del livello inferiore.

**Perche il sandwich è utile?**
Permette di unire i vantaggi di bottom up e top down, quindi testare facilmente le interfacce grafiche e subito.