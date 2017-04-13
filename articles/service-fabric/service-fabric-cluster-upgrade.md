<properties
   pageTitle="Frissítse az Azure Service háló fürt |} Microsoft Azure"
   description="A szolgáltatás háló kódot és/vagy a szolgáltatás háló fürt fürt frissítési mód tanúsítványok, az alkalmazás-portok OS javítások módon hozzáadása frissítés beállításáról futó konfigurációs frissítése, és így tovább. Mi, számíthat megtörténik a frissítés?"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-an-azure-service-fabric-cluster"></a>Az Azure Service háló fürt frissítése

> [AZURE.SELECTOR]
- [Azure fürthöz](service-fabric-cluster-upgrade.md)
- [Különálló fürthöz](service-fabric-cluster-upgrade-windows-server.md)

Olyan modern rendszerekhez bővíthetőség megtervezése billentyűt a termék hosszú távú sikeres eléréséhez. Az Azure Service háló fürt egy erőforrást, saját, de részben a Microsoft kezeli. Ez a cikk ismerteti, mit automatikusan kezeli, és mi beállíthatja saját magának.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>A fürt futó háló-verziót szabályozása

Beállíthatja, hogy a frissítések automatikus háló kap, ha a Microsoft által kiadott egy új verzió, vagy válassza ki, jelölje ki a kívánt a fürt kell az támogatott háló verzióját fürt.

Ehhez a "upgradeMode" fürt konfigurálása beállítása a portálon vagy erőforrás-kezelő létrehozása vagy később élő fürt idején használatával 

>[AZURE.NOTE] Ellenőrizze, hogy a támogatott háló verzióját futtató mindig fürt megtartása. És azt a szolgáltatást háló új verziójának megjelenése bejelentése, az előző verzióhoz képest legalább adott dátumtól 60 nap után támogatás vége van megjelölve. az új kiadásaihoz bejelentett [az szolgáltatás háló termékcsapat blogjában](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Új kiadásának kiválasztásához, majd érhető el. 

14 nappal, a kibocsátás, az fürt fut, a lejárat előtt egy állapot jön létre, amely a fürt figyelmeztetés állapot állapotba helyezi. A fürt állapot marad, amíg nem frissíti az támogatott háló verziójához.


### <a name="setting-the-upgrade-mode-via-portal"></a>Állítsa a frissítési mód portálon keresztül 

Beállíthatja, hogy a fürt automatikus vagy kézi a fürt létrehozásakor.

![Create_Manualmode][Create_Manualmode]

Beállíthatja, hogy a fürt automatikus vagy kézi a élő fürt, hogy az manage felületen használatával. 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Frissítés a portálon keresztüli kézi módba van állítva, fürthöz új verzió.
 
Új verzió frissítéséhez kell tennie mindössze jelölje ki a legördülő menü a rendelkezésre álló verziót, és mentse. A háló frissítés módja megrúgni automatikusan. A fürt állapot házirendek (csomópont állapot és a vonatkozó összes a fürt az alkalmazások) betartását a frissítés során.

A fürt állapot házirendek nem éri el, ha a frissítés visszaáll. Görgessen lefelé a további információ a dokumentum egyéni állapot házirendekhez beállításáról. 

Miután a problémákat, amelyek következtében a visszaállítás van rögzített, kell a frissítés ismét kezdeményezzen ezeket a lépéseket, mielőtt követve.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a>Az erőforrás-kezelő sablon keresztül frissítési üzemmód beállítása 

Adja hozzá a "upgradeMode" konfiguráció Microsoft.ServiceFabric/clusters erőforrás-definíciós verziójának és a "clusterCodeVersion" értékűre egyikét a támogatott háló alább látható módon, és telepítheti a sablont. Az érvényes "upgradeMode" értékei "Kézi" vagy "Automatikus"
 
![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>A via erőforrás-kezelő sablon kézi módba van állítva, fürthöz új verzió frissítése.
 
Ha a fürt kézi módban van, egy új verziójára való módosítása a "clusterCodeVersion" verzióját és üzembe. A sablon példányban a háló frissítési megrúgni kap megrúgni automatikusan. A fürt állapot házirendek (csomópont állapot és a vonatkozó összes a fürt az alkalmazások) betartását a frissítés során.

A fürt állapot házirendek nem éri el, ha a frissítés visszaáll. Görgessen lefelé a további információ a dokumentum egyéni állapot házirendekhez beállításáról. 

Miután a problémákat, amelyek következtében a visszaállítás van rögzített, kell a frissítés ismét kezdeményezzen ezeket a lépéseket, mielőtt követve.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Lista beszerzése az összes rendelkezésre álló verzió minden környezetben a megadott előfizetéshez

A következő parancsot, és olyan eredménye hasonló kapja.

"supportExpiryUtc" Ez az információ, a Ha egy adott érvényessége hamarosan lejár vagy lejárt. A legújabb kiadásra nincs érvényes dátum -, értéke "9999-12-31T23:59:59.9999999", amely csak azt jelenti, hogy a lejárati dátum van még nem állította.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/clusterVersions?api-version= 2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a>Háló frissítési viselkedése, amikor a fürt frissítése mód: automatikus

A Microsoft fenntartja a háló kód és a beállításokat, amelyek egy Azure fürthöz fut. Azt a szoftver automatikus felügyelt verziófrissítése végezze el szükség szerint kombinálásával. Ezek a frissítések kódot vagy konfigurációs lehet. Győződjön meg arról, hogy az alkalmazás szenved térképeket, illetve ezek frissítése miatt minimális hatása, hogy végezze el a frissítések az alábbi lépéseket:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>1 fázis: Frissítés a végrehajtott minden fürt állapot házirendek használatával

Ebben a fázisban frissítések egy frissítési tartomány folytassa egyszerre, és az alkalmazások, a fürt futó tovább futnak bármely legrövidebb leállás nélkül. A fürt állapot házirendek (csomópont állapot és a vonatkozó összes a fürt az alkalmazások) betartását a frissítés során.

A fürt állapot házirendek nem éri el, ha a frissítés visszaáll. Majd az előfizetés tulajdonosa e-mailt küldi. Az e-mailben a következő információkat tartalmazza:

- Az értesítés, hogy azt is visszaállíthatja egy fürt frissítés.
- Javasolt javító a műveletek, ha van ilyen.
- (N), csak akkor hajtsa végre a fázis 2 napok számát.

Próbálja meg végrehajtani a azonos frissítés néhány többször abban az esetben, ha minden frissítések infrastruktúra okok miatt sikertelen volt. Az e-mail elküldésének dátuma az n számú nap után a Folytatás fázis 2.

A fürt állapot házirendek teljesülése esetén a frissítés sikeres tekintett és készre. Ez akkor történhet során a kezdeti frissítés, vagy a frissítés ismétlések ebben a szakaszban. A sikeres Futtatás nincs e-mail megerősítése nem. Ez a ne küldjön túl sok e-mailek – értesítő e-mailben, normál kivételként kell értékelni. A legtöbb anélkül, hogy az alkalmazás elérhetőségét érintő sikeres fürt frissítések megakadályozási.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>2 fázis: Egy frissítés végrehajtása az alapértelmezett állapot házirendek használatával

Ebben a szakaszban a rendszerállapot házirendek oly módon, hogy alkalmazásokat, amelyekre a frissítés elején megfelelő számú változatlan marad a frissítési folyamat az időtartamra vannak beállítva. Fázis 1, ahogy a fázis 2 frissítések egy frissítési tartomány folytassa egyszerre, és az alkalmazások, a fürt futó tovább futnak bármely legrövidebb leállás nélkül. A fürt állapot házirendek (csomópontot a egészségügyi és együttes összes a fürt az alkalmazások) a frissítés időtartama tartani.

Ha a fürt állapot házirendek gyakorlatilag egyike nem teljesül, a frissítés visszaáll. Majd az előfizetés tulajdonosa e-mailt küldi. Az e-mailben a következő információkat tartalmazza:

- Az értesítés, hogy azt is visszaállíthatja egy fürt frissítés.
- Javasolt javító a műveletek, ha van ilyen.
- (N), csak akkor hajtsa végre a fázis 3 napok számát.

Próbálja meg végrehajtani a azonos frissítés néhány többször abban az esetben, ha minden frissítések infrastruktúra okok miatt sikertelen volt. A emlékeztető e-mailt küldi el néhány nap előtt n naponta be. Az e-mail elküldésének dátuma az n számú nap után a Folytatás fázis 3. Az e-mailek általunk Önnek küldött fázis 2 komolyan kell venni, és a műveletek segíthetnek kell tenni.

A fürt állapot házirendek teljesülése esetén a frissítés sikeres tekintett és készre. Ez akkor történhet során a kezdeti frissítés, vagy a frissítés ismétlések ebben a szakaszban. A sikeres Futtatás nincs e-mail megerősítése nem.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>3 fázis: Egy frissítés végrehajtása olyan szigorú állapot szabályok használatával

Ebben a szakaszban a rendszerállapot házirendek szolgálják az alkalmazások állapotának, hanem a frissítés felé. Ebben a szakaszban a végeredmény nagyon kevés fürt frissítések. A fürt esetekben ez a fázis, ha nincs esély arra, hogy az alkalmazás sérült válik, illetve elérhetősége elvesznek.

Hasonlóan, mint a másik két szakaszból, fázis 3 frissítések a következő lépésben egy frissítési tartomány egyszerre.

A fürt állapot házirendek nem éri el, ha a frissítés visszaáll. Próbálja meg végrehajtani a azonos frissítés néhány többször abban az esetben, ha minden frissítések infrastruktúra okok miatt sikertelen volt. Ezt követően a fürt ki van emelve, így a már nem kap támogatási, illetve frissítést.

Ezt az információt tartalmazó e-mail az előfizetés tulajdonosa, a műveletek segíthetnek együtt küldi. Azt nem a várt érték bármely fürt megszerezni, hol fázis 3 sikerült állapotba.

A fürt állapot házirendek teljesülése esetén a frissítés sikeres tekintett és készre. Ez akkor történhet során a kezdeti frissítés, vagy a frissítés ismétlések ebben a szakaszban. A sikeres Futtatás nincs e-mail megerősítése nem.

## <a name="cluster-configurations-that-you-control"></a>Szabályozhatja, hogy fürt konfigurációk

Az azt jelenti, hogy a fürt beállítása mellett frissítési mód, az alábbiakban a konfigurációk, amely élő fürtre módosíthatja.

### <a name="certificates"></a>Tanúsítványok

Új, vagy egyszerűen törölje a tanúsítványok fürt és a portálon keresztüli ügyfél. Olvassa el [a részletes útmutatást a](service-fabric-cluster-security-update-certs-azure.md) dokumentumhoz

![Képernyőkép, melyen a tanúsítvány thumbprints az Azure-portálon.][CertificateUpgrade]


### <a name="application-ports"></a>Alkalmazás-portok

Alkalmazás-portok a csomópont típusa társított terheléselosztó erőforrás-tulajdonságok megváltoztatásával módosíthatja. A portál is használhatja, vagy közvetlenül erőforrás-kezelő PowerShell használható.

A csomópont típusát egy új portjához összes VMs megnyitásához tegye a következőket:

1. Egy új vizsgálati vehet fel a megfelelő terheléselosztó.

    Ha a fürt telepítette a portál használatával, a terheléselosztókkal elnevezése "LB-neve az erőforrás csoport NodeTypename", egy, az egyes csomópontot. Mivel a betöltés terheléselosztó nevek olyan egyedi, csak egy erőforrás csoporton belül, célszerű keresésekor őket egy adott erőforrás csoportban.

    ![Képernyőkép, melyen egy vizsgálati hozzáadása egy terheléselosztó a portálon.][AddingProbes]

2. Új szabály hozzáadása a terheléselosztó.

    Új szabály hozzáadása a azonos terheléselosztó a vizsgálati az előző lépésben létrehozott használatával.

    ![Új szabály hozzáadása egy terheléselosztó a portálon.][AddingLBRules]


### <a name="placement-properties"></a>Elhelyezési tulajdonságai

Az egyes csomópont típusú egyéni elhelyezése tulajdonságaik vannak, amelyek az alkalmazások használni kívánt is hozzáadhat. NodeType egy alapértelmezett tulajdonsága, amelyekkel egyértelműen helyezés nélkül.

>[AZURE.NOTE] Elhelyezése kényszerek, csomópont tulajdonságait és definiálásáról őket használatára vonatkozó részletes olvassa el a "Elhelyezése kényszerek és csomópont tulajdonságai" a szolgáltatás háló fürt erőforrás-kezelő dokumentumban, [A fürt ismertető](service-fabric-cluster-resource-manager-cluster-description.md)részt.

### <a name="capacity-metrics"></a>Kapacitás mérőszámok

Az egyes csomópont típusú egyéni kapacitásának mértékek, amely a használni kívánt jelentés terhelés az alkalmazásokban is hozzáadhat. Jelentés kapacitás mértékek használatára vonatkozó részletes betöltése, olvassa el a [mértékek és a terhelést](service-fabric-cluster-resource-manager-metrics.md) [A fürt leíró](service-fabric-cluster-resource-manager-cluster-description.md) és háló fürt erőforrás-kezelő szolgáltatás dokumentumokat.

### <a name="fabric-upgrade-settings---health-polices"></a>Frissítési beállítások háló - irányelveinek

Egyéni irányelveinek háló frissítésre is megadhat. Ha a fürt háló az automatikus frissítések állította, ezek a házirendek vonatkozik beszerzése a frissítések automatikus háló fázis-1.
Ha meg van a kézi háló fürthöz frissítések, ezek a házirendek minden alkalommal, amikor a rendszer a fürt a háló frissítés elindításához el az új verzió ki első érvényes. Ha nem bírálja felül az házirendeket, az alapértelmezett beállításokat használja.

Adja meg az egyéni állapot házirendeket, illetve áttekinteni a jelenlegi beállításokat, a "frissítés háló" lap, a Speciális beállítások frissítése kiválasztásával. Tekintse át a következő képen olvashat. 

![Egyéni állapot házirendek kezelése][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>A fürt háló beállításainak testreszabása

Keresse meg a [szolgáltatás háló fürt háló beállítások](service-fabric-cluster-fabric-settings.md) milyen, és hogyan szabhatja őket.

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a>A VMs a fürt alkotó javítást operációs rendszer

Ezt a lehetőséget a jövőben tervezett automatizált szolgáltatásként. De jelenleg felelős a VMs javítást. El kell végeznie ezt egy virtuális egyszerre, hogy nem feltétlenül egyszerre egynél több lefelé.

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>A VMs a fürt alkotó OS frissítések

Ha frissítenie kell a képet a fürt virtuális gépeken futó operációs rendszer, el kell végeznie ezt egy virtuális egyszerre. Ön a felelős a frissítésre – jelenleg nincs automatizálási ehhez.

## <a name="next-steps"></a>Következő lépések
- A [szolgáltatás háló fürt háló beállításainak](service-fabric-cluster-fabric-settings.md) testreszabása
- Megtudhatja, hogyan kell [a fürt és kicsinyítés méretezése](service-fabric-cluster-scale-up-down.md)
- További tudnivalók: [alkalmazás frissítése](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG