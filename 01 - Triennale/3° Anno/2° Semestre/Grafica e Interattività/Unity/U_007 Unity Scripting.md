---
aliases:
  - C#
  - C Sharp
  - Namespace
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Unity Scripting
cssclasses:
  - 
---
>Lo scripting serve per creare dei **[[U_002 GameObject#Componenti dei GameObject|Componenti]] Personalizzati**. 
>
>Ogni script viene *considerato come un componente* di un [[U_002 GameObject|GameObject]].

In Unity si utilizza **[C#](https://dotnet.microsoft.com/en-us/languages/csharp)** (C Sharp), un linguaggio simile a Java.

> [!success] [Differenze tra C# e Java](http://www.javacamp.org/javavscsharp/)

## Creare un nuovo Script
A differenza della maggior parte degli altri asset, gli script vengono solitamente creati direttamente all'interno di Unity. 

È possibile creare un nuovo script: 
- Dal menu `Create` in alto a sinistra del pannello Project 
- Da `Assets ➡️ Create ➡️ C# Script` dal menu in alto
- Aggiungendo un nuovo componente `Script` nell'Inspector di un GameObject

Il nuovo script verrà creato nella cartella selezionata nel pannello Progetto.

![[Pasted image 20240416152602.png]]

![[Pasted image 20240415184419.png]]

## Anatomia di uno Script
Si può impostare l'editor per aprire i file di script in `Edit ➡️ Preferences... ➡️ External Tools ➡️ External Script Editor`.

Il contenuto iniziale del file sarà simile a questo:

```CS
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MyNewScript : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
		
    }
    
    // Update is called once per frame
    void Update()
    {
		
    }
}
```

> [!note]- Namespace
>I **Namespace** sono utilizzati per organizzare i programmi in C# in unità logiche, sia come sistema di organizzazione "interna" di un programma, sia come sistema di organizzazione "esterna".
>
>Possono essere visti come contenitori che contengono un insieme di classi, interfacce, strutture, [[024 Riutilizzo#Delegazione|delegati]] e altri elementi del linguaggio. Una volta importato un namespace attraverso `using nomenamespace` , è possibile usare il codice fornito al suo interno.
>
>Sono *simili* ad i package in Java, con alcune differenze. Infatti, in Java, i package rappresentano la struttura nel filesystem dei file, mentre in C#, i namespace possono essere anche diversi dalla struttura dei file.
>
>Java:
>```Java
>package mypackage;
>
>class myClass {}
>```
>
>C#:
>```CS
>namespace mynamespace
>{
>	class myClass {}
>}
>```

1. **Intestazione e importazioni**: In C#, l'intestazione inizia con l'utilizzo del comando `using` per *importare i namespace necessari*. In questo caso, il codice utilizza `System.Collections`, `System.Collections.Generic`, e `UnityEngine`.
2. **Dichiarazione della classe**: La *classe* è dichiarata utilizzando il costrutto `public class` seguito dal nome della classe, che in questo caso è `MyNewScript`.
3. **Ereditarietà**: La classe `MyNewScript` *eredita da `MonoBehaviour`*, che è una classe di Unity utilizzata per *creare comportamenti dei GameObject*.
4. **Metodi Principali di `MonoBehaviour`**:
    - Il metodo `Start()` viene chiamato *una sola volta* non appena lo script è *abilitato*.
    - Il metodo `Update()` viene chiamato *continuamente* ad ogni frame del gioco.


Esistono anche altri tipi metodi:
- Il metodo `Awake()` viene chiamato *una sola volta* al completamento del *caricamento dello script*, anche se è disabilitato.
- Il metodo `FixedUpdate()` viene chiamato *dopo ogni aggiornamento della fisica*.
- Il metodo `LateUpdate()` viene chiamato *dopo che `Update()` e `FixedUpdate()`* hanno finito l'esecuzione, è consigliabile utilizzarlo sugli script della [[U_003 Telecamere in Unity|camera]] poiché può tenere traccia degli oggetti che sono già stati spostati.

## Coroutine
Una **Coroutine** consente di distribuire le attività su più fotogrammi. In Unity, una coroutine è un metodo che *può mettere in pausa l'esecuzione* e restituire il controllo a Unity, per poi continuare *nel frame successivo*.

Nella maggior parte dei casi, quando si chiama un metodo normale, questo viene eseguito *fino al completamento* e poi restituisce il controllo al metodo chiamante. Ciò significa che qualsiasi azione che si svolge all'interno di un metodo *deve avvenire all'interno di un singolo frame*.  

Nelle situazioni in cui si desidera utilizzare una chiamata di metodo per contenere un'animazione procedurale o una sequenza di eventi nel tempo, è possibile utilizzare una coroutine.  

Tuttavia, è importante ricordare che le coroutine *non sono [[023 Thread|thread]]*. 

È dichiarata con:

```CS
IEnumerator coroutine_myname() 
{
	// lavoro...
}
```

E viene chiamata con:

```CS
StartCoroutine(coroutine_myname());
```




___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

Altri collegamenti: 