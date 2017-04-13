<properties
   pageTitle="Telepítse a frissítés 2 StorSimple eszközön |} Microsoft Azure"
   description="Megtudhatja, hogyan telepítheti StorSimple 8000 sorozat frissítés 2 StorSimple 8000 sorozat eszközén."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="install-update-2-on-your-storsimple-device"></a>Telepítse a frissítés 2 StorSimple mobileszközön

## <a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogyan frissítés 2 telepítése egy korábbi verzióját szoftver az Azure klasszikus portálon keresztül StorSimple eszközre. Az oktatóprogram is bemutatja az átjáró nem adat 0 az StorSimple eszköz hálózati kapcsolaton van beállítva, és 1 szoftver frissítés előtti verziójáról frissíteni próbálja a frissítés szükséges lépéseket.

2 frissítés eszköz szoftverfrissítések, a LSI illesztőprogram frissítéseket és a lemez belső vezérlőprogram frissítések tartalmazza. Az eszköz szoftver LSI frissítések nem zavaró frissítések és alkalmazhatók az Azure klasszikus portálon keresztül. A lemez belső vezérlőprogram frissítéseket zavaró frissítéseket, és csak az eszközön a Windows PowerShell felületén keresztül kell alkalmazni.

> [AZURE.IMPORTANT]

> -  Frissítés 2 nem láthatók azonnal, mert a frissítéseket a szakaszos bevezetésének mi. Néhány nap ismét a frissítések ellenőrzése mint a frissítés lesz elérhető hamarosan.
> - Kézzel és automatikusan előtti ellenőrzések csoportja a hardver állapot és a hálózati kapcsolat pedig az eszköz állapotáról a telepítés előtt történik. Ezek előtti engedélyezné csak akkor, ha a frissítések telepítését az Azure klasszikus portálról.
> - Azt javasoljuk, hogy telepítse a szoftverek és -illesztőprogram frissítések az Azure klasszikus portálon keresztül. A Windows PowerShell-felület (a frissítések telepítése) az eszköz csak meg kell megnyitásához, ha a frissítés előtti átjáró jelölőnégyzet nem sikerül a portálon. A frissítések órát is igénybe vehet 4-7 (beleértve a Windows-frissítések) telepítéséhez. A karbantartás mód frissítések telepíteni kell az eszközön a Windows PowerShell felületén keresztül. Mivel karbantartási mód frissítések zavaró frissítések, ezek az eszköz lefelé időpontját eredményezi.
> - Ha a választható StorSimple pillanatkép kezelő fut, győződjön meg arról, hogy frissített pillanatkép Manager verziójától a frissítés 2 az eszköz frissítése előtt.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-2-via-the-azure-classic-portal"></a>Frissítés 2 telepítse az Azure klasszikus portálon keresztül

A következő lépésekkel frissítse az eszköz [frissítése 2](storsimple-update2-release-notes.md).


> [AZURE.NOTE]
2 frissítés lehetővé teszi a Microsoft arra, hogy lekérje az eszközről a diagnosztikai tájékozódhat. Eredményt adja Ha a tevékenységek csoportunk azonosítja problémák vannak eszközök, vagyunk jobban ellátni a információk összegyűjtése az eszközről és problémáinak diagnosztizálása. Frissítés 2 elfogadásával engedélyezi a velünk a megelőző támogatása érdekében.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Győződjön meg arról, hogy eszköze problémamentesen működik **StorSimple 8000 sorozat frissítés 2 (6.3.9600.17673)**. A **Legutóbbi frissítés dátum** módosítani kell a is. Is látni fogja, hogy elérhetők-e a karbantartási mód frissítéseit (Ez az üzenet előfordulhat, hogy továbbra is megjelennek a frissítések telepítése után akár 24 órát).

    Karbantartási mód frissítései eszköz legrövidebb leállás eredményez, és csak az eszközön a Windows PowerShell felületén keresztül vonatkozni zavaró frissítéseket. Bizonyos esetekben, amikor frissítés 1.2-es operációs rendszert futtató a lemez belső vezérlőprogram már valószínűleg naprakész, ebben az esetben nem kell minden karbantartási mód frissítések telepítése.

13. Töltse le a karbantartás mód frissítések kereséséhez és letöltéséhez KB3121899, mely telepítések lemez belső vezérlőprogram frissítések (a frissítések kell telepítve kell lennie, most) [letöltéséhez gyorsjavítások](#to-download-hotfixes) szereplő lépéseket használatával.

13. Kövesse a lépéseket az [telepítése, és ellenőrizze a karbantartás mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes) a karbantartás mód frissítések telepítéséhez.


## <a name="install-update-2-as-a-hotfix"></a>Telepítse a frissítés 2 gyorsjavításként

Akkor használja ezt az eljárást, ha a meg az Azure klasszikus portálon keresztül a frissítések telepítése nem sikerül az átjáró jelölőnégyzetet. A jelölőnégyzet nem sikerül az átjáró nem adatok 0 hálózati kártya rendelt van, és az eszköze problémamentesen működik 1 frissítés előtt a szoftver verziószáma.

A szoftver a gyorsjavítás módszerrel frissíthető változatokat frissítés 0,1 értéket, 0,2, frissítése és frissítés 0,3, frissítés 1, frissítés 1.1-es és frissítés 1.2-es. A gyorsjavítás módszer a következő három lépésből:

- Töltse le a Microsoft Update katalógus gyorsjavítások.
- Telepítse, és ellenőrizze a normál mód gyorsjavításokat.
- Telepítse, és ellenőrizze a karbantartás mód javítás.

Telepítse a frissítés 2 gyorsjavításként, töltse le és telepítse a következő gyorsjavításokat szükséges:

| Sorrend  | KB        | Leírás                    | Frissítés típusa  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3121901 | Frissítés         |  Normál     |
| 2      | KB3121900 | LSI illesztőprogram              |  Normál     |
| 3      | KB3080728 | Storport kijavítása </br> A Windows Server 2012 R2 |  Normál     |
| 4      | KB3090322 | Spaceport kijavítása </br> A Windows Server 2012 R2 |  Normál     |
| 5      | KB3121899 | A belső vezérlőprogram lemez           | Karbantartási  |


> [AZURE.IMPORTANT]
>
> - Ha az eszköz megjelenés kiadás verziója fut, lépjen kapcsolatba a segítséget nyújtanak a frissítés a [Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) .
> - Ez az eljárás szükséges alkalmazása a frissítés 2 csak egyszer kell elvégezni. Az Azure klasszikus portál alkalmazni a későbbi frissítések is használhatja.
> - Minden egyes gyorsjavításainak telepítési készíthet körülbelül 20 percet befejezéséhez. Teljes telepítés ideje közelébe 2 óra.
> - Ezzel az eljárással a frissítés előtt győződjön meg arról, hogy mindkét eszköz vezérlők online állapotban, és a hardver-összetevő is megfelelő.

Hajtsa végre az alábbi lépéseket a frissítés gyorsjavításként.

[AZURE.INCLUDE [storsimple-install-update2-hotfix](../../includes/storsimple-install-update2-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]



## <a name="next-steps"></a>Következő lépések

További tudnivalók a [frissítés 2 megjelenés](storsimple-update2-release-notes.md).
