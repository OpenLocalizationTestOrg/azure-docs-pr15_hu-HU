<properties 
    pageTitle="Mi az Azure RemoteApp? | Microsoft Azure" 
    description="Megtudhatja, hogy miként alkalmazások és a bármilyen eszközön keresztül Azure RemoteApp erőforrások megosztása." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="what-is-azure-remoteapp"></a>Mi az Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp felvette a a helyszíni Microsoft RemoteApp program által távoli asztali szolgáltatásokat Azure biztonsági funkciókat. Azure RemoteApp segít biztonságos, távoli hozzáférés biztosítása az alkalmazások számos különböző felhasználói eszközökről. Azure RemoteApp alapvetően tárolja állandó munkamenetet a felhőben, és használhatja őket, és megoszthatja őket a felhasználókkal kap.

Azure RemoteApp megoszthat alkalmazások és az erőforrások szinte bármilyen eszközről felhasználókkal. A felhőben, az alkalmazások akkor üzemeltethet, ami azt jelenti, hogy gondoskodik a hardver és a felhasználók igényeinek kielégítéséhez méretezés. Az összes kell elvégeznie, az alkalmazások megosztani kívánt feltöltése majd beszerzése a felhasználóknak, hogy e-alkalmazások használata. [Felhasználók eléréséhez kattintson a saját eszközök megőrzése](remoteapp-clients.md), amíg minden keresztül az Azure portálon kezelheti. Akkor is, hogy a vállalati hitelesítő adataival, alkalmazások és az adatok biztonsága érdekében, hogy.

További információt az Azure RemoteApp, olvasson tovább, vagy ha azt van már MEGGYŐZŐDVE, akkor [próbálja ki most](https://azure.microsoft.com/services/remoteapp/).

Azure RemoteApp kérdése van? Olvassa el a [Gyakori kérdések](remoteapp-faq.md).

Azure RemoteApp a [Microsoft virtuális asztali infrastruktúra](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx)része.

**Új!** Ha többet szeretne tudni az Azure RemoteApp szeretne? Vagy készen áll a méretezés az Azure RemoteApp érvényesítés? Bekapcsolódás a heti [a szakértőknek webinárium](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## <a name="azure-remoteapp-collections"></a>Azure RemoteApp gyűjtemények
[Azure RemoteApp](remoteapp-collections.md)gyűjtemények két fő típusba sorolhatók:


- **Felhőalapú webhelycsoport** a üzemelteti, és tárolja az adatokat, a program a felhőben. Felhasználók hozzá tudnak férni alkalmazások jelentkezik be a Microsoft-fiók vagy a vállalati hitelesítő adatok szinkronizált vagy az Azure Active Directory címtárral összevont.

    Ha kattintson a felhőben gyűjtemény a megosztani kívánt alkalmazás nem igénylő kapcsolatot bármely erőforrás a vállalat magánhálózaton (például keresztül VPN-eszközhöz). Az alkalmazás az erőforrásokat használó az interneten, a onedrive-on vagy az Azure, ha egy felhőalapú webhelycsoport meg fog működni. Érdemes emellett a leggyorsabb hozhat létre.

- **Hibrid gyűjtemény** a üzemelteti, és tárolja az adatokat az Azure felhőben, de a lehetővé teszi, hogy felhasználók access-adatok és a erőforrásokat a helyi hálózaton tárolt. Felhasználók hozzá tudnak férni alkalmazások jelentkezik be a vállalati hitelesítő adatok szinkronizált vagy az Azure Active Directory címtárral összevont.

    Ha a cége magánhálózaton erőforrások kapcsolat van szüksége, válassza a hibrid gyűjtemény. Ha például az alkalmazás a következő hozzáférésre van szüksége:

    - A vállalati intranethez található fájlkiszolgálókhoz
    - Gyorsabb
    - Adatbázisok tűzfal mögött

    Az általában még hasznosabbá, a saját személyes hálózatok, amelyek nem helyezhetők át a felhőalapú levelezésre erőforrások rengeteg nagyvállalatok számára.

A különböző gyűjtemények különböző beállítások hálózatokat, beleértve a szám, [amely a webhelycsoport](remoteapp-collections.md) a legjobban, hogy rendelkezik. 


### <a name="updating-your-collection"></a>A webhelycsoport frissítése
A főbb különbségek vannak a hibrid és a felhő gyűjtemények között egyik szoftverfrissítések kezelésének módját. Az felhő gyűjteménye, amely a előtelepített Office 365 ProPlus vagy az Office 2013 kép használ nem rendelkezik frissítéseiről aggódnia. A szolgáltatás maga kezeli, és frissítések összesíti az alkalmazások és az operációs rendszer egyaránt folyamatos kombinálásával.

A hibrid gyűjtemények, valamint egy egyéni sablont képet felhő gyűjtemények ajánljuk, akik a kép és az alkalmazások fenntartása. Képek a tartományhoz szabályozhatja, hogy frissítéseket eszközök, például a Windows Update, csoportházirend vagy System Center használatával.

Miután módosította az egyéni sablon képe, akkor az új kép feltöltése a Azure felhőben, és frissítse a a gyűjtemény használni az új kép. (Ehhez az irányítópulton vagy az Azure RemoteApp az **Első lépések** lapon.)

[A webhelycsoport frissítése](remoteapp-update.md) talál további információt.

## <a name="supported-remoteapp-clients"></a>Támogatott RemoteApp ügyfelek
Azure RemoteApp támogatott az RemoteApp ügyfél alkalmazásokat a Windows és a Windows RT rendszerben, valamint a Microsoft távoli asztali alkalmazások Mac, az iOS és az Android rendszerhez. A felhasználók az alkalmazások használata a Mobile vagy új Azure RemoteApp programok eszközök számítja ki.

[Az alkalmazások az Azure RemoteApp elérése](remoteapp-clients.md) talál további információt az ügyfelek.

## <a name="next-steps"></a>Következő lépések
Ugrás a! Próbálja ki! Ezek a cikkek segítséget Azure RemoteApp használatának megkezdéséhez:

- [Milyen típusú webhelycsoport van szüksége az Azure RemoteApp?](remoteapp-collections.md)
- [Hozzon létre egy Azure RemoteApp képe](remoteapp-imageoptions.md)
- [Hogyan hozhat létre az Azure RemoteApp felhő gyűjteménye](remoteapp-create-cloud-deployment.md)
- [Hogyan hozhat létre az Azure RemoteApp hibrid gyűjteménye](remoteapp-create-hybrid-deployment.md)
- [Licencelési működése az Azure RemoteApp](remoteapp-licensing.md)
- [Ajánlott eljárások az Azure RemoteApp használatához](remoteapp-bestpractices.md)
- [Azure RemoteApp – gyakori kérdések](remoteapp-faq.md)
 

### <a name="help-us-help-you"></a>Segítse a szolgáltatás segítségével 
Tudta, hogy mellett ez a cikk minősítés, és a megjegyzések megadására lefelé alatt, módosíthatja a cikkben magát? Valami hiányzó? Valami hiba történt? DID lehet Írjon valamit, amely csak áttekinthetőbb legyen? Görgetés felfelé, majd kattintson **a GitHub szerkeszteni** vagy a **Szerkesztés** módosításához - e érkezzenek us véleményezésre, és majd, ha azt jelentkezzen, a őket, látni fogja a módosításokat, és itt javítása.