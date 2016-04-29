---
layout: post
title: Testare-API-REST-con-Restfuse
published: false
	comments: true
categories: 
-Java
-development
tags:
-Restfuse
-REST
-Java
---
![Jekyll]({{site.baseurl}}/assets/rest_API.png)

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
Il modo migliore per gestire le dipendenze delle librerie in un progetto java è sicuramente tramite [Maven](https://maven.apache.org/ "Maven"), riporto quindi la dipendenza da aggiungere al proprio [Maven POM](https://maven.apache.org/pom.html#Introduction "Maven POM"). La libreria è perfettamente presente su [Maven Central](https://maven-repository.com/artifact/com.restfuse/com.eclipsesource.restfuse/1.2.0 "Maven Central") quindi non serve aggiungere altri repository al proprio POM.
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
####Primo esempio
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
L'Annotation ``@HttpTest``contrassegna il test vero e proprio che vogliamo eseguire come possiamo vedere è composta da diversi parametri ovviamente il ``method`` che indica il verbo Http che della nostra API, il ``path`` ovvero il percorso alla quale risponde la nostra API.
####Secondo esempio
Quest'esempio è molto semplice, passiamo ad un'esempio leggeremnte più complesso in modo da conoscere meglio Restfuse che è possibile trovare [qui](http://https://github.com/eclipsesource/restfuse/blob/master/com.eclipsesource.restfuse.example/src/com/eclipsesource/restfuse/example/DynamicHeaderTest.java "qui")
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

In questo esempio la differenza principale con quello precedente sta nel metodo ``getDestination()``, attraverso questo metodo infatti possiamo settare i vari parametri nel path che andremo ad utilizzare nei nostri test, nel caso specifico il parametro ``{file}``. Una cosa molto importante da sapere è che il metodo ``getDestination()`` dell'esempio viene eseguito ad ogni test.
####Ultimo esempio
L'ultimo esempio che voglio mostrare è un'inserimento di dati tramite POST.
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

    @HttpTest(method = Method.POST, path = "/music/{song}/{artist}", order = 1, content = "dummy")
    public void testAddTrack() {
        assertOk(response);
    }

    @HttpTest(method = Method.GET, path = "/music/{song}", order = 2)
    public void testGetSong() {
        assertOk(response);
    }
{% endhighlight %}

In questo esempio viene eseguita la POST di alcuni dati la cosa interessante che mi ha fatto perdere del tempo è il parametro ``content`` nel caso infatti stia inserendo dei parametri tramite il path della mia API non ho un contenuto nel body, ma Restfuse nella versione attuale non permette di non inserire o di lasciare vuoto il parametro, da qui il valore ``content="dummy"`` utilizzato,
maggiori informazioni su questo bug le potete trovare [qui](https://github.com/eclipsesource/restfuse/issues/42 "qui").
Un'altro parametro molto importante è ``order`` che permette di definire un'ordine di esecuzione dei test, questo parametro farà storcere il naso ai puristi dell'idempotenza dei test, ma in taluni casi può essere molto utile.  

###Conclusioni
Restfuse è sicuramente una buona libreria per testare REST API, ma presenta diversi bug al momento ed un supporto a tratti latitante, speriamo che il progetto riparta in qualche modo.
Aggiungo anche questo [repo Github](https://github.com/andreasmihm/restfuse "repo Github") di [Andreas Mihm](https://github.com/andreasmihm "Andreas Mihm") che aggiunge ulteriori esempi, rispetto a quello originale citato in precedenza.

Se qualcuno è interessato all'utilizzo di Maven con il [plugin failsafe](https://maven.apache.org/surefire/maven-failsafe-plugin/ "plugin failsafe"), consiglio l'ottimo [post](http://codingjam.it/maven-integration-tests/ "post"), sempre su [Coding Jam](http://codingjam.it "Coding Jam").

Per adesso è tutto.
Come sempre **Stay Tuned**!