<properties
   pageTitle="Fordított proxykiszolgáló háló szolgáltatás |} Microsoft Azure"
   description="A microservices belüli és kívüli a fürt közötti kommunikáció szolgáltatás háló fordított proxykiszolgáló használata"
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/04/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-reverse-proxy"></a>Szolgáltatás háló fordított proxykiszolgáló

A szolgáltatás háló fordított proxykiszolgáló egy fordított proxykiszolgáló beépített szolgáltatás lehetővé teszi, hogy a szolgáltatás háló fürt microservices, elérhetővé tenni a HTTP-végpontok megcímezheti háló.

## <a name="microservices-communication-model"></a>Microservices kommunikációs modell

A szolgáltatás háló Microservices általában futtassa a virtuális meg egy részét a fürt és lehet váltani egy virtuális különböző okok miatt. A végpontokat microservices dinamikusan alakíthatja. A szokásos mintázatot a microservice való kommunikáció, az alábbi feloldás le

1. Ezek az SRV, először a elnevezési szolgáltatáson keresztül.
2. Csatlakozás a szolgáltatáshoz.
3. A csatlakozási hibák okát, és újra feloldása az SRV, szükség esetén.

Ez a folyamat általában magában foglalja a körbefuttatás ügyféloldali kommunikációs tárakat, amely a szolgáltatás megoldás, és próbálkozzon újra házirendek újrapróbálkozási ismétlődve.
A témával kapcsolatos további tudnivalókért olvassa el a [szolgáltatások kommunikáció](service-fabric-connect-and-communicate-with-services.md)című témakört.

### <a name="communicating-via-sf-reverse-proxy"></a>Kommunikáció KB fordított proxyn keresztül
Szolgáltatás háló fordított proxykiszolgáló fut, a fürt csomópontjait. Az ügyfél nevében végrehajtja a teljes szolgáltatás megoldási folyamatának azt, és ezután továbbítja az ügyfél kérése. Így a a fürthöz ügyfelek csak segítségével egy ügyféloldali HTTP kommunikációs tárat beszélgetni a cél szolgáltatás az ugyanazon a csomóponton helyileg futó KB fordított proxyn keresztül.

![Belső kommunikációhoz][1]

## <a name="reaching-microservices-from-outside-the-cluster"></a>A Microservices alkalmazáson kívüli a fürt
Az alapértelmezett külső kommunikáció modell microservices **kiválaszthatják, a** hol alapértelmezés szerint minden szolgáltatás nem érhető el közvetlenül a külső ügyfeleket. Az [Azure terheléselosztó](../load-balancer/load-balancer-overview.md) hálózati oszlopazonosító jobboldali microservices és a külső ügyfelek közötti hálózati címfordítást hajt végre, és a külső kérések továbbítja az belső **IP:port** végpontok pontra. Kezdeményezéséhez egy microservice végpont közvetlenül elérhető külső ügyfelek, a Azure terheléselosztó kell először beállítania, hogy minden olyan portot, a fürt a szolgáltatás által használt előre forgalmat. Ezenkívül a legtöbb microservices (esp. állapot-nyilvántartó microservices) nem aktív a fürt minden csomóponton találhatók, és azok csomópontok feladatátvételt, áthelyezhet ezekben az esetekben az Azure terheléselosztó nem hatékony alapján határozza meg a cél csomópont replikán továbbítani a forgalmat.

### <a name="reaching-microservices-via-the-sf-reverse-proxy-from-outside-the-cluster"></a>A KB fordított proxykiszolgáló a kívül a fürt keresztül elérje a Microservices

Az azure terheléselosztó egyes szolgáltatási portokat konfigurálásáról, hanem csak a KB fordított proxykiszolgáló portja az Azure betöltése terheléselosztó beállíthatók. Ez a szolgáltatások belül a fürt nélkül egy további beállításokat a fordított proxyn keresztül elérje a fürt kívüli ügyfelek lehetővé teszi.

![A külső kommunikáció][0]

>[AZURE.WARNING] A micro szolgáltatások konfigurálása a fordított proxykiszolgáló portja a terheléselosztó, lehetővé teszi a jelenítik meg a http végpont kell a fürt kívüli addressible fürt.


## <a name="uri-format-for-addressing-services-via-the-reverse-proxy"></a>A fordított proxyn keresztül szolgáltatások megcímezheti URI-formátum

A fordított proxykiszolgáló egy bizonyos URI-formátumban használja, mely szolgáltatás partíciót a beérkező kérelem következőnek azonosítása:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&Timeout=<timeout_in_seconds>
```

 - **hibáról:** A fordított proxykiszolgáló beállítható úgy, hogy HTTP vagy HTTPS-forgalom elfogadásához. Abban az esetben, ha HTTPS-forgalom SSL lemondási következnek be a fordított proxykiszolgáló A rendszer átirányítja a fordított proxykiszolgáló szolgáltatások a fürt összehívások http keresztül történik.
 - **Fürt FQDN |} belső IP:** A külső ügyfelek esetében az, fordított proxykiszolgáló beállíthatók úgy, hogy a fürt tartomány (például mycluster.eastus.cloudapp.azure.com) keresztül érhető el. Minden csomóponton futtatja az, fordított proxykiszolgáló alapértelmezés szerint úgy belső forgalmához azt is érhető el a localhost, vagy a minden belső csomópont IP-(például 10.0.0.1).
 - **Port:** A port, amely a fordított proxykiszolgáló lett megadva. Pl.: 19008.
 - **ServiceInstanceName:** A telepített szolgáltatást teljesen minősített példányának neve a elérni kívánt szolgáltatás a "háló: /" színsémát. Például, hogy elérje a szolgáltatás *háló: / SajátPr/myservice/*, *SajátPr/myservice*célszerű használni.
 - **Utótag elérési útja:** Ez a tényleges URL-címe a szolgáltatást, amely, amelyhez csatlakozni szeretne. Ha például *myapi/értékek/hozzáadása/3*
 - **PartitionKey:** Egy particionált szolgáltatás az elérni kívánt partíciót a kiszámított partíciót kulcsa. Megjegyzendő, hogy ez *nem* a partíciót GUID azonosítója. Ez a paraméter nem a egyszeres partíciót séma szolgáltatásokhoz szükséges.
 - **PartitionKind:** A szolgáltatás sémája. Ez lehet "Int64Range" vagy "nevű. Ez a paraméter nem a egyszeres partíciót séma szolgáltatásokhoz szükséges.
 - **Időtúllépés:**  Ez a adja meg a HTTP-kérés a fordított proxykiszolgáló nevében az ügyfél kérése a szolgáltatás által létrehozott időtúllépés. Ez az alapértelmezett értéke 60 másodperc. Ez a választható paraméter.

### <a name="example-usage"></a>Példa a használatra

Példaként nézzük szolgáltatás **háló: / SajátPr/MyService** , amely megnyitja az alábbi URL-címet a HTTP-figyelő:

```
http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Az alábbi forrásokat:

 - `/index.html`
 - `/api/users/<userId>`

Ha a szolgáltatás által a egyszeres particionálási sémát, *PartitionKey* és *PartitionKind* karakterlánc lekérdezésparaméter nem szükséges, és a szolgáltatást, az átjáró keresztül érhető el:

 - Külső felhasználókkal:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService`
 - Belső:`http://localhost:19008/MyApp/MyService`

Ha a szolgáltatás által a egységes Int64 particionálási sémát, *PartitionKey* és *PartitionKind* karakterlánc lekérdezésparaméter elérje a szolgáltatás partíciót kell használni:

 - Külső felhasználókkal:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
 - Belső:`http://localhost:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

A szolgáltatás által elérhetővé tett erőforrások eléréséhez egyszerűen helyezze az erőforrás elérési út az URL-cím szolgáltatás neve:

 - Külső felhasználókkal:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
 - Belső:`http://localhost:19008/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

Az átjáró majd továbbítja e kérelmek a szolgáltatás URL-címe:

 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Különleges kezelési port megosztási szolgáltatások

Az alkalmazás átjáró próbálja újra megoldani a szolgáltatás címet, és ismételje meg a kérelmet, ha a szolgáltatás nem érhető el. Az egyik az átjáró, jelentős előnyökkel jár, az ügyfél kódot, nem szükséges saját szolgáltatás felbontás megvalósítása és hurok megoldásához.

Általában nem érhető el a szolgáltatás azt jelzi a szolgáltatás példányának vagy replika helyezte át egy másik csomópontot a normál életciklus részeként. Amikor ez történik, az átjáró jelző zárólap már nem nyissa meg az eredetileg megoldott címet a hálózati kapcsolat hiba jelenhet meg.

Azonban kópiák vagy szolgáltatás példányainak megoszthat egy host folyamatot, és előfordulhat, hogy is megoszthatja a port egy http.sys-alapú webes kiszolgáló által üzemeltetett együtt:

 - [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
 - [ASP.NET Core WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
 - [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

Ebben az esetben valószínű, hogy az érintett webkiszolgálóra érhető el a host folyamat és kérelmek megválaszolásában, de a megszűnt szolgáltatást példány vagy replika már nem érhető el az állomáson. Ebben az esetben az átjáró érkezik HTTP 404-es választ az érintett webkiszolgálóra. HTTP 404-es eredményt adja, két különböző jelentését foglalja magában:

 1. A szolgáltatás cím helyes, de nem létezik a felhasználó által kért erőforrás.
 2. A szolgáltatás címe nem megfelelő, és a felhasználó által kért erőforrás ténylegesen található előfordulhat, hogy egy másik csomópontot.

Az első lehetőséget választja az egy normál HTTP 404-es, a felhasználó hibára minősülő. Jó helyen jár a második lehetőséget választja, a felhasználó által kért megtalálható erőforrás, de az átjáró nem tudott úgy találhatja meg, mert a szolgáltatás magát megváltozott, ebben az esetben az átjáró elvégzendő újra feloldani a címét, és próbálkozzon újra.

Az átjáró így megkülönböztetni ezekben az esetekben két lehetőség van szüksége. Adott különbséget kezdeményezéséhez egy emlékeztetőt a kiszolgálóról szükség.

 - Alapértelmezés szerint az alkalmazás átjáró eset #2 feltételezi, és megpróbálja újra megoldani, és ismét ki a kérést.
 - Annak jelzésére, az alkalmazás átjáró eset #1, a szolgáltatás a következő HTTP-válasz fejléc térjen vissza:

`X-ServiceFabric : ResourceNotFound`

A HTTP-válasz fejléc azt jelzi, hogy egy normál HTTP 404-es helyzet, amelyben a kért erőforrás nem létezik, és az átjáró nem próbálja meg újra a webszolgáltatás címének feloldásához.

## <a name="setup-and-configuration"></a>Telepítés és beállítás
A szolgáltatás háló fordított proxykiszolgáló engedélyezhető a fürt az [erőforrás-kezelő Azure-sablon](./service-fabric-cluster-creation-via-arm.md)segítségével.

Ha befejezte a sablon a telepítéshez használni kívánt fürt (a mintasablonok vagy egyéni erőforrás-kezelő sablonként létrehozásával) a fordított proxykiszolgáló engedélyezhető a sablon az alábbi lépéseket.

1. A sablon a [Paraméterek rész](../resource-group-authoring-templates.md) a fordított proxykiszolgáló olyan portot határozza meg.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19008,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Az egyes a nodetype objektumok **fürt** [erőforrás típusa szakaszban](../resource-group-authoring-templates.md) adja meg, a port

    A apiVersion's előtt "2016-09-01' a port azonosítjuk a paraméter ***httpApplicationGatewayEndpointPort*** neve

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "httpApplicationGatewayEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

    Az apiVersion's be- vagy után "2016-09-01' a port azonosítjuk a paraméter ***reverseProxyEndpointPort*** neve

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

3. A fordított proxy az azure fürt kívül eső megoldására lépés: 1 megadott port **azure betöltés terheléselosztó szabályok** beállítása

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. A fordított proxykiszolgáló használatához konfigurálja az SSL-tanúsítványok a port, a tanúsítvány felvétele az [erőforrás típusa szakaszban](../resource-group-authoring-templates.md) **fürt** a httpApplicationGatewayCertificate tulajdonság

    A apiVersion's előtt "2016-09-01' a paraméter azonosítjuk a tanúsítvány neve ***httpApplicationGatewayCertificate***

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "httpApplicationGatewayCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```
    Az apiVersion's be- vagy után "2016-09-01' a paraméter azonosítjuk a tanúsítvány neve ***reverseProxyCertificate***
    
    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

## <a name="next-steps"></a>Következő lépések
 - Példa HTTP kommunikációs szolgáltatások [GitHUb minta projekt](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount)között.

 - [A megbízható szolgáltatások távelérési távoli eljáráshívás](service-fabric-reliable-services-communication-remoting.md)

 - [Webes API-t használó OWIN megbízható szolgáltatások](service-fabric-reliable-services-communication-webapi.md)

 - [WCF kommunikációs megbízható Services használatával](service-fabric-reliable-services-communication-wcf.md)


[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
