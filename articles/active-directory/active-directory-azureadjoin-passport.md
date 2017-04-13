<properties
    pageTitle="Hitelesítés nélkül – Microsoft Passport jelszavakat identitások |} Microsoft Azure"
    description="Áttekintést nyújt a Microsoft Passport és további információt a Microsoft Passport üzembe helyezése."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="authenticating-identities-without-passwords-through-microsoft-passport"></a>Jelszavak – Microsoft Passport nélkül hitelesítő identitások

Az aktuális egyedül jelszavakkal hitelesítési módszereket nem elegendő ahhoz, hogy a felhasználók biztonsága. A felhasználók újrafelhasználása és elfelejtett jelszót. A jelszavak megkülönböztetik breachable, phishable, repedések szeretné jobban és kitalálható. Azok is megnyithatja, ne feledje, hogy nehéz és támadásokkal célú támadásokkal, például "[a kivonat megfelelt](https://technet.microsoft.com/dn785092.aspx)".

## <a name="about-microsoft-passport"></a>Tudnivalók a Microsoft Passport
Microsoft Passport személyes és nyilvános kulcs vagy hitelesítési tanúsítvány-alapú megközelítés szervezetek és a fogyasztói előbb jelszavak túl. A hitelesítési mód kulcs pár hitelesítő adatok lecserélheti a jelszavakat és megsértésével, thefts és az adathalászat támaszkodik.

 A Microsoft Passport lehetővé teszi, hogy a felhasználók egy Microsoft-fiókkal, a Windows Server Active Directory-fiókját, a Microsoft Azure Active Directory (Azure Active Directory) fiókot vagy egy-Microsoft szolgáltatás, amely támogatja a gyors identitás Online (FIDO) hitelesítési hitelesítést végezni. Az első két lépésből álló ellenőrzése során Microsoft Passport regisztrációs után Microsoft Passport be van állítva, a felhasználó eszközön, és a felhasználó lehet Windows Helló vagy a PIN-kód kézmozdulat állítja be. A felhasználó a kézmozdulat, mellyel ellenőrizheti a személyazonosságot biztosít. A Windows használja fel a Microsoft Passport hitelesíteni a felhasználót, és segítségével védett erőforrások és a szolgáltatások elérése.

A titkos kulcs lett kizárólag a PIN-kód, biometria és a távoli eszköz, például a felhasználó által jelentkezzen be az eszközt használó intelligens kártyát "felhasználó kézmozdulat" keresztül érhető el. Ezt az információt a tanúsítvány vagy egy aszimmetrikus kulcs pár van csatolva. A titkos kulcs tanúsítása, ha az eszköz a modul (Platformmegbízhatósági) processzor hardver. A titkos kulcs soha ne hagyja az eszközt.

A nyilvános kulcs regisztrálva van az Azure Active Directory és a Windows Server Active Directory (helyszíni). Identitásszolgáltatók (IDPs) ellenőrzése a felhasználó által a titkos kulcs hozzárendelése a nyilvános kulcs annak a felhasználónak, és adja meg bejelentkezési adatokat egy idő jelszó (OTP), PhoneFactor vagy egy másik értesítés mechanizmusa keresztül.

## <a name="why-enterprises-should-adopt-microsoft-passport"></a>Miért nagyvállalatoknak szolgáltatás részét képező el kell fogadnia a Microsoft Passport

Microsoft Passport engedélyezésével nagyvállalatoknak szolgáltatás részét képező teheti az erőforrások szerint még jobban biztonságos:

* A Microsoft Passport beállítása a hardver által előnyben részesített lehetőséggel. Ez azt jelenti, hogy kulcsokat hoz létre TPM 1.2-es vagy TPM 2.0-s, ha lehetséges. TPM nem érhető el, ha a szoftvert a kulcsot hoz létre.

* A PIN-kód hossza és összetettsége meghatározását, és a Yammerrel használatát van engedélyezve van-e a szervezet.

* Konfigurálja az intelligens kártya – például a tanúsítvány-alapú adatvédelmi használatával támogatása a Microsoft Passport.

## <a name="how-microsoft-passport-works"></a>Hogyan működik a Microsoft Passport
1. Billentyűk generálja a hardveren TPM vagy a szoftvert. Sok szinkronizált eszköz van egy beépített TPM processzor, hogy titkosítja a hardver cryptographic billentyűkkel integrálása eszközök szerint. TPM 1.2-es vagy TPM 2.0-s hoz létre, billentyűk vagy alapján létrehozott kulcsok létrehozott tanúsítványok.

2. A TPM igazolja a hardver kötött billentyűk.

3. Egy egyetlen feloldás kézmozdulatot feloldja az eszközt. Ez a kézmozdulat lehetővé teszi, hogy több erőforrásokhoz Ha az eszköz a tartományhoz és Azure Active Directory-tartományhoz.

## <a name="how-the-microsoft-passport-lifecycle-works"></a>Hogyan működik a a Microsoft Passport életciklus

![A Microsoft Passport életciklus](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

Az előző ábra mutatja be, a személyes és nyilvános kulcs pár és az érvényesítési az identitásszolgáltató által. Ezeket a lépéseket mindegyik magyarázza részletesen:

1. A felhasználó a személyazonosságot bizonyul több beépített helyesírás-ellenőrző módot (kézmozdulatok, fizikai intelligens kártya többtényezős hitelesítés) keresztül, és elküldi ezeket az adatokat szeretne egy személyes szolgáltató (IDP) például Azure Active Directory vagy a helyszíni Active Directory.

2. Az eszköz majd a kulcsot hoz létre, igazolja a billentyűt, a kulcs nyilvános részét elfoglalja, csatolja az állomás kimutatások, bejelentkezik és elküldi azt a IDP regisztrálhatja a billentyűt.

4. Amint a IDP regisztrálja a kulcs nyilvános része, akkor a IDP lekérdezi az eszközt, jelentkezzen be a kulcs személyes része.

5. A IDP majd ellenőrzi, és a hitelesítési jogkivonat, amely lehetővé teszi, hogy a felhasználó és eszköz-hozzáférési a védett erőforrások problémák. IDPs platformok alkalmazások írhat vagy használata támogatott böngészők (keresztül JavaScript/Webcrypto API-k) létrehozása és használata a Microsoft Passport hitelesítő adatok azok a felhasználók számára.

## <a name="the-deployment-requirements-for-microsoft-passport"></a>A Microsoft Passport telepítésének követelményei
### <a name="at-the-enterprise-level"></a>A vállalati szintű

* A vállalati az Azure előfizetéssel rendelkezik.

### <a name="at-the-user-level"></a>A felhasználói szintjén

* A felhasználó számítógépén futtatja a Windows 10 Professional vagy Enterprise.

Részletes telepítési útmutatásért lásd: [Microsoft Passport engedélyezése a szervezet munkáért](active-directory-azureadjoin-passport-deployment.md).


## <a name="additional-information"></a>További információk

* [A Windows 10, a nagyvállalati: eszközök használata munkahelyi módjai](active-directory-azureadjoin-windows10-devices-overview.md)
* [Felhőalapú lehetőségeket a Windows 10-es eszközökön keresztül Azure Active Directory Bekapcsolódás bővítése](active-directory-azureadjoin-user-upgrade.md)
* [Használatát esetek is találhat az Azure Active Directory csatlakozás](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás tartományhoz csatlakoztatott eszközök Azure AD a Windows 10-élmény](active-directory-azureadjoin-devices-group-policy.md)
* [Azure Active Directory Bekapcsolódás beállítása](active-directory-azureadjoin-setup.md)
