---
aliases: 
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Unity Scripting
cssclasses:
  - 
---
Di default, si può accedere al [[U_002 GameObject|GameObject]] attraverso *`gameObject`*, ad esempio per cambiare il nome del GameObject quando il gioco inizia:

```CS
void Start()
{
    gameObject.name = "My Object";
}
```

Normalmente si può interagire solamente con le *informazioni basilari del GameObject* (nome, se è statico, ecc.) e le *informazioni di trasformazione*. Per far comunicare lo script con gli altri componenti del GameObject, si deve creare un **riferimento** al componente.

### Riferimento Public
Rendendo **public** il riferimento, è possibile *impostare questi valori direttamente dall'editor* di Unity nell'inspector sotto il componente script.

> [!example]- <font color="orange">Esempio</font>
>```CS
>using System.Collections;
>using System.Collections.Generic;
>using UnityEngine;
>
>public class MyNewScript : MonoBehaviour
>{
>	public Rigidbody2D myRigidBody; // Riferimento ad un Rigidbody2D
>	public float FlapStrength;
>	
>    // Start is called before the first frame update
>    void Start()
>    {
>		
>    }
>	
>	// Update is called once per frame
>	void Update()
>	{
>	    
>	}
>}
>```
>
>![[Pasted image 20240415120506.png]]
>Basta *trascinare* il componente all'interno dello spazio giusto oppure lo si può selezionare dal menu.
>
>![[Pasted image 20240415115605.png]]

Le variabili pubblic possono anche essere modificate durante l'esecuzione del gioco, ma i valori non saranno salvati. In questo modo è possibile modificare dei valori senza correre il rischio di rompere qualcosa. 

Si può anche utilizzare il metodo `GetComponent<Component>()` di *`gameObject`* per ottenere un riferimento al componente.

> [!example]- <font color="orange">Esempio</font>
>```CS
>Rigidbody2D rigidBody;
>
>void Start() 
>{
>	rigidBody = gameObject.GetComponent<‎ Rigidbody2D‎ >();
>}
>```

### Variabili statiche
È possibile rendere una variabile *pubblica* anche **[[Keyword static|statica]]**, questo *permette ad altri script di accedere* a quella variabile. 

> [!example]- <font color="orange">Esempio</font>
>```CS
>// Script1
>public static int variable = 1;
>
>// Script2
>Debug.Log(Script1.variable);  // 1
>Script1.variable = 5;
>Debug.Log(Script1.variable);  // 5
>```

È anche applicabile alle *funzioni pubbliche*.

> [!example]- <font color="orange">Esempio</font>
>```CS
>// Script1
>public static void MyFunc()
>{
>    Debug.Log("MyFunc from Script1");
>}
>
>// Script2
>MyFunc();  // MyFunc from Script1
>```

## Tags
>Un **Tag** è una *parola di riferimento* che si può assegnare a uno o più GameObject.
>Ad un GameObject *può essere assegnato un solo tag*.

![[Pasted image 20240415182000.png]]

È possibile utilizzare qualsiasi parola come tag. Si possono usare anche frasi brevi, ma potrebbe essere necessario allargare l'Inspector per vedere il nome completo del tag.

I tag aiutano ad *identificare i GameObject* a fini di scripting. Assicurano che non sia necessario aggiungere manualmente i GameObject alle proprietà esposte di uno script con il drag and drop, in modo da risparmiare tempo quando si utilizza lo stesso codice di script in più GameObject.

È possibile utilizzare il metodo `GameObject.FindGameObjectsWithTag()` per trovare *qualsiasi* GameObject che contenga un tag specificato.

> [!example]- <font color="orange">Esempio</font>
>```CS
>// Imposta il tag di un GameObject
>go.tag = "MyTag";
>
>// Ottiene tutti i GameObject con un tag specifico
>GameObject[] res = GameObject.FindGameObjectsWithTag("MyTag");
>```


___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [Manuale - Tags](https://docs.unity3d.com/Manual/Tags.html)