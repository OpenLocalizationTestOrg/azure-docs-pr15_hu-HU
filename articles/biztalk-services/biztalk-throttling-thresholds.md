<properties 
    pageTitle="Tudnivalók a BizTalk szolgáltatások szabályozásának |} Microsoft Azure" 
    description="Tudjon meg többet a küszöbértékek szabályozásának és az eredményül kapott futtatókörnyezet viselkedése BizTalk szolgáltatások. Szabályozásának alapuló memóriahasználat és az üzenetek számát. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>





# <a name="biztalk-services-throttling"></a>BizTalk szolgáltatások: szabályozásának

Azure BizTalk szolgáltatások végrehajtja a szolgáltatás szabályozásának két feltétel alapján: memóriahasználat és feldolgozása egyidejű üzenetek számát. Ez a témakör felsorolja a szabályozási küszöbértékek, és ismerteti a futtatókörnyezet működését szabályozási feltétel esetén.

## <a name="throttling-thresholds"></a>Küszöbértékek szabályozása

Az alábbi táblázat a szabályozási forrás- és küszöbértékei:

||Leírás|Alacsony küszöbérték|Felső küszöbértéket|
|---|---|---|---|
|A memória|teljes rendszer memória elérhető/PageFileBytes %-át. <p><p>Teljes elérhető PageFileBytes körülbelül 2 és a rendszer RAM.|60 %-kal|70 %-os|
|Üzenetek feldolgozása|Egyidejű feldolgozása üzenetek száma|40 * magmintákat száma|100 * magmintákat száma|

A felső küszöbértéket elérésekor Azure BizTalk Services szabályozása kezdődik. Az alsó küszöbértéket elérésekor szabályozásának a végpontok. Ha például a szolgáltatás 65 %-os rendszer memória használ. Ebben az esetben a szolgáltatás nem szabályozása. A szolgáltatás elindul, 70 %-os rendszer memória használatával. Ebben az esetben a szolgáltatás lehetővé, és továbbra is fennáll, mindaddig, amíg a szolgáltatás által 60 % (alsó küszöbértéket) rendszer memória szabályozása.

Azure BizTalk szolgáltatások nyomon követi a szabályozási állapot normál összehasonlítása csökkentett állam és szabályozási időtartama.


## <a name="runtime-behavior"></a>Futási idejű viselkedése

Amikor Azure BizTalk Services egy szabályozási állapotba kerül, a következő történik:

- Szerepkör példányonként szabályozásának van. Példa:<br/>
RoleInstanceA program szabályozását. Nem szabályozásának a RoleInstanceB. Ebben az esetben a RoleInstanceB üzenetének vártnak feldolgozása. Üzenetek RoleInstanceA nem őrződnek meg, és a következő hiba miatt sikertelen:<br/><br/>
**Kiszolgáló túlterhelt. Próbálkozzon újra.**<br/><br/>
- Bármely ki források ne lekérdezik, és töltse le egy üzenetet. Példa:<br/>
Egy folyamat üzenetek FTP külső forrásból gyűjti össze. A szerepkör-példányt, hajtsa végre a leküldéses szabályozási állapotba kap. Ebben az esetben a folyamat leállítása további üzenetek letöltését, amíg a szerepkör-példány leáll szabályozásának.
- A visszajelzés elküldése az ügyfélnek, az ügyfél is küldje el az üzenetet.
- Meg kell várnia, amíg a szabályozásának megoldódott. Kifejezetten meg kell várnia, amíg a alacsony megkapják.

## <a name="important-notes"></a>Fontos megjegyzések
- Nem lehet letiltani a szabályozásának.
- Nem lehet módosítani a küszöbértékek szabályozásának.
- Szabályozásának rendszerbeli hajtanak végre.
- Az Azure SQL-adatbázis kiszolgálója tartalmaz beépített szabályozásának.

## <a name="additional-azure-biztalk-services-topics"></a>További Azure BizTalk Services témakörök

-  [Az Azure BizTalk szolgáltatások SDK telepítése](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Oktatóprogram: Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Hogyan tudom használatával indítsa el az Azure BizTalk szolgáltatások SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Lásd még:
- [BizTalk szolgáltatások: Fejlesztőeszközök, Basic, normál és Premium kiadásában diagram](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk szolgáltatások: Kiépítési használatával Azure klasszikus portál](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk szolgáltatások: Kiépítési állapot diagram](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk szolgáltatások: Irányítópult, a Monitor és a méret lapon](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk szolgáltatások: Biztonsági mentése és visszaállítása](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk szolgáltatások: Kibocsátó neve és a kibocsátó billentyűk](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
 
