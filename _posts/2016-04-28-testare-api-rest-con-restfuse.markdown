---
layout: post
title: Testare API REST con Restfuse
published: true
comments: true
categories: 
- Java
- development
tags:
- Restfuse
- REST
- Java
---
![Jekyll]({{site.baseurl}}/assets/rest_API.png)

<h3>REST API</h3>
Le [API](https://it.wikipedia.org/wiki/Application_programming_interface "API") REST o meglio [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer "RESTful"), sono oggi molto utilizzate, presentano infatti molteplici vantaggi, in particolar modo la semplicità di utilizzo ed il fatto di utilizzare il [protocollo HTTP](https://it.wikipedia.org/wiki/Hypertext_Transfer_Protocol "protocollo HTTP"), le rendono una standard de facto per il World Wide Web. Vengono utilizzate praticamente ovunque e da chiunque, basta colleagarsi ad un [sito del genere](https://www.publicapis.com) per capire la vastità dell'universo REST.
<!--more-->
<h3>Restfuse</h3>
Ma andiamo al sodo, per un progetto sul quale sto lavorando al momento, avevo la necessità, non solo di sviluppare un'API REST ma anche di eseguire dei test di integrazione per verificare che tutte le chiamate realizzate, funzionassero in modo corretto.
Questo [post](http://codingjam.it/junit-test-the-rest/ "post") su [CodingJam](http://codingjam.it/ "CodingJam") mi ha ispirato.<br/>
I possibili framework infatti per eseguire una serie di Test in ambiente REST non mancano, come potete verificare nell'ottimo post citato.<br/>
La mia scelta però è andata su **[Restfuse](http://developer.eclipsesource.com/restfuse/)**, per la *presunta semplicità*, e per il fatto che fosse **un'estensione di [JUnit](http://junit.org/junit4/ "JUnit")**, la più famosa libreria per i test automatici nel mondo Java.
<br/>
<br/>
*Devo subito fare una premessa doverosa*, al momento il progetto [Restfuse](http://developer.eclipsesource.com/restfuse/ "Restfuse") sembra in stallo, si capisce subito dal fatto che l'homepage del progetto restituisce un'allarmante **404 Not Found**.<br/><br/>
Non mi sono comunque lasciato intimidire da questo funesto segnale ed ho iniziato a documentarmi sul [repository Github](https://github.com/eclipsesource/restfuse "repository Github del progetto") del progetto.<br/>
La documentazione in rete su questo progetto non è molto ampia e tutto quello che c'è ruota intorno al repository Github.
<h3>Installazione della Libreria</h3>
Il modo migliore per gestire le dipendenze delle librerie in un progetto java è sicuramente tramite [Maven](https://maven.apache.org/ "Maven"), riporto quindi la dipendenza da aggiungere al proprio [Maven POM](https://maven.apache.org/pom.html#Introduction "Maven POM").<br/>
La libreria è perfettamente presente su [Maven Central](https://maven-repository.com/artifact/com.restfuse/com.eclipsesource.restfuse/1.2.0 "Maven Central"), quindi non serve aggiungere altri repository al proprio POM.
{% highlight java %}
        <dependency>
            <groupId>com.restfuse</groupId>
            <artifactId>com.eclipsesource.restfuse</artifactId>
            <version>1.2.0</version>
        </dependency>
{% endhighlight %}
Se non fosse possibile utilizzare Maven, nella sezione [download](http://developer.eclipsesource.com/restfuse/downloads/http:// "download") del sito ufficiale è possibile scaricare la libreria.
<h3>Esempio di test con Restfuse</h3>
Passiamo subito al [codice di esempio](https://github.com/eclipsesource/restfuse/blob/master/com.eclipsesource.restfuse.example/src/com/eclipsesource/restfuse/example/SimpleHttpTest.javatp:// "codice di esempio") presente nel repository del progetto.
<h4>Primo esempio</h4>
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

Come si vede il tutto è perfettamente configurabile tramite [Annotations](http://https://en.wikipedia.org/wiki/Java_annotation "Annotations"), ``@RunWith`` è la direttiva con la quale scegliamo la classe chi eseguirà i nostri test, come dicevo all'inizio del post, alla fine Restfuse è un'estensione di JUnit.
L'Annotation ``@HttpTest``contrassegna il test vero e proprio che vogliamo eseguire.<br/>
Come possiamo vedere l'Annotation è composta da diversi parametri ovviamente il ``method`` che indica il verbo HTTP della nostra API ed il ``path`` ovvero il percorso nel quale risponde la nostra API.
<h4>Secondo esempio</h4>
Quest'esempio è molto semplice, passiamo ad un'esempio leggermente più complesso in modo da conoscere meglio Restfuse. L'esempio è disponibile a questo [indirizzo](http://https://github.com/eclipsesource/restfuse/blob/master/com.eclipsesource.restfuse.example/src/com/eclipsesource/restfuse/example/DynamicHeaderTest.java "indirizzo").
{% highlight java lineos%}
RunWith(HttpJUnitRunner.class)
public class DynamicPathTest {

  @Rule
  public Destination restfuse = getDestination();
  
  @Context
  private Response response;

  @HttpTest(method = Method.GET, path = "/{file}.jar")
  public void checkRestfuseDownloadJarStatus() {
    assertOk( response );
  }
  
  @HttpTest(method = Method.GET, path = "/{file}-javadoc.jar")
  public void checkRestfuseDownloadDocsStatus() {
    assertOk( response );
  }

  private Destination getDestination() {
    Destination destination = new Destination( this, 
                                               "http://search.maven.org/remotecontent?filepath="+"com/restfuse/com.eclipsesource.restfuse/{version}/" );
    RequestContext context = destination.getRequestContext();
    context.addPathSegment( "file", "com.eclipsesource.restfuse-1.1.1" ).addPathSegment( "version", "1.1.1" );
    return destination;
  }
}
{% endhighlight %}

In questo esempio la differenza principale con quello precedente sta nel metodo ``getDestination()``, attraverso questo metodo infatti possiamo settare i vari parametri nel path che andremo ad utilizzare nei nostri test, nel caso specifico il parametro ``{file}``. Una cosa molto importante da sapere è che il metodo ``getDestination()`` dell'esempio **viene eseguito per ciascun test**.
<h4>Ultimo esempio</h4>
L'ultimo esempio che voglio mostrare è un'inserimento di dati tramite [POST](https://en.wikipedia.org/wiki/POST_(HTTP)).
{% highlight java lineos%}
@RunWith(HttpJUnitRunner.class)
public class RestTestIT extends Assert {
    @Rule
    public Destination destination = getDestination();

    @Context
    private Response response;

    private static ResourceBundle finder = ResourceBundle.getBundle("my-music-service");
    
    private Destination getDestination() {
        Destination destination = new Destination(this, finder.getString("destination.url.integration"));
        RequestContext context = destination.getRequestContext();
        context.addPathSegment("song", "Under Pressure")
                .addPathSegment("artist", "David Bowie");
        return destination;
    }

    @HttpTest(method = Method.POST, path = "/music/{song}/{artist}",
     order = 1, content = "dummy")
    public void testAddTrack() {
        assertOk(response);
    }

    @HttpTest(method = Method.GET, path = "/music/{song}", order = 2)
    public void testGetSong() {
        assertOk(response);
    }
{% endhighlight %}

In questo esempio viene eseguita un POST di alcuni dati.<br/>
Una cosa curiosa che mi ha fatto perdere del tempo è il parametro ``content``.<br/> Nel caso infatti che si debbano inserire dei parametri semplicemente tramite il ``path`` dell'API, senza quindi contenuto nel body, nella versione attuale della libreria non è permesso di non inserire o di lasciare vuoto il parametro in questione pena un NullPointerException, da qui il valore ``content="dummy"`` utilizzato.<br/>
Maggiori informazioni su questo **bug** le potete trovare [qui](https://github.com/eclipsesource/restfuse/issues/42 "qui").<br/>
Un'altro parametro molto importante è ``order`` che permette di definire **un'ordine di esecuzione dei test**.<br/>
Questo parametro farà storcere il naso ai puristi dell'idempotenza dei test, ma in taluni casi può essere molto utile.  

<h3>Conclusioni</h3>
Restfuse è sicuramente una buona libreria per testare REST API, ma presenta diversi [bug](https://github.com/eclipsesource/restfuse/issues) al momento ed un supporto a tratti latitante, speriamo davvero che il progetto riparta in qualche modo.<br/>
Aggiungo questo ulteriore [repo Github](https://github.com/andreasmihm/restfuse "repository Github") di [Andreas Mihm](https://github.com/andreasmihm "Andreas Mihm") che aggiunge ulteriori esempi, rispetto a quello originale citato in precedenza.

Se qualcuno è interessato all'utilizzo di Maven con il [plugin failsafe](https://maven.apache.org/surefire/maven-failsafe-plugin/ "plugin failsafe"), consiglio l'ottimo [post](http://codingjam.it/maven-integration-tests/ "post") sempre su [Coding Jam](http://codingjam.it "Coding Jam").

Per adesso è tutto.<br/>
Come sempre **Stay Tuned**!
<br/>
<br/>
*Image Courtesy of [http://developer.decarta.com](http://developer.decarta.com/Docs/REST)*