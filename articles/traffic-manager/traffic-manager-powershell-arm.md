<properties
    pageTitle="Azure erőforrás-kezelő támogatási forgalom Manager |} Microsoft Azure "
    description="Az adatforgalom-kezelő az Azure erőforrás-kezelő PowerShell használatával"
    services="traffic-manager"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="azure-resource-manager-support-for-azure-traffic-manager"></a>Azure erőforrás-kezelő támogatási az Azure forgalom parancsra

Azure erőforrás felügyelője az előnyben részesített kezelőfelület szolgáltatások Azure-ban. Az erőforrás-kezelő Azure-alapú API-hoz és eszközök Azure forgalom Manager profilokat kell kezelni.

## <a name="resource-model"></a>Erőforrás-modell

Azure forgalom Manager van konfigurálva, a forgalom Manager profil nevű beállítások használatával. Profil a DNS-beállítások, forgalom útválasztási beállítások, végpont ellenőrzési beállítások és szolgáltatási végpontok, amelyhez a rendszer irányítja a forgalmat listáját tartalmazza.

Minden forgalom Manager profil "TrafficManagerProfiles" típusú erőforrás jelöli. A REST API szintjén az egyes profilok URI a következőképpen történik:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="comparison-with-the-azure-traffic-manager-classic-api"></a>Az Azure forgalom Manager klasszikus API összehasonlítása

Az Azure erőforrás-kezelő támogatási forgalom Manager eltérő kifejezéseket, mint a klasszikus telepítési modellt használja. A következő táblázat mutatja az erőforrás-kezelő és klasszikus kifejezéseket közötti különbségeket:

| Erőforrás-kezelő kifejezés | Klasszikus kifejezés |
|-----------------------|--------------|
| Adatforgalom útválasztás módszer | Terheléselosztási módszer |
| Prioritás módszer | Feladatátvevő módszer |
| Súlyozott módszer | Ciklikus módszer |
| Teljesítmény módszer | Teljesítmény módszer |

Ügyfeleink visszajelzései alapján számúra módosítása a referenciacikk olyan kifejezéseket áttekinthetővé javítása és a gyakori félreértést csökkentése. Nem funkció nincs különbség.

## <a name="limitations"></a>Korlátozások

"AzureEndpoints" típusú zárólap hivatkozik egy webalkalmazás, forgalom Manager végpontok csak hivatkozhat az alapértelmezett (gyártási) [webalkalmazás tárolóhely](../app-service-web/web-sites-staged-publishing.md). Saját helyek nem támogatottak. Kerülő egyéni helyek konfigurálhatók a "ExternalEndpoints" típusát.

## <a name="setting-up-azure-powershell"></a>Azure PowerShell beállítása

Microsoft Azure PowerShell használata az ezeket az utasításokat. Az alábbi cikkből megtudhatja, hogyan telepítheti, állíthatja Azure PowerShell.

- [Telepítse és állítsa be a Azure PowerShell hogyan](../powershell-install-configure.md)

Ez a cikk példáiban feltételezik, hogy meglévő erőforráscsoport. Létrehozhat egy erőforrás csoportot a következő parancsot:

```powershell
    New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

>[AZURE.NOTE] Azure erőforrás-kezelő használatához az összes erőforráscsoport egy helyre van szükség. Ezen a helyen használják alapértelmezés szerint az erőforrások erőforráscsoport létrehozott. Jó helyen jár, mivel forgalom Manager profil erőforrások globális, nem területi, erőforrás csoport helyét választható nincs hatással van Azure forgalom Manager.

## <a name="create-a-traffic-manager-profile"></a>Adatforgalom Manager profil létrehozása

A New-AzureRmTrafficManagerProfile parancsmag használatával forgalom Manager profil létrehozása:

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

Az alábbi táblázat ismerteti a Paraméterek:

| Paraméter | Leírás |
|-----------|-------------|
| név | Az adatforgalom-kezelő profil erőforrás erőforrás neve. Az azonos erőforráscsoport profilok egyedieknek kell lenniük. Ez a név nem azonos a DNS-név, a DNS-lekérdezéseket használni.|
| ResourceGroupName | Az erőforráscsoport tartalmazó a profil erőforrás neve.|
| TrafficRoutingMethod | Megadja azt határozza meg, hogy melyik végpont válaszként ad vissza egy DNS-lekérdezést adatforgalom útválasztás módszer. Lehetséges értékek a következők: "Teljesítmény", "Weighted" vagy "Priority".|
| RelativeDnsName | Adja meg a DNS-név forgalom Manager profil által biztosított hostname (állomásnév) részét. Ez az érték együtt a DNS-tartománynév Azure forgalom kezelő által használt űrlap a profil teljes tartománynevét (FQDN). Például "contoso" érték lesz "contoso.trafficmanager.net."|
| TTL (ÉLETTARTAM) | Itt adhatja meg a DNS Time-to-Live (TTL), a másodpercben megadott. A TTL (élettartam) tájékoztatja a helyi DNS-névfeloldók és a DNS-ügyfelek mennyi forgalom Manager profil gyorsítótár DNS válaszokat.|
| MonitorProtocol | Adja meg a protokoll használatával végpont rendszerállapot figyelése. Lehetséges értékek a következők: "HTTP" és "HTTPS".|
| MonitorPort | Adja meg a végpontot állapot nyomon követésére portot.|
| MonitorPath | Elérési útja végpontot egészségügyi probe használt a végpontot tartománynév viszonyított.|

A parancsmaggal forgalom Manager profil létrehozása az Azure, és a megfelelő felhasználóiprofil-objektumot visszatér a PowerShell. Ezen a ponton a profil nem tartalmaz olyan végpontok. Való felvételével a forgalom Manager végpontok kapcsolatos további tudnivalókért olvassa el a [Forgalom Manager végpontok hozzáadása](#adding-traffic-manager-endpoints)című témakört.

## <a name="get-a-traffic-manager-profile"></a>A forgalom Manager profilok beszerzése

Meglévő forgalom Manager profil objektum beolvasásához, használja a Get-AzureRmTrafficManagerProfle parancsmag:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Ezzel a parancsmaggal a forgalom kezelő profil objektum adja eredményül.

## <a name="update-a-traffic-manager-profile"></a>A forgalom Manager profil frissítése

Forgalom Manager profilok módosításával a következőképpen egy 3 lépésből áll:

1. A Get-AzureRmTrafficManagerProfile használatával profil beolvasása, vagy használja a New-AzureRmTrafficManagerProfile által visszaadott profilt.
2. A profil módosítása. Hozzáadása és eltávolítása a végpontok vagy végpontot, vagy a profil paramétereinek módosítása. Offline műveletek, amelyek a módosítások. A helyi memóriát, amely a profil-objektumot csak módosítja.
3. Hajtsa végre a módosításokat a Set-AzureRmTrafficManagerProfile parancsmaggal.

Felhasználóiprofil-tulajdonságok segítségével módosíthatók a profil RelativeDnsName kivételével. Ha módosítani szeretné a RelativeDnsName, törölnie kell a és egy új profil új néven.

A következő példa bemutatja, hogyan módosíthatja a profil TTL (élettartam):

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    $profile.Ttl = 300
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Háromféle forgalom Manager végpontok van:

1. **Azure végpontok** azok Azure-ban tárolt szolgáltatások
2. **Külső végpontok** azok Azure-Ön kívül üzemeltetett szolgáltatások
3. **Egymásba ágyazott végpontok** forgalom Manager profilok beágyazott hierarchiájának létrehozása használhatók. Beágyazott végpontok engedélyezése összetett alkalmazások forgalom útválasztás speciális beállításait.

Három esetben végpontok kétféle módon adhatók hozzá:

1. 3 lépésből álló eljárással a korábban leírt használatával. Ez a módszer az az előnye, hogy több végpont módosítás egy frissítés.
2. A New-AzureRmTrafficManagerEndpoint parancsmaggal. Ezzel a parancsmaggal zárólap ad egy műveletben egy meglévő forgalom Manager profilhoz.

## <a name="adding-azure-endpoints"></a>Azure végpontok hozzáadása

Azure végpontok szolgáltatások Azure-ban is hivatkozhat. Azure végpontok háromféle támogatottak:

1. Azure Web Apps alkalmazások
2. Klasszikus"cloud services (amelyek tartalmazhatnak PaaS szolgáltatás vagy a IaaS virtuális gépeken futó)
3. Azure PublicIpAddress erőforrások (amely lehet csatolni egy terheléselosztó vagy hálózati kártya virtuális gépen). A PublicIpAddress rendelkeznie kell használni az adatforgalom-kezelőben rendelt DNS nevét.

Minden esetben:

- A szolgáltatás hozzáadása-AzureRmTrafficManagerEndpointConfig vagy új-AzureRmTrafficManagerEndpoint "targetResourceId" paramétere megadva.
- A "Cél" és "EndpointLocation" a TargetResourceId vet.
- Megadásához a "súly" nem kötelező. Súlyok csak akkor használható, ha a profil "Weighted" forgalom útválasztás-metódus használatára van beállítva. Egyéb esetben ezeket figyelmen kívül hagyja. Adja meg, ha az értéket kell egy 1 és 1000 közötti számot. Az alapértelmezett értéke "1".
- Megadásához a 'prioritása' nem kötelező. Prioritásának csak akkor használja, ha a profil "Prioritása" forgalom útválasztás-metódus használatára van beállítva. Egyéb esetben ezeket figyelmen kívül hagyja. 1 és 1000 magasabb prioritást jelző kisebb értékű van érvényes értékeket. Ha egyik végpontját megadva, azokat meg kell adni az összes végponton. Elhagyása esetén értéke "1" kezdve alapértékek veszi a sorrendben jelennek meg, hogy a végpontok.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>1 – példa Felvétele Web App végpontok hozzáadása-AzureRmTrafficManagerEndpointConfig használatával

Ez a példa azt forgalom Manager profil létrehozása, és írjon a Hozzáadás-AzureRmTrafficManagerEndpointConfig parancsmaggal végpontokat Web App.

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $webapp1 = Get-AzureRMWebApp -Name webapp1
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
    $webapp2 = Get-AzureRMWebApp -Name webapp2
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-a-classic-cloud-service-endpoint-using-new-azurermtrafficmanagerendpoint"></a>2 – példa Egy "klasszikus" felhő szolgáltatás végpontjának használatával új-AzureRmTrafficManagerEndpoint felvétele

Ebben a példában a "klasszikus" felhőalapú szolgáltatás végpontjának hozzáadva forgalom Manager profil. Ebben a példában a profilt a profil- és erőforrás csoport nevek használata helyett egy profilt objektum áthaladó megadott azt. Mindkét megközelítésnek támogatottak.

    $cloudService = Get-AzureRmResource -ResourceName MyCloudService -ResourceType "Microsoft.ClassicCompute/domainNames" -ResourceGroupName MyCloudService
    New-AzureRmTrafficManagerEndpoint -Name MyCloudServiceEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $cloudService.Id -EndpointStatus Enabled

### <a name="example-3-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Például: 3: A New-AzureRmTrafficManagerEndpoint használatával publicIpAddress végpont hozzáadása

Ebben a példában egy nyilvános IP-cím erőforrás bekerül a forgalom Manager profilt. A nyilvános IP-címet kell rendelkeznie a DNS-név, és a hálózati kártya egy virtuális gép vagy egy terheléselosztó is kötött.

    $ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled

## <a name="adding-external-endpoints"></a>Külső végpontok hozzáadása

Adatforgalom Manager használja a külső végpontok irányítsa át az Azure-Ön kívül üzemeltetett services-alapú forgalmat. Azure végpontok, a külső végpontok vagy használja a Set-AzureRmTrafficManagerProfile, vagy új-AzureRMTrafficManagerEndpoint hozzáadása AzureRmTrafficManagerEndpointConfig felvehetők.

Külső végpontok megadásakor:

- Az endpoint tartománynevet meg kell adni a "Cél" paraméter használatával
- A "Teljesítmény" forgalom útválasztás módszert, ha a ": EndpointLocation szükség. Egyéb esetben nem kötelező. Az érték [érvényes Azure régió nevét](https://azure.microsoft.com/regions/)kell lennie.
- A "Súly" és "Priority" nem kötelező.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>1 – példa Hozzáadás-AzureRmTrafficManagerEndpointConfig és a Set-AzureRmTrafficManagerProfile külső végpontok felvétele

Ebben a példában azt forgalom Manager profil létrehozása, adja hozzá a külső végpontokat, és hajtsa végre a módosításokat.

    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>2 – példa Új-AzureRmTrafficManagerEndpoint használatával külső végpontok felvétele

Ebben a példában a külső zárólap egy meglévő profilhoz hozzáadunk. A profil van megadva, a profil- és erőforrás csoport a nevek használatáról.

    New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled

## <a name="adding-nested-endpoints"></a>"Beágyazott" végpontok hozzáadása

Minden forgalom Manager profilja egységes forgalom útválasztás módszert. Vannak azonban olyan esetek bonyolultabb forgalom útválasztás, mint a továbbítás nyújtotta egyetlen forgalom Manager profil igénylő. Egynél több forgalom útválasztás módszer előnyei egyesítéséhez forgalom Manager profilok ágyazhatók egymásba. Beágyazott profilok lehetővé teszi a támogatási nagyobb és összetettebb alkalmazás telepítések alapértelmezett forgalom Manager működési módjának felülbírálására. Részletes példák [egymásba ágyazott forgalom Manager profilok](traffic-manager-nested-profiles.md)című témakör tartalmaz.

Beágyazott végpontok megtörténik a szülő profilnál, egy adott végpont típust, a "NestedEndpoints" használatával. Beágyazott végpontok megadásakor:

- Az endpoint meg kell adni a "targetResourceId" paraméter használatával
- A "Teljesítmény" forgalom útválasztás módszert, ha a "EndpointLocation" szükség. Egyéb esetben nem kötelező. Az érték [érvényes Azure régió nevét](http://azure.microsoft.com/regions/)kell lennie.
- A "Súly" és "Priority" megadása nem kötelező, mint az Azure végpontok.
- A "MinChildEndpoints" paramétert a lépés nem kötelező. Az alapértelmezett értéke "1". Ha a rendelkezésre álló végpontok számát e küszöb alá esik, a szülő-profil úgy véli, a gyermek profil csökkent"és forgalom végpontjait a szülő-profil hozunk.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>1 – példa Beágyazott végpontok hozzáadása-AzureRmTrafficManagerEndpointConfig és a Set-AzureRmTrafficManagerProfile felvétele

Ebben a példában azt hozzon létre új forgalom Manager gyermek és szülő profilok, mint egy beágyazott végpontot a gyermek hozzáadása a szülő, és hajtsa végre a módosításokat.

    $child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

Ebben a példában fentebb did nem hozzáadunk bármely más végpontja a gyermek, illetve a szülő profilokhoz.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>2 – példa Új-AzureRmTrafficManagerEndpoint használatával beágyazott végpontok felvétele

Ebben a példában hozzáadunk egy meglévő gyermek profil mint egy beágyazott végpontot egy meglévő szülő-profilhoz. A profil van megadva, a profil- és erőforrás csoport a nevek használatáról.

    $child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2

## <a name="update-a-traffic-manager-endpoint"></a>A forgalom Manager végpont frissítése

Kétféleképpen meglévő forgalom Manager zárólap frissítése:

1. A Get-AzureRmTrafficManagerProfile használatával forgalom Manager profil első, frissítheti a profiljában végpont tulajdonságait, és hajtsa végre a módosításokat a Set-AzureRmTrafficManagerProfile. Ez a módszer az az előnye, hogy egy lépésben egynél több végpont frissíthetők tartalmaz.
2. A forgalom Manager végpont Get-AzureRmTrafficManagerEndpoint első, frissítse a végpontot tulajdonságait, és hajtsa végre a módosításokat a Set-AzureRmTrafficManagerEndpoint. Ez a módszer egyszerűbb, mert a végpontok tömb profilban az indexelő nincs szükség.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Példa 1: A Get-AzureRmTrafficManagerProfile és a Set-AzureRmTrafficManagerProfile végpontok frissítése

Ebben a példában a két végponton belül egy meglévő profilhoz prioritás azt módosíthatja.

    $profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
    $profile.Endpoints[0].Priority = 2
    $profile.Endpoints[1].Priority = 1
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>2 példa: A Get-AzureRmTrafficManagerEndpoint és a Set-AzureRmTrafficManagerEndpoint zárólap frissítése

Ebben a példában egy meglévő profilhoz egyetlen végpont vastagságának módosítása azt.

    $endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
    $endpoint.Weight = 20
    Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Engedélyezése és letiltása a végpontok és profilok

Adatforgalom Manager lehetővé teszi, hogy engedélyezve van, és le van tiltva, és amely lehetővé teszi, engedélyezése és letiltása a teljes profilok egyéni végpontok.
Ezek a változások tehetők az első/frissítése és beállítások a végpontot, vagy a profil erőforrásokat. Hogy korszerűsíthessék ezeket a gyakori műveleteket, azok is támogatott dedikált parancsmagok keresztül.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Példa 1: Engedélyezése és letiltása a forgalom Manager profilok

Ahhoz, hogy a forgalmat Manager profilok, használja az Enable-AzureRmTrafficManagerProfile. A profil egy felhasználóiprofil-objektum segítségével adható meg. A profil objektum használatával, vagy a folyamat keresztül továbbíthatók a "-TrafficManagerProfile" paramétert. Ez a példa azt adja meg a profil által a profil- és erőforrás-csoport nevét.

```powershell
    Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

A forgalom Manager profilok letiltása:

```powershell
    Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

A letiltás-AzureRmTrafficManagerProfile parancsmag megerősítését kéri. Ez a kérdés is leállítva használata a "-hatályba" paramétert.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Példa 2: Engedélyezése és letiltása a forgalom Manager végpont

Ahhoz, hogy egy forgalom Manager végpontot, használja az Enable-AzureRmTrafficManagerEndpoint. Adja meg a végpont kétféleképpen

1. A via a folyamat átadott TrafficManagerEndpoint objektum vagy használata a "-TrafficManagerEndpoint" paraméter
2. A végpontjának neve, végpont típusa, profil nevét, és az erőforrás csoport neve:

```powershell
    Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Hasonlóképpen a forgalom Manager végpont letiltása:

```powershell
     Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Letiltása-AzureRmTrafficManagerProfile, a megerősítést kérő a letiltás-AzureRmTrafficManagerEndpoint parancsmag kéri. Ez a kérdés is leállítva használata a "-hatályba" paramétert.

## <a name="delete-a-traffic-manager-endpoint"></a>A forgalom Manager végpont törlése

Egyes végpontok eltávolításához használja a eltávolítása-AzureRmTrafficManagerEndpoint parancsmagot:

```powershell
    Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Ezzel a parancsmaggal megerősítését kéri. Ez a kérdés is leállítva használata a "-hatályba" paramétert.

## <a name="delete-a-traffic-manager-profile"></a>A forgalom Manager profilok törlése

A forgalom Manager profilok törléséhez használja a eltávolítása-AzureRmTrafficManagerProfile parancsmag, adja meg a profil- és erőforrás neve:

```powershell
    Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Ezzel a parancsmaggal megerősítését kéri. Ez a kérdés is leállítva használata a "-hatályba ' paraméter.

A profil törlődik a profil objektuma segítségével is adható:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

A sorozat is átirányítható:

```powershell
    Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Következő lépések

[Adatforgalom Manager figyelése](traffic-manager-monitoring.md)

[Adatforgalom Manager teljesítménnyel kapcsolatos szempontok](traffic-manager-performance-considerations.md)
