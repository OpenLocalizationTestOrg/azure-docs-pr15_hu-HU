<properties 
    pageTitle="A PowerShell Azure Media Services-fiókok kezelése" 
    description="Megtudhatja, hogy miként kezelheti a PowerShell-parancsmagok az Azure Media Services fiókokat." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"
    ms.author="juliako"/>


#<a name="manage-azure-media-services-accounts-with-powershell"></a>A PowerShell Azure Media Services-fiókok kezelése

> [AZURE.SELECTOR]
- [Portál](media-services-portal-create-account.md)
- [A PowerShell](media-services-manage-with-powershell.md)
- [TÖBBI](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Is használhatja az Azure Media Services-fiók létrehozása, rendelkeznie kell az Azure-fiók. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure ingyenes próbaverziót</a>.

##<a name="overview"></a>– Áttekintés 

Ez a cikk az Azure Media Services (AMS) felsorolja az Azure PowerShell-parancsmagok az erőforrás-kezelő Azure keretében. A parancsmagok létezik a **Microsoft.Azure.Commands.Media** névtér.

## <a name="versions"></a>Verziók

**ApiVersion**: "a Skype 2015-10-01"
               

## <a name="new-azurermmediaservice"></a>Új AzureRmMediaService

Létrehoz egy media szolgáltatás.

### <a name="syntax"></a>Szintaxis

A paraméter megadása: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

A paraméter megadása: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek

**-ResourceGroupName &lt;karakterlánc&gt;**

Adja meg a csoport nevét, az erőforrás media szolgáltatást, amelyhez tartozik.

Aliasok | nincs lehetőség
---|---
Szükség?   |  Igaz
Pozíció?   |  0
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat? |TRUE(ByPropertyName)
Fogadja el a helyettesítő karakterek?  |hamis

**Fióknév - &lt;karakterlánc&gt;**

A médiafájlok szolgáltatás nevét adja meg.

Aliasok |név
---|---
Szükség? |Igaz
Pozíció? |1
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat? |hamis
Fogadja el a helyettesítő karakterek? |hamis

**-Hely &lt;karakterlánc&gt;**

A médiafájlok szolgáltatás erőforrás helyét adja meg.

Aliasok |nincs lehetőség
---|---
Szükség? |Igaz
Pozíció? |2
Alapérték  |nincs lehetőség
Fogadja el a beviteli folyamat? |TRUE(ByPropertyName)
Fogadja el a helyettesítő karakterek? |hamis

**-StorageAccountId &lt;karakterlánc&gt;**

Megad egy elsődleges tárterület-fiókot, amely a media szolgáltatás társítva.

- Tárterület (hozzon létre fiókot az erőforrás-kezelő API-val) csak támogatja.

- A tároló fiók léteznie kell, és az ugyanazon a helyen, a media szolgáltatással.

Aliasok |nincs lehetőség
---|---
Szükség? |Igaz
Pozíció? |3
Alapérték  |nincs lehetőség
Fogadja el a beviteli folyamat? |TRUE(ByPropertyName)
Paraméterek neve |StorageAccountIdParamSet
Fogadja el a helyettesítő karakterek?|hamis

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Itt adhatja meg, hogy a médiafájlok szolgáltatáshoz tartozó tárterület-fiókok.

- Tárterület (hozzon létre fiókot az erőforrás-kezelő API-val) csak támogatja.

- A tároló fiók léteznie kell, és az ugyanazon a helyen, a media szolgáltatással.

- Csak egy tárterület-fiókot az elsődleges adható meg.

Aliasok |nincs lehetőség
---|---
Szükség?  |Igaz
Pozíció?  |3
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat? |TRUE(ByPropertyName)
Paraméterek neve |StorageAccountsParamSet
Fogadja el a helyettesítő karakterek? |hamis

**-Címkék &lt;Hashtable&gt;**

Adja meg a címkéket a media szolgáltatás társított kivonat táblához.

- Példa:@{"tag1"="value1";"tag2"=:value2"}

Aliasok |nincs lehetőség
---|---
Szükség?  |hamis
Pozíció?  |névvel ellátott
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat? |hamis
Fogadja el a helyettesítő karakterek? |hamis

**&lt;Parancssori_paraméterek&gt;**

Ezzel a parancsmaggal támogatja a közös paraméterek:-hibakeresési, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - részletes - WarningAction, és - WarningVariable.

### <a name="inputs"></a>Bemeneti adatok alapján

A beviteli típus az objektumokat, akkor is pipe való parancsmag típusú is.

### <a name="outputs"></a>Kimeneti értékeket

A kimeneti típus az objektumokat, amelyek a parancsmag bocsát típusát.

## <a name="set-azurermmediaservice"></a>Set-AzureRmMediaService

A médiafájlok szolgáltatás frissíti.

### <a name="syntax"></a>Szintaxis

    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek

**-ResourceGroupName &lt;karakterlánc&gt;**

Adja meg a csoport nevét, az erőforrás media szolgáltatást, amelyhez tartozik.

Aliasok |nincs lehetőség
---|---
Szükség?  |Igaz
Pozíció?  |0
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat? |TRUE(ByPropertyName)
Fogadja el a helyettesítő karakterek? |hamis

**Fióknév - &lt;karakterlánc&gt;**

A médiafájlok szolgáltatás nevét adja meg.

Aliasok |név
---|---
Szükség? |Igaz
Pozíció? |1
Alapérték |Nincs lehetőség
Fogadja el a beviteli folyamat? |TRUE(ByPropertyName)
Fogadja el a helyettesítő karakterek? |Hamis

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Itt adhatja meg, hogy a médiafájlok szolgáltatáshoz tartozó tárterület-fiókok.

- Tárterület (hozzon létre fiókot az erőforrás-kezelő API-val) csak támogatja.

- A tároló fiók léteznie kell, és az ugyanazon a helyen, a media szolgáltatással.

- Csak egy tárterület-fiókot az elsődleges adható meg.

Aliasok |nincs lehetőség
---|---
Szükség? |hamis
Pozíció? |Névvel ellátott
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat? |TRUE(ByPropertyName)
Paraméterek neve |StorageAccountsParamSet
Fogadja el a helyettesítő karakterek? |hamis

**-Címkék &lt;Hashtable&gt;**

Adja meg a címkéket a médiafájlok szolgáltatás társított kivonat táblához.

- A címkéket a media szolgáltatás társított ügyfél által megadott érték helyére kerülnek.

Aliasok |nincs lehetőség
---|---
Szükség? |Hamis
Pozíció?  |Névvel ellátott
Alapérték |Nincs lehetőség
Fogadja el a beviteli folyamat? |TRUE(ByPropertyName)
Fogadja el a helyettesítő karakterek? |hamis

**&lt;Parancssori_paraméterek&gt;**

Ezzel a parancsmaggal támogatja a közös paraméterek:-hibakeresési, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - részletes - WarningAction, és - WarningVariable.

### <a name="inputs"></a>Bemeneti adatok alapján

A beviteli típus az objektumokat, akkor is pipe való parancsmag típusú is.

### <a name="outputs"></a>Kimeneti értékeket

A kimeneti típus az objektumokat, amelyek a parancsmag bocsát típusát.

## <a name="remove-azurermmediaservice"></a>Eltávolítás-AzureRmMediaService

A médiafájlok szolgáltatás eltávolítja.

### <a name="syntax"></a>Szintaxis

    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek

**-ResourceGroupName &lt;karakterlánc&gt;**

Adja meg a csoport nevét, az erőforrás media szolgáltatást, amelyhez tartozik.

Aliasok |nincs lehetőség
---|---
Szükség? |Igaz
Pozíció? |0
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat? |TRUE(ByPropertyName)
Fogadja el a helyettesítő karakterek? |hamis

**Fióknév - &lt;karakterlánc&gt;**

A médiafájlok szolgáltatás nevét adja meg.

Aliasok |nincs lehetőség
---|---
Szükség? |Igaz
Pozíció? |2
Alapérték |Nincs lehetőség
Fogadja el a beviteli folyamat?  |TRUE(ByPropertyName)
Fogadja el a helyettesítő karakterek? |Hamis

**&lt;Parancssori_paraméterek&gt;**

Ezzel a parancsmaggal támogatja a közös paraméterek:-hibakeresési, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - részletes - WarningAction, és - WarningVariable.

### <a name="inputs"></a>Bemeneti adatok alapján

A beviteli típus az objektumokat, akkor is pipe való parancsmag típusú is.

### <a name="outputs"></a>Kimeneti értékeket

A kimeneti típus az objektumokat, amelyek a parancsmag bocsát típusát.

## <a name="get-azurermmediaservice"></a>Get-AzureRmMediaService

Az erőforráscsoport szolgáltatások media vagy egy megadott névvel rendelkező media szolgáltatás kap.

### <a name="syntax"></a>Szintaxis

ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>] 

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek

**-ResourceGroupName &lt;karakterlánc&gt;**

Adja meg a csoport nevét, az erőforrás media szolgáltatást, amelyhez tartozik.

Aliasok |nincs lehetőség
---|---
Szükség? |Igaz
Pozíció?  |0
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat? |TRUE(ByPropertyName)
Paraméterek neve |ResourceGroupParameterSet, AccountNameParameterSet
Fogadja el a helyettesítő karakterek?   hamis

**Fióknév - &lt;karakterlánc&gt;**

A médiafájlok szolgáltatás nevét adja meg.

Aliasok |nincs lehetőség
---|---
Szükség? |Igaz
Pozíció?  |1
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat? |TRUE(ByPropertyName)
Paraméterek neve  |AccountNameParameterSet
Fogadja el a helyettesítő karakterek? |hamis

**&lt;Parancssori_paraméterek&gt;**

Ezzel a parancsmaggal támogatja a közös paraméterek:-hibakeresési, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - részletes - WarningAction, és - WarningVariable.

### <a name="inputs"></a>Bemeneti adatok alapján

A beviteli típus az objektumokat, akkor is pipe való parancsmag típusú is.

### <a name="outputs"></a>Kimeneti értékeket

A kimeneti típus az objektumokat, amelyek a parancsmag bocsát típusát.

## <a name="get-azurermmediaservicekeys"></a>Get-AzureRmMediaServiceKeys

Egy media szolgáltatás billentyűk kap.

### <a name="syntax"></a>Szintaxis

    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek

**-ResourceGroupName &lt;karakterlánc&gt;**

Adja meg a csoport nevét, az erőforrás media szolgáltatást, amelyhez tartozik.

Aliasok |nincs lehetőség
---|---
Szükség? |Igaz
Pozíció?  |0
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat? |TRUE(ByPropertyName)
Fogadja el a helyettesítő karakterek? |hamis

**Fióknév - &lt;karakterlánc&gt;**

A médiafájlok szolgáltatás nevét adja meg.

Aliasok |nincs lehetőség
---|---
Szükség? |Igaz
Pozíció? |1
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat? |TRUE(ByPropertyName)
Fogadja el a helyettesítő karakterek? |hamis

**&lt;Parancssori_paraméterek&gt;**

Ezzel a parancsmaggal támogatja a közös paraméterek:-hibakeresési, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - részletes - WarningAction, és - WarningVariable.

### <a name="inputs"></a>Bemeneti adatok alapján

A beviteli típus az objektumokat, akkor is pipe való parancsmag típusú is.

### <a name="outputs"></a>Kimeneti értékeket

A kimeneti típus az objektumokat, amelyek a parancsmag bocsát típusát.

## <a name="set-azurermmediaservicekey"></a>Set-AzureRmMediaServiceKey

Egy elsődleges és másodlagos kulcsa media szolgáltatás újragenerálása.

### <a name="syntax"></a>Szintaxis

    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek

**-ResourceGroupName &lt;karakterlánc&gt;**

Adja meg a csoport nevét, az erőforrás media szolgáltatást, amelyhez tartozik.

Aliasok |nincs lehetőség
---|---
Szükség?  |Igaz
Pozíció?  |0
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat?  |TRUE(ByPropertyName)
Fogadja el a helyettesítő karakterek? |hamis

**Fióknév - &lt;karakterlánc&gt;**

A médiafájlok szolgáltatás nevét adja meg.

Aliasok |nincs lehetőség
---|---
Szükség? |Igaz
Pozíció?  |1
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat?   |TRUE(ByPropertyName)
Fogadja el a helyettesítő karakterek? |hamis

**-KeyType &lt;KeyType&gt;**

Adja meg a fő a media szolgáltatás.

- Elsődleges és másodlagos

Aliasok |nincs lehetőség
---|---
Szükség?  |Igaz
Pozíció?  |2
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat? |hamis
Fogadja el a helyettesítő karakterek? |hamis

**&lt;Parancssori_paraméterek&gt;**

Ezzel a parancsmaggal támogatja a közös paraméterek:-hibakeresési, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - részletes - WarningAction, és - WarningVariable.

### <a name="inputs"></a>Bemeneti adatok alapján

A beviteli típus az objektumokat, akkor is pipe való parancsmag típusú is.

### <a name="outputs"></a>Kimeneti értékeket

A kimeneti típus az objektumokat, amelyek a parancsmag bocsát típusát.

## <a name="sync-azurermmediaservicestoragekeys"></a>Szinkronizálási-AzureRmMediaServiceStorageKeys

Tárterület fiók kulcsok a médiafájlok szolgáltatáshoz tartozó tárterület-fiók szinkronizálása.

### <a name="syntax"></a>Szintaxis

    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Paraméterek

**-ResourceGroupName &lt;karakterlánc&gt;**

Adja meg a csoport nevét, az erőforrás media szolgáltatást, amelyhez tartozik.

Aliasok |nincs lehetőség
---|---
Szükség? |Igaz
Pozíció? |0
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat? |TRUE(ByPropertyName)
Fogadja el a helyettesítő karakterek? |hamis

**Fióknév - &lt;karakterlánc&gt;**

A médiafájlok szolgáltatás nevét adja meg.

Aliasok |nincs lehetőség
---|---
Szükség? |Igaz
Pozíció? |1
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat? |TRUE(ByPropertyName)
Fogadja el a helyettesítő karakterek? |hamis

**-StorageAccountId &lt;karakterlánc&gt;**

Megadja, hogy a tárterület a media szolgáltatás társított.

Aliasok |Azonosító
---|---
Szükség? |Igaz
Pozíció?  |2
Alapérték |nincs lehetőség
Fogadja el a beviteli folyamat? |      TRUE(ByPropertyName)
Fogadja el a helyettesítő karakterek? |hamis

**&lt;Parancssori_paraméterek&gt;**

Ezzel a parancsmaggal támogatja a közös paraméterek:-hibakeresési, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - részletes - WarningAction, és - WarningVariable.

### <a name="inputs"></a>Bemeneti adatok alapján

A beviteli típus az objektumokat, akkor is pipe való parancsmag típusú is.

### <a name="outputs"></a>Kimeneti értékeket

A kimeneti típus az objektumokat, amelyek a parancsmag bocsát típusát.

## <a name="next-step"></a>Következő lépés 

Olvassa el a tanulási javaslatok Media Services.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
