<properties 
    pageTitle="Első lépések az iOS-alapú tanúsítvány hitelesítés |} Microsoft Azure" 
    description="Megtudhatja, hogy miként megoldások alapú hitelesítés beállítása az iOS-eszközök" 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/21/2016" 
    ms.author="markvi" />



# <a name="get-started-with-certificate-based-authentication-on-ios---public-preview"></a>Első lépések az iOS - Public Preview tanúsítvány-alapú hitelesítés

> [AZURE.SELECTOR]
- [iOS](active-directory-certificate-based-authentication-ios.md)
- [Android-alapú](active-directory-certificate-based-authentication-android.md)


Ez a témakör bemutatja, hogyan beállítása és az Office 365 Enterprise, vállalati és oktatási csomag a tanúsítvány-alapú hitelesítés (CBA) felhasználók iOS-eszközökön bérlők a csatlakozást. 

CBA sikerült hitelesíteni Azure Active Directory Android és IOS rendszerű eszközökön ügyfél-tanúsítványt az Exchange online-fiókkal való csatlakozáskor teszi lehetővé: 

- Például a Microsoft Outlook és a Microsoft Word mobile Office-alkalmazások   
- Exchange ActiveSync (EAS) ügyfelek 

Ez a szolgáltatás konfigurálását szükségtelenné felhasználónév és jelszó együttes kezdenek bizonyos Posta és a Microsoft Office-alkalmazásokat a mobileszközén. 
 

## <a name="supported-scenarios-and-requirements"></a>Támogatott esetek és követelmények  



### <a name="general-requirements"></a>Általános követelmények 


Ha a jelen témakör minden esetben az alábbi műveleteket szükség:  

- Tanúsítvány authority(s) ügyfél tanúsítványokat való hozzáférést.  

- A tanúsítványok authority(s) az Azure Active Directory kell beállítania. Végezze el a konfigurálását az [Első lépések](#getting-started) szakaszában olvashat a lépések részletes leírását is megkeresheti.  

- A legfelső szintű hitelesítésszolgáltató és bármely köztes hitelesítésszolgáltatók Azure Active Directory kell beállítania.  

- Minden egyes hitelesítésszolgáltató rendelkeznie kell a tanúsítvány-visszavonási lista (CRL), hogy az URL-címet az internetes keresztül hivatkozhat.  

- Az ügyfél tanúsítványt az ügyfél-hitelesítést.  


- Csak az Exchange ActiveSync-ügyfelek az ügyfél-tanúsítvány rendelkeznie kell a felhasználó átirányítható e-mail címét az Exchange online egyszerű nevét vagy a tárgy alternatív neve mező értékének RFC822 nevére. Azure Active Directory a proxycím attribútum a címtárban a RFC822 értéket rendeli.  



### <a name="office-mobile-applications-support"></a>Office mobile-alkalmazásokat támogatja 

| Alkalmazások                      | Támogatás      |
| ---                       | ---          |
| A Word és az Excel vagy PowerPoint | ![Jelölőnégyzet][1]  |
| OneNote-ban                   | ![Jelölőnégyzet][1]  |
| Onedrive-on                  | ![Jelölőnégyzet][1]  |
| Az Outlook                   | ![Jelölőnégyzet][1]  |
| A Yammer                    | ![Jelölőnégyzet][1]  |
| A Skype vállalati verzió        | hamarosan  |


### <a name="requirements"></a>Követelmények  

Az eszköz operációs rendszer verziója kell lennie az iOS 9-es vagy újabb 

Összevonási kiszolgálók kell beállítania.  

Azure hitelesítő szükség az iOS Office-alkalmazásokat.  

Az Azure Active Directory vissza szeretné vonni az ügyfél-tanúsítványt a ADFS jogkivonat a következő jogcímeken kell rendelkeznie:  

  - `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
(Dátumértékét az ügyfél-tanúsítvány) 

  - `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
(Karakterlánc, az ügyfél-tanúsítvány a kibocsátó) 

Azure Active Directory hozzáadja ezeket a frissítés jogkivonat jogcímalapú, ha elérhetők az ADFS jogkivonathoz (vagy bármely más SAML jogkivonat). Ha a frissítés jogkivonat kell érvényesíteni, ezt az információt a visszavonás ellenőrzése szolgál. 

A legjobb módszer a az ADFS hibalapjainak frissítenie kell a következő:

- Az Azure hitelesítő telepítése iOS kötelező

- Felhasználói tanúsítvány beszerzése útmutatást. 

További részletekért olvassa el [az AD FS bejelentkezés lapok testreszabása](https://technet.microsoft.com/library/dn280950.aspx).  



### <a name="exchange-activesync-clients-support"></a>Támogatja az Exchange ActiveSync-ügyfelek 


A 9-es vagy újabb verzió iOS az adott nyelvhez tartozó iOS levelezőprogram használata támogatott. Minden más Exchange ActiveSync alkalmazást annak megállapításához, ha ez a funkció támogatásának, lépjen kapcsolatba az alkalmazás fejlesztője.  



## <a name="getting-started"></a>Első lépések 


Első lépésként meg kell a hitelesítésszolgáltatók konfigurálása az Azure Active Directory. Az egyes hitelesítésszolgáltató töltse fel a következőket: 

- A tanúsítvány *.cer* formátumban nyilvános része 

- Az Internet szemben lévő URL-címeit, amelyben lakik a tanúsítvány-visszavonási listák (CRL-ek)
 

Az alábbi van egy hitelesítésszolgáltató sémájának: 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 


Töltse fel az adatokat, az Azure Active Directory modul Windows Powershellen keresztül is használhatja.  
Az alábbiakban példákat is tartalmaz gyarapításával, eltávolításával vagy egy hitelesítésszolgáltató módosítása. 



### <a name="configuring-your-azure-ad-tenant-for-certificate-based-authentication"></a>A tanúsítvány Azure AD-bérlő konfigurálása alapú hitelesítés 

1. Indítsa el a Windows PowerShell rendszergazdai jogosultságokkal. 

2. Az Azure Active Directory modul telepítése. [1.1.143.0](http://www.powershellgallery.com/packages/AzureADPreview/1.1.143.0) verzió telepítéséhez szükséges vagy újabb verziójában.  

        Install-Module -Name AzureADPreview –RequiredVersion 1.1.143.0 

3. Csatlakozás a célhely bérlőjén: 

        Connect-AzureAD 

### <a name="adding-a-new-certificate-authority"></a>Egy új hitelesítésszolgáltató hozzáadása

1. Adja meg a hitelesítésszolgáltató különböző tulajdonságait, és vegye fel az Azure Active Directory: 

        $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
        $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
        $new_ca.AuthorityType=0 
        $new_ca.TrustedCertificate=$cert 
        New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 

5. A hitelesítésszolgáltatók kap: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="retrieving-the-list-certificate-authorities"></a>A lista hitelesítésszolgáltatók beolvasása

A jelenleg a bérlői webhelyen tárolt az Azure Active Directory hitelesítésszolgáltatók beolvasása: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="removing-a-certificate-authority"></a>Egy hitelesítésszolgáltató eltávolítása

1.  A hitelesítésszolgáltatók beolvasása: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Távolítsa el a hitelesítésszolgáltató tanúsítványát: 

        Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiying-a-certificate-authority"></a>Modfiying a hitelesítésszolgáltató 

1.  A hitelesítésszolgáltatók beolvasása: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Kattintson a hitelesítésszolgáltató tulajdonságainak módosítása: 

        $c[0].AuthorityType=1 

3. Állítsa be a **hitelesítésszolgáltató**: 

        Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 




## <a name="testing-office-mobile-applications"></a>Office mobile-alkalmazások tesztelése  

A mobil Office-alkalmazás tanúsítvány-hitelesítés ellenőrzése: 

1.  Teszt eszközén telepítése egy mobil Office-alkalmazást (például onedrive-on) az alkalmazás-áruházából.

2.  Győződjön meg arról, hogy a felhasználó tanúsítvány van kiépítve próba eszközére. 

3.  Indítsa el az alkalmazást. 

4.  Írja be felhasználónevét, és válassza ki a használni kívánt felhasználói tanúsítványt. 

Meg kell tud bejelentkezni. 





## <a name="testing-exchange-activesync-client-applications"></a>Az Exchange ActiveSync-ügyfélalkalmazásokban tesztelése

Exchange ActiveSync használatához tanúsítvány-alapú hitelesítés keresztül egy EAS profilt az ügyfél tanúsítványt tartalmazó elérhetővé kell lennie. Az EAS-profilnak tartalmaznia kell a következő adatokat:

- A felhasználó tanúsítvány hitelesítéshez 

- Az EAS végpontot kell outlook.office365.com (Ez a funkció jelenleg csak az Exchange online több bérlői környezetben támogatja)

Az EAS profil konfigurálható és elhelyezni egy MDM, például az Intune vagy kézzel helyezze a tanúsítvány az eszközön EAS profilban hasznosítása keresztül az eszközön.  

### <a name="testing-eas-client-applications-on-ios"></a>EAS ügyfélalkalmazások iOS tesztelése 

A natív mail alkalmazással a iOS 9-es vagy újabb hitelesítés ellenőrzése: 

1.  Az EAS-profilt, amely megfelel a fenti állíthat be. 

2.  Telepíteni a profilt az iOS-eszközön (vagy használatával-ös mobileszköz-kezelés, például az Intune és az Apple konfigurációs alkalmazás)

3.  A profil megfelelően van telepítve, nyissa meg az eredeti Posta alkalmazást, és ellenőrizze, hogy levelezési szinkronizál



## <a name="revocation"></a>Visszavonási

Azure Active Directory vissza szeretné vonni az ügyfél-tanúsítvány, beolvassa a tanúsítvány-visszavonási lista (CRL) hitelesítésszolgáltató adatai részeként feltöltött URL-címét a, és azt a gyorsítótárban. Az utolsó közzététele időbélyeg (**Napjával** tulajdonság), a CRL-alapú legyen a CRL továbbra is érvényes. A CRL rendszeres szeretné visszavonni a hozzáférést a listából egy részét képező tanúsítványok hivatkozik.

Ha egy további azonnali visszavonási szükséges (például a felhasználó elveszíti eszköz), a jóváhagyási jogkivonat, a felhasználó érvényét a is. A jóváhagyási jogkivonat érvénytelenítik, **StsRefreshTokenValidFrom** mező beállítása a Windows PowerShell szolgáltatással adott felhasználó számára. A **StsRefreshTokenValidFrom** mező minden szeretné visszavonni a hozzáférést a felhasználónak kell frissítenie.
 
Győződjön meg arról, hogy a visszavonási továbbra is fennáll, értékre kell állítani a CRL a **Változási dátum** után az érték **StsRefreshTokenValidFrom** által beállított, és a tanúsítvány biztosítása egy dátumot az adott van a CRL.
 
Az alábbi lépésekkel tagolása a folyamat, frissítése és a jóváhagyási jogkivonat érvénytelenítsék az **StsRefreshTokenValidFrom** mező megadásával. 

1. Csatlakozás a MSOL szolgáltatás rendszergazdai hitelesítő adataival: 

        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

1.  A felhasználó aktuális StsRefreshTokensValidFrom érték beolvasása: 

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 


1.  Az új StsRefreshTokensValidFrom értéket beállítása az aktuális időbélyeg a felhasználó egyenlő: 

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")


A dátum, beállíthatja a jövőben kell lennie. Ha a dátumot a jövőben nem szerepel, a **StsRefreshTokensValidFrom** tulajdonság nincs beállítva. Ha a dátuma későbbi, az aktuális idő (nem a Set-MsolUser parancs jelölt dátum) **StsRefreshTokensValidFrom** van beállítva. 



<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png