<properties
   pageTitle="A helyszíni adatok átjáró |} Microsoft Azure"
   description="Egy helyszíni átjáró szükség, ha az Analysis Services-kiszolgáló Azure-ban a helyszíni adatforrások fog csatlakozni."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="on-premises-data-gateway"></a>A helyszíni adatok átjáró

Az átjáró a helyszíni működik-e egy azt a helyszíni adatforrások és a felhőben az Azure Analysis Services-kiszolgáló közötti biztonságos adatátvitel kezeléséről.

Az átjárók telepítve van a hálózatban lévő. Egy átjáró telepíteni kell az Azure-előfizetése van Azure Analysis Services-kiszolgálókon. Például ha két kiszolgálókhoz az Azure-előfizetése, amelyek a helyszíni adatforrások, az átjáró telepítenie kell a hálózat két különböző számítógépen.

## <a name="requirements"></a>Követelmények

**Minimális követelmények:**

- .NET 4.5 keretrendszer
- 64 bites Windows 7 / Windows Server 2008 R2 (vagy újabb)

**Javasolt:**

- 8 core Processzor
- 8 GB memóriát
- 64 bites verzióját a Windows 2012 R2 (vagy újabb)

**Fontos tudnivalók:**

- Az átjáró nem telepíthető a tartományvezérlőnek.

- Csak egy átjáró egyetlen számítógépre telepíthető.

- Telepítse az átjáró használt számítógépen marad, és nem megy alvó. Ha a számítógép nem szerepel, az Azure Analysis Services-kiszolgáló nem tud csatlakozni a helyszíni adatforrások frissítheti az adatokat.

- Az átjáró nem telepíthető a számítógép kapcsolódik vezeték nélküli a hálózathoz. Teljesítmény is kell szüntetni.

- A kiszolgáló nevét, amelyek már konfigurálva átjáró módosításához meg kell telepítse újra, majd adjon meg egy új átjárót.

- Egyes esetekben csatlakozás adatforrásokhoz, például SQL Server natív ügyfele (SQLNCLI11) natív szolgáltatókkal táblázatos modellekben esetleg hibát jelez. További tudnivalókért lásd: az [adatforrás-kapcsolatok](analysis-services-datasource.md).

## <a name="supported-on-premises-data-sources"></a>Támogatott helyszíni adatforrások
Előzetes verzió az átjáró támogatja az Azure Analysis Services-kiszolgáló és a következő helyszíni adatforrások közötti kapcsolatok:

- Az SQL Server
- SQL-adatraktár
- APPS ALKALMAZÁSOK
- Az Oracle
- Teradata


## <a name="download"></a>Letöltés
 [Az átjáró letöltése](https://aka.ms/azureasgateway)


## <a name="install-and-configure"></a>Telepítse és állítsa be

1. A telepítést.

2. Válassza ki a telepítés helyét, és fogadja el a licencszerződést.

3. Jelentkezzen be az Azure.

4. Adja meg az Azure-elemzés kiszolgáló neve. Csak egy átjáró egy kiszolgálón is megadhat. Kattintson a **Konfigurálás** gombra, és minden rendben lépjen.

    ![Jelentkezzen be az azure](./media\analysis-services-gateway\aas-gateway-configure-server.png)


## <a name="how-it-works"></a>Működése
Az átjáró szolgáltatásként Windows, **a helyszíni adatok átjárót**, a szervezet hálózatán a számítógépen. Az átjáró, telepítse az Azure Analysis Services használata más szolgáltatások, például a Power BI használt átjárón alapul, de a hogyan eltérések rendelkező úgy van beállítva.

![Működése](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Lekérdezések és adatok folyamat munka jelennek meg:

1.  A lekérdezés a felhőalapú szolgáltatást a titkosított hitelesítő adatokkal a helyszíni adatforrás hozható létre. Az átjáró feldolgozása egy sorba, majd küldi.

2.  Az átjáró felhőszolgáltatásba elemzi a lekérdezés és ezt a kérelem nyújtja [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).

3.  Az átjáró a helyszíni lekérdezi az Azure Service Bus, a függőben lévő kérelmek.

4.  Az átjáró kapja meg a lekérdezést, használatával visszafejti a hitelesítő adatokat, és a hitelesítő adatokat tartalmazó adatforrásokhoz kapcsolódik.

5.  Az átjáró az adatforrás-végrehajtási elküldi a lekérdezést.

6.  Az eredmények az adatforrásból kerülnek vissza az átjáró, majd a felhőbeli szolgáltatástól alakzatot.

## <a name="windows-service-account"></a>A Windows szolgáltatásfiók

Az átjáró a helyszíni *NT SERVICE\PBIEgwService* használja a Windows szolgáltatás bejelentkezési hitelesítő adatokhoz van beállítva. Alapértelmezés szerint van jobbra lévő bejelentkezési szolgáltatásként; a gép, hogy az átjáró telepíti a környezetben. A hitelesítő adatok nem ugyanazzal a fiókkal használja a helyszíni adatforrások az Azure-fiók.  

Ha a proxykiszolgáló hitelesítési miatt a problémákat, előfordulhat, hogy módosítani kívánt a Windows-szolgáltatási fiók egy felhasználóhoz, a tartomány vagy kezeli a szolgáltatási fiók.

## <a name="ports"></a>Portok

Az átjáró létrehoz egy kimenő Azure Service Bus kapcsolat. A kimenő portokon kommunikál: TCP 443-as (alapértelmezett), 5671, 5672, 9350 9354 keresztül.  Az átjáró bejövő portokat nincs szükség.

Az alábbi helyeken ajánlott azt az IP-címek a adatok régió, a tűzfal whitelist. Letöltheti a [Microsoft Azure adatközpont IP-lista](https://www.microsoft.com/download/details.aspx?id=41653). Ebben a listában a heti frissül.

> [AZURE.NOTE]  Az IP-címek szerepel az Azure adatközpont IP-lista CIDR formátumban vannak. Ha például 10.0.0.0/24 nem jelent meg 10.0.0.0 10.0.0.24 keresztül. További tudnivalók a [CIDR jelöléssel](http://whatismyipaddress.com/cidr).

Az alábbiakban a teljesen minősített tartománynév az átjáró által használt.

|A tartománynevek|Kimenő portok|Leírás|
|---|---|---|
|*. powerbi.com|80|HTTP, a telepítő letöltéséhez használt.|
|*. powerbi.com|443-as|HTTPS|
|*. analysis.windows.net|443-as|HTTPS|
|*. login.windows.net|443-as|HTTPS|
|*. servicebus.windows.net|5671-5672|Speciális üzenetsor Protocol (AMQP)|
|*. servicebus.windows.net|443-as 9350-9354|A szolgáltatás Bus továbbítási (megköveteli a 443-as hozzáférési jogkivonat WIA) TCP felett hallgatók|
|*. frontend.clouddatahub.net|443-as|HTTPS|
|*. core.windows.net|443-as|HTTPS|
|login.microsoftonline.com|443-as|HTTPS|
|*. msftncsi.com|443-as|Az átjáró nem érhető el a Power BI szolgáltatás vizsgálata internetkapcsolattal használja.|
|*.microsoftonline-p.com|443-as|Attól függően, hogy a konfigurációs hitelesítéshez használt.|


### <a name="forcing-https-communication-with-azure-service-bus"></a>Azure Service Bus HTTPS kommunikációt kényszerítése

Az átjáró Azure Service Bus közvetlen TCP; helyett HTTPS használatával kommunikál kényszerítheti Ez azonban nagyban csökkentheti teljesítményét. A *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* fájl módosításához szükséges. Módosítsa az értéket a `AutoDetect` való `Https`. Ez a fájl található, a *C:\Program Files\On helyszíni adatok átjáró*alapértelmezés szerint.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```


## <a name="troubleshooting"></a>Hibaelhárítás
A motorháztető alatt fülre a helyszíni adatok átjáró a helyszíni adatforrások Azure Analysis Services való csatlakozáshoz használt a Power BI használt átjárón.

Ha problémákat tapasztal, amikor való telepítéséről és konfigurálásáról az átjárók, feltétlenül lásd: [a Power BI átjáró hibaelhárítás](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem-tshoot/). Ha úgy gondolja, hogy problémát tapasztal, a tűzfal, olvassa el a tűzfal vagy proxykiszolgáló szakaszok című témakört.

Ha úgy gondolja, hogy ha a proxy problémák, az átjáró, tanulmányozza [a Power BI átjárók konfigurálása proxybeállításait](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy.md).

## <a name="next-steps"></a>Következő lépések
- [Az Analysis Services kezelése](analysis-services-manage.md)
- [Adatok beolvasása az Azure Analysis Services rendszerből](analysis-services-connect.md)
