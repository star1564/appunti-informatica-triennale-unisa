---
aliases:
  - Componente di un GameObject
  - Componenti di un GameObject
  - Prefab GameObject
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Unity
cssclasses:
  - 
---
>I **GameObject** sono gli oggetti fondamentali di Unity che *rappresentano tutti i tipi di oggetti* che si trovano all'interno di una [[U_001 Unity#Scene|Scena]], come personaggi, oggetti di scena, luci, telecamere, effetti sonori ed altro ancora.

![[Pasted image 20240415174218.png]]

> [!tip] Creazione
>  `(Tasto destro su uno spazio vuoto dell'Hierarchy) ➡️ (Selezionare cosa si vuole creare)`

Non realizzano molto di per sé, ma fungono da *contenitori per i Componenti*.

Possono essere anche essere *disattivati* per rimuoverli temporaneamente dalla scena attraverso l'Inspector.

![[Pasted image 20240415180612.png]]

Se un GameObject non si muove in fase di esecuzione, è noto come GameObject *Statico*.

![[Pasted image 20240415182651.png]]

## Componenti dei GameObject
>I **Componenti** sono i pezzi funzionali di ogni GameObject. Infatti, *forniscono funzionalità ai GameObject*.

Contengono **proprietà** che possono essere modificate per *definire il comportamento* di un GameObject.

> [!tip] Vedere i componenti di un GameObject
> Per visualizzare un elenco dei componenti collegati a un GameObject nella *finestra Inspector*, selezionare un GameObject nella finestra Hierarchy o nella vista Scena.
> Da qui si possono anche *aggiungere nuovi componenti e rimuovere/modificare quelli esistenti*.

È possibile collegare *molti componenti a un GameObject*. 

Unity dispone di molti tipi di componenti incorporati, ma è anche possibile *crearne di propri* utilizzando l'[[U_007 Unity Scripting|API Unity Scripting]].

### Transform
Ogni GameObject deve avere esattamente un solo componente **Transform**. Questo perché il componente Transform determina la *posizione*, la *rotazione* e la *scala* dell'oggetto di gioco. 

![[Pasted image 20240415175508.png]]

Per dare a un GameObject le proprietà di cui ha bisogno per diventare una luce, un albero o una telecamera, è necessario *aggiungergli dei componenti*. 
A seconda del tipo di oggetto che si vuole creare, si aggiungono diverse combinazioni di componenti a un GameObject.

Nella vista Scena, è possibile utilizzare gli *strumenti* Sposta, Ruota e Scala per modificare le trasformazioni.

![[Pasted image 20240416084140.png]]



## Parenting
>Unity utilizza il concetto di *gerarchie genitore-figlio*, o **Parenting**, per raggruppare gli oggetti di gioco. Un oggetto *può contenere altri* GameObject che ne *ereditano le proprietà*. 

È possibile collegare tra loro i GameObject per spostare, scalare o trasformare un insieme di GameObject. Quando si sposta l'oggetto di livello superiore, o GameObject genitore, si spostano *anche* tutti i GameObject figli.

![[Pasted image 20240415180759.png]]

È inoltre possibile creare GameObject padre-figlio annidati. Tutti gli oggetti annidati sono comunque discendenti del GameObject genitore originale, o GameObject radice.

## Prefab
>Il sistema **Prefab** di Unity consente di creare, configurare e memorizzare un GameObject completo di tutti i suoi componenti, i valori delle proprietà e i GameObject figli *come risorsa riutilizzabile*. 

Quando si desidera riutilizzare un oggetto di gioco configurato in modo particolare, come un personaggio non giocante (NPC), un oggetto di scena o un pezzo di scenario, in più punti della scena o in più scene del progetto, è necessario convertirlo in un prefab. 

![[Pasted image 20240415200859.png]]

Tutte le modifiche apportate ad un prefab *si riflettono automaticamente nelle istanze* di quel prefab, consentendo di apportare facilmente modifiche di ampia portata all'intero progetto senza dover ripetere la stessa modifica a ogni copia dell'attività.

___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [Manuale - GameObject](https://docs.unity3d.com/Manual/GameObjects.html)
- [Manuale - Transform](https://docs.unity3d.com/Manual/class-Transform.html)
- [Manuale - Componenti](https://docs.unity3d.com/Manual/Components.html)
- [Manuale - Utilizzo Componenti](https://docs.unity3d.com/Manual/UsingComponents.html)
- [Manuale - Prefabs](https://docs.unity3d.com/Manual/Prefabs.html)
- [Manuale - Parenting](https://docs.unity3d.com/Manual/Hierarchy.html)