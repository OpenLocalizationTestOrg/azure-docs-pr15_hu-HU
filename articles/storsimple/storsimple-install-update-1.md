<properties
   pageTitle="Telepítse a frissítés 1.2-es StorSimple eszközén |} Microsoft Azure"
   description="Megtudhatja, hogyan telepítheti StorSimple 8000 sorozat frissítés 1.2-es StorSimple 8000 sorozat eszközén."
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
   ms.date="08/22/2016"
   ms.author="alkohli" />

# <a name="install-update-12-on-your-storsimple-device"></a>Telepítse a frissítés 1.2-es StorSimple mobileszközön

## <a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogyan 1.2-es frissítés telepítése előtt frissítés 1 szoftver változatában StorSimple eszközt. Az oktatóprogram is bemutatja a további szükséges lépésekről frissítése, ha az átjárók eltérő adat 0 az StorSimple eszköz hálózati kapcsolaton van beállítva.

Frissítés 1.2-es eszköz szoftverfrissítések, a LSI illesztőprogram frissítéseket és a lemez belső vezérlőprogram frissítések tartalmazza. A szoftver LSI illesztőprogram frissítések nem zavaró frissítések és alkalmazhatók az Azure klasszikus portálon keresztül. A lemez belső vezérlőprogram frissítéseket zavaró frissítéseket, és csak az eszközön a Windows PowerShell felületén keresztül kell alkalmazni.

Attól függően, hogy melyik verziót az eszköz futása, eldöntheti, ha a frissítés 1.2-es fog vonatkozni. Az eszköz **Irányítópult** **fontos** szakaszához navigálással érdemes az eszköz a szoftver verziószáma.

</br>

| Ha a szoftvert futtat...   | Mi történik a portálon?                              |
|---------------------------------|--------------------------------------------------------------|
| A Megjelenés – ám                    | Ha verzióját (kiadás) rendszerben nem vonatkoznak a frissítés. Adja meg, [lépjen kapcsolatba a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) frissítse az eszközt.|
| Frissítse a 0,1                      | Portál frissítés 1.2-es vonatkozik.                                |
| 0,2 frissítése                      | Portál frissítés 1.2-es vonatkozik.                                |
| 0,3 frissítése                      | Portál frissítés 1.2-es vonatkozik.                                |
| 1 frissítése                        | A frissítés nem lesz elérhető.                           |
| 1.1-es frissítés                      | A frissítés nem lesz elérhető.                           |

</br>

> [AZURE.IMPORTANT]

> -  Frissítés 1.2-es nem láthatók azonnal, mert a frissítéseket a szakaszos bevezetésének mi. Néhány nap ismét a frissítések ellenőrzése mint a frissítés lesz elérhető hamarosan.
> - A frissítés tartalmaz egy sor olyan kézzel és automatikusan előtti ellenőrzi, hogy az eszköz állapotáról értelmez hardver állapot és a hálózati kapcsolat. Ezek előtti engedélyezné csak akkor, ha a frissítések telepítését az Azure klasszikus portálról.
> - Azt javasoljuk, hogy telepítse a szoftverek és -illesztőprogram frissítések az Azure klasszikus portálon keresztül. A Windows PowerShell-felület (a frissítések telepítése) az eszköz csak meg kell megnyitásához, ha a frissítés előtti átjáró jelölőnégyzet nem sikerül a portálon. A frissítések órát is igénybe vehet 5-10 (beleértve a Windows-frissítések) telepítéséhez. A karbantartás mód frissítések telepíteni kell az eszközön a Windows PowerShell felületén keresztül. Mivel karbantartási mód frissítések zavaró frissítések, ezek az eszköz lefelé időpontját eredményezi.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-the-azure-classic-portal"></a>Frissítés 1.2-es telepítse az Azure klasszikus portálon keresztül

Hajtsa végre az alábbi lépéseket kell az eszköz [Frissítése](storsimple-update1-release-notes.md)1.2-es verziójára. Az alábbi eljárással csak akkor, ha az átjárók adatok 0 hálózati kapcsolaton az eszközön konfigurálva van.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Győződjön meg arról, hogy eszköze problémamentesen működik **StorSimple 8000 sorozat frissítés 1.2 (6.3.9600.17584)**. A **Legutóbbi frissítés dátum** módosítani kell a is. Is látni fogja, hogy elérhetők-e a karbantartási mód frissítéseit (Ez az üzenet előfordulhat, hogy továbbra is megjelennek a frissítések telepítése után akár 24 órát).

    Karbantartási mód frissítései eszköz legrövidebb leállás eredményez, és csak az eszközön a Windows PowerShell felületén keresztül vonatkozni zavaró frissítéseket.

    ![Karbantartása lap] (./media/storsimple-install-update-1/InstallUpdate12_10M.png "Karbantartása lap")

13. Töltse le a karbantartás mód frissítések kereséséhez és letöltéséhez KB3063416, mely telepítések lemez belső vezérlőprogram frissítések (a frissítések kell telepítve kell lennie, most) [letöltéséhez gyorsjavítások]( #to-download-hotfixes) szereplő lépéseket használatával.

13. Kövesse a lépéseket az [telepítése, és ellenőrizze a karbantartás mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes) a karbantartás mód frissítések telepítéséhez.

14. Az Azure klasszikus portálon nyissa meg a **Karbantartás** lapot, és a lap alján, kattintson a **Beolvasás frissítések** bármely Windows-frissítések keresése hivatkozásra, és kattintson a **Frissítések telepítése**gombra. Ezzel elkészült a frissítéseket a sikeres telepítése után.



## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>Telepítse a frissítés 1.2-es egy eszközön, amelyen az átjáró nem adatok 0 hálózati kártya be van állítva

Ez az eljárás csak akkor, ha meg az Azure klasszikus portálon keresztül a frissítések telepítése nem sikerül az átjáró ellenőrzés kell használni. A jelölőnégyzet nem sikerül az átjáró nem adatok 0 hálózati kártya rendelt van, és az eszköze problémamentesen működik 1 frissítés előtt a szoftver verziószáma. Ha az eszköz nem rendelkezik az átjáró nem adatok 0 hálózati kapcsolaton, frissítheti az eszköz közvetlenül az Azure klasszikus portálról. Lásd: a [telepítés frissítése az 1.2-es az Azure klasszikus portálon keresztül](#install-update-1.2-via-the-azure-classic-portal).

A szoftver ezzel a módszerrel frissíthető változatokat Update 0,1 értéket, a frissítés 0,2 és a frissítés 0,3.


> [AZURE.IMPORTANT]
>
> - Ha az eszköz megjelenés kiadás verziója fut, lépjen kapcsolatba a segítséget nyújtanak a frissítés a [Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) .
> - Ezt az eljárást csak egyszer kell elvégezni alkalmazása a frissítés 1.2-es szükséges. Az Azure klasszikus portál alkalmazni a későbbi frissítések is használhatja.

Ha eszköze problémamentesen működik a frissítés előtti 1 szoftver, és az átjáró beállítása a hálózati illesztő eltérő adatok 0, az alábbi két módon alkalmazhat frissítés 1.2-es:

- **A beállítás 1**: a frissítés letöltéséhez és alkalmazza a a `Start-HcsHotfix` az eszköz a Windows PowerShell felületről parancsmag. Ez a javasolt. **Ne használja ezt a módszert alkalmazása a frissítés 1.2-es, ha eszköze problémamentesen működik, frissítés 1.0 vagy a frissítés 1.1-es.**

- **A beállítás 2**: távolítsa el az átjáró konfiguráció, és telepítse a frissítést közvetlenül az Azure klasszikus portálról.


Az alábbiak mindegyike vonatkozó részletes útmutatást a következő szakaszban találhatók.

## <a name="option-1-use-windows-powershell-for-storsimple-to-apply-update-12-as-a-hotfix"></a>1 a beállítás: A Windows PowerShell StorSimple gyorsjavításként frissítés 1.2-es alkalmazni az

Ez az eljárás csak akkor, ha futtatja a 0,1 értéket, 0,2, a 0,3 és a Ha az átjáró jelölőnégyzet meg az Azure klasszikus portálról frissítések telepítése nem sikerült frissítés kell használni. Megjelenés kiadás szoftvert futtat, tájékoztassa a [Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) frissítse az eszközt.

Telepítse a frissítés 1.2-es gyorsjavításként, töltse le, és telepítse a következő gyorsjavításokat:

| Sorrend  | KB        | Leírás             | Frissítés típusa  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3063418 | Frissítés         |  Normál     |
| 2      | KB3043005 | LSI Társítások vezérlő frissítése |  Normál     |
| 3      | KB3063416 | A belső vezérlőprogram lemez           | Karbantartási  |

Ezzel az eljárással a frissítés előtt győződjön meg arról, hogy:

- Mindkét eszköz vezérlők online állapotban.

A következő lépésekkel alkalmazása a frissítés 1.2-es. **A frissítések óráig is eltarthat körülbelül 2 (körülbelül 30 perc szoftverek,-illesztőprogram 30 percig lemez belső vezérlőprogram esetén 45 perc) befejezéséhez.**

[AZURE.INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]


## <a name="option-2-use-the-azure-classic-portal-to-apply-update-12-after-removing-the-gateway-configuration"></a>2 lehetőség: Alkalmazása a frissítés 1.2-es az átjáró konfiguráció eltávolítása után az Azure klasszikus portál használatával

Ez az eljárás csak a szoftver 1 frissítés előtti verzióját futtatja, és az átjáró beállítása adatok 0-től különböző hálózati kapcsolaton van StorSimple eszközök vonatkozik. Meg kell törölje a jelet az átjáró beállítása a frissítés alkalmazása előtt.

A frissítés órát is igénybe vehet néhány befejezéséhez. A hosts különböző alhálózathoz is, ha az átjáró konfigurációjának az iSCSI felület eltávolítása vezethet legrövidebb leállás. Azt javasoljuk, hogy úgy beállítani, hogy adat 0 iSCSI forgalom az állásidőt csökkentése érdekében.

Hajtsa végre az alábbi lépésekkel tiltsa le az átjáró hálózati kapcsolaton és követően telepítse a frissítést.

[AZURE.INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]


## <a name="next-steps"></a>Következő lépések

További tudnivalók a [frissítés 1.2-es kiadás](storsimple-update1-release-notes.md).
