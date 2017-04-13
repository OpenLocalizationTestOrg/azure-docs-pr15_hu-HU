<properties 
    pageTitle="GEO elosztott méretezés a alkalmazás szolgáltatási környezetben" 
    description="Megtudhatja, hogy miként vízszintesen méretezni alkalmazások geo-eloszlás használatával forgalom felettes, és az alkalmazás-szolgáltatási környezetben." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="stefsch"/>   

# <a name="geo-distributed-scale-with-app-service-environments"></a>GEO elosztott méretezés a alkalmazás környezetek

## <a name="overview"></a>– Áttekintés ##
Nagyon magas skála igénylő helyzeteket túllépheti a rendelkezésre álló egy egyetlen az alkalmazás-példányhoz számítási erőforráshoz kapacitás.  A szavazás alkalmazások, sportesemények, és a közvetített szórakozásra események példák összes nagyon magas skála igénylő helyzeteket. Magas skála követelményeknek is teljesülniük a több az alkalmazások telepítésének egyetlen terület és területek között történik szélső betöltés követelmények kezelni az alkalmazásokat, ki vízszintesen méretezéssel.

Alkalmazás környezetek egy ideális platform a vízszintes méret ki.  Egyszer alkalmazás szolgáltatási környezetben konfigurációs ki van jelölve, amelyek támogatják a ismert kérelem díjának fejlesztők telepítheti a további alkalmazás szolgáltatási környezetben "cookie-k eltávolító" módon kívánt csúcs betöltés kapacitású eléréséhez.

Tegyük fel például egy alkalmazás szolgáltatás környezet konfigurációja futó alkalmazás teszteléssel 20K kérések (RPS) másodpercenként kezelésére.  Ha a kívánt csúcs betöltés kapacitás 100 KB RPS, majd öt (5) alkalmazás környezetek létrehozható és annak érdekében, hogy az alkalmazás képes kezelni a maximális tervezett betöltés beállítva.

Felhasználók általában access-alkalmazások egyéni (vagy méltóság) tartomány használata, mivel a fejlesztők kell alkalmazás kéréseket elosztása minden alkalmazás szolgáltatási környezetben előfordulása lehetőséget.  Az egyéni tartomány használata az [Azure forgalom Manager profil]feloldásához ehhez remekül[AzureTrafficManagerProfile].  A forgalom Manager profil beállítható úgy, hogy az összes az egyes alkalmazás környezetek mutasson.  Forgalom Manager terjesztésével ügyfelek összes a alkalmazás szolgáltatási környezetben, a terheléselosztás a forgalom Manager profil beállításai alapján automatikusan kezeli.  Ez a módszer akkor működik, függetlenül attól, hogy az összes a alkalmazás szolgáltatási környezetben egyetlen Azure régióban rendszerbe, vagy ha rendszerbe világszerte több Azure területek között.

Ezenkívül ügyfelek access-alkalmazások között a méltóság tartomány, mivel a felhasználókat, tudomása alkalmazást futtató alkalmazás környezetek számát.  Eredményt fejlesztők számára is gyorsan és egyszerűen hozzáadása és eltávolítása, alkalmazás környezetek megfigyelt forgalom betöltés alapján.

Az alábbi koncepciódiagram támogatása három alkalmazás szolgáltatás környezetekben egy egyetlen régión belüli ki vízszintesen méretezett alkalmazás ábrázolja.

![Magyarázó rész architektúra][ConceptualArchitecture] 

Ez a témakör a többi végigvezeti a minta alkalmazás használatával több alkalmazás környezetek egy elosztott topológia beállításának lépései.

## <a name="planning-the-topology"></a>A topológia megtervezése ##
Mielőtt meg egy megosztott alkalmazás helyigénye épület, könnyebben időszakokra néhány darab információt tartalmaznak.

- **Az alkalmazást az egyéni tartomány:**  Mi az, hogy az egyéni tartománynevet, amelyekkel a felhasználók elérni az alkalmazás?  A minta alkalmazás az egyéni tartománynevet *www.scalableasedemo.com*
- **Forgalom Manager tartomány:**  A tartománynév kell választják ki egy [Azure forgalom Manager profil]létrehozása[AzureTrafficManagerProfile].  Ez a név egy tartomány tétel forgalom kezelő által kezelt rögzítése *trafficmanager.net* utótaggal összevonni.  A minta alkalmazáshoz a választott neve *méretezhető ase bemutatása*.  A teljes tartománynevet, amelyek a forgalom kezelő által felügyelt eredményeként a *méretezhető ase demo.trafficmanager.net*.
- **Az alkalmazás helyigénye méretezés vonatkozó stratégia:**  Lesz az alkalmazások helyigénye legyen elosztva egyetlen területen több alkalmazás szolgáltatási környezetben?  Több területre?  Mix – és – egyező mindkét módszerekkel?  Döntés a várakozásoknak, ahonnan ügyfél forgalom származnak fog, valamint a többi az alkalmazás támogatja a háttéradatbázist infrastruktúra méretezheti mennyire kell alapján.  Például az állapot nélküli 100 %-os alkalmazásokban, az alkalmazás nagymértékben megadhat egy Azure régió, szorozva az alkalmazás környezetek több Azure területek között rendszerbe több alkalmazás környezetek kombinációjával.  15 + nyilvános Azure régióban elérhető közül választhat, az ügyfelek valóban egy világszerte hyper skála alkalmazások helyigénye hozhat létre.  A minta alkalmazáshoz használható ez a cikk három alkalmazás környezetek létrehozott egy egyetlen Azure régióban (Dél központi US).
- **Elnevezési a alkalmazás szolgáltatási környezetben:**  Minden alkalmazás szolgáltatási környezetben van szükség egy egyedi nevet.  Egy vagy két alkalmazás környezetek túl célszerű, hogy az egyes alkalmazás szolgáltatási környezetben azonosításához elnevezni.  A minta alkalmazáshoz használt egy egyszerű elnevezésük is egységes.  A három alkalmazás szolgáltatási környezetben azoknak a *fe1ase* *fe2ase*és *fe3ase*.
- **Elnevezési alkalmazások:**  Az alkalmazás több példány telepítve lesz, mivel a nevét a telepített alkalmazás minden példányában van szükség.  Egy kis ismert, de nagyon hasznos a alkalmazás szolgáltatás környezetek funkció, hogy az azonos alkalmazás nevére támogatása több alkalmazás szolgáltatás környezetekben is használható.  Minden alkalmazás szolgáltatási környezetben van egy egyedi tartomány utótag, mivel a fejlesztők is válassza a pontos azonos alkalmazás neve minden környezetben.  A fejlesztők van például alkalmazások nevű az alábbiak szerint: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*és stb.  A minta alkalmazáshoz Bár minden alkalmazás példány tartalmaz egy egyedi nevet.  Az alkalmazás példány használt megkülönböztetik a kis *webfrontend1* *webfrontend2*és *webfrontend3*.


## <a name="setting-up-the-traffic-manager-profile"></a>A forgalom Manager profil beállítása ##
Miután alkalmazás több példányban a több alkalmazás szolgáltatási környezetben van telepítve, az egyes alkalmazás példányok forgalom Manager lehet regisztrálni.  A minta alkalmazáshoz forgalom kezelőjének profil valamelyik, az alábbi telepített alkalmazás példányok is átirányíthatja az ügyfelek *méretezhető ase demo.trafficmanager.net* van szükség:

- **webfrontend1.fe1ase.p.azurewebsites.net:**  A minta alkalmazást, az alkalmazás első szolgáltatási környezetben rendszerre telepíthető példánya.
- **webfrontend2.fe2ase.p.azurewebsites.net:**  A minta alkalmazás, a második alkalmazás szolgáltatási környezetben rendszerre telepíthető példánya.
- **webfrontend3.fe3ase.p.azurewebsites.net:**  A minta alkalmazás, a harmadik alkalmazás szolgáltatási környezetben rendszerre telepíthető példánya.

A több Azure alkalmazás szolgáltatás végpontok, az **azonos** Azure régió, a futó regisztrálni legegyszerűbben az [Azure erőforrás-kezelő forgalom kezelő támogatási]Powershell[ARMTrafficManager].  

Első lépésként az Azure forgalom Manager profil létrehozása.  Az alábbi kód jeleníti meg, hogyan a profilt a minta alkalmazáshoz készült:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

Figyelje meg, hogyan lett beállítva a *RelativeDnsName* paraméter *scalable ase bemutatása*.  Ez az a tartomány neve *méretezhető ase demo.trafficmanager.net* létrehozott és forgalom Manager profil társított módját.

A *TrafficRoutingMethod* paramétert a terheléselosztási házirend forgalom Manager húzza szét a felhasználói betöltés végig az elérhető végpontok annak segítségével határozza meg.  Ebben a példában a *Weighted* módot választotta.  Ez a regisztrált alkalmazás végpontok alapján a relatív súlyok minden végpontra társított összes folyamatban terjeszteni felhasználói kérések eredményez. 

Azzal a profillal, létrehozott minden alkalmazás példánya bekerül a profil natív Azure végpontjának.  Az alábbi kód beolvassa az egyes előtér webalkalmazás hivatkozást, és majd összeadja a minden alkalmazás formájában a *TargetResourceId* paraméter egy forgalom Manager végpontjának.


    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10
    
    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile
    
Figyelje meg, hogyan van egy hívás *Hozzáadása-AzureTrafficManagerEndpointConfig* minden egyes alkalmazás előfordulásra vonatkozóan.  Az egyes Powershell-parancsot a *TargetResourceId* paraméter hivatkozik, a három telepített alkalmazás példányok közül.  A forgalom Manager profil a betöltés húzza szét regisztrált a profilban lévő összes három végponton keresztül.

A három végpontok írja be az azonos értéket (10) a *Vastagság* paraméter.  Ennek eredményeképpen forgalom Manager elterjedésének felhasználói kérések minden három alkalmazás példányára viszonylag terhelést kialakítani. 


## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>Az alkalmazás egyéni tartományt a forgalom Manager tartományban mutató kapcsolatokat. ##
A végleges szükséges lépésként, mutasson az egyéni tartományt az alkalmazás a forgalom Manager tartományban.  A minta alkalmazás Ez azt jelenti, *www.scalableasedemo.com* mutató *scalable ase demo.trafficmanager.net*elemre.  Ezt a lépést kell elvégezni a tartományregisztrálónál, amely az egyéni tartomány kezeli.  

A tartományregisztráló tartomány-kezelő eszközök használ, a CNAME rekordok igények hozható létre, amely mutat, az egyéni tartományt a forgalom Manager tartományban.  A következő ábrán látható példa ezt a CNAME konfigurációt néz ki:

![Egyéni tartomány CNAME][CNAMEforCustomDomain] 

Bár nem vonatkozik a jelen témakör, ne feledje, hogy minden egyes app-példány van szüksége az egyéni tartományát, valamint az általa regisztrált.  Egyéb esetben kérelmének teszi, hogy egy app példány, és az alkalmazás nem rendelkezik az egyéni tartomány regisztrált az alkalmazást, ha a kérelem sikertelen lesz.  

Ebben a példában az egyéni tartomány *www.scalableasedemo.com*, és minden alkalmazás példány van társítva az egyéni tartomány.

![Egyéni tartomány][CustomDomain] 

Egy recap, az egyéni tartomány regisztrációja az Azure alkalmazás szolgáltatás-alkalmazásokhoz, az alábbi cikkben talál a [Regisztrálás az egyéni tartományok][RegisterCustomDomain].

## <a name="trying-out-the-distributed-topology"></a>A normális eloszlású topológia ki szeretné próbálni ##
A befejezési a forgalom Manager és a DNS-konfigurációs eredménye, hogy a kérelem *www.scalableasedemo.com* fog flow között a következő sorrendben:

1. Böngészőben vagy eszköz láthatóvá válik a *www.scalableasedemo.com* DNS keresés
2. A CNAME-bejegyzést a tartományregisztrálónál hatására a DNS-ben a Azure forgalom Manager átirányítását.
3. A DNS-keresés *méretezhető ase demo.trafficmanager.net* szemben az Azure forgalom Manager DNS-kiszolgálók egyike történik.
4. A terheléselosztási házirend (a *TrafficRoutingMethod* paramétert a forgalom Manager profil létrehozásakor korábban használt) megfelelően forgalom Manager fog válasszon egyet a beállított végpontok és végpontra teljes Tartománynevét térjen vissza a a böngészőben vagy az eszközön.
5.  Mivel a végpont teljes Tartománynevét az alkalmazás-szolgáltatási környezetben futó alkalmazás példány URL-címét, a böngészőben vagy eszközön megkérdezi, a Microsoft Azure DNS-kiszolgáló teljes Tartománynevét úgy, hogy IP-címet. 
6. A böngésző vagy eszköz a HTTP/S kérelem küld IP-címére.  
7. A kérelem érkezik, a alkalmazás szolgáltatási környezetben futó app-példányok egyikén.

Konzol az alábbi képen a minta alkalmazást futtató egyik a három példa alkalmazás szolgáltatási környezetben (ebben az esetben a második három alkalmazás szolgáltatás környezet) alkalmazás példányhoz sikeresen megoldása egyéni tartományához tartozó DNS keresés:

![DNS-keresés][DNSLookup] 

## <a name="additional-links-and-information"></a>További hivatkozások és információk ##
Az összes cikk, és hogyan – a hangpostáján az alkalmazás-szolgáltatási környezetben érhetők el az [alkalmazás-szolgáltatási környezetben – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

A Powershell [Azure erőforrás-kezelő forgalom kezelő támogatási]dokumentáció[ARMTrafficManager].  

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]:  https://azure.microsoft.com/documentation/articles/traffic-manager-manage-profiles/
[ARMTrafficManager]:  https://azure.microsoft.com/documentation/articles/traffic-manager-powershell-arm/
[RegisterCustomDomain]:  https://azure.microsoft.com/en-us/documentation/articles/web-sites-custom-domain-name/


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
