---
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: GlassFish Help
cssclasses:
  - 
---
Un Progetto Enterprise serve per raggruppare un [[4 GlassFish Help - EJB|EJB Module]] ed un Web Module.

**Creare una nuova Enterprise Application**:
- `New Project ➡️ Java with Ant ➡️ Java Enterprise ➡️ Enterprise Application`

![[Pasted image 20231128122303.png]]

**Alla sua creazione verranno automaticamente creati i progetti extra**:

![[Pasted image 20231128122333.png]]

![[Pasted image 20231128121459.png]]

**Creare l'EJB**: si può copiare ed incollare il pacchetto specificato nella parte [[4 GlassFish Help - EJB]].

> [!warning] Ricordare di inserire nell'EJB Module i file `persistence.xml` e `beans.xml` all'interno di `Configuration Files` e del file `gf-client.jar` nelle librerie.
> ![[Pasted image 20231128123018.png]]


**Creare la [[013 Servlet|Servlet]] nel Web Module**:
- `New ➡️ Servlet... ➡️ (dare un nome alla Servlet) ➡️ (Per accedere velocemente alla pagina selezionare come URL Pattern(s) solo "/") ➡️ Add information to deployment descriptor (web.xml) ➡️ Finish`

![[Pasted image 20231128123302.png]]

L'XML deve essere così:
```xml
<servlet-mapping>
    <servlet-name>NewServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

**Eseguire il progetto (aspettare che ogni passo finisca)**:
- `(Tasto destro sul Progetto Enterprise) ➡️ Clean and Build`
- `(Tasto destro sul Progetto Enterprise) ➡️ Deploy`
- `(Tasto destro sul Progetto Enterprise) ➡️ Run ➡️ (dopo l'esecuzione si dovrebbe aprire il browser)`

> [!tip]- Codice Servlet
> Testo.
>```Java
>package myclient;
>
>import it.pd2023.musiclibrary.MusicLibraryRemote;
>import it.pd2023.musiclibrary.Song;
>import java.io.IOException;
>import java.io.PrintWriter;
>import javax.ejb.EJB;
>import javax.servlet.ServletException;
>import javax.servlet.http.HttpServlet;
>import javax.servlet.http.HttpServletRequest;
>import javax.servlet.http.HttpServletResponse;
>
>public class MusicPlayer extends HttpServlet {
>	@EJB
>	MusicLibraryRemote ejb;
>
>	@Override
>	protected void doGet(HttpServletRequest request, HttpServletResponse response)
>			throws ServletException, IOException {
>		
>		response.setContentType("text/html;charset=UTF-8");
>		try ( PrintWriter out = response.getWriter()) {
>			Song s = ejb.findSongs().get(1);
>			out.println("<!DOCTYPE html>");
>			out.println("<html>");
>			out.println("<head>");
>			out.println("<title>Servlet MusicPlayer</title>");
>			out.println("</head>");
>			out.println("<body>");
>			out.println("<h1>Servlet MusicPlayer at " + request.getContextPath() + "</h1>");
>			out.println("<h3>" + s.getName() + " by " + s.getAuthors() + "</h3>");
>			out.println("<br>");
>			out.println("<iframe width=\"420\""
>					+ "height=\"315\"\n"
>					+ "src=\"" + s.getUrl() + "\">\n"
>					+ "</iframe>");
>			out.println("</body>");	
>			out.println("</html>");
>		}
>	}
>
>	@Override
>	protected void doPost(HttpServletRequest request, HttpServletResponse response)
>			throws ServletException, IOException {
>		doGet(request, response);
>	}
>}
>```

![[Pasted image 20231128122241.png]]


%%
[[000 Indice PD|↖ Ritorna all'indice ↖]]
%%