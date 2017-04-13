<properties
   pageTitle="Frissítse az StorSimple eszköz |} Microsoft Azure"
   description="Megtudhatja, hogyan telepítheti az StorSimple update szolgáltatás rendszeres és karbantartás mód frissítések és gyorsjavítások."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="06/28/2016"
   ms.author="v-sharos" />

# <a name="update-your-storsimple-8000-series-device"></a>A StorSimple 8000 sorozat eszköz frissítése

## <a name="overview"></a>– Áttekintés

A StorSimple frissítések szolgáltatások lehetőséget biztosítanak naprakészen egyszerűen StorSimple eszközére. Attól függően, hogy a frissítés típusa frissítések is alkalmazhat, az eszközre, vagy a Windows PowerShell felületén keresztül az Azure klasszikus portálon keresztül. Ebben az oktatóanyagban a frissítés típusa és telepítéséhez mindegyiket ismerteti.

Kétféle típusú eszköz frissítése alkalmazhat: 

- Normál (vagy normál mód) frissítések
- Karbantartási mód frissítések

Az Azure klasszikus portálon vagy a Windows PowerShell; rendszeres frissítések telepítése a Windows PowerShell a karbantartás mód frissítések telepítése kell használnia. 

Minden egyes frissítés típusa: külön-külön alább.

### <a name="regular-updates"></a>Normál frissítések

Normál frissítései nem zavaró frissítések, ha az eszköz a szokásos módon telepíthető. A frissítések alkalmazza a program a Microsoft Update webhelyen keresztül minden eszköz vezérlő. 

> [AZURE.IMPORTANT] A vezérlő feladatátvétel fordulhat elő, a frissítés során. Azonban ez nem érinti rendszer elérhetősége vagy a műveletet.

- Olvashat az Azure klasszikus portálon keresztül normál frissítések telepítéséhez, akkor olvassa el a [normál frissítések telepítése a Azure klasszikus portal(#install-regular-updates-via-the-azure-classic-portal) keresztül.

- Normál frissítések a Windows PowerShell-StorSimple is telepítheti. Részletekért olvassa el [a Windows PowerShell-StorSimple keresztül rendszeres frissítések telepítése](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Karbantartási mód frissítések

Karbantartási mód frissítései zavaró frissítések, például a lemez belső vezérlőprogram frissítések. A frissítések elhelyezendő karbantartási mód az eszköz szükséges. A részletekért olvassa [lépés 2: Adja meg a karbantartási mód](#step2). Az Azure klasszikus portál karbantartási mód frissítések telepítése nem használható. Ehelyett StorSimple a Windows PowerShell kell használnia. 

Karbantartási mód frissítések telepítési részletekért olvassa el [telepítése karbantartási mód frissítések a Windows PowerShell-StorSimple keresztül](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [AZURE.IMPORTANT] Karbantartási mód frissítések minden vezérlő külön-külön kell alkalmazni. 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Az Azure klasszikus portálon keresztül normál frissítések telepítése

Az Azure klasszikus portal segítségével StorSimple eszköze-frissítések telepítése.

[AZURE.INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>StorSimple rendszeres frissítések a Windows PowerShell telepítése

Azt is megteheti szeretné alkalmazni a szokásos (normál mód) frissítéseit a Windows PowerShell-StorSimple is használhatja.

> [AZURE.IMPORTANT] Bár a Windows PowerShell használatának StorSimple rendszeres frissítések telepítéséhez azt ajánljuk, hogy az Azure klasszikus portálon keresztül normál frissítések telepítését. Frissítés 1 kezdve előtti szempontú vizsgálatok a portálról-frissítések telepítése előtt történik. Az előzetes ellenőrzések hibák preempt, és a a egyenletesebb környezet biztosítása. 

[AZURE.INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>StorSimple karbantartási mód frissítések a Windows PowerShell telepítése

A Windows PowerShell-StorSimple segítségével karbantartási mód frissítéseinek StorSimple eszközére. Az összes I/O kérések ebben az üzemmódban fel van függesztve. Szolgáltatások, például nem ideiglenes elérésű memória (NVRAM) vagy a fürtszolgáltatásnak is leállt. Adja meg, vagy lépjen ki ezt a módot, a újraindítása után az mindkét vezérlők. Való kilépéskor ezt a módot, a szolgáltatások folytatódik, és a megfelelő legyen. (Ez eltarthat néhány percig.)

Ha karbantartási mód frissítéseket kell, hogy rendelkezik-e telepítendő frissítések a Azure klasszikus portálon keresztül értesítés kap. Ez a figyelmeztetés tartalmazni fogja a frissítések telepítése a Windows PowerShell-StorSimple az utasításokat. Miután módosította az eszközön, ugyanezt az eljárást segítségével módosíthatja az eszköz normál módra. Lépésenkénti útmutatásért lásd: [lépés: 4: Kilépés karbantartási mód](#step4).

> [AZURE.IMPORTANT] 
> 
> - Karbantartási üzemmódba előtt győződjön meg róla, hogy mindkét eszköz vezérlők megfelelő, jelölje be a **Hardver állapota** a **Karbantartás** lap az Azure klasszikus portálon. Ha a vezérlő nem megfelelő, forduljon a Microsoft Support a következő lépésekkel. További információért lépjen kapcsolatfelvétel a Microsoft támogatási szolgálattal. 
> - Miután karbantartási módban, kell egy vezérlőn, majd kattintson a többi vezérlő először telepítse a frissítést.

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>Lépés: 1: Csatlakozás a soros konzol<a name="step1">

Első lépésként használhatja az alkalmazást, például a gitt a soros konzol eléréséhez. Az alábbi lépésekkel csatlakozhat gitt a soros konzol ismerteti.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Lépés: 2: Karbantartási módba<a name="step2">

A konzol kapcsolódás után határozza meg, hogy vannak-e a frissítések telepítése, és írja be a karbantartási mód legyenek telepítve.

[AZURE.INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>3 lépés: A frissítések telepítése<a name="step3">

Ezután telepítse a frissítést.

[AZURE.INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]
 
### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Lépés: 4: Kilépés karbantartási mód<a name="step4">

Végül lépjen ki a karbantartási mód.

[AZURE.INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>A Windows PowerShell gyorsjavítások StorSimple telepítése

Microsoft Azure StorSimple frissítései eltérően gyorsjavítások megosztott mappából vannak telepítve. Frissítések, a két típusa van gyorsjavítások: 

- Normál gyorsjavítások 
- Karbantartási mód gyorsjavítások  

Az alábbi eljárások bemutatják, hogy miként telepítheti Windows PowerShell-StorSimple rendszeres és a karbantartás mód telepített.

[AZURE.INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[AZURE.INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Mi történik frissítések, ha az eszköz gyári alaphelyzetbe állítása hajtja végre?

Ha az eszköz gyári beállításainak visszaállítása van, majd a frissítések elvesznek. A gyár-visszaállítása eszköz van regisztrálva és beállította, szüksége lesz StorSimple keresztül az Azure klasszikus portál és/vagy a Windows PowerShell frissítéseinek kézi telepítése. További információt a gyári alaphelyzetbe állítása [az eszköz gyári alapértelmezett beállításainak visszaállítása](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings)találhat.

## <a name="next-steps"></a>Következő lépések

- További tudnivalók [felügyelheti StorSimple eszköze StorSimple a Windows PowerShell használatával](storsimple-windows-powershell-administration.md).
- További tudnivalók [felügyelheti StorSimple eszköze az StorSimple kezelő szolgáltatás használatával](storsimple-manager-service-administration.md).
