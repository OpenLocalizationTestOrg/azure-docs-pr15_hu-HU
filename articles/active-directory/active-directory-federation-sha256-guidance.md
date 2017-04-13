<properties
    pageTitle="Aláírás ujjlenyomatot módosítása az Office 365 válasz felek adatvédelmi |} Microsoft Azure"
    description="Ezen az oldalon nyújt útmutatást az Office 365-tel összevonási megbízhatósági SHA algoritmus módosítása"
    keywords="SHA1, SHA256, Office 365, az összevonási aadconnect, adfs, az ad fs, módosítás sha, összevonási megbízhatósági felek adatvédelmi használna"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="samueld"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="change-signature-hash-algorithm-for-office-365-replying-party-trust"></a>Aláírás ujjlenyomatot módosítás felek adatvédelmi válasz Office 365-höz

## <a name="overview"></a>– Áttekintés

Azure Active Directory összevonási szolgáltatások (AD FS) bejelentkezik a Microsoft Azure Active Directory annak érdekében, hogy azok nem lehet hamisították tokenek. Az aláírás SHA1 vagy SHA256 alapulhatnak. Azure Active Directory-aláírással rendelkező SHA256 algoritmus tokenek támogatja, és azt javasoljuk, a jogkivonat-aláíró algoritmus a legmagasabb szintű biztonság SHA256 beállítása. Ez a cikk ismerteti a jogkivonat-aláíró algoritmus szintű a biztonságosabbá SHA256 beállításához szükséges lépéseket.

## <a name="change-the-token-signing-algorithm"></a>Jogkivonat-aláíró algoritmus módosítása

Miután beállította az aláírási algoritmus az alábbi két folyamatok egy, az Active Directory összevonási szolgáltatások ellátja a tokenek használna felek adatvédelmi SHA256 az Office 365-höz. Nem kell bármilyen további konfigurációs módosítást, és ez módosítása nincs hatással van az Ön számára az Office 365-ös vagy más Azure AD-alkalmazások eléréséhez.

### <a name="ad-fs-management-console"></a>Active Directory összevonási szolgáltatások kezelése konzolban

1. Nyissa meg az Active Directory összevonási szolgáltatások kezelőkonzol az elsődleges AD FS-kiszolgálón.
2. Bontsa ki az Active Directory összevonási szolgáltatások csomópontot, és kattintson a **Külső megbízik használna**.
3. Kattintson a jobb gombbal az Office 365/Azure megbízó felek adatvédelmi, és válassza a **Tulajdonságok parancsot**.
4. Kattintson a **Speciális** fülre, és válassza a biztonságos ujjlenyomatot SHA256.
5. Kattintson az **OK gombra**.

![SHA256 aláírási algoritmus – MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>Active Directory összevonási szolgáltatások PowerShell-parancsmagok

1. Nyissa meg bármelyik AD FS-kiszolgáló rendszergazdai jogosultságokkal a PowerShell.
2. Állítsa a biztonságos ujjlenyomatot a **Set-AdfsRelyingPartyTrust** parancsmag használatával.

 <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Érdemes elolvasnia a

* [Javítsa ki az Office 365 adatvédelmi az Azure AD Connect](./active-directory-aadconnect-federation-management.md#repairing-the-trust)
