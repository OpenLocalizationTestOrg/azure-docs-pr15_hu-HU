<properties 
    pageTitle="Azure kormányzati fejlesztői útmutatója" 
    description="Ez ez a témakör a szolgáltatást, és útmutatást összehasonlítás Azure kormányzati alkalmazások fejlesztéséhez" 
    services="" 
    cloud="gov"
    documentationCenter="" 
    authors="Joharve2" 
    manager="Chrisnie" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="azure-government" 
    ms.date="10/29/2015" 
    ms.author="jharve"/>


#  <a name="microsoft-azure-government-developer-guide"></a>Útmutató a Microsoft Azure kormányzati Developer 

<p> Microsoft Azure kormányzati van egy fizikailag és a Microsoft Azure elszigetelt példányára hálózati.  Ez a fejlesztők az útmutató fog szolgálni részletei a különbségeket adott alkalmazásfejlesztő és rendszergazdák kimutatásadatokat interaktív módon, és ezek Azure külön területe használata.

<!--Table of contents for topic, the words in brackets must match the heading wording exactly-->


## <a name="in-this-topic"></a>Ebben a témakörben


+ [– Áttekintés](#Overview)
+ [Útmutató a fejlesztők számára](#Guidance)
+ [A Microsoft Azure kormányzati jelenleg elérhető szolgáltatások](#Features)
+ [Végpont hozzárendelése](#Endpoint)
+ [Következő lépések](#next)


## <a name="Overview"></a>– Áttekintés

Microsoft Azure kormányzati a Microsoft Azure szolgáltatás amerikai szövetségi kormányügynökségeknek, az állapot és a helyi kormányok és a saját megoldások szolgáltatók biztonság és megfelelőség szükségleteinek megcímezheti külön példányát. Azure kormányzati kínál fizikai elkülönítési nem USA-beli kormányzati környezetekben a hálózati és árnyékolt USA-beli személyzeti biztosít. 

A Microsoft számos eszközök hozhat létre és helyezhetnek üzembe a Microsoft globális Microsoft Azure szolgáltatás ("globális") és a Microsoft Azure kormányzati szolgáltatások felhő alkalmazásokat.

Létrehozásakor, és az alkalmazások az Azure kormányzati szolgáltatások, nem pedig a globális szolgáltatás üzembe helyezése a fejlesztők kell, hogy a két szolgáltatás a főbb különbségek vannak.  Kifejezetten körül és programozási környezetükben beállításával, konfigurálja az alkalmazásokat, és az Azure kormányzati szolgáltatásként helyezése végpontokon.

A dokumentumban szereplő információk azokat a különbségeket összesíti, és egészíti ki az információkat az [Azure kormányzati](http://www.azure.com/gov "Azure kormányzati") webhelyen, és a [Microsoft Azure technikai könyvtár](http://msdn.microsoft.com/cloud-app-development-msdn "MSDN") MSDN érhető el. Hivatalos információkat is elérhetők lehetnek számos más helyekről például a [Microsoft Azure Trust Center] (https://azure.microsoft.com/support/trust-center/ "Microsoft Azure Trust Center" /), [Azure dokumentáció központ](https://azure.microsoft.com/documentation/) és a [Azure blogok] (https://azure.microsoft.com/blog/ "Azure blogok" /). 

A tartalom a partnerek és a Microsoft Azure kormányzati telepít fejlesztőknek íródott.



## <a name="Guidance"></a>Útmutató a fejlesztők számára
A műszaki tartalmat, amely jelenleg elérhető a legtöbb feltételezi, hogy alkalmazásokat a globális szolgáltatás, hanem a Microsoft Azure kormányzati kifejlesztett, mert fontos biztosítja, hogy a fejlesztők ismerjék szerepeltethetők az Azure kormányzati kifejlesztett alkalmazásokban a főbb különbségek vannak.

- Első lépésként szolgáltatások és szolgáltatásbeli különbségek vannak, ez azt jelenti, hogy bizonyos funkciókat, amelyek az adott régióban a globális szolgáltatás nem feltétlenül vehető igénybe az Azure kormányzati.

- Második, a program felajánlja az Azure kormányzati funkciói konfigurációs különbségek vannak a globális szolgáltatásból.  Ezért nézze át a példakódot, a beállításokat és a lépéseket követve győződjön meg arról, hogy, létrehozása és az Azure kormányzati Cloud Services környezetén belül végrehajtása.


## <a name="Features"></a>A Microsoft Azure kormányzati jelenleg elérhető szolgáltatások
Azure kormányzati által jelzett érhető el az alábbi szolgáltatások US GOV IOWA és a Kapcsolatfelvételi GOV VIRGINIA régióban:

- Virtuális gépeken futó
- Cloud Services
- Tárhely
- Az Active Directory
- A Feladatütemező
- Virtuális hálózatok
- SQL-adatbázis
- Azure fájlok
- Media Services
- Adatforgalom Manager
- Szolgáltatás Bus
- StorSimple
- Gyorsítótár vgx.dll
- Azure biztonsági mentése
- Automatizálási
- Készült ExpressRoute
- stb.

Más szolgáltatások érhetők el, és több szolgáltatás, hozzáadódik a folyamatos kombinálásával.  A legfrissebb szolgáltatások listáját olvassa el a [régiók lap](https://azure.microsoft.com/regions/#services) , amely minden elérhető terület és a szolgáltatások kiemeli.  

Kapcsolatfelvételi GOV Iowa és US GOV Virginia jelenleg az Azure kormányzati támogató adatközpontokban.  Olvassa el a régiók lap aktuális adatközpontokban és a rendelkezésre álló szolgáltatások felett.

## <a name="Endpoint"></a>Végpont hozzárendelése

Az alábbi táblázat segítségével segítenek nyilvános Microsoft Azure és SQL-adatbázis végpontok Azure kormányzati adott végpontok megfeleltetéskor.


Szolgáltatás típusa|Azure nyilvános|Azure kormányzati
---|---|---
Adatkezelési portál|manage.windowsazure.com|manage.windowsazure.us
Általános|*. windows.net|*. usgovcloudapi.net
Alapvető|*. core.windows.net|*. core.usgovcloudapi.net
Számítási|*. cloudapp.net|*. usgovcloudapp.net
BLOB-tárolóhoz|*. blob.core.windows.net|   *. blob.core.usgovcloudapi.net
A várakozási tárhely|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Táblatároló|*. table.core.windows.net|*. table.core.usgovcloudapi.net
Szolgáltatáskezelés|Management.Core.Windows.NET|Management.Core.usgovcloudapi.NET
SQL-adatbázis|*. database.windows.net|*. database.usgovcloudapi.net
ARM terheléselosztás végpont|https://Management.Windows.NET|https://Management.usgovcloudapi.NET  

* ARM hitelesítéshez Azure AD-e kérjük, [Azure erőforrás-kezelő kérések hitelesítő](https://msdn.microsoft.com/library/azure/dn790557.aspx) hivatkozás

## <a name="next"></a>Következő lépések

Ha Ön érdekel további és Azure kormányzati kérjük kihasználhatja az alábbi hivatkozásokat részét a következő címen.

- **[Feliratkozás a próbaverzió](https://azuregov.microsoft.com/trial/azuregovtrial)**

- **[Beszerzése és Azure kormányzati elérése](http://azure.com/gov)**

- **[Azure kormányzati – áttekintés](/azure-government-overview)**

- **[Azure kormányzati Blog](http://blogs.msdn.com/b/azuregov/)**

- **[Azure megfelelőség](https://azure.microsoft.com/support/trust-center/compliance/)**

<!--Anchors-->



<!-- Images. -->

[1]: ./media/azure-government-developer-guide/publisherguide.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: storage-whatis-account.md
