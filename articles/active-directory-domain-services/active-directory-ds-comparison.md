<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Összehasonlítása Azure Active Directory tartományi szolgáltatások való DIY tartomány vezérlők |} Microsoft Azure"
    description="Azure Active Directory tartományi szolgáltatások hasonlítsa DIY tartomány vezérlők"
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
    ms.date="10/01/2016"
    ms.author="maheshu"/>

# <a name="how-to-decide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>Döntse el, ha az Azure Active Directory tartományi szolgáltatások hogyan mindig a használati eset
Azure Active Directory tartományi szolgáltatások lehetővé teszi az Azure infrastruktúrájának szolgáltatásai, a feladatok üzembe anélkül, hogy az identitás infrastruktúra fenntartása aggódnia. A felügyelt szolgáltatás különbözik egy tipikus Windows Server Active Directory-telepítési önálló felügyelete és terjesztése. A szolgáltatás készült könnyű-a-deployment, automatikus állapot ellenőrzése és remediation és egy egyszerű identitás infrastruktúra a felhőben. Azt is folyamatosan bővül a szolgáltatás telepítési tipikus esetei támogatásának hozzáadására.

Döntse el, hogy Azure Active Directory tartományi szolgáltatások és léptetőnyíl vezérlőelem be és kezelheti a saját (önálló) Azure Active Directory-infrastruktúrát:

- [Azure Active Directory tartományi szolgáltatások által kínált szolgáltatások](active-directory-ds-features.md)listájának megtekintéséhez.

- Tekintse át a gyakori [telepítési forgatókönyvek az Azure Active Directory tartományi szolgáltatások](active-directory-ds-scenarios.md).

- Végül, [Hasonlítsa össze az Azure Active Directory tartományi szolgáltatások egy önálló AD lehetőséget](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).


## <a name="compare-azure-ad-domain-services-to-diy-ad-domain-in-azure"></a>Hasonlítsa össze az Azure Active Directory tartományi szolgáltatások DIY Active Directory tartományi Azure-ban
Az alábbi táblázat segítségével eldöntheti, hogy Azure Active Directory tartományi szolgáltatások használata és kezelése a saját Azure Active Directory-infrastruktúrát között.

|**A szolgáltatás**|**Azure Active Directory tartományi szolgáltatások**|**Önálló"AD az Azure VMs**|
|---|:---:|:---:|
|[**Felügyelt szolgáltatás**](active-directory-ds-comparison.md#managed-service)|**& #x 2713;**|**& #x 2715;**|
|[**Biztonságos telepítések**](active-directory-ds-comparison.md#secure-deployments)|**& #x 2713;**|A rendszergazdának biztonságossá tétele a telepítést.|
|[**DNS-kiszolgáló**](active-directory-ds-comparison.md#dns-server)|**& #x 2713;** (felügyelt szolgáltatás)|**& #x 2713;**|
|[**Tartomány vagy vállalati rendszergazdai jogosultságokkal**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges)|**& #x 2715;**|**& #x 2713;**|
|[**Tartomány illesztés**](active-directory-ds-comparison.md#domain-join)|**& #x 2713;**|**& #x 2713;**|
|[**Tartomány hitelesítése NTLM és Kerberos**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos)|**& #x 2713;**|**& #x 2713;**|
|[**Egyéni szervezeti struktúra**](active-directory-ds-comparison.md#custom-ou-structure)|**& #x 2713;**|**& #x 2713;**|
|[**Séma bővítmények**](active-directory-ds-comparison.md#schema-extensions)|**& #x 2715;**|**& #x 2713;**|
|[**Active Directory tartományerdő bizalmi kapcsolatok**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts)|**& #x 2715;**|**& #x 2713;**|
|[**LDAP olvasása**](active-directory-ds-comparison.md#ldap-read)|**& #x 2713;**|**& #x 2713;**|
|[**Biztonságos LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap)|**& #x 2713;**|**& #x 2713;**|
|[**LDAP írási**](active-directory-ds-comparison.md#ldap-write)|**& #x 2715;**|**& #x 2713;**|
|[**A csoportházirend**](active-directory-ds-comparison.md#group-policy)|Egyszerű|Teljes|
|[**GEO elosztott telepítések**](active-directory-ds-comparison.md#geo-dispersed-deployments)|**& #x 2715;**|**& #x 2713;**|


#### <a name="managed-service"></a>Felügyelt szolgáltatás
Microsoft Azure Active Directory tartományi szolgáltatások a tartományok kezelése. Nem kell aggódnia amiatt, hogy javítani, frissítések, figyelése, biztonsági mentést, és a tartománynevet elérhetőség biztosítása. A felügyeleti feladatok szolgáltatás által felajánlott Microsoft Azure felügyelt tartományai.

#### <a name="secure-deployments"></a>Biztonságos telepítések
A felügyelt tartomány biztonságosan rögzítve van a Microsoft biztonsággal kapcsolatos gyakorlati tanácsok az Active Directory-telepítések szerint. Ezek a gyakorlati tanácsok az Active Directory termékfejlesztő csapatának évtizedekben, amikor mérnöki és az Active Directory-telepítések támogató erednek. Önálló telepítésekhez kell lépésekkel speciális telepítési lefelé/biztonságos zárolása a üzembe.

#### <a name="dns-server"></a>DNS-kiszolgáló
Az Azure Active Directory tartományi szolgáltatások felügyelt tartomány kezelt DNS-szolgáltatást tartalmaz. A "AAD Adatközpont" rendszergazdacsoportjának tagjai kezelhetik a felügyelt tartomány a DNS. Ennek a csoportnak a tagjai kapnak a teljes DNS felügyelete jogosultságokkal felügyelt tartományhoz tartozó. DNS-kezelési végezhető el a távoli kiszolgáló felügyeleti eszközei (RSAT) csomagban a "DNS felügyeleti konzolban".

#### <a name="domain-or-enterprise-administrator-privileges"></a>Tartomány vagy vállalati rendszergazdai jogosultságok
Ezek magasabb szintű jogosultságokkal nem ajánlja fel egy AAD-DS felügyelt tartományban. Ezek magasabb szintű jogosultságokkal kell telepítése és futtatása igénylő alkalmazások szemben kezelt tartományok nem futtatható. Egy kisebb részét rendszergazdai jogosultságokkal a delegált felügyeleti csoport neve "AAD Adatközpont rendszergazdák" tagjai számára érhető el. Ezek a jogosultságok olyan jogosultságokkal konfigurálja a DNS-, állítsa be a csoportházirend, nyereség rendszergazdai jogosultságokkal gépeken tartományhoz stb.

#### <a name="domain-join"></a>Tartomány illesztés
A felügyelt hasonlít hogyan számítógépek csatlakozzon az Active Directory tartományi tartományhoz virtuális gépeken futó bekapcsolódhat.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>Tartomány hitelesítése NTLM és Kerberos
Az Azure Active Directory tartományi szolgáltatások használhatja a vállalati hitelesítő adatait a felügyelt tartománnyal hitelesítést végezni. Hitelesítő adatok megmaradnak a Azure AD-bérlő szinkronban. Szinkronizált bérlők Azure AD Connect biztosítja, hogy a helyszíni hitelesítő adatokkal végzett módosítások vannak szinkronizálva az Azure Active Directory. A DIY tartománybeállítási előfordulhat beállítása a felhasználóknak a vállalati hitelesítő adatokkal hitelesítést végezni a helyszíni fiókerdők tartomány megbízhatósági kapcsolat. Másik megoldás akkor előfordulhat, hogy be kell állítania AD replikációs annak érdekében, hogy a felhasználói jelszavak szinkronizál az Azure tartomány vezérlő virtuális gépeken futó.

#### <a name="custom-ou-structure"></a>Egyéni szervezeti struktúra
A "AAD Adatközpont rendszergazdák" csoport tagjai létrehozhatnak egyéni szervezeti a felügyelt tartományon belül.

#### <a name="schema-extensions"></a>Séma bővítmények
Az alap séma az Azure Active Directory tartományi szolgáltatások felügyelt tartomány nem bővítése. Ezért Active Directory-séma (például új attribútumok csoportban a user objektumban) bővítései támaszkodó alkalmazások szüntetni nem, és AAD-DS tartományok eltolt.

#### <a name="ad-domain-or-forest-trusts"></a>Active Directory tartományi vagy erdőszintűek
Felügyelt tartományok nem állíthatók be más tartományokkal (bejövő/kimenő) meghatalmazotti beállítása. Ezért például az esetben, ha nem szeretne szinkronizálni az Azure Active Directory jelszavakat vagy az erőforrás erdő telepítéseknél esetek Azure Active Directory tartományi szolgáltatások nem használható.

#### <a name="ldap-read"></a>LDAP olvasása
A felügyelt tartomány LDAP, olvassa el a munkaterhelésekből támogat. Ezért hajtanak végre LDAP olvasási szemben kezelt tartomány alkalmazást telepítheti.

#### <a name="secure-ldap"></a>LDAP biztonságos
A felügyelt tartomány, beleértve az interneten keresztül LDAP biztonságos hozzáférés nyújtása Azure Active Directory tartományi szolgáltatások konfigurálása

#### <a name="ldap-write"></a>LDAP írási
A felügyelt tartomány csak olvasható felhasználói objektumoknál. Alkalmazásokat, amelyek az LDAP írási műveletek attribútumok szemben a user objektumban, ezért nem működnek a felügyelt tartományban. Emellett a felhasználói jelszavak nem módosítható a felügyelt tartományon belül. Egy másik példa lenne módosítás csoporttagság vagy a csoport attribútumok a felügyelt tartományban, amelyek nem engedélyezett. Azonban módosításokat felhasználói attribútumok vagy jelszavakat készült Azure Active Directory (keresztül PowerShell/Azure portal) vagy a helyszíni Active Directory a rendszer szinkronizálja a AAD-felügyelt tartományi.

#### <a name="group-policy"></a>A csoportházirend
Összetett csoport házirend szerkezeteket AAD-tartományi felügyelt nem támogatott. Például, nem hozhat létre és üzembe helyezése a tartomány minden egyéni szervezeti külön csoportházirend-objektumok és szűrés általános védelmi célba juttatása WMI használható. Van egy beépített csoportházirend-objektum minden, testre szabható konfigurálása csoportházirendet alkalmaz, amely a "AADDC számítógépek" és "AADDC felhasználók" tárolók.

#### <a name="geo-dispersed-deployments"></a>Telepítések GEO szétosztja között
Az Azure virtuális egyetlen hálózat az Azure Active Directory tartományi szolgáltatások felügyelt tartományok érhetők el. Ha a tartomány vezérlők szeretné elérhetővé tenni több Azure régióban világszerte megkövetelése esetben beállítása a tartomány vezérlők az Azure IaaS VMs valószínűleg a jobb megoldás.


## <a name="do-it-yourself-diy-ad-deployment-options"></a>Önálló"(DIY) AD telepítési lehetőségek
Telepítési használati-esetek lehetnek, ahol szükséges néhány azon a Windows Server Active Directory-telepítés által kínált szolgáltatások. Ebben az esetben vegye figyelembe az alábbi önálló (DIY) lehetőségek közül:

- **Önálló felhő tartomány:** Beállíthatja a "felhő tartomány" Azure virtuális gépeken futó tartomány vezérlők, konfigurált használatával önálló. Ez az infrastruktúra nem integrálása helyszíni Active Directory-környezetben. Ez a beállítás a 'felhő hitelesítő adatok' más-más szabálykészletet lenne szükség való bejelentkezés/VMs a felhőben.

- **Erőforrás erdő telepítési:** Beállíthatja, hogy be egy tartományt az erőforrás erdő topológiában Azure virtuális gépeken futó konfigurált tartomány vezérlők használatával. Ezután beállíthatja az Active Directory megbízhatósági kapcsolat a helyszíni Active Directory-környezetben. Tartomány-illesztés számítógépek (Azure VMs) is el a erőforrás a felhőben. Felhasználói hitelesítés fölé vagy történik a helyszíni címtárában VPN/készült ExpressRoute kapcsolat.

- **Azure használatának kiterjesztése a helyszíni tartomány:** Csatlakoztathatja az Azure virtuális hálózat VPN/készült ExpressRoute kapcsolattal, a helyszíni hálózaton, hogy az Azure VMs is csatlakozik a helyszíni Active Directory. Másik lehetőség előléptetése replika tartományvezérlőnek a helyszíni tartomány, mint egy virtuális Azure-ban. Ezután beállíthatja azt való replikáció helyszíni címtárában VPN/készült ExpressRoute kapcsolaton keresztül. A telepítési mód hatékony kiterjed a helyszíni tartomány Azure.

> [AZURE.NOTE] Előfordulhat, hogy határozhatja meg, hogy egy DIY beállítást jobban megfelel-e a telepítési használata esetek. Fontolja meg a [Visszajelzés megosztása](active-directory-ds-contact-us.md) segítse a szolgáltatás milyen szolgáltatások segít megérteni a jövőben választotta az Azure Active Directory tartományi szolgáltatások. A visszajelzés segít a szolgáltatást, hogy jobban megfeleljen a igényeinek telepítési és használati-esetek is alapkoncepciójára.

Azt közzétették [telepítése a Windows Server Active Directory a Azure virtuális gépeken futó útmutató](https://msdn.microsoft.com/library/azure/jj156090.aspx) DIY telepítések egyszerűbbé tétele érdekében.


## <a name="related-content"></a>Kapcsolódó tartalom
- [Szolgáltatások – Azure Active Directory tartományi szolgáltatások](active-directory-ds-features.md)

- [Telepítési forgatókönyvek - Azure Active Directory tartományi szolgáltatások](active-directory-ds-scenarios.md)

- [A Windows Server Active Directory Azure virtuális gépeken telepítési útmutatója](https://msdn.microsoft.com/library/azure/jj156090.aspx)
