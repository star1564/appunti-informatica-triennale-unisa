---
aliases: 
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Eclipse Help
cssclasses:
  - 
---
![[Args (Java)]]

Per passare degli argomenti (`String[] args`) ad un programma java in Eclipse ci sono due metodi:
1. [[2 Eclipse Help - Nuovo Progetto Maven#CREARE UN JAR|Creare un file .jar]] ed inserire gli argomenti da linea di comando (es: `java -jar file.jar arg0 arg1 arg...`);
2. Utilizzare "Run Configurations...".

Per il secondo metodo, quando si va in "Run Configurations..." 

![[Pasted image 20220927181757.png]]

si deve scegliere tra i diversi "<font color="CC241D">Java Application</font>" il nome <font color="yellow">della classe che contiene il main</font>, per poi cliccare su "<font color="61AFEF">Arguments</font>". 

![[Pasted image 20221008095241.png]]

Inserire gli argomenti all'interno di "Program arguments" e cliccare "Run". 

![[Pasted image 20221008095554.png]]


%%[[000 Indice POO]]%%