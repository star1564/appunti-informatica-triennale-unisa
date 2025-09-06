---
aliases:
  - Rigidbody
  - Collider
  - Trigger
  - Physic Material
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: 
cssclasses:
  - 
---
## Rigidbody
>Un **Rigidbody** è il [[U_002 GameObject#Componenti dei GameObject|componente]] principale che *abilita la fisica* di un GameObject. Con un Rigidbody collegato, l'oggetto risponde immediatamente alla *gravità*.

## Collider
>I componenti **Collider** definiscono la *forma di un oggetto* ai fini delle collisioni fisiche.

Se vengono aggiunti uno o più componenti Collider, l'oggetto di gioco viene spostato dalle collisioni in arrivo.

Quando viene applicato a un oggetto, *non* si dovrebbe cercare di spostarlo da uno script modificando le proprietà `Transform`. Si devono invece applicare delle forze per spingere l'oggetto di gioco e lasciare che il motore fisico calcoli i risultati.

Esistono tre tipi di collider:
- **Primitive Collider** - Gli oggetti primitivi sono usati per approssimare la forma dell'oggetto

![[Pasted image 20240418173434.png]]

> [!warning] Primitive Collider già esistente per i Primitive
> Quando si crea un GameObject primitivo (cubo, sfera, ecc...) viene già applicato un Primitive Collider.

- **Mesh Collider** - Le [[002 Modelli 3D Poligonali|Mesh]] sono usate come collider per l'oggetto
- **Compound Collider** - Le primitive sono composte in modo da simulare la forma degli oggetti nel modo più affidabile possibile


> [!info]- Kinematic
>Ci sono situazioni in cui si preferisce "bypassare" le simulazioni fisiche, cercando di essere coerenti con il resto della scena anche utilizzando operazioni di traslazione/rotazione.
>
>In questi casi, Unity mette a disposizione l'opzione `is Kinematic`. È possibile spostare un oggetto rigidbody cinematico da uno script modificando il suo componente `Transform`, ma *non risponderà alle collisioni e alle forze* come un rigidbody non cinematico.

### Eventi delle collisioni
È possibile utilizzare i metodi seguenti per gestire gli avvenimenti in caso di una collisione:
- ***`OnCollisionEnter(Collision collision){...}`***
- ...

> [!example]- <font color="orange">Esempio</font>
>```CS
>private void OnCollisionEnter(Collision collision)
>{
>    // Controlla se il nome dell'oggetto che ha avuto una collisione
>    // con questo oggetto ha un nome specifico
>    if(collision.collider.name == "Un nome")
>    {
>        // Fai qualcosa
>    }
>}
>```
>
>---
>Quando si ha una situazione del genere con il [[U_002 GameObject#Parenting|parenting dei GameObject]], dove: 
>- Il parent ha un [[#Rigidbody|Rigidbody]] ed uno script per le collisioni
>	- Un children ha un [[#Collider|Collider]] 
>	- Altri children hanno dei Collider
>
>![[Pasted image 20240605110801.png]]
>
>È possibile verificare quale dei figli del parent ha avuto una collisione, attraverso:
>
>```CS
>// Nello script del parent
>private void OnCollisionEnter(Collision collision)
>{
>    Collider childCollider = collision.GetContact(0).thisCollider;
>    // GetContacts ritorna un array di contatti
>    
>    Debug.Log(childCollider.name); // Utilizza il nome
>}
>```


## Trigger
>I **Trigger** sono una versione speciale di `Collider`, dedicati alla creazione di oggetti *non tangibili*, di cui però si vogliono comunque *rilevare le collisioni*.

Per *creare trigger*, è necessario spuntare la casella `Is Trigger` nell'Inspector di un qualunque collider. Così facendo l'oggetto potrà compenetrare altri collider e non li spingerà via.

Quando un collider entra nello spazio di un trigger, viene chiamata una funzione di attivazione. Queste sono:
- `OnTriggerEnter`
- `OnTriggerStay`
- `OnTriggerExit`

> [!example]- <font color="orange">Esempio</font>
>```CS
>void OnTriggerEnter(Collider otherCollider)
>{
>	Debug.Log("Trigger attivato");
>}
>```


> [!note]+ Trigger "inverso"
> Da notare che è possibile inserire un metodo di trigger (come `OnTriggerEnter`) anche nel **GameObject che va a interagire** con il trigger.
> 
> Ad esempio, abbiamo una sfera (rigidbody) ed una moneta (trigger, con un tag `Coins`). La moneta deve scomparire quando la sfera la tocca e deve essere aumentato il punteggio.
> 
> ![[Pasted image 20240605123328.png|300]]
> 
> Nel codice della sfera inseriamo:
>
>```CS
>public Text scoreText; // Testo di Unity UI
>private int score = 0;
>
>void OnTriggerEnter(Collider other)
>{
>    if (other.gameObject.CompareTag("Coins")) // Oppure si controlla il nome di other
>    {
>        score++;
>        scoreText.text = $"Score: {score}";
>        other.gameObject.SetActive(false);
>    }
>}
>```
>
>Si attiva solamente quando il player entra in un trigger con tag `Coins`.


## Physic Material
>Il **Physic Material** viene utilizzato per regolare gli effetti di *attrito* e *rimbalzo* quando due oggetti *si scontrano*.

Per creare un Materiale fisico `Assets ➡️ Create ➡️ Physic Material`. Poi bisogna trascinarlo dalla vista del progetto su un collider.

![[Pasted image 20240418175743.png|600]]

| Proprietà            | Descrizione                                                                                                                                                                                                                                                                                   |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Dynamic Friction** | L'*attrito* utilizzato quando *si è già in movimento*. Di solito un valore da 0 a 1. <br>Un valore pari a zero dà la sensazione di *ghiaccio*, mentre un valore pari a 1 fa sì che l'oggetto si <br>fermi molto *rapidamente*, a meno che non sia spinto da una grande forza o dalla gravità. |
| **Static Friction**  | L'*attrito* utilizzato quando un oggetto *è fermo su una superficie*. Di solito il valore varia da 0 a 1. <br>Stessa cosa di quello dinamico.                                                                                                                                                 |
| **Bounciness**       | Determina *quanto rimbalza* la superficie a cui è attribuita. Un valore di 0 non rimbalza. <br>Un valore di 1 rimbalzerà senza alcuna perdita di energia, anche se sono previste alcune <br>approssimazioni che potrebbero aggiungere piccole quantità di energia alla simulazione.           |



### Combine
Le proprietà **Combine** cambiano l'*intensità generale* della friction e del bounciness. 
In pratica, determina come i due materiali fisici nella collisione determinano l'intensità dell'azione.

Hanno valori:
- Average - Effetto default, media tra i due valori
- *Minimum* - Minimizza l'effetto, minimo tra i due valori
- *Maximum* - Massimizza l'effetto, massimo tra i due valori
- Multiply - Moltiplica i valori insieme, da notare che la Multiply renderà il rimbalzo nullo, se uno dei due oggetti non si muove

> [!example]- <font color="orange">Esempio</font>
>È possibile creare una sfera che rimbalza quasi all'infinito mettendo questi valori:
>- Bounciness: 1
>- Bounce Combine: Maximum

| Proprietà            | Descrizione                                                                                                                                                                                                             |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Friction Combine** | Tradizionalmente le proprietà di attrito dipendono dalla *combinazione dei due materiali a contatto*. È possibile utilizzare la modalità *combine* per regolare la combinazione dei valori di attrito di due materiali. |
| **Bounce Combine**   | Stessa cosa del friction combine, ma inerente al rimbalzo.                                                                                                                                                              |



___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [Manuale - Collision](https://docs.unity3d.com/ScriptReference/Collision.html)
- [HTML.it - Collider e Trigger](https://www.html.it/pag/48885/collider-gestire-le-collisioni-in-unity/)
- [Manuale - Physic Material](https://docs.unity3d.com/ScriptReference/PhysicMaterial.html)
- [Collider con parenting](https://gamedev.stackexchange.com/questions/151670/how-to-detect-collision-occurring-on-a-child-object-from-a-parent-script)