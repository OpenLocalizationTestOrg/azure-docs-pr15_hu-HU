<properties
    pageTitle="Az Outlook használata Azure RemoteApp |} Microsoft Azure" 
    description="Megtudhatja, hogy miként beállítása és használata az Outlook az Azure RemoteApp |} Microsoft Azure"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>A Microsoft Outlook használata Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp támogatja a Microsoft Outlook O365. További információ a hogyan [Office Azure RemoteApp működik](remoteapp-officesubscription.md). Létezik néhány ajánlott Azure RemoteApp használatakor az Outlook beállításai.

## <a name="cached-mode"></a>Gyorsítótárazott
Gyorsítótárazott ajánlott konfiguráció esetén az Azure RemoteApp az Outlookkal. Gyorsítótáras Exchange üzemmód használata az Outlook 2013-ban fiók beállításakor a felhasználó Microsoft Exchange-postaláda a felhasználó számítógépén a kapcsolat nélküli cím címjegyzék (Címjegyzék) együtt-kapcsolat nélküli adatok fájlban (OST-fájlt) tárolt egy helyi példányt működik az Outlook 2013-ban. A gyorsítótárban tárolt postaláda és a Címjegyzék rendszeresen frissül az Office 365 szolgáltatásból. További információ [a különbségeket gyorsítótárazott és az online üzemmód között](https://technet.microsoft.com/library/jj683103.aspx).

A felhasználó **a gyorsítótáras Exchange üzemmód** vagy az **Online üzemmód** fiókbeállítás alatt vagy választhatja a fiók beállításainak módosításával. Az Office testreszabási eszköze vagy csoportházirendet alkalmaz egy mód vagy a többi is telepítheti.  

Olvassa el a [gyorsítótárban tárolt mód engedélyezése lépésenkénti útmutatást](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## <a name="search"></a>Keresés
Azure RemoteApp, az Outlook Keresés korlátai. Azure RemoteApp használja a készletezett VMs felhasználói munkamenetek igazodik. Keresési indexelés attól függ, hogy a gép azonosító, amely címzettenként különböző lesz különböző VMs. Akkor lehet, hogy minden olyan alkalommal, amikor a felhasználó bejelentkezik az Azure RemoteApp, azok irányítja át egy új virtuális. Amely volna középérték, ha engedélyezi a helyi keresés, hogy az indexelő futtathatók minden alkalommal, amikor a gép azonosító (Ha a felhasználó egy másik virtuális) változik. Méretétől függően a. OST-fájlt, az indexelő befejezéséhez és egyéb alkalmazások szükséges erőforrásokat használata hosszú időt is eltelhet. Keresés nem csak lenne lassú, de esetleg nem hoz eredményt. Az Online üzemmód fiók profillal volna kerülhető meg, de általános teljesítményére volna érheti helyi gyorsítótár hiánya miatt (lásd: a hivatkozás fölé további információt a különbség a gyorsítótárban tárolt és az online üzemmód között). Sajnos nem lehet letiltani a keresés/helyi indexelt, és az online keresés nem engedélyezhető az Outlook 2013 alapértelmezés szerint.
