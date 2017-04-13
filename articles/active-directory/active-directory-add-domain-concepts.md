<properties
    pageTitle="Egyéni tartományneveket az Azure Active Directory szolgáltatásainak elvi áttekintése |} Microsoft Azure"
    description="Ebből a cikkből megtudhatja elvi keretében egyéni tartományneveket az Azure Active directory összevonási az egyszeri bejelentkezési együtt"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Egyéni tartományneveket az Azure Active Directory szolgáltatásainak elvi áttekintése

A tartománynév része fontos sok címtár erőforrás azonosítóját: a felhasználó nevét vagy e-mail címét a felhasználó, csoport, a cím része szerepel, és az alkalmazás azonosítója URI az alkalmazás részei lehetnek. Egy erőforrás az Azure Active Directory (Azure Active Directory) is elhelyezhet, amely már ellenőrzött lesz a címtárban, amely tartalmazza az erőforráshoz tartozó tartománynév. Csak a globális rendszergazda az Azure Active Directory tartományi felügyeleti feladatok végezheti el.

Azure Active Directory tartományi nevek globálisan egyedi. Egy egyetlen Azure AD egy tartománynevet használhatja. Ha egy Azure Active directory ellenőrizte a megfelelő tartományt, majd nincs Azure Active Directory könyvtár is ellenőrzése vagy a ugyanarra a tartománynévre.

## <a name="initial-and-custom-domain-names"></a>Eredeti és az egyéni tartomány neve

Minden tartománynevet az Azure Active Directory-eredeti tartománynevet, vagy a saját tartománynév.

Minden Azure AD-az űrlap contoso.onmicrosoft.com az eredeti tartománynevet megtalálható. A harmadik szintű tartománynevet, ebben a példában "contoso.onmicrosoft.com," hozta létre, amikor a címtárban hozta létre, általában a rendszergazda a címtárban létrehozó. Az eredeti tartománynevet könyvtár nem változott, és nem törölhető. Az eredeti tartománynevet, miközben a teljes funkcionalitású célja elsősorban használható egy bootstrapping mechanizmusként, amíg az egyéni tartománynév ellenőrzött lesz.

A legtöbb gyártási környezetben könyvtárában legalább egy igazolt egyéni tartományneve van, (például "contoso.com),", és azt a végfelhasználók számára látható egyéni tartományt. Egyéni tartománynevet (például "contoso.com),", a szervezet által használt, és saját tartománynév helyett felhasználásra, például a saját webhely szolgáltatója. Ezt a tartománynevet oka az alkalmazottak ismerős a felhasználó nevét, bejelentkezni a vállalati hálózathoz, vagy a Küldés és letöltéséhez e-mail használó része.

Azure Active Directory használható, mielőtt az egyéni tartománynevet kell hozzáadni a címtárában és ellenőrzött.

## <a name="verified-and-unverified-domain-names"></a>Igazolt és nem ellenőrzött tartománynevek

Az eredeti tartománynevet könyvtár implicit módon kiértékeli Azure Active Directory által ellenőrzött. Rendszergazda egyéni tartománynév hozzáadása az Azure AD, amikor először állapotban van egy nem ellenőrzött. Azure Active Directory nem teszi lehetővé a címtár erőforrások egy nem ellenőrzött tartománynevet szeretne használni. Ez biztosítja, hogy csak egy címtár egy adott tartománynevet használhatja, és a ténylegesen, hogy a szervezete használ-e a tartomány tulajdonosa, hogy a tartománynév.

Azure Active Directory ellenőrzi a tartománynév tulajdonjogának, hogy egy adott bejegyzést a tartomány nevét (DNS) szolgáltatás zónafájl tartománynév keres. A tartománynév tulajdonjogának ellenőrzéséhez rendszergazda megszerzi a DNS-bejegyzés Azure Active Directory Azure Active Directory fog keresni, és a bejegyzés hozzáadása a DNS-zónafájl a tartománynév. A tartománynév-regisztráló az adott tartományhoz tartozó kezeli a DNS-zónafájl. A tartomány hitelesítése a lépéseit a cikk [az Azure Active directory szeretne egyéni tartományt](active-directory-add-domain.md)vehet fel.

A DNS-bejegyzés hozzáadása a zónafájlba tartományneve más tartományi szolgáltatások, például az e-mailbe vagy a webes szolgáltatója nem befolyásolja.

## <a name="federated-and-managed-domain-names"></a>A szövetséges és felügyelt tartománynevek

Egyéni tartománynevet az Azure Active Directory beállítható úgy, hogy a felhasználók számára lehetőséget adni egy összevont bejelentkezési között a helyszíni Active Directory és Azure Active Directory-asként. Frissítések az Azure Active Directory jogosultsággal rendelkező erőforrások és a Windows Server Active Directory összevonási a tartomány konfigurálása szükséges. A szövetséges tartományban kell elvégezni az Azure AD Connect konfigurálása, vagy a PowerShell használatával. Egyéni tartomány összevonása az Azure klasszikus portálról nem tud kezdeményezni. [Ebből a videóból megismerheti az AD FS-felhasználó be kell jelentkeznie a Azure AD Connect konfigurálását](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect).

Tartományok, amelyek nem szövetséges tartozó felügyelt tartományok is nevezik. Az eredeti tartománynak a Azure AD-hez implicit módon kiértékeli a felügyelt tartománynak.

## <a name="primary-domain-names"></a>Elsődleges tartománynevek

Elsődleges tartománynévnek könyvtár a tartomány nevét, amikor egy rendszergazda hoz létre új felhasználót az [Azure klasszikus portál](https://manage.windowsazure.com/) vagy egy másik, például az Office 365 felügyeleti központból portál előre az alapértelmezett érték, a felhasználónév "tartomány" részének kijelölt. A könyvtár beállíthatja, hogy csak egy elsődleges tartománynévnek. A rendszergazda módosíthatja bármely ellenőrizve, amely nem szövetséges, egyéni tartományt kell elsődleges tartománynévnek, illetve az eredeti tartománynak.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>A tartománynevek az Azure Active Directory és a többi Microsoft Online Services

A tartománynév ellenőriznünk kell az Azure Active Directory ahhoz, hogy egy másik Microsoft Online szolgáltatás például az Exchange Online, a SharePoint Online és Intune kell használni. Ezek a szolgáltatások általában szükség van a szolgáltatás adott egy vagy több DNS-bejegyzések hozzáadása a rendszergazda.

Az Azure-webappokban saját mechanizmusa használja a tartomány tulajdonjogának igazolása. Akkor is, ha korábban már ellenőrizte használatra, amely a Azure Active Directory támaszkodik előfizetés-Azure webes alkalmazás által Azure Active Directory való használatra ellenőriznünk kell tartományt. Az Azure web app a címtárból, hogy titkosítja a web app különböző könyvtárában található ellenőrizte tartománynév helyett használható.

## <a name="managing-domain-names"></a>A tartománynevek kezeléséhez

Tartomány adatkezelési feladatok hajtható az Azure klasszikus portálról, és a PowerShell. Számos feladat az Azure Active Directory Graph API (használata nyilvános előzetes verzió) hajtható végre.

-   [Hozzáadás és egyéni tartománynevet ellenőrzése](active-directory-add-domain.md)

-   [Kezelheti a tartományokat az Azure klasszikus portálon](active-directory-add-manage-domain-names.md)

-   [A tartománynevek kezeléséhez az Azure Active Directory PowerShell használatával](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Az Azure Active Directory Graph API segítségével kezelheti a tartományneveket az Azure Active Directory](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)
