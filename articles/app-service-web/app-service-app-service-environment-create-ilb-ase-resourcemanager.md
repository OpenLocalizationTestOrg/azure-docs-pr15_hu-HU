<properties 
    pageTitle="Hogyan hozhat létre egy ILB ASE Azure erőforrás-kezelő sablonokkal |} Microsoft Azure" 
    description="Megtudhatja, hogy miként hozhat létre egy belső betöltés terheléselosztó ASE Azure erőforrás-kezelő sablonok használata." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>Hogyan hozhat létre egy ILB ASE Azure erőforrás-kezelő sablonok használata

## <a name="overview"></a>– Áttekintés ##
Alkalmazás környezetek helyett egy nyilvános virtuális virtuális hálózati belső címmel rendelkező hozhat létre.  A belső cím egy belső terheléselosztó (ILB) nevű Azure összetevő által biztosított.  Egy ILB ASE hozhat létre az Azure portálon.  Azt is létrehozhatók segítségével automatizálási Azure erőforrás-kezelő sablonok révén.  Az alábbiakban ismertetjük a lépéseket, és Azure erőforrás-kezelő sablonokkal egy ILB ASE létrehozásához szükséges szintaxis.

Három lépésből áll egy ILB ASE kibocsátása automatizálása részt:
1. Az alap ASE először egy belső betöltés terheléselosztó cím helyett egy nyilvános virtuális használatával virtuális hálózati jön létre.  Ebben a lépésben részeként legfelső szintű tartomány nevét a ILB ASE van rendelve.
2. Amikor létrejött a ILB ASE, SSL-tanúsítvány van feltöltve.  
3. A feltöltött SSL-tanúsítvány kifejezetten az "alapértelmezett" SSL-tanúsítvány, a ILB ASE rendelt.  Az SSL-tanúsítványt fogja használni, az SSL-alapú forgalmat a ILB ASE-alkalmazásokat, ha az alkalmazások használata a közös gyökértartomány a ASE (pl. https://someapp.mycustomrootcomain.com) rendelt foglalkozik

## <a name="creating-the-base-ilb-ase"></a>A következő ILB ASE létrehozása ##
Egy példa Azure erőforrás-kezelő sablont, és a kapcsolódó paraméterek fájl érhetők el a GitHub [Itt][quickstartilbasecreate].

A paraméterek a *azuredeploy.parameters.json* fájl többsége közös egyaránt ILB ASEs, valamint egy nyilvános virtuális kötött ASEs létrehozása.  A lista alatti hívásokat a különleges megjegyzés határozza meg, vagy amelyeket egyediek, egy ILB ASE létrehozása:


- *interalLoadBalancingMode*: A legtöbb esetben ez 3, ami azt jelenti, HTTP-/ HTTPS-forgalom portokra 80 és 443-as és az ellenőrzés adatok csatorna portok való navigációval a ASE, kattintson az FTP-szolgáltatás által beállított lesz kötni egy virtuális hálózati belső cím kiosztott ILB.  Ha ezt a tulajdonságot inkább értéke 2, csak az FTP-szolgáltatás kapcsolatos, portok (vezérlők, és a adatok csatornák) lesznek kötve ILB címre a HTTP-/ HTTPS-forgalmat a nyilvános virtuális bekapcsolva marad, amíg.
-  *dnsSuffix*: ezt a paramétert jogokat kap az ASE alapértelmezett legfelső szintű tartomány határozza meg.  Az Azure alkalmazás szolgáltatás nyilvános változata az alapértelmezett legfelső szintű tartomány összes web Apps alkalmazások *azurewebsites.net*.  Azonban mivel egy ILB ASE belső ügyfél virtuális hálózatába, nem értelme a nyilvános szolgáltatás alapértelmezett legfelső szintű tartomány használatára.  Ehelyett egy ILB ASE alapértelmezett gyökértartományt van ilyesmire lehetőség a vállalat belső virtuális hálózaton belül használatra kell rendelkeznie.  Például a hipotetikus Contoso Corporation használhat *belső contoso.com* alapértelmezett legfelső szintű tartomány-alkalmazást, amely elsősorban csak akkor felbontási és könnyen kezelhető Contoso virtuális hálózaton belül. 
-  *ipSslAddressCount*: ezt a paramétert automatikusan származik a *azuredeploy.json* fájlt a 0 értéket, mivel ILB ASEs csak egyetlen ILB címet.  Nincsenek egy ILB ASE az explicit IP-SSL címek, így az IP-SSL cím készlet-ILB ASE nulla értékre kell állítani, egyéb esetben a kiépítési hibaüzenet akkor fordul elő. 

Miután a *azuredeploy.parameters.json* fájl esetében egy ILB ASE van töltve, a ILB ASE majd hozhatók létre használja az alábbi Powershell-kódtöredéket.  Módosítsa a fájl elérési utak megfelelően, az erőforrás-kezelő Azure sablonfájlok helyét a számítógépen.  Ne feledje a Azure erőforrás-kezelő telepítési nevét, és erőforrás-csoport nevére a saját értéket ad meg.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Miután az Azure erőforrás-kezelő sablon elküldése megnyílik a létrehozandó ILB ASE néhány órát.  Ha befejeződött a létrehozási, a ILB ASE megjelenik a portálon UX a listában, az alkalmazás-szolgáltatási környezetben az előfizetéshez, amelyen a telepítés.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>Feltöltés és az "Alapértelmezett" SSL-tanúsítvány beállítása ##

Amikor létrejött a ILB ASE, SSL-tanúsítvány kell társítani a ASE az "alapértelmezett" SSL-tanúsítvány használható alkalmazások SSL-kapcsolat.  A Contoso Corporation kitalált példa folytatása, ha a ASE alapértelmezett DNS utótag ( *belső contoso.com*), majd a *https://some-random-app.internal-contoso.com* kapcsolat szükséges SSL-tanúsítvány érvényes **.internal-contoso.com*. 

Vannak olyan többféle módon beszerzése érvényes SSL-tanúsítvány többek között a belső hitelesítésszolgáltatók vásárolt szóló tanúsítványt egy külső kibocsátó és önaláírt tanúsítvány használatával.  Az SSL-tanúsítvány forrását, függetlenül a következő tanúsítvány attribútumok kell megfelelően beállítva:

- *Subject*: meg kell az attribútum **.your-legfelső szintű-tartomány-here.com*
- *Alternatív tulajdonosneve*: attribútum tartalmaznia kell mindkét * *.your-legfelső szintű-tartomány-here.com*, és * *.scm.your-legfelső szintű-tartomány-here.com*.  A második tételt oka, hogy az egyes-alkalmazással társított SCM/Kudu webhely SSL-kapcsolatot történik-e az űrlap *your-app-name.scm.your-root-domain-here.com*címének használatával.

Egy érvényes SSL-tanúsítványt az aktuális, a két további előkészítő lépésekre van szükség.  Az SSL-tanúsítvány kell lennie a konvertált/egy fájlba lettek mentve .pfx.  Ne feledje, hogy a .pfx fájl kell hozzáadni az összes köztes, majd a legfelső szintű tanúsítványok és is van szüksége a jelszóval titkosított.

Kattintson a eredő .pfx fájl kell base64 karakterlánc alakított, mert az SSL-tanúsítvány feltöltődnek erőforrás-kezelő Azure-sablon segítségével.  Erőforrás-kezelő Azure-sablonok szövegfájlokból, mivel a .pfx fájl kell base64 karakterlánc alakított, így fel lehet venni, mint a sablon egy paraméter.

Az alábbi Powershell kódrészletet látható, önaláírt tanúsítvány létrehozása, a tanúsítvány exportálása .pfx fájl, mint a .pfx fájl átalakítása egy base64 címként kódolt karakterláncot, majd menti a base64 címként kódolt karakterláncot egy külön fájlba.  A Powershell-kód base64 kódolás volt a [Powershell-parancsfájlokat Blog]az igazítani[examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     
    
    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")
    
Miután az SSL-tanúsítvány sikeresen létre és egy base64 kódolású konvertálva karakterlánc, például Azure erőforrás-kezelő sablon GitHub [az alapértelmezett SSL-tanúsítvány beállítása] az[ configuringDefaultSSLCertificate] is használható.

A paraméterek a *azuredeploy.parameters.json* fájlt az alábbiak:

- *appServiceEnvironmentName*: a ILB ASE épp konfigurálja a nevét.
- *existingAseLocation*: szöveges karakterlánc, amely az Azure régió, ahol a ILB ASE telepítették.  Például: "Dél központi US".
- *pfxBlobString*: A based64 címként kódolt karakterláncot ábrázolása a .pfx fájl.  A korábban bemutatott kódrészletet használ, volna másolja a található "exportedcert.pfx.b64" karakterlánc és illessze be a program a *pfxBlobString* attribútum.
- *jelszó*: A jelszót, amellyel biztonságossá a .pfx fájl.
- *certificateThumbprint*: A tanúsítványt ujjlenyomat.  Ha ezt az értéket lekérése Powershell (pl. *$certificate. Ujjlenyomat* a korábbi kódtöredéket az), használhatja a számot, mint-van.  Ha az érték a Windows a tanúsítvány párbeszédpanel, azonban ne felejtse el távolítsa el a felesleges szóközöket.  A *certificateThumbprint* hasonlóan kell kinéznie: AF3143EB61D43F6727842115BB7F17BBCECAECAE
- *certificateName*: identitás a tanúsítvány használatával egy rövid karakterlánc azonosítója, a saját kiválasztása.  A nevét az SSL-tanúsítvány, amely *Microsoft.Web/certificates* entitás használható az erőforrás-kezelő Azure egyedi azonosító részeként.  A következő utótaggal nevét **kell** vége: \_yourASENameHere_InternalLoadBalancingASE.  Az utótag azt jelzi, hogy a tanúsítvány amellyel biztonságossá tehető az ILB engedélyező ASE a portálon használja.


Rövidített példa *azuredeploy.parameters.json* nézhet ki:


    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

A *azuredeploy.parameters.json* fájl kitöltése után az alapértelmezett SSL-tanúsítvány konfigurálhatók az alábbi Powershell-kódtöredéket.  Módosítsa a fájl elérési utak megfelelően, az erőforrás-kezelő Azure sablonfájlok helyét a számítógépen.  Ne feledje a Azure erőforrás-kezelő telepítési nevét, és erőforrás-csoport nevére a saját értéket ad meg.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Miután az erőforrás-kezelő Azure-sablon elküldése időt vesz igénybe nagyjából negyven perc perc ASE előtér / a módosítás érvénybe léptetéséhez.  Például a két első részek használata egy alapértelmezett méretű ASE, a sablon percig is eltarthat körülbelül egy óra és húsz befejezéséhez.  A sablon futása közben a ASE nem tudja méretezett.  

Ha befejeződött a sablont, a HTTPS-alkalmazásokat a ILB ASE is elérhető, és használja az alapértelmezett SSL-tanúsítvány a kapcsolatok fog vonatkozni.  Az alapértelmezett SSL-tanúsítvány lesz, amikor a ILB ASE-alkalmazásokat az alkalmazás nevét, valamint az alapértelmezett hostname (állomásnév) együttes használatával foglalkozik.  Példa *https://mycustomapp.internal-contoso.com* használja az alapértelmezett az SSL-tanúsítvány **.internal-contoso.com*.

Azonban a nyilvános több bérlői szolgáltatást futtató alkalmazásokat, hasonlóan a fejlesztők is is beállíthatja az egyéni állomásnevek az egyes alkalmazások, és állítson be egyedi SNI SSL-tanúsítvány kötések az egyes alkalmazások.  


## <a name="getting-started"></a>Első lépések

Az alkalmazás-szolgáltatási környezetben, című cikkben ismerkedhet [alkalmazás szolgáltatási környezetben – bevezetés](app-service-app-service-environment-intro.md)

Az összes cikk, és hogyan – a hangpostáján az alkalmazás-szolgáltatási környezetben érhetők el az [alkalmazás-szolgáltatási környezetben – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 
 
