<properties 
   pageTitle="Módosíthatja a StorSimple eszköz módot |} Microsoft Azure"
   description="A StorSimple eszköz üzemmód és használata a Windows PowerShell-StorSimple eszköz módjának megváltoztatása ismerteti."
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
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-device-mode-on-your-storsimple-device"></a>StorSimple eszközén eszköz módjának megváltoztatása

Ebben a cikkben egy rövid leírást a különböző módok, amelyben az StorSimple eszköz is működik. Az StorSimple eszköz működhet három üzemmódban: normál, karbantartási és helyreállítási. 

Ez a cikk elolvasása, után, tudni fogja, hogy:

- Mi a StorSimple eszköz módok
- Hogyan találnia, hogy melyik mód az StorSimple eszköz van
- A normál karbantartási mód, és *Ez fordítva is igaz* módosítása


A fenti felügyeleti feladatok csak az StorSimple eszközön a Windows PowerShell felületén keresztül hajtható végre.

## <a name="about-storsimple-device-modes"></a>Tudnivalók a StorSimple eszköz módok

Az StorSimple eszköz normál, karbantartási vagy helyreállítási módban is működik. Minden, a következő üzemmódok rövid leírása alább.

### <a name="normal-mode"></a>Normál mód

Ez a definíciója mód a szokásos működési egy teljesen konfigurált StorSimple eszközhöz. Alapértelmezés szerint az eszköz normál módban kell lennie.

### <a name="maintenance-mode"></a>Karbantartási mód

A StorSimple eszköz időnként szükség lehet karbantartási módba helyét. Ezt a módot lehetővé teszi, hogy az eszközön karbantartásához és zavaró, például a lemez belső vezérlőprogram kapcsolódó frissítések telepítése.

Az StorSimple elhelyezheti a rendszer csak a Windows PowerShell keresztül karbantartási módba. Az összes I/O kérések ebben az üzemmódban fel van függesztve. Szolgáltatások, például nem ideiglenes elérésű memória (NVRAM) vagy a fürtszolgáltatásnak is leállt. Mind a vezérlők újraindítja beírásakor, vagy lépjen ki ezt a módot. A karbantartási módban való kilépéskor a szolgáltatások folytatódik, és a megfelelő legyen. Ez a néhány percig is eltarthat.

>[AZURE.NOTE]**Karbantartási mód csak támogatott megfelelően működik készüléken. Nem támogatott egy eszközön, amelyen egyikének vagy mindkettőnek a vezérlők nem működnek.**
</br>

### <a name="recovery-mode"></a>Helyreállítási mód

Helyreállítási módot is alakban a "Windows hálózati támogatás csökkentett mód". Helyreállítási mód a Microsoft Support csapatának folytat, és lehetővé teszi őket a rendszer diagnosztika végrehajtásához. Az elsődleges helyreállítási mód célja beolvasni a rendszer naplókat.

Ha a rendszer helyreállítási állapotba kerül, lépjen kapcsolatba a Microsoft Support a következő lépéseket. További információért lépjen [Kapcsolatfelvétel a Microsoft támogatási szolgálattal](storsimple-contact-microsoft-support.md).

>[AZURE.NOTE] **Az eszköz nem helyezhetők el helyreállítási módban. Ha az eszköz rossz állapotban van, próbálja helyreállítási mód első az eszközt, amelyben Microsoft Support személyzeti is megvizsgálni azt állapotba.**

## <a name="determine-storsimple-device-mode"></a>StorSimple eszköz mód meghatározása

#### <a name="to-determine-the-current-device-mode"></a>Az aktuális eszköz üzemmódot meghatározása

1. Jelentkezzen be a soros eszköz-konzol [Használata gitt való csatlakozáshoz a soros eszköz-konzol](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)utasításait követve.

2. Nézze meg az eszköz a soros konzol menü transzparens üzenetére. Ez az üzenet kifejezetten azt jelzi, hogy az eszköz karbantartási és helyreállítási módban. Ha az üzenet nem tartalmaz a rendszer mód kapcsolatos információk, az eszköz normál üzemmódban van.

## <a name="change-the-storsimple-device-mode"></a>StorSimple eszköz módjának megváltoztatása 

Az StorSimple eszköz be karbantartási üzemmódról (normál) is elhelyezhet a karbantartás végrehajtásához, illetve karbantartási mód frissítések telepítése. Hajtsa végre az alábbi műveletek megadása vagy kilépés a karbantartási módban.

> [AZURE.IMPORTANT] Karbantartási üzemmódba előtt győződjön meg róla, hogy mindkét eszköz vezérlők megfelelő elérésével **Hardveres állapotát** a **Karbantartás** lap az Azure klasszikus portálon. Ha a vezérlők egyik vagy mindkét nem megfelelő, forduljon a Microsoft Support a következő lépésekkel. További információért lépjen [Kapcsolatfelvétel a Microsoft támogatási szolgálattal](storsimple-contact-microsoft-support.md).

#### <a name="to-enter-maintenance-mode"></a>Karbantartási mód megadása

1. Jelentkezzen be a soros eszköz-konzol [Használata gitt való csatlakozáshoz a soros eszköz-konzol](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)utasításait követve.

2. A soros konzol menüben válassza az 1, **Jelentkezzen be a teljes hozzáférést biztosít**. Amikor a rendszer kéri, adja meg az **eszközt rendszergazdai jelszót**. Az alapértelmezett jelszava: `Password1`.

3. A parancssorba írja be a 

    `Enter-HcsMaintenanceMode`

4. Láthatja, hogy egy figyelmeztető üzenet jelzi, hogy karbantartási mód összes I/O kérések zavarhatják, és azt a kapcsolatot az Azure klasszikus portal Server alkalmazás, és megerősítését kéri. Írja be az **Y** karbantartási módba.

5. Mindkét vezérlők újraindul. Az újraindítás befejeződése után egy másik üzenet jelenik meg, ezzel jelezve, hogy az eszköz karbantartási módban.


#### <a name="to-exit-maintenance-mode"></a>Kilép a karbantartási mód

1. Jelentkezzen be a soros eszköz-konzolba. Győződjön meg arról, hogy az eszköz karbantartási üzemmódban van transzparens üzenetből.

2. A parancssorba írja be:

    `Exit-HcsMaintenanceMode`

3. Figyelmeztető üzenet jelenik meg, és a megerősítést kérő üzenet jelenik meg. Írja be az **Y** karbantartási módból való kilépéshez.

4. Mindkét vezérlők újraindul. Ha az újraindítás kész, egy másik üzenet jelenik meg, ezzel jelezve, hogy az eszköz a szokásos módon.


## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan [normál és karbantartás mód frissítéseinek](storsimple-update-device.md) StorSimple eszközén.

