---
aliases: 
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Eclipse Help
cssclasses:
  - 
---
[[#CREARE UN JAR|Creare un jar ↓]]

## CREARE UN PROGETTO MAVEN CON ECLIPSE
Maven è uno strumento di Build Automation e gestione di progetti software Java-based. Produce i file `.jar` del progetto.

Su Eclipse: 
- "File" > "New" > "Project" > Apri la cartella "Maven" > "Maven Project";
- Barrare "Use default Workspace Location" > "Next";
- In "Filter" inserire `maven-archetype-quickstart`;
- Selezionare quello che ha come "Group Id" "*org.apache.maven.archetypes*" > "Next".

![[Pasted image 20220927174926.png|400]]
![[Pasted image 20220927174946.png|400]]
![[Pasted image 20220927175017.png|400]]

Verrà richiesto il:
- **Group Id** (es. `oop22`);
- **Artifact Id**: è il nome del progetto (es. `HelloMavenEclipse`).

![[Pasted image 20220927175058.png|400]]

Cliccare "Finish". Tutti i file necessari verranno creati dopo essere scaricati. Il terminale all'interno di Eclipse potrebbe richiedere un comando, inserire `y` e premere invio.

I file che si possono modificare o creare si trovano nella cartella `ArtifactId > src/main/java > GroupId.ArtifactId`.
Il file `App.java` sarà un file main pronto per poter essere eseguito e modificato.

![[Pasted image 20220927175224.png]]

Per testare se un programma funziona, lo si può compilare ed eseguire direttamente in Eclipse con il comando "Run".

Dopo aver apportato tutte le modifiche necessarie ed aver risolto tutti i bug, il programma è pronto per l'esportazione in `.jar`.

## CREARE UN JAR
Per creare un `.jar` con Maven attraverso Eclipse si deve inserire nel file `pom.xml` (**dopo `</pluginManagement>` e prima di `</build>`**) il seguente codice. 
> [!tip]- Codice per avere un jar
>
>```XML
>		<plugins>
>			<plugin>
>				<artifactId>maven-assembly-plugin</artifactId>
>				<version>3.3.0</version>
>				<configuration>
>					<archive>
>						<manifest>
>							<mainClass>Program1</mainClass> <!-- CAMBIA -->
>						</manifest>
>					</archive>
>					<descriptorRefs>
>						<descriptorRef>jar-with-dependencies</descriptorRef>
>					</descriptorRefs>
>				</configuration>
>				<executions>
>					<execution>
>						<id>make-assembly</id>
>						<phase>package</phase> <!-- bind to the packaging phase -->
>						<goals>
>							<goal>single</goal>
>						</goals>
>					</execution>
>				</executions>
>			</plugin>
>		</plugins>
>```

Su Eclipse l'hotkey `Ctrl` + `Shift` + `F` **indenta automaticamente** tutto il codice del file.

Al posto di `Program1` tra `<mainClass>` e `</mainClass>` si deve inserire il path del file dove si trova il main in questo formato: `GroupId.ArtifactId.Main` (es. `oop22.HelloMaveneclipse.App`).

- Quando si è pronti per creare il `.jar` cliccare "Run Configurations...";
![[Pasted image 20220927181757.png]]
- Cliccare su "Maven Build". 
- Creare una nuova configurazione:
	- Cliccare "Workspace" per ottenere la "Base directory"; 
	- Scrivere `package assembly:single` in "Goals".
- Cliccare "Run".
![[Pasted image 20220927182246.png]]

Potrebbe richiedere un refresh del Package Explorer.
Eseguire il file `...-jar-with-dependencies.jar` attraverso il terminale con il metodo mostrato [[File .jar#^5d8eeb|qui]].

> [!info] Creare un JAR eseguibile da un progetto java SEMPLICE
> Tasto destro sul progetto > "Export" > "Java" > "Runnable JAR file" > Seleziona il nome e la destinazione


%%[[000 Indice POO]]%%