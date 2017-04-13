<properties 
   pageTitle="Azure automatizálási adatok kezelése |} Microsoft Azure"
   description="Ez a cikk automatizálást Azure környezetben kezelésére szolgáló több témakör tartalmazza.  Jelenleg tartalmazza, adatmegőrzés és Azure automatizálási vészhelyreállítás Azure automatizálási mentésével."
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/02/2016"
   ms.author="bwren;sngun" />

# <a name="managing-azure-automation-data"></a>Azure automatizálási adatok kezelése

Ez a cikk automatizálási Azure környezetben kezelésére szolgáló több témakör tartalmazza.

## <a name="data-retention"></a>Adatmegőrzés

Amikor töröl egy erőforrás az Azure automatizálás, mielőtt végleges eltávolítása csoportból naplózási célokra 90 napig tartja meg.  Nem látható, és nem használja az erőforráshoz megadott időszakban.  Ezzel a házirenddel információforrások, amelyek a program törli az automatizálási fiók tagja is érinti.

Azure automatizálási automatikusan törli, és feladatok 90 napnál régebbi végleg eltávolítja.

Az alábbi táblázat összefoglalja a különböző erőforrásokat az adatmegőrzési szabályt.

|Adatok|Házirend|
|:---|:---|
|Fiókok|Követő 90 napon belül a fiók törlődik a felhasználó által véglegesen törlődnek.|
|Eszközök|Véglegesen törlődnek a követő 90 napon belül az eszköz a program törli a felhasználó vagy a fiókot, amely tartalmazza az eszköz a program törli a felhasználó által követő 90 napon.|
|Modulok|Véglegesen törlődnek a követő 90 napon belül a modul a program törli a felhasználó vagy a fiókot, amely rendelkezik a modul a program törli a felhasználó által követő 90 napon.|
|Runbooks|Véglegesen törlődnek a követő 90 napon belül az erőforrás a program törli a felhasználó vagy a fiókot, amely tartalmazza az erőforrás a program törli a felhasználó által követő 90 napon.|
|Feladatok|Törölt és véglegesen eltávolított követő 90 napon az utolsó módosítás alatt. Ennek oka lehet, miután a feladat befejezése, leállítása vagy fel van függesztve.|
|Csomópont konfigurációk/MOF fájlok| Régi csomópont konfigurálása végleg törlődik a követő 90 napon belül új csomópont konfiguráció jön létre.|
|DSC csomópontokat| Véglegesen törlődnek követő 90 napon belül a csomópont nem regisztrált Azure portálon vagy a [Unregister-AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) parancsmag használatával a Windows PowerShellben automatizálást-fiókból. Csomópontokat is véglegesen törlődnek a fiókot, amely tartalmazza a csomópontra a program törli a felhasználó által követő 90 napon. |
|Csomópont-jelentések| Véglegesen törlődnek a követő 90 napon belül új jelentés csomóponton jön létre|

Az adatmegőrzési szabály az összes felhasználóra vonatkozik, és jelenleg nem szabható testre.

## <a name="backing-up-azure-automation"></a>Azure automatizálást mentésével

Microsoft Azure-automatizálási fiók törlésekor a fiókot az összes objektumot törölt runbooks, modulok, konfigurációk, beállítások, feladatok és eszközök. Miután a fiók törlődik, az objektumok nem állíthatók helyre.  Az alábbi információk segítségével biztonsági másolatot készíteni a automatizálási fiók tartalmának törlés előtt. 

### <a name="runbooks"></a>Runbooks

Az Azure felügyeleti portál vagy a [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) parancsmag használatával a Windows PowerShell parancsprogramot fájlokat a runbooks is exportálhatók.  Ezek a parancsfájlok importálható másik automatizálási fiók [létrehozása vagy importálása egy Runbook](https://msdn.microsoft.com/library/dn643637.aspx)ismertetett módon.


### <a name="integration-modules"></a>Integrációs modulok

Az Azure automatizálási integrációs modulok nem exportálhatók.  Győződjön meg arról, hogy azok az automatizálási fiók kívüli érhető el.

### <a name="assets"></a>Eszközök

Az Azure automatizálási [eszközök](https://msdn.microsoft.com/library/dn939988.aspx) nem exportálhatók.  Az Azure adatkezelési portál használatával, meg kell tartsa szem előtt a változók, a hitelesítő adatokat, a tanúsítványok, a kapcsolatok és a ütemezések.  Ezután kézzel kell létrehozni bármelyik másik automatizálási programba importált runbooks által használt eszközök.

[Azure parancsmagok](https://msdn.microsoft.com/library/dn690262.aspx) használatával jövőbeli hivatkozási titkosítatlan eszközök, és mentse a módosításokat vagy részleteit beolvasásához, vagy egy másik automatizálási fiók egyenértékű eszközök létrehozása.

Nem lehet beolvasni a titkosított változók vagy hitelesítő adatok parancsmagok használatáról a jelszó mező értékét.  Ha nem tudja, hogy ezeket az értékeket, majd beolvasható őket egy runbook a [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) és a [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) tevékenységek használatával.

Tanúsítványok az Azure automatizálási nem exportálhatók.  Győződjön meg arról, hogy minden tanúsítványok Azure-Ön kívüli elérhetők.

### <a name="dsc-configurations"></a>DSC konfigurációk

A beállításokat az Azure felügyeleti portál vagy az [Exportálás-AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) parancsmag használatával a Windows PowerShell parancsprogramot fájlok exportálhatja. Ezek a konfigurációk aztán importálhat egy másik automatizálási fiók használt.


##<a name="geo-replication-in-azure-automation"></a>Az Azure automatizálás GEO replikációs

Replikációs GEO, az Azure automatizálási fiókok, normál biztonsági másolatot készít partneradatokat a redundancia más földrajzi területére. A fiók beállításakor választhat egy elsődleges tartomány, és kattintson egy másodlagos régió van rendelve automatikusan. A másodlagos, az elsődleges régió, a másolt adatok abban az esetben, ha az adatok elvesztését folyamatosan frissülnek.  

A következő táblázat mutatja az elérhető elsődleges és másodlagos régió párosítása.

|Elsődleges            |Másodlagos
| ---------------   |----------------
|A központi Dél-Amerikai Egyesült Államok   |A központi Észak-amerikai
|USA keleti 2          |A központi Amerikai Egyesült Államok
|Nyugati Európa        |Észak-Európa
|Dél-kelet-ázsiai    |Kelet-ázsiai
|Japán keleti         |Japán nyugati

A Microsoft, hogy egy elsődleges régió adatai elvész valószínű semmiképpen kísérel meg visszanyerésében. Az elsődleges adatokat nem lehet visszaállítani, majd geo átváltó történik, és a szóban forgó ügyfelek értesítést kap információt előfizetésének keresztül.

