<properties
   pageTitle="Logika alkalmazások telepítése a helyszíni adatok átjáró |} Microsoft Azure"
   description="Információ a helyszíni adatok átjáró telepítési összefüggés-alkalmazásban."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="install-the-on-premises-data-gateway-for-logic-apps"></a>Az átjáró a helyszíni összefüggés-alkalmazások telepítése

## <a name="installation-and-configuration"></a>Telepítési és konfigurálási

### <a name="prerequisites"></a>Előfeltételek

Minimális:

* .NET 4.5 keretrendszer
* Windows 7 vagy Windows Server 2008 R2 64 bites verzióját (vagy újabb)

Javasolt:

* 8 core Processzor
* 8 GB memóriát
* 64 bites verzióját a Windows 2012 R2 (vagy újabb)

Kapcsolódó szempontok

* Az átjáró nem telepíthető a tartományvezérlőnek.
* Az átjárók számítógépet használ, például laptopon, előfordulhat, hogy ki van kapcsolva, alvó, hogy ne telepítése vagy nem csatlakozik az internethez, mert az átjáró nem futtathatók bármely olyan körülmények között. Ezeken kívül átjáró teljesítmény érheti előfordulhat, hogy a vezeték nélküli hálózaton.

### <a name="install-a-gateway"></a>Az átjáró telepítése

A [telepítő az helyszíni átjáró itt](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409)elérheti.

Adja meg a **helyszíni adatok átjáró** a mód, jelentkezzen be a munkahelyi vagy iskolai fiókjával, és kattintson vagy adjon meg egy új átjárót vagy áttelepítése, visszaállítása vagy egy meglévő átjáró átvétele.

* Adjon meg egy átjárót, írja be a **nevét** , és a **helyreállítási billentyűt**, majd kattintson vagy koppintson a **konfigurálása**.

    Legalább nyolc karaktert tartalmazó kulcsról adja meg, és a biztonságos helyen tartani. Ha szeretné áttelepíteni, visszaállítása vagy az átjáró átvétele szüksége lesz az e billentyűt.

* Szeretné áttelepíteni, visszaállítása vagy egy meglévő átjáró átvétele, adja meg a helyreállítási billentyűt az átjáró létrehozásakor megadott.

### <a name="restart-the-gateway"></a>Indítsa újra az átjáró

Az átjáró Windows szolgáltatásként, és bármely más Windows szolgáltatás, a elindítása és leállítása, több módon. Ha például nyisson meg egy parancssort, emelt engedélyekkel az átjárót futtató számítógépen, és futtassa az alábbi parancsok egyikét:

* A szolgáltatás leállítása, ez a parancs futtatása:

    `net stop PBIEgwService`

* Indítsa el a szolgáltatást, hogy ez a parancs futtatása:

    `net start PBIEgwService`

### <a name="configure-a-firewall-or-proxy"></a>A tűzfal vagy proxykiszolgáló beállítása

Adja meg az átjáró proxy adatokat, akkor olvassa el [a proxykiszolgáló beállításainak](https://powerbi.microsoft.com/en-us/documentation/powerbi-gateway-proxy/)olvashat.

Ellenőrizheti, hogy a tűzfal vagy proxykiszolgáló, hogy nem blokkolja-kapcsolatok egy PowerShell-parancssorában a következő parancs futtatásával. Ez lesz tesztelje a kapcsolatot az Azure Service Bus. Ez csak azt vizsgálja, a hálózati kapcsolat és azért, az a felhő kiszolgáló szolgáltatás és az átjáró nem tartozik. Könnyebben határozza meg, hogy a gép tudjanak lépni az internethez.

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

Az eredmények ebben a példában láthatóhoz hasonlóan kell kinéznie. Ha **TcpTestSucceeded** nem teljesül, előfordulhat, hogy a tűzfal blokkolja.

```
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Ha teljes, helyettesítés sorolt [konfigurálása portokat](#configure-ports) a témakör későbbi **számítógépnév** és **Port** értékeket.

A tűzfal is előfordulhat, hogy blokkolja a kapcsolatokat, amely az Azure Service Bus teszi az Azure adatközpontokban. Ha ez a helyzet, akkor érdemes whitelist (tiltásának feloldása) az összes az IP-címek a területhez tartozik-e adatközpontokban. [Itt Azure IP-címek](https://www.microsoft.com/download/details.aspx?id=41653)listája elérheti.

### <a name="configure-ports"></a>Portok konfigurálása

Az átjáró létrehoz egy kimenő Azure Service Bus kapcsolat. A kimenő portokon kommunikál: TCP 443-as (alapértelmezett), 5671, 5672, 9350-9354. Az átjáró bejövő portokat nincs szükség.

További tudnivalók [a hibrid megoldásokat](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| A TARTOMÁNYNEVEK | KIMENŐ PORTOK | LEÍRÁS |
| ----- | ------ | ------ |
| *. analysis.windows.net | 443-as | HTTPS |
| *. login.windows.net | 443-as | HTTPS |
| *. servicebus.windows.net |5671-5672 | Speciális üzenetsor Protocol (AMQP) |
| *. servicebus.windows.net | 443-as 9350-9354 | A szolgáltatás Bus továbbítási (megköveteli a 443-as hozzáférési jogkivonat WIA) TCP felett hallgatók |
| *. frontend.clouddatahub.net | 443-as | HTTPS |
| *. core.windows.net | 443-as | HTTPS |
| login.microsoftonline.com | 443-as | HTTPS |
| *. msftncsi.com | 443-as | Az átjáró nem érhető el a Power BI szolgáltatás vizsgálata internetkapcsolattal használja. |

Ha fehér lista IP-címek a tartományok helyett kell, töltse le, és használja a [Microsoft Azure adatközpont IP-tartományok listája](https://www.microsoft.com/download/details.aspx?id=41653). Egyes esetekben az Azure Service Bus kapcsolatok történik IP-címet a teljesen minősített tartománynév helyett.

### <a name="sign-in-account"></a>Bejelentkezési fiókkal

Felhasználók fog jelentkezzen be a munkahelyi vagy iskolai fiókjával. Ez a szervezet fiókját. Ha az Office 365 felületek regisztrált, és nem adja meg a tényleges munka e-mailek, úgy tűnhet, például jeff@contoso.onmicrosoft.com. A fiók belül egy felhőalapú szolgáltatásba belül egy bérlői az Azure Active Directory (AAD) vannak tárolva. A legtöbb esetben AAD fiókja UPN fognak egyezni az e-mail címet.

### <a name="windows-service-account"></a>A Windows szolgáltatásfiók

Az átjáró a helyszíni NT SERVICE\PBIEgwService használja a Windows szolgáltatás bejelentkezési hitelesítő adatokhoz van beállítva. Alapértelmezés szerint akkor jogosult napló szolgáltatás. Ez a, a számítógépen, amelyen telepíti az átjáró környezetében.

Ez a helyszíni adatforrások vagy a munkahelyi vagy iskolai fiókjával, amellyel bejelentkezik a felhőszolgáltatásokba való kapcsolódáshoz használt fiók nem.

##<a name="frequently-asked-questions"></a>Gyakori kérdések

### <a name="general"></a>Általános

**Kérdés**: milyen adatforrások az átjáró támogatja-e?<br/>
**Válasz**: ezen ír, SQL Server kezdve.

**Kérdés**: van szükségem az átjáró a felhőben, például SQL Azure adatforrások? <br/>
**Válasz**: nem értékre. Csak a helyszíni adatforrások átjárók csatlakozik.

**Kérdés**: Mi a tényleges Windows szolgáltatás neve?<br/>
**Válasz**: szolgáltatások, az átjáró neve a Power BI vállalati átjáró szolgáltatással.

**Kérdés**: vannak-e minden bejövő kapcsolatokat az átjáró a felhőből? <br/>
**Válasz**: nem értékre. Az átjáró Azure Service Bus kimenő kapcsolatok használja.

**Kérdés**: Mi a teendő, ha a kimenő kapcsolatokat akadályozhatom? Mit kell megnyitni? <br/>
**Válasz**: olvassa el a portokat és az átjáró használó hosts.


**Kérdés**: az átjáró muszáj telepítve van a adatforrásként az ugyanarra a gépre? <br/>
**Válasz**: nem értékre. Az átjáró fog csatlakozni az adatforráshoz a kapott a kapcsolat adatait. Ebben az értelemben ügyfél alkalmazásként gondolja az átjáró. Csak kell engedélyezni szeretné a kiszolgáló nevét, amely a megadott csatlakozni.


**Kérdés**: Mi az a késése futó lekérdezések az átjáró az adatforrás? Mi az, hogy a legjobb architektúra? <br/>
**Válasz**: hálózati késés csökkentéséhez telepítse az átjáró az adatforrás a lehető legközelebb. A tényleges adatforrás az átjáró telepíthető, ha azt fog lekicsinyítheti a késés jelent meg. Fontolja meg a adatközpontokban. Például a szolgáltatás a nyugati USA-beli adatközpont használ, és az SQL Server-Azure virtuális kiszolgálón van, ha szeretné a Azure virtuális nyugati USA-beli is van. A Késleltetés minimalizálása, és a Azure virtuális kilépési költségek elkerülésére.


**Kérdés**: vannak-e bármelyik hálózati sávszélességre vonatkozó követelmények? <br/>
**Válasz**: azt ajánljuk, hogy a hálózati kapcsolat jó kapacitása. Minden más környezete, és küldött adatok mennyiségét hatással van az eredményeket. A helyszíni és az Azure adatközpontokban közötti átviteli szintű zökkenőmentes segíthet a készült ExpressRoute használatával.

A külső eszköz Azure sebesség tesztelése alkalmazás segítségével rakszelvénye a Mi az a kapacitása.


**Kérdés**: Azure Active Directory-fiókkal az átjáró Windows szolgáltatás futtatható? <br/>
**Válasz**: nem értékre. A Windows szolgáltatás érvényes Windows-fiókkal kell rendelkeznie. Alapértelmezés szerint a szolgáltatás biztonsági NT SERVICE\PBIEgwService AZONOSÍTÓT tartalmazó fog futni.


**Kérdés**: hogyan eredmények küldjük vissza az a felhő? <br/>
**Válasz**: Ez az Azure Service Bus alapján történik. További tudnivalókért lásd: hogyan működik.


**Kérdés**: saját hitelesítő adatok tároló? <br/>
**Válasz**: be az adatforrás hitelesítő adatait az átjáró felhőszolgáltatásában titkosítva vannak tárolva. Az átjáró a helyszíni a visszafejtése történik meg a hitelesítő adatokat.

### <a name="high-availabilitydisaster-recovery"></a>Magas elérhetősége/vészhelyreállítás

**Kérdés**: vannak-e az átjáró magas elérhetősége forgatókönyvek engedélyezése megtehesse? <br/>
**Válasz**: Ez az útmutató a, de azt még nincs ütemterv.


**Kérdés**: milyen lehetőségeket lehet elérni vészhelyreállítás? <br/>
**Válasz**: a helyreállítási billentyű segítségével visszaállítása vagy áthelyezése egy átjárót. Ha telepíti az átjáró, adja meg a helyreállítási billentyűt.


**Kérdés**: Mi az a helyreállítási kulcs előnyeit? <br/>
**Válasz**: áttelepítéséhez, és az átjáró beállításai helyreállítása után katasztrófa biztosít.

### <a name="troubleshooting"></a>Hibaelhárítás

**Kérdés**: hol találhatók a átjáró naplókat? <br/>
**Válasz**: a témakör későbbi eszközök jelennek meg.


**Kérdés**: hogyan jelennek meg mi lekérdezések folyamatban van a helyszíni adatforrás küldeni? <br/>
**Válasz**: engedélyezheti a lekérdezés nyomkövetési, amely tartalmazni fogja a küldött lekérdezések. Ne feledje, hogy állítsa vissza az eredeti értéket, amikor befejezte a hibaelhárítás. A nyomkövetés lekérdezés elhagyása okoz a naplókat, nagyobb méretű lesz.

Az adatforrás tartalmazó nyomkövetési lekérdezések eszközök is tekintse. Ha például is használhatja bővített események vagy SQL Profilert és az SQL Server Analysis Services.

## <a name="how-the-gateway-works"></a>Az átjáró működése

Amikor a felhasználó csatlakozik egy helyszíni adatforrás elemet kommunikáljon:

1. A felhőbeli szolgáltatástól lekérdezést hoz létre, a titkosított az adatforrás hitelesítő adatokkal együtt, és elküldi a lekérdezést a sorba, az átjáró feldolgozása.
1. A szolgáltatás elemzi a lekérdezés és ezt a kérelem nyújtja az Azure Service Bus szeretne.
1. Az átjáró a helyszíni lekérdezi az Azure Service Bus, a függőben lévő kérelmek.
1. Az átjáró kapja a lekérdezés, használatával visszafejti a hitelesítő adatokat, és csatlakozik a adatforrás(ok) és a hitelesítő adatokat.
1. Az átjáró az adatforrás-végrehajtási elküldi a lekérdezést.
1. Az eredmények az adatforrásból kerülnek vissza az átjáró, majd a felhőbeli szolgáltatástól alakzatot. A szolgáltatás akkor használja az eredményeket.

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="update-to-the-latest-version"></a>A legújabb verzióra frissíteni

Problémák sok elhelyezhetők a átjáróverzió elavultak esetén.  Győződjön meg róla, hogy a legújabb verzióra szóló általános javasolt.  Ha még nem frissíti az átjáró, havonta vagy hosszabb ideig, érdemes az átjárót a legújabb verziójának telepítése, és megtekintheti, ha a probléma reprodukálása is.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users-"></a>Hiba: Sikertelen a felhasználók hozzáadása csoporthoz. (-2147463168 PBIEgwService teljesítménybeli napló felhasználók)

Ez a hiba jelenhet meg, ha az átjáró telepítése tartományvezérlőnek, ami nem támogatott. Az átjáró, amely nem egy tartományvezérlőnek a számítógépen telepíteni kell.

## <a name="tools"></a>Eszközök

### <a name="collecting-logs-from-the-gateway-configurator"></a>Az átjáró konfigurációs naplók összegyűjtése

Az átjáró több naplók összegyűjtheti. Mindig indíthat a naplókat.

#### <a name="installer-logs"></a>Installer naplók

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Konfigurációs naplók

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Vállalati átjáró szolgáltatás naplók

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Eseménynaplók

Az adatkezelési átjáró és PowerBIGateway naplók találhatók az **alkalmazás- és szolgáltatásnaplók**.

### <a name="fiddler-trace"></a>Fiddler nyomon követése

[Fiddler](http://www.telerik.com/fiddler) egy ingyenes eszköz Telerik, és figyeli a HTTP-forgalmat.  Lásd: a Vissza gombra, és a kibontott készült Power BI az ügyfélgép a szolgáltatás. Ez lehet megjelenítése a hibák és egyéb kapcsolódó információk.

## <a name="next-steps"></a>Következő lépések
- [Egy helyszíni kapcsolata összefüggés-alkalmazás létrehozása](app-service-logic-gateway-connection.md)
- [Nagyvállalati integrációs szolgáltatások](app-service-logic-enterprise-integration-overview.md)
- [Logika alkalmazások összekötők](../connectors/apis-list.md)