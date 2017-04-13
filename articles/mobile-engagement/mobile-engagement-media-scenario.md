<properties 
    pageTitle="Multimédia-alkalmazásokban Azure Mobile tetszés szerint elmélyedhet végrehajtása"
    description="Médiafájlok alkalmazás forgatókönyv Azure Mobile tevékenységek végrehajtásához" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

#<a name="implement-mobile-engagement-with-media-app"></a>Mobil részvételét Media alkalmazással megvalósítása

## <a name="overview"></a>– Áttekintés

János egy mobil projektmenedzser egy nagy media vállalat számára. Őt a nagyon magas letöltése darabszám tartalmazó új alkalmazás nemrégiben indította el. Ő elérte a kitűzött célok letöltésre, de továbbra is a visszatérési a Investment(ROI) felhasználónként nem felel meg a saját követelményeknek. 

Adott már azonosította, hogy mennyi a BMT túl halk. Felhasználók gyakran megszüntetése után csak 2 hétből saját alkalmazással, és a legtöbb soha nem térjen vissza. Azt szeretné, hogy az adott alkalmazás adatmegőrzési növeléséhez.

Után néhány kezdeti tesztelés, ő még tanultakhoz ő folytat, hogy a felhasználók a leküldéses értesítéseket, amikor azok gyakran használja a saját alkalmazás továbbra is. Inaktív amelyekre felhasználók is gyakran ad vissza az alkalmazásba, attól függően, hogy ő küldjön nekik, értesítéseket. János úgy dönt, hogy bizonyos típusú tetszés szerint elmélyedhet Program saját alkalmazás használó speciális kiválasztásával a leküldéses értesítések fektetni.

János [Azure Mobile tetszés szerint elmélyedhet - – első lépések a gyakorlati tanácsok](mobile-engagement-getting-started-best-practices.md) a legutóbb olvasási van, és úgy döntött, a javaslatok a útmutató végrehajtásához.

##<a name="objectives-and-kpis"></a>Célok és fő teljesítménymutatók

János alkalmazásának főbb érdekeltekkel felel meg. Üzleti a hirdetések jön létre, a felhasználók saját media felhasználni. Felhasználónként elfogyasztott tartalom növelésével János növeli a bevételek. Azzal az összes egyik legfontosabb cél: a hirdetések értékesítés növeléséhez 25 %-kal. Ez a cél készíthetnek üzleti fő teljesítménymutatók (KPI-k) mérték és a meghajtó

* Felhasználónként rákattintott hirdetések száma
* Hány cikklapok megtekintett (/ felhasználó / / munkamenet / heti / hónap...)
* Mik azok a kedvenc kategóriák

Fő érintettekkel János értekezlet alapján ő definiálva van a vállalati KPI-k. Ő követi az [Azure Mobile tetszés szerint elmélyedhet - – első lépések a gyakorlati tanácsok](mobile-engagement-getting-started-best-practices.md)a kijelző 1. 

Ezután ő hozza létre a következő tetszés szerint elmélyedhet a KPI-k annak érdekében, hogy célok elérésekor:

* Adatmegőrzési figyelheti a következő időszakok keresztül: napi, heti, üzletiintelligencia-heti és havi.
* Az aktív felhasználók száma
* Az alkalmazás értékelése az alkalmazásban tárolja.

Műszaki KPI-k felvették informatikai csapattag javaslatok alapján, az alábbi kérdésekre választ:

* Mi az a felhasználói elérési útját (mely megnyitják, hány alkalommal felhasználók töltött meg)
* Hány összeomlik vagy minden munkamenetben talált hibákat?
* Mely operációs rendszer verziója felhasználóim fut?
* Mi az, hogy a felhasználók számára képernyője átlagos méretének?
* Milyen típusú internetkapcsolattal rendelkeznek a felhasználók számára?

Az egyes KPI-k ő sorolja a szükséges adatokat, és saját playbook megfelelő helyét a ő rekordok.

## <a name="engagement-program-and-integration"></a>Tetszés szerint elmélyedhet program és integrációja

Most, hogy az adott befejeződött, a KPI-k létrehozása, automatikus indítása a tetszés szerint elmélyedhet stratégia fázis 4 tetszés szerint elmélyedhet programok és a célok meghatározása:    ![][1]

János megnyitja mélyebb által részletező minden programhoz leküldéses értesítéseket. Leküldéses értesítést öt elem határozzák meg:

1. Cél: Mi az a cél az értesítés
2. Hogyan érhető el a cél
3. Cél: ki a értesítést kap?
4. Tartalom: Mi az a szöveg és a formátum (a App/Out az alkalmazás) értesítés
5. Mikor: Mi az, hogy a legjobb pillanatig küldi el a leküldéses értesítést

    ![][2]

További információt talál az [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks).

Aszerint, hogy a kijelző a tanulmány János 2 adatok általa összegyűjtéséhez meghatározására cél szakasz használ, és írja be a saját címke megtervezése közösen informatikai csapattal megvalósítását a megoldást. 1 hét végrehajtása és a felhasználó elfogadó tesztelés, után János végül indíthatja el a program.

##<a name="program-results"></a>Program eredménye

4 hónappal később János áttekinti a programok előadásokhoz. Üdvözli a Program és a heti programot a kitűzött célok értekezlet. Csökkenti a felhasználó csak egyetlen munkamenethez számát, az alkalmazás további funkcióinak használata érdekli, és a heti kapcsolatok száma kétszeres foglalja magában.

Az **Inaktív Program** elősegíti az adott felhasználó tendenciák feltárásához megértéséhez. Úgy tűnik, hogy a felhasználókat 15 %-kal térjen vissza az alkalmazásba. Azonban őket a legtöbb ne maradjon aktív több, mint 1 hónap. János úgy rendelkezik, a potenciális optimalizálása a sorozat további értesítések és bővítése a tartalom lehetőségeket.

A **Program felfedezése** a nem jól működik. Növeli a tartományok értékesítő, de nem elég a célok eléréséhez. János azonosítja, hogy ő nincs elég, hogy a fontos adatok kiválasztásával és a megfelelő tartalmat felajánlása. Ő leállítja a program, és "Szerkesztői leküldéses értesítéseket" Azure Mobile tetszés szerint elmélyedhet a küldő koncentrál. Saját újságírók már rendelkezik a leküldéses értesítések küldésére CMS megoldást, és nem kíván váltani.

János úgy dönt, hogy az elérjen API, amely egy HTTP REST API-val, amely lehetővé teszi, hogy vannak kampányok kezelése AZME webes felület használata nélkül kapcsoljon. Az eljárás János összegyűjtheti ő van szüksége, és lehetővé teszi a szövegírói továbbra is a CMS megoldást használni kívánja az adatokat.

Annak érdekében, hogy helyesen működik ezt a szolgáltatást, János arra utasítja az informatikai csoport éberen meg az alábbiakat:

1. **A művelet rendszerek** : minden rendelkeznek a saját szabályok felügyelheti a leküldéses értesítéseket, így János úgy dönt, hogy a lista minden olyan esetben, és ellenőrzi az API-k kezeli-e meg.
Pl.: A leküldéses Android rendszer lehetővé teszi, hogy átfogó képet, amelyek nem a helyzet iOS operációs rendszerrel.

2. **Időkereten**: János szeretne egy API-val, amely időkereten és vége kampányokkal. Azt szeretné, hogy minden olyan értesítési zavaró bombing felhasználóit megőrzése érdekében.

3. **Kategóriák**: Marketing-csoportwebhely sablon előkészíti a riasztási hibatípusonként. Adott informatikai csoport belül az API kategóriák megadását kéri.

Néhány vizsgálatok után János meggyőződött. Ez az API köszönetnyilvánító újságírók is elküldheti a CMS és Azure Mobile tetszés szerint elmélyedhet a leküldéses értesítések összes viselkedési adatokat gyűjtenek számukra

A 4-es után először hónapokban, eredmények megfelelően egy jó általános teljesítményt és ad megbízhatóság János és a falat, 15 %-át egy felhasználót nő per megtérülési ráta, és a mobil értékesítés 17.5 %-át képviseli összes értékesítés, csak négy hónapban 7.5 %-os növekedést.

<!--Image references-->
[1]: ./media/mobile-engagement-media-scenario/engagement-strategy.png
[2]: ./media/mobile-engagement-media-scenario/push-scenarios.png

<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
