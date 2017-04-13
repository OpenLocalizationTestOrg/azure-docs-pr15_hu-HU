<properties 
   pageTitle="Egy StorSimple eszköz vezérlő cseréje |} Microsoft Azure"
   description="Megtudhatja, hogyan cserélje ki az egyik vagy mindkét vezérlő modulok StorSimple eszközén."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-a-controller-module-on-your-storsimple-device"></a>A vezérlő modul StorSimple eszközén cseréje

## <a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogyan cserélje ki az egyik vagy mindkét vezérlő modulok StorSimple eszközt. A cikk ismerteti az egy vagy két vezérlő cseréjekor az alapul szolgáló logikai is.

>[AZURE.NOTE] Előtt hajt végre a vezérlőhöz helyettesítő, azt javasoljuk, hogy mindig frissíti a vezérlő belső vezérlőprogram a legújabb verzióra.
>
>Sérülések megakadályozására StorSimple eszközére, ne vegye ki a vezérlő mindaddig, amíg a LED meg vannak jelenítve a következők egyikét:
>
>- Az összes fény ki.
>- LED 3-as ![zöld pipa ikonra](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), és ![ikon piros kereszt](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) villogó vannak, és LED 0 LED 7 **ON**lévő.

A következő táblázat mutatja a támogatott vezérlő cseréjekor.

|Eset|Új eset|Alkalmazandó eljárás|
|:---|:-------------------|:-------------------|
|1|Egy vezérlő sikertelen állapotban van, a többi vezérlő megfelelő és az aktív.|[Egy vezérlő feltölteni](#replace-a-single-controller), amely leírja a [logika mögött egy vezérlőt helyettesíti](#single-controller-replacement-logic), valamint a [Csere lépéseket](#single-controller-replacement-steps).|
|2|Mind a vezérlők nem sikerült, és a csere kötelezővé tétele A váz, lemezen and.disk ház is megfelelő.|[Kettős vezérlő feltölteni](#replace-both-controllers), amely leírja a [kettős vezérlőt helyettesíti mögött logika](#dual-controller-replacement-logic), valamint a [Csere lépéseket](#dual-controller-replacement-steps). |
|3|Vezérlők ugyanarra az eszközre, vagy más eszközökről is cserélni. A váz, a lemez és a lemez letölthetés is megfelelő.|Tárolóhely eltéréseket figyelmeztető üzenet jelenik meg.|
|4|Egy vezérlő hiányzik, és a többi vezérlő sikertelen lesz.|[Kettős vezérlő feltölteni](#replace-both-controllers), amely leírja a [kettős vezérlőt helyettesíti mögött logika](#dual-controller-replacement-logic), valamint a [Csere lépéseket](#dual-controller-replacement-steps).|
|5|Egyik vagy mindkét vezérlők sikertelen volt. Az eszköz nem érhetők el a soros konzoljában vagy a Windows PowerShell távelérési keresztül.|[Kapcsolatfelvétel a Microsoft támogatási szolgálattal](storsimple-contact-microsoft-support.md) kézi vezérlő helyettesítő eljárást.|
|6|A vezérlők van egy másik verziójával, amely oka lehet:<ul><li>Vezérlők különböző szoftver verziójával rendelkezik.</li><li>Vezérlők különböző belső vezérlőprogram verziójával rendelkezik.</li></ul>|Ha a vezérlő szoftver verziók eltérő, a csere logika észleli, hogy, és frissíti a szoftver verziószáma, a csere vezérlőn.<br><br>Ha a vezérlő belső vezérlőprogram változatokat különböző, és a régi belső vezérlőprogram verziója **nem** automatikusan bővíthető egy figyelmeztető üzenet jelenik meg az Azure klasszikus portálon. Érdemes frissítések keresése hivatkozásra, és a belső vezérlőprogram frissítések telepítéséhez.</br></br>A vezérlő belső vezérlőprogram változatokat különböző és a régi belső vezérlőprogram verzió automatikusan frissíthető, ha a vezérlő helyettesítő logika észleli ez, és miután elindult a vezérlő, a belső vezérlőprogram automatikusan frissülnek.|

Szeretné eltávolítani egy vezérlő modulra, ha sikertelen volt. Egyik vagy mindkét a vezérlő modulok sikertelen lehet, hogy egy vezérlő helyettesítő vagy kettős vezérlő helyettesítő eredményező. Helyettesítő útmutatást és a logika mögött őket talál a következő:

- [Csak egy vezérlő cseréje](#replace-a-single-controller)
- [Mindkét vezérlők cseréje](#replace-both-controllers)
- [Egy vezérlő eltávolítása](#remove-a-controller)
- [Egy vezérlő beszúrása](#insert-a-controller)
- [Azonosítsa az aktív az eszközön](#identify-the-active-controller-on-your-device)

>[AZURE.IMPORTANT] Előtt eltávolítása, és a vezérlő cseréje, tekintse át a biztonsági [StorSimple hardver összetevő](storsimple-hardware-component-replacement.md)helyett.

## <a name="replace-a-single-controller"></a>Csak egy vezérlő cseréje

Ha a Microsoft Azure StorSimple eszközön két vezérlőket nem sikerült, hibásan működik vagy hiányzik, kell cserélni egy adatkezelő. 

### <a name="single-controller-replacement-logic"></a>Egy vezérlő helyettesítő logikája

Az egyetlen vezérlőt helyettesíti el kell távolítania a hibás vezérlő. (A többi vezérlő az eszköz a az aktív vezérlőhöz.) A csere vezérlő beszúrásakor jelentkezhet a következő műveleteket:

1. A csere vezérlő azonnal elindul, a StorSimple lejátszóval való kommunikáció.

2. A virtuális merevlemez (virtuális) az aktív vezérlő pillanatképét a program a vezérlőn feltölteni.

3. A pillanatkép módosul, hogy a csere vezérlő indításakor a virtuális a felismerhető lesz készenléti szerepkörű.

4. Amikor elkészült a módosításokat, a csere vezérlő indul el, a készenléti vezérlő.

5. Ha mind a vezérlők fut, a fürt online állapotba kerül.

### <a name="single-controller-replacement-steps"></a>Egy vezérlő helyettesítő lépések

Ha nem sikerül egy a Microsoft Azure StorSimple eszközön vezérlőket, kövesse az alábbi lépéseket. (A többi vezérlő kell lennie az aktív és futnia. Ha mindkét vezérlők sikertelen közvetíthetnek, látogasson el [kettős vezérlő helyettesítő lépést](#dual-controller-replacement-steps).)

>[AZURE.NOTE] A vezérlő indítani, és az adatkezelő helyettesítő eljárás teljesen helyreállítása 30 – 45 perc is eltarthat. A teljes időt, a teljes folyamathoz csatolása a kábelek, beleértve az körülbelül 2 órán.

#### <a name="to-remove-a-single-failed-controller-module"></a>Ha el szeretne távolítani egy nem sikerült a vezérlő modul

1. Az Azure klasszikus portálon nyissa meg a StorSimple kezelő szolgáltatás, kattintson az **eszközök** fülre, és kattintson a nevére a figyelni kívánt eszköz.

2. Nyissa meg a **karbantartás > hardver állapot**. Vezérlő 0 vagy 1 vezérlő állapotának kell piros, amely hibát jelez.

    >[AZURE.NOTE] A hibás vezérlő egyetlen vezérlőt helyettesíti be értéke mindig egy készenléti vezérlő.

3. Ábra 1 és az alábbi táblázat segítségével keresse meg a hibás vezérlő modulra.  

    ![Az eszköz elsődleges ház modulok hátlap](./media/storsimple-controller-replacement/IC740994.png)

    **Ábra 1** Hátsó StorSimple eszköz

  	|Címke|Leírás|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Vezérlő 0|
  	|4|1 vezérlő|

4. Sikertelen a vezérlőn távolítsa el a csatlakoztatott hálózati kábel csatlakoztatva az adatokat portokat. Egy 8600 modellt használja, ha is távolítsa el a csatlakozás a vezérlő a EBOD vezérlő Társítások kábelek.

5. Kövesse a [egy vezérlő eltávolítása](#remove-a-controller) a hibás vezérlő eltávolításához. 

6. A gyár helyettesítő telepítse a azonos tárolóhely, amelyből el lett távolítva a hibás vezérlő. Ez elindítja az adatkezelő helyettesítő logika. További tudnivalókért lásd: az [adatkezelő helyettesítő összefüggés](#single-controller-replacement-logic).

7. Az adatkezelő helyettesítő logika halad, a háttérben fut, amíg újra a kábelek. Csatlakozás minden kábel pontosan a várt módon, hogy azok csatlakoztatva a csere előtt gondoskodik.

8. A vezérlő újraindítása után ellenőrizze a **vezérlő állapotát** , és az **állapot fürt** az Azure klasszikus portálon ellenőrizheti, hogy a vezérlő van egy megfelelő állapotra, és készenléti módban van.

>[AZURE.NOTE] Ha az eszköz a soros konzolon figyelni, jelenhetnek meg több újraindul közben a vezérlő helyreállítás van, a csere eljárásból. Ha a soros konzol menüben látható, majd tudni fogja, hogy helyesek-e a csere. Ha nem jelenik meg a menü, a vezérlő csere indítása két órán belül, tájékoztassa a [lépjen kapcsolatba a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md).

## <a name="replace-both-controllers"></a>Mindkét vezérlők cseréje

Ha mind a Microsoft Azure StorSimple eszközön vezérlők nem sikerült, vannak hibás vagy hiányzik, is kell mindkét vezérlők cseréje. 

### <a name="dual-controller-replacement-logic"></a>Kettős vezérlő helyettesítő logikája

A kettős vezérlőt helyettesíti először távolítsa el a mindkét hibás vezérlők, és helyezze a javaslatok. A két helyettesítő vezérlők beillesztésekor az alábbi műveletek történnek:

1. Az új adatkezelő tárolóhely 0 ellenőrzi a következőket:
 
   1. Azt a belső vezérlőprogram és a szoftver jelenlegi verzióját használja?

   2. Ez a fürt része?

   3. Az operációs rendszert futtató peer vezérlő és csoportosított?
                            
    Ha e feltételek valamelyike sem igaz, a vezérlő keres a legújabb napi biztonsági (a **nonDOMstorage** S meghajtón található). A vezérlő másolja a virtuális legújabb pillanatképe a biztonsági mentés.

2. Az adatkezelő tárolóhely 0 kép magát a pillanatkép használja.

3. Az adatkezelő tárolóhely 1 eközben a vezérlő 0, és fejezze be a imaging várakozik.

4. Mailekkel, miután vezérlő 0-1 vezérlő észleli, a 0, a vezérlő által létrehozott fürt, amely elindítja az adatkezelő helyettesítő logika. További tudnivalókért lásd: az [adatkezelő helyettesítő összefüggés](#single-controller-replacement-logic).

5. Ezután mindkét vezérlők fog futni és a fürt online állapotba.

>[AZURE.IMPORTANT] A kettős vezérlő helyettesítő követően után a StorSimple eszköz be van állítva, fontos, hogy Ön által a manuális az eszköz biztonsági. Napi eszköz konfigurációs biztonsági másolatok nem indított addig, amíg 24 óra letelte után. [A Microsoft támogatási](storsimple-contact-microsoft-support.md) , hogy az eszköz biztonsági másolatot a manuális dolgozhat.

### <a name="dual-controller-replacement-steps"></a>Kettős vezérlő helyettesítő lépések

Ez a munkafolyamat szükség, ha mindkét a vezérlőkkel kapcsolatban a Microsoft Azure StorSimple eszközt nem sikerült. Ennek a adatközpontban, amelyben a hűtési rendszer nem működik, és emiatt nem sikerül mindkét vezérlők belül egy rövid ideig. Attól függően, hogy a az StorSimple eszköz engedélyezve van-e be- és kikapcsolása, és e-8600 használja, vagy egy 8100 modellt, más beállítási lépéseket szükség.

>[AZURE.IMPORTANT] 45 perc is eltarthat, indítsa újra, és egy kettős vezérlő helyettesítő eljárást teljesen helyreállítása a vezérlő 1 óra értéket. A teljes a teljes eljárás csatolása a kábelek, például ideje körülbelül 2,5 óra.

#### <a name="to-replace-both-controller-modules"></a>Mindkét vezérlő modulok cseréje

1. Ha az eszköz ki van kapcsolva, a lépés kihagyását, és folytassa a következő lépéssel. Ha az eszköz be van kapcsolva, kapcsolja ki az eszközt.
                                        
    1. Ha egy 8600 modellt használja, kapcsolja ki az elsődleges ház első, és kapcsolja ki a EBOD ház.

    2. Várja meg, amíg az eszköz teljesen leállt. Az eszköz háttereként összes LED ki lesz.

2. Távolítson el minden az adatok portokat csatlakozó hálózati kábel. Egy 8600 modellt használja, ha is távolítsa el a csatlakozás az elsődleges ház a EBOD ház Társítások kábelek.

3. Mindkét vezérlők eltávolítása az StorSimple eszközről. További tudnivalókért olvassa el a [egy vezérlő eltávolítása](#remove-a-controller)című témakört.

4. A vezérlő 0 gyári helyett előbb szúrja be, és helyezze a vezérlő 1. További tudnivalókért olvassa el a [egy vezérlő beszúrása](#insert-a-controller)című témakört. Ez elindítja a kettős vezérlő helyettesítő logika. További tudnivalókért lásd: a [kettős vezérlő helyettesítő logika](#dual-controller-replacement-logic).

5. A vezérlő helyettesítő logika halad, a háttérben fut, amíg újra a kábelek. Csatlakozás minden kábel pontosan a várt módon, hogy azok csatlakoztatva a csere előtt gondoskodik. [A StorSimple 8100 eszköz telepítése](storsimple-8100-hardware-installation.md) vagy [a StorSimple 8600 eszköz telepítése](storsimple-8600-hardware-installation.md)a kábelt a eszköz csoportban a modell részletes útmutatást találhat.

6. Kapcsolja be a StorSimple eszközt. Ha egy 8600 modellt használja:

    1. Győződjön meg arról, hogy a EBOD ház első van-e kapcsolva.

    2. Várja meg, amíg fut az EBOD ház.

    3. Kapcsolja be az elsődleges ház.

    4. Miután az első vezérlő újraindítása és megfelelő állapotban van, a rendszer fog futni.

    >[AZURE.NOTE] Ha az eszköz a soros konzolon figyelni, jelenhetnek meg több újraindul közben a vezérlő helyreállítás van, a csere eljárásból. Ha megjelenik a soros konzol menüre, majd tudni fogja, hogy helyesek-e a csere. Ha nem jelenik meg a menü, a vezérlő csere indítása 2,5 órán belül, tájékoztassa a [lépjen kapcsolatba a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md).

## <a name="remove-a-controller"></a>Egy vezérlő eltávolítása

A következő eljárással hibás vezérlő modul eltávolításához StorSimple eszközére.

>[AZURE.NOTE] A következő ábrák vezérlő 0 készültek. 1-vezérlő ezek volna vonható vissza.

#### <a name="to-remove-a-controller-module"></a>Ha el szeretne távolítani egy vezérlő modul

1. A hordozható és mutatóujj között a modul zár bonyolultnak.

2. Nyomja meg össze enyhén a hordozható és mutatóujj való közös kiadás a vezérlő együtt.

    ![Vezérlő együtt feloldása](./media/storsimple-controller-replacement/IC741047.png)

    **Ábra 2** Vezérlő együtt feloldása

2. A zár használni egy leíró diaelrendezés a váz ki a vezérlőt.

    ![Kijelentkezés a váz csúsztatható vezérlő](./media/storsimple-controller-replacement/IC741048.png)

    **Ábra 3** A vezérlő ki a váz csúszó

## <a name="insert-a-controller"></a>Egy vezérlő beszúrása

Az alábbi eljárással egy vezérlő gyári által biztosított modul telepítése, miután eltávolított egy hibás modulra StorSimple eszközéről.

#### <a name="to-install-a-controller-module"></a>Egy vezérlő modul telepítése

1. Ellenőrizze, hogy van-e a felület összekötőkhöz károkért. A modul nem telepíthető, ha az összekötő PIN bármelyikét sérült vagy elhajolnak.

2. Dia a váz be a vezérlő modul, miközben a együtt teljesen megjelent. 

    ![Az váz csúsztatható vezérlő](./media/storsimple-controller-replacement/IC741053.png)

    **A 4-es szám** A váz be csúsztatható vezérlő

3. A beszúrt vezérlő modulban kezdődhet a zár bezárása továbbra is a vezérlő modul leküldéses be a váz közben. A zár vesz részt, hogy segítségével iránymutatást adjon a vezérlő tetszőleges helyére.

    ![Vezérlő együtt bezárása](./media/storsimple-controller-replacement/IC741054.png)

    **Ábra 5** A vezérlő zár bezárása

4. Ezzel elkészült, amikor a zár illesztése tetszőleges helyére. Kattintson az **OK** LED most már.  

    >[AZURE.NOTE] Legfeljebb 5 percig tart a vezérlő és a LED aktiválása is eltarthat.

5. Ellenőrizze, hogy a csere sikeres, az Azure klasszikus portálon, lépjen az **eszközök** > **karbantartási** > **Hardveres állapotát**, és győződjön meg róla, hogy megfelelő vezérlő 0 és 1 vezérlő is (az Állapot oszlop értéke zöld).

## <a name="identify-the-active-controller-on-your-device"></a>Azonosítsa az aktív az eszközön

Vannak olyan helyzetek, például az első eszközön regisztráció vagy vezérlő feltölteni, melyekhez fel kell keresse meg az aktív vezérlő StorSimple eszközön. Az aktív vezérlő feldolgozásával az összes lemezre belső vezérlőprogram és a hálózati művelet. Az aktív vezérlő azonosítása használhatja az alábbi módszerek egyikét:

- [Az Azure klasszikus portal segítségével azonosítani az aktív vezérlő](#use-the-azure-classic-portal-to-identify-the-active-controller)

- [Azonosítsa az aktív StorSimple a Windows PowerShell használatával](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)

- [Az aktív vezérlő azonosítása fizikai eszköz ellenőrzése](#check-the-physical-device-to-identify-the-active-controller)

Ezeket a műveleteket leírását Tovább gombra.

### <a name="use-the-azure-classic-portal-to-identify-the-active-controller"></a>Az Azure klasszikus portal segítségével azonosítani az aktív vezérlő

Az Azure klasszikus portálon nyissa meg az **eszközök** > **karbantartási**, és görgetéssel keresse meg a **vezérlők** csoport. Itt ellenőrizheti, mely vezérlő tevékenykedik.

![A klasszikus portál Azure active vezérlő azonosítása](./media/storsimple-controller-replacement/IC752072.png)

**Ábra 6** Azure klasszikus portal, rajta az aktív vezérlő

### <a name="use-windows-powershell-for-storsimple-to-identify-the-active-controller"></a>Azonosítsa az aktív StorSimple a Windows PowerShell használatával

Az eszköz a soros konzolon elérésekor transzparens üzenet jelenik meg. A szalagcím üzenet alapvető eszközadatok, például a modell nevét, telepített szoftver verziószáma és érik el a vezérlő állapotának tartalmazza. Az alábbi képen az látható transzparens üzenet:

![Üzenet a soros transzparens](./media/storsimple-controller-replacement/IC741098.png)

**Ábra 7** Üzenet megjelenítő vezérlő 0 aktív szalagcím

A szalagcím üzenet segítségével megállapítható, hogy a vezérlő csatlakozik az aktív vagy passzív-e.

### <a name="check-the-physical-device-to-identify-the-active-controller"></a>Az aktív vezérlő azonosítása fizikai eszköz ellenőrzése

Azonosítsa az aktív az eszközön, keresse meg a kék vezetett az adatok felett 5 port az elsődleges ház hátulján.

Ha a LED villogó van, a vezérlő aktív és a többi vezérlő készenléti. Használja az alábbi ábra és a táblázat elősegítésére.

![Eszköz elsődleges ház hátlap adatportok együtt](./media/storsimple-controller-replacement/IC741055.png)

**Ábra 8** Az adatok portokat és felügyeleti LED elsődleges ház oldalán

|Címke|Leírás|
|:----|:----------|
|1 – 6|ADATOK 0-5 hálózati portok|
|7|Kék LED|


## <a name="next-steps"></a>Következő lépések

További tudnivalók a [StorSimple hardver összetevő feltölteni](storsimple-hardware-component-replacement.md).
