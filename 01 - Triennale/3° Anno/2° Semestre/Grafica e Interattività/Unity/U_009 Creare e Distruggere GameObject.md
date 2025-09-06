---
aliases: 
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Unity Scripting
cssclasses:
  - 
---
> [!note]- Classe Vector3
>Si utilizza spesso questa classe per rappresentare dei **[[002 Vettori|Vettori]] 3D** e **punti**.
>
>```CS
>public Vector3(float x, float y, float z);
>```
>
> Abbreviazione         | Significato         |
> --------------------- | ------------------- |
> ***`Vector3.back`***    | `Vector3(0, 0, -1)` |
> ***`Vector3.down`***    | `Vector3(0, -1, 0)` |
> ***`Vector3.forward`*** | `Vector3(0, 0, 1)`  |
> ***`Vector3.left`***    | `Vector3(-1, 0, 0)` |
> ***`Vector3.one`***     | `Vector3(1, 1, 1)`  |
> ***`Vector3.right`***   | `Vector3(1, 0, 0)`  |
> ***`Vector3.up`***      | `Vector3(0, 1, 0)`  |
> ***`Vector3.zero`***    | `Vector3(0, 0, 0)`  |
>

---
I [[U_002 GameObject#Prefab|Prefab GameObject]] sono molto utili quando si vogliono *istanziare oggetti di gioco* o collezioni di oggetti di gioco **al runtime**. 

>Per fare questo si utilizza il metodo ***`Instantiate()`***. 


Per istanziare un prefab in fase di esecuzione, il codice ha bisogno di un *riferimento a quel prefabbricato*. Per ottenere questo riferimento, si può creare una variabile *pubblica* nel codice che contenga il riferimento al prefabbricato. 

La variabile pubblica nel codice appare come un campo assegnabile nell'Inspector. Si può quindi assegnare il prefab effettivo che si desidera utilizzare nell'Inspector.

![[Pasted image 20240416152058.png]]

> [!example]- <font color="orange">Esempio</font>
>Questo ha una singola variabile pubblica, `myPrefab`, che è un riferimento a un prefab. Nel metodo `Start()` viene creata un'istanza del prefabbricato.
>
>```CS
>using UnityEngine;
>public class InstantiationExample : MonoBehaviour 
>{
>    // Reference to the Prefab. Drag a Prefab into this field in the Inspector.
>    public GameObject myPrefab;
>
>    // This script will simply instantiate the Prefab when the game starts.
>    void Start()
>    {
>        // Instantiate at position (0, 0, 0) and zero rotation.
>        Instantiate(myPrefab, new Vector3(0, 0, 0), Quaternion.identity);
>    }
>}
>```
>Quando si avvia la modalità di riproduzione, si dovrebbe vedere il prefabbricato istanziato nella posizione $(0, 0, 0)$ della scena.

### Eliminazione di GameObjects al runtime
Per eliminare un GameObject al runtime, si utilizza il metodo ***`Destroy()`***.

> [!example]- <font color="orange">Esempio</font>
>```CS
>GameObject[] res = GameObject.FindGameObjectsWithTag("MyTag");
>
>foreach(GameObject g in res)
>	Destroy(g);
>```


___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [Manuale - Instantiate](https://docs.unity3d.com/ScriptReference/Object.Instantiate.html)