<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások biztonságos LDAP (LDAPS) beállítása |} Microsoft Azure"
    description="Azure Active Directory tartományi szolgáltatások felügyelt tartomány esetén állítsa be a biztonságos LDAP (LDAPS)"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Az Azure Active Directory tartományi szolgáltatások felügyelt tartományhoz biztonságos LDAP (LDAPS) beállítása
Ebből a cikkből megtudhatja, hogyan engedélyezheti biztonságos Lightweight Directory Access Protocol (LDAPS) Azure Active Directory tartományi szolgáltatások felügyelt tartománya. Biztonságos LDAP is nevezik "Lightweight Directory Access Protocol (LDAP) Secure Sockets Layer (SSL) rétegen felett / átviteli réteg Security (TLS)'.

## <a name="before-you-begin"></a>Első lépések
A jelen cikkben felsorolt feladatok elvégzéséhez szükséges:

1. **Azure előfizetés**érvényes.

2. **Azure Active directory** - vagy szinkronizálva egy helyszíni directory vagy a felhőben könyvtárában.

3. **Azure Active Directory tartományi szolgáltatások** az Azure Active Directory engedélyezve kell lennie. Ha még nem tette, kövesse az [első lépések útmutató](./active-directory-ds-getting-started.md)című témakörben ismertetett feladatokat.

4. **Ahhoz, hogy a biztonságos LDAP használt tanúsítvány**.
    - **Ajánlott** - tanúsítvány beszerzése a hitelesítésszolgáltató vagy a nyilvános hitelesítésszolgáltató. Ez a beállítás használata biztonságosabb.
    - Másik megoldás dönthet úgy is [önaláírt tanúsítvány](#task-1---obtain-a-certificate-for-secure-ldap) létrehozása a jelen cikk látható módon.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>A biztonságos LDAP tanúsítvány vonatkozó követelmények
Szerezheti be az alábbi útmutatásokat egy érvényes tanúsítványt, biztonságos LDAP engedélyezése előtt. Ha engedélyezi a biztonságos LDAP-érvénytelen helytelen tanúsítvány felügyelt tartománya próbál hibákat tapasztal.

1. **Megbízható kibocsátó** – a kell lennie tanúsítványt egy azokon a számítógépeken, még nem csatlakozott a tartomány használata a biztonságos LDAP megbízható hitelesítésszolgáltató. Ez a szervezet a szervezet vállalati hitelesítésszolgáltató vagy ezen a számítógépen a megbízható hitelesítésszolgáltató nyilvános lehetnek.

2. **Leírási idő** – a tanúsítvány legalább a következő 3 – 6 hónapra érvényes kell. A felügyelt tartomány LDAP biztonságos hozzáférés megszakadt a tanúsítvány lejártakor.

3. **Tulajdonosneve** – a tárgy a tanúsítvány neve a felügyelt tartomány helyettesítő kell lennie. Például ha a tartomány neve "contoso100.com", a tanúsítvány tulajdonosneve kell lennie "*. contoso100.com". Állítsa a helyettesítő nevét a DNS-név (tárgy másodlagos név).

3. **Billentyű használatát** - célokra a következő - digitális aláírások és kulcstitkosítás a tanúsítvány kell beállítania.

4. **Tanúsítvány célja** – a tanúsítványt kell érvényes, a kiszolgáló SSL-hitelesítést.

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Tevékenység-1 - biztonságos LDAP tanúsítvány beszerzése
Az első tevékenység magában foglalja a felügyelt tartományhoz biztonságos LDAP hozzáféréshez használt tanúsítvány beszerzése. Két lehetőség közül választhat:

- Tanúsítvány beszerzése hitelesítésszolgáltatótól. Lehet, hogy a szervezet a szervezet vállalati Hitelesítő vagy egy nyilvános hitelesítésszolgáltató.

- Önaláírt tanúsítvány létrehozása.


### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Option (ajánlott) - biztonságos LDAP tanúsítvány beszerzése hitelesítésszolgáltatótól
Ha a szervezet egy vállalati nyilvános kulcs infrastruktúrájának üzembe helyezése, meg kell tanúsítvány beszerzése hitelesítésszolgáltatótól a vállalati (CA) a szervezet számára. Ha a szervezet nyilvános hitelesítésszolgáltatótól beolvassa a saját tanúsítványok, meg kell szerezze be a biztonságos LDAP-tanúsítvány, hogy nyilvános hitelesítésszolgáltatótól.

Tanúsítvány kérésekor győződjön meg arról, hogy hajtsa végre a követelményeket, [a biztonságos LDAP tanúsítvány követelményt](#requirements-for-the-secure-ldap-certificate)tagolt.

> [AZURE.NOTE] Még nem csatlakozott a felügyelt tartományhoz biztonságos LDAP ügyfélszámítógépek meg kell bíznia a kibocsátó LDAPS tanúsítvány.


### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>B beállítás – biztonságos LDAP önaláírt tanúsítvány létrehozása
Előfordulhat, hogy választhatja önaláírt tanúsítvány létrehozása a biztonságos LDAP, ha:

- a szervezet tanúsítványok nem vállalati hitelesítésszolgáltató által kibocsátott vagy
- nem a várt nyilvános hitelesítésszolgáltatótól tanúsítvány használatára.

**A PowerShell használatával önaláírt tanúsítvány létrehozása**

Windows rendszerű számítógépén **rendszergazdaként** PowerShell új ablak megnyitása, és írja be a következő parancsokat, új önaláírt tanúsítvány létrehozása.

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

A fenti minta cserélje ki az Azure Active Directory tartományi szolgáltatások felügyelt tartomány a DNS-tartománynév "contoso100.com".

![Jelölje ki az Azure Active Directory](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Az újonnan létrehozott önaláírt tanúsítványt küldi a helyi számítógép zónában tanúsítvány áruházból.


## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a>2 – exportálás a biztonságos LDAP tevékenység tanúsítványt egy. PFX-fájl
Ez a tevékenység megkezdése előtt győződjön meg arról, hogy a biztonságos LDAP tanúsítvány beszerzett a vállalati hitelesítésszolgáltató vagy egy nyilvános hitelesítésszolgáltató, vagy nem hozott létre önaláírt tanúsítvány.

A következő lépésekkel, a LDAPS tanúsítvány exportálása egy. PFX fájlt.

1. Kattintson a **Start** gombra, és írja be az **R**. A **Futtatás** párbeszédpanelen írja be az **mmc** , és kattintson az **OK gombra**.

    ![Indítsa el az MMC konzolhoz](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)

2. Kattintson a **Felhasználói fiókok felügyelete** üzenet kattintson az **Igen** MMC (Microsoft Management Console) rendszergazdaként elindítására.

3. Kattintson a **fájl** menü **és törlése az illesztés – ezzel …**.

    ![Beépülő modul hozzáadása MMC konzolhoz](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)

4. **Hozzáadása vagy eltávolítása a beépülő modul** párbeszédpanelen jelölje be a **tanúsítványok** beépülő modult, és kattintson a **hozzáadása >** gombra.

    ![Tanúsítványok beépülő modul hozzáadása MMC konzolhoz](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)

5. A **Tanúsítványok beépülő modul** varázslóban jelölje ki a **számítógépfiók** , és kattintson a **Tovább**gombra.

    ![Tanúsítványok beépülő modul számítógépfiók hozzáadása](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)

6. A **Számítógép kiválasztása** lapon válassza az **helyi számítógépen: (a számítógép a konzol fut)** , és kattintson a **Befejezés gombra**.

    ![Tanúsítványok beépülő modul - választó számítógép hozzáadása](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)

7. **Hozzáadása vagy eltávolítása a beépülő modul** párbeszédpanelen kattintson **az OK gombra** a tanúsítványok beépülő modul hozzáadása az MMC.

    ![Tanúsítványok beépülő modul hozzáadása MMC - kész](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)

8. Az MMC ablak kattintással bontsa ki a **Konzol legfelső szintű**. Meg kell jelennie a tanúsítványok beépülő modul betöltött. Kattintson a **tanúsítványok (helyi számítógép)** kibontásához. Kattintással bontsa ki a **személyes** csomópontot, és utána a **tanúsítványok** csomópontot.

    ![Nyissa meg a személyes tanúsítványok áruházból](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)

9. Meg kell jelennie a létrehozott önaláírt tanúsítványt. Ellenőrizheti, hogy annak érdekében, hogy az ujjlenyomatot egyezik-e a PowerShell windows jelenteni a tanúsítvány létrehozása után a tanúsítvány tulajdonságait.

10. Jelölje ki az önaláírt tanúsítványt, és **kattintson a jobb gombbal**. A helyi menüből válassza a **Minden tevékenység** , és válassza a **Exportálás lehetőséget**.

    ![Tanúsítvány exportálása](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)

11. A **Tanúsítvány exportálás varázsló**kattintson a **Tovább**gombra.

    ![Exportálás varázsló tanúsítvány](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)

12. A **Titkos kulcs exportálása** lapon jelölje be az **Igen, a titkos kulcs exportálását**, és kattintson a **Tovább**gombra.

    ![Exportálás tanúsítvány titkos kulcs](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [AZURE.WARNING] A tanúsítványt és titkos kulcs exportálhatók Ha megad egy PFX, amely nem tartalmaz a titkos kulcs a tanúsítvány, biztonságos LDAP felügyelt tartománya engedélyezése sikertelen lesz.

13. A **Fájlformátum exportálása** lapon válassza a személyes információkat Exchange - PKCS #12 **(. PFX)** az exportált tanúsítványt fájlformátumot.

    ![Exportálási tanúsítvány fájlformátum](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [AZURE.NOTE] Csak a. PFX fájlformátum támogatott. A tanúsítvány nem exportálja a. CER fájlformátumban.

14. A **Biztonság** lapon jelölje be a **jelszó** lehetőséget, és írja a jelszóval védheti a. PFX fájlt. Ne felejtse el ezt a jelszót, mivel a következő feladat lesz szükség. Kattintson a **Tovább gombra** a folytatáshoz.

    ![Tanúsítvány exportálás jelszavának ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [AZURE.NOTE] Jegyezze fel a jelszót. Ha szüksége lenne rá engedélyezése a felügyelt tartományhoz a [tevékenység-3 - engedélyezze a biztonságos LDAP felügyelt tartományhoz tartozó](#task-3---enable-secure-ldap-for-the-managed-domain) biztonságos LDAP során

15. **Az exportálandó fájl** lapján adja meg a fájl nevét, és a helyet, ahol meg szeretné exportálni a tanúsítvány.

    ![Tanúsítvány exportálás elérési útja](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)

16. A következő lapon kattintson a **Befejezés** exportálja a tanúsítványt PFX-fájlba. A tanúsítvány exportálásakor látnia kell törlésének megerősítését kérő párbeszédpanel.

    ![Kész tanúsítvány exportálása](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="task-3---enable-secure-ldap-for-the-managed-domain"></a>Tevékenység 3 - engedélyezése biztonságos LDAP felügyelt tartományhoz tartozó
Ahhoz, hogy a biztonságos LDAP, hajtsa végre az alábbi konfigurálási lépéseket:

1. Nyissa meg az **[Azure klasszikus portálon](https://manage.windowsazure.com)**.

2. Jelölje ki az **Active Directory** -csomópontot, a bal oldali ablaktáblában.

3. Jelölje ki, amelynek engedélyezte a Azure Active Directory tartományi szolgáltatások (más néven "bérlői"), Azure Active Directory könyvtárat.

    ![Jelölje ki az Azure Active Directory](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Kattintson a **beállítás** fülre.

    ![Konfigurálja a címtár lapja](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Görgessen le a **tartományi szolgáltatások**című szakaszt. Meg kell jelennie című **Biztonságos LDAP (LDAPS)** , az alábbi képernyőképen látható módon lehetőségek közül:

    ![Tartományi szolgáltatások konfigurációs szakasz](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)

6. A **beállítás tanúsítvány …** gombra kattintva jelenítse meg a **Biztonságos LDAP tanúsítvány beállítása** párbeszédpanel.

    ![Biztonságos LDAP-tanúsítvány beállítása](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)

7. Kattintson a mappa ikonra, adja meg a felügyelt tartomány biztonságos LDAP eléréséhez használni kívánt tanúsítványt tartalmazó fájlt PFX **PFX fájl tanúsítvánnyal** alatt. A tanúsítvány exportálásakor a PFX-fájl a megadott jelszót is megadhatja. Kattintson az alján a Kész gombra.

    ![Biztonságos LDAP PFX fájl és a jelszó megadása](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)

8. A **tartományi szolgáltatások** szakasz a **Configure** lap első szürke, és a **függőben...** állapot néhány percig. Az időszak alatt a LDAPS tanúsítvány helyességét ellenőrzött lesz, és biztonságos LDAP felügyelt tartománya van beállítva.

    ![LDAP - biztonságos Függőben állapot](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

    > [AZURE.NOTE] Felügyelt tartománya biztonságos LDAP engedélyezése körülbelül 10 – 15 percet vesz igénybe. Ha a megadott biztonságos LDAP tanúsítvány nem egyezik meg a szükséges feltételt, biztonságos LDAP engedélyezve van a címtárában, és hiba jelenik. Például a tartománynév nem megfelelő, a tanúsítvány lejárt vagy stb hamarosan lejár.

9. Ha a biztonságos LDAP sikeresen engedélyezve van a felügyelt tartomány a **függőben lévő...** üzenet el kell tűnniük. Meg kell jelennie a ujjlenyomat a tanúsítvány jelenik meg.

    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>


## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a>Tevékenység - 4-es engedélyezése biztonságos LDAP hozzáférést az interneten
A **választható tevékenység** – Ha nem szeretné a felügyelt tartomány LDAPS használatával az interneten keresztül hozzáférés, kihagyhatja ezt a műveletet konfigurációs.

Ez a tevékenység megkezdése előtt győződjön meg róla, befejezése a [tevékenység 3](#task-3---enable-secure-ldap-for-the-managed-domain)című témakörben ismertetett lépéseket.

1. Meg kell jelennie egy **Engedélyezése biztonságos LDAP ACCESS keresztül az INTERNETEN** **a lap** **tartományi szolgáltatások** részében lehetőséget. Ez a beállítás értéke **nem** alapértelmezés szerint óta a felügyelt tartomány felett biztonságos LDAP interneten keresztüli elérését alapértelmezés szerint nincs engedélyezve.

    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)

2. Váltás a **ENGEDÉLYEZZE a biztonságos LDAP HOZZÁFÉRÉST az INTERNETEN keresztül** **Igen**értékre. Kattintson az alsó panelen kattintson a **MENTÉS** gombra.
    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)

3. A **tartományi szolgáltatások** szakasz a **Configure** lap első szürke, és a **függőben...** állapot néhány percig. Egy idő után internet-hozzáféréssel a felügyelt tartomány biztonságos LDAP keresztül érhető el.

    ![LDAP - biztonságos Függőben állapot](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

    > [AZURE.NOTE] Internet-hozzáférés engedélyezése a felügyelt tartomány biztonságos LDAP fölé KB tart.

4. Ha az interneten keresztül a felügyelt tartomány LDAP biztonságos hozzáférés sikeresen engedélyezve van, a **függőben...** üzenet el kell tűnniük. Meg kell jelennie a külső IP-cím LDAPS a **Külső IP-CÍMÉT az LDAPS ELÉRÉST**mező fölé a címtárában eléréséhez használható.

    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a>A tevékenység 5 – a felügyelt tartomány eléréséhez az internetről származó DNS konfigurálása
A **választható tevékenység** – Ha nem szeretné a felügyelt tartomány LDAPS használatával az interneten keresztül hozzáférés, kihagyhatja ezt a műveletet konfigurációs.

Ez a tevékenység megkezdése előtt győződjön meg róla, befejezése a [tevékenység 4](#task-4---enable-secure-ldap-access-over-the-internet)című témakörben ismertetett lépéseket.

Miután engedélyezte a biztonságos LDAP hozzáférést az interneten keresztül a felügyelt tartomány, módosítania kell, hogy az ügyfélszámítógépek megtalálhatja a felügyelt tartomány DNS. A tevékenység 4 végén külső IP-címet a **beállítás** lapon, a **Külső IP-CÍMÉT az LDAPS ELÉRÉST**jelenik meg.

Állítsa be a külső DNS-szolgáltatónál, úgy, hogy a tartománynév felügyelt tartomány (például contoso100.com) mutató külső IP-cím. Ebben a példában az alábbi DNS-bejegyzés létrehozása szükség:

    contoso100.com  -> 52.165.38.113

Ez - most már készen áll a felügyelt tartomány használata a biztonságos LDAP az interneten keresztül csatlakozhat.

> [AZURE.WARNING] Ne feledje, hogy az ügyfélszámítógépek a kibocsátó LDAPS tanúsítvány engedélyezni szeretné a sikeres csatlakozás LDAPS felügyelt tartomány kell bíznia. Ha használja a vállalati hitelesítésszolgáltató, illetve egy nyilvánosan megbízható hitelesítésszolgáltató, nem kell tennie semmit sem, mivel ügyfélszámítógépekre megbízható ezek tanúsítvány kibocsátó. Ha önaláírt tanúsítványt használja, akkor telepítenie kell az önaláírt tanúsítványt nyilvános részét az ügyfélszámítógépen megbízható tanúsítvány tárolóba.

<br>

## <a name="related-content"></a>Kapcsolódó tartalom

- [Az Azure Active Directory tartományi szolgáltatások felügyelt tartomány felügyelete](active-directory-ds-admin-guide-administer-domain.md)
