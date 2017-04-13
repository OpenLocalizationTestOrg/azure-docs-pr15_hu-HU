<properties 
    pageTitle="A hibrid kezelővel |} Microsoft Azure" 
    description="Telepítése és konfigurálása a hibrid kapcsolatkezelő, valamint a helyszíni összekötők logika alkalmazások csatlakoztatása" 
    services="app-service\logic" 
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/>

# <a name="connect-to-on-premises-connectors-using-the-hybrid-connection-manager"></a>A hibrid kezelővel helyszíni összekötők összekapcsolása

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások 2014-es – 12-01-séma verziót vonatkozik. Logika alkalmazások általános elérhetőség (kiadás) az átjáró a helyszíni kapcsolatot használja. További információ az új [átjáró](app-service-logic-gateway-connection.md) és [Logika alkalmazások Georgia](https://azure.microsoft.com/documentation/services/logic-apps/).

Logika alkalmazások egy helyszíni rendszert használ, használja a hibrid kapcsolatkezelő. Egyes összekötők csatlakozhat egy helyi rendszer, például SQL Server, SAP, SharePoint és így tovább. 

A hibrid kapcsolat Manager (HCM) egy kattintással-egyszer a telepítve van az IIS-kiszolgálón a hálózaton, a tűzfal mögött belül telepítőt. Az Azure Service Bus továbbítási használ, táblabeli HCM hitelesíti a helyszíni rendszer összekötő Azure-ban. 

> [AZURE.NOTE] Hibrid kapcsolatkezelő csak egy helyszíni erőforrás csatlakozik a tűzfal mögött szükség. Ha nem csatlakozik a helyszíni rendszerre, akkor a hibrid kapcsolatkezelő nem szükséges.

Első lépések a következők szükségesek:

- Azure Service Bus továbbítási névtér Társítások kapcsolati karakterláncot. Lásd: a [Szolgáltatás Bus árak](https://azure.microsoft.com/pricing/details/service-bus/) megállapíthatja, hogy melyik réteg jelfogók tartalmazza.
- A helyszíni system bejelentkezési adatokat, többek között a felhasználónevet és jelszót. Például ha egy helyszíni SQL Server kapcsolódik, akkor az SQL Server bejelentkezési fiók és a jelszavát.
- A helyszíni kiszolgálói adatait, beleértve a port száma és a kiszolgáló nevét. Például ha egy helyszíni SQL Server kapcsolódik, akkor az SQL-kiszolgáló neve és a TCP-portszámot.

## <a name="get-the-service-bus-connection-string"></a>A szolgáltatás Bus kapcsolati karakterláncát

Az Azure-portálon másolja a szolgáltatás Bus legfelső szintű Társítások kapcsolati karakterláncot. A kapcsolati karakterláncot az Azure összekötő csatlakozik, a helyszíni rendszerhez. 

1. Az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885)válassza ki a szolgáltatás Bus névteret, és válassza a **Kapcsolat adatait**:

    ![][SB_ConnectInfo]

2. Másolja a Társítások kapcsolati karakterláncot:

    ![][SB_SAS]

## <a name="install-the-hybrid-connection-manager"></a>A hibrid kapcsolatkezelő telepítése

1. Az [Azure portál](http://go.microsoft.com/fwlink/p/?LinkID=525040)jelölje ki az összekötőt, hozott létre. A megnyitáshoz, válassza a **Tallózás gombra**, jelölje be az **API-alkalmazások**, és jelölje ki az összekötő vagy API-alkalmazás. 
<br/><br/>
A **Hibrid kapcsolat**a telepítő nem **teljes**:
<br/>
![][2] 

2. Jelölje ki **a hibrid kapcsolatot**. A korábban megadott szolgáltatás Bus kapcsolati karakterlánc szerepel.
3. Másolja a vágólapra az **elsődleges konfigurációs karakterlánc**:
<br/>
![][PrimaryConfigString]

4. A **Helyszíni hibrid kapcsolatkezelő**töltse le a hibrid kapcsolat kezelő, vagy közvetlenül a portálon telepítheti. 
<br/><br/>
Közvetlenül a portálon telepítéséhez lépjen a helyszíni IIS-kiszolgálóhoz, keresse meg a portált, és válassza a **Letöltés és konfigurálása**.
<br/><br/>
A hibrid kapcsolatkezelő letöltéséhez nyissa meg a helyszíni IIS-kiszolgálót, és nyissa meg az **alkalmazás ClickOnce** (http://hybridclickonce.azurewebsites.net/install/Microsoft.Azure.BizTalk.Hybrid.ClickOnce.application). A telepítés automatikusan elindul, hogy meg tudja nyitni.

5. A **Figyelő beállítási** ablakban adja meg az **Elsődleges konfigurációs karakterlánc** (a 3) beillesztett és **telepítéséhez**.

Ha a telepítés befejeződött, a következő jeleníti meg:
<br/>
![][3] 

Most már az összekötő újra tallózással hibrid kapcsolati állapotának esetén **csatlakoztatva**. Szükség lehet, és zárja be az összekötő újra:
<br/>
![][4] 

> [AZURE.NOTE] Váltás a másodlagos kapcsolati karakterláncot, futtassa újra a hibrid kapcsolat beállítása, és írja be a **Másodlagos konfigurációs karakterlánc**.


## <a name="tcp-ports-and-security"></a>TCP-portokat és biztonság

Hibrid kapcsolatot hoz létre, ha egy webhely a helyi helyszíni IIS-kiszolgálón jön létre. A DMZ az IIS-kiszolgálón is lehet. Az IIS-kiszolgálón TCP portokon a következők:

TCP-Port | miért
--- | ---
 | Nincs bejövő TCP-portok szükség.
9350 - 9354 | Ezek a portok adatátvitel segítségével. A szolgáltatás Bus továbbítási kezelő ellenőrzi a port 9350 annak megállapításához, hogy a TCP-kapcsolat érhető el. Ha elérhető, majd azt feltételezi, hogy port 9352 is elérhető. Adatforgalom port 9352 áttekintést. <br/><br/>Engedélyezze a kimenő kapcsolatokat portokhoz.
5671 | Adatforgalom port 9352 használatakor port 5671 szolgál a a vezérlő csatorna. <br/><br/>Engedélyezze a kimenő kapcsolatokat a porthoz. 
80, 443-as | Ha 9352 és 5671 nem használható, *majd* 80 és 443-as legyen az adatátvitel és a vezérlő csatorna használt alaplekérdezések portokat.<br/><br/>Engedélyezze a kimenő kapcsolatokat portokhoz.
Helyszíni rendszer port | Nyissa meg a rendszer által használt port a helyszíni, a rendszer. Ha például SQL Server általában az 1433 portot használja. Nyissa meg a TCP-portot.

[A szolgáltatás Bus tűzfal mögött szolgáltatónál](http://msdn.microsoft.com/library/azure/ee706729.aspx)

## <a name="troubleshooting"></a>Hibaelhárítás

![][HCMFlow]

### <a name="on-premises-troubleshooting"></a>A helyszíni – hibaelhárítás

1. Erősítse meg az IIS webes szerepkör telepítve van, és az IIS-szolgáltatásokat elindított az IIS-kiszolgálón.
2. Az IIS-kiszolgálón ellenőrizze a hibrid kapcsolatkezelő telepíteni és futtatni:
 - Az IIS-kezelőben (inetmgr) található, a ***MicrosoftAzureBizTalkHybridListener*** webhely listában, és futnia. 
 - A webhely használ-e a ***HybridListenerAppPool*** fut, a *hálózati* helyi beépített felhasználói szolgáltatásfiók. Ez a AppPool is el kell kezdődnie.
3. Az IIS-kiszolgálón erősítse meg az összekötő telepíteni és futtatni: 
 - A webhely az összekötő jön létre. Ha például egy SQL-összekötő hozta létre, ha van ***MicrosoftSqlConnector_nnn*** webhelye. Az IIS-kezelőben (inetmgr) található, győződjön meg arról, a webhely szerepel a listában, és lépések. 
 - A webhely használ-e saját IIS-alkalmazáskészlet ***HybridAppPoolnnn***nevű. A *hálózati* helyi beépített felhasználói szolgáltatásfiók fut ez AppPool. A webhely és a AppPool kell mindkét indítani. 
 - Tallózással keresse meg a helyi összekötő. Például ha összekötő webhelye 6569 portot használja, nyissa meg http://localhost:6569. Nincs beállítva alapértelmezett dokumentumot, egy `HTTP Error 403.14 - Forbidden error` várható.
4. A tűzfalat győződjön meg arról az ebben a témakörben TCP-portokat is meg nyitva.
5. Nézze meg a forrás- vagy rendszerben:
 - Egyes helyszíni rendszerek szükség további függőség fájlokat. Például ha helyszíni SAP kapcsolódik, néhány további SAP-fájlok telepítenie kell az IIS-kiszolgálón.
 - Jelölje be a rendszer a bejelentkezési fiókkal szeretne kapcsolatot. A olyan portot, a rendszer által használt például meg kell nyitni, például SQL Server 1433 portot. Az Azure-portálon végezze el a beírt bejelentkezési fiókját kell rendelkeznie a rendszer a hozzáférést.
6. Az IIS-kiszolgálót jelölje be az eseménynaplók hibákat. 
7. Karbantartási, és telepítse újra a hibrid kapcsolatkezelő: 
 - Az IIS manuálisan törölje az összekötő és a alkalmazáskészlet. 
 - Futtassa újra a hibrid kapcsolatkezelő, és erősítse meg, a megfelelő **Elsődleges konfigurációs karakterlánc** beviszi az összekötő.



### <a name="in-the-azure-classic-portal"></a>Az Azure klasszikus portálon

1. Ellenőrizze a szolgáltatás Bus névtér van egy **aktív** állapot.
2. Az összekötő létrehozásakor adja meg a szolgáltatás Bus Társítások kapcsolati karakterláncot. Nem kell megadni a ACS kapcsolati karakterláncot.


## <a name="faq"></a>GYAKORI KÉRDÉSEK

**Kérdés**: van két hibrid kapcsolat menedzserek. Mi a különbség? 

**Válasz**: a [Hibrid kapcsolatok](../biztalk-services/integration-hybrid-connection-overview.md) technológia való csatlakozáshoz a helyszíni elsősorban a Web Apps alkalmazások (korábbi nevén webhelyek) és a Mobile-alkalmazások (korábbi nevén mobil szolgáltatások) által használt van. A hibrid kapcsolatok Manager saját [beállítása](../biztalk-services/integration-hybrid-connection-create-manage.md) és használja az Azure BizTalk szolgáltatásainak (a háttérben). Csak a TCP- és a HTTP protokollok támogat.

Azure alkalmazás szolgáltatás összekötőkkel azt is, hogy egy hibrid kapcsolatkezelő.  A hibrid kapcsolatkezelő jelent *nem* használja az Azure BizTalk Service (a háttérben) és támogatja a több, mint a TCP- és a HTTP protokollok. Lásd az [összekötők és az API-alkalmazások listájában](app-service-logic-connectors-list.md).

Azure Service Bus egyaránt használja, a helyszíni rendszerhez való csatlakozáshoz.

**Kérdés**: egyéni API-alkalmazás létrehozásakor használhatom az alkalmazás szolgáltatás hibrid kapcsolatkezelő helyszíni csatlakozni? 

**Válasz**: nem szerepel a hagyományos értelemben. A beépített összekötő használata, állítsa be az alkalmazás szolgáltatás hibrid kapcsolatkezelő a helyszíni rendszerhez való csatlakozáshoz. Ezután használja a összekötő az egyéni API-alkalmazást, esetleg logikájának alkalmazással. Jelenleg nem fejlesztése, vagy hozzon létre saját hibrid API-alkalmazás (például az SQL-összekötő vagy fájl összekötő).

Ha az egyéni API TCP- vagy HTTP portot használ, [A hibrid kapcsolatok](../biztalk-services/integration-hybrid-connection-overview.md) és a hibrid kapcsolatkezelő is használhatja. Ebben az esetben az Azure BizTalk szolgáltatásainak használják. [Csatlakozás helyszíni SQL Server egy web app alkalmazásból](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md) nyújthatnak segítséget:  


## <a name="read-more"></a>További információ

[A logikai alkalmazások figyelése](app-service-logic-monitor-your-logic-apps.md)<br/>
[Bus árak szolgáltatás](https://azure.microsoft.com/pricing/details/service-bus/)



[SB_ConnectInfo]: ./media/app-service-logic-hybrid-connection-manager/SB_ConnectInfo.png
[SB_SAS]: ./media/app-service-logic-hybrid-connection-manager/SB_SAS.png
[PrimaryConfigString]: ./media/app-service-logic-hybrid-connection-manager/PrimaryConfigString.png
[HCMFlow]: ./media/app-service-logic-hybrid-connection-manager/HCMFlow.png
[2]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupIncomplete.jpg
[3]: ./media/app-service-logic-hybrid-connection-manager/HybridSetup.jpg
[4]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupComplete.jpg

 
