---
aliases: 
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: 
cssclasses:
  - 
---
>La classe **`Input`** di Unity fornisce metodi per *interagire con gli input dell'utente*, come tasti premuti, mouse e touch. È utilizzato insieme alla classe **`KeyCode`** che contiene le costanti per i tasti.

## Tastiera
I tre metodi principali per la tastiera sono:
- ***`GetKey(KeyCode key)`*** - Restituisce `true` *mentre* il tasto specificato è premuto.
- ***`GetKeyDown(KeyCode key)`*** - Restituisce `true` *durante il frame* in cui il tasto specificato è stato *premuto*.
- ***`GetKeyUp(KeyCode key)`*** - Restituisce `true` *durante il frame* in cui il tasto specificato è stato *rilasciato*.

> [!example]- <font color="orange">Esempio</font>
>```CS
>using System.Collections;
>using System.Collections.Generic;
>using UnityEngine;
>
>public class MyScriptInput : MonoBehaviour
>{
>
>    public GameObject prefab;
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
>    }
>}
>```

### Frecce direzionali
Si può utilizzare una stringa nel metodo ***`GetAxis()`*** per ottenere il valore del tasto premuto di un "asse":
- `GetAxis("Horizontal")` - Per prendere il valore dell'asse orizzontale, freccia destra e sinistra
- `GetAxis("Vertical")` - Per prendere il valore dell'asse verticale, freccia su e giù

## Mouse
I quattro metodi principali per il mouse sono:
- ***`OnMouseEnter(){...}`*** - Chiamato quando il mouse *entra* all'interno di un GameObject.
- ***`OnMouseExit(){...}`*** - Chiamato quando il mouse *non si trova più* all'interno di un GameObject.
- ***`OnMouseOver(){...}`*** - Chiamato quando il mouse *si trova* su un GameObject.
- ***`OnMouseDown(){...}`*** - Chiamato quando viene *premuto il pulsante* del mouse.


> [!example]- <font color="orange">Esempio</font>
>```CS
>void OnMouseOver()
>{
>	Color color = GetComponent<Renderer‎ >().material.color;
>	color.a = 1F;
>	GetComponent<Renderer‎ >().material.color = color;
>}
>
>void OnMouseExit()
>{
>	Color color = GetComponent<Renderer‎ >().material.color;
>	color.a = 0.5F;
>	GetComponent<Renderer‎ >().material.color = color;
>}
>
>void OnMouseDown()
>{
>	Color color = GetComponent<Renderer‎ >().material.color;
>	color.r = Random.value;
>	color.g = Random.value;
>	color.b = Random.value;
>	color.a = 1f;
>	GetComponent<Renderer‎ >().material.color = color;
>}
>```

___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

Altri collegamenti: 

- [Manuale - Mouse](https://docs.unity3d.com/Manual/UIE-Mouse-Events.html)