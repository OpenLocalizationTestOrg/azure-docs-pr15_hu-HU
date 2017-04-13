<properties
   pageTitle="A Visual Studio segítségével szolgáltatás háló fürt beállítása |} Microsoft Azure"
   description="Megtudhatja, hogy miként állíthatja be a szolgáltatás háló fürtre, Azure erőforráscsoport projektté a Visual Studio által létrehozott erőforrás-kezelő Azure-sablon segítségével"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>Állítsa be a szolgáltatás háló fürtre Visual Studio használatával
Ez a cikk ismerteti, hogy miként állíthatja be az Azure Service háló fürt a Visual Studio és egy erőforrás-kezelő Azure-sablon segítségével. A Visual Studio Azure csoport-projekt létrehozása a sablon fogjuk használni. A sablon létrehozása után ezt elvégezhető közvetlenül az Azure Visual Studio alkalmazásból. Azt is használható parancsfájlt vagy folyamatos integrációs (ki) létesítmény részeként.

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>Szolgáltatás háló fürt sablon létrehozása az Azure erőforrás projektek használatával
Első lépésként nyissa meg a Visual Studióban, és (érhető el a **felhőben** mappában) Azure erőforrás csoport projekt létrehozása:

![Az új projekt párbeszédpanel a kiválasztott Azure erőforráscsoport projekthez][1]

Hozzon létre egy új Visual Studio megoldás ehhez a projekthez, vagy vegye fel a meglévő megoldás.

>[AZURE.NOTE] Ha nem látható az Azure csoport projektet az a felhő csomópontot, nincs telepítve az Azure-SDK. Indítsa el a webes Platform telepítő ([letöltése, telepítése](http://www.microsoft.com/web/downloads/platform.aspx) , ha már nem), és keresse meg a "Azure SDK a .NET rendszerhez" és a Visual Studio azon verziójával kompatibilis verziója.

Miután az OK gombra kattintva gombra kattint, a Visual Studio kérni fogja válassza ki az erőforrás-kezelő sablont szeretne létrehozni:

![Jelölje ki az Azure-sablon párbeszédpanel a kiválasztott szolgáltatás háló fürt sablon][2]

Jelölje ki a szolgáltatás háló fürt sablont, majd kattintson ismét a az OK gombra. A projekt- és erőforrás-kezelő sablon most lettek létrehozva.

## <a name="prepare-the-template-for-deployment"></a>Felkészülés a sablon telepítéshez
A sablon létrehozása a fürt telepíti, mielőtt a szükséges sablon paraméterekkel értékeket kell adnia. Következő paraméterértékeket a program olvassa be a `ServiceFabricCluster.parameters.json` fájl, amely a `Templates` a erőforrás csoport projekt mappát. Nyissa meg a fájlt, és adja meg a következő értékeket:

|Paraméterek neve           |Leírás|
|-----------------------  |--------------------------|
|adminUserName            |A rendszergazdai fiók neve szolgáltatás háló gépekhez (csomópontok).|
|certificateThumbprint    |A tanúsítvány, hogy titkosítja a fürt ujjlenyomat.|
|sourceVaultResourceId    |Az *erőforrás-azonosító* titkosítja a fürt tanúsítványra tárolási helyére a fő tárolóra.|
|certificateUrlValue      |A fürt biztonsági tanúsítványt URL-CÍMÉT.|

A Visual Studio szolgáltatás háló erőforrás-kezelő sablon hoz létre egy tanúsítványt védett biztonságos fürt. A tanúsítvány azonosítjuk az utolsó három sablon paraméterek (`certificateThumbprint`, `sourceVaultValue`, és `certificateUrlValue`), és léteznie kell egy **Azure kulcs tárolóból elemre**. Hogyan hozhat létre a fürt biztonsági tanúsítványt, lásd: a [szolgáltatás háló fürt biztonsági esetek](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) cikk további információt.

## <a name="optional-change-the-cluster-name"></a>Nem kötelező: fürt nevének módosítása
Minden szolgáltatás háló fürt van neve. Háló fürt Azure-ban létrehozásakor fürt neve határozza meg, hogy (együtt az Azure régió) a fürt a tartománynév (DNS) nevet. Ha például tetszés szerinti nevet a fürt `myBigCluster`, és az erőforráscsoport ugyanis az új fürt helyét (Azure régió) kelet-Amerikai Egyesült Államok, a DNS-neve a fürt lesz `myBigCluster.eastus.cloudapp.azure.com`.

Alapértelmezés szerint a fürt nevének automatikusan létre, és egyedi tett véletlen utótag csatolása "fürt" előtag. Ezzel megkönnyíti nagyon a sablon használata a **folyamatos integrációs** (ki) rendszert részeként. Egy egyedi nevet szeretne adni a fürt tetszés találó, egy értékének beállítása a `clusterName` változó a az erőforrás-kezelő sablonfájlt (`ServiceFabricCluster.json`) a választott névre. Az adott fájlban definiált első változó.

## <a name="optional-add-public-application-ports"></a>Nem kötelező: nyilvános alkalmazás portok hozzáadása
Érdemes azt is, módosíthatja a nyilvános alkalmazás portokat a fürt, mielőtt beállítaná őket. Alapértelmezés szerint be a két nyilvános TCP-portok (80 és 8081) megnyílik a sablon. Ha többre is szükség van az alkalmazás, módosítsa a sablonban Azure terheléselosztó meghatározása. A fő sablonfájl tárolja a definíció (`ServiceFabricCluster.json`). Nyissa meg a fájlt, és keresse meg `loadBalancedAppPort`. Minden port társítva három eltérések:

1. A port a TCP port értéket meghatározó sablon változó:

    ```json
    "loadBalancedAppPort1": "80"
    ```

2. Egy *vizsgálati* , amely meghatározza, hogy milyen gyakran és meddig Azure terhelés terheléselosztó próbálja meg használni egy adott szolgáltatás háló csomópont próbálkozások fölé egy másikra. A szondákat az terheléselosztó erőforrás részét képezik. Az alapértelmezett alkalmazás első port vizsgálati definíciója a következő:

    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```

3. A *szabály terheléselosztási* kötelékek együtt a port és a vizsgálati, amely lehetővé teszi a terheléselosztás meg szolgáltatás háló fürt csomópontok keresztül:

    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
Ha az alkalmazásokat, amelyet használni szeretne a fürthöz üzembe további portok van szükség, akkor hozzáadhatja őket létrehozása további vizsgálati és terheléselosztási Állapotszabály-definíciók. Szeretné a Azure betöltése terheléselosztó erőforrás-kezelő sablonok között a további tudnivalókért lásd: [első lépések megtételében egy belső terheléselosztó sablon használatával](../load-balancer/load-balancer-get-started-ilb-arm-template.md).

## <a name="deploy-the-template-by-using-visual-studio"></a>A sablon üzembe Visual Studio segítségével
Az összes szükséges paraméterértékeket a mentés után a`ServiceFabricCluster.param.dev.json` fájl készen áll a sablon telepíthető, és a szolgáltatás háló fürt létrehozása. Kattintson a jobb gombbal az erőforrás a Visual Studio Solution Explorer projektek, és válassza a központi telepítés **|} Új telepítési...**. Ha szükséges, Visual Studio jelennek meg a **Deploy erőforrás csoporthoz** párbeszédpanelen kéri, hogy az Azure hitelesíteni:

![Erőforráscsoport párbeszédpanel telepítése][3]

A párbeszédpanel lehetővé teszi a fürt meglévő erőforrás-kezelő erőforráscsoport, és felajánlja a hozzon létre egy újat. Általában célszerű használni egy külön erőforráscsoport szolgáltatás háló fürt.

Miután a központi telepítés gomb gombra kattint, a Visual Studio kérni fogja erősítse meg a sablon paraméterértékeket. Kattintson a **Mentés** gombra. Egy vagy több paraméter nincs állandó értéke: a fürt rendszergazdai fiók jelszava. Meg kell adnia egy jelszót értéket, amikor a Visual Studio felajánlja az egyik.

>[AZURE.NOTE] Visual Studio Azure SDK 2.9 kezdve, a telepítés során támogatja olvasási jelszavakat a **Azure kulcs tárolóból elemre** . A sablon paraméterek párbeszédpanelen figyelje meg, hogy a `adminPassword` paraméter szövegdoboz van "kulcs" ábrázoló kis ikonra a jobb oldalon. Ez az ikon lehetővé teszi, hogy egy meglévő kulcs tárolóra titkos jelöli ki a fürt rendszergazdai jelszavát. Önnek elég gondoskodnia arról első engedélyezése Azure erőforrás-kezelő access telepítéshez sablon az Access speciális házirendek, a fő tárolóból elemre. 

A telepítési folyamatot a Visual Studio kimeneti ablakban végrehajtásának figyelheti meg. A sablon telepítés befejezése után az új fürt használatára!

>[AZURE.NOTE] Ha PowerShell soha nem használta a számítógépről, most használni kívánt Azure való, meg kell egy kis előkészítés tegye.
>1. Engedélyezze a PowerShell-parancsfájlokat futtatásával a [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) parancsot. Fejlesztési gépekhez "korlátlan" házirend elfogadható általában.
>2. Döntés engedélyezni szeretné a diagnosztikai adatok gyűjtése Azure PowerShell-parancsait, és futtassa [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) vagy [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) szükség szerint. Sablon a telepítés során így elkerülhető a szükségtelen képernyőn megjelenő utasításokat.

Hiba történik, ha az [Azure-portálra](https://portal.azure.com/) , és nyissa meg az erőforráscsoport a telepített. **Összes beállításai**parancsra, majd kattintson a beállítások lap **telepítések** . Erőforráscsoport telepítés nem sikerült elhagyja a diagnosztikai információ van.

>[AZURE.NOTE] Szolgáltatás háló fürt csak bizonyos számú csomópontok rendelkezésre álljon, és a state - néven "kvórum megőrzése" megőrzése be kell Tehát még nem biztonságos, állítsa le a fürt gépek összes, kivéve, ha először hajtotta végre [teljes biztonsági másolatot készíteni a állapotát](service-fabric-reliable-services-backup-restore.md).

## <a name="next-steps"></a>Következő lépések
- [Megtudhatja, hogyan állíthatja be az Azure portálon szolgáltatás háló fürthöz](service-fabric-cluster-creation-via-portal.md)
- [Megtudhatja, hogy miként kezelheti, és a Visual Studio segítségével szolgáltatás háló alkalmazások terjesztése](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
