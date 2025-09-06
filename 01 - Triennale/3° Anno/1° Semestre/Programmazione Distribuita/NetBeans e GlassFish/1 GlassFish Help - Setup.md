---
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: GlassFish Help
cssclasses:
  - 
---
## Installare l'IDE
Installare la [JDK 8](https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html).

In questo corso si utilizza come IDE [NetBeans 15](https://netbeans.apache.org/front/main/download/nb15/). 

> [!warning] Installare la versione 15 di NetBeans
> È l'ultima versione che supporta la JDK8.

Durante l'installazione, impostare la JDK utilizzata da NetBeans alla 1.8.

![[Pasted image 20231111092601.png]]

Se non la utilizza modificare `netbeans/etc/netbeans.conf`.

## Installare GlassFish
GlassFish è il [[011 Web Dinamico|Web Container]] che viene usato durante il corso.

Riavviare NetBeans dopo il primo avvio.

**Aggiungere il server**:
- `Tools ➡️ Servers ➡️ Add Server... ➡️ GlassFish Server`

![[Pasted image 20231111093214.png]]

> [!warning] Scaricare ed installare la versione 4.1 di GlassFish Server
> ![[Pasted image 20231111093532.png]]

Dopo aver scaricato il server, lasciare i `Domain Location` come sono e cliccare su `Finish`.

Verificare che il server GlassFish utilizza la JDK 8.

![[Pasted image 20231111094448.png]]

Nel caso modificare il file `glassfish/config/asenv.conf` per impostare il percorso corretto della JAVA_HOME.

%%
[[000 Indice PD|↖ Ritorna all'indice ↖]]
%%