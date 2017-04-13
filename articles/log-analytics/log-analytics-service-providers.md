<properties
    pageTitle="Jelentkezzen Analytics szolgáltatások szolgáltatók számára |} Microsoft Azure"
    description="Log Analytics segítséget felügyelt szolgáltatók (MSPs), nagyvállalatok számára, független szoftver keresve és üzemeltetési szolgáltatók kezelése, és figyelemmel követheti a kiszolgálókat az ügyfél a helyszíni vagy felhőalapú infrastruktúrájának."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="richrund"/>

# <a name="log-analytics-features-for-service-providers"></a>Jelentkezzen be a Analytics szolgáltatások szolgáltatók számára

Log Analytics felügyelt szolgáltatók (MSPs), a nagyvállalatok számára, a független szoftver keresve és a üzemeltetési szolgáltatók kezelése, és figyelemmel követheti a kiszolgálókat az ügyfél a helyszíni vagy felhőalapú infrastruktúrájának segítséget. 

Nagyvállalatok számára megosztása sok hasonlóságokat szolgáltatók, különösen akkor, ha van egy központi IT csoport kezelése felelős számos különböző részlegek IT. Az egyszerűség kedvéért ehhez a dokumentumhoz a szerződési időszak *szolgáltató* használ, de ugyanazokat a szolgáltatásokat nagyvállalatoknak és más felhasználók is érhető el.

## <a name="cloud-solution-provider"></a>Felhőalapú megoldás szolgáltató

Partnerek és a [Felhő megoldás szolgáltató (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) program részeként szolgáltatók napló Analytics egyike a Azure CSP előfizetés elérhető szolgáltatásokat. 

Log Analytics az alábbi funkciók a *Felhőben megoldás szolgáltató* előfizetések engedélyezve vannak.

Egy *Felhőalapú megoldás szolgáltató* a következőkre van lehetősége:

+ Log Analytics-munkaterületek létrehozása egy bérlői (ügyfél)-előfizetésben.
+ Access-munkaterületek bérlők által létrehozott. 
+ Adja meg, és távolítsa el a felhasználók hozzáférését a munkaterület Azure felhasználókezelés használatával. A bérlő munkaterületen a MOBILE portálon a felhasználók kezelése lap a beállítások nem érhető el
  - Log Analytics nem támogatja a szerepköralapú hozzáférés- még - megadása a felhasználó `reader` az Azure-portálon jogosultsági lehetővé teszi a MOBILE portál beállításainak módosítása

Jelentkezzen be egy bérlői előfizetés, meg kell adja meg a bérlői azonosítóját. A bérlői azonosító része, gyakran utolsó való bejelentkezéshez használt e-mail címe.

+ Adja meg a MOBILE portál `?tenant=contoso.com` a portál URL-címét. Ha például`mms.microsoft.com/?tenant=contoso.com`
+ A PowerShell, használja a `-Tenant contoso.com` paraméter használata esetén `Add-AzureRmAccount` parancsmag
+ A bérlői azonosító használatakor automatikusan felkerül a `OMS portal` hivatkozásra kattintva nyissa meg, és jelentkezzen be a kijelölt munkaterületről a MOBILE portál az Azure portálról

Egy *vevő* egy felhőalapú megoldás szolgáltató a következőkre van lehetősége:

+ Az előfizetés CSP analytics-munkaterületek napló létrehozása
+ A CSP által létrehozott Access-munkaterületek
  -  Használja a `OMS portal` hivatkozásra kattintva nyissa meg, és jelentkezzen be a kijelölt munkaterületről a MOBILE portál az Azure portálról
+ Megtekintése és felhasználó kezelése lapon a beállítások csoportban a MOBILE portál használata

>[AZURE.NOTE] A biztonsági mentés és a webhely helyreállítási-megoldások a napló Analytics nem tud csatlakozni a helyreállítási szolgáltatások tárolóból elemre, és nem konfigurálhatók CSP-előfizetésben.

## <a name="managing-multiple-customers-using-log-analytics"></a>Log Analytics használatával több ügyfél kezelése 

Javasoljuk, hogy Ön kezeli ügyfelek napló Analytics munkaterület létrehozása. Log Analytics munkaterület nyújtja:

+ Tárolja az adatokat földrajzi helyét. 
+ A számlázási Granularitás 
+ Adatok elkülönítési 
+ Egyedi konfigurációja

Ügyfél egy munkaterületre létrehozásával tudunk egyes felhasználói adatok külön megtartani, és is követheti az ügyfelek a használatát.

Mikor és miért hozhat létre több munkaterületek további tájékoztatást ismertetett [analytics bejelentkezni hozzáférés kezelése a] (log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Létrehozását és az ügyfél-munkaterületek konfigurálása automatizálható [PowerShell](log-analytics-powershell-workspace-configuration.md), [erőforrás-kezelő sablonok](log-analytics-template-workspace-configuration.md)használata, vagy a [REST API -t](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).

A munkaterület konfigurálása az erőforrás-kezelő sablonok használatát lehetővé teszi, hogy kell rendelkeznie a fő konfiguráció hozhat létre és munkaterületek konfigurálása használható. Biztos lehet benne, hogy az ügyfeleknek munkaterületek létrehozásakor automatikusan konfigurálhatók a követelményeknek. Ha az igényeknek megfelelően alakíthatja, akkor a sablon frissül, és újra alkalmazza a a meglévő munkaterületek. Ez a folyamat biztosítja, hogy akár meglévő munkaterületek felel meg az új szabványoknak.    

Több napló Analytics-munkaterületek kezelése esetén javasoljuk egyes munkaterületeken integrálása a meglévő Feladatkezelő rendszer / műveleti konzol [Figyelmeztetési](log-analytics-alerts.md) szolgáltatásait használt. Integrálja az alkalmazást a meglévő rendszerek, az ügyfélszolgálati munkatársak is hajtsa végre a már jól ismert folyamatok. Log Analytics rendszeresen ellenőrzi az egyes munkaterületeken szemben a riasztási megadott feltételek és riasztást jelenít meg, amikor a művelet van szükség.

Vezetői szintű jelentésekhez, adatok összegzése keresztül munkaterületek napló analitikai és a [PowerBI](log-analytics-powerbi.md)integrációja használható. Ha egy másik jelentési rendszer integrálása van szüksége, használhatja a keresési API (keresztül PowerShell vagy a [többi](log-analytics-log-search-api.md)) lekérdezések futtatása és a keresési eredmények exportálása.

## <a name="next-steps"></a>Következő lépések

+ Automatizálható létrehozását és [erőforrás-kezelő sablonok](log-analytics-template-workspace-configuration.md) használata munkaterületek konfigurálása
+ A [PowerShell](log-analytics-powershell-workspace-configuration.md) használatá munkaterületek létrehozása automatizálása 
+ [Ha integrálni a meglévő rendszerek értesítésekkel](log-analytics-alerts.md)
+ Használja a [PowerBI](log-analytics-powerbi.md) összegző jelentések készítése
