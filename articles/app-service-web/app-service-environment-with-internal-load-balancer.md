<properties
    pageTitle="Létrehozásával és használatával egy belső terheléselosztó-alkalmazás szolgáltatás környezettel rendelkező |} Microsoft Azure"
    description="Hozhat létre, és egy ILB egy ASE használata"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Az alkalmazás-szolgáltatási környezetben egy belső terheléselosztó használata #

Az alkalmazás szolgáltatás Environments(ASE) funkció az Azure alkalmazás szolgáltatás olyan bővített konfigurációs lehetőséggel, amely nem érhető el a több elem bérlői bélyegzőket biztosítja prémium szolgáltatás lehetőség.  A ASE funkció lényegében az Azure virtuális Network(VNet) az Azure alkalmazás szolgáltatás üzembe helyezése.  Az alkalmazás szolgáltatás környezetekben, olvassa el a [Mi az, hogy az alkalmazás-szolgáltatási környezetben] által kínált lehetőségek megértése eléréséhez[ WhatisASE] dokumentációt.  Ha nem tudja, hogy egy VNet működő előnyeit, olvassa el az [Azure virtuális hálózati gyakran ismételt kérdések][virtualnetwork].  


## <a name="overview"></a>– Áttekintés ##


Egy ASE telepíthető, könnyen kezelhető internetes zárólap vagy a VNet IP-címet.  Az IP-cím beállításához VNet címre is telepíteni kell a ASE egy belső betöltés Balancer(ILB) együtt.  Amikor a ASE egy ILB van beállítva, adja meg a következő műveleteket:

- saját tartomány és altartomány.  Megkönnyíti, hogy a dokumentum feltételezi, hogy altartomány, de beállíthatja, hogy mindkét módon.  
- HTTPS-hez használt tanúsítvány.
- Az altartomány a DNS-kezelési.  


Ismét dolog, amit például végezheti el:

- a Host intranetes alkalmazáshoz, például a üzletági alkalmazások biztonságosan a felhőben, amely egy webhelyre vagy készült ExpressRoute VPN-webhelyen keresztül érheti el
- a Host alkalmazások a felhőben, amelyek nem jelennek meg a nyilvános DNS-kiszolgálók
- internetes elszigetelt kódmentes Apps alkalmazásokkal, amelyek az előtér-alkalmazások és biztonságos módon egyesíthető létrehozása


#### <a name="disabled-functionality"></a>Letiltott funkciók ####

Néhány dolgot, amely nem egy ILB ASE használata esetén.  Adott dolog, amit a következők:

- IPSSL használata
- IP-címek hozzárendelése az adott alkalmazás között
- vételi, és a tanúsítvánnyal az alkalmazás a portálon keresztül.  Szerezze be a tanúsítványok közvetlenül egy hitelesítésszolgáltató természetesen továbbra is, és vele együtt az alkalmazásokat, de nem az Azure portálon keresztül.


## <a name="creating-an-ilb-ase"></a>Egy ILB ASE létrehozása ##

Egy ILB ASE létrehozása nincs lényegesen eltér-ASE általában létrehozása.  A mélyebb vitafórum-ASE létrehozásával kapcsolatban, olvassa el [az alkalmazás-szolgáltatási környezetben létrehozása][HowtoCreateASE].  Egy ILB ASE létrehozása megegyezik egy VNet létrehozása ASE létrehozása során, vagy egy már meglévő VNet kijelölése között.  Egy ILB ASE létrehozása: 

1.  Az Azure-portálon válassza az **Új webhely -> + Mobile-alkalmazás szolgáltatási környezetben >**
2.  Jelölje ki azt az előfizetést
3.  Válasszon vagy hozzon létre egy erőforráscsoport
4.  Válasszon vagy hozzon létre egy VNet
5.  Alhálózat létrehozása, ha egy VNet kijelölése
6.  Jelölje ki a **virtuális hálózati/hely-> VNet konfigurációs** , és állítsa a virtuális típus belső
7.  Adja meg altartomány nevét (Ez lesz az használt-e ASE készült alkalmazások altartomány)
8.  Kattintson az OK gombra, és hozzon létre


![][1]


A virtuális hálózati lap belül van egy VNet konfiguráció lehetőséget.  Válassza a között egy virtuális külső vagy belső virtuális lehetővé.  Az alapértelmezett érték külső.  Ha van rá külső beállítása a ASE egy internetes elérhető virtuális fogja használni.  Ha bejelöli a belső, a ASE az IP-címet a VNet belül a egy ILB konfigurálhatók.  


Miután kiválasztotta a belső, az azt jelenti, hogy több IP-címek hozzáadása a ASE törlődik, és helyette meg kell adnia az altartomány a hajlamosnak.  Be-és egy külső virtuális ASE a ASE nevét használja az altartomány a adott ASE készült alkalmazások.  
Ha a ASE hívták ***contosotest*** és az alkalmazás, hogy ASE hívták meg ***mytest*** majd az altartomány a formátum ***contosotest.p.azurewebsites.net*** lennének, és ***mytest.contosotest.p.azurewebsites.net***URL-CÍMÉT, hogy az alkalmazás a következő lesz.  
Belső virtuális típusát állítja be, ha a ÁZIS neve nem használatos az altartomány a ASE számára.  Az altartomány explicit módon adja meg.  Ha a altartomány ***contoso.corp.net*** és annak, hogy ASE nevű ***timereporting*** elvégzett alkalmazás URL-CÍMÉT, hogy az alkalmazás ***timereporting.contoso.corp.net***lenne.


## <a name="apps-in-an-ilb-ase"></a>Egy ILB ASE az alkalmazások ##

Alkalmazás létrehozása egy ILB ASE megegyezik egy ASE alkalmazás létrehozása a szokásos módon.  

1. Az Azure portálon válassza az **Új webhely -> + Mobile -> webhely** vagy a **mobil** vagy az **API-alkalmazás**
2. Írja be az alkalmazás nevére
2. Jelölje ki az előfizetés
3. Válasszon vagy hozzon létre erőforráscsoport
4. Jelölje ki, vagy hozzon létre az alkalmazás szolgáltatás Plan(ASP).  Ha egy új ASP létrehozása a ASE jelöli ki a helyet, majd válassza ki a kívánt, létre kell hozni az ASP dolgozó készlet.  Az ASP létrehozásakor jelölje ki a ASE a helyet, valamint a dolgozó készletre.  Miután megadta az alkalmazás nevére jelenik meg, hogy az alkalmazás neve alatt a altartomány helyébe az altartomány a ASE.   
5. Jelölje be a létrehozott.  Akkor válassza a **PIN-kód irányítópult** jelölőnégyzetet, ha azt szeretné, hogy az alkalmazás az irányítópult megjelennek.  

![][2]


Az alkalmazás neve alatti altartomány nevét az altartomány a hajlamosnak megfelelően frissül a.  


## <a name="post-ilb-ase-creation-validation"></a>Bejegyzés ILB ASE létrehozási érvényesítése ##

Egy ILB ASE némileg eltérő mint a nem - ILB ASE.  A már jegyezni kezeléséhez, a saját DNS- és HTTPS-kapcsolatokat a szükséges saját tanúsítványt is.  


A ASE létrehozása után, láthatja, hogy a altartomány jeleníti meg a altartomány megadott és a **Beállítások** menü **ILB tanúsítvány**nevű új elem található.  Önaláírt tanúsítvány, így könnyebb HTTPS tesztelje a ASE jön létre.  A portál program tájékoztatja, hogy meg kell adnia a saját tanúsítvány HTTPS-, de ez kell rendelkeznie a altartomány az előbb tanúsítvány meghajtóra.  

![][3]


Ha egyszerűen próbál dolog, amit, és nem tudja, hogy miként hozhat létre egy tanúsítványt, az IIS MMC konzolhoz alkalmazás segítségével saját aláírt tanúsítvány létrehozása.  A már létrehozott .pfx fájlba exportálhatja, és töltse fel a felhasználói felület ILB tanúsítvány. Önaláírt tanúsítványt biztonságos webhely nyitja meg, amikor a böngésző, figyelmeztetést ad, hogy a webhely érik el nem biztonságos megakadályozhatják a tanúsítvány érvényesítése miatt.  Ha el szeretné kerülni, hogy a figyelmeztetés, amely megfelel a altartomány és a böngésző felismert megbízhatósági lánca megfelelően aláírt tanúsítvány szüksége.

![][6]

Ha azt szeretné, próbálja meg a saját tanúsítványok folyamat, és a HTTP és HTTPS hozzáférést a ASE tesztelése:

1.  Nyissa meg a ASE felhasználói felület ASE létrehozását követően **ASE-beállítások > ILB tanúsítványok ->**
2.  Tanúsítványfájl pfx kiválasztva adja meg a tanúsítvány ILB, és adja meg a jelszót.  Ebben a lépésben feldolgozása egy kis ideig tart, és az üzenetet, amelyet egy méretezési művelet folyamatban van meg fog jelenni.
3.  A ILB cím beszerzése a ASE (**ASE-Tulajdonságok > virtuális IP-cím ->**)
4.  Hozzon létre webalkalmazást ASE a létrehozása után 
5.  Hozzon létre egy virtuális, ha nem rendelkezik egy adott VNET (nem a ugyanahhoz az alhálózathoz a ASE vagy dolgokat szünet másként)
6.  Az altartomány DNS beállítása  Helyettesítő karakter használata a subdomain a DNS-ben, vagy néhány egyszerű vizsgálatok műveletek szerkesztheti a hosts fájl a virtuális web app nevének megadása virtuális IP-címére.  Ha a ASE volna altartomány nevét. ilbase.com, és elvégzett a web app mytestapp, hogy szeretné kell mytestapp.ilbase.com a címzett majd beállítása, hogy a hosts fájlban.  (Windows a hosts fájlt a van C:\Windows\System32\drivers\etc\)
7.  A virtuális használata a böngészőben, és válassza a http://mytestapp.ilbase.com (vagy a függetlenül a webes alkalmazás neve és a altartomány)
8.  Használata a böngészőben, hogy virtuális, és be kell fogadja el a biztonsági hiánya, ha egy önaláírt tanúsítvány használatával https://mytestapp.ilbase.com lépjen.  


A ILB IP-címe szerepel az a tulajdonságokat a virtuális IP-cím

![][4]


## <a name="using-an-ilb-ase"></a>Egy ILB ASE használatával ##

#### <a name="network-security-groups"></a>Hálózati biztonsági csoportok ####

Egy ILB ASE lehetővé teszi, hogy az alkalmazások hálózati elkülönítve, az alkalmazások nem érhető el vagy az interneten, még akkor is ismert.  Ez egy kiváló intranetes webhelyeken, például üzleti elhelyezésére.  Ha még akkor is korlátozhatja a hozzáférést szeretne további továbbra is használhatja hálózati biztonsági Groups(NSGs) a hálózaton szinten hozzáférés szabályozása. 


Ha szeretne további korlátozhatja a hozzáférést, akkor győződjön meg arról, hogy nem megszakítja a kommunikáció a ASE igénylő annak érdekében, hogy működjön, majd a NSGs használatával.  Annak ellenére, hogy a HTTP-/ HTTPS-hozzáférési csak az a ASE által használt ILB használatával a ASE továbbra is függ, hogy a VNet kívüli erőforrás.  Megtudhatja, milyen hálózati hozzáférés továbbra is szükséges jelenik meg a dokumentumot az adatokat a [Bejövő forgalom szabályozására az alkalmazás-szolgáltatási környezetben] [ ControlInbound] és a dokumentumot a [Hálózati konfigurációs részleteket az alkalmazás szolgáltatás környezetek készült ExpressRoute][ExpressRoute].  


A NSGs konfigurálása kell tudnia a ASE kezelése Azure által használt IP-címét.  Az IP-címet esetén is a kimenő IP-címét a ASE internetes kérelmek van.  Keresse meg az IP-cím nyissa meg a **Tulajdonságok-beállítások >** , és keresse meg a **Kimenő IP-címet**.  

![][5]


#### <a name="general-ilb-ase-management"></a>Általános ILB ASE kezelése ####

Egy ILB ASE kezelése megegyezik nagymértékben egy ASE általában kezelése.  Ha át kívánja méretezni a dolgozó készletek további ASP példányok és felfelé kezelheti a nagyobb adatmennyiségek HTTP-/ HTTPS-forgalom az előtér-kiszolgálókon méretezése felfelé szüksége.  Az általános információkat egy ASE, konfigurációját kezelésével kapcsolatban, olvassa el a [szolgáltatás alkalmazás környezet beállítása]a dokumentum[ASEConfig].  


A további felügyeleti cikkeket tanúsítványkezelés és a DNS-kezelési.  Kell beszerzése és HTTPS-ILB ASE létrehozását követően használt tanúsítvány feltöltése és helyére, még mielőtt lejárna.  Mivel az Azure alap a tartomány tulajdonosa elvégezheti a ASEs egy külső virtuális a tanúsítványok is szükséges.  Mivel az egy ILB ASE által használt altartomány lehet semmit, meg kell adnia a saját tanúsítvány HTTPS. 


#### <a name="dns-configuration"></a>DNS-konfigurációja ####

Egy külső virtuális használatakor Azure kezeli a DNS.  Azure DNS, amely egy nyilvános DNS automatikusan felkerül bármely a ASE létrehozott alkalmazásra.  Egy ILB ASE van saját DNS kezelése.  Egy adott altartomány, létre kell hoznia DNS A rekordokat, mutasson a ILB contoso.corp.net például címe:

    * 
    *.SCM ftp közzététele 


## <a name="getting-started"></a>Első lépések
Az összes cikk, és hogyan – a hangpostáján az alkalmazás-szolgáltatási környezetben érhetők el az [alkalmazás-szolgáltatási környezetben – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Első lépések az alkalmazás-szolgáltatási környezetben, lásd: [alkalmazás szolgáltatási környezetben – bevezetés][WhatisASE]

Az Azure alkalmazás szolgáltatás platform kapcsolatos további tudnivalókért lásd a [Azure alkalmazás szolgáltatás][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
