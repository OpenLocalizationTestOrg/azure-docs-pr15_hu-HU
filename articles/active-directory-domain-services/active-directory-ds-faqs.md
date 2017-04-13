<properties
    pageTitle="Gyakori kérdések – Azure Active Directory tartományi szolgáltatások |} Microsoft Azure"
    description="Gyakori kérdések a Azure Active Directory tartományi szolgáltatások"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure Active Directory tartományi szolgáltatások: Gyakori kérdések

Ezen az oldalon az Azure Active Directory tartományi szolgáltatások kapcsolatos gyakori kérdésekre ad választ. Tartsa vissza frissítések keresése.

### <a name="troubleshooting-guide"></a>Hibaelhárítási útmutatója
Olvassa el a [Hibaelhárítás útmutató](active-directory-ds-troubleshooting.md) konfigurálásakor és Azure Active Directory tartományi szolgáltatások felügyelete észlelt gyakori problémák megoldásainak.


### <a name="configuration"></a>Konfiguráció

#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory"></a>Létrehozhatok-e több tartomány egyetlen az Azure Active directory?
nem. Csak egyetlen tartomány egyetlen az Azure Active Directory tartományi szolgáltatások kiszolgált hozhat létre Azure Active directory.  

#### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Az erőforrás-kezelő Azure virtuális hálózat Azure Active Directory tartományi szolgáltatások engedélyezése
nem. Azure Active Directory tartományi szolgáltatások csak engedélyezhető klasszikus Azure virtuális hálózatban. A klasszikus virtuális hálózat csatlakozhat egy erőforrás-kezelő virtuális hálózaton, az erőforrás-kezelő virtuális hálózatban felügyelt tartomány használatára peering virtuális hálózaton keresztül.

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Tehetem Azure Active Directory tartományi szolgáltatásokban elérhető több virtuális hálózatok belül az előfizetésem?
A szolgáltatás magát közvetlenül nem támogatja a ebben az esetben. Azure Active Directory tartományi szolgáltatások érhető el a csak egy virtuális hálózati egyszerre. Virtuális hálózatok összevonása kattintva jelenítse meg az Azure Active Directory tartományi szolgáltatások, a többi virtuális hálózatokhoz közötti kapcsolatot is beállíthatja. Ez a cikk azt ismerteti, hogyan dolgozhat [az Azure virtuális hálózatok csatlakoztatása](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>A PowerShell használatával Azure Active Directory tartományi szolgáltatások engedélyezése
Azure Active Directory tartományi szolgáltatások telepítésének PowerShell/automatikus jelenleg nem érhető el.

#### <a name="is-azure-ad-domain-services-available-in-the-new-azure-portal"></a>Azure Active Directory tartományi szolgáltatások érhető el az új Azure-portálon?
nem. Azure Active Directory tartományi szolgáltatások csak az [Azure klasszikus portál](https://manage.windowsazure.com)beállíthatók. Az [Azure portál](https://portal.azure.com) támogatása a jövőben meghosszabbítása megakadályozási.

#### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Vehetek fel tartomány vezérlők az Azure Active Directory tartományi szolgáltatások felügyelt tartomány?
nem. Azure Active Directory tartományi szolgáltatások által biztosított tartománya felügyelt tartományt. Nem kell rendelkezni, konfigurálása vagy egyéb módon kezelje a tartomány vezérlők a tartományhoz – ezek a fájlkezelési műveletekhez szolgáltatás által biztosított Microsoft. Emiatt nem vehet fel további tartományt vezérlők (csak olvasásra vagy csak olvasható) a felügyelt tartomány.

### <a name="administration-and-operations"></a>Igazgatási és műveletek

#### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Tudok csatlakozni a tartományvezérlőnek a távoli asztali változatában felügyelt tartomány?
nem. Nincs engedélye a csatlakozás távoli asztali keresztül felügyelt tartományhoz tartozó domain vezérlők. A "AAD Adatközpont rendszergazdák" csoport tagjainak felügyelheti a felügyelt tartományt AD felügyeleti eszközök, például az Active Directory – felügyeleti központ (ADAC) vagy az Active Directory PowerShell használatával. Ezek az eszközök vannak telepítve a "Távoli kiszolgáló felügyeleti eszközei" funkció használata a Windows server a felügyelt tartományhoz.

#### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>E már engedélyezni Azure Active Directory tartományi szolgáltatások. Milyen felhasználói fiók segítségével tartomány illesztés gépek ezt a tartományt?
A felügyeleti csoporthoz (például "AAD Adatközpont rendszergazdák") felvett felhasználók tartomány illesztés gépek. Emellett a felhasználók ebben a csoportban gépek, hogy a tartományba csatlakoztak távoli asztali hozzáférést kapnak.

#### <a name="can-i-wield-domain-administrator-privileges-for-the-domain-provided-by-azure-ad-domain-services"></a>Azure Active Directory tartományi szolgáltatások által biztosított tartományhoz tartozó domain rendszergazdai jogosultságokkal is wield?
nem. A felügyelt tartományban rendszergazdai jogokkal nem kapnak. Tartományi rendszergazdája és a "vállalati rendszergazda" jogosultsággal nem érhetők el szeretne használni a tartományban. Meglévő tartományi rendszergazdája vagy vállalati rendszergazdai csoportok belül az Azure Active directory is nem kapnak tartomány vagy vállalati rendszergazdai jogosultságokkal a tartományban.

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-domains-provided-by-azure-ad-domain-services"></a>Módosíthatja a csoporttagság LDAP vagy más AD felügyeleti eszközök használata az Azure Active Directory tartományi szolgáltatások által biztosított tartományok?
nem. A tartományok Azure Active Directory tartományi szolgáltatások által kiszolgált csoporttagság nem módosítható. Felhasználói attribútumok ugyanúgy vonatkoznak. Azonban lecserélheti csoporttagság vagy felhasználói attribútumok az Azure Active Directory vagy a helyszíni tartomány. Ezek a változások a rendszer automatikusan szinkronizálja az Azure Active Directory tartományi szolgáltatások.

#### <a name="can-i-extend-the-schema-of-the-domain-provided-by-azure-ad-domain-services"></a>Bővíthetik a séma Azure Active Directory tartományi szolgáltatások által megadott tartomány?
nem. A séma Microsoft kezeli a felügyelt tartomány. Séma bővítmények Azure Active Directory tartományi szolgáltatások által nem támogatott.

#### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Módosítani vagy DNS-rekordok hozzáadása a felügyelt tartomány?
igen. A "AAD Adatközpont rendszergazdák" csoportba tartozó felhasználók kapnak DNS rendszergazdája jogosultságok a felügyelt tartomány DNS-rekordok módosítását. Ezek a felhasználók a DNS-kezelőben konzol segítségével a tartományhoz felügyelt, Windows Server operációs rendszert futtató számítógépen DNS kezelése. A DNS-kezelőben konzol használatához telepítenie "DNS-kiszolgálói eszközök", a kiszolgáló a "Távoli kiszolgáló felügyeleti eszközei" választható funkció részét képező. [Felügyelete, a figyelés és a DNS hibaelhárítási segédprogramok](https://technet.microsoft.com/library/cc753579.aspx) további információt a TechNet webhelyen érhető el.


### <a name="billing-and-availability"></a>Számlázás és elérhetősége

#### <a name="is-azure-ad-domain-services-a-paid-service"></a>Az Azure Active Directory tartományi szolgáltatások fizetett szolgáltatás?
igen. További tudnivalókért olvassa el a [árak, oldal](https://azure.microsoft.com/pricing/details/active-directory-ds/)című témakört.

#### <a name="is-there-a-free-trial-for-the-service"></a>Van-e az ingyenes próbaverzióra, a szolgáltatás?
Ez a szolgáltatás a próba-előfizetésre az Azure is tartalmazni fogja. [Azure hónap ingyenes próbaverziója](https://azure.microsoft.com/pricing/free-trial/)jelentkezhetnek.

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems"></a>Hogyan juthatok Azure Active Directory tartományi szolgáltatások keretében vállalati mobilitás csomagja (EMS)?
#### <a name="do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Azure Active Directory prémium Azure Active Directory tartományi szolgáltatások használatára van szükség?
nem. Azure Active Directory tartományi szolgáltatások egy kirovó Azure szolgáltatás, és nem EMS részét. Azure Active Directory tartományi szolgáltatások érhetők el az Azure Active Directory kiadásainak (ingyenes, egyszerű, és Premium), és vannak számlázható óránkénti kombinálásával, attól függően, hogy használatát.

#### <a name="what-azure-regions-is-the-service-available-in"></a>Milyen Azure régióban érhető el a szolgáltatás?
Keresse meg a [Régió szerint Azure-szolgáltatások](https://azure.microsoft.com/regions/#services/) lapon, ahol Azure Active Directory tartományi szolgáltatások áll rendelkezésre az Azure régiók listájának megtekintéséhez.
