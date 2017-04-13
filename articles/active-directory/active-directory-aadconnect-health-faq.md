<properties
    pageTitle="Azure AD Connect állapotjelző – gyakori kérdések"
    description="Ez a cikk az Azure Active Directory csatlakozás állapotjelentések kérdésekre ad választ. Ez a cikk bemutatja a szolgáltatást, beleértve a számlázási modell, Funkciók, korlátozások és támogatási használatával kapcsolatban."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-frequently-asked-questions-faq"></a>Azure AD Connect egészségügyi gyakran ismételt kérdések

Ez a cikk az Azure Active Directory csatlakozás állapotjelentések kérdésekre ad választ. Ez a cikk bemutatja a szolgáltatást, beleértve a számlázási modell, Funkciók, korlátozások és támogatási használatával kapcsolatban.

## <a name="general-questions"></a>Általános kérdések



**Probléma: a több Azure AD-könyvtárak kezelése. Hogyan válthatok az Azure Active Directory prémium egy?**

Választhat másik Azure AD-bérlők kijelölése aláírt jelenleg a felhasználó nevét a jobb felső sarokban, és válassza a megfelelő fiókot. Ha a fiók nem szerepel itt, kattintson a Kijelentkezés parancsra, és a könyvtár, amelynek az Azure Active Directory prémium verzió lehetővé tenni, hogy jelentkezzen be a globális rendszergazdai hitelesítő adataival.

## <a name="installation-questions"></a>Telepítési kérdések



**Kérdés: Mi az, hogy milyen következményekkel járnak az Azure Active Directory csatlakozás állapot ügynök telepíti az egyes kiszolgálók?**

A Microsoft Azure Active Directory csatlakozás egészségügyi ügynökök ADFS, a webes alkalmazás proxykiszolgálót, az Azure AD Connect (sycn) kiszolgálók, tartomány vezérlők telepítése a hatása minimális a Processzor memória felhasználási hálózat sávszélessége és a tárhely részletez.

A számok, az alábbi közelítő.

- Processzor-felhasználás: ~ 1 % -os növelése
- Memóriahasználat: a teljes rendszer memória legfeljebb 10 %-át

>[AZURE.NOTE] Az éppen nem lehet kommunikálni az Azure ügynök, megelőzve a ügynök felfelé meghatározott maximális helyi meghajtóra, adatait tárolja. A agent felülírja a "gyorsítótárazott" adatok "legalább nemrégiben kiszolgált" időközönkénti.

- Azure Active Directory csatlakozás állapotjelző ügynökök helyi pufferelési tárterületet: ~ 20 MB
- Az AD FS-kiszolgálók javasolt, hogy a 1024 MB (1 GB) az AD FS-naplózási csatorna az Azure Active Directory csatlakozás állapot ügynökök beállítása a naplóadatokat feldolgozása, mielőtt a rendszer felülírja azt egy lemezterületet kiépítése.

**K: kell indítsa újra a kiszolgálókat az Azure Active Directory csatlakozás állapot ügynökök telepítése során?**

nem. A telepítés a anyagok nincs szükség a indítsa újra a kiszolgálót. Azonban néhány lépést kapcsolatban előzetesen szükséges telepítése is kell indítani a kiszolgáló.

Például a Windows Server 2008 R2 telepítésének .net keretrendszer 4.5 újra kell indítani kiszolgáló.


**Kérdés: jelent az Azure Active Directory csatlakozás egészségügyi szolgáltatások használata egy átadó HTTP-proxyn keresztül?**

igen.  Az Áttekintés műveletek, állítható be az állapot ügynök kimenő http kérések HTTP-Proxy továbbítani szeretné. További tudnivalókért lásd: [konfigurálása Azure Active Directory csatlakozás állapot ügynökök használni a HTTP-Proxy.](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)
A proxybeállítások ügynök regisztráció során van szüksége, ha előfordulhat előzetesen módosítása az Internet Explorer proxybeállításokat.
1. Nyissa meg az Internet Explorer-beállítások > Internet -> kapcsolatok-beállítások > -> helyi hálózati beállítások.
2. Jelölje ki a proxykiszolgáló használata a helyi hálózaton.
3. Válassza a Speciális lehetőséget, ha telepítve van más proxy portokat HTTP- és HTTPS/biztonságos.

**Kérdés: eladja Azure AD-állapot szolgáltatások Csatlakozás alapszintű hitelesítés támogatására Http-proxy való csatlakozáskor?**

nem. Alapszintű hitelesítés tetszőleges felhasználónév és jelszó megadása a kisalkalmazások jelenleg nem támogatott.


**Kérdés: milyen AD DS verzióját az Active Directory tartományi szolgáltatások Azure Active Directory csatlakozás állapot által támogatott?**

Active Directory tartományi szolgáltatások figyelemmel kísérése támogatott közben az alábbi operációs rendszer verziója telepítve:

- A Windows Server 2008 R2 rendszerben
- A Windows Server 2012
- A Windows Server 2012 R2

## <a name="operations-questions"></a>Műveletek kérdések



**Kérdés: van szükségem az Active Directory összevonási szolgáltatások alkalmazás proxykiszolgálók vagy a webes alkalmazás proxykiszolgálók naplózás engedélyezése?**

Nem, a naplózás nem kell engedélyezni kell a webes alkalmazás Proxy (WAP) kiszolgálókon. Engedélyezze az AD FS-kiszolgálókon.


**Kérdés: hogyan tegye Azure Active Directory csatlakozás állapot riasztások beszerzése a rendszer?**

Azure Active Directory csatlakozás állapot riasztások sikeres feltétel első megoldását. Azure Active Directory csatlakozás állapot ügynökök észleli és a sikeres feltételek a szolgáltatás rendszeres időközönként jelentést. Néhány küldött értesítések esetén visszaszorításáról időalapú. Más szóval a riasztási generációs 72 órán belül nem teljesülnek ugyanazt a hibát, ha a figyelmeztető automatikusan megoldódott-e.




**Kérdés: milyen tűzfalportokat kell nyissa meg az Azure Active Directory csatlakozás állapotjelző ügynök a munkát?**

TCP/UDP-portok 443-as és nyissa meg az Azure Active Directory csatlakozás állapot ügynök az Azure Active Directory állapot szolgáltatás végpontokat kommunikálni tudjanak 5671 van szükség.


**Kérdés: Miért jelennek meg az Azure Active Directory csatlakozás állapot portálon azonos nevű két servers?**

A kiszolgálóról ügynökszoftvert eltávolításakor a kiszolgáló nem törlődik automatikusan portálról az Azure Active Directory csatlakozni.  Ha manuálisan ügynökszoftvert eltávolítani a kiszolgálóról, illetve eltávolítja a kiszolgáló magát, manuálisan törölni a kiszolgáló bejegyzését az Azure Active Directory csatlakozás állapot portálról szeretne. További tudnivalókért kattintson a [törlése egy kiszolgáló vagy szolgáltatás példány.](active-directory-aadconnect-health-operations.md#delete-a-server-or-service-instance)

Kiszolgáló reimaged vagy létrehozott új kiszolgáló (például a számítógép neve) az azonos adataival, és már regisztrált a kiszolgáló nem távolította el az Azure Active Directory csatlakozás egészségügyi portálról, ha telepítve van a agent az új kiszolgálón azonos nevű két bejegyzés jelenhetnek meg.  
Ebben az esetben a bejegyzést, a régebbi kiszolgáló tartozó manuálisan kell törölni. Az adatok kiszolgáló elavult kell lennie.

## <a name="related-links"></a>Kapcsolódó hivatkozások

* [Azure AD Connect állapota](active-directory-aadconnect-health.md)
* [Azure AD Connect egészségügyi ügynök telepítése](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect állapot műveletek](active-directory-aadconnect-health-operations.md)
* [Azure Active Directory használatával kapcsolatba állapot Active Directory összevonási szolgáltatások](active-directory-aadconnect-health-adfs.md)
* [Azure Active Directory csatlakozás állapota szinkronizálás használata](active-directory-aadconnect-health-sync.md)
* [Azure Active Directory segítségével kapcsolatba állapotjelző Active Directory tartományi szolgáltatások](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect állapot korábbi verziók](active-directory-aadconnect-health-version-history.md)
