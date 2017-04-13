<properties
    pageTitle="Gyakorlati tanácsok az Azure alkalmazás szolgáltatás"
    description="További tudnivalók a gyakorlati tanácsokat és Azure alkalmazás szolgáltatás kapcsolatos problémák megoldása."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="best-practices-for-azure-app-service"></a>Gyakorlati tanácsok az Azure alkalmazás szolgáltatás

Ez a cikk a gyakorlati tanácsok az [Azure](http://go.microsoft.com/fwlink/?LinkId=529714)-alkalmazás szolgáltatással foglalja össze. 

## <a name="colocation"></a>Colocation
Ha Azure erőforrások, például a megfelelő web App alkalmazásban és az adatbázis megoldást szerkesztési különböző régióban találhatók az effektusok a következők lehetnek:

*  Az erőforrások közötti kommunikáció megnövelt időtartama
*  Kimenő adat pénzügyi díjai határokon-régió az [Azure árak lap](https://azure.microsoft.com/pricing/details/data-transfers)amint át.

Ugyanabban a régióban colocation a legjobb megoldást például webalkalmazást, és egy adatbázist, illetve a tárhely fiók tartalmat vagy adatok tárolására szolgáló szerkesztési Azure erőforrások. Győződjön meg arról, hogy erőforrások létrehozásakor vannak ugyanabban a Azure régióban kivéve, ha meghatározott üzleti van, és a tervezési őket, hogy nem lehet az oka. Régió ugyanazt az adatbázis alkalmazás szolgáltatás alkalmazás mozgatja vizsgál, az [alkalmazás szolgáltatás klónozhatja funkció](app-service-web-app-cloning-portal.md) jelenleg elérhető az alkalmazás prémium alkalmazás szolgáltatás megtervezése.   

## <a name="memoryresources"></a>Ha a alkalmazások vártnál több memóriát igényelnek
Azt tapasztalja, amikor az alkalmazás fogyasztása további memóriahasználat figyelése keresztül megjelölt vártnál, vagy szolgáltatás javaslatok, fontolja meg az [App szolgáltatás automatikus javítás funkció](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Az automatikus javítás funkció lehetőségek közül a memória küszöbértéket alapján egyéni műveletek tart. Műveletek a dokumentumhasználat vizsgálatot memória kiírása, hogy a helyszíni kezelési keresztül az e-mail időtartamát, a munkafolyamat újrafelhasználás szerint. Automatikus javító is beállítható, és egy felhasználóbarát kezelőfelület című témakörben leírt módon a blogbejegyzésben az [Alkalmazás támogatási webhely bővítmény](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps)web.config keresztül.   

## <a name="CPUresources"></a>Ha az alkalmazások felhasználása vártnál további Processzor
Ha azt veszi észre az alkalmazás fogyasztása Processzor több, mint várható vagy teljesen ismételni Processzor kiugrásainak megfelelő keresztül figyelése megjelölt vagy szolgáltatás javaslatok fontolja meg a méretezés vagy a méretezés meg az App szolgáltatáscsomagja. Statefull alkalmazás esetén méretezés csak így tudja, miközben alkalmazás esetén állapot nélküli, méretezési out képet ad nagyobb rugalmasság és magasabb skála lehetséges. 

További információt a "statefull" és "állapot nélküli" alkalmazások, ebből a videóból: [tervezési a végpontok közötti méretezhető több szálon alkalmazások Microsoft Azure Web App](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). További információt az alkalmazás méretezése és autoscaling Szolgáltatásbeállítások olvasni: [skála Azure App szolgáltatásban webalkalmazást](web-sites-scale.md).  

## <a name="socketresources"></a>Ha a szoftvercsatorna erőforrások vannak merülnek
Skálázását kimenő TCP-kapcsolatok gyakori oka ügyfél tárakat, amelyek nem felelnek meg újra felhasználhatja a TCP-kapcsolatok, illetve például a HTTP - életben nem készül kapcsolatos károkozásra magasabb szintű protokoll használatát. Olvassa el a az egyes a tárak, az alkalmazások között a alkalmazás szolgáltatás tervezi, hogy konfigurálva vagy a kimenő kapcsolatokat a hatékony újrafelhasználása kódban elérhető biztosítása hivatkoznak. Is hajtsa végre a megfelelő létrehozásának és megjelenés vagy karbantartása httpserversockethandlers kapcsolatok elkerülése érdekében a tár dokumentáció útmutatást. Amíg az ilyen ügyfél tárak vizsgálatok folyamatban hatása ki több példányban való méretezéssel szüntethető.  

## <a name="appbackup"></a>Amikor az alkalmazás indítása adatkapcsolat történő biztonsági mentéséhez
Miért nem érhetők el alkalmazás biztonsági sikertelen két leggyakoribb okok miatt: Érvénytelen tárolási beállítások és érvénytelen adatbázis konfigurálása. E hibák általában akkor fordulhat elő, ha vannak a tárhely vagy egy adatbázis-erőforrások módosításait, illetve a változások alábbi forrásokat (pl. hitelesítő adatok frissítése a biztonsági mentés beállításait a kijelölt adatbázis) módját. Biztonsági másolatok általában időközönként futtassa és szükségük tárterület (a írása a biztonsági másolat), és az adatbázisok (a másolás és a biztonsági mentés szerepeltetni olvasásakor). Ezek az erőforrások közül eléréséhez adatkapcsolat eredményét lenne a biztonsági mentés sikerült. 

Biztonsági mentési hibái fordulhat elő, amikor olvassa el a legutóbbi eredmények megértéséhez, hogy milyen típusú hiba történik. Esetén tárolás access hibák ellenőrizze, és a biztonsági másolat a konfigurációban használt tárterület beállításainak frissítése. Az adatbázis az access hibák esetében kérjük ellenőrizheti és frissítheti a kapcsolatok karakterláncok alkalmazás beállításokban; Ezután folytassa a biztonságimásolat-konfiguráció megfelelően tartalmazzák a szükséges adatbázisok frissítése. További információt az alkalmazás biztonsági mentése használatáról a [biztonsági másolatot az Azure alkalmazás szolgáltatás webalkalmazást](web-sites-backup.md) talál.

## <a name="nodejs"></a>Amikor új Node.js alkalmazások telepítésének Azure alkalmazás szolgáltatás
Azure alkalmazás alapértelmezett konfigurációjának Node.js alkalmazásai legjobban az igényeinek leggyakrabban használt alkalmazások íródott. Az Node.js alkalmazás konfigurációs előnyös a teljesítmény javítása vagy Erőforrás kihasználtsága Processzor/memória/hálózati erőforrások optimalizálása személyre szabott beállítása, ha, sikerült ellenőrizheti a gyakorlati tanácsok és hibaelhárítási lépéseket. Ez a dokumentáció cikk ismerteti a iisnode beállításait, be kell állítania az Node.js számára, ismerteti a különböző forgatókönyveket vagy az alkalmazás szemben lévő is lehet, és bemutatja, hogyan problémák megoldásához problémáinak: [Gyakorlati tanácsok és hibaelhárítási útmutató a Microsoft Azure alkalmazás szolgáltatás csomópont-alkalmazásokkal](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   


