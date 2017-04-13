<properties 
   pageTitle="A kiemelt StorSimple helyileg kötet – gyakori kérdések |} Microsoft Azure"
   description="A kiemelt helyileg StorSimple kötet kapcsolatos gyakori kérdésekre választ ad."
   services="storsimple"
   documentationCenter="NA"
   authors="manuaery"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/16/2016"
   ms.author="manuaery" />

# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>A kiemelt kötet StorSimple helyileg: gyakori kérdések

## <a name="overview"></a>– Áttekintés

Kérdések és válaszok, lehet, hogy hozzon létre egy helyi kiemelt StorSimple mennyiségi, egy helyi rögzített mennyiségi (vagy fordítva), többszintű kötet konvertálása vagy biztonsági mentése és visszaállítása egy helyi rögzített mennyiségi az alábbiakban.

Kérdések és válaszok az alábbi kategóriákba vannak rendezve

- Helyi meghajtóra rögzített kötet létrehozása
- Biztonsági mentése helyi meghajtóra rögzített
- A többszintű mennyiségi konvertálása helyi rögzített kötet
- A helyi meghajtóra rögzített mennyiségi visszaállítása
- Hibás helyileg rögzített kötet keresztül

## <a name="questions-about-creating-a-locally-pinned-volume"></a>A helyi meghajtóra rögzített mennyiségi létrehozásával kapcsolatos kérdések

**KÉRDÉSEK.** Mi az a 8000 sorozat eszközökön létrehoznom helyileg rögzített mennyiségig maximális mérete?

**A** is kiépítése helyileg rögzített kötet 8.5 TB vagy többszintű kötet legfeljebb 200 TB 8100 az eszközre. A nagyobb 8600 eszközön helyileg rögzített kötet is kiépítése 22.5 TB vagy többszintű kötet legfeljebb 500 TB.

**KÉRDÉSEK.** Nemrég frissített 8100 eszközömön a frissítés 2, és hozzon létre egy helyi rögzített mennyiségi megpróbálom, a maximális méret csak 6 TB és nem 8.5 TB. Miért nem hozható létre egy 8.5 TB mennyiségi?

**A** is kiépítése felfelé 8.5 helyileg rögzített kötet TB vagy többszintű kötet legfeljebb 200 TB 8100 az eszközre. Ha az eszköz már rendelkezik többszintű kötet, akkor hozhat létre a rendelkezésre álló hely a helyi meghajtóra rögzített mennyiségi arányosan kisebb, mint a maximális érték lesz. Például 100 TB többszintű mennyiségének van már kiépítve (amely a többszintű kapacitásának fele) 8100 eszközén, ha majd 8100 eszközre hozhat létre helyi mennyiségig maximális hossza ennek megfelelően csökken és 4 TB (nagyjából helyileg maximális felét kiemelt kötet kapacitásának).

Néhány helyi helyet az eszközön használja a munkakészlet többszintű mennyiségének tárolni, mert a rendelkezésre álló hely helyileg rögzített kötet létrehozása, ha az eszköz van többszintű kötet csökken. Viszont helyileg rögzített kötet arányosan létrehozása csökkenti a rendelkezésre álló hely többszintű kötet. Az alábbi táblázatban áttekintheti a rendelkezésre álló többszintű kapacitás 8100 és 8600 eszközön, helyben rögzített kötet létrehozásakor.

|Helyi meghajtóra rögzített kötet kiépített kapacitás|Rendelkezésre álló kapacitása ahhoz, hogy a kötet többszintű - kell építenie 8100|Rendelkezésre álló kapacitása ahhoz, hogy a kötet többszintű - kell építenie 8600|
|-----|------|------|
|0 | 200 TB | 500 TB |
|1 TB | 176.5 TB | 477.8 TB|
|4 TB | 105.9 TB | 411.1 TB |
|8.5 TB | 0 TB | 311.1 TB|
|10 TB | HIÁNYZIK | 277.8 TB |
|15 TB | HIÁNYZIK | 166.7 TB |
|22.5 TB | HIÁNYZIK | 0 TB |


**KÉRDÉSEK.** Miért van helyileg rögzített kötet létrehozása egy ideig futó művelet? 

**VÁLASZOK PARANCSRA.** Helyi meghajtóra rögzített kötet thickly kiépített környezetet. A helyi rétegek az eszköz terület létrehozásához néhány adatát létező többszintű kötet előfordulhat, hogy kell eltolni a felhőbe a kiépítési folyamat során. És mivel ez attól függ, hogy a kötet kiépítve, a meglévő adatokat, az eszköz és a felhőben, az elérhető sávszélesség méretének helyi kötet létrehozásához szükséges idő lehet-e több óra.

**KÉRDÉSEK.** Mennyi ideig tart a helyi meghajtóra rögzített kötet létrehozása?

**VÁLASZOK PARANCSRA.** Helyi meghajtóra rögzített kötet thickly kiépített, mert bizonyos többszintű kötet meglévő adatainak előfordulhat, hogy kell eltolni a felhőbe a kiépítési folyamat során. Ezért helyileg rögzített kötet létrehozásához szükséges idő attól függ, hogy több tényező, többek között a hangerőt, az adatokat az eszközön, és a rendelkezésre álló sávszélesség méretét. Frissen telepített eszközön, amelyen nincs kötet a helyi meghajtóra rögzített kötet létrehozása ideje KB használati adatok adjon. Azonban a helyi kötet létrehozása órát is igénybe vehet számos eszközt használja a fentiekben tényezők alapján.

**KÉRDÉSEK.** Hozzon létre egy helyi rögzített mennyiségi szeretném. Van bármilyen tartsa szem előtt kell gyakorlati tanácsokat?

**VÁLASZOK PARANCSRA.** Helyi meghajtóra rögzített kötet, hogy az adatok mindig helyi garanciákkal, valamint a kis-és nagybetűket késések cloud munkaterhelésekből alkalmasak. Miközben kiválasztja a munkaterhelésekből valamelyike helyi kötet használatát, ne feledje, hogy a megfelelő műveletet:

- Helyi meghajtóra rögzített kötet thickly kiépített környezetet, és többszintű kötet a rendelkezésre álló hely létrehozása a helyi kötet hatással van. Ezért ajánlott a kisebb méretű kötet kezdődik, és szerkezetének kialakítása a a tárhely követelmény növekedése.

- Helyi mennyiségének kiépítési egy ideig futó műveletet tartalmazó meglévő adatok küldése a többszintű kötet a felhőbe. Emiatt tapasztalhatja meg ezeket a kötet csökkentett teljesítményét.

- Helyi mennyiségének kiépítési egy olyan időigényes művelet. Több tényezőktől függ, a tényleges időigénye: a mennyiségi folyamatban, az adatok az eszközön, és a rendelkezésre álló sávszélesség méretét. Ha Ön nem készített biztonsági másolatot a meglévő kötet a felhőbe, kötet létrehozása lassabban. Javasoljuk, hogy előtt végezze el a felhőben pillanatfelvételek a meglévő mennyiségének helyi kötet létesítése.
 
- Meglévő többszintű kötet átalakítása helyi rögzített kötet, és a konvertálás magában foglalja a kiépítési terület az eredményül kapott helyileg rögzített kötet (leállítása többszintű adatok [esetleges] a felhőből) mellett az eszközön. Ismét az egy ideig futó műveletet, amely azt, hogy a fent ismertetett tényezőktől függ. Javasoljuk, hogy biztonsági másolatot készíteni a meglévő kötet konvertálás előtt, a folyamat lassabban még akkor is, ha a létező kötet nem készített biztonsági másolatot lesz. Az eszköz is teljesítményre csökkentett folyamat során.
    
További információt [a helyi meghajtóra rögzített mennyiségi](storsimple-manage-volumes-u2.md#add-a-volume) létrehozása

**KÉRDÉSEK.** Létrehozhatok-e több helyileg rögzített kötet egyszerre?

**VÁLASZOK PARANCSRA.** Igen, de minden helyileg rögzített mennyiségi létrehozását és kiterjesztett feladatok feldolgozási sorrendben.

Helyi meghajtóra rögzített kötet thickly kiépített környezetet, és a helyi lemezterület (amely miatt esetleg a kiépítési folyamat során a felhőbe kell nyomni az többszintű kötet meglévő adatainak) az eszközre létrehozása ehhez. Emiatt a kiépítési feladat folyamatban van, ha más helyi mennyiségi létrehozási feladatot fog sorba állítani, hogy a feladat befejezése.

Hasonlóképpen ha egy meglévő helyi mennyiségi éppen ki van bontva, vagy egy helyi rögzített mennyiségi konvertálva, a többszintű kötet, majd az új helyi rögzített kötet kibocsátása van aszinkron az előző feladat befejeztéig. Helyi meghajtóra rögzített kötet méretének magában foglalja a meglévő helyi terület, hogy a kötet kiterjesztését. A többszintű helyileg kiemelt konverzió mennyiségi is magában foglalja a helyi lemezterület létrehozását a mennyiségi helyileg az így rögzített. A mind a fenti műveletek, létrehozása vagy a helyi lemezterület kiterjesztett hosszú fut feladatot.

Ezek a feladatok megtekintése az Azure StorSimple kezelő szolgáltatás **feladatok** lapján. A feladat, amely aktívan feldolgozása folyamatosan térköz-kialakítási végrehajtásának megfelelően frissül. A hátralévő mennyiségi helyileg rögzített feladatok futtatásával, van-e jelölve, de segítséget haladásuk használatát van, és azokat a kiválasztott abban a sorrendben, azok sorba állított vannak.

**KÉRDÉSEK.** A helyi meghajtóra rögzített mennyiségi törlése Miért nem látható a szabad terület megjelennek, amikor megkísérlem hozzon létre egy új mennyiségi regenerált térköz? 

**VÁLASZOK PARANCSRA.** Ha töröl egy helyi rögzített mennyiség, az új kötet rendelkezésre álló hely előfordulhat, hogy nem frissülnek azonnal. A StorSimple kezelő szolgáltatás frissíti a helyi lemezterület óránként körülbelül. Javasoljuk, hogy egy óra próbál létrehozni az új kötet előtt várja.

**KÉRDÉSEK.** Helyi meghajtóra rögzített kötet használhatók a felhő készülék?

**VÁLASZOK PARANCSRA.** Helyi meghajtóra rögzített kötet nem támogatottak a felhőben készülék (8010 és 8020 eszközök korábbi néven a StorSimple virtuális eszközt).

**KÉRDÉSEK.** Használhatom-e az Azure PowerShell-parancsmagok létrehozásához és kezeléséhez helyileg rögzített kötet? 

**VÁLASZOK PARANCSRA.** Via (bármely mennyiségi Azure PowerShell keresztül létrehozhat többszintű) Azure PowerShell-parancsmagok helyileg rögzített kötet nem, nem hozható létre. Is javasoljuk, hogy nem használjon Azure PowerShell-parancsmagok helyileg rögzített mennyiségig bármely tulajdonságainak módosítása fog volna nemkívánatos hatása a mennyiségi típus módosítása, hogy többszintű.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>A helyi meghajtóra rögzített mennyiségi mentésével kapcsolatos kérdések

**KÉRDÉSEK.** Támogatott helyileg rögzített mennyiségének helyi pillanatképek vannak?

**VÁLASZOK PARANCSRA.** Igen, a helyi meghajtóra rögzített mennyiségének helyi pillanatképek készíthet. Azonban nyomatékosan javasoljuk, hogy rendszeresen biztonsági másolatot készíteni a felhőben pillanatképek annak érdekében, hogy az adatok katasztrófa esetben védett helyileg rögzített kötet.

**KÉRDÉSEK.** Van bármilyen irányelvek helyileg rögzített kötet helyi pillanatképek kezelésére szolgáló?

**VÁLASZOK PARANCSRA.** Gyakori helyi pillanatképek mellett a helyi meghajtóra rögzített kötet adatok tejeskanna nagy mértékű okozhat a helyi lemezterület gyorsan elfogyasztott mennyiség és éppen tolni a felhőbe többszintű kötet adatainak esetében az eszközön. Ajánlott tehát a helyi pillanatképek számának csökkentése.

**KÉRDÉSEK.** Értesítés arról, hogy a helyi pillanatképek helyileg rögzített mennyiségének lehet-e érvényét kaptam. Ha tudja a problémát?

**VÁLASZOK PARANCSRA.** Gyakori helyi pillanatképek mellett a helyi meghajtóra rögzített kötet adatok tejeskanna nagy mértékű okozhat a helyi lemezterület gyorsan felhasználandó az eszközön. Az eszköz a helyi rétegek van kitéve, ha egy bővített felhő üzemszünetek eredményezhet az eszköz teljes valamit, és a mennyiségi bejövő írások eredményezhet a pillanatképek érvénytelenítése (a szóköz nélkül létezik a pillanatképek, ha nézni szeretné a régebbi adatblokkok, amely rendelkezik felülírás frissítése). Ebben az esetben a mennyiségi írás továbbra is szerepelni fognak jeleníthető meg, de lehet, hogy a helyi pillanatképek érvénytelen. Nincs hatással a már meglévő felhő pillanatfelvételek a nem.

A riasztási figyelmeztetés, ha értesítést szeretne, hogy ilyen esetben merülnek fel, és győződjön meg róla, akkor cím azonos időben lehet a helyi pillanatképek ütemezések kevésbé gyakori helyi rögzítenie megtekintésével vagy régebbi helyi elérhető információk, amelyek már nem szükséges törlése.

Ha a helyi pillanatképek érvénytelenné válnak, kap információt jelzést meg, amely tudatja, hogy az adott biztonsági házirend a helyi pillanatképek érvényét van mellett, amelyeket érvényét helyi pillanatfelvételt időbélyegeket listáját. Ezeket a pillanatképek lesz, automatikusan törli, és már nem tudja megtekinteni a **Biztonsági másolat katalógusok** az Azure klasszikus portál lapján az.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>A többszintű mennyiségi helyileg rögzített kötet konvertálásával kapcsolatos kérdések

**KÉRDÉSEK.** A többszintű mennyiségi konvertálásakor a helyi meghajtóra rögzített mennyiségi lehet vagyok betartásával néhány lassúsága az eszközön. Miért oka? 

**VÁLASZOK PARANCSRA.** A konvertálás két lépésből áll: 

  1. A kiemelt mennyiségi kiépítési térközt a leggyorsabban-a--alakulnak helyileg az eszközön.
  2. Bármely többszintű adatainak letöltése helyi biztosítása érdekében a felhőből garantálja.

Ezeket a lépéseket a hosszúak futó műveletek, amelyek a hangerőt, a konvertálandó, az eszközön, és a rendelkezésre álló sávszélesség adatok méretének függnek. Meglévő többszintű kötet néhány adatát a kiépítési folyamat részeként a felhőbe előfordulhat, hogy spill, mint az eszköz teljesítményre csökkentett ez idő alatt. Ezenkívül a konvertálás lassabb lehet, ha:

- Meglévő kötet nem mentett a felhőbe; Javasoljuk, hogy a kötet kezdeményezése egy konvertálás előtt készítsen biztonsági másolatot.

- Szabályozási sávszélesség-házirendek alkalmazott, előfordulhat, hogy lehetőségeket, amelyek az elérhető sávszélesség a felhőbe; ajánlott tehát van egy dedikált 40 MB vagy több kapcsolat a felhőbe.

- A konvertálás is eltarthat, a fenti; több tényezők miatt ezért javasoljuk, hogy a művelet nem csúcsok időszakokban vagy a befejezési fogyasztói hatással elkerülése érdekében a hétvége végrehajtása.

További információt [a helyi meghajtóra rögzített mennyiségi többszintű kötet](storsimple-manage-volumes-u2.md#change-the-volume-type) konvertálása

**KÉRDÉSEK.** Lemondhatja a mennyiségi átalakítási műveletet?

**VÁLASZOK PARANCSRA.** Nem, nem a Mégse gombra a egyszer kezdeményezett átalakítási műveletet. Az előző kérdésre a tárgyalt, ne feledje, hogy az esetleges teljesítmény problémák, előfordulhat, hogy a folyamat során felmerülő, és kövesse a gyakorlati tanácsok a konvertálás megtervezésekor tartalmazza.

**KÉRDÉSEK.** Mi történik a hangerőt, ha a konvertálás művelet sikertelen lesz?

**VÁLASZOK PARANCSRA.** Mennyiségi átalakítási hiba történhet a felhőben való kapcsolódási problémák miatt. Az eszköz ahányat leállhat a konvertálás többszintű adatok leállíthatja a felhőből sikertelen kísérletek sorozata után. Az ilyen esetben a kötet típusa továbbra is a mennyiségi forrástípus előtt az átalakítás és:

- A kritikus riasztás-ra értesítést szeretne a mennyiségi típuskonverziós hiba. További információt a [helyi meghajtóra rögzített kötet kapcsolatos értesítések](storsimple-manage-alerts.md#locally-pinned-volume-alerts)

- Konvertálni a többszintű egy helyi rögzített mennyiségi, ha a mennyiségi folytatja többszintű kötet tulajdonságainak mutathat, mint adatok továbbra is előfordulhat, hogy állnak a felhőben. Javasoljuk, hogy a csatlakozási problémák megoldásához, és próbálkozzon újra a átalakítási műveletet.
 
- Hasonlóképpen ha alakítása helyileg rögzített többszintű kötet nem sikerül, bár a mennyiségi lesz egy helyi rögzített mennyiségi megjelölve, azt működik, mint többszintű kötet (mivel adatokat is van kiömlött a felhőbe). Azonban továbbra is az eszköz hely a helyi rétegek vezérlőelemmel. Ezen a helyen nem érhető el a többi helyileg rögzített kötet. Javasoljuk, hogy, ismételje meg ezt a műveletet annak érdekében, hogy a mennyiségi konvertálás befejeződött, és a helyi helyet az eszközön is nyerhető.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>A helyi meghajtóra rögzített mennyiségi visszaállításával kapcsolatos gyakori kérdésekre

**KÉRDÉSEK.** Azonnali vissza helyileg rögzített kötet?

**VÁLASZOK PARANCSRA.** Igen, azonnali szolgáltatáscsomagot helyileg rögzített kötet. Amint a kötet metaadatainak lekért a felhőbe a visszaállítási művelet részeként, a kötet online állapotba kerül, és az állomás is elérhető. Azonban a kötet helyi garanciákkal adatok maradnak mindaddig, amíg az összes adatot töltődnek a felhőben, és tapasztalhatja bemutató csökkenteni a visszaállítás időtartama ezek kötet teljesítményét.

**KÉRDÉSEK.** Mennyi ideig tart a helyi meghajtóra rögzített mennyiségi visszaállítása?

**VÁLASZOK PARANCSRA.** Helyi meghajtóra rögzített kötet azonnali vissza és online állapotba, amint a felhőből beolvasni a mennyiségi metaadat-alapú adatait, miközben továbbra is a mennyiségi adatok tölthető le a háttérben. A visszaállítás – vissza a helyi garanciákkal mennyiségi adatokhoz – első ez utóbbi része időigényes művelet, és elvégzendő helyi ismét az adatok több óráig is eltarthat. Az azonos végrehajtásához szükséges idő több tényezők, például a visszaállítandó kötet méretének és a rendelkezésre álló sávszélesség függ. Ha törli az eredeti kötet, amely visszaállítják, további időt a helyi terület kialakítása az eszközre a visszaállítási művelet részeként kell tenni.

**KÉRDÉSEK.** Ha vissza szeretne állítani egy régebbi pillanatkép (készítésekor a mennyiségi lett többszintű) a meglévő helyileg rögzített mennyiségi van szükségem. A hangerő visszaáll, ebben az esetben többszintű?

**VÁLASZOK PARANCSRA.** A hangerő nem, mint helyileg rögzített kötet visszaáll. Bár az idő, amikor a mennyiségi többszintű volt, a pillanatkép dátumok visszaállítása létező kötet, közben StorSimple mindig használ a mennyiségi típusú a lemezen jelenleg található.

**KÉRDÉSEK.** A legutóbb bővített lehet a helyi meghajtóra rögzített mennyiségi, de most kell állítani az adatokat egy időpontja hangerejének kisebb méretű. Átméretezi visszaállítása az aktuális mennyiségi és fog van szükség a mennyiségi méretének növelése a helyreállítás befejezése után?

**VÁLASZOK PARANCSRA.** Igen, a visszaállítás átméretezi a hangerőt, és a hangerő méretének növelése a visszaállítás befejeződése után kell.

**KÉRDÉSEK.** Megváltoztathatom-e a mennyiségi típusú visszaállítás során?

**A.** A hangerő-típus nem, visszaállítás során nem módosítható.

- Törölt kötet szolgáltatáscsomagot a pillanatkép tárolt típust.

- Meglévő kötet szolgáltatáscsomagot aktuális típusuk alapján, függetlenül a típus van tárolva a pillanatkép (lásd az előző két kérdés).
 
**KÉRDÉSEK.** A helyi meghajtóra rögzített mennyiségi visszaállítása kell, de lehet a helytelen pont kiválasztott idő pillanatfelvétel. Lemondhatja az aktuális visszaállítási művelet?

**VÁLASZOK PARANCSRA.** Igen, megszakíthatja a folyamatban lévő visszaállítási művelet. Vissza a mennyiségi állapotának lesz görgetve a visszaállítás elején állapotát. Azonban bármelyik írások végzett szeretné a hangerőt, miközben folyamatban volt a visszaállítás el fog veszni.

**KÉRDÉSEK.** Tudom használni a helyi meghajtóra rögzített kötet egyik visszaállítási művelet, és most látható pillanatkép lehet nem recollect létrehozása a tartalék katalógust. Ki mire való?

**VÁLASZOK PARANCSRA.** Ez a az ideiglenes pillanatfelvételek a visszaállítás előtt hoz létre, és visszaállítás használatos abban az esetben a visszaállítás megszakad, vagy hiba jelenik meg. Ne törölje ezt a pillanatfelvételt; akkor fog automatikusan törli a visszaállítás befejeződésekor. Ez akkor fordulhat elő, ha a visszaállítási feladat van csak a helyi számítógépre rögzített, kötet vagy kombinálja a helyi meghajtóra rögzített és többszintű kötet. Ha a visszaállítási feladat csak többszintű kötet tartalmazza, majd a probléma nem történik.

**KÉRDÉSEK.** A helyi meghajtóra rögzített mennyiségi is klónozhatja?

**VÁLASZOK PARANCSRA.** Igen, megteheti. Jó helyen jár a helyi meghajtóra rögzített mennyiségi fog klónozható mint többszintű kötet alapértelmezés szerint. További információt olvashat [a helyi meghajtóra rögzített mennyiségi adatfeliratsor](storsimple-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Hibás fölé egy helyi rögzített mennyiségi kapcsolatos kérdések

**KÉRDÉSEK.** Fizikai egy másik készülékre eszközömön átadni van szükségem. A helyi meghajtóra rögzített kötet sikertelen lesz a kiemelt helyileg vagy többszintű feletti?

**VÁLASZOK PARANCSRA.** Attól függően, hogy a célként megadott eszköz verzióját helyben rögzített kötet sikertelen lesz fölé szerint:

- Helyi meghajtóra kiemelt, ha a célként megadott eszköz futása StorSimple 8000 sorozat 2 frissítése
- Ha a célként megadott eszköz futása StorSimple többszintű 8000 sorozat frissítése 1.x
- Ha a célként megadott eszköz a felhőben készülék többszintű (szoftver verziófrissítés 2 meg és frissítheti a 1.x)

További információt a [feladatátvétel és DR helyileg kiemelt kötet verziója között](storsimple-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**KÉRDÉSEK.** Helyi meghajtóra rögzített kötet azonnali szolgáltatáscsomagot vészhelyreállítás (DR)?

**VÁLASZOK PARANCSRA.** Igen, a helyi meghajtóra rögzített kötet azonnali feladatátvételkor szolgáltatáscsomagot. Amint a kötet metaadatainak lekért a felhőbe a feladatátvevő során, a kötet online állapotba kerül cél az eszközre, és az állomás is elérhető. Eközben a mennyiségi adatok továbbra is szerepelni fognak a háttérben letöltése, és a feladatátvételi időtartama ezek kötet csökkentett teljesítmény tapasztalhatja.

**KÉRDÉSEK.** Jelenik meg a feladatátvevő feladat kész, hogyan lehet nyomon követheti visszaállítják helyileg rögzített mennyiségének meg a célként megadott eszköz?

**VÁLASZOK PARANCSRA.** Feladatátvevő művelet során a feladatátvevő feladat megjelölést kövesse egyszer feladatátvevő megadása a kötet van azonnali visszaállítva és online állapotba cél az eszközre. Ide tartoznak a bármely helyileg rögzített kötet, amely fölé; sikertelen lehet azonban helyi biztosítékokat az adatok csak akkor érhető el a kötet az adatok letöltésekor. A nyilvántartása minden helyileg rögzített kötet alapoktól figyelése a megfelelő visszaállítási feladatok a feladatátvételi részeként létrehozott sikertelen volt. Ezek a egyedi visszaállítási feladatok csak helyileg rögzített kötet hozhatók létre.

**KÉRDÉSEK.** Megváltoztathatom-e a mennyiségi típusú feladatátvételkor?

**VÁLASZOK PARANCSRA.** A hangerő-típus nem, meghibásodáskor nem módosítható. Nem működnek, ha fölé egy másik fizikai eszközre StorSimple 8000 futtató sorozat frissítése 2, a kötet sikertelen lesz fölé a mennyiség típus van tárolva a pillanatkép alapján. Feladat átvétele bármely más eszköz verziójához, olvassa el a fenti kérdést a mennyiségi adja feladatátvevő után.

**KÉRDÉSEK.** E sikertelen lehet az a felhő készüléken helyileg rögzített kötet mennyiségi tároló keresztül?

**VÁLASZOK PARANCSRA.** Igen, megteheti. A helyi meghajtóra rögzített kötet sikertelen lesz fölé többszintű kötet. További információt a [feladatátvétel és DR helyileg kiemelt kötet verziója között](storsimple-device-failover-disaster-recovery.md#considerations-for-device-failover)


