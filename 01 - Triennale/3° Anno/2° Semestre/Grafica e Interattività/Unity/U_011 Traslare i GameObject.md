---
aliases: 
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Unity Scripting
cssclasses:
  - 
---
>**Traslare** un oggetto in Unity significa *spostarlo da una posizione a un'altra* nello spazio 3D del gioco. Questo è uno degli elementi fondamentali per il movimento degli oggetti in un gioco. Questo sistema utilizza la classe ***`Transform`***.

^c26b0d

Si può traslare un oggetto in due modi:
- Assegnare lo script ad un GameObject ed utilizzare `transform.Translate()`
- Ottenere il riferimento al GameObject `obj` all'interno dello script (attraverso le variabili pubbliche o altri modi) ed utilizzare `obj.transform.Translate()`

La `Translate()` ha bisogno di solo un `Vector3`, ma è possibile moltiplicarlo per una velocità di movimento (float) e con `Time.deltaTime`.

> [!info]+ Framerate independent
> Senza `Time.deltaTime` la velocità di movimento è determinata anche dal numero di [[Frames per Second|Frame al Secondo]].
> 
> Infatti, questo permette agli oggetti di essere traslati di una quantità uguale in *un secondo* invece di ogni frame.

> [!example]- <font color="orange">Esempio - Assegnato ad un GameObject</font>
>```CS
>using System.Collections;
>using System.Collections.Generic;
>using UnityEngine;
>
>public class ObjectTraslation : MonoBehaviour
>{
>    public float moveSpeed = 10;
>
>    // Start is called before the first frame update
>    void Start()
>    {
>        
>    }
>
>    // Update is called once per frame
>    void Update()
>    {
>        if (Input.GetKey(KeyCode.Space))
>        {
>            transform.Translate(moveSpeed * Time.deltaTime * Vector3.up);
>        }
>        if (Input.GetKey(KeyCode.LeftControl))
>        {
>            transform.Translate(moveSpeed * Time.deltaTime * Vector3.down);
>        }
>    }
>}
>```

> [!example]- <font color="orange">Esempio - Attraverso il riferimento</font>
>```CS
>using System.Collections;
>using System.Collections.Generic;
>using UnityEngine;
>
>public class MyScriptInput : MonoBehaviour
>{
>
>    public GameObject prefab;
>    public float movespeed = 20;
>
>    private GameObject obj;
>    private bool created = false;
>
>    // Start is called before the first frame update
>    void Start()
>    {
>        
>    }
>
>    // Update is called once per frame
>    void Update()
>    {
>        // Creazione e distruzione dell'oggetto
>        if (Input.GetKeyDown(KeyCode.C) && !created) 
>        {
>            obj = Instantiate(prefab, Vector3.zero, Quaternion.identity);
>            created = true;
>        }
>        else if (Input.GetKeyDown(KeyCode.D) && created)
>        {
>            Destroy(obj);
>            created = false;
>        }
>
>        // Movimento oggetto
>        if (Input.GetKey(KeyCode.UpArrow) && created)
>        {
>            obj.transform.Translate(movespeed * Time.deltaTime * Vector3.up);
>        }
>        if (Input.GetKey(KeyCode.DownArrow) && created)
>        {
>            obj.transform.Translate(movespeed * Time.deltaTime * Vector3.down);
>        }
>        if (Input.GetKey(KeyCode.RightArrow) && created)
>        {
>            obj.transform.Translate(movespeed * Time.deltaTime * Vector3.right);
>        }
>        if (Input.GetKey(KeyCode.LeftArrow) && created)
>        {
>            obj.transform.Translate(movespeed * Time.deltaTime * Vector3.left);
>        }
>    }
>}
>```

> [!tip]+ `transform.position` vs `transform.Translate()`
>- **Assoluto vs. Relativo**: 
>	- `transform.position = new Vector3(...)` imposta la *posizione assoluta* nel mondo di gioco
>	- `transform.Translate(new Vector3(...))` applica un *movimento relativo* all'oggetto originale
>
>![[Pasted image 20240417130739.png|500]]
>- **Immediato vs. Time-Based**: 
>	- I cambiamenti di `transform.position` avvengono *istantaneamente*
>	- I cambiamenti di `transform.Translate()` *possono* essere istantanei, ma si può anche usare con la [[U_012 Fisica di un GameObject|fisica]] o le animazioni per avere un *movimento fluido nel tempo*

### Roteazione
Per la **roteazione** si applicano gli stessi modi della Traslazione, ma in questo caso si usa il metodo ***`Rotate()`***.

> [!example]- <font color="orange">Esempio</font>
>```CS
>using System.Collections;
>using System.Collections.Generic;
>using UnityEngine;
>
>public class ObjectTraslation : MonoBehaviour
>{
>    public float moveSpeed = 10;
>    public float rotateSpeed = 5;
>
>    // Start is called before the first frame update
>    void Start()
>    {
>        
>    }
>
>    // Update is called once per frame
>    void Update()
>    {
>        if (Input.GetKey(KeyCode.W))
>        {
>            transform.Translate(moveSpeed * Time.deltaTime * Vector3.up);
>        }
>        if (Input.GetKey(KeyCode.S))
>        {
>            transform.Translate(moveSpeed * Time.deltaTime * Vector3.down);
>        }
>        if (Input.GetKey(KeyCode.A))
>        {
>            transform.Rotate(rotateSpeed * moveSpeed * Time.deltaTime * Vector3.forward);
>        }
>        if (Input.GetKey(KeyCode.D))
>        {
>            transform.Rotate(rotateSpeed * moveSpeed * Time.deltaTime * Vector3.back);
>        }
>    }
>}
>```

### Look At
Ruota la `transform` in modo che il vettore `forward` punti alla posizione corrente del target.

> [!example]- <font color="orange">Esempio</font>
>```CS
>using UnityEngine;
>// This complete script can be attached to a camera to make it
>// continuously point at another object.  
>  
>public class ExampleClass : MonoBehaviour
>{
>    public Transform target;  
>  
>    void Update()
>    {
>        // Ruota la telecamera ogni frame in modo che guardi il target
>        transform.LookAt(target);  
>
>		// Stessa cosa di sopra, ma viene specificato il verso in cui si deve roteare la telecamera
>        transform.LookAt(target, Vector3.up);
>    }
>}
>```
>
>![[Pasted image 20240418170750.png|600]]

### Lerp

> [!note]- Interpolazione Lineare
> È il processo per calcolare un valore all'interno dell'intervallo tra due valori conosciuti. Si tratta di una tecnica che utilizza la formula della retta per determinare un punto intermedio tra due punti dati.

La `Lerp` è l'abbreviazione di "Linear Interpolation" (interpolazione *lineare*). Una funzione che prende due valori (di solito numeri o vettori) e un valore `t` compreso tra 0 e 1, e restituisce un valore che è una combinazione proporzionale dei due valori originali.

![[Pasted image 20240518161729.png]]

Esiste anche una versione *sferica* chiamata `Slerp`.


> [!example]- <font color="orange">Esempio</font>
>```CS
>private Vector3 newposition;
>	public float smoothing = 1;
>		
>	void Start () 
>	{
>		transform.position = new Vector3 (0, 0.5F, 0);
>	}
>	
>	// Update is called once per frame
>	void Update () {
>		if (Input.GetKeyDown (KeyCode.V)) 
>		{
>			newposition = new Vector3(Random.value*10, 0.5F, Random.value*10);
>		}
>		
>		transform.position = Vector3.Lerp
>		(
>			transform.position, 
>			newposition, 
>			smoothing * Time.deltaTime
>		);
>	}
>```



___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [Manuale - Translate](https://docs.unity3d.com/ScriptReference/Transform.Translate.html)
- [Manuale - Rotate](https://docs.unity3d.com/ScriptReference/Transform.Rotate.html)
- [Manuale - LookAt](https://docs.unity3d.com/ScriptReference/Transform.LookAt.html)
- [Manuale - Lerp](https://docs.unity3d.com/ScriptReference/Vector3.Lerp.html)
	- [Lerp e Slerp visualizzati](https://www.youtube.com/watch?v=AzmVVPWao8U)