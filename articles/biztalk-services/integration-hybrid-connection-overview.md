<properties
    pageTitle="Hibrid az adatkapcsolatok áttekintése |} Microsoft Azure"
    description="Tudjon meg többet a hibrid kapcsolatok, a biztonság, a TCP-portokat és a támogatott konfigurációk. MABS, WABS."
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ccompy"/>


# <a name="hybrid-connections-overview"></a>Hibrid az adatkapcsolatok áttekintése
Hibrid kapcsolatok – bevezetés felsorolja a támogatott beállításokat, és a szükséges TCP-portokat sorolja fel.


## <a name="what-is-a-hybrid-connection"></a>Mi az hibrid kapcsolat

Hibrid kapcsolatok újdonsága Azure BizTalk szolgáltatások. Hibrid kapcsolatok Azure App szolgáltatásban (korábbi nevén webhelyeket) a Web Apps alkalmazások funkció és a Mobile-alkalmazások funkció Azure (korábbi nevén Mobile szolgáltatások) alkalmazás szolgáltatásban csatlakozás helyszíni erőforrásokhoz a tűzfal mögött egyszerű és kényelmes lehetőséget biztosítanak.

![Hibrid kapcsolatok][HCImage]

Többek között a hibrid kapcsolatok előnyöket nyújtja:

- Web Apps alkalmazások és a Mobile-alkalmazások hozzáférhetnek a meglévő helyszíni adatok és szolgáltatások biztonságosan.
- Több Web Apps alkalmazások vagy Mobile-alkalmazások megoszthatnak egy helyszíni erőforrás eléréséhez hibrid kapcsolatot.
- A hálózat eléréséhez szükséges minimális TCP-portok.
- Hibrid-kapcsolatot használó alkalmazások csak közzétett bizonyos helyszíni erőforráshoz hozzáférni a hibrid kapcsolaton keresztül.
- Bármely, amely egy statikus portot használja, például SQL Server, a MySQL, a HTTP webes API-hoz és a legtöbb egyéni webszolgáltatásokhoz helyszíni erőforrás csatlakozhat.

    > [AZURE.NOTE] TCP-alapú szolgáltatások (például az FTP passzív vagy bővített passzív üzemmódban) dinamikus portokat használó jelenleg nem támogatott. LDAP még nem támogatott. LDAP egy statikus portot használja, de azt is használhatja UDP. Emiatt nem támogatja.

- A Web Apps (.NET PHP, Java, Python, Node.js) és a Mobile-alkalmazások (Node.js, .NET) által támogatott összes keretek használható.
- Web Apps alkalmazások és a Mobile-alkalmazások hozzáférhetnek a helyszíni erőforrások pontosan megegyező módon, mintha az interneten vagy Mobile alkalmazásban található, a helyi hálózaton. Ha például az azonos csatlakozási karakterlánc a helyszíni is használható a Azure.


Hibrid kapcsolatok is adja meg a vállalati rendszergazdák a vezérlő és betekintést kap abba, hogy az elérhető hibrid alkalmazásai, köztük a vállalati erőforrások:

- Using csoportházirend-beállításai, a rendszergazdák hibrid kapcsolatok engedélyezése a hálózaton és erőforrások hibrid alkalmazások által is elérhető is kijelölhet.
- A vállalati hálózaton esemény és naplózási naplók betekintést kap abba, hogy a hibrid kapcsolatok elérhető erőforrások biztosítanak.


## <a name="example-scenarios"></a>Példák

Hibrid kapcsolatok a következő keretrendszer és az alkalmazás kombinációk támogatja:

- A .NET keretrendszer access SQL Server
- A .NET keretrendszer hozzáférést WebClient HTTP-/ HTTPS-szolgáltatások
- SQL Server, MySQL PHP eléréséhez
- SQL Server-, MySQL- vagy Oracle Java eléréséhez
- Java hozzáférést a HTTP-/ HTTPS-szolgáltatásokhoz

Amikor hibrid kapcsolatok szeretne elérni a helyszíni SQL Server, vegye figyelembe a következőket:

- SQL Server-Express nevű példányok kell beállítania, hogy statikus portok használatát. Alapértelmezés szerint SQL Express nevű példányok használata a dinamikus portok.
- SQL Server-Express alapértelmezett példányok egy statikus portot használja, de a TCP engedélyezve kell lennie. TCP alapértelmezés szerint nincs engedélyezve.
- Fürtképzés vagy rendelkezésre állási csoportok, használata esetén a `MultiSubnetFailover=true` mód jelenleg nem támogatott.
- A `ApplicationIntent=ReadOnly` jelenleg nem támogatott.
- Lehet, hogy SQL-hitelesítés szükséges a végpont engedélyt módszere az Azure alkalmazás és a helyszíni SQL server által támogatott.


## <a name="security-and-ports"></a>Biztonság és portok

Hibrid kapcsolatok megosztott Access-aláírás (Társítások) hitelesítés használatával biztonságos a kapcsolatokat az Azure-alkalmazások és a helyszíni hibrid kapcsolatkezelő a hibrid kapcsolatra. Az alkalmazás és a helyszíni hibrid kapcsolatkezelő billentyűk külön kapcsolat jön létre. Kapcsolat billentyűk is közzétételének, és önállóan visszavonva.

Az alkalmazások és a helyszíni hibrid kapcsolatkezelő billentyűk zökkenőmentes és biztonságos megoszlása a hibrid kapcsolatok szükséges.

Lásd: [hibrid adatkapcsolatok létrehozása és kezelése](integration-hybrid-connection-create-manage.md).

Az *alkalmazás engedély külön értendő a hibrid kapcsolatot*. Bármely megfelelő hitelesítési módszer használható. A hitelesítési módszer attól függ, hogy a végpontok közötti engedélyezési módszerek az Azure felhő és a helyszíni összetevők támogatott. Az Azure-alkalmazásokat, például egy helyszíni SQL Server fér hozzá. Ebben az esetben SQL engedélyezési lehet, hogy a hitelesítési módszer, amely támogatja a végpontok közötti.

#### <a name="tcp-ports"></a>TCP-portok
Hibrid kapcsolatok csak kimenő TCP- vagy HTTP-kapcsolatot, a személyes hálózatról igényelnek. Nem kell nyissa meg bármelyik tűzfalportokat vagy a hálózati külső konfiguráció engedélyezése a hálózatba minden bejövő kapcsolat módosítása.

Az alábbi TCP-portokat hibrid kapcsolat által használt:

Port | Miért van szükség
--- | ---
9350 - 9354 | Ezeket a portokat adatátvitel segítségével. A szolgáltatás Bus továbbítási kezelő ellenőrzi a port 9350 annak megállapításához, hogy a TCP-kapcsolat érhető el. Ha elérhető, majd azt feltételezi, hogy port 9352 is elérhető. Adatforgalom port 9352 áttekintést. <br/><br/>Engedélyezze a kimenő kapcsolatokat portokhoz.
5671 | Adatforgalom port 9352 használatakor port 5671 szolgál a vezérlő csatorna. <br/><br/>Engedélyezze a kimenő kapcsolatokat a porthoz.
80, 443-as | Ezeket a portokat bizonyos adatok kérések Azure segítségével. Ha 9352 és 5671 nem használható, *majd* a 80 és 443-as port is használható adatátvitel és a vezérlő csatorna alaplekérdezések portok.<br/><br/>Engedélyezze a kimenő kapcsolatokat portokhoz. <br/><br/>**Megjegyzés:** Ezek a visszalépési portokat a TCP-portok helyett használandó nem ajánlott. A HTTP/WebSocket adatcsatornát protokolljaként natív TCP helyett használható. A eredményezhet alsó teljesítményét.



## <a name="next-steps"></a>Következő lépések

[Hibrid adatkapcsolatok létrehozása és kezelése](integration-hybrid-connection-create-manage.md)<br/>
[Azure Web Apps alkalmazások csatlakoztatása egy helyszíni erőforrás](../app-service-web/web-sites-hybrid-connection-get-started.md)<br/>
[Csatlakozás helyszíni SQL Server-Azure-webalkalmazásból](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/>


## <a name="see-also"></a>Lásd még:

[A Microsoft Azure BizTalk szolgáltatások kezelésére szolgáló REST API-t](http://msdn.microsoft.com/library/azure/dn232347.aspx)
[BizTalk szolgáltatások: kiadásai diagram](biztalk-editions-feature-chart.md)<br/>
[Hozzon létre egy BizTalk szolgáltatás Azure portál használatával](biztalk-provision-services.md)<br/>
[BizTalk szolgáltatások: Irányítópult, a Monitor és a méret lapon](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png
