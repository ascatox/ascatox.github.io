---
layout: post
title: Introduzioe a ngrock
published: true
categories: Servizi Web
tags:
- DevOps
---
<h3>A cosa serve</h3>
Quando si sviluppa un nuovo progetto software, una delle cose che ne determinano la riuscita è il feedback continuo del cliente, mostrargli gli avanzamenti in modo costante e continuo infatti, lo rende più partecipe e si riducono gli errori derivanti da requisiti non compresi a pieno.
<br/><br/>
Non a caso questo infatti è uno dei principi cardine di tra le principali metodologie Agili, XP Programming che parla proprio di *Customer on site*.
Spesso, realizzare un'ambiente, dove il cliente possa visionare il prodotto nel durante, sarebbe una delle cose da pensare subito, nella fase di boostrap di un progetto, ma spesso questo è molto difficile per molti motivi, la soluzione più facile sarebbe mostrare il prodotto che si sta visonando in una macchina di sviluppo, ma questo potrebbe essere non immediato.<br/>

<h3>Tunnel sicuri</h3>
Una soluzione che ho scoperto da poco è il servizio ngrock.
Questo servizio, effettua un tunnel sicuro a localhost anche se la propria rete è dietro un proxy oppure un firewall, cosa tipica nelle reti aziendali.
Ma andiamo subito a vedere come funziona, la prima cosa da fare come ovvio che sia è il download del software per la propria piattaforma (Mac, Windows, Linux) da questo [indirizzo](https://ngrok.com/download "indirizzo").<br/>

<h3>Comandi</h3>
Una volta scaricato e scompattato se lanciamo il comando senza parametri abbiamo questo risultato:
{% highlight bash %}
$ ngrok
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
  Il semplice help del comando è di per sé molto esplicativo, lo scenario classico che andrà bene per molti progetti Web è quello di avere un Web Server per esempio **Apache2** oppure **Nginx** sulla porta standard **80**, in questo caso basterà digitare questo comando:
{% highlight bash %}
   ngrok http 80 
  {% endhighlight %}
per mostrare al mondo il nostro capolavoro di progetto.<br/>
Nel mio caso specifico, di sviluppatore Java non utilizzo Apache2 per i miei siti  bensì Tomcat che di *default* crea una connessione sulla porta 8080 quindi:
{% highlight bash %}
   ngrok http 8080 
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
 ngrok http 8080 --log=stdout
{% endhighlight %}
tutto molto semplice ed immediato.
Se invece fossimo dietro ad un server proxy che non permette ad ngrock di connettersi, basta impostare la variabile d'ambiente in questo modo:
{% highlight bash %}
export http_proxy="http://username:password@host:port"
ngrock http 8080
{% endhighlight %}
ngrock infatti utilizza la variabile ambiente in pieno stile dei programmi linux (anche su macchine Windows).
Proviamo a vedere cosa succede se decido di mostrare al mondo il mio blog direttamente dalla versione ospitata sulla mia macchina locale senza deploy su Github.<br/>
Il server jekyll lanciato con il comando ```jekyll serve``` si mette in ascolto su ```localhost:4000```andiamo a vedere come utilizzare ngrock: 
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

Il mio jekyll locale è accessibile in due indirizzi della rete di ngrock **http://020de402.ngrok.io** e  **https://020de402.ngrok.io** come si vede da queste immagini:
![Jekyll]({{site.baseurl}}/assets/ngrock_localhost.png)
![Jekyll]({{site.baseurl}}/assets/ngrock_http.png)
![Jekyll]({{site.baseurl}}/assets/ngrock_https.png)

Una cosa per tutti coloro che storceranno il naso nel vedere gli indirizzi, ngrock è un servizio che offre dei piani a pagamento dove fornisce molte funzionalità aggiuntive,
tra le quali anche la possibilità di poter utilizzare degli indirizzi personalizzati.
<br/>
Per concludere ngrock è uno strumento molto potente che permette di gestire anche situazioni molto complesse come viene spiegato nella [documentazione ufficiale](https://ngrok.com/docs "documentazione ufficiale") e nelle [FAQ](https://ngrok.com/faq "FAQ") e che in tantissime situazioni può davvero essere davvero molto utile.<br/>

Interessante il post di Odino che mostra l'utilizzo di ngrock all'interno di un container docker, davvero un servizio utile.


