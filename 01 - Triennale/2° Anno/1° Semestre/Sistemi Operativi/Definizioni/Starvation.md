---
tags:
  - corsi/informatica/sistemi_operativi
---
La **Starvation** descrive una situazione in cui un [[Processo (Job)|Task]] (o [[023 Thread|Thread]]) *non è in grado di ottenere un accesso alle risorse condivise* e non è in grado di fare progressi. 

Ciò accade quando le risorse condivise vengono rese non disponibili per lunghi periodi da task o thread "*avidi*". 

![[Pasted image 20231003123857.png|400]]

> [!example]+ <font color="orange">Esempio</font>
>Supponiamo, ad esempio, che un oggetto fornisca un metodo sincronizzato che spesso richiede molto tempo per essere restituito. Se un thread richiama frequentemente questo metodo, anche altri thread che necessitano di un accesso sincronizzato frequente allo stesso oggetto verranno spesso bloccati.


%%[[000 Indice SO]]%%