<properties
   pageTitle="A Windows biztonsági használata Windows rendszerű fürtre biztonságos |} Microsoft Azure"
   description="Megtudhatja, hogy miként csomópontok és az ügyfél-csomópont biztonság beállítása a Windows biztonsági használata Windows rendszeren futó önálló fürthöz."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="secure-a-standalone-cluster-on-windows-using-windows-security"></a>A Windows biztonsági használata Windows önálló fürt biztonságos

Ezzel az illetéktelen hozzáférést megakadályozására szolgáltatás háló fürt kell a biztonság, különösen akkor, ha van rá futó termelési feladatok. Ez a cikk ismerteti, hogyan lehet az *ClusterConfig.JSON* fájl használatával a Windows biztonsági csomópontok és az ügyfél-csomópont biztonság beállítása és a configure biztonsági lépéssel [önálló fürt létrehozása a Windows rendszeren futó](service-fabric-cluster-creation-for-windows-server.md)felel meg. Hogyan szolgáltatás háló használja a Windows biztonsági kapcsolatos további tudnivalókért olvassa el a [fürt biztonsági esetek](service-fabric-cluster-security.md)című témakört.

>[AZURE.NOTE]
Érdemes biztonsági a csomópont-csomópont biztonsági beállítástól gondosan, mivel nincs fürt frissítés egy olyan biztonsági választási lehetőségek egy másikra. A biztonsági kijelölés módosításával, akkor a teljes fürt újraépítő szükséges.

## <a name="configure-windows-security"></a>A Windows biztonsági konfigurálása
A minta *ClusterConfig.Windows.JSON* konfigurációs fájl letöltése a [Microsoft.Azure.ServiceFabric.WindowsServer a.<version>. zip](http://go.microsoft.com/fwlink/?LinkId=730690) önálló fürt csomag beállítása a Windows biztonsági sablon tartalmazza.  A Windows biztonsági **Tulajdonságok** szakaszában van beállítva:

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

|**Kereséskonfigurációs beállítás**|**Leírás**|
|-----------------------|--------------------------|
|ClusterCredentialType|A Windows biztonsági engedélyezve van a **ClusterCredentialType** paraméter *Windows*értékre állításával.|
|ServerCredentialType|A Windows biztonsági ügyfélalkalmazások engedélyezve van a *Windows* **ServerCredentialType** paraméterének beállításával. Ez azt jelzi, hogy az ügyfelek a fürt és a fürthöz fut az Active Directory-tartománya belül.|
|WindowsIdentities|A fürt és az ügyfél-azonosító tartalmazza.|
|ClusterIdentity|Adja meg a biztonsági a csomópont-csomópont. Csoport vesszővel tagolt listáját felügyelt fiókok szolgáltatás vagy a számítógép nevét.|
|ClientIdentities|Adja meg az ügyfél-csomópont biztonsági. Egy tömb ügyfél felhasználói fiókok.|
|Azonosító|Ügyfél azonosítója, a tartomány felhasználó.|
|IsAdmin|Igaz Itt adhatja meg, hogy a tartomány felhasználónak rendszergazdai ügyfélhozzáférés, a felhasználó ügyfélhozzáférés hamis.|

[Biztonsági a csomópont csomópontjának](service-fabric-cluster-security.md#node-to-node-security) beállítás használatával **ClusterIdentity**állítható be. Annak érdekében, hogy a csomópontok között meghatalmazotti készítéséhez is el kell szem előtt, minden más. A két különböző módon lehet elvégezni: Adja meg a csoport felügyelt szolgáltatásfiók, amely tartalmazza a fürt csomópontjait vagy az összes csomópontot, a tartomány csomópont identitások a fürt. Kifejezetten ajánljuk, használja a [Csoport felügyelt szolgáltatási fiók (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) megközelítés, különösen a nagyobb fürt (10-nél több csomópontok) vagy fürt, amelyek valószínűleg a nagyobb vagy kisebb.
Ezt a megközelítést lehetővé teszi, hogy a csomópontok, illetve a gMSA, a anélkül, hogy a fürt jegyzék módosításait el. Ezt a megközelítést nincs szükség a tartomány, amelynek fürt rendszergazdák kapott jogokkal hozzáadása és eltávolítása a tagok csoport létrehozását. További tudnivalókért olvassa el a [Felügyelt szolgáltatás csoportfiókok – első lépések](http://technet.microsoft.com/library/jj128431.aspx)című témakört.

[Biztonsági a csomópont ügyfele](service-fabric-cluster-security.md#client-to-node-security) van beállítva, **ClientIdentities**használatával. Annak érdekében, hogy az ügyfél és a fürt közötti megbízhatónak, meg kell adnia a fürt az, hogy milyen ügyfél identitások megbízhatónak is. Két különböző módokon erre: Adja meg a tartomány felhasználók csoporthoz csatlakozás és adja meg a tartomány csomópont csatlakozó felhasználók számára is. Szolgáltatás háló két különböző hozzáférési vezérlőtípus támogatja az ügyfelek, amelyeket a szolgáltatási háló fürtre csatlakoztatott: rendszergazdája és a felhasználó. Hozzáférés-vezérlés lehetővé teszi a fürt műveletek, felhasználók, a fürt biztonságosabbá tétele különböző csoportokhoz bizonyos típusú való hozzáférés korlátozása a fürt rendszergazdák számára.  A rendszergazdák kezelése-funkciók (köztük a olvasási/írási funkciók) teljes hozzáférése van. A felhasználók, alapértelmezés szerint rendelkeznek csak olvasási hozzáférést kezelési lehetőségek (például a lekérdezés lehetőségek), és az azt jelenti, hogy feloldása az alkalmazások és szolgáltatások.

A következő példa **biztonsági beállításai** szakaszban konfigurálja a Windows biztonsági, és adja meg, hogy a gép az *ServiceFabric/clusterA.contoso.com* a fürt részei, és hogy *CONTOSO\usera* felügyeleti ügyfél-hozzáférési:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
        "IsAdmin": true
        }]
    }
},
```

## <a name="next-steps"></a>Következő lépések

Miután beállította a Windows biztonsági, a *ClusterConfig.JSON* fájlban, folytathatja a fürt létrehozásának folyamata a [különálló fürt létrehozása a Windows rendszeren futó](service-fabric-cluster-creation-for-windows-server.md).

További információt olvashat biztonsági a csomópont-csomópont ügyfél-csomópont biztonsági és szerepköralapú hozzáférés-vezérlés Lásd: [fürt biztonsági esetek](service-fabric-cluster-security.md).

Olvassa el a [Csatlakozás a biztonságos fürtre](service-fabric-connect-to-secure-cluster.md) példák a csatlakozás a PowerShell alrendszerrel vagy FabricClient.
