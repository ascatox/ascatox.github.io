---
layout: post
title: Perchè Jekyll
published: false
categories:
categories:
- Tech
tags:
- Jekyll
- Blog
---
###Perchè Jekyll
Sono molti anni che possiedo questo blog, ma purtroppo come spesso succede, dopo la fiammata iniziale ho abbandonato il blog per molti molti motivi. 
Un motivo che mi teneva lontano dallo scrivere nel blog è sempre stato Wordpress, la gestione di questo famoso CMS per i miei utilizzi era troppo macchinosa e lenta, inoltre non riuscivo mai a trovare un tema, neanche modificandolo che mi soddisfacesse a pieno.

Quando ho iniziato a conoscere [Jekyll](https://jekyllrb.com/) è stato subito amore a prima vista per questo blog engine statico, semplice ma molto efficace.
[Questo post](http://paulstamatiou.com/how-to-wordpress-to-jekyll/) di [Paul Stamatiou](http://paulstamatiou.com/about/) è stato il mio apri pista per il passaggio da Wordpress a Jekyll.

Questo post nasce proprio con questo scopo, un piccolo diario di questo transizione con una serie di mie considerazioni personali, sui passaggi più difficili, sia sulla migrazione, che sull'utilizzo.

*Disclaimer: Per una guida esaustiva per effettuare la migrazione consiglio caldamente quella di [Girliemac](http://www.girliemac.com/blog/2013/12/27/wordpress-to-jekyll/) o quella di Paul Stamatiou già citata in precedenza.*

###Punti critici della mia migrazione
La mia migrazione non è stata molto facile ed ancora non del tutto completa, né nei layout né nei contenuti. Mi mancano infatti tutti i vecchi commenti che non sono riuscito a spostare sulla piattaforma [Disqus](https://disqus.com/) e le vecchie categorie, che ho deciso di abbandonare, per partire in una nuova avventura con temi diversi.

1. Dopo aver installato Jekyll sul mio Mac con il classico comando: 
`gem install jekyll`

2. La prima cosa che ho fatto, come suggerito in [moltissime guide](http://girliemac.com/blog/2013/12/27/wordpress-to-jekyll/) è stata quella di esportare tutti i contenuti dal mio [Blog Wordpress](http://www.antonioscatoloni.it/blog) self hosted  alla nuova struttura Jekyll.
Ho provato ad utilizzare diversi plugin Wordpress, in particolare [Jekyll Exporter](https://it.wordpress.org/plugins/jekyll-exporter/), ma a causa di una serie di errori derivanti dalla mancanza di alcune librerie nel mio hosting ho deciso di utilizzare una soluzione più radicale.
Ho scaricato il file contenente tutti i miei post in formato xml, direttamente dalla Dashboard di Wordpress nella sezione *Strumenti->Esporta*. In questo modo ho generato un file xml contenente le informazioni relative ai post.

3. Il passo successivo è stato quello di utilizzare uno script Ruby direttamente dalla console Ruby, fornito direttamente sul sito di [Jekyll](https://import.jekyllrb.com/docs/wordpressdotcom/) che serve per generare i post in formato Html o Markdown pronti (o quasi :-D) per la nuova piattaforma. ```$ ruby -rubygems -e 'require "jekyll-import";
    JekyllImport::Importers::WordpressDotCom.run({
      "source" => "wordpress.xml",
      "no_fetch_images" => false,
      "assets_folder" => "assets"
    })'```
Attenzione il file *wordpress.xml* è quello esportato in precedenza, è possibile scegliere una cartella con nome diverso per le immagini scaricate.
Proprio in questi giorni ho notato che si può utilizzare un'altro [script](http://import.jekyllrb.com/docs/wordpress/) che accede direttamente al DB Mysql del proprio sito Wordpress.
*Comunque per coloro che vogliono spostare il loro sito da Wordpress self hosted a Jekyll il plugin sopra citato è una soluzione piu efficace e semplice.*

Mi rimangono ancora alcuni punti aperti che spero di risolvere il prima possibile, specialmente il trasferimento dei commenti presenti sul vecchio Blog. La soluzione da tutti adottatata è quella di installare il plugin per Wordpress di Disqus, esportare il tutto in un file e poi importare il tutto nell'[apposita sezione](https://import.disqus.com/login/?next=/) del sito Disqus. Nel mio caso tutto è andato come doveva andare ma bisogna stare attenti che **il formato dei link dei post** sia lo stesso tra il vecchio Blog e quello con Jekyll. Ogni link deve avere lo stesso identico formato permalink per esempio:
```http://www.antonioscatoloni.it/blog/2016/03/21/codemotion-una-bella-giornata-prima-parte diventa
http://ascatox.github.io/2016/03/21/codemotion-una-bella-giornata-prima-parte ``` ed ovviamente il dominio con il sottolivello devono essere uguali, nel mio caso (temporaneo) non è così quindi l'associazione che esegue Disqus non funziona.

###Note sull'utilizzo
Devo dire che all'inizio ero molto intimorito di scrivere post con Jekyll, l'utilizzo della piattaforma infatti si appoggia tutto su Github ed in particolare sulle Github Pages. Per farla semplice, una volta creato un repo si Github con nome ``username.github.io`` nel mio caso quindi [ascatox.github.io](https://github.com/ascatox/ascatox.github.io), basta gittare nel repo il sito  creato con Jekyll in locale e GitHub lo ospiterà per noi, senza dover pagare un centesimo. I post sono semplicemente dei file scritti in Markdown con una particolare intestazione che devono essere gittati nel repo, per essere pubblicati. Il workflow di pubblicazione di un post può sembrare molto più laborioso di quanto lo fosse su Wordpress, ma una volta presa la mano e con i giusti strumenti, diventa meno complesso[^1].
L'applicazione che ho deciso di usare per scrivere i miei post è [Mou](http://25.io/mou/), la trovo semplice ed efficace, peccato che non supporti lo [YAML front matter di Jekyll](https://jekyllrb.com/docs/frontmatter/).

Per adesso è tutto, ma il mio** Blog riparte da qui**, nuova piattaforma nuova energia.
Stay Tuned!

[^1]: http://dottorblaster.it/2015/12/jekyll-flusso-pubblicazione/

