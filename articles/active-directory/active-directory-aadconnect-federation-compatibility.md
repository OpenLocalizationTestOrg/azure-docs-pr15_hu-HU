<properties
    pageTitle="Azure Active Directory összevonási kompatibilitási listája"
    description="Ezen az oldalon az egyszeri bejelentkezés végrehajtásához használt-Microsoft Identitásszolgáltatók tartalmaz."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="billmath"/>

# <a name="azure-ad-federation-compatibility-list"></a>Azure Active Directory összevonási kompatibilitási listája
Azure Active Directory Ez a témakör egyszeri bejelentkezés, és a bővített az alkalmazást az access biztonsági az Office 365 és az egyéb Microsoft Online services hibrid és a felhőben megvalósítás anélkül, hogy a Microsoft megoldások. Az Office 365-be, például a Microsoft Online services, a legtöbb integrálva van Azure Active Directory címtárszolgáltatással, hitelesítési és engedélyezési. Azure Active Directory is biztosít az egyszeri bejelentkezés szoftver alkalmazások ezer, és a helyszíni webalkalmazások. Olvassa el az Azure Active Directory-alkalmazás gyűjtemény támogatott szoftver alkalmazások.

Azoknak a szervezeteknek szól, amelyek az összevonási nem Microsoft-megoldások fekteti van ez a témakör tartalmaz útmutatást konfigurálja az egyszeri bejelentkezés a Windows Server Active Directory-felhasználóknak a Microsoft Online services használatával nem Microsoft Identitásszolgáltatók "Azure Active Directory összevonási kompatibilitási"alábbi listából. 


![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
A vizsgált ezek egyszeri bejelentkezéses szolgáltatások összehasonlítja a közös használat esetek a Microsoft Identitásszolgáltatók használata az Azure Active Directory [Oxford számítógép csoport](http://oxfordcomputergroup.com/), harmadik fél, nevében, a Microsoft.

Arról olvashat, hogy hogyan érheti el, az itt szereplő külső identitásszolgáltató, lépjen kapcsolatba a Oxford számítógép csoport [idp@oxfordcomputergroup.com](mailto:idp@oxfordcomputergroup.com).

>[AZURE.IMPORTANT] Oxford számítógép csoport tesztelte a csak ezek az egyszeri bejelentkezési forgatókönyvek összevonási funkcióit. Nem hajtotta végre Oxford számítógép csoport bármilyen teszteli a szinkronizálást, kétfaktoros hitelesítés, ezek az egyszeri bejelentkezési forgatókönyvek stb összetevői.

>Bejelentkezés UPN alternatív azonosító szerint felhasználása nem is tesztelte a programban.



- [Azure Active Directory](#azure-active-directory)
- [Optimális IDM virtuális identitás kiszolgáló összevonási szolgáltatásokat.](#optimal-idm-virtual-identity-server-federation-services) 
- [PingFederate 6.11](#pingfederate-611) 
- [PingFederate 7.2.](#pingfederate-72) 
- [PingFederate 8.x](#pingfederate-8x)
- [Centrify](#centrify) 
- [IBM Tivoli összevont identitáskezelő 6.2.2](#ibm-tivoli-federated-identity-manager-622) 
- [SecureAuth IdP 7.2.0](#secureauth-idp-720) 
- [Hitelesítésszolgáltató SiteMinder 12.52](#ca-siteminder-1252-sp1-cumulative-release-4) 
- [RadiantOne CFS 3.0](#radiantone-cfs-30) 
- [Okta](#okta) 
- [OneLogin](#onelogin) 
- [NetIQ Access Manager 4.0.1](#netiq-access-manager-401) 
- [NAGY IP-az Access házirendkezelő nagy IP-ver. 11.3 x – 11.6 x](#big-ip-with-access-policy-manager-big-ip-ver-113x-116x) 
- [VMware munkaterület portál 2.1-es verziójában](#vmware-workspace-portal-version-21) 
- [Jelentkezzen be, és válassza a 5.3.](#signampgo-53) 
- [IceWall összevonási 3.0-s verzió](#icewall-federation-version-30) 
- [Hitelesítésszolgáltató biztonságos felhő](#ca-secure-cloud) 
- [Dell egy identitáskezelő felhő Access v7.1](#dell-one-identity-cloud-access-manager-v71) 
- [4.5 AuthAnvil egyetlen bejelentkezés](#authavil-single-sign-on-45) 

>[AZURE.IMPORTANT] Ezek a harmadik féltől származó termékeket, mivel a Microsoft nem nyújt támogatást a kiépítési, konfigurációs, hibaelhárítás, ajánlott eljárások, stb problémák és ezek Identitásszolgáltatók kapcsolatos kérdések. Támogatási és ezek Identitásszolgáltatók kapcsolatos kérdések közvetlenül kapcsolatba lépni a támogatott külső felek.

>A külső Identitásszolgáltatók interoperability voltak vizsgálni a Microsoft felhőszolgáltatásaihoz Webszolgáltatás-Összevonás és csak az adatvédelmi WS protokollok használatával. Tesztelés nem tartalmazta a SAML protokoll használatával.

## <a name="azure-active-directory"></a>Azure Active Directory 
Azure Active Directory hitelesíthet felhasználókat a helyszíni Active Directory vagy a jegyzetek nélkül egy helyszíni összevonási kiszolgálón való jelszó-szinkronizálás összevonása által. 

Az alábbiakban a bejelentkezéses megoldást forgatókönyvet támogatja mátrixát: 


| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nincs lehetőség|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban |  Támogatott |Nincs lehetőség|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|
|Modern alkalmazásokat, például az Office 2016 ADAL használatával| Támogatott|Nincs lehetőség|

Azure Active Directory az AD FS használatáról további információt talál [Az Active Directory összevonási szolgáltatások (ADFS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)

A jelszó-szinkronizálás Azure Active Directory használatáról további információt talál az [Azure AD Connect](active-directory-aadconnect.md).


## <a name="optimal-idm-virtual-identity-server-federation-services"></a>Optimális IDM virtuális identitás kiszolgáló összevonási szolgáltatásokat. 
Optimális IDM virtuális identitás kiszolgáló összevonási szolgáltatások hitelesíthet a felhasználóknak, hogy az ügyfelek a helyszíni Active könyvtárak tárolnak.

Az alábbiakban a forgatókönyvet támogatja a mátrix a egyszeri bejelentkezéses megoldást:


| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nincs lehetőség|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban |  Támogatott |Integrált Windows-hitelesítés|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Az ügyfélhozzáférés további információkért lásd: házirendeket [a hozzáférés korlátozása az Office 365 szolgáltatás helye alapján az ügyfél.](https://technet.microsoft.com/library/hh526961.aspx)|



## <a name="pingfederate-611"></a>PingFederate 6.11 

PingFederate 6.11 egyszeri bejelentkezés és attribútum exchange keretrendszer megadására elterjedt Webszolgáltatás összevonás identitás standard hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát:


| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nincs lehetőség|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban |  Támogatott |NONE (korábbi verzióiban frissítenie kell az 6.11|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|

E beállítás a PingFederate útmutatást STS ahhoz, hogy az egyszeri bejelentkezéses megoldást az Active Directory-felhasználók, töltse le a PDF-fájl [itt.](http://go.microsoft.com/fwlink/?LinkID=266321)

## <a name="pingfederate-72"></a>PingFederate 7.2. 
PingFederate 7.2 egyszeri bejelentkezés és attribútum exchange keretrendszer megadására elterjedt összevonási/WS – adatvédelmi WS identitás standard hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát:


| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nincs lehetőség|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban |  Támogatott |Nincs lehetőség|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|

A PingFederate beállításáról a STS ahhoz, hogy az egyszeri bejelentkezéses megoldást az Active Directory-felhasználók, tanulmányozza [itt.](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)

## <a name="pingfederate-8x"></a>PingFederate 8.x 
PingFederate 8.x egyszeri bejelentkezés és attribútum exchange keretrendszer megadására elterjedt összevonási/WS – adatvédelmi WS identitás standard hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát:


| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nincs lehetőség|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban |  Támogatott |Nincs lehetőség|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|

A PingFederate beállításáról a STS ahhoz, hogy az egyszeri bejelentkezéses megoldást az Active Directory-felhasználók, tanulmányozza [itt.](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="centrify"></a>Centrify 
Centrify segít a szövetséges egyszeri bejelentkezéses megoldást az Office 365-höz egy helyszíni összevonási kiszolgáló-annak követelménye nélkül.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát:


| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nincs lehetőség|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban |  Támogatott |Nincs lehetőség|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Ügyfél-hozzáférési nem támogatott 

Centrify kapcsolatos további tudnivalókért lásd: [itt.](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp)|

## <a name="ibm-tivoli-federated-identity-manager-622"></a>IBM Tivoli összevont identitáskezelő 6.2.2 
Az IBM Tivoli összevont identitáskezelő 6.2.2 az IBM biztonsági Access Manager for Microsoft alkalmazások 1.4 egyszeri bejelentkezés és attribútum exchange keretrendszer megadására elterjedt összevonási/WS – adatvédelmi WS identitás standard hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát: 

| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nincs lehetőség|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban |  Támogatott |Nincs lehetőség|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|

Információ IBM Tivoli összevont identitáskezelő további tudnivalókért lásd: [IBM biztonsági Access Manager for Microsoft Applications.](http://www-01.ibm.com/support/docview.wss?uid=swg24029517)

## <a name="secureauth-idp-720"></a>SecureAuth IdP 7.2.0 
SecureAuth IdP 7.2.0 egyszeri bejelentkezéses megoldást és attribútum exchange keretrendszer megadására elterjedt összevonási/WS – adatvédelmi WS identitás standard hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát: 

| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nincs lehetőség|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban |  Támogatott |Nincs lehetőség|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|

SecureAuth kapcsolatos további tudnivalókért olvassa el a [SecureAuth IdP](http://go.microsoft.com/?linkid=9845293)című témakört.

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>Hitelesítésszolgáltató SiteMinder 12.52 SP1 göngyölt kiadásában 4
Hitelesítésszolgáltató SiteMinder összevonási 12.52 egyszeri bejelentkezés és attribútum exchange keretrendszer megadására elterjedt összevonási/WS – adatvédelmi WS identitás standard hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát: 

| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nincs lehetőség|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban |  Támogatott |Nincs lehetőség|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|

Hitelesítésszolgáltató SiteMinder kapcsolatos további tudnivalókért lásd: [hitelesítésszolgáltató SiteMinder összevonási.](http://www.ca.com/us/products/ca-single-sign-on.html) 

## <a name="radiantone-cfs-30"></a>RadiantOne CFS 3.0 
RadiantOne felhő összevonási szolgáltatás (CFS) 3.0 egyszeri bejelentkezés és attribútum exchange keretrendszer megadására elterjedt összevonási/WS – adatvédelmi WS identitás standard hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát: 

| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nincs lehetőség|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban |  Támogatott |Integrált Windows-hitelesítés|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|

RadiantOne CFS kapcsolatos további tudnivalókért lásd [RadiantOne CFS.](http://www.radiantlogic.com/products/radiantone-cfs/)


## <a name="okta"></a>Okta 
Okta egyszeri bejelentkezés és attribútum exchange keretrendszer megadására elterjedt összevonási/WS – adatvédelmi WS identitás standard hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát: 


| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Integrált Windows-hitelesítés szükséges további webkiszolgáló és Okta alkalmazás beállítása.|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban |  Támogatott |Integrált Windows-hitelesítés|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|

Okta kapcsolatos további tudnivalókért lásd: [Okta.](https://www.okta.com/)
 
## <a name="onelogin"></a>OneLogin 
OneLogin szerint vizsgálni május 2014-es egyszeri bejelentkezés és attribútum exchange keretrendszer megadására elterjedt összevonási/WS – adatvédelmi WS identitás standard hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát: 

| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Integrált Windows-hitelesítés|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban |  Támogatott |Integrált Windows-hitelesítés|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|

OneLogin kapcsolatos további tudnivalókért lásd: [OneLogin.](https://www.onelogin.com/)

## <a name="netiq-access-manager-401"></a>NetIQ Access Manager 4.0.1 
NetIQ Access Manager 4.0.1 egyszeri bejelentkezés és attribútum exchange keretrendszer megadására elterjedt összevonási/WS – adatvédelmi WS identitás standard hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát:

| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |* Kerberos szerződéseket támogatott|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban |  Támogatott |Nem támogatott integrált Windows-hitelesítés|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|

* NetIQ támogatási Kerberos-hitelesítés konfigurálása Kerberos szerződést keresztül.  Ebben a konfigurációban segítségért forduljon NetIQ vagy megtekintheti a beállítási útmutatója. NetIQ kezelője kapcsolatos további tudnivalókért lásd: [NetIQ kezelője.](https://www.netiq.com/documentation/netiqaccessmanager4/identityserverhelp/data/b12iqp0m.html)

## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>NAGY IP-az Access házirendkezelő nagy IP-ver. x – 11.6 x 11.3 
A nagy IP-a hozzáférési házirend-kezelőben (APM) nagy IP-ver. 11.3 x – 11.6 x egyszeri bejelentkezéses megoldást és attribútum exchange keretrendszer megadására elterjedt SAML identitás standard hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát: 

| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nincs lehetőség|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban |  Nem támogatott |Nem támogatott|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|

Az IP-nagy Access házirendkezelő kapcsolatos további tudnivalókért lásd: [nagy IP-hozzáférési házirend-kezelő.](https://f5.com/products/modules/access-policy-manager) 

E beállítás a nagy IP-hozzáférési házirend-kezelő útmutatást STS ahhoz, hogy az egyszeri bejelentkezéses megoldást az Active Directory-felhasználók, töltse le a PDF-fájl [itt.](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf)

## <a name="vmware--workspace-portal-version-21"></a>VMware munkaterület portál 2.1-es verziójában 
VMware munkaterület portál 2.1-es verziójában az egyszeri bejelentkezés és attribútum exchange keretrendszer megadására elterjedt összevonási/WS – adatvédelmi WS identitás standard hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát:

| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nem támogatott integrált Windows-hitelesítés|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban | Támogatott |Nem támogatott integrált Windows-hitelesítés|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|

További információt a VMware munkaterület portál 2.1-es verziójával töltse le a PDF-fájl [itt.](http://pubs.vmware.com/workspace-portal-21/topic/com.vmware.ICbase/PDF/workspace-portal-21-resource.pdf)

## <a name="signgo-53"></a>Jelentkezzen be, és válassza a 5.3. 
Jelentkezzen be, és válassza a 5.3 eszközök elterjedt összevonási/WS – adatvédelmi WS azonosítója szabványos egyszeri bejelentkezés és attribútum exchange keretrendszer megadására.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát:

| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Kerberos szerződéseket támogatott |
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban | Támogatott |Nincs lehetőség|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|


Bejelentkezési és Ugrás 5.3 támogatja a Kerberos-hitelesítés konfigurálása Kerberos szerződést keresztül.  Ebben a konfigurációban segítségért forduljon Ilex vagy megtekintheti a beállítási útmutatója [itt.](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)


## <a name="icewall-federation-version-30"></a>IceWall összevonási 3.0-s verzió 
IceWall összevonási 3.0-s verziója a elterjedt összevonási/WS – adatvédelmi WS identitás standard egyszeri bejelentkezés és attribútum exchange keretrendszer megadására hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát:

| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nem támogatott integrált Windows-hitelesítés|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban | Támogatott |Nem támogatott integrált Windows-hitelesítés|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|

IceWall összevonási további információkért lásd [az alábbi](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) és [itt.](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html)

## <a name="ca-secure-cloud"></a>Hitelesítésszolgáltató biztonságos felhő 

Hitelesítésszolgáltató biztonságos felhő egyszeri bejelentkezés és attribútum exchange keretrendszer megadására elterjedt összevonási/WS – adatvédelmi WS identitás standard hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát:

| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nem támogatott integrált Windows-hitelesítés|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban | Támogatott |Nem támogatott integrált Windows-hitelesítés|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|

Hitelesítésszolgáltató biztonságos felhő kapcsolatos további tudnivalókért lásd [hitelesítésszolgáltató biztonságos felhő.](http://www.ca.com/us/products/security-as-a-service.aspx)

## <a name="dell-one-identity-cloud-access-manager-v71"></a>Dell egy identitáskezelő felhő Access v7.1 
Dell egy felhőalapú Access identitáskezelő egyszeri bejelentkezés és attribútum exchange keretrendszer megadására elterjedt összevonási/WS – adatvédelmi WS identitás standard hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát:

| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nincs lehetőség|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban |  Támogatott |Nincs lehetőség|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|

Információ Dell egy felhőalapú Access identitáskezelő további tudnivalókért lásd: [Dell egy felhőalapú Access identitáskezelő.](http://software.dell.com/products/cloud-access-manager)

 A hogyan kell beállítani a STS ahhoz, hogy az egyszeri bejelentkezéses megoldást az Office 365-felhasználóknak című cikkben olvashat [konfigurálása az Office 365-felhasználóknak.](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365) 

## <a name="authanvil-single-sign-on-45"></a>4.5 AuthAnvil egyetlen bejelentkezés 
AuthAnvil egyszeri bejelentkezési a 4.5 egyszeri bejelentkezés és attribútum exchange keretrendszer megadására elterjedt összevonási/WS – adatvédelmi WS identitás standard hajtja végre.

Az alábbiakban a egyszeri bejelentkezéses megoldást forgatókönyvet támogatja mátrixát:

| Ügyfél |Támogatás  |A kivételek|
| --------- | --------- |--------- |
| Webes ügyfelek, mint például az Exchange Web Access és a SharePoint online-ban | Támogatott |Nem támogatott integrált Windows-hitelesítés|
| A Lync, az Office-előfizetés, CRM például Rich-ügyfélalkalmazásokban | Támogatott |Nem támogatott integrált Windows-hitelesítés|
| E-mailek gazdag ügyfelek, mint például az Outlook és az ActiveSync |  Támogatott |Nincs lehetőség|


További tudnivalókért lásd: [AuthAnvil egyszeri bejelentkezést.](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-)
