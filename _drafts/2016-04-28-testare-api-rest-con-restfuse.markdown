---
layout: post
title: Testare-API-REST-con-Restfuse
published: false
	comments: true
categories:
---

#Testare API REST con Restfuse
###REST API
Le [API](https://it.wikipedia.org/wiki/Application_programming_interface "API") REST o meglio ancora [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer "RESTful"), sono oggi molto utilizzate, hanno infatti molteplici vantaggi, in particolar modo la semplicità di utilizzo ed il fatto di utilizzare il [protocollo HTTP](https://it.wikipedia.org/wiki/Hypertext_Transfer_Protocol "protocollo HTTP"), le rendono una standard de facto per il World Wide Web. Vengono utilizzate praticamente ovunque e da chiunque, basta andare in un sito del genere per capire la vastità dell'universo REST.
###Restfuse
Ma andiamo al sodo, per un progetto sul quale sto lavorando al momento, avevo la necessità non solo di sviluppare un'API REST ma eseguire dei test di integrazione per verificare che tutte le chiamate realizzate, funzionassero in modo corretto. Questo [post](http://codingjam.it/junit-test-the-rest/ "post") su [CodingJam](http://codingjam.it/ "CodingJam") mi ha ispirato, i possibili framework infatti per eseguire una serie di Test in ambiente REST non mancano davvero, come potete verificare nell'ottimo post citato.
La mia scelta però è andata su **Restfuse**, per la *presunta semplicità*, e per il fatto che fosse **un'estensione di [JUnit](http://junit.org/junit4/ "JUnit")** la più famosa libreria per i test automatici nel mondo Java.
<br/>
Devo subito fare una premessa doverosa, al momento il progetto [Restfuse](http://developer.eclipsesource.com/restfuse/ "Restfuse") sembra in stallo, si capisce subito dal fatto che la homepage del progetto restituisce un'allarmante **404 Not Found**.
Non mi sono comunque lasciato intimidire da questo funesto segnale ed ho iniziato a documentarmi sul [repository Github del progetto](https://github.com/eclipsesource/restfuse "repository Github del progetto").
La doumentazione in rete su questo progetto non è molto ampia purtroppo, e tutto quello che c'è, ruota intorno al repository Github.
###Installazione della Libreria
Il modo migliore per gestire le dipendenze delle librerie in un progetto java è sicuramente tramite [Maven](http://https://maven.apache.org/ "Maven"), riporto quindi la dipendenza da aggiungere al proprio [Maven POM](http://https://maven.apache.org/pom.html#Introduction "Maven POM"). La libreria è perfettamente presente su [Maven Central](http://https://maven-repository.com/artifact/com.restfuse/com.eclipsesource.restfuse/1.2.0 "Maven Central") quindi non serve aggiungere altri repository al proprio POM.
```
		<dependency>
			<groupId>com.restfuse</groupId>
			<artifactId>com.eclipsesource.restfuse</artifactId>
			<version>1.2.0</version>
		</dependency>
```
Se non fosse possibile utilizzare Maven, nella sezione [download](http://developer.eclipsesource.com/restfuse/downloads/http:// "download") del sito ufficiale è possibile scaricare la libreria.
###Esempio di test con Restfuse
Passiamo subito al [codice di esempio](hthttps://github.com/eclipsesource/restfuse/blob/master/com.eclipsesource.restfuse.example/src/com/eclipsesource/restfuse/example/SimpleHttpTest.javatp:// "codice di esempio") presente nel repository del progetto.
{% highlight java%}
@RunWith( HttpJUnitRunner.class )
public class SimpleHttpTest {
  
  @Rule
  public Destination restfuse = new Destination( this, "http://restfuse.com" );
  
  @Context
  private Response response;
  
  @HttpTest( method = Method.GET, path = "/" ) 
  public void checkRestfuseOnlineStatus() {
    assertOk( response );
  }
  
}
{% endhighlight %}

Come si vede il tutto è perfettamente configurabile tramite [Annotations](http://https://en.wikipedia.org/wiki/Java_annotation "Annotations"), ``@RunWith`` è la direttiva con la quale scegliamo la classe che eseguirà i nostri test, come dicevo all'inizio del post, alla fine Restfuse è un'estensione di JUnit.
L'Annotation ``@HttpTest``contrassegna il test vero e proprio che vogliamo eseguire 




