---
aliases:
  - private
  - protected
  - package
  - public
  - signature
cssclasses:
  - 
tags:
  - corsi/informatica/ingegneria_software
---

Si definiscono varie proprietà di un attributo.

Il **Tipo** specifica il range di valori consentiti e le possibili operazioni; ^a3165d

La **Signature** (*firma del metodo*) fornisce le informazioni sui parametri passati alle operazioni (funzioni) ed i loro valori di ritorno. ^ecb02b

## Visibilità
La **Visibilità** specifica se l'attributo o l'operazione può essere usata da altre classi: ^870765
- Un attributo **`private`** è *accessibile solo dalla classe in cui è definito*. Analogamente, un'operazione privata può essere invocata solo dalla classe in cui è definita. Gli attributi e le operazioni private non sono accessibili alle sottoclassi o alle classi chiamanti.
 ^60f564
- Un attributo o un'operazione **`protcted`** può essere *accessibile dalla classe in cui è definita e da qualsiasi discendente ([[015 Ereditarietà#^a5acea|sottoclasse]]) di tale classe*. Le operazioni e gli attributi protetti *non possono essere accessibili da nessun'altra classe*.
 ^3e550d
- Un attributo o un'operazione **`public`** è *accessibile a qualsiasi classe*. 
^9f7106
- Un attributo o un'operazione con visibilità **`package`** può essere *accessibile da qualsiasi classe del package più vicino*. Questa visibilità consente a un insieme di classi correlate (ad esempio, che formano un [[019 System Design#Sottosistema|Sottosistema]]) di condividere un insieme di attributi o operazioni senza doverli rendere pubblici all'intero sistema. ^b51fae

![[UML 5 - Diagramma di Classi#Modificatori di Accesso]]


%%
[[000 Indice IS|↖ Ritorna all'indice ↖]]
%%