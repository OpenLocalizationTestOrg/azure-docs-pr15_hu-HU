<properties 
    pageTitle="Nagyvállalati integrációs áttekintése |} Microsoft Azure alkalmazás szolgáltatás |} Microsoft Azure" 
    description="Nagyvállalati integrációs funkciók használata az összefüggés-alkalmazások használata üzleti folyamatok és integrációs forgatókönyvek engedélyezése" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-the-enterprise-integration-pack"></a>A nagyvállalati integrációs csomag áttekintése

## <a name="what-is-the-enterprise-integration-pack"></a>Mi az a nagyvállalati integrációs csomag?
A nagyvállalati integrációs csomagot a Microsoft felhőalapú megoldás zökkenőmentes engedélyezése az üzleti vállalati verzió (B2B) kommunikáció. A csomag iparágban szabványos protokollok, például [AS2](./app-service-logic-enterprise-integration-as2.md), [X12](./app-service-logic-enterprise-integration-x12.md)és [EDIFACT](./app-service-logic-enterprise-integration-edifact.md) használja üzenetváltásra partnerei között. Üzenetek másik lehetőségként védhetők titkosítást és a digitális aláírás. 

A csomag lehetővé teszi, hogy a különböző protokollok használó szervezetek, és az üzenetek elektronikusan exchange által a különböző formátumú átalakítása formátumúvá alakítja mindkét szervezetnél rendszerek értelmezése, és a művelet végrehajtása a formátumok. 

Ha ismeri a BizTalk kiszolgálón vagy a Microsoft Azure BizTalk szolgáltatások, az Ön könnyen használható a nagyvállalati integrációs szolgáltatások, mivel a fogalmak többsége hasonló találhatók. Több fő különbség, hogy a nagyvállalati integrációs integrációs fiókok használja tárolása és B2B kommunikáció használt eltérések kezelésének egyszerűsítése érdekében. 

Architecturally a nagyvállalati integrációs csomag **integrációs fiókok** tervezése, telepítése, és a B2B alkalmazások kezelése használható minden eltérések tárolható alapul. Integráció fiók alapjában eltéréseket, például sémák, a partnerek, a tanúsítványok, a térképeket és a rendelkezést tárolására felhőalapú tároló. Ezeket az eltéréseket majd használható alkalmazások logika B2B munkafolyamatok létrehozásához. Az eltérések a logika alkalmazásban használata előtt kell integrációs fiókja csatolása az logika alkalmazást. Csatolása őket, után a logika alkalmazás hozzáférést kap a integrációs fiók eltérések.  

## <a name="why-should-you-use-enterprise-integration"></a>Miért érdemes használni nagyvállalati integrációs?
- Enterprise-integrációval képes a az eltéréseket tárolni a integrációs fiók egy helyen. 
- A logikai alkalmazások motor és B2B munkafolyamatok létrehozását, és a 3 szoftver alkalmazásait, a helyszíni alkalmazások, valamint az egyéni alkalmazások integrálása az összekötők is élvezheti.
- Azure függvények is is élvezheti

## <a name="how-to-get-started-with-enterprise-integration"></a>Első lépések a nagyvállalati integrációs hogyan?
Össze, és a nagyvállalati integrációs csomag keresztül a logika alkalmazások tervező használata az **Azure portál**B2B alkalmazások kezelése.  

(https://msdn.microsoft.com/library/azure/mt652195.aspx "Logika alkalmazások PowerShell témakörök") [PowerShell]használatával a logika alkalmazások kezelése. 

Az alábbiakban a előtt alkalmazások hozhat létre az Azure-portálon végezze el a szükséges lépések áttekintését: ![áttekintés képe](./media/app-service-logic-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Mik azok a néhány gyakori alkalmazási területek

Nagyvállalati integrációs ezek iparági szabványokat támogat:   

- Szerkesztése - elektronikus adatcsere  
- EAI - integráció a vállalati alkalmazás  

## <a name="heres-what-you-need-to-get-started"></a>Íme a kezdéshez szükséges
- Az Azure előfizetéssel integrációs fiókkal
- Visual Studio 2015 térképek és sémák létrehozása
- [Microsoft Azure logika alkalmazások nagyvállalati integrációs Tools for Visual Studio 2015 2.0-s verziója](https://aka.ms/vsmapsandschemas)  

## <a name="try-it"></a>További információk
[Próbálja ki most](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) teljesen műveleti minta AS2 üzembe küldése és fogadása logika alkalmazások B2B funkciók használó logika app.

## <a name="learn-more-about"></a>További információ:
- [Rendelkezést] (./app-service-logic-enterprise-integration-agreements.md "Megtudhatja, hogy nagyvállalati integrációs rendelkezést")
- [Üzleti az üzleti (B2B) felhasználási területei] (./app-service-logic-enterprise-integration-b2b.md "Megtudhatja, hogy miként B2B funkcióival logika alkalmazások létrehozása")  
- [Tanúsítványok] (./app-service-logic-enterprise-integration-certificates.md "Nagyvállalati integrációs tanúsítványokkal kapcsolatos további tudnivalók")
- [Dekódolását és kódolását strukturálatlan fájlhoz] (./app-service-logic-enterprise-integration-flatfile.md "Megtudhatja, hogy miként kódolását és dekódolását strukturálatlan fájlhoz tartalma")  
- [Integráció számlák] (./app-service-logic-enterprise-integration-accounts.md "Megtudhatja, hogy integrációs fiókok")
- [Térképek] (./app-service-logic-enterprise-integration-maps.md "Megtudhatja, hogy nagyvállalati integrációs megfeleltetések")
- [Partnerek] (./app-service-logic-enterprise-integration-partners.md "Megtudhatja, hogy nagyvállalati integrációs partnerek")
- [A sémák] (./app-service-logic-enterprise-integration-schemas.md "Megtudhatja, hogy a sémák nagyvállalati integrációs")
- [XML-adatok üzenet érvényesítése] (./app-service-logic-enterprise-integration-xml.md "Megtudhatja, hogy miként logika alkalmazással XML-üzenetek ellenőrzése")
- [Az XML-átalakítás] (./app-service-logic-enterprise-integration-transform.md "Megtudhatja, hogy nagyvállalati integrációs megfeleltetések")
- [Nagyvállalati integrációs összekötők] (../connectors/apis-list.md "Megtudhatja, hogy nagyvállalati integrációs csomag összekötők")



