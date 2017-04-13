<properties
   pageTitle="Telepítse a frissítés 2.2 StorSimple eszközön |} Microsoft Azure"
   description="Megtudhatja, hogyan telepítheti StorSimple 8000 sorozat frissítés 2.2 StorSimple 8000 sorozat eszközén."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/02/2016"
   ms.author="alkohli" />

# <a name="install-update-22-on-your-storsimple-device"></a>Telepítse a frissítés 2.2 StorSimple mobileszközön

## <a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogyan frissítés 2.2 telepítése egy korábbi verzióját szoftver az Azure klasszikus portálon keresztül, és a gyorsjavítás módszerrel StorSimple eszközre. A gyorsjavítás módszer használható az átjárók be van állítva az StorSimple eszköz adatainak 0-től különböző hálózati kapcsolaton és 1 szoftver frissítés előtti verziójáról frissíteni próbálja.

Frissítés 2.2 eszközhöz, WMI és iSCSI frissítések tartalmazza. Ha frissíti a 2.1-es verzió, az eszköz biztonsági frissítés kell vonatkozni. Ha frissíti a frissítés előtti 2 verziójú, akkor is kötelező LSI illesztőprogram, Spaceport, Storport és lemezre belső vezérlőprogram frissítéseket alkalmazni. Az eszköz szoftver, a WMI, iSCSI, LSI illesztőprogram, Spaceport és Storport javítások nem zavaró frissítéseket, és az Azure klasszikus portálon keresztül alkalmazhatók. A lemez belső vezérlőprogram frissítéseket zavaró frissítéseket, és csak az eszközön a Windows PowerShell felületén keresztül kell alkalmazni. 

> [AZURE.IMPORTANT]

> - Kézzel és automatikusan előtti ellenőrzések csoportja a hardver állapot és a hálózati kapcsolat pedig az eszköz állapotáról a telepítés előtt történik. Ezek előtti engedélyezné csak akkor, ha a frissítések telepítését az Azure klasszikus portálról.
> - Azt javasoljuk, hogy telepítse a szoftverek és -illesztőprogram frissítések az Azure klasszikus portálon keresztül. A Windows PowerShell-felület (a frissítések telepítése) az eszköz csak meg kell megnyitásához, ha a frissítés előtti átjáró jelölőnégyzet nem sikerül a portálon. Függően a frissíteni kívánt verzió, a frissítések órát is igénybe vehet 1.5-2,5 telepítéséhez. A karbantartás mód frissítések telepíteni kell az eszközön a Windows PowerShell felületén keresztül. Mivel karbantartási mód frissítések zavaró frissítések, ezek az eszköz lefelé időpontját eredményezi.
> - Ha a választható StorSimple pillanatkép kezelő fut, győződjön meg arról, hogy frissített pillanatkép Manager verziójától a frissítés 2.2 az eszköz frissítése előtt.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-22-via-the-azure-classic-portal"></a>Frissítés 2.2 telepítse az Azure klasszikus portálon keresztül

A következő lépésekkel [frissítése 2.2](storsimple-update21-release-notes.md)frissítése az eszközön.


> [AZURE.NOTE]
Ha meg vannak alkalmazása a frissítés 2 vagy újabb (beleértve a frissítés 2.1), Microsoft tudják csoportosítani az eszközről a diagnosztikai tájékozódhat. Eredményt adja Ha a tevékenységek csoportunk azonosítja problémák vannak eszközök, vagyunk jobban ellátni a információk összegyűjtése az eszközről és problémáinak diagnosztizálása. Frissítés 2-es vagy újabb elfogadásával engedélyezi a velünk a megelőző támogatása érdekében.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Győződjön meg arról, hogy eszköze problémamentesen működik **StorSimple 8000 sorozat frissítés 2.2 (6.3.9600.17708)**. A **Legutóbbi frissítés dátum** módosítani kell a is. 

    Ha frissítés 2 előtt verzióról frissít, akkor is látható, hogy a karbantartás mód frissítések is elérhetők (Ez az üzenet előfordulhat, hogy továbbra is megjelennek a frissítések telepítése után akár 24 órát).

    Karbantartási mód frissítései eszköz legrövidebb leállás eredményez, és csak az eszközön a Windows PowerShell felületén keresztül vonatkozni zavaró frissítéseket. Bizonyos esetekben, amikor frissítés 1.2-es operációs rendszert futtató a lemez belső vezérlőprogram már valószínűleg naprakész, ebben az esetben nem kell minden karbantartási mód frissítések telepítése.

    Ha a frissítés 2 frissít, az eszköz most már naprakész. Kihagyhatja a hátralévő lépéseket.

13. Töltse le a karbantartás mód frissítések kereséséhez és letöltéséhez KB3121899, mely telepítések lemez belső vezérlőprogram frissítések (a frissítések kell telepítve kell lennie, most) [letöltéséhez gyorsjavítások](#to-download-hotfixes) szereplő lépéseket használatával.

13. Kövesse a lépéseket az [telepítése, és ellenőrizze a karbantartás mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes) a karbantartás mód frissítések telepítéséhez. 

  

## <a name="install-update-22-as-a-hotfix"></a>Telepítse a frissítés 2.2 gyorsjavításként

Akkor használja ezt az eljárást, ha a meg az Azure klasszikus portálon keresztül a frissítések telepítése nem sikerül az átjáró jelölőnégyzetet. A jelölőnégyzet nem sikerül az átjáró nem adatok 0 hálózati kártya rendelt van, és az eszköze problémamentesen működik 1 frissítés előtt a szoftver verziószáma.

A szoftver a gyorsjavítás módszerrel frissíthető változatokat:

- Frissítse a 0,1 értéket, 0,2, 0,3
- 1, 1.1-es, 1.2-es frissítése
- 2.1-es 2, frissítése 

> [AZURE.IMPORTANT]
>
> - Ha az eszköz megjelenés kiadás verziója fut, lépjen kapcsolatba a segítséget nyújtanak a frissítés a [Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) .

A gyorsjavítás módszer a következő három lépésből:

- Töltse le a Microsoft Update katalógus gyorsjavítások.
- Telepítse, és ellenőrizze a normál mód gyorsjavításokat.
- Telepítse, és ellenőrizze a karbantartás mód javítás, (csak frissítésekor a frissítés előtti 2 szoftver).

#### <a name="download-updates-for-a-device-running-update-21-software"></a>Töltse le a frissítések frissítés 2.1 szoftvert futtat eszköz

**Ha eszköze problémamentesen működik frissítése 2.1-es**, le kell töltenie a csak az eszköz szoftver frissítésének KB3179904. A "minden-hcsmdssoftwareudpate" tartalmazni binárisfájl csak telepíteni. Nem telepíthető a Cis és a minimális letöltési stratégia ügynök frissítés előtt a létrehozás `all-cismdsagentupdatebundle`. Ezt a hibát hibát eredményez. Ez a zavaró frissítés, IO hibásan nem fognak működni, és az eszköz nem lesz bármely legrövidebb leállás.


#### <a name="download-updates-for-a-device-running-update-2-software"></a>Töltse le a frissítések frissítés 2 szoftvert futtat eszköz

**Ha eszköze problémamentesen működik frissítés 2**, töltse le és telepítse a következő gyorsjavításokat a előírt sorrendben:

| Sorrend  | KB        | Leírás                    | Frissítés típusa  | Idő telepítése |
|--------|-----------|-------------------------|------------- |-------------|
| 1.      | KB3179904 | #42; & szoftver frissítése  |  Normál <br></br>Nem zavaró     | ~ 45 perc |
| 2.      | KB3146621 | iSCSI csomag | Normál <br></br>Nem zavaró  | ~ 20 perc |
| 3.      | KB3103616 | WMI csomag |  Normál <br></br>Nem zavaró      | ~ 12 perc |


 & #42;  Két bináris fájl *megjegyzést, szoftver frissítés áll: eszköz szoftver frissítése előtt a létrehozás `all-hcsmdssoftwareupdate` és a tartalmazni a Cis és a minimális letöltési stratégia agent `all-cismdsagentupdatebundle`. A Cis és a minimális letöltési stratégia agent előtt telepíteni kell az eszköz frissítését. Az aktív vezérlő útján is újra kell indítani a `Restart-HcsController` parancsmagot a Cis és a minimális letöltési stratégia agent alkalmazása után módosítása (és frissíti a fennmaradó alkalmazása előtt).* 

#### <a name="download-updates-for-a-device-running-pre-update-2-software"></a>Töltse le a frissítések frissítés előtti 2 szoftvert futtat eszköz

**Ha eszköze problémamentesen működik 0,2, 0,3, 1.0, és 1.1-es verzió**, töltse le és telepítse a LSI illesztőprogram és a belső vezérlőprogram frissítése mellett a szoftver iSCSI és WMI frissítéseket. A frissítés már telepítve van, ha a frissítés 1.2-es és 2. 
 
| Sorrend  | KB        | Leírás                    | Frissítés típusa  | Idő telepítése |
|--------|-----------|-------------------------|------------- |-------------|
| 4.      | KB3121900 | LSI illesztőprogram és a belső vezérlőprogram             |  Normál <br></br>Nem zavaró      | ~ 20 perc |


<br></br>
**Ha eszköze problémamentesen működik 0,2, 0,3, 1.0, 1.1-es és 1.2-es verzió**, töltse le és telepítse a Spaceport és a Storport fix. Ezek már telepítve, ha futtatja a frissítés 2.

| Sorrend  | KB        | Leírás                    | Frissítés típusa  | Idő telepítése |
|--------|-----------|-------------------------|------------- |-------------|
| 5.      | KB3090322 | Spaceport kijavítása </br> A Windows Server 2012 R2 |  Normál <br></br>Nem zavaró      | ~ 20 perc |
| 6.      | KB3080728 | Storport kijavítása </br> A Windows Server 2012 R2 |  Normál <br></br>Nem zavaró      | ~ 20 perc |



<br></br>
Is előfordulhat lemez belső vezérlőprogram frissítések telepítéséhez. Ellenőrizheti, hogy kell-e a lemez belső vezérlőprogram frissítések futtatásával a `Get-HcsFirmwareVersion` parancsmag. Ha ezek a belső vezérlőprogram verziók futtatja: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, majd a frissítések telepítése nem szükséges.


| Sorrend  | KB        | Leírás                    | Frissítés típusa  | Idő telepítése |
|--------|-----------|-------------------------|------------- |-------------|
| 7.      | KB3121899 | A belső vezérlőprogram lemez              |  Karbantartási <br></br>Zavaró      | ~ 30 perc |
 
<br></br>

> [AZURE.IMPORTANT]
>
> - Ez az eljárás szükséges 2.2 frissítés alkalmazása csak egyszer kell elvégezni. Az Azure klasszikus portál alkalmazni a későbbi frissítések is használhatja.
> - Ha frissíti a frissítés 2, az összes telepítés ideje közelébe 1,5 óra.
> - Ezzel az eljárással a frissítés előtt győződjön meg arról, hogy a mind az eszköz vezérlők online állapotban, és a hardver-összetevő is megfelelő.

A következő lépésekkel töltheti le és telepítse a gyorsjavításokat.

[AZURE.INCLUDE [storsimple-install-update21-hotfix](../../includes/storsimple-install-update21-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Következő lépések

További tudnivalók a [frissítés 2.1-es kiadás](storsimple-update21-release-notes.md).
