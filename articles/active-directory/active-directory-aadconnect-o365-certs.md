<properties
    pageTitle="Tanúsítvány-megújítás útmutató az Office 365-ben és az Azure Active Directory-felhasználóknak |} Microsoft Azure"
    description="Ez a cikk ismerteti az Office 365-felhasználók e-maileket, amelyek értesítik tanúsítvány megújításával kapcsolatos problémák megoldásához."
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
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Office 365-ben és az Azure Active Directory összevonási tanúsítványok megújítása

##<a name="overview"></a>– Áttekintés

Azure Active Directory (Azure Active Directory) és az Active Directory összevonási szolgáltatások (AD FS) közötti sikeres összevonáshoz biztonsági tokenek az Azure Active Directory aláírásához AD FS által használt tanúsítványokat kell egyezniük, mi van beállítva az Azure Active Directory. Bármely eltéréseket hibás adatvédelmi vezethet. Azure Active Directory biztosítja, hogy ez az információ akkor szinkronizálja Ha telepíti az Active Directory összevonási szolgáltatások és a webes alkalmazás Proxy (a extranetes hozzáférés).

Ez a cikk tartalmaz további információt a jogkivonat-aláíró tanúsítványok kezelése és szinkronizálására őket az Azure Active Directory, a következő esetekben:

* Nem a webes alkalmazás Proxy üzembe helyezése, és ezért az összevonási metaadatok az nem áll rendelkezésre az extranet.
* Jogkivonat-aláíró tanúsítványok az Active Directory összevonási szolgáltatások alapértelmezett beállítása nem használ.
* Egy külső identitásszolgáltató használata

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>Jogkivonat-aláíró tanúsítványok az Active Directory összevonási szolgáltatások alapértelmezett beállítása

Jogkivonat-aláíró és tanúsítványok visszafejtése jogkivonat általában önaláírt tanúsítványok, illetve jó egy évig tart. Alapértelmezés szerint az AD FS egy automatikus megújítási folyamatot **AutoCertificateRollover**nevű tartalmazza. Ha az AD FS 2.0-s vagy újabb verzióját használja, az Office 365-ben és az Azure Active Directory automatikusan frissíti a tanúsítvány lejárta előtt.

### <a name="renewal-notification-from-the-office-365-portal-or-an-email"></a>Megújítás értesítés az Office 365 portálon vagy e-mailben

>[AZURE.NOTE] Ha az e-mailben vagy a tanúsítvány megújítani az Office portál szólít fel értesítést, olvassa el a [kezelése módosításaira jogkivonat-aláíró tanúsítványok](#managecerts) ellenőrzése, ha meg kell tennie semmit című témakört. Microsoft küldi el, akkor is, ha nem a műveletet nem kötelező tanúsítvány megújításához kapcsolatos értesítések vezethet lehetséges kiadásának tisztában.

Azure Active Directory összevonási metaadatok figyelése és frissítése a jogkivonat-aláíró tanúsítványok, amint azt a metaadatok megpróbálja. a jogkivonat-aláíró tanúsítványok, a lejárat előtt 30 nap Azure Active Directory ellenőrzi, ha új tanúsítványok úgy érhetők el az összevonási metaadat-alapú lekérdezési.

* Ha sikeresen lekérdezik az összevonási metaadat-alapú és az új tanúsítványok beolvasásához, nincs értesítő e-mailt vagy figyelmeztetés az Office 365-portálon a felhasználó adja ki.
* Ha nem tudja beolvasni a új jogkivonat-aláíró tanúsítványok, vagy mert nem érhető el az összevonási metaadat-alapú vagy automatikus tanúsítvány átváltási nincs engedélyezve, Azure Active Directory hibák e-mailben értesítést, és a figyelmeztetés az Office 365-portálon.

![Az Office 365 portál értesítés](./media/active-directory-aadconnect-o365-certs/notification.png)

>[AZURE.IMPORTANT] Ha az AD FS, üzemkészségének biztosítása szolgáltatást használ, győződjön meg arról, hogy a kiszolgáló rendelkezik-e az alábbi frissítéseket, így az ismert problémák a hitelesítési hibák fordul elő. Ez megszünteti a ismert AD FS proxy kiszolgáló problémák a megújítás és a későbbi megújítási időszakok:
>
>Server 2012 R2 - [Windows Server május 2014-es összesítő](http://support.microsoft.com/kb/2955164)
>
>Server 2008 R2 és 2012 - [hitelesítés proxyn keresztül nem sikerült a Windows Server 2012-ben vagy Windows 2008 R2 SP1](http://support.microsoft.com/kb/3094446)

## Ha a tanúsítványokat kell-e frissíteni kell ellenőrzése<a name="managecerts"></a>

### <a name="step-1-check-the-autocertificaterollover-state"></a>Lépés: 1: Jelölje be a AutoCertificateRollover állam

Nyissa meg a PowerShell az ADFS-kiszolgálóra. Ellenőrizze, hogy a AutoCertificateRollover értéke igaz.

    Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

[AZURE.NOTE] Active Directory összevonási szolgáltatások 2.0-s verzióját használja, ha először futtassa a Hozzáadás-Pssnapin Microsoft.Adfs.Powershell.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>Lépés: 2: Győződjön meg arról, hogy az AD FS és Azure Active Directory szinkronizáló

Az ADFS-kiszolgálóra nyissa meg az Azure Active Directory PowerShell-parancssorában, és Azure Active Directory csatlakozni.

>[AZURE.NOTE] Letöltheti Azure Active Directory PowerShell [Itt](https://technet.microsoft.com/library/jj151815.aspx).

    Connect-MsolService

Jelölje be a tanúsítványok az Active Directory összevonási szolgáltatások és Azure AD-megbízhatósági tulajdonságokat a megadott tartomány beállítva.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

A mind a kimeneti értékeket a thumbprints megfeleltetéséhez a tanúsítványok Azure Active Directory szinkronizáló is.

### <a name="step-3-check-if-your-certificate-is-about-to-expire"></a>3 lépés: Ellenőrizze, hogy a tanúsítvány lejárta

A kimenet: a Get-MsolFederationProperty vagy a Get-AdfsCertificate jelölje be az időpontra a "Nem után." Ha a dátumot 30 napnál nem vagyok a gépnél, a művelet kell tennie.

| AutoCertificateRollover | Azure Active Directory szinkronizáló tanúsítványok | Összevonás metaadatai nyilvánosan elérhető | Érvényesség | Művelet |
|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|
| igen | igen | igen | - | Nincs teendő. Lásd: [az aláíró tanúsítvány automatikus megújítási jogkivonat](#autorenew). |
| igen | nem  | - | Kisebb, mint a 15 nap | Újítsa meg azonnal. Lásd: [az aláíró tanúsítvány manuális megújítása jogkivonat](#manualrenew). |
| nem | - | - | 30 napnál | Újítsa meg azonnal. Lásd: [az aláíró tanúsítvány manuális megújítása jogkivonat](#manualrenew). |

\[Nem számít -]

## A jogkivonat-aláíró tanúsítvány automatikus megújítás (ajánlott)<a name="autorenew"></a>

Nem kell minden kézi hajtsa végre a következő két teljesülése esetén:
- Engedélyezheti a hozzáférést az összevonási metaadatnak a extranetes a webes alkalmazás Proxy telepítette.
- Használja az Active Directory összevonási szolgáltatások alapértelmezett konfiguráció (AutoCertificateRollover engedélyezett).

Jelölje be az alábbi módon győződjön meg arról, hogy a tanúsítvány automatikusan frissíthető.

**1 az Active Directory összevonási szolgáltatások AutoCertificateRollover csak akkor tulajdonsága igaz.** Ez azt jelzi, hogy az Active Directory összevonási szolgáltatások automatikusan hoz létre új jogkivonat-aláíró és jogkivonat visszafejtés tanúsítványok, a régi előtt szegélyei lejár.

**2. a Active Directory összevonási szolgáltatások összevonási metaadatai nyilvánosan elérhető.** Ellenőrizze, hogy az összevonási metaadatok nyilvánosan hozzáférhető navigálással (elhagyja a vállalati hálózathoz) nyilvános internetkapcsolat a számítógépén, az alábbi URL-címre:


https:// (your_FS_name) /federationmetadata/2007-06/federationmetadata.xml

Ha `(your_FS_name) `az összevonási szolgáltatás állomásnév a szervezete használ, például fs.contoso.com helyére.  Ha Ön is ellenőrizheti ezeket a beállításokat sikeresen, nem rendelkezik a teendő, semmi másra.  

Példa: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Az aláíró tanúsítvány manuális jogkivonat megújítása<a name="manualrenew"></a>

Előfordulhat, hogy szeretné megújítani az aláíró tanúsítványok manuális jogkivonathoz. Például az alábbi esetekben előfordulhat, hogy működne kézi megújításhoz:
* Jogkivonat-aláíró tanúsítványok nem önaláírt tanúsítványokat is. A leggyakoribb ennek oka, hogy a szervezet kezeli-e a szervezeti tanúsítványszolgáltatótól: Ebben az AD FS-tanúsítványok.
* Hálózati biztonság nem teszi lehetővé az összevonási metaadatok nyilvánosan elérhető legyen.

Forgatókönyvekben minden alkalommal, amikor frissíti a jogkivonat-aláíró tanúsítvány is frissítenie kell az Office 365-tartomány használatával frissítés-MsolFederatedDomain PowerShell-parancsot.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>Lépés: 1: Gondoskodjon arról, hogy az AD FS új jogkivonat-aláíró tanúsítványok

**Nem alapértelmezett konfiguráció**

Az Active Directory összevonási szolgáltatások (ahol **AutoCertificateRollover** értéke **hamis**) nem alapértelmezett beállításait használja, valószínűleg használata egyéni tanúsítványok (nem önaláírt). Aláíró tanúsítványok AD FS-jogkivonat megújítása kapcsolatos további tudnivalókért olvassa el [az ügyfelek nem használja az Active Directory összevonási szolgáltatások önaláírt tanúsítványok](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert)című témakört.

**Metaadat-alapú összevonási nem érhető el nyilvánosan**

Kézzel Ha **AutoCertificateRollover** értéke **Igaz**, de nem érhető el nyilvánosan összevonási metaadatainak, először győződjön meg arról, hogy új jogkivonat aláíró tanúsítványok AD FS által lett létrehozva. Erősítse meg, akkor új jogkivonat-aláíró tanúsítványok szakértőjévé válhat az alábbi lépéseket:

1. Győződjön meg arról, hogy be van jelentkezve az elsődleges ADFS-kiszolgálóra.
2. Jelölje be az aktuális aláírási tanúsítványok az Active Directory összevonási szolgáltatások megnyitásával a PowerShell-parancsablakban, és futtassa az alábbi parancsot:

    PS C:\>Get-ADFSCertificate – CertificateType jogkivonat-aláíró

    >[AZURE.NOTE] Active Directory összevonási szolgáltatások 2.0-s verzióját használja, ha először hozzáadása-Pssnapin Microsoft.Adfs.Powershell kell futtatnia.

3. A parancs a felsorolt bármely tanúsítványok megtekintése: Active Directory összevonási szolgáltatások hozott létre egy új tanúsítvány, ha meg kell jelennie a kimenet két tanúsítványok:, amelynek **IsPrimary** értéke **Igaz,** és a **NotAfter** dátum egyik 5 napon belül, és egy, amelynek **IsPrimary** értéke **hamis** , és **NotAfter** körülbelül egy év a jövőben.

4. Ha csak megjelenik egy tanúsítványt, és a **NotAfter** dátum 5 napon belül, meg kell új tanúsítvány létrehozása.

5. Új tanúsítvány szeretne létrehozni, hajtsa végre a következő parancsot a PowerShell parancssorba: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.

6. A frissítés ellenőrizze ismét a következő parancs futtatásával: PS C:\>Get-ADFSCertificate – CertificateType jogkivonat-aláíró

Két tanúsítványok most szerepelnie kell, amelyek közül dátumú **NotAfter** körülbelül egy évvel későbbi a és, amelynek **IsPrimary** értéke **hamis**.

### <a name="step-2-update-the-new-token-signing-certificates-for-the-office-365-trust"></a>Lépés: 2: A új jogkivonat-aláíró számára az Office 365 adatvédelmi tanúsítványok frissítése

Frissítse az Office 365-ben a új jogkivonat-aláíró tanúsítványok használandó a meghatalmazás, az alábbi képlettel történik.

1.  Nyissa meg a Microsoft Azure Active Directory modul Windows Powershellhez készült.
2.  Futtassa a $cred = Get-hitelesítő adatait. Ha ezzel a parancsmaggal, hitelesítő adatokat kér, írja be a felhőalapú szolgáltatás rendszergazdai fiók hitelesítő adatait.
3.  Csatlakozás-MsolService futtatni – a hitelesítő adatok $cred. Ezzel a parancsmaggal a felhőbeli szolgáltatástól csatlakozik. A felhőbeli szolgáltatástól el környezetben létrehozása bármelyik a további parancsmagok telepítve van az eszköz futtatása előtt van szükség.
4.  Ha ezek a parancsok futtat egy olyan számítógépen, amely nem az elsődleges összevonási kiszolgálót, futtassa a Set-MSOLAdfscontext-számítógép <AD FS primary server>, ahol <AD FS primary server> FQDN belső az elsődleges AD FS-kiszolgáló neve. Ezzel a parancsmaggal hoz csatlakozik, az Active Directory összevonási szolgáltatások környezetben.
5.  Futtassa a frissítés-MSOLFederatedDomain – tartománynév <domain>. Ezzel a parancsmaggal frissíti a beállításokat, az Active Directory összevonási szolgáltatások be a felhőalapú szolgáltatásba, és konfigurálja a meghatalmazás a kettő között.


>[AZURE.NOTE] Ha módosítani szeretné, hogy több legfelső szintű tartományok, például: contoso.com és fabrikam.com, támogatja a **SupportMultipleDomain** váltás bármely parancsmagokat kell használnia. További tudnivalókért olvassa el a [több legfelső szintű tartományok támogatása](active-directory-aadconnect-multiple-domains.md)című témakört.

## Azure Active Directory adatvédelmi javítása az Azure AD Connect segítségével<a name="connectrenew"></a>

Ha úgy állította be az AD FS-farm és Azure Active Directory adatvédelmi Azure AD Connect használatával, a Azure AD Connect feltárása, ha akkor kell tennie semmit a jogkivonat-aláíró tanúsítványok is használhatja. Ha a tanúsítványok kell Azure AD Connect ezt is használhatja.

További tudnivalókért olvassa el a [javítása az adatvédelmi](./active-directory-aadconnect-federation-management.md#repairing-the-trust)című témakört.
