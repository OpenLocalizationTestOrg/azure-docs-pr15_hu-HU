Miközben a kód MongoLab URI beillesztése lehetőség, javasoljuk, hogy konfigurálja úgy a környezetben esetében könnyű kezelés. Ezzel a módszerrel, ha megváltozik a URI, módosíthatja azt az Azure-portálon keresztül nélkül a kódot.


1. Az Azure-portálon válassza a **Web Apps alkalmazások**.
1. Kattintson a nevére a web app a Web Apps alkalmazások listájában.  
![WebAppEntry][entry-website]  
A Web App irányítópult jeleníti meg.

1. A menüsávon kattintson a **beállítás** gombra.  
![WebAppDashboardConfig][focus-mongolab-websitedashboard-config]

1. Görgessen le a kapcsolati karakterláncot szakaszban.  
![WebAppConnectionStrings][focus-mongolab-websiteconnectionstring]

1. Írja be a **nevét**, MONGOLAB_URI.
1. **Érték**esetén illessze be a kapcsolati karakterlánc azt az előző szakaszban kapott.
1. Válassza az **egyéni** (helyett az alapértelmezett **SQLAzure**), a **típus** legördülő listában.
1. Az eszköztáron kattintson a **Mentés** gombra.  
![SaveWebApp][button-website-save]

**Megjegyzés:** Azure ad a **CUSTOMCONNSTR\_ ** előtag a változó, ezért a fenti hivatkozások kódot **CUSTOMCONNSTR\_MONGOLAB_URI.**

[entry-website]: ./media/howto-save-connectioninfo-mongolab/entry-website.png
[focus-mongolab-websitedashboard-config]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websitedashboard-config.png
[focus-mongolab-websiteconnectionstring]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websiteconnectionstring.png
[button-website-save]: ./media/howto-save-connectioninfo-mongolab/button-website-save.png
