---
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: GlassFish Help
cssclasses:
  - 
---

## SETUP PROGETTO WS PROVIDER
**Creare il progetto per i WS**:
- `New Project ➡️ Java with Ant ➡️ Java Web ➡️ Web Application ➡️ (Crea un package)`

**Creare un oggetto utilizzando JAXB**:
- `(Tasto destro sul Package) ➡️ New ➡️ Java Class... ➡️ (inserire le annotazioni XML)`

> [!example]- <font color="orange">Esempio</font>
>```Java
>package service;
>
>import javax.xml.bind.annotation.XmlAccessType;
>import javax.xml.bind.annotation.XmlAccessorType;
>import javax.xml.bind.annotation.XmlAttribute;
>import javax.xml.bind.annotation.XmlRootElement;
>
>@XmlRootElement
>@XmlAccessorType(XmlAccessType.FIELD)
>public class CreditCard  {
>	@XmlAttribute(required = true)
>	private String number;
>	
>	@XmlAttribute(name = "expiry_date", required = true)
>	private String expiryDate;
>	
>	@XmlAttribute(name = "control_number", required = true)
>	private Integer controlNumber;
>	
>	@XmlAttribute(required = true)
>	private String type;
>
>	// Costruttore vuoto obbligatorio
>	public CreditCard() {}
>
>	// Costruttore completo
>	public CreditCard(String number, String expiryDate, Integer controlNumber, String type) {
>		this.number = number;
>		this.expiryDate = expiryDate;
>		this.controlNumber = controlNumber;
>		this.type = type;
>	}
>
>	public String getNumber() {
>		return number;
>	}
>
>	public void setNumber(String number) {
>		this.number = number;
>	}
>
>	public String getExpiryDate() {
>		return expiryDate;
>	}
>
>	public void setExpiryDate(String expiryDate) {
>		this.expiryDate = expiryDate;
>	}
>
>	public Integer getControlNumber() {
>		return controlNumber;
>	}
>
>	public void setControlNumber(Integer controlNumber) {
>		this.controlNumber = controlNumber;
>	}
>
>	public String getType() {
>		return type;
>	}
>
>	public void setType(String type) {
>		this.type = type;
>	}
>
>	@Override
>	public String toString() {
>		return "CreditCard{" + "number=" + number + ", expiryDate=" + expiryDate + ", controlNumber=" + controlNumber + ", type=" + type + '}';
>	}
>}
>```

### Creare il WS
- `(Tasto destro sul Package) ➡️ New ➡️ Java Interface... ➡️ (Annotare con @WebService)`
- `(Tasto destro sul Package) ➡️ New ➡️ Web Service... ➡️ (Crea come Session Bean) ➡️ (Annotare con @WebService)`

![[Pasted image 20231124175843.png]]

![[Pasted image 20231124175903.png]]

**Implementare i metodi richiesti dall'interfaccia**.

**Come creare un metodo attraverso il wizard di NetBeans**:
- `Progetto ➡️ Web Services ➡️ (Tasto destro sul WS desiderato) ➡️ Add Operation...`

![[Pasted image 20231124181306.png]]

![[Pasted image 20231124181530.png]]

> [!example]- <font color="orange">Esempio</font>
> 
>Interfaccia *Validator*:
>```Java
>package service;
>
>import javax.jws.WebService;
>
>@WebService
>public interface Validator {
>	public boolean validate(CreditCard creditCard);
>}
>```
>*CardValidator*:
>```Java
>package service;
>
>import javax.ejb.Stateless;
>import javax.jws.WebMethod;
>import javax.jws.WebParam;
>import javax.jws.WebService;
>
>@WebService(serviceName = "CardValidator")
>@Stateless()
>public class CardValidator implements Validator {
>	
>	@Override
>	@WebMethod(operationName = "validate")
>	public boolean validate(@WebParam(name = "credit_card") CreditCard creditCard) {
>		Character lastDigit = creditCard.getNumber().charAt(
>				creditCard.getNumber().length() - 1
>		);
>		
>		if(Integer.parseInt(lastDigit.toString()) % 2 == 0)
>			return true;
>		else
>			return false;
>	}
>
>	@WebMethod(operationName = "validateData")
>	public boolean validateData(
>		@WebParam(name = "number") String number, @WebParam(name = "expiry_date") String expiryDate, 
>		@WebParam(name = "control_number") Integer controlNumber, @WebParam(name = "type") String type
>	) {
>		return validate(new CreditCard(number, expiryDate, controlNumber, type));
>	}
>	
>}
>```

**Fare il deploy del progetto Provider**:
- `(Tasto destro sul Progetto) ➡️ Deploy`

### Testare il WS con NetBeans
- `Progetto ➡️ Web Services ➡️ (Tasto destro sul WS desiderato) ➡️ Test Web Service`

![[Pasted image 20231124192548.png]]

Copia il link a WSDL File se necessario per dopo.

![[Pasted image 20231124182302.png|900]]

## SETUP PROGETTO WS CLIENT

**Creare il progetto Client**:
- `New Project ➡️ Java with Ant ➡️ Java Application`

**Creare un package**:
- `(Tasto destro sul progetto) ➡️ New ➡️ Java Package...`

**Creare un Web Service Client**:
- `(Tasto destro sul Progetto) ➡️ New ➡️ Web Service Client ➡️ (Specifica in "Project" il Progetto "Provider" > WS, altrimenti specificare in WSDL URL il link che si può copiare nel Web Service Tester al di sotto dell'header)`

![[Pasted image 20231124184355.png]]

![[Pasted image 20231124182925.png]]

**All'interno di Generated Sources sono stati creati dei file, utilizzarli per ottenere la porta ed eseguire i metodi del WS**.

![[Pasted image 20231124184436.png]]

> [!example]- <font color="orange">Esempio</font>
>```Java
>package myclient;
>
>// Il package service si riferisce al package generato 
>// automaticamente all'interno del progetto client!
>import service.CardValidator;
>import service.CardValidator_Service;
>import service.CreditCard;
>
>public class Main {
>
>	public static void main(String[] args) {
>		CardValidator port = new CardValidator_Service().getCardValidatorPort();
>		
>		CreditCard card = new CreditCard();
>		card.setNumber("6011111111111118");
>		
>		System.out.println(port.validate(card));
>	}
>	
>}
>```
>Output:
>```markup
>true
>```

**Eseguire il main**:
- `(Tasto destro sul Progetto) ➡️ Clean and build`
- `(Tasto destro sul file del main) ➡️ Run File`


> [!bug] Service package not found error
> Effettuare il clean and bulid del progetto e rieseguire il file main.

%%
[[000 Indice PD|↖ Ritorna all'indice ↖]]
%%