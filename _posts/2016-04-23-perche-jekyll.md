---
layout: post
title: Perchè Jekyll
published: true
categories:
categories:
- Tech
tags:
- Jekyll
- Blog
---
![Jekyll]({{site.baseurl}}/assets/jekyll_header.png)

<h3>Perchè Jekyll</h3>

Sono molti anni che possiedo questo [blog](http://www.antonioscatoloni.it/blog), ma purtroppo come spesso succede, dopo la fiammata iniziale ho abbandonato il blog per molti molti motivi. 
Un motivo che mi teneva lontano dallo scrivere nel blog è sempre stato [Wordpress](https://wordpress.org/), la gestione di questo famoso CMS per i miei utilizzi era troppo macchinosa e lenta, inoltre non riuscivo mai a trovare un tema, neanche modificandolo che mi soddisfacesse a pieno.

Quando ho iniziato a conoscere [Jekyll](https://jekyllrb.com/) è stato subito amore a prima vista per questo blog engine di tipo statico, semplice ma molto efficace.
[Questo post](http://paulstamatiou.com/how-to-wordpress-to-jekyll/) di [Paul Stamatiou](http://paulstamatiou.com/about/) è stato il mio apripista per il passaggio da Wordpress a Jekyll.

Questo post nasce proprio con questo scopo, un **piccolo diario di questo transizione** con una serie di mie considerazioni personali, sui passaggi più difficili, sia sulla migrazione, che sull'utilizzo.

*Disclaimer: Per una guida esaustiva per effettuare la migrazione, consiglio caldamente quella di [Girliemac](http://www.girliemac.com/blog/2013/12/27/wordpress-to-jekyll/) o quella di [Paul Stamatiou](http://paulstamatiou.com/how-to-wordpress-to-jekyll) già citata in precedenza.*

<h3>Punti critici della mia migrazione</h3>

La mia migrazione non è stata molto facile ed ancora adesso non è del tutto completa, sia per quanto riguarda il layout sia per i contenuti. Mi mancano infatti tutti i vecchi commenti che non sono riuscito a spostare sulla piattaforma [Disqus](https://disqus.com/) e le vecchie categorie, che ho deciso di abbandonare, per ricominciare una una nuova avventura con temi diversi.

1. Dopo aver installato Jekyll sul mio Mac con il classico comando: 
`gem install jekyll`

2. La prima cosa che ho fatto, come suggerito in [moltissime guide](http://girliemac.com/blog/2013/12/27/wordpress-to-jekyll/) è stata quella di esportare tutti i contenuti dal mio [Blog Wordpress](http://www.antonioscatoloni.it/blog) self hosted  alla nuova struttura Jekyll.
Ho provato ad utilizzare diversi plugin Wordpress, in particolare [Jekyll Exporter](https://it.wordpress.org/plugins/jekyll-exporter/), ma a causa di una serie di errori derivanti dalla mancanza di alcune librerie nel mio hosting, ho deciso di utilizzare una soluzione più radicale.
Ho scaricato il file contenente tutti i miei post in formato xml, direttamente dalla Dashboard di Wordpress nella sezione *Strumenti->Esporta*. In questo modo ho generato un file xml contenente le informazioni relative ai post.

3. Il passo successivo è stato quello di utilizzare uno script Ruby dalla console Ruby, fornito direttamente sul sito di [Jekyll](https://import.jekyllrb.com/docs/wordpressdotcom/) che serve per generare i post in formato Html o [Markdown](https://daringfireball.net/projects/markdown/) pronti (o quasi :-D) per la nuova piattaforma.
{% highlight bash %}
$ ruby -rubygems -e 'require "jekyll-import";
    JekyllImport::Importers::WordpressDotCom.run({
      "source" => "wordpress.xml",
      "no_fetch_images" => false,
      "assets_folder" => "assets"
    })
 {% endhighlight %}
*Attenzione il file wordpress.xml è quello esportato in precedenza, è possibile scegliere una cartella con nome diverso per le immagini scaricate.*<br/>
Proprio in questi giorni ho notato che si può utilizzare un'altro [script](http://import.jekyllrb.com/docs/wordpress/), che accede direttamente al DB Mysql del proprio sito Wordpress.<br/>
*Comunque per coloro che vogliono spostare il loro sito da Wordpress self hosted a Jekyll il plugin sopra citato è una soluzione piu efficace e semplice.*

Mi rimangono ancora alcuni punti aperti che spero di risolvere il prima possibile, specialmente il trasferimento dei commenti presenti sul vecchio Blog.
La soluzione da tutti adottatata è quella di installare il [plugin per Wordpress di Disqus](https://wordpress.org/plugins/disqus-comment-system/), esportare il tutto in un file e poi importare il tutto nell'[apposita sezione](https://import.disqus.com/login/?next=/) del sito Disqus. Nel mio caso tutto è andato come doveva andare ma bisogna stare attenti che **il formato dei link dei post** sia lo stesso tra il vecchio Blog e quello con Jekyll. Ogni link deve avere lo stesso identico formato permalink per esempio:
```http://www.antonioscatoloni.it/blog/2016/03/21/codemotion-una-bella-giornata-prima-parte diventa
http://ascatox.github.io/2016/03/21/codemotion-una-bella-giornata-prima-parte ``` ed ovviamente il dominio con il sottolivello devono essere uguali, nel mio caso (temporaneo) non è così, quindi l'associazione che esegue Disqus non funziona.

<h3>Note sull'utilizzo</h3>

Devo dire che all'inizio ero molto intimorito di scrivere post con Jekyll, l'utilizzo della piattaforma infatti si appoggia tutto su Github ed in particolare sulle [Github Pages](https://pages.github.com/).
Per farla molto semplice, una volta creato un repo si Github con nome ``username.github.io`` nel mio caso quindi [ascatox.github.io](https://github.com/ascatox/ascatox.github.io), basta gittare nel repo il sito  creato con Jekyll in locale e GitHub lo ospiterà per noi, senza dover pagare un centesimo.<br/>
I post sono semplicemente dei file scritti in Markdown con una particolare intestazione che devono essere gittati nel repo, per essere pubblicati. Il workflow di pubblicazione di un post può sembrare molto più laborioso di quanto lo fosse su Wordpress, ma una volta presa la mano e con i giusti strumenti, diventa [meno complesso](http://dottorblaster.it/2015/12/jekyll-flusso-pubblicazione).

<h3>Applicazioni per creare post</h3>

L'applicazione che ho deciso di usare per scrivere i miei post è [Mou](http://25.io/mou/), la trovo semplice ed efficace, peccato che non supporti lo [YAML front matter di Jekyll](https://jekyllrb.com/docs/frontmatter/).
Una buona alternativa per chi non vuole mettere troppo le mani nel Markdown, potrebbe essere il [Jekyll Editor](https://chrome.google.com/webstore/detail/jekyll-editor/dfdkgbhjmllemfblfoohhehdigokocme "Jekyll Editor"), un'applicazione per Chrome che presenta un'interfaccia molto simile all'editor di Wordpress e che genera la sintassi Markdown. Al momento però presenta ancora numerosi bug in fase di creazione che ne rendono un pò complicato l'utilizzo.

Per adesso è tutto.<br/>
Il mio **Blog riparte da qui**, nuova piattaforma nuova energia.
<br/>
Stay Tuned!

*PS: Se volete effettuare un fork del [mio umile Weblog](https://github.com/ascatox/ascatox.github.io) fatelo pure.*
<br/>
Update: Purtroppo ho scoperto che con il mio piano di Hosting presso Aruba, non è possibile collegare al mio dominio antonioscatoloni.it spazi diversi da quelli di Aruba stessi, quindi non era possibile agganciare il mio dominio a Github Pages.
<br/>
Ho deciso quindi di comprare un nuovo dominio presso [Namecheap](https://www.namecheap.com) ed ho scelto **antonioscatoloni.site**.
<br/><br/>
Fatto questo ho aggiunto un file *[CNAME](https://github.com/ascatox/ascatox.github.io/blob/master/CNAME)* al mio repo Github con dentro semplicemente ```antonioscatoloni.site``` poi mi sono recato nel [pannello di controllo](https://ap.www.namecheap.com/Domains/DomainControlPanel) di Namecheap, nella sezione **Advanced DNS** ho inserito gli indirizzi IP di Github ed l'indirizzo della mia Github page che ricordo essere **username.github.io**.<br/>
![Jekyll]({{site.baseurl}}/assets/CNAME.png)