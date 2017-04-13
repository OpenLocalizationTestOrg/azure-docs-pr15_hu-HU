<properties 
   pageTitle="A telepített StorSimple eszköz hibáinak elhárítása |} Microsoft Azure"
   description="Megtudhatja, hogy miként diagnosztizálása és hibák megoldása a jelenleg telepített és műveleti StorSimple eszközt."
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
   ms.date="05/16/2016"
   ms.author="v-sharos" />

# <a name="troubleshoot-an-operational-storsimple-device"></a>Egy műveleti StorSimple eszköz hibáinak elhárítása

## <a name="overview"></a>– Áttekintés

Ebben a cikkben hasznos hibaelhárítási útmutatót konfigurációs problémáinak megoldása, hogy miután StorSimple eszköze telepítve, és műveleti előforduló. Gyakori problémák, lehetséges okok és ajánlott lépéseket, segít, hogy a Microsoft Azure StorSimple futtatásakor esetleg tapasztalható problémák megoldásához azt mutatja be. Ezt az információt a StorSimple helyszíni fizikai eszközök és a StorSimple virtuális eszköz is érinti.

Ez a cikk a Microsoft Azure StorSimple művelet során előforduló hibakódok listáját megtalálhatja végén lépéseket és készíthet a hibák megoldásáról. 

## <a name="setup-wizard-process-for-operational-devices"></a>Műveleti eszközök esetén varázsló telepítést

A beállítási varázsló használata ([Invoke-HcsSetupWizard][1]) az eszköz konfigurációjának ellenőrzése és korrekciós lépéseket, ha szükséges.

A beállítási varázsló egy korábban beállított és műveleti eszközön futtatásakor ezt a folyamatot különbözik. Módosíthatja, hogy csak a következő bejegyzéseket:

- IP-cím, a alhálózat maszk és az átjáró
- Elsődleges DNS-kiszolgáló
- Elsődleges NTP-kiszolgáló
- Nem kötelező webes proxy konfigurációja

A beállítási varázsló nem jelszó webhelycsoport és eszköz regisztrációs kapcsolódó műveletek végrehajtása.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>A beállítási varázsló későbbi futtatása során előforduló hibák

Az alábbi táblázat ismerteti a hibák, a beállítási varázsló-eszközökön működik, a hiba lehetséges okai futtatásakor esetleg előforduló, és ajánlott műveletek megoldási lehetőséget. 

| nem. | Hibaüzenet jelenik meg vagy feltétel | Lehetséges okai | Javasolt teendő |
|:--- |:-------------------------- |:--------------- |:------------------ |
|  1  | Hiba 350032: Ez az eszköz már inaktív. | Ez a hiba jelenik meg, ha futtatja a beállítási varázsló inaktiválódik eszközt. | [Kapcsolatfelvétel a Microsoft támogatási szolgálattal](storsimple-contact-microsoft-support.md) , a következő lépéseket. Szolgáltatás nem kell elhelyezni egy inaktiválva eszközt. Az eszköz újra aktiválása előtt szükség lehet a gyári alaphelyzetbe állítása. |
|  2  | Meghívása HcsSetupWizard: ERROR_INVALID_FUNCTION (kivétel HRESULT: 0x80070001) | A DNS-kiszolgálói frissítés sikertelen. DNS-beállítások a globális beállításokat, és alkalmazza az engedélyezett hálózati adaptereken keresztül. | A felület engedélyezése, és újra alkalmazza a DNS-beállítások. Ez lehet megszakíthatja a hálózat többi engedélyezett felületekhez, mert ezek a beállítások általános. |
|  3  | Az eszköz úgy tűnik, hogy a StorSimple kezelő szolgáltatás portálon online, de próbál fejezze be a minimális beállítást, és mentse a konfigurációt, amikor a művelet sikertelen lesz. | Kezdeti telepítés során a webes proxy nincs konfigurálva, akkor is, ha volt-tényleges proxykiszolgáló helyen. | A [próba-HcsmConnection parancsmag] [ 2] keresse meg a hibát. [Kapcsolatfelvétel a Microsoft támogatási szolgálattal](storsimple-contact-microsoft-support.md) , ha nem tudja kijavítani a hibát. |
|  4  | Meghívása HcsSetupWizard: Érték nem várt tartományba esik. | Az alhálózathoz nem a megfelelő maszkot ezt a hibát eredményez. Lehetséges okai: <ul><li> Az alhálózathoz maszk nem található vagy üres.</li><li>Az Ipv6 előtag formázása nem megfelelő.</li><li>A felület felhő engedélyezni, de az átjáró hiányzó vagy téves.</li></ul>Ne feledje, hogy adatokat 0 automatikusan felhő engedélyező Ha konfigurálva a beállítási varázsló lépéseit. | Határozza meg a problémát, alhálózat 0.0.0.0 vagy 256.256.256.256 használja, és tekintse meg a kimenet. Írja be a megfelelő értékeket a alhálózat maszk, az átjáró és az IPv6-előtagot, szükség szerint. |
 
## <a name="error-codes"></a>Hibakódok esetén

Névsorban hibák jelennek meg.

|Hibaszám|Hiba szövegét, vagy a leírás|Javasolt teendő|
|:---|:---|:---|
|10502|Hiba történt a tárterület-fiók használata közben.|Várjon néhány percet, és próbálkozzon újra. Ha a hiba nem szűnik meg, állítsa a kapcsolatfelvétel a Microsoft támogatási szolgálattal a következő lépéseket.|
|40017|A biztonsági mentés sikerült, szerepelnek a biztonságimásolat házirendben kötet nem található az eszközön.|Ismételje meg a biztonsági mentés művelet, ha a hiba nem szűnik meg, lépjen kapcsolatba a Microsoft Support. a következő lépéseket.|
|40018|A biztonsági mentés sikerült, a kötet szerepelnek a biztonságimásolat házirendben egyike sem található az eszközön. |Ismételje meg a biztonsági mentés művelet, ha a hiba nem szűnik meg, lépjen kapcsolatba a Microsoft Support. a következő lépéseket.|
|390061|A rendszer nem elfoglalt vagy nem érhető el.|Várjon néhány percet, és próbálkozzon újra. Ha a hiba nem szűnik meg, állítsa a kapcsolatfelvétel a Microsoft támogatási szolgálattal a következő lépéseket.|
|390143|Hiba történt a 390143 kódszámú hiba jelenik meg. (Ismeretlen hiba ").|Ha a hiba nem szűnik meg, lépjen kapcsolatba a Microsoft Support a következő lépéseket.|

## <a name="next-steps"></a>Következő lépések

Ha nem tudja, hogy oldja meg a problémát, segítségért [forduljon a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) . 


[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx
