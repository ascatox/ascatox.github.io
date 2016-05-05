---
layout: post
title: Introduzione a ngrok
published: true
categories: Servizi Web
tags:
- DevOps
---
![Jekyll]({{site.baseurl}}/assets/ngrok.png)
<h3>A cosa serve</h3>
Quando si sviluppa un nuovo progetto software, una delle cose che ne determinano la riuscita è il feedback continuo del cliente, mostrargli gli avanzamenti in modo costante e continuo infatti, lo rende più partecipe e si riducono gli errori derivanti da requisiti non compresi a pieno.
Non a caso questo infatti è uno dei principi cardine di una tra le principali [metodologie Agili](https://it.wikipedia.org/wiki/Metodologia_agile), *[XP Programming](https://it.wikipedia.org/wiki/Extreme_programming)* che parla proprio di **[On site Customer](http://www.extremeprogramming.org/rules/customer.html)**.<br/><br/>
Un altro ambito molto utile, potrebbe essere la condivisione con altri membri del Team di progetto della propria versione locale del prodotto, se lavorano in sedi differenti o come sempre più spesso accade in **[Remote Working](https://www.ideato.it/news-for-all/remote-working-istruzioni-per-luso)**. <br/>

<h3>Tunnel sicuri</h3>
Per risolvere queste problematiche una soluzione che ho scoperto da poco è il servizio **[ngrok](https://ngrok.com)**.
Questo servizio, effettua un tunnel sicuro dell'indirizzo locale (localhost), anche se la propria rete è dietro un proxy oppure un firewall, cosa tipica nelle reti aziendali.<br/>
Ma andiamo subito a vedere come funziona, la prima cosa da fare come ovvio che sia, è il download del software per la propria piattaforma (Mac, Windows, Linux) da questo [indirizzo](https://ngrok.com/download "indirizzo").<br/>

<h3>Comando</h3>
Una volta scaricato e scompattato se lanciamo il comando senza parametri abbiamo questo risultato:
{% highlight bash %}
$ ./ngrok
NAME:
   ngrok - tunnel local ports to public URLs and inspect traffic

DESCRIPTION:
    ngrok exposes local networked services behinds NATs and firewalls to the
    public internet over a secure tunnel. Share local websites, build/test
    webhook consumers and self-host personal services.
    Detailed help for each command is available with 'ngrok help <command>'.
    Open http://localhost:4040 for ngrok's web interface to inspect traffic.

EXAMPLES:
    ngrok http 80                    # secure public URL for port 80 web server
    ngrok http -subdomain=baz 8080   # port 8080 available at baz.ngrok.io
    ngrok http foo.dev:80            # tunnel to host:port instead of localhost
    ngrok tcp 22                     # tunnel arbitrary TCP traffic to port 22
    ngrok tls -hostname=foo.com 443  # TLS traffic for foo.com to port 443
    ngrok start foo bar baz          # start tunnels from the configuration file

VERSION:
   2.0.25

AUTHOR:
  inconshreveable - <alan@ngrok.com>

COMMANDS:
   authtoken    save authtoken to configuration file
   credits      prints author and licensing information
   http         start an HTTP tunnel
   start        start tunnels by name from the configuration file
   tcp          start a TCP tunnel
   test         test ngrok service end-to-end
   tls          start a TLS tunnel
   update       update ngrok to the latest version
   version      print the version string
   help         Shows a list of commands or help for one command
    {% endhighlight %}
  Il semplice help del comando è di per sé molto esplicativo, lo scenario classico che andrà bene per molti progetti Web, è quello di avere un Web Server per esempio **[Apache](https://httpd.apache.org)**, sulla porta standard **80**, in questo caso basterà digitare questo comando:
{% highlight bash %}
   ./ngrok http 80 
{% endhighlight %}
per mostrare al mondo il nostro capolavoro di progetto.<br/>
Nel mio caso specifico, di sviluppatore Java non utilizzo Apache2 per i miei siti  bensì [Tomcat](http://tomcat.apache.org) che di *default* crea una connessione sulla porta 8080 quindi:
{% highlight bash %}
   ./ngrok http 8080 
{% endhighlight %}
Una volta lanciato si presenta in questo modo:
{% highlight bash %}
ngrok by @inconshreveable                                                                                                                               (Ctrl+C to quit)

Tunnel Status                 reconnecting (dial tcp 198.58.96.133:443: ConnectEx tcp: i/o timeout)
Version                       2.0.25/
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       0       0.00    0.00    0.00    0.00
{% endhighlight %}
In questo modo però il processo non è in background per lanciarlo in background, basterà aggiungere questo:
{% highlight bash %}
 ./ngrok http 8080 --log=stdout
{% endhighlight %}
tutto molto semplice ed immediato.
Se invece fossimo dietro ad un server proxy che non permette ad ngrok di connettersi, basta impostare la variabile d'ambiente in questo modo.
{% highlight bash %}
export http_proxy="http://username:password@host:port"
ngrok http 8080
{% endhighlight %}
Il servizio ngrok infatti, utilizza la variabile ambiente *http_proxy* in pieno stile linux (funziona anche su macchine Windows).<br/>
Proviamo a vedere cosa succede se decido di mostrare al mondo il mio blog direttamente dalla versione ospitata sulla mia macchina locale senza deploy su Github.<br/>
Il server [Jekyll](https://jekyllrb.com) lanciato con il comando ```jekyll serve``` si mette in ascolto su ```localhost:4000```andiamo a vedere come utilizzare ngrok: 
{% highlight bash %}
./ngrok http 4000

ngrok by @inconshreveable                                                                                                                                                       (Ctrl+C to quit)

Tunnel Status                 online
Version                       2.0.25/2.0.25
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://020de402.ngrok.io -> localhost:4000
Forwarding                    https:///020de402.ngrok.io -> localhost:4000

Connections                   ttl     opn     rt1     rt5     p50     p90
                              10      0       0.00    0.00    31.03   32.86

HTTP Requests
-------------

GET /public/favicon.ico                                 200 OK
GET /public/font-awesome/fonts/fontawesome-webfont.woff 200 OK
GET /assets/codemotion_2016_header.png                  200 OK
GET /assets/jekyll_header.png                           200 OK
GET /assets/rest_API.png                                200 OK
GET /assets/ascatox.jpg                                 200 OK
GET /public/font-awesome/css/font-awesome.min.css       200 OK
GET /public/css/ascatox.css                             200 OK
GET /public/css/syntax.css                              200 OK
GET /public/css/poole.css                               200 OK
{% endhighlight %}

Il mio sito jekyll locale è ora accessibile tramite due indirizzi pubblici della rete di ngrok **http://020de402.ngrok.io** e  **https://020de402.ngrok.io** come si vede da queste immagini.
<br/><br/>
![Jekyll]({{site.baseurl}}/assets/ngrok_localhost.png)
![Jekyll]({{site.baseurl}}/assets/ngrok_http.png)
![Jekyll]({{site.baseurl}}/assets/ngrok_https.png)
<br/>
Una cosa per tutti coloro che storceranno il naso nel vedere gli indirizzi, ngrok è un servizio che offre dei piani a pagamento dove fornisce molte funzionalità aggiuntive,
tra le quali anche la possibilità di poter utilizzare degli indirizzi personalizzati.<br/>
Un'altra possibilità è quella di modificare gli indirizzi in modo da appartenere al proprio dominio come spiegato nell'apposita 
[documentazione](https://ngrok.com/docs#custom-domains).
<h3>Conclusioni</h3>
Questa è solamente una piccola introduzione al servizio ngrok, uno strumento molto potente che permette di gestire anche situazioni molto complesse come viene spiegato nella [documentazione ufficiale](https://ngrok.com/docs "documentazione ufficiale") e nelle [FAQ](https://ngrok.com/faq "FAQ") e che in tantissime situazioni può davvero essere molto utile.<br/><br/>
PS: Per chi vuole approfondire, consiglio [questo post](http://unnikked.tk/ngrok-tunnel-sicuri-su-localhost/) e [questo](http://unnikked.tk/come-installare-ngrok-con-un-certificato-self-signed/) che forniscono una guida esaustiva al servizio.
<br/>
Interessante poi il [post di Odino](http://odino.org/how-docker-changed-me/), che mostra l'utilizzo di ngrok all'interno di un container [docker](https://www.docker.com).
<br/>
<br/> 
**Stay Tuned!**
<br/>
<br/>
*Image Courtesy of [ngrok.com](https://ngrok.com)*