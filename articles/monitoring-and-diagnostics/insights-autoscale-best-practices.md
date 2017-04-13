<properties
    pageTitle="Gyakorlati tanácsok az Azure Monitor autoscaling. | Microsoft Azure"
    description="Ismerje meg, hogy hatékony használatához autoscaling az Azure Monitor elvek."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="best-practices-for-azure-monitor-autoscaling"></a>Gyakorlati tanácsok az Azure Monitor autoscaling

A dokumentumokban az alábbi szakaszok megismerheti a gyakorlati tanácsok az Automatikus méretezéssel az Azure. Ez az információ áttekintése, után is jobban hatékony használatához Automatikus méretezéssel az Azure infrastruktúra.

## <a name="autoscale-concepts"></a>Automatikus méretezéssel fogalmak

- Erőforrás beállíthatja, hogy csak *egy* Automatikus méretezéssel beállítás
- Az Automatikus méretezéssel beállítás beállíthatja, hogy egy vagy több profilok és az egyes profilok beállíthatja, hogy egy vagy több Automatikus méretezéssel szabályok.
- Az Automatikus méretezéssel beállítás méretezze át példányok vízszintesen, amely olyan *,* a példányok és *a* példányok száma csökkentésével növelésével.
 Az Automatikus méretezéssel beállítás egy maximum, minimum és példányok az alapértelmezett értéket tartalmaz.
- Az Automatikus méretezéssel feladat mindig a társított mérőszám, ha át kívánja méretezni, ellenőrizni, hogy meg van Metszéspont méretezési vagy a méretezés az megadott küszöbértékét. Listáját megtekintheti a mértékek, hogy az Automatikus méretezéssel méretezheti által [Azure Monitor autoscaling közös mértékek](insights-autoscale-common-metrics.md)elemre.
- Az összes küszöbértékek egy példány szintjén kerülnek kiszámításra. Például "1 példány által végzett skála mikor átlagos Processzor > 80 %-os, ha példányok száma 2", méretezési azt jelenti, hogy az átlagos Processzor minden példányára 80 %-nál nagyobb esetén.
- Akkor mindig értesítéseket hiba mailben. Konkrétan a tulajdonos, közreműködői és a cél erőforrás olvasók fogadhat e-maileket. Ha Automatikus méretezéssel helyreállítása a meghibásodása, és elindítja a szokásos módon működik-e még mindig kap egy *helyreállítási* e-mailben.
- Akkor is részvétel-ben e-mailek és a webhooks sikeres skála művelet értesítést kapnak.

## <a name="autoscale-best-practices"></a>Automatikus méretezéssel ajánlott eljárások

Használatával az alábbi tanácsokat Automatikus méretezéssel.

### <a name="ensure-the-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>Érdekében a legkisebb és legnagyobb értékek másik, és egy megfelelő margó közötti őket
Ha rendelkezik, amelynek legalább egy beállítás = 2, maximális = 2 és az aktuális példány számláló értéke 2, akkor fordulhat elő, nincs további teendő skála. Tartsa meg egy megfelelő margó közötti a legkisebb és legnagyobb példány megszámolja, hogy mely végpontokat is beleértve. Automatikus méretezéssel mindig méretezze át, ezek a korlátok között.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Automatikus méretezés vissza nem állítja Automatikus méretezéssel min és max
Manuálisan frissíteni a példányok száma fölött vagy alatt a maximális érték, ha az Automatikus méretezéssel motor automatikus méretezés vissza az a legkisebb (ha alatt) vagy a legnagyobb (ha felett). Például beállíthatja a tartományt, 3 és 6 közé. Ha egy futó példányát, az Automatikus méretezéssel motor méretezze át a következő futtatás a 3 példányaiban. Hasonlóképpen, akkor szeretné skála a 8-példányok visszatérhet 6 a következő futtatás.  Automatikus méretezés csak nagyon ideiglenes, kivéve ha az Automatikus méretezéssel szabályokat, valamint új.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Méretezési és skála szabály kombinációja, amellyel hajt végre olyan növelése és csökkentése mindig használata
Ha csak egy részét a kombinációját használja, Automatikus méretezéssel skála modul, amely egyetlen, vagy, a maximum vagy a minimum, amíg eléréséig.

### <a name="do-not-switch-between-the-azure-portal-and-the-azure-classic-portal-when-managing-autoscale"></a>Nem válthat az Azure-portál és az Azure klasszikus portál Automatikus méretezéssel kezelése
Cloud Services és az alkalmazás szolgáltatásaihoz (Web Apps), az Azure portal segítségével (portal.azure.com) létrehozása és kezelése az Automatikus méretezéssel beállításait. Virtuális gép skála eredménye a segítségével PoSH, CLI vagy REST API létrehozása és kezelése az Automatikus méretezéssel beállítást. Nem válthat az Azure klasszikus portal (manage.windowsazure.com) és az Azure-portálra (portal.azure.com) Automatikus méretezéssel konfigurációk kezelésekor. Az Azure klasszikus portál és az alapul szolgáló kódmentes korlátokkal rendelkezik. Az Azure portálon lépjen a grafikus felhasználói felület használatával Automatikus méretezéssel kezelése. A választható nézetbeállítás a Automatikus méretezéssel PowerShell, CLI vagy REST API-t (keresztül Azure erőforrás Explorer) használatára.

### <a name="choose-the-appropriate-statistic-for-your-diagnostics-metric"></a>Válassza ki a megfelelő statisztikai a diagnosztika mérőszám
A diagnosztikai mértékek közül választhat *átlag*, *Minimum*, a *legnagyobb* és *teljes* egy mérőszám, ha át kívánja méretezni. A leggyakoribb statisztikai az *átlag*.

### <a name="choose-the-thresholds-carefully-for-all-metric-types"></a>Gondosan metrikus diagramtípusokat küszöbértékei kiválasztása
Javasoljuk, hogy gondosan válasszon másik küszöbértékei méretezési és a méretezés gyakorlati helyzetek alapján.

Automatikus méretezéssel beállítások *nem javasoljuk,* hogy például az alatt a példák az ugyanazon vagy hasonló küszöb értékkel ki- és körülmények között:

- Példányok növelése 1 amikor megszámlálása szálak száma < = 600
- Példányok csökkentése 1 amikor megszámlálása szál Count > = 600

Nézzük meg, mi a probléma, amely áttekinthetőbb legyen tűnhet vezethet példát. Az alábbi lépések cosider.

1. Tegyük fel, előfordulnak olyan esetek 2, rajta, és ezután példányonként szálak átlagos száma növekszik, 625.
2. Automatikus méretezéssel hozzáadása egy 3-példányt, méretezze át.
3. Ezután feltételezik, hogy az átlagos szálak keresztül példány 575 tartozik.
4. Méretezés, mielőtt Automatikus méretezéssel biztos, hogy milyen a végleges állapot becslése lesz, ha átméretezi a. Ha például 575 x 3-as (aktuális példányok száma) = 1,725 / 2 (végleges méretezésekor példányok száma) 862.5 szálak =. Ez azt jelenti, hogy Automatikus méretezéssel kellene azonnal skála meg újra meg átméretezi, ha az átlagos szálak változatlan marad, vagy még akkor is tartozik egy kis összeg után. Jó helyen jár Ha azt méretezett mentése ismét a teljes folyamat volna ismételje meg, végtelen ciklust vezető.
5. Ez a helyzet (meghatározásuk "flapping") elkerülése érdekében Automatikus méretezéssel nem méretezheti egyáltalán. Ehelyett átugrása, és reevaluates a feltétel a későbbiekben újra a szolgáltatás feladatot hajt végre. Mivel Automatikus méretezéssel látszólag nem működnek, ha az átlagos szálak 575 volt, ez az sokan sikerült tévessze össze.

Becslés során a méretezés a "flappy" elkerülje szolgál. Ez a jelenség szem előtt, ha úgy dönt, hogy az azonos küszöbértékei méretezési és a kell megtartani.

Javasoljuk, hogy válasszon egy megfelelő margó közötti skála kivételének és küszöbértékei. Szerepel példaként vegye figyelembe az alábbi jobb szabály kombinációt.

- Példányok növelése 1 amikor megszámlálása Processzor % > = 80
- Példányok csökkentése 1 amikor megszámlálása Processzor % < = 60

Ebben az esetben  

1. Tegyük fel, hogy nincsenek 2 példányok induljon.
2. Az átlag Processzor % különböző példányok 80 Ugrás, ha Automatikus méretezéssel hozzáadása egy harmadik-példányt, méretezze át.
3. Most már feltételezik, hogy adott idő alatt a Processzor % esik 60.
4. Automatikus méretezéssel a méretezés a szabály megbecsüli a végleges állapot, mintha a méretezés a. Ha például 60 x 3-as (aktuális példányok száma) = 180 / 2 (végleges méretezésekor példányok száma) = 90. Így Automatikus méretezéssel nem skála beépülő modul, mert azt szeretné, hogy a méretezés meg nyomban újrakezdődik. Inkább átugorja méretezés.
5. A következő alkalommal Automatikus méretezéssel ellenőrzi a Processzor továbbra is 50 esik. Újra - 50 x 3-as példány megbecsüli = 150 / 2 példányok = 75, amely alatt a méretezési küszöbértékét 80, így a méretezés a sikeres 2 példányaiban.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>A speciális mértékek küszöbértéket méretezés előtt megfontolandó kérdések
 Speciális mértékek például tároló vagy szolgáltatás Bus várólista hosszúság metrikus a küszöbértékét értéke nem elérhető az aktuális példányainak száma egy üzenetek átlagos száma. Gondosan válassza ki a mérőszám küszöbértéke kiválasztása.

Vegyük szemléltetése, és annak érdekében, hogy megismeri a vezérlő viselkedése jobb példa

- Példányok növelése 1 száma, amikor a tároló várólista üzenet száma > = 50
- Példányok csökkentése 1 száma, amikor a tároló várólista üzenet darab < = 10

Fontolja meg a következő sorrendben:

1. Előfordulnak olyan 2 tároló várólista esetek.
2. Üzenetek megtartása lesz, és megtekintheti a tárhely várakozási sorban található, a végösszeg beolvassa 50. Előfordulhat, hogy feltételezik, hogy Automatikus méretezéssel kell egy indítása méretezési műveletet. Jó helyen jár, ne feledje, hogy továbbra is 50/2 = példányonként 25 üzeneteket. Igen a méretezési nem jelentkezik. Az első skála ki fordulhat elő az összes üzenet száma a tárhely várakozási sorban található 100 kell lennie.
3. Ezután feltételezik, hogy az összes üzenet száma eléri 100.
4. A 3 tároló várólista példány bekerül a méretezési művelet miatt.  A következő méretezési művelet nem történik meg, amíg az összes üzenet száma a várakozási sorban található 150 eléri, mert 150/3 = 50.
5. Most már a várakozási sorban található üzenetek számát is kisebb lesz. A 3-példányok, az első skála művelet történik, ha az összes üzenet minden sorban várakozó legfeljebb 30 adja meg, mert 30/3 = 10 üzenetek példányonként, amely a méretezés küszöbértékét.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Ha több profil beállítva az Automatikus méretezéssel beállítás a méretezés előtt megfontolandó kérdések

Egy Automatikus méretezéssel beállítással választhat egy alapértelmezett profil nélkül bármely függés ütemezzen, illetve idő mindig alkalmazott vagy dátum és idő tartományba meghatározott időtartamra választhat egy ismétlődő vagy egy profilt.

Ha Automatikus méretezéssel szolgáltatás dolgozza fel őket, akkor mindig ellenőrzi a következő sorrendben:

1. Fix dátum profil
2. Ismétlődő profil
3. Alapértelmezett profilként ("mindig")

Ha egy profilt feltétel teljesül, Automatikus méretezéssel nem ellenőrzi a következő profil feltétel alatti. Automatikus méretezéssel egyszerre csak egy profilt dolgozza fel. Ez azt jelenti, hogy ha szeretné is alsóbb szintű profilból feldolgozás feltétel, meg kell adni ezeket a szabályokat, valamint az aktuális profilban.

Tanulmányozzuk a példa használata:

Az alábbi képen látható egy Automatikus méretezéssel beállítása, hogy az alapértelmezett profil minimális példányainak = 2 és maximális példányok = 10. Ebben a példában szabályok vannak beállítva, hogy skála meg, amikor az üzenet száma a várakozási sorban található értéke meghaladja a 10 és skála segítségével az üzenetek száma a várakozási sorban található kisebb, mint a 3 esetén. Most az erőforrás méretezheti 2 és 10-példányok között.

Ezenkívül van egy ismétlődő profil hétfő beállítása. Minimális előfordulását beállítás = 2 és maximális példányok = 12. Ez azt jelenti, hétfővel Ez a feltétel ellenőrzi az első alkalommal Automatikus méretezéssel esetén a példányok száma 2, 3 új minimális méretezés. Mindaddig, amíg az Automatikus méretezéssel továbbra is keresse meg a profil feltétel egyező (hétfő), csak feldolgozásával a Processzor skála blokkolás vagy a szabályok be van állítva a profilt. Most azt nem ellenőrzi a sor hossza. Jó helyen jár Ha ellenőrizni kell a várólista hossza feltétel is szeretne kell szerepeltetni kívánt ezeket a szabályokat az alapértelmezett profilból, valamint a hétfő profil.

Hasonlóképpen amikor Automatikus méretezéssel vált vissza az alapértelmezett profilját, azt először ellenőrzi, ha teljesülnek-e a minimális és maximális feltételek. Ha a szám időben példányainak 12, méretezés a 10, a maximálisan engedélyezett az alapértelmezett profil.

![Automatikus méretezéssel beállításai](./media/insights-autoscale-best-practices/insights-autoscale-best-practices.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Ha több szabállyal beállítva profil a méretezés előtt megfontolandó kérdések
Előfordulhatnak olyan esetek, amikor is több szabályok beállítása a profil. A következő Automatikus méretezéssel szabálykészletet szolgáltatások használatával használatosak, amikor több szabályokat állít be.

*Ki skála*Automatikus méretezéssel fut. bármely szabály teljesülése esetén.
A *Méretezés az*Automatikus méretezéssel a összes szabály kell teljesülnie kell.

Ábrázolására, tegyük fel, hogy a következő 4 Automatikus méretezéssel szabályok:

- Ha a Processzor < 30 %, skála-1-gyel
- Ha a memória < 50 %-kal, skála-1-gyel
- Ha a Processzor > 75 %, 1-gyel skála meg
- Ha a memória > 75 %-os, 1-gyel skála meg

Kattintson a követés fordul elő:

- Ha Processzor 76 % és memória 50 %-os, hogy skála meg.
- Ha Processzor 50 %-os és memória 76 % azt skála meg.

Kézzel, ha a Processzor 25 %-os pedig memória 51 % Automatikus méretezéssel nem **nem** skála modul. Annak érdekében, hogy skála a Processzor 29 % és a memória 49 % legyen.

### <a name="always-select-a-safe-default-instance-count"></a>Mindig jelölje ki a megbízható alapértelmezett példány száma
Az alapértelmezett példány száma fontos Automatikus méretezéssel méretezze át, hogy ez a szolgáltatás, ha mértékek nem érhető el. Ezért jelölje ki a alapértelmezett példány száma, amely a munkaterhelésekből biztonságosként.

### <a name="configure-autoscale-notifications"></a>Automatikus méretezéssel értesítések konfigurálása
Automatikus méretezéssel értesíti a rendszergazdák és a munkatársak az erőforrás e-mailben, ha az alábbi feltételek bármelyike teljesül:

- Automatikus méretezéssel szolgáltatáshiba műveletet.
- Mértékek skála döntést Automatikus méretezéssel szolgáltatás nem érhetők el.
- Mértékek elérhető (helyreállítási) ismét a méretezés döntést.
A fenti feltételek mellett beállíthatja az e-mailbe vagy webhook értesítések sikeres skála műveletekhez értesítést kaphat.
