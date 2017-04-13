<properties 
    pageTitle="A Web Apps alkalmazások Linux NodeJS PM2 konfigurációval |} Microsoft Azure" 
    description="A Web Apps alkalmazások Linux NodeJS PM2 konfiguráció használata" 
    keywords="Azure alkalmazás szolgáltatás, webalkalmazás, nodejs, pm2, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="using-pm2-configuration-for-nodejs-in-web-apps-on-linux"></a>A Web Apps alkalmazások Linux Node.js PM2 konfiguráció használata

Ha az alkalmazás objektumhalomban Node.js Web Apps alkalmazások Linux, állíthat be az alábbi képen látható módon Node.js indítási fájl jelenik meg.

![][1]

Ezzel vagy

-   Adja meg az indítási parancsfájlt, az Node.js számára (például: /bin/server.js)
-   Adja meg a Node.js alkalmazásokban használandó PM2 kereséskonfigurációs fájlt (például: /foo/process.json)

 >[AZURE.NOTE] Ha azt szeretné, hogy az automatikusan indítani az egyes fájlok módosításakor a csomópont folyamatok, szüksége lesz PM2 konfiguráció használata. Egyéb esetben az alkalmazás nem indul Ha kapott módosítása értesítések összetevőjét, például a folyamatos telepítési az alkalmazás kódja megváltozásakor.

Az összes beállítást Node.js [folyamat fájl dokumentáció](http://pm2.keymetrics.io/docs/usage/application-declaration/) ellenőrizni, de az alábbi képen egy mintája szerepel az process.json fájlként szeretne használata

        {
          "name"        : "worker",
          "script"      : "/bin/server.js",
          "instances"   : 1,
          "merge_logs"  : true,
          "log_date_format" : "YYYY-MM-DD HH:mm Z",
          "watch": ["/bin/server.js", "foo.txt"],
          "watch_options": {
            "followSymlinks": true,
            "usePolling"   : true,
            "interval"    : 5
          }
        }

Fontos megjegyzés ebben a konfigurációban vannak 

-   A "parancsfájl" tulajdonság adja meg az alkalmazás indítása parancsfájl.
-   A "példányok" tulajdonság adja meg a csomópont folyamat elindításához hány példánya. Ha az alkalmazás nagyobb virtuális méretek, amelyek több magmintákat futtat, érdemes az erőforrások maximalizálása Itt a magasabb értékek megadásával.
-   A "Megtekintés" tömböt adja meg az összes fájlt, amelynek módosítására el szeretné indítani a csomópont folyamatok.
-   "Watch_options" meg kell adnia a "usePolling" igaz a alkalmazás tartalom csatlakoztatva van módja miatt jelenleg.


## <a name="next-steps"></a>Következő lépések ##

* [Mi az alkalmazás szolgáltatás Linux?](./app-service-linux-intro.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png