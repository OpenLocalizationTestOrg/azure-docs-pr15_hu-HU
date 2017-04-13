<properties
    pageTitle="Teljesítménnyel kapcsolatos szempontok Azure forgalom Manager |} Microsoft Azure"
    description="Adatforgalom-kezelő és hogyan ellenőrizheti a webhely teljesítményének a forgalom Manager használata esetén a teljesítmény megértése"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="performance-considerations-for-traffic-manager"></a>Teljesítménnyel kapcsolatos szempontok forgalom Manager

Ezen az oldalon teljesítménnyel kapcsolatos szempontok forgalom-kezelővel ismerteti. Vegye figyelembe az alábbiakat:

Ha a webhely példányai WestUS és EastAsia régiókban. A forgalom manager vizsgálati az állapot-ellenőrzés sikertelen az példányok közül. A megfelelő terület átirányításához alkalmazás forgalmat. A feladatátvételi várhatóan, de a teljesítmény alapján a forgalmat, most út közben mennie területére késleltetése probléma lehet.

## <a name="how-traffic-manager-works"></a>Hogyan működik a forgalom Manager

A csak teljesítményre gyakorolt hatás, amelyek a forgalom Manager lehetnek a webhelyen a kezdeti DNS-ben. A Microsoft DNS legfelső szintű tartalmazó kiszolgálón történik; a trafficmanager.net zóna kezeli a forgalom Manager profil nevét a DNS-kérelmet. Adatforgalom felettes kitölt és rendszeresen frissíti a Microsoft DNS-legfelső szintű kiszolgálók a forgalom Manager házirendet, és a vizsgálati eredmények alapján. Úgy is során a kezdeti DNS-ben nem DNS-lekérdezések forgalom Manager küld.

Adatforgalom Manager tevődnek össze több összetevőket: DNS-kiszolgálók, az API-szolgáltatás, a tárhely réteg és zárólap szolgáltatás felügyelete a névhez. Ha nem sikerül egy forgalom kezelő szolgáltatás összetevő, nem a forgalom Manager profil társított DNS-neve nincs hatással. A Microsoft DNS-kiszolgálók rekordjaihoz változatlan marad. Azonban végpont figyelemmel kísérésére és a DNS frissítése nem kerül sor. Emiatt nem frissíthetők, mutasson a feladatátvevő webhelyre, ha megszakad a elsődleges webhely DNS forgalom Manager.

A DNS-névfeloldás gyors, és az eredmények gyorsítótárban. A kezdeti DNS-keresés sebességének attól függ, hogy a DNS-kiszolgálóiról az ügyfélgép névfeloldásra. Egy ügyfél általában ~ 50 ms DNS kikeresni végezhetik el. A keresés eredményeit gyorsítótárban való a DNS Time-to-live (TTL) időtartamát. A TTL (élettartam) forgalom Manager alapérték 300 másodperc.

A forgalom nem enged forgalom Manager. Ha befejeződött a DNS-ben, az ügyfél IP-címének a webhely egy példányának foglalja magában. Az ügyfél közvetlenül csatlakozik, hogy a címére, és nem felel meg az adatforgalom-kezelővel. A forgalom Manager házirendet, kiválaszthatja, hogy a DNS teljesítménye nincs hatással van. Azonban a teljesítmény útválasztás-mód negatív hatással lehet a alkalmazás felület. Például a házirend irányítja a forgalmat a Észak-Amerika Ázsiában üzemeltetett példányhoz, a hálózati késés ezeket a munkamenetek probléma lehet.

## <a name="measuring-traffic-manager-performance"></a>Mérési forgalom Manager teljesítmény

Vannak olyan több webhelyekre is használhatja a teljesítmény és a forgalom Manager profil működésének megértése. Webhelyek számos ingyenes, de esetleg a korlátozásokat. Bizonyos webhelyek is fokozott nyomon követése és díjköteles jelentése nyújtanak.

Az eszközök a következő helyek mérték DNS késések és megjelenítése a megoldott IP-címek, az ügyfél helyek a világon. A legtöbb ezeket az eszközöket gyorsítótárazásának a DNS-között. Emiatt az eszközök megjelenítése a teljes DNS-ben a vizsgálat futtatásakor. A saját ügyfélprogramból tesztelésekor csak tapasztal egyszer a TTL (élettartam) érvényességi ideje alatt a teljes DNS-keresési teljesítményét.

## <a name="sample-tools-to-measure-dns-performance"></a>A DNS teljesítménye mérésére minta eszközök

- [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS sok teljesítmény eszközöket kínál. A DNS-összehasonlító eszköz megjelenítheti, mennyi ideig tart a DNS-név feloldásához, és hogy, hogy miben más DNS-szolgáltatók.

- [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    A legegyszerűbb eszközök egyik WebSitePulse. Írja be az URL-cím DNS megoldás időt, az első bájt, az utolsó bájt és a más Teljesítménystatisztikák. Három különböző vizsgálati helyek közül választhat. Ebben a példában látható, hogy az első végrehajtása jeleníti meg, hogy a DNS-keresés 0.204 sec tart.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Az eredmények gyorsítótárban, mivel a második feltétel a azonos forgalom Manager végpont a DNS-ben veszi 0,002 sec.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

- [Hitelesítésszolgáltató alkalmazás szintetikus Monitor](https://asm.ca.com/en/checkit.php)

    Korábbi neve a Watchmouse jelölőnégyzet webhely eszközt, a webhely jelzik, hogy a DNS-megoldás időt több földrajzi régiók egyidejű. Írja be a DNS-megoldás időt, a kapcsolódáskor és a több földrajzi helyek sebessége URL-CÍMÉT. A tesztet használatával megtekintheti, hogy melyik szolgáltatott szolgáltatás a világ különböző pontjaira vonatkozó ad vissza.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

- [Pingdom](http://tools.pingdom.com/)

    Ez az eszköz Teljesítménystatisztikák biztosít az weblapon minden elemet. Az elemzés lap lap DNS-keresés töltött időt százalékos jeleníti meg.

- [Mi az DNS?](http://www.whatsmydns.net/)

    Ezen a webhelyen a DNS-keresés jelent 20 más helyekről, és megjeleníti az eredményeket egy térképen.

- [Webes felület alaposabban](http://www.digwebinterface.com)

    Ezen a webhelyen látható, többek között a, CNAME és A rekordok DNS-adatok részletesebb. Győződjön meg arról, hogy a "Kifestés egyszínűre kimeneti" és "Stat" jelölje be a beállítások csoportban, és jelölje ki az összes"a névkiszolgálói rekordok.

## <a name="next-steps"></a>Következő lépések

[Adatforgalom Manager forgalom útválasztási módjairól](traffic-manager-routing-methods.md)

[Tesztelje a forgalom Manager](traffic-manager-testing-settings.md)

[Műveletek a forgalom Manager (REST API-referencia)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure forgalom kezelő parancsmagok](http://go.microsoft.com/fwlink/p/?LinkId=400769)
