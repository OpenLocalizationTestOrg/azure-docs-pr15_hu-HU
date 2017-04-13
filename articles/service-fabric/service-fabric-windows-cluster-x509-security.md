<properties
   pageTitle="Csatlakozás biztonságos magánjellegű fürthöz |} Microsoft Azure"
   description="Ez a cikk ismerteti, hogyan biztonságos kommunikációt a különálló vagy a személyes fürt, valamint az előbb ügyfelek és a fürt belül."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/08/2016"
   ms.author="dkshir"/>

# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>A különálló fürtre X.509 tanúsítványok használata Windows biztonságos

Ez a cikk ismerteti a közötti biztonságos kommunikációt a különböző csomópontok a különálló Windows fürt, valamint miként tanúsítványok használatával X.509 ehhez a fürthöz csatlakozó ügyfelek hitelesítést végezni. Ez biztosítja, hogy csak hitelesített felhasználó fürt, a telepített alkalmazások érhetik el és felügyeleti feladatokat.  Biztonsági tanúsítványt akkor célszerű engedélyezni a fürt a fürt létrehozásakor.  

Fürt biztonsági biztonsági csomópontok – például ügyfél-csomópont biztonsági és szerepköralapú hozzáférés-vezérlés a további tudnivalókért olvassa el a [fürt biztonsági esetek](service-fabric-cluster-security.md)című témakört.

## <a name="which-certificates-will-you-need"></a>Mely tanúsítványokat kell?

Kezdődik, [Töltse le a különálló fürt csomagot](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) a fürt csomópontját egyik. A letöltött csomag **ClusterConfig.X509.MultiMachine.json** fájl találja. Nyissa meg a fájlt, és ellenőrizze a **Biztonság** csoportban a **Tulajdonságok** szakasz szakasz:

    "security": {
        "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        "CertificateInformation": {
            "ClusterCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ServerCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ClientCertificateThumbprints": [{
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            }, {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }],
            "ClientCertificateCommonNames": [{
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint" : "[Thumbprint]",
                "IsAdmin": true
            }]
            "HttpApplicationGatewayCertificate":{
                "Thumbprint": "[Thumbprint]",
                "X509StoreName": "My"
            }
        }
    }

Ez a szakasz ismerteti a tanúsítványok van szüksége a különálló Windows fürt biztonságossá tétele. Tanúsítvány-alapú biztonsági engedélyezéséhez állítsa *X509* **ClusterCredentialType** és **ServerCredentialType** értékeit.

>[AZURE.NOTE] Egy [ujjlenyomatot](https://en.wikipedia.org/wiki/Public_key_fingerprint) tanúsítvány elsődleges azonosítása. Olvassa el a [hogyan tanúsítvány ujjlenyomat beolvasásához](https://msdn.microsoft.com/library/ms734695.aspx) megtudhatja, hogy az ujjlenyomatot a tanúsítványok létrehozott.

A következő táblázat felsorolja a tanúsítványok, hogy szüksége lesz a fürt beállítása:

|**CertificateInformation beállítás**|**Leírás**|
|-----------------------|--------------------------|
|ClusterCertificate|A tanúsítvány biztonságos kommunikációt a csomópontok fürtre között van szükség. Két különböző tanúsítványok, az elsődleges és másodlagos frissítésre használható. Az elsődleges tanúsítvány ujjlenyomat beállítása a **ujjlenyomat** szakaszt, és a másodlagos **ThumbprintSecondary** változók.|
|ServerCertificate|Amikor csatlakozik a fürthöz a tanúsítvány bemutatják az ügyfél. Kényelmesebbé megadhatja az *ClusterCertificate* és *ServerCertificate*tanúsítványt használja. Két különböző kiszolgálói tanúsítvány, egy elsődleges és másodlagos frissítésre használható. Az elsődleges tanúsítvány ujjlenyomat beállítása a **ujjlenyomat** szakaszt, és a másodlagos **ThumbprintSecondary** változók. |
|ClientCertificateThumbprints|Ez a tanúsítványok, telepítse a hitelesített ügyfélalkalmazásokon kívánt csoportja. Beállíthatja, hogy egy másik ügyfél tanúsítványok, amelyet a fürt való hozzáférés engedélyezése a számítógépen telepített számát. Beállíthatja az egyes tanúsítvány ujjlenyomatot **CertificateThumbprint** változó. Ha a **IsAdmin** *Igaz*, akkor ezzel a tanúsítvánnyal telepítve van az ügyfél végezhető műveletek a rendszergazda a fürt fájlkezelési műveletekhez. A **IsAdmin** értéke *hamis*, ha az ügyfél, ezzel a tanúsítvánnyal csak a műveletek végezhetők engedélyezett felhasználói engedélyeket, általában csak olvasható. További információt a szerepkörök olvassa el a [szerepköralapú hozzáférés-szerepalapú](service-fabric-cluster-security.md/#role-based-access-control-rbac)  |
|ClientCertificateCommonNames|A közös nevét az első ügyfél-tanúsítvány beállítása az **CertificateCommonName**. A **CertificateIssuerThumbprint** az ujjlenyomatot a kibocsátó a tanúsítvány. Olvassa el a [tanúsítványok dolgozunk](https://msdn.microsoft.com/library/ms731899.aspx) további tudnivalók a gyakori nevét és a kibocsátó.|
|HttpApplicationGatewayCertificate|Ez egy olyan választható tanúsítvány, amely a megadott meg szeretné-e a Http-alkalmazás átjáró biztonságos. Ellenőrizze, hogy reverseProxyEndpointPort az nodeTypes van állítva, ha ezt a tanúsítványt használni.|

Az alábbiakban példa fürt konfigurálása hol fürt, a kiszolgáló és az ügyfél tanúsítványok kapott.

 ```
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
      "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
      "iPAddress": "10.7.0.6",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ServerCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpoint": "19001",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="aquire-the-x509-certificates"></a>Beszerezni a X.509 tanúsítványok
Biztonságos kommunikáció a fürt belül, akkor először X.509 tanúsítványok a fürt csomópontok juthat. Emellett a meghatalmazott gépek/felhasználók fürt kapcsolat korlátozására szüksége lesz letöltése és telepítése az ügyfél gépekhez tanúsítványok.

Gyártási munkaterhelésekből futó fürtre, vonatkozóan kell használnia a [Tanúsítvány hitelesítésszolgáltató](https://en.wikipedia.org/wiki/Certificate_authority) aláírt biztonságossá tétele a fürt x.509-es tanúsítvány. További információ a tanúsítványok beszerzése, nyissa meg [hogyan: tanúsítvány beszerzése](http://msdn.microsoft.com/library/aa702761.aspx).

Teszt célra használt fürt megadhatja önaláírt tanúsítványt használni.

## <a name="optional-create-a-self-signed-certificate"></a>Nem kötelező: Önaláírt tanúsítvány létrehozása
Egy, hogy helyesen védett önaláírt tanúsítvány létrehozása egy úgy, hogy a *CertSetup.ps1* parancsfájl használatát a címtárban *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*szolgáltatás háló SDK mappában. Szerkesztheti a fájlt, és ezzel a paranccsal egy megfelelő nevet a tanúsítvány létrehozása.

Jelszóval védett PFX fájlba exportálja a tanúsítvány. Először meg kell szereznie a ujjlenyomat a tanúsítvány. A certmgr.exe alkalmazásnak a futtatására. Keresse meg a **Helyi számítógép\Személyes** mappát, és keresse meg az imént létrehozott tanúsítványát. Kattintson duplán a megnyitáshoz, és jelölje ki a *Részletek* fülre, és görgessen le a *ujjlenyomat* mező tanúsítvány. Másolja az ujjlenyomatot értéket, az alábbi PowerShell-parancsot eltávolítja a szóközöket.  Módosítsa a *$pswd* értéket védelmét, és futtassa a PowerShell alkalmasság biztonságos jelszó:

```   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

A számítógépen telepítve a tanúsítvány részleteinek megtekintéséhez futtatását is lehetővé teszi az alábbi PowerShell-parancsot:

```
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Azt is megteheti Ha az Azure előfizetéssel rendelkezik, kövesse a szakasz [hozzáadása tanúsítványok kulcs tárolóból elemre](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).

## <a name="install-the-certificates"></a>A tanúsítványok telepítése
Ha befejezte az (oka) t, telepítheti őket a fürt csomóponton. A legújabb Windows PowerShell rendelkeznie kell a csomópontok 3.x telepítve van a őket. Ismételje meg ezeket a lépéseket mindegyik csomóponton fürt és a kiszolgáló tanúsítványok és a másodlagos tanúsítványokat kell.

1. .Pfx fájl vagy fájlok másolása a csomópontot.
2. Rendszergazdaként PowerShell ablak megnyitása, és írja be a következő parancsokat. A *$pswd* cserélje le a tanúsítvány létrehozásához használt jelszót. A *$PfxFilePath* cserélje le a teljes elérési útját a .pfx, másolja a csomópontot.

    ```
    $pswd = "1234"
    $PfcFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```

3. Ezután kell beállítania a hozzáférés-vezérlés ebben a tanúsítványban meg, hogy a szolgáltatás háló folyamat a hálózati szolgáltatás fiókon fut, használhatja a következő parancsfájl futtatásával. Adja meg a ujjlenyomat a tanúsítvány és a "Hálózati szolgáltatás" a szolgáltatás fiókjához. Ellenőrizheti, hogy a tanúsítvány a hozzáférés-vezérlési listák helyesek a certmgr.exe eszközzel, és megjeleníti a tanúsítvány a titkos kulcsok kezelése.

    ```
    param
    (
        [Parameter(Position=1, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$pfxThumbPrint,

        [Parameter(Position=2, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$serviceAccount
        )

        $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; };

        # Specify the user, the permissions and the permission type
        $permission = "$($serviceAccount)","FullControl","Allow"
        $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission;

        # Location of the machine related keys
        $keyPath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\";
        $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName;
        $keyFullPath = $keyPath + $keyName;

        # Get the current acl of the private key
        $acl = (Get-Item $keyFullPath).GetAccessControl('Access')

        # Add the new ace to the acl of the private key
        $acl.SetAccessRule($accessRule);

        # Write back the new acl
        Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop

        #Observe the access rights currently assigned to this certificate.
        get-acl $keyFullPath| fl
        ```

4. Repeat the steps above for each server certificate. You can also use these steps to install the client certificates on the machines that you want to allow access to the cluster.

## Create the secure cluster
After configuring the **security** section of the **ClusterConfig.X509.MultiMachine.json** file, you can proceed to [Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section to configure the nodes and create the standalone cluster. Remember to use the **ClusterConfig.X509.MultiMachine.json** file while creating the cluster. For example, your command might look like the following:

```
.\CreateServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab - AcceptEULA $true
```

Once you have the secure standalone Windows cluster successfully running, and have setup the authenticated clients to connect to it, follow the section [Connect to a secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) to connect to it. For example:

```
Csatlakozás ServiceFabricCluster - ConnectionEndpoint 10.7.0.4:19000 - KeepAliveIntervalInSec 10 - X509Credential - ServerCertThumbprint 057b9544a6f2733e0c8d3a60013a58948213f551 - FindType FindByThumbprint - FindValue 057b9544a6f2733e0c8d3a60013a58948213f551 StoreLocation értéke - CurrentUser - storename mezőt a
```

If you are logged on to one of the machines in the cluster, since this already has the certificate installed locally you can simply run the Powershell command to connect to the cluster and show a list of nodes:

```
Csatlakozás ServiceFabricCluster Get-ServiceFabricNode
```
To remove the cluster call the following command:

```
.\RemoveServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab
```
