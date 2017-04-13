<properties
    pageTitle="Mit tartalmaz az Azure RemoteApp a webhelysablonok képét? | Microsoft Azure"
    description="Tudjon meg többet az Azure RemoteApp részét képező a webhelysablonok képét."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="what-is-in-the-azure-remoteapp-template-images"></a>Mit tartalmaz az Azure RemoteApp a webhelysablonok képét?

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp előfizetése tartalmazza a három a webhelysablonok képét:


- A Windows Server 2012
- A Microsoft Office 365 ProPlus (az Office 365-előfizetéshez szükséges)
- A Microsoft Office 2013 Professional Plus (csak a próbaverzió)

> [AZURE.IMPORTANT]Az Azure RemoteApp előfizetés biztosít, és elérheti a szoftver, a képeket, az Office 365 ProPlus, amelyek külön előfizetést igényel, és az Office 2013, mely nem használhatók a termelési kivételével. Ez azt jelenti, hogy bárkivel megoszthatja a programokat, vagy a webhelysablonok képét-alkalmazásokat, a felhasználók számára. Például ha használja a Windows Server 2012 R2 képét rendeltetésre hoz létre, közzéteheti System Center Endpoint Protection RemoteApp keresztül elérje a felhasználók számára.
>
> Nézze meg a [Távoli alkalmazás formájában használva licencelési részletei](remoteapp-licensing.md) további információt. [Azure RemoteApp az Office használata](remoteapp-o365.md) az Office-licencek információ és.

A cikkben olvashat részletesen minden képet tartalmazza.

## <a name="windows-server-2012-r2--the-vanilla-image"></a>A Windows Server 2012 R2 ("vanília kép")
Ezen a képen a Microsoft Windows Server 2012 R2 adatközponthoz operációs rendszere alapján, és az alábbi szerepkörök és az Azure RemoteApp sablonképek követelményeknek telepített szolgáltatások:


- .NET-keretrendszer 4.5, 3.5.1-es, 3.5-ös
- Az asztali élmény
- Szabadkézi elemek és kézírás-szolgáltatások
- Multimédia alaprendszer
- Távoli asztali munkamenetgazda
- A Windows PowerShell 4.0
- A Windows PowerShell ISE
- WoW64 támogatás

Ezen a képen van telepítve van a következőkre is:

- Adobe Flash Player
- A Microsoft Silverlight
- A Microsoft System Center 2012 Endpoint Protection
- Microsoft Windows Media Player


## <a name="microsoft-office-365-proplus-subscription-required"></a>A Microsoft Office 365 ProPlus (előfizetés szükséges)
Az Office 365 a helyzet a legtöbb kért alkalmazás létrehozott egy "custom" képet kezelheti.

Ezen a képen a vanília kép bővítménye, és a Microsoft Office 365 ProPlus telepítve van az összetevők, a Windows Server 2012 R2 kép ismertetett kívül az alábbi összetevőit tartalmazza:


- Az Access
- Excel
- A Lync
- OneNote-ban
- A OneDrive vállalati verzió (figyelje meg, hogy a szinkronizálási agent Azure RemoteApp használata nem támogatott)
- Az Outlook
- A PowerPoint
- A Word
- A Microsoft Office nyelvi ellenőrző eszközök

A kép a Visio Pro és a Project Pro is tartalmaz.

És a következő alkalmazásokat, valamint:

- SQL natív ügyfele
- ODBC-illesztő
- Az SQL Server adatbányászati ügyfél
- MasterDataServices ügyfél
- A Microsoft Publisher
- PowerQuery
- Power Mapbe


Az Office 365 ProPlus-alkalmazások teljes funkció használható, csak azoknak a felhasználóknak, akik az Office 365 ProPlus-csomaggal rendelkezik-e. Az Office 365-ös előfizetési csomagok lásd: [az Office 365 szolgáltatás tervek](http://technet.microsoft.com/library/office-365-plan-options.aspx). További kérdései vannak? Nézze meg az [Office 365 + RemoteApp](remoteapp-o365.md) információkat. Is nézze meg az új hozzászólás [használata az Office 365-előfizetés Azure RemoteApp](remoteapp-officesubscription.md).

Figyelje meg, hogy az Office 365 ProPlus, a Visio Pro és a Project Pro külön-külön licenc szükséges,-egyes rendelkeznek a saját licencet.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>A Microsoft Office 2013 Professional Plus (csak a próbaverzió)
Ingyenes próbaverzió időszak alatt a szolgáltatást az Office 2013 képpel tesztelheti.

Ezen a képen a vanília kép bővítménye, és a Microsoft Office 2013 Professional Plus telepítve van az összetevők, a Windows Server 2012 R2 kép ismertetett kívül az alábbi összetevőit tartalmazza:


- Az Access
- Excel
- A Lync
- OneNote-ban
- A OneDrive vállalati verzió (figyelje meg, hogy a szinkronizálási agent Azure RemoteApp használata nem támogatott)
- Az Outlook
- A PowerPoint
- Projekt
- A Visio
- A Word
- A Microsoft Office nyelvi ellenőrző eszközök

> [AZURE.IMPORTANT]**Jogi információk:** Ezen a képen nem része a Microsoft Office-licenc és *gyártási nem használhatók*. Az Office 2013 Professional Plus kép csak kipróbálásra szól. Ha szeretne Office-alkalmazások használata az Azure RemoteApp gyártási, akkor az Office 365 ProPlus kép használata. Az Office licencelési, lásd: [az Office 365 használata Azure RemoteApp](remoteapp-o365.md)
