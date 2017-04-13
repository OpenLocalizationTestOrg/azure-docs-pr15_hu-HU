<properties
   pageTitle="Azure AD Connect: Előfeltételek és hardver |} Microsoft Azure"
   description="Ez a témakör ismerteti a előtti követelményeit és Azure AD Connect hardverkövetelményei"
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="prerequisites-for-azure-ad-connect"></a>Csatlakozás az Azure Active Directory vonatkozó követelmények
Ez a témakör ismerteti a előtti követelményeit és Azure AD Connect hardverkövetelményei.

## <a name="before-you-install-azure-ad-connect"></a>Mielőtt telepítené az Azure AD Connect
Azure AD Connect a telepítés előtt dolgot néhány szükséges.

### <a name="azure-ad"></a>Azure Active Directory
- Az Azure-előfizetéssel és [Azure próba-előfizetés](https://azure.microsoft.com/pricing/free-trial/). Csak az Azure portál megnyitása és nem használja az Azure AD Connect szükséges. A PowerShell vagy az Office 365 használata esetén szükségtelen Azure AD Connect használni egy Azure-előfizetést. Ha van Office 365-licenc is használhatja az Office 365 portálon. A fizetős Office 365-licenc is megnyithatja őket a Azure portálra az Office 365 portálról.
    - Az Azure Active Directory az Előnézet az [Azure portálon](https://portal.azure.com)is használhatja. A portál nincs szükség az Azure licencet.
- [Hozzáadása és a tartomány tulajdonjogának igazolása](active-directory-add-domain.md) az Azure Active Directory használni kívánt. Ha szeretné használni a contoso.com a felhasználók számára, majd ellenőrizze, hogy például ellenőrizte ezt a tartományt, és nem használja, csak contoso.onmicrosoft.com alapértelmezett tartományát.
- Az Azure AD-bérlő lehetővé teszi, hogy 50k alapértelmezett objektumokkal. Ha a tartomány tulajdonjogának igazolása, a korlátját 300 k objektumokhoz. Ha még több objektum van szüksége az Azure AD meg kell nyitnia a korlátozást még tovább nagyobb támogatási esetet. Ha több mint 500 KB objektumok kell majd licencre van szüksége egy, például az Office 365-ben, Azure Active Directory egyszerű, Azure Active Directory prémium vagy vállalati mobilitás csomagja.

### <a name="prepare-your-on-premises-data"></a>A helyszíni adatok előkészítése
- Tekintse át [az Azure Active Directory engedélyezheti választható szinkronizálási szolgáltatások](active-directory-aadconnectsyncservice-features.md) , és mely funkcióit, engedélyeznie kell a értékeli.

### <a name="on-premises-servers-and-environment"></a>A helyszíni kiszolgálók és környezet
- Az Active Directory séma verziója és erdő működési szintjét Windows Server 2003-as vagy újabb kell lennie. A tartomány vezérlők futtatását is lehetővé teszi olyan verziójú mindaddig, amíg a séma és erdők szintű feltételek teljesülése.
- Ha a szolgáltatás **jelszó visszaírást** használni kívánt a tartomány vezérlők (a legújabb SP) a Windows Server 2008 vagy újabb kell lennie. Ha a DCs 2008-on (előtti-R2), majd a [gyorsjavítást KB2386717](http://support.microsoft.com/kb/2386717)is kell telepíteni.
- Azure Active Directory tartományi vezérlőn írható kell lennie. Használni egy írásvédett (csak olvasható tartományvezérlőnek) nem támogatott, és Azure AD Connect nem követik bármely írási átirányítások.
- Azure AD Connect Small Business Server vagy a Windows Server Essentials nem lehet telepíteni. A kiszolgáló a Windows Server standard vagy jobb kell használnia.
- Az Azure AD Connect kiszolgáló rendelkeznie kell a teljes grafikus telepítve. Alapvető server telepítése nem támogatott.
- A Windows Server 2008 vagy újabb telepíteni kell az Azure AD Connect. A kiszolgáló lehet egy tartományvezérlőnek vagy a kiszolgáló tagja expressz beállítások használata. Ha egyéni beállításokat használ, a kiszolgáló akkor is önálló, és nem kell lennie tartományhoz.
- Azure AD Connect a Windows Server 2008 telepítése esetén győződjön meg róla, hogy a Windows Update legújabb alkalmazza. A telepítés még nem tud elindulni javítással kiszolgálóval.
- Ha azt tervezi, hogy a szolgáltatás **Jelszó-szinkronizálás**, az Azure AD Connect kiszolgáló a Windows Server 2008 R2 SP1 vagy újabb kell lennie.
- Az Azure AD Connect kiszolgáló kell rendelkeznie a [.NET-keretrendszer 4.5.1](#component-prerequisites) vagy újabb és [Microsoft PowerShell 3.0-s](#component-prerequisites) vagy újabb verziója van telepítve.
- Ha az Active Directory összevonási szolgáltatások telepíti, az Active Directory összevonási szolgáltatások vagy a webes alkalmazás proxykiszolgáló futtató kiszolgálók kell lennie a Windows Server 2012 R2 vagy újabb verziójában. [A Windows távfelügyelet](#windows-remote-management) alábbi kiszolgálókon távoli telepítéshez engedélyezve kell lennie.
- Ha az Active Directory összevonási szolgáltatások telepíti, [Az SSL-tanúsítványok](#ssl-certificate-requirements)szüksége.
- Ha az Active Directory összevonási szolgáltatások telepíti, majd meg kell [névfeloldás](#name-resolution-for-federation-servers)konfigurálása.
- Azure AD Connect a személyes adatok tárolására SQL Server-adatbázis szükséges. Alapértelmezés szerint egy SQL Server 2012 Express LocalDB (az SQL Server Express egy egyszerűsített verziója) telepítve van, és a szolgáltatásfiók a szolgáltatás a helyi számítógépen hoz létre. Az SQL Server Express méretkorlátja 10 GB-os, amely lehetővé teszi, hogy körülbelül 100 000 objektumok kezelése. Ha szeretne egy újabb mennyiségű címtárobjektumok kezelése, az telepítővarázslóban mutasson az SQL Server egy másik telepítés van szüksége.
- Ha egy külön SQL-kiszolgálót használ, akkor ezekről a követelményekről alkalmazni:
    - Azure AD Connect támogatja a Microsoft SQL Server az SQL Server 2008 (a SP4) az SQL Server 2014-es összes változatok. Microsoft Azure SQL-adatbázis a **nem támogatott** adatbázisként.
    - Nagybetűk SQL rendezési sorrend kell használnia. Ezek a megadott egy \_CI_ a nevüket. **Nem támogatott** a kis-és nagybetűket illesztés, jelölt használatára \_CS_ a nevüket.
    - Csak egy szinkronizálási motor adatbázis példányonként is. **Nem támogatott** FIM/MIM szinkronizálási DirSync és Azure AD-szinkronizálás megosztása az adatbázis-példány.

### <a name="accounts"></a>Fiókok
- Ha integrálni a kívánt az Azure Active Directory Azure Active Directory globális rendszergazdai fiókkal. Ez a **iskolában vagy szervezeti fiókkal** kell lennie, és nem lehet a **Microsoft-fiókjával**.
- Ha sürgős beállításokkal vagy frissíteni az DirSync, majd rendelkeznie kell egy vállalati rendszergazdai fiókkal a helyi Active Directory számára.
- [Az Active Directory fiókok](./connect/active-directory-aadconnect-accounts-permissions.md) Ha használja az egyéni beállítások telepítési elérési útját.

### <a name="azure-ad-connect-server-configuration"></a>Azure AD Connect kiszolgáló konfigurálása
- Ha a globális rendszergazdák MFA engedélyezve van, majd az URL-cím **https://secure.aadcdn.microsoftonline-p.com** kell lennie a megbízható helyek listájához. Ha nem adja hozzá a előtt meg kell adni egy MFA beavatkozás igazolására szolgáló eljárás hozzáadása a megbízható helyek listájához kéri. Az Internet Explorer segítségével hozzáadhatja a megbízható helyekhez.

### <a name="connectivity"></a>Kapcsolat
- Az Azure AD Connect kiszolgáló DNS-feloldási intranetes és az internetről is szüksége van. A DNS-kiszolgáló tudja oldani a neveket, mind a helyszíni Active Directory, valamint az Azure Active Directory végpontokat kell lennie.
- Ha az intraneten tűzfalak van, és meg kell nyitnia az Azure AD Connect kiszolgálók és a tartomány vezérlők között portok, majd [Azure AD Connect portok](active-directory-aadconnect-ports.md) további információt talál.
- Ha a proxykiszolgáló vagy a tűzfal határ URL-címeit is elérhető, majd az URL-címek dokumentált [csak jelszóval módosítható tartományok Office 365 URL-címei és IP-cím](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) meg kell nyitni.
    - Ha a Microsoft Cloud Németország vagy a Microsoft Azure kormányzati felhőalapú szolgáltatást használ, lásd [Azure AD Connect szinkronizálási szolgáltatás példányok kapcsolatos szempontok](active-directory-aadconnect-instances.md) URL-ek.
- Azure AD Connect TLS 1.0 használatával kommunikál az Azure Active Directory alapértelmezés szerint be van. Módosíthatja a TLS 1.2-es verziójára [az Azure AD Connect engedélyezése TLS 1.2](#enable-tls-12-for-azure-ad-connect)-es lépéseket követve.
- Egy kimenő proxyn használatakor az internetcsatlakozást a következő beállítást a **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** fájlban hozzá kell adnia a telepítővarázslóban és Azure AD Connect szinkronizálási engedélyezni szeretné a csatlakozás az internetes és Azure Active Directory. Ez a szöveg meg kell adni a fájlt alján. Ez a kód a &lt;PROXYADRESS&gt; jelöli a tényleges proxy IP-cím vagy állomásnév neve.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

- Ha a proxykiszolgáló hitelesítést igényel, a [szolgáltatásfiók](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) kell elhelyezni, a tartomány, és adjon meg egy [egyéni szolgáltatásfiók](./connect/active-directory-aadconnect-get-started-custom.md#install-required-components)a testre szabott beállítások telepítési elérési utat kell használnia. Különböző módosításának machine.config is szükséges. Ezzel a machine.config az telepítővarázslóban módosítása és a szinkronizálási motor hitelesítés kérelmekre válaszolni a proxykiszolgáló. Az összes telepítővarázslóban használt lapok, amelyen kizárható **a lap az aláírt** a felhasználó hitelesítő adatait. **A lap végén található a telepítés varázslót** a helyi az Ön által létrehozott [szolgáltatásfiók](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) másikra váltani. A machine.config szakasz így néz ki.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

Az MSDN webhelyen talál további információt az [alapértelmezett proxy elemet](https://msdn.microsoft.com/library/kd3cf2ex.aspx).

Kapcsolódási problémák esetén tanulmányozza a [kapcsolódási problémák elhárítása](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Más
- Nem kötelező: Próba felhasználói fiók szinkronizálási megerősítéséhez.

## <a name="component-prerequisites"></a>Összetevő vonatkozó követelmények

### <a name="powershell-and-net-framework"></a>A PowerShell és a .net keretrendszer
Azure AD Connect Microsoft PowerShell és a .NET-keretrendszer 4.5.1 függ. Ez a verzió vagy újabb verziója telepítve a kiszolgálóra van szükség. A Windows Server verziójától függően tegye a következőket:

- A Windows Server 2012R2
  - Alapértelmezés szerint telepítve van a Microsoft PowerShell, nincs további teendő szükség.
  - .NET-keretrendszer 4.5.1 és a későbbi kiadásokban ajánlja fel a Windows Update szolgáltatáson keresztül. Ellenőrizze, hogy a Vezérlőpultot a Windows Server telepítette a legújabb frissítéseket.
- A Windows Server 2008R2 és a Windows Server 2012
  - A legújabb Microsoft PowerShell érhető el a **Windows Management Framework 4.0**, a [Microsoft letöltőközpontból](http://www.microsoft.com/downloads).
  - .NET-keretrendszer 4.5.1 és a későbbi kiadásokban érhetők el a [Microsoft letöltőközpontból](http://www.microsoft.com/downloads).
- A Windows Server 2008
  - A PowerShell legújabb támogatott verzióját a **Windows Management Framework 3.0**programban elérhető a [Microsoft letöltőközpontban](http://www.microsoft.com/downloads)érhető el.
 - .NET-keretrendszer 4.5.1 és a későbbi kiadásokban érhetők el a [Microsoft letöltőközpontból](http://www.microsoft.com/downloads).

### <a name="enable-tls-12-for-azure-ad-connect"></a>Engedélyezi a TLS 1.2-es Azure AD Connect
Azure AD Connect használ TLS 1.0 alapértelmezés szerint a szinkronizálási motor kiszolgáló és Azure Active Directory közötti kommunikáció titkosításához. .Net-alkalmazások használata a TLS 1.2-es alapértelmezés szerint a kiszolgáló a konfigurálásával módosíthatja. További információt a TLS 1.2-es [Microsoft biztonsági tanácsadó 2960358](https://technet.microsoft.com/security/advisory/2960358)találhatók.

1. A Windows Server 2008 nem lehet engedélyezni a TLS 1.2-es. A Windows Server 2008R2 van szüksége, vagy később. Győződjön meg arról, hogy a .net 4.5.1 javítás az operációs rendszerének telepítve van a [Microsoft biztonsági tanácsadó 2960358](https://technet.microsoft.com/security/advisory/2960358)című. Lehet, hogy ez vagy annál újabb már telepítve a kiszolgálóra.
2. Ha használja a Windows Server 2008R2, végezze el, hogy engedélyezve van a TLS 1.2-es. A Windows Server 2012-kiszolgáló és újabb verzióiban TLS 1.2-es már engedélyezni kell.
```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
```
3. Operációs rendszerek az alábbi beállításkulcs beállítása, és indítsa újra a kiszolgálót.
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
"SchUseStrongCrypto"=dword:00000001
```
4. Ha is engedélyezni szeretné TLS 1.2-es a szinkronizálási motor kiszolgáló és a távoli SQL Server között, majd ellenőrizze, hogy telepítve van az [1.2-es TLS támogatása a Microsoft SQL Server](https://support.microsoft.com/kb/3135244)szükséges verziójával rendelkezik.

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Az összevonási telepítési és konfigurálási vonatkozó követelmények

### <a name="windows-remote-management"></a>A Windows távfelügyelet
Amikor Azure AD Connect üzembe az Active Directory összevonási szolgáltatásokat vagy a webes szolgáltatásalkalmazás-Proxy, jelölje be az alábbi követelmények való csatlakozást, és konfigurációs fog sikerülni:

- Ha a tároló kiszolgáló tartományhoz csatlakoztatott, akkor győződjön meg arról, hogy engedélyezve van-e a Windows távoli felügyelt
    - Egy rendszergazda jogú PSH parancsablakban parancs használata`Enable-PSRemoting –force`
- Ha a cél kiszolgáló egy tartományon kívüli illesztett WAP számítógépre, néhány további követelmények
    - A cél számítógépen (WAP gépen):
         - A winrm biztosítása (Windows távfelügyelet / szolgáltatás) szolgáltatás fut a Services beépülő modul használatával
         - Egy rendszergazda jogú PSH parancsablakban parancs használata`Enable-PSRemoting –force`
    - A számítógépen, amelyen a varázsló fut (Ha a cél gép tartományon kívüli illesztett vagy a nem megbízható tartomány):
        - Egy rendszergazda jogú PSH parancsablakban paranccsal`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
        - A Kiszolgálókezelő:
             - gépi készlet DMZ WAP állomás hozzáadása (Kiszolgálókezelő-kezelés >...-kiszolgálók hozzáadása > DNS lapon)
             - A Kiszolgálókezelő minden kiszolgálók lapján: kattintson a jobb gombbal az WAP server, és válassza a Manage másként elemre, adja meg a helyi (nem tartomány) creds a WAP gépen
             - Távoli PSH való csatlakozással, a Kiszolgálókezelő minden kiszolgálók lapján ellenőrizze, hogy: kattintson a jobb gombbal az WAP server, és válassza a Windows PowerShell.  Annak biztosítása érdekében a távoli PowerShell-munkamenet hozható létre PSH távoli munkamenetet nyílik meg.

### <a name="ssl-certificate-requirements"></a>SSL-tanúsítvány követelmények
**Fontos:** erősen ajánlott használja az SSL-tanúsítvány át az AD FS-farm csomópontjait, valamint az összes webalkalmazás proxykiszolgálóiban.

- A tanúsítvány kell lennie egy X509 tanúsítvány.
- Önaláírt tanúsítvány az összevonási kiszolgálók laboratóriumi vizsgálat környezetben is használhatja. Azonban az munkakörnyezetben, azt javasoljuk, hogy egy nyilvános hitelesítésszolgáltatótól szerezze be a tanúsítvány.
    - Ha egy tanúsítványt, amely nem nyilvánosan megbízható használ, győződjön meg arról, hogy a tanúsítvány minden webes alkalmazás proxykiszolgáló telepítve a helyi kiszolgálón, valamint az összevonási kiszolgálók a megbízható
- A tanúsítvány azonosítóját egyeznie kell az összevonási szolgáltatás neve (például sts.contoso.com).
    - Az identitás vagy a tárgy alternatív (SAN)-kiterjesztés típusú Dns_név, illetve nincsenek SAN bejegyzések, ha a tulajdonos nevét a megadott közös nevével.  
    - Több SAN bejegyzés szerepel a tanúsítvány, lehet, amennyiben ezek egyikét megegyezik az összevonási szolgáltatás neve.
    - Munkahelye Bekapcsolódás használni szeretné, ha szükséges, az az érték-e egy további SAN **enterpriseregistration.** követi, egyszerű felhasználónév (UPN) a szervezet, például **enterpriseregistration.contoso.com**.
- Tanúsítványok alapján CryptoAPI következő generációs (CNG) kulcsok és a fő tárterület-szolgáltatók használatának támogatása megszűnik. Ez azt jelenti, hogy egy tanúsítványt egy CSP (cryptographic szolgáltató) és a nem KSP (kulcsot tároló szolgáltató) alapján kell használnia.
- Helyettesítő karakter tanúsítványok támogatottak.

### <a name="name-resolution-for-federation-servers"></a>Az összevonási kiszolgálók névfeloldás
- Állítsa be a DNS-rekordjait az Active Directory összevonási szolgáltatások összevonási szolgáltatás neve (például sts.contoso.com) az intraneten (a belső DNS-kiszolgáló) és a extranetes (nyilvános DNS keresztül a tartományregisztrálója). Az intranet DNS record győződjön meg arról, hogy használja-e A rekordokat, és nem a CNAME rekordot. Az a tartomány illesztett számítógépről megfelelően működjön a windows-hitelesítés szükséges.
- Ha egynél több AD FS-kiszolgáló vagy a webes alkalmazás proxykiszolgáló telepítését, majd győződjön meg arról, hogy van-e beállítva a terheléselosztó és, hogy a DNS-rekordokat, az Active Directory összevonási szolgáltatások összevonási szolgáltatás nevét (például sts.contoso.com) mutasson a terheléselosztó.
- A windows-hitelesítés intranet használ az Internet Explorer böngésző alkalmazások személyre győződjön meg arról, hogy az Internet Explorer intranet zóna bekerül az Active Directory összevonási szolgáltatások összevonási szolgáltatás neve (például sts.contoso.com). Ez csoportházirenden keresztül ellenőrzött és tartományhoz csatlakoztatott összes számítógépre telepítve.

## <a name="azure-ad-connect-supporting-components"></a>Azure AD Connect támogatása összetevők
Az alábbiakban megtalálja a kiszolgálón, amelyen telepítve van-e az Azure AD Connect telepített Azure AD Connect összetevők listáját. Ez a lista nem egy egyszerű Express telepítésével.  Ha úgy dönt, hogy egy másik SQL Server használata a telepítés szinkronizálási szolgáltatások lapon, majd az SQL Express LocalDB nincs telepítve helyi meghajtóra.

- Azure AD Connect állapota
- A Microsoft Online Services – bejelentkezési segéd (de nem függés telepítve) informatikai szakembereknek
- A Microsoft SQL Server 2012 parancssori segédeszközök
- A Microsoft SQL Server 2012 Express LocalDB
- A Microsoft SQL Server 2012 natív ügyfele
- Microsoft Visual C++ 2013 terjesztés céljából csomag

## <a name="hardware-requirements-for-azure-ad-connect"></a>Azure AD Connect hardverkövetelményei
Az alábbi táblázatban látható az Azure AD Connect szinkronizálása számítógép minimális követelmények.

| Az Active Directory objektumok száma | PROCESSZOR | A memória | Merevlemez-meghajtó mérete |
| ------------------------------------- | --- | ------ | --------------- |
| Legfeljebb 10 000 | 1,6 GHz-es | 4 GB | 70 GB |
| 10 000 – 50 000 | 1,6 GHz-es | 4 GB | 70 GB |
| 50 000 – 100 000 | 1,6 GHz-es | A 16 GB | 100 GB |
| Az SQL Server teljes verziója 100 000 vagy több objektum szükség|  |  |  |
| 100 000 – 300,000 | 1,6 GHz-es | 32 GB | 300 GB |
| 300,000 – 600,000 | 1,6 GHz-es | 32 GB | 450 GB |
| Több mint 600,000 | 1,6 GHz-es | 32 GB | 500 GB |

Active Directory összevonási szolgáltatások vagy webkiszolgálón alkalmazást futtató számítógépeken minimális követelmények a következőket:

- Processzor: Kettős alapvető 1,6 GHz-es vagy újabb
- MEMÓRIA: 2 GB-os vagy újabb
- Azure virtuális: A2 konfigurációs vagy újabb

## <a name="next-steps"></a>Következő lépések
További tudnivalók [a helyszíni identitások Azure Active Directoryval való integrálásának](active-directory-aadconnect.md).
