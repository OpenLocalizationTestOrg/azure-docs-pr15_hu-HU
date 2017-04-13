<properties
   pageTitle="Telepítse a frissítés 3 StorSimple eszközön |} Microsoft Azure"
   description="Megtudhatja, hogyan telepítheti StorSimple 8000 sorozat frissítés 3 StorSimple 8000 sorozat eszközén."
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
   ms.date="10/05/2016"
   ms.author="alkohli" />

# <a name="install-update-3-on-your-storsimple-device"></a>Telepítse a frissítés 3 StorSimple mobileszközön

## <a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogyan frissítés 3 telepítése egy korábbi verzióját szoftver az Azure klasszikus portálon keresztül, és a gyorsjavítás módszerrel StorSimple eszközre. A gyorsjavítás módszer használható az átjárók be van állítva az StorSimple eszköz adatainak 0-től különböző hálózati kapcsolaton és 1 szoftver frissítés előtti verziójáról frissíteni próbálja.

Frissítés 3 eszköz szoftver, a LSI illesztőprogram és a belső vezérlőprogram, Storport tartalmazza, és Spaceport frissíti. Ha frissíti a frissítés 2 vagy korábbi verziójában, is fogja alkalmazni iSCSI WMI, és bizonyos esetekben a lemez belső vezérlőprogram frissítések szükség. Az eszköz szoftver, a WMI, iSCSI, LSI illesztőprogram, Spaceport és Storport javítások nem zavaró frissítéseket, és az Azure klasszikus portálon keresztül alkalmazhatók. A lemez belső vezérlőprogram frissítéseket zavaró frissítéseket, és csak az eszközön a Windows PowerShell felületén keresztül kell alkalmazni. 

> [AZURE.IMPORTANT]

> - Kézzel és automatikusan előtti ellenőrzések csoportja a hardver állapot és a hálózati kapcsolat pedig az eszköz állapotáról a telepítés előtt történik. Ezek előtti engedélyezné csak akkor, ha a frissítések telepítését az Azure klasszikus portálról.
> - Azt javasoljuk, hogy telepítse a szoftverek és -illesztőprogram frissítések az Azure klasszikus portálon keresztül. A Windows PowerShell-felület (a frissítések telepítése) az eszköz csak meg kell megnyitásához, ha a frissítés előtti átjáró jelölőnégyzet nem sikerül a portálon. Függően a frissíteni kívánt verzió, a frissítések órát is igénybe vehet 1.5-2,5 telepítéséhez. A karbantartás mód frissítések telepíteni kell az eszközön a Windows PowerShell felületén keresztül. Mivel karbantartási mód frissítések zavaró frissítések, ezek az eszköz lefelé időpontját eredményezi.
> - Ha a választható StorSimple pillanatkép kezelő fut, győződjön meg arról, hogy frissített pillanatkép Manager verziójától a frissítés 2 az eszköz frissítése előtt.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-3-via-the-azure-classic-portal"></a>Frissítés 3 telepítse az Azure klasszikus portálon keresztül

Hajtsa végre az alábbi lépéseket kell az eszköz [Frissítése](storsimple-update3-release-notes.md)3.


> [AZURE.NOTE]
Ha meg vannak alkalmazása a frissítés 2 vagy újabb (beleértve a frissítés 2.1), Microsoft tudják csoportosítani az eszközről a diagnosztikai tájékozódhat. Eredményt adja Ha a tevékenységek csoportunk azonosítja problémák vannak eszközök, vagyunk jobban ellátni a információk összegyűjtése az eszközről és problémáinak diagnosztizálása. Frissítés 2-es vagy újabb elfogadásával engedélyezi a velünk a megelőző támogatása érdekében.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Győződjön meg arról, hogy eszköze problémamentesen működik **StorSimple 8000 sorozat frissítés 3 (6.3.9600.17759)**. A **Legutóbbi frissítés dátum** módosítani kell a is. 

    Ha frissítés 2 előtt verzióról frissít, akkor is látható, hogy a karbantartás mód frissítések is elérhetők (Ez az üzenet előfordulhat, hogy továbbra is megjelennek a frissítések telepítése után akár 24 órát).

    Karbantartási mód frissítései eszköz legrövidebb leállás eredményez, és csak az eszközön a Windows PowerShell felületén keresztül vonatkozni zavaró frissítéseket. Bizonyos esetekben, amikor frissítés 1.2-es operációs rendszert futtató a lemez belső vezérlőprogram már valószínűleg naprakész, ebben az esetben nem kell minden karbantartási mód frissítések telepítése.

    Ha frissítés 2 frissítése vagy újabb verzió, az eszköz most már naprakész. Kihagyhatja a hátralévő lépéseket.

13. Töltse le a karbantartás mód frissítések kereséséhez és letöltéséhez KB3121899, mely telepítések lemez belső vezérlőprogram frissítések (a frissítések kell telepítve kell lennie, most) [letöltéséhez gyorsjavítások](#to-download-hotfixes) szereplő lépéseket használatával.

13. Kövesse a lépéseket az [telepítése, és ellenőrizze a karbantartás mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes) a karbantartás mód frissítések telepítéséhez. 

  

## <a name="install-update-3-as-a-hotfix"></a>Telepítse a frissítés 3 gyorsjavításként

Akkor használja ezt az eljárást, ha a meg az Azure klasszikus portálon keresztül a frissítések telepítése nem sikerül az átjáró jelölőnégyzetet. A jelölőnégyzet nem sikerül az átjáró nem adatok 0 hálózati kártya rendelt van, és az eszköze problémamentesen működik 1 frissítés előtt a szoftver verziószáma.

A szoftver a gyorsjavítás módszerrel frissíthető változatokat:

- Frissítse a 0,1 értéket, 0,2, 0,3
- 1, 1.1-es, 1.2-es frissítése
- 2, 2.1-es vagy 2.2 frissítése 

> [AZURE.IMPORTANT]
>
> - Ha az eszköz megjelenés kiadás verziója fut, lépjen kapcsolatba a segítséget nyújtanak a frissítés a [Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) .

A gyorsjavítás módszer a következő három lépésből:

1.  Töltse le a Microsoft Update katalógus gyorsjavítások.

2.  Telepítse, és ellenőrizze a normál mód gyorsjavításokat.

3.  Telepítse, és ellenőrizze a karbantartás mód javítás, (csak frissítésekor a frissítés előtti 2 szoftver).


#### <a name="download-updates-for-your-device"></a>Töltse le a frissítések az eszközön

**Ha eszköze problémamentesen működik, frissítés 2.1-es vagy 2.2**, töltse le és telepítse a következő gyorsjavításokat a előírt sorrendben:

| Sorrend  | KB        | Leírás                    | Frissítés típusa  | Idő telepítése |
|--------|-----------|-------------------------|------------- |-------------|
| 1.      | KB3186843 | #42; & szoftver frissítése  |  Normál <br></br>Nem zavaró     | ~ 45 perc |
| 2.      | KB3186859 | LSI illesztőprogram és a belső vezérlőprogram             |  Normál <br></br>Nem zavaró      | ~ 20 perc |
| 3.      | KB3121261 | Storport és Spaceport javítása </br> A Windows Server 2012 R2 |  Normál <br></br>Nem zavaró      | ~ 20 perc |

& #42;  Két bináris fájl *megjegyzést, szoftver frissítés áll: eszköz szoftver frissítése előtt a létrehozás `all-hcsmdssoftwareupdate` és a tartalmazni a Cis és a minimális letöltési stratégia agent `all-cismdsagentupdatebundle`. A Cis és a minimális letöltési stratégia agent előtt telepíteni kell az eszköz frissítését. Az aktív vezérlő útján is újra kell indítani az `Restart-HcsController` parancsmagot a Cis és a minimális letöltési stratégia agent alkalmazása után frissítése (és frissíti a fennmaradó alkalmazása előtt).* 


**Ha eszköze problémamentesen működik a 0,1 értéket, 0,2, 0,3, 1.0, 1.1-es, 1.2-es, vagy 2.0-s frissítés**, kell letölti és telepíti az alábbi gyorsjavításokat mellett a szoftver, a LSI illesztőprogram és a belső vezérlőprogram frissítések (lásd a fenti táblázatban), a előírt sorrendben:

| Sorrend  | KB        | Leírás                    | Frissítés típusa  | Idő telepítése |
|--------|-----------|-------------------------|------------- |-------------|
| 4.      | KB3146621 | iSCSI csomag | Normál <br></br>Nem zavaró  | ~ 20 perc |
| 5.      | KB3103616 | WMI csomag |  Normál <br></br>Nem zavaró      | ~ 12 perc |


<br></br>

**Ha eszköze problémamentesen működik 0,2, 0,3, 1.0, 1.1-es és 1.2-es verzió**, is szükség lehet lemez belső vezérlőprogram frissítések felett látható az előző táblák minden frissítések telepítéséhez. Ellenőrizheti, hogy kell-e a lemez belső vezérlőprogram frissítések futtatásával a `Get-HcsFirmwareVersion` parancsmag. Ha ezek a belső vezérlőprogram verziók futtatja: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, majd a frissítések telepítése nem szükséges.


| Sorrend  | KB        | Leírás                    | Frissítés típusa  | Idő telepítése |
|--------|-----------|-------------------------|------------- |-------------|
| 6.      | KB3121899 | A belső vezérlőprogram lemez              |  Karbantartási <br></br>Zavaró      | ~ 30 perc |
 
<br></br>

> [AZURE.IMPORTANT]
>
> - Ez az eljárás szükséges 3 frissítés alkalmazása csak egyszer kell elvégezni. Az Azure klasszikus portál alkalmazni a későbbi frissítések is használhatja.
> - Ha frissíti a frissítés 2.2, az összes telepítés ideje közelébe 1.1-es óra.
> - Ezzel az eljárással a frissítés előtt győződjön meg arról, hogy a mind az eszköz vezérlők online állapotban, és a hardver-összetevő is megfelelő.

A következő lépésekkel töltheti le és telepítse a gyorsjavításokat.

[AZURE.INCLUDE [storsimple-install-update3-hotfix](../../includes/storsimple-install-update3-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Következő lépések

További tudnivalók a [frissítés 3 megjelenési](storsimple-update3-release-notes.md).
