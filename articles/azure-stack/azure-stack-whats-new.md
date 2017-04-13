<properties
    pageTitle="Azure Papírhalom újdonságai |} Microsoft Azure"
    description="Azure Papírhalom újdonságai"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="whats-new-in-azure-stack-technical-preview-2"></a>Azure Papírhalom technikai előzetes verzió 2 újdonságai
Ebben a kiadásban bérlők és a rendszergazdák új szolgáltatásokat nyújt.

## <a name="network"></a>Hálózati   
   - [IDN](azure-stack-understanding-dns-in-tp2.md) belső hálózati név regisztráció és a tartománynév (DNS) felbontása nélkül további DNS-infrastruktúrát biztosít.
   - [Virtuális hálózati átjárók](azure-stack-create-vpn-connection-one-node-tp2.md) virtuális Magánhálózati kapcsolat lehetőségeket Azure vagy a helyszíni erőforrások adnak.
   - Felhasználó által definiált útvonalak keresztül tűzfalak, biztonsági, más berendezések és más szolgáltatások hálózati forgalmat is.
   - Hálózati erőforrások hozhat létre a piactérről.   

## <a name="storage"></a>Tárhely
 - [Azure sorban várakozó](https://msdn.microsoft.com/library/dd179353.aspx) megbízható és állandó szolgáltatás üzenetküldés engedélyezése.
 - [Tárterület analytics](https://msdn.microsoft.com/library/azure/hh343270.aspx) rögzítési tároló teljesítményadatokat. Az adatok segítségével nyomon követése kérések használatát trendek elemzése és problémáinak diagnosztizálása a tárterület-fiókjával.
 - BLOB-tárolóhoz támogatja a [Továbbfejlesztett fájlblokkolás művelet hozzáfűzése](https://msdn.microsoft.com/library/azure/mt427365.aspx).
 - Prémium tároló API-val fiók támogatása.
 - Bérlői tárterület szolgáltatás támogatása közös eszközök és SDK, például Azure CLI, PowerShell, .NET, Python és Java SDK csomagjában talál. 
 - [Tárterület fiók megosztott Access aláírás](https://msdn.microsoft.com/library/azure/mt584140.aspx) részletes delegálás a tárhely szolgáltatások való hozzáférés engedélyezése a anélkül, hogy a teljes fiókkulcs megosztása.  
 - Tárterület-szolgáltatások [Felügyelt szolgáltatás csoportfiókok](https://technet.microsoft.com/library/hh831477.aspx) most alacsony management terhelést erős értékpapír használja.
 - Használaton kívüli bérlői erőforrások igény szerinti is visszanyeréséhez.
 - A törölt tároló fiók adatmegőrzési hossza állítható be.
 - Visszaállíthatja a törölt bérlői tárterület fiókok.

## <a name="compute"></a>Számítási
- Virtuális gépeken futó is felszabadítani.
- Hibaelhárítás és konfigurációs kezelése céljából virtuális gép bővítmények is telepítsen újra.
- Virtuális gép lemezt méretezheti át.
- Virtuális gépeken futó beállíthatja, hogy több hálózati kapcsolaton.

### <a name="portal-experience"></a>Portál élmény
 - Azure Papírhalom régiók olyan logikai egység méretezése és kezelése Azure Papírhalom belül. Ebben a villámnézetben szolgáltatásai, például a számítási, hálózati és régió szerint tároló információkat is megtekintheti.
 - Most már a (frissítés) [azure Papírhalom, updates.md] felület tekintheti meg.

## <a name="key-vault"></a>Fő tárolóból elemre
- [Azure egymást fedő kulcs tárolóra](azure-stack-kv-intro.md) biztonságos kezelése a kulcsok és a felhő alkalmazások jelszavának biztosít.
- Naplózási, és a Lync-alkalmazások és VMs kulcs használatát.

## <a name="billing-and-usage"></a>Számlázás és használata
 - [Számlázást és az API-khoz felhasználás](azure-stack-billing-and-chargeback.md) elérhetővé meg a szolgáltatások felhasznált hogyan.  
 - Delegált szolgáltatók engedélyezése a Azure Papírhalom szolgáltatások kínálatát ügyfeleik viszonteladói.

## <a name="monitoring-and-health"></a>Figyelés, és állapota
 - Új felügyeleti lehetőségeket a portál és az API-khoz ezzel kapcsolatban beérkező megtekintése és kezelése a környezettől értesítéseket teszi lehetővé.  

## <a name="next-steps"></a>Következő lépések
- [Azure Papírhalom ez architektúráját ismertetése](azure-stack-architecture.md)      
- [Telepítési előfeltételek ismertetése](azure-stack-deploy.md)
- [Azure Papírhalom terjesztése](azure-stack-run-powershell-script.md)

  
