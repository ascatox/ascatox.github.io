###Perchè Jekyll (prima parte)
Sono molti anni che possiedo questo blog, ma purtroppo come spesso succede dopo la fiammata iniziale ho abbandonato il blog per molti molti motivi. 
Un motivo che mi teneva lontano dallo scrivere nel blog è sempre stato Wordpress, la gestione di questo famoso CMS per i miei utilizzi era troppo macchinoso e lento, inoltre poi non riuscivo mai a trovare un tema, neanche modificandolo che mi soddisfacesse a pieno.

Quando ho iniziato a conoscere [Jekyll](https://jekyllrb.com/) è stato subito amore per questo blog engine statico, semplice ma molto efficace.
[Questo post](http://paulstamatiou.com/how-to-wordpress-to-jekyll/) di [Paul Stamatiou](http://paulstamatiou.com/about/) è stato il mio apripista per il passaggio da Wordpress a Jekyll.

Questo post nasce proprio per questo, un piccolo diario di questo transizione con una serie di mie considerazioni personali, sui passaggi più difficili sia sulla migrazione, sia sull'utilizzo.

Per una guida esaustiva per effettuare la migrazione consiglio caldamente quella di [Girliemac](http://www.girliemac.com/blog/2013/12/27/wordpress-to-jekyll/) o quella di Paul Stamatiou già citata in precedenza.

###Considerazioni sulla migrazione
La mia migrazione non è stata molto facile ed ancora non del tutto completa, né come layout né come contenuti, mi mancano infatti tutti i vecchi commenti che non sono riuscito a spostare sulla piattaforma [Disqus](https://disqus.com/) e le vecchie categorie, che ho deciso di abbandonare per partire in una nuova avventura con temi diversi.

1. Dopo aver installato Jekyll sul mio Mac con il classico comando: 
`gem install jekyll`

2. La prima cosa che ho fatto, come suggerito in [moltissime guide](http://girliemac.com/blog/2013/12/27/wordpress-to-jekyll/) è stata quella di esportare tutti i contenuti dal mio [Blog Wordpress](http://www.antonioscatoloni.it/blog) self hosted  alla nuova struttura Jekyll.
Ho provato ad utilizzare diversi plugin Wordpress, in particolare [Jekyll Exporter](https://it.wordpress.org/plugins/jekyll-exporter/), ma a causa di una serie di errori derivanti dalla mancanza di librerie nel mio hosting ho deciso di utilizzare una soluzione più radicale.
Ho scaricato il file contenente tutti i miei post in formato xml, direttamente dalla Dashboard di Wordpress nella sezione *Strumenti->Esporta*, in questo modo ho generato un file xml contenente le informazioni relative ai post.

3. Il passo successivo è stato quello di utilizzare uno script ruby direttamente dalla console ruby, fornito direttamente sul sito di [Jekyll](https://import.jekyllrb.com/docs/wordpressdotcom/) che serve per generare i post in formato html o Markdown pronti (o quasi :-D) per la nuovo blog engine. 
`$ ruby -rubygems -e 'require "jekyll-import";
    JekyllImport::Importers::WordpressDotCom.run({
      "source" => "wordpress.xml",
      "no_fetch_images" => false,
      "assets_folder" => "assets"
    })'`
Attenzione il file *wordpress.xml* è quello esportato in precedenza, è possibile scegliere una cartella con nome diverso per le immagini scaricate.
Proprio in questi giorni ho notato che si può utilizzare un'altro [script](http://import.jekyllrb.com/docs/wordpress/) che accede direttamente  al db Mysql del proprio blog Wordpress.
*Comunque per coloro che vogliono spostare il loro sito da Wordpress self hosted a Jekyll il plugin sopra citato è una soluzione piu efficace e semplice.*

Continua...