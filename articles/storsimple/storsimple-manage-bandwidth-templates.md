<properties
   pageTitle="A StorSimple sávszélesség-sablonok kezelése |} Microsoft Azure"
   description="Ismerteti, hogyan kezelheti a StorSimple sávszélesség sablonokat, amely lehetővé teszi, hogy sávszélességet."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storsimple-bandwidth-templates"></a>A StorSimple kezelő szolgáltatás használatával StorSimple sávszélesség-sablonok kezelése

## <a name="overview"></a>– Áttekintés

Sávszélesség-sablonok végig az első csoportba tartozó az adatokat a StorSimple eszközről a felhőbe történő több időt nap ütemezés konfigurálása a hálózati sávszélesség-használat teszi lehetővé.

A sávszélesség-szabályozási ütemezések van lehetősége:

- Adja meg a testre szabott sávszélesség ütemezések attól függően, hogy a terhelést hálózati típusú helytelen szóhasználat.

- Kezelésének központosítása, és újra felhasználhatja a kimutatások egy egyszerű és zavartalan módon különféle eszközökön keresztül.

> [AZURE.NOTE] Ez a funkció csak az StorSimple fizikai eszközök, nem pedig virtuális eszközök érhető el.

A szolgáltatás sávszélesség-sablonok táblázatos formában jelennek meg, és az alábbi információkat tartalmazzák:

- **Név** – egy egyedi nevet a sávszélesség-sablon rendelt létrehozásakor jött létre automatikusan.

- **Ütemezés** – egy adott sávszélesség sablonban lévő ütemezések számát.

- **Használja** a sávszélesség-sablonok használata kötet száma.

Használatával a StorSimple kezelő szolgáltatás **beállítása** lapon az Azure klasszikus portálon kezelheti a sávszélesség sablonokat.

Is talál további információt találhat a sávszélesség-sablonok a konfigurálása:

- Kérdések és válaszok sávszélesség-sablonok
- Gyakorlati tanácsok a sávszélesség-sablonok

## <a name="add-a-bandwidth-template"></a>A sávszélesség-sablon hozzáadása

A következő lépésekkel hozzon létre egy új sávszélesség-sablont.

#### <a name="to-add-a-bandwidth-template"></a>A sávszélesség-sablon hozzáadása

1. StorSimple kezelő szolgáltatás **beállítása** lapon kattintson a **sávszélesség-sablon hozzáadása vagy szerkesztése**.

2. A **Sávszélesség-sablon hozzáadása/módosítása** párbeszédpanelen:

   1. A **sablon** legördülő listából válassza az **Új létrehozása** , egy új sávszélesség-sablon hozzáadása lehetőséget.
   2. Adja meg, hogy egy egyedi nevet a sávszélesség-sablon.

3. **Sávszélesség ütemezés**meghatározásához. Az ütemterv készítése:

   1. A legördülő listából válassza ki az ütemtervet van konfigurálva a hét napjai. Több napig választhat a megfelelő jelölőnégyzetek bejelölésével, amely a listában a megfelelő napok előtt.
   2. Ha az ütemezés lép érvénybe a teljes nap válassza az **Egész napos** lehetőséget. Ha ez a beállítás be van jelölve, már nem adhatja meg a **Kezdési idő** vagy a **Befejezési időpontot**. Az ütemezés 11:59 PM fut a 12:00 de.
   3. A legördülő listából válassza ki a **Kezdés időpontja**. Az amikor az ütemezés elindul.
   4. A legördülő listából jelölje be a **Befejezési időpontot**. Az amikor az ütemezés leáll.

         > [AZURE.NOTE] Egymást átfedő ütemtervek nem használhatók. A kezdő és záró időpont az átfedő ütemezés eredményez, ha értelmű egy hibaüzenet jelenik meg.

   5. Adja meg a **sávszélesség ráta**. Ez a második (MB), az a felhő (feltöltések és letöltések) érintő műveletek StorSimple eszköze által használt MB sávszélességének. Adjon meg egy 1 és ehhez a mezőhöz 1000 közötti számot.

   6. Kattintson az ellenőrzés ikon ![ellenőrzés ikon](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). A sablont, Ön által létrehozott hozzáadódik a szolgáltatás **konfigurálása** lapon sávszélesség-sablonok listájában.

    ![Új sávszélesség-sablon létrehozása](./media/storsimple-manage-bandwidth-templates/HCS_CreateNewBT1.png)

4. Kattintson a **Mentés** elemre a lap alján, és kattintson az **Igen** , amikor a program megerősítést kér gombra. Ez menti a módosításokat elvégzett módosításokkal.

## <a name="edit-a-bandwidth-template"></a>A sávszélesség-sablon szerkesztése

A következő lépésekkel sávszélesség sablon szerkesztése.

### <a name="to-edit-a-bandwidth-template"></a>A sávszélesség-sablon szerkesztése

1. A **sávszélesség-sablon hozzáadása/szerkesztése**parancsra.

2. A **Sávszélesség-sablon hozzáadása/módosítása** párbeszédpanelen:

   1. A **sablon** legördülő listából válassza ki a meglévő sávszélesség sablon, amely a módosítani kívánt.
   2. Végezze el a módosításokat. (Módosíthatja a meglévő beállítások bármelyikének.)
   3. Kattintson az ellenőrzés ikon ![Ellenőrzés ikon](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). A szolgáltatás lap látni fogja a módosított sablont a sávszélesség-sablonok listájában.

3. A módosítások mentéséhez kattintson a **Mentés** elemre a lap alján. Kattintson az **Igen** , amikor a program megerősítést kér.

> [AZURE.NOTE] Akkor fog nem tenni, hogy a módosítások mentéséhez, ha egy meglévő kimutatás módosítani a sávszélesség-sablonban átfedésben a szerkesztett ütemezés.

## <a name="delete-a-bandwidth-template"></a>A sávszélesség sablon törlése

A következő lépésekkel sávszélesség sablon törlése.

#### <a name="to-delete-a-bandwidth-template"></a>A sávszélesség sablon törlése

1. A sávszélesség-sablonok a szolgáltatásbeli táblázatos listájában válassza ki a törölni kívánt sablont. A Törlés (**x**) ikonra a jobb szélső a kiválasztott sablon jelenik meg. Kattintson a sablon törlése az **x** ikonra.

2. Megerősítését kéri. Kattintson az **OK gombra** a folytatáshoz.

Ha a sablont minden olyan kötet(ek) használja, akkor nem használható kattintva törölheti azt. Ekkor megjelenik egy hiba üzenet azt jelzi, hogy a sablont használják. Üzenet párbeszédpanel jelenik meg értesítő, hogy el kell távolítani a sablont a hivatkozásokat.

A **Mennyiségi tárolók** lap elérése, és a hangerő tárolók, amely használja ezt a sablont, hogy a sablon egy másik, vagy egyéni vagy korlátlan sávszélesség beállítás használata módosításával törölheti a sablont a hivatkozásokat. Ha az összes hivatkozás eltávolítása a sablon törölheti.

## <a name="use-a-default-bandwidth-template"></a>A sávszélesség, alapértelmezett sablon használatához

Alapértelmezett sávszélesség sablon hiányzik, és segítségével mennyiségi tárolók alapértelmezés szerint a hivatkozási sávszélesség-vezérlők, amikor elérése a felhőben. Az alapértelmezett sablon is figyelmezteti arra a felhasználóknak, akik a saját sablonok létrehozása készen áll a hivatkozást. Ezzel a sablonnal részleteit a következők:

- **Név** – korlátlan éjszakák és hétvégék

- **Ütemezés** – egyetlen ütemezett hétfőtől péntekig, amely egy 1 MB közötti 8 AM és 5 eszköz idő sávszélesség aránya vonatkozik. A sávszélesség a hét maradéka korlátlan értékre van állítva.

Szerkesztheti a fájlsablont. Az ezzel a sablonnal (beleértve a szerkesztett verziók) használatát tényét.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Egy egész napos egy adott időben kezdődik sávszélesség-sablon létrehozása

Kövesse az alábbi eljárást egy megadott idő és fut, egész napos ütemezés létrehozása. A példában az ütemezés 9 AM délelőtt kezdődő és fut, csak a következő reggeli teendőkhöz 9 óra. Fontos, hogy ne feledje, hogy az adott ütemezett kezdési és befejezési időpontot mindkét tartalmaznia kell a 24 órás azonos ütemtervben, és nem lehet a több napon. Ha több napon átnyúló sávszélesség-sablonok beállítása kell szüksége lesz a használni kívánt több ütemezés (a példában látható).

#### <a name="to-create-an-all-day-bandwidth-template"></a>Egy egész napos sávszélesség-sablon létrehozása

1. 9 AM délelőtt és éjfél fut ütemterv készítése.

2. Adja hozzá a másik ütemezése. Állítsa be a második ütemezés csak 9 óra délelőtt éjfél futtathat.

3. A sávszélesség-sablon mentéséhez.

Az összetett ütemezés ezután indítsa el a választás az egyszerre, és futtassa az egész napos.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Kérdések és válaszok sávszélesség-sablonok

**Q**. Mi történik a sávszélesség-vezérlők, legyen az ütemterveket közben? (Ütemezés véget ért, és egy másikat még nem kezdődött.)

**A**. Ezekben az esetekben sávszélesség-vezérlők nélküli alkalmazzák. Ez azt jelenti, hogy az eszközön használhatja a adatok a felhőbe tiering korlátlan sávszélességet.

**Q**. Módosíthatja a kapcsolat nélküli eszközökön sávszélesség-sablonok?

**A**. Nem tudja módosítani a sávszélesség-sablonok kötet tárolók az, ha a megfelelő eszköz offline állapotban.

**Q**. Szerkesztheti a társított kötet használata offline módban mennyiségi tároló társított sávszélesség sablon?

**A**. Módosíthatja, amelynek kötet használata offline módban mennyiségi tároló társított sávszélesség sablon. Figyelje meg, hogy kötet offline módban, amikor adatokat nem fog kell többszintű az eszközről a felhőbe.

**Q**. Törölheti egy alapértelmezett sablont?

**A**. Bár a egy alapértelmezett sablont, még nem célszerű erre. Az alapértelmezett sablont, köztük a szerkesztett-verziók használatát tényét. A nyomon követési adatok elemzése van, és ideje alatt, használja az alapértelmezett sablon javítása érdekében.

**Q**. Hogyan, meg, hogy kell-e a sávszélesség-sablonok módosítani kell?

**A**. A jelek módosítania kell a sávszélesség-sablonok egyike jelenik meg a hálózat lelassulhat vagy többször a nap fojtótekercs indításakor. Ez történik, ha figyelheti a tárhely és a használatát a hálózat megjeleníti a I/O teljesítmény és a hálózati teljesítmény diagramokat.

A hálózati teljesítmény adatokból azonosítása: a nap és a hangerő tárolók hálózati szűk fordul elő. Ha ez történik, ha az adatokat a felhőbe éppen többszintű (Ez az információ beolvasása I/O teljesítményt cloud-eszközökre készült összes mennyiségi tárolók), majd meg kell a mennyiségi tárolók társított sávszélesség-sablonok módosítása.

A módosított sablonok használatban vannak, miután szüksége lesz figyelheti a hálózat ismét jelentős késések. Ha továbbra is vannak, majd szüksége lesz a sávszélesség-sablonok látogatnia.

**Q**. Mi történik, ha több mennyiségi tárolók eszközeimre ütemez, hogy az átfedő, de minden más korlátok vonatkoznak?

**A**. Tegyük fel, hogy 3 mennyiségi jegyzettárolót egy eszközt. Az alábbi tárolók teljesen társított ütemezések átfedik egymást. Az egyes tárolókban a sávszélesség használt határértékeket 5, 10 és 15 MB rendre. Műveletekkel együtt egyszerre összes tárolókban történnek, amikor a legkisebb értéke a 3 sávszélesség-korlátok alkalmazhatók: Ebben az esetben az 5 MB, ezek a kimenő I/O kérések megosztása a ugyanazon sor.

## <a name="best-practices-for-bandwidth-templates"></a>Gyakorlati tanácsok a sávszélesség-sablonok

Hajtsa végre az alábbi gyakorlati tanácsok az StorSimple eszköz:

- Állítsa be a sávszélesség-sablonok változó szabályozásának a hálózati teljesítmény, az eszköz a nap különböző időpontokban engedélyezése az eszközön. A sávszélesség-sablonok biztonsági ütemezések együtt használva hatékony is kihasználhatja a felhőben műveletekhez további hálózati sávszélesség során essen.

- Kiszámítja a tényleges sávszélesség méretét a telepítési és a szükséges helyreállítási idő cél (RTO) alapján egy adott telepítéshez szükséges.

## <a name="next-steps"></a>Következő lépések

További tudnivalók [felügyelheti StorSimple eszköze az StorSimple kezelő szolgáltatás használatával](storsimple-manager-service-administration.md).
