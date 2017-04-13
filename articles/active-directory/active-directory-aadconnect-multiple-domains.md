<properties
    pageTitle="Azure AD Connect több tartományt"
    description="A dokumentum beállításáról és konfigurálásáról több legfelső szintű tartomány az Office 365-ös és Azure Active Directory ismertetése"
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

# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Több tartomány támogatás az Azure Active Directory összevonása
A következő dokumentáció legfelső szintű tartományok és a részleges tartományok használatát, amikor az Office 365-ben vagy az Azure Active Directory – tartományok összevonása ismerteti.

## <a name="multiple-top-level-domain-support"></a>Több legfelső szintű tartomány támogatása
Azure AD a legfelső szintű tartományok szükséges összevonása több, néhány további beállítási, nem szükséges, amikor egy felső szintű tartománnyal összevonása.

Amikor egy tartományt az Azure Active Directory identitásszolgáltatóval összevont, több tulajdonságának Azure-ban a tartományban vannak beállítva.  Egy fontos egyik IssuerUri lesz.  Ez a URI, amely segítségével az Azure Active Directory azonosítja azt a tartományt az jogkivonathoz társított.  A URI nem kell úgy, hogy bármilyen fájlok, de érvényes URI kell lennie.  Alapértelmezés szerint Azure Active Directory állítja be ezt az összevonási szolgáltatás azonosító értékét a helyszíni Active Directory összevonási szolgáltatások konfigurálása.

>[AZURE.NOTE]Az összevonási szolgáltatás azonosítója egyedileg azonosító összevonás URI.  Az összevonási szolgáltatás az Active Directory összevonási szolgáltatások funkcionáló egy példánya, a biztonsági jogkivonat szolgáltatással. 

A PowerShell parancs használatával is View IssuerUri `Get-MsolDomainFederationSettings - DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

A probléma merül fel, ha azt szeretné, hogy egynél több legfelső szintű tartomány felvétele.  Ha például vegyük beállítási összevonási közötti Azure Active Directory és a helyszíni környezetben.  A dokumentum használok bmcontoso.com.  Miután hozzáadott egy második legfelső szintű tartomány bmfabrikam.com.

![A tartományok](./media/active-directory-multiple-domains/domains.png)

Próbálja meg az összevonáshoz bmfabrikam.com tartományban konvertálni, amikor azt hibaüzenetet kap.  A hiba okát, Azure Active Directory, amely nem teszi lehetővé, hogy az azonos értékű egynél több tartományt a IssuerUri tulajdonság kényszer.  
  

![Összevonás hiba](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>SupportMultipleDomain paraméter

Megoldás: a, szükség hozzáadása egy másik IssuerUri, amelyek használatával elvégezhető a `-SupportMultipleDomain` paraméter.  A paraméter használata a következő parancsmagokat:
    
- `New-MsolFederatedDomain`
- `Convert-MsolDomaintoFederated`
- `Update-MsolFederatedDomain`

Ez a paraméter ellenőrzi, hogy a tartománynevet alapul, állítsa be a IssuerUri Azure AD.  Ez lesz egyedi könyvtárak végig az Azure Active Directory.  A paraméter használatával lehetővé teszi, hogy a sikeres PowerShell-parancsot.

![Összevonás hiba](./media/active-directory-multiple-domains/convert.png)

Megjeleníti a beállításait az új bmfabrikam.com tartomány, a következőket láthatja:

![Összevonás hiba](./media/active-directory-multiple-domains/settings.png)

Megjegyzendő, hogy `-SupportMultipleDomain` nem változtatja meg a végpontok, amelyek továbbra is megtörténik az összevonási szolgáltatásnak adfs.bmcontoso.com mutasson.

Másik megoldás, hogy `-SupportMultipleDomain` jelent azt ellenőrzi, hogy az Active Directory összevonási szolgáltatások rendszer szerepel a megfelelő kibocsátó értéket az Azure Active Directory kiadott tokenek. Ezt nem tart a felhasználóknak egyszerű Felhasználónévi tartomány részét, és állítsa ezt a IssuerUri, azaz https://{upn utótag a tartományként} / adfs/szolgáltatások/adatvédelmi. 

Így az Azure Active Directory hitelesítés alatt vagy az Office 365-ben, a felhasználó jogkivonat IssuerUri elemének segítségével keresse meg a tartomány Azure AD.  Ha nem talál egyezést a hitelesítés sikertelen lesz. 

Például ha egy felhasználó adatait a UPN is bsimon@bmcontoso.com, a token Active Directory összevonási szolgáltatások problémákat IssuerUri elemének http://bmcontoso.com/adfs/services/trust állítja be. Ez az Azure Active Directory-konfiguráció meg fognak egyezni, és hitelesítési fog sikerülni.

Az alábbiakban a testre szabott állítást szabályt, amely a összefüggés:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type =   "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


>[AZURE.IMPORTANT]A SupportMultipleDomain – Váltás a használatához, ha a hozzáadott új vagy már konvertálása kísérel meg a tartomány, a telepítő eredetileg támogatása őket a szövetséges adatvédelmi van szükség.  


## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>Az Active Directory összevonási szolgáltatások és Azure Active Directory között adatvédelmi frissítése
Ha a művelet nem beállítás: az Active Directory összevonási szolgáltatások és az Azure Active Directory-példány között a szövetséges adatvédelmi, előfordulhat, hozza létre újból a meghatalmazás.  Ennek oka, hogy mikor célszerű eredetileg beállítása nélkül a `-SupportMultipleDomain` paraméter, a IssuerUri be van állítva a az alapértelmezett értéket.  Az alábbi képernyőképen tekintheti meg a IssuerUri https://adfs.bmcontoso.com/adfs/services/trust van beállítva.

Így most, ha egy új tartomány sikeresen hozzáadta az Azure Active Directory-portálon és próbálja meg átalakíthatja használatával `Convert-MsolDomaintoFederated -DomainName <your domain>`, akkor a következő hibaüzenet.

![Összevonás hiba](./media/active-directory-multiple-domains/trust1.png)

Ha adni a `-SupportMultipleDomain` azt az alábbi hibaüzenet fog váltás:

![Összevonás hiba](./media/active-directory-multiple-domains/trust2.png)

Egyszerűen a futtatni kívánt `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` az eredeti tartományban is jár hibát jelez.

![Összevonás hiba](./media/active-directory-multiple-domains/trust3.png)

További legfelső szintű tartomány kövesse az alábbi lépéseket.  Ha már hozzáadta a tartományt, és nem használta a `-SupportMultipleDomain` paraméter először a lépéseket, eltávolítása és módosítása az eredeti tartomány.  Ha nem adja hozzá a legfelső szintű tartomány még készítésénél kiindulhat is a PowerShell az Azure AD Connect használatával tartomány hozzáadásának lépéseit.

Kövesse az alábbi lépéseket a Microsoft Online adatvédelmi eltávolításához, valamint az eredeti tartomány frissítése.

2.  Az Active Directory összevonási szolgáltatások összevonási kiszolgáló nyissa meg a **AD FS-kezelés.** 
2.  A bal oldali bontsa ki a **Megbízhatósági kapcsolatok** és **Használna fél bizalmi kapcsolatok**
3.  A jobb oldalon a **Microsoft Office 365-ben identitás Platform** bejegyzés törlése.
![A Microsoft Online eltávolítása](./media/active-directory-multiple-domains/trust4.png)
1.  Egy olyan számítógépen, amelyen telepítve van [Azure Active Directory modul Windows Powershellhez](https://msdn.microsoft.com/library/azure/jj151815.aspx) futtassa a következő: `$cred=Get-Credential`.  
2.  Adja meg a felhasználónevét és jelszavát a globális rendszergazda az Azure Active Directory tartomány, a rendszer összevonása
2.  Írja be a PowerShell`Connect-MsolService -Credential $cred`
4.  Írja be a PowerShell `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Az eredeti tartományhoz tartozó.  A fenti tartományok lenne, így használata:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`


Kövesse az alábbi lépéseket a PowerShell használatával új legfelső szintű tartomány

1.  Egy olyan számítógépen, amelyen telepítve van [Azure Active Directory modul Windows Powershellhez](https://msdn.microsoft.com/library/azure/jj151815.aspx) futtassa a következő: `$cred=Get-Credential`.  
2.  Adja meg a felhasználónevét és jelszavát a globális rendszergazda az Azure Active Directory tartomány, a rendszer összevonása
2.  Írja be a PowerShell`Connect-MsolService -Credential $cred`
3.  Írja be a PowerShell`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Az új legfelső szintű tartomány használata az Azure AD Connect kövesse az alábbi lépéseket.

1.  Indítsa el a az asztalról Azure AD Connect és a start menü
2.  Válassza a "További Azure Active Directory tartományt hozzáadása" ![felvesz egy további Azure Active Directory tartományi](./media/active-directory-multiple-domains/add1.png)
3.  Írja be az Azure Active Directory és az Active Directory hitelesítő adatok
4.  Jelölje ki a kívánt konfigurálása az összevonási második tartományt.
![Felvesz egy további Azure Active Directory tartományi](./media/active-directory-multiple-domains/add2.png)
5.  Kattintson a telepítés gombra.


### <a name="verify-the-new-top-level-domain"></a>Ellenőrizze az új legfelső szintű tartomány
A PowerShell parancs használatával `Get-MsolDomainFederationSettings - DomainName <your domain>`tekintheti meg a frissített IssuerUri.  Az alábbi képernyőképen az összevonási az eredeti tartomány http://bmcontoso.com/adfs/services/trust a frissített beállításai

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Kattintson az új tartományhoz a IssuerUri értékűre https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)


##<a name="support-for-sub-domains"></a>Szükség támogatása
A alszint tartományt, a kezelt módon Azure AD-tartományok miatt hozzáadásakor a beállításokat a szülő örökli.  Ez azt jelenti, hogy a IssuerUri van szüksége a szülők megfelelően.

Így lehetővé teszi, hogy tegyük fel például, hogy e bmcontoso.com van, és vegye fel corp.bmcontoso.com.  Ez azt jelenti, hogy a felhasználó corp.bmcontoso.com a IssuerUri kell **http://bmcontoso.com/adfs/services/trust.**  A szokásos szabályt az Azure Active Directory, a fenti végrehajtott hoz létre, mint a kibocsátó token azonban **http://corp.bmcontoso.com/adfs/services/trust.** hitelesítés meghiúsul, és amelyek nem felelnek meg a tartomány szükséges értéket.

### <a name="how-to-enable-support-for-sub-domains"></a>Hogyan lehet szükség támogatásának engedélyezése
Annak érdekében, hogy kerülhető meg az Active Directory összevonási szolgáltatások a Microsoft Online megbízó felek adatvédelmi frissítésre szorul.  Ehhez igénylése egyéni szabály meg kell adnia, hogy ki a felhasználó egyszerű Felhasználónévi utótag bármelyik alszint tartományait, szalagok, az egyéni kibocsátó érték kiszámításakor. 

Az alábbi állítás fog tegye a következőket:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));

A támogatási szükség egyéni igényt kövesse az alábbi lépéseket.

1.  Nyissa meg az Active Directory összevonási szolgáltatások kezelése
2.  Kattintson a jobb gombbal a Microsoft Online RP adatvédelmi, és kattintson a Szerkesztés állítást szabályok
3.  Jelölje ki a harmadik állítást szabályt, és a csere ![Szerkesztés igénylése](./media/active-directory-multiple-domains/sub1.png)
4.  Az aktuális állítást cseréje:
    
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
        
    a
    
        `c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));`
    
![Csere igénylése](./media/active-directory-multiple-domains/sub2.png)
5.  Kattintson az OK gombra.  Kattintson az Alkalmaz gombra.  Kattintson az OK gombra.  Zárja be az Active Directory összevonási szolgáltatások kezelése.

