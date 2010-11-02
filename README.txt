What Is This?
=============
This sample app is just a translation of play-gae[1]'s lists-with-gae[2] in Scala 
with Scalate[3][4].


How To Try It?
==============
This sample app will work with play-1.1, but the scalate module is not ready
for it.  So, we will build its jar from its source code.

  $ pwd
  /tmp
  $ wget http://download.playframework.org/releases/play-1.1.zip
  $ unzip play-1.1.zip
  $ export PLAY_PATH=/tmp/play-1.1
  $ export PATH=$PLAY_PATH:$PATH
  $ play install scala
  $ play install gae
  $ play install siena

  $ wget wget http://googleappengine.googlecode.com/files/appengine-java-sdk-1.3.7.zip
  $ unzip appengine-java-sdk-1.3.7.zip
  $ export GAE_PATH=/tmp/appengine-java-sdk-1.3.7

  $ git clone git://github.com/ymnk/play-scalate.git scalate-ymnk-trunk
  $ (cd scalate-ymnk-trunk; ant -Dplay.path=$PLAY_PATH)
  $ mv scalate-ymnk-trunk $PLAY_PATH/modules/

  $ git clone git://github.com/ymnk/lists-with-play-scalate-gae.git
  $ cd lists-with-play-scalate-gae
  $ vi war/WEB-INF/appengine-web.xml // set your GAE app-id into application tag.
  $ play gae:deploy


Notes
=====
  * Refer to properties 'scalate.allowReload' and 'scalate.workdir' 
    in war/WEB-INF/appengine-web.xml.  These properties are mandatory to run Scalate
    on GAE.

  * This app is using siena[6] to manipulate the datastore.  The original sample
    app has been using @Filter annotation, but it seems it will cause an exception 
    on Scala.  So that annotation is not used in this app.
    Refer to app/models/List.scala.

  * The original 'lists-with-gae' has a problem in deleting an item.
    It seems '/lists/deleteitem/?id=x&itemId=y' has been encoded as
    '/lists/deleteitem/?id=x&amp;itemId=y' unexpectedly.
    To work around this problem, a following line has bee added to conf/routes

    POST    /lists/{<\d+>id}/items/{<\d+>itemId}/delete  Lists.deleteItem

  * I don't know the reason, but on my configuration a mail notification 
    functionality was not worked with original code.
    So I'm using GAE's mail APIs instead of Play!'s mailer APIs.
    Refer to app/notifiers/Notifier.scala


[1] http://www.playframework.org/modules/gae
[2] http://github.com/guillaumebort/play-gae/tree/master/samples-and-tests/lists-with-gae/
[3] http://scalate.fusesource.org/
[4] http://www.playframework.org/modules/scalate
[5] http://github.com/ymnk/play-scalate
[6] http://www.playframework.org/modules/siena

