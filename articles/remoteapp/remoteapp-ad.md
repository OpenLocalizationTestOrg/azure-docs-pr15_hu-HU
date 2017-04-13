
<properties 
    pageTitle="Azure Active Directory Azure RemoteApp Active Directory-követelményei a + |} Microsoft Azure" 
    description="Megtudhatja, hogy miként állíthatja be az Active Directory Azure RemoteApp végezhető." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Azure Active Directory + Azure RemoteApp Active Directory-követelményei

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .


Az Azure RemoteApp hibrid webhelycsoport vagy a felhőben webhelycsoport összevonására AD Connect használ, amelyet tegye a következőket kell.

### <a name="connect-azure-ad-and-active-directory"></a>Csatlakozás az Azure Active Directory és az Active Directory

Ha a használni kívánt a Azure AD-bérlő és a helyszíni Active Directory-környezetekben, használja a AD Connect. Egyetlen [4 kattintással](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) való csatlakozáshoz a két könyvtárak tart.

Megjegyzés: a címtár-szinkronizálás szükség a hibrid gyűjtemények.

### <a name="make-sure-your-domaincom-match"></a>Győződjön meg arról, hogy a "@domain.com" megfelelően
Mielőtt hozzákezdene, győződjön meg arról, hogy a helyszíni erdő (UPN) megfelel-e az utótag az Azure Active Directory-tartomány. 

Miután beállította a tartományt UPN-utótag az Azure Active Directory, minden felhasználónak bejelentkezés Azure RemoteApp fog bejelentkezni “user@ <the suffix you set up>". Győződjön meg arról, hogy a felhasználók is jelentkezhetnek be azonos user@suffix a helyszíni tartományba. Bizonyos esetekben állíthat be egy tartománynevet az Azure AD, miközben a felhasználó a-prem. másik tartományt utótag megadása Ebben az esetben a felhasználók nem tud csatlakozni bármelyik tartományhoz számítógépek és más erőforrások Azure RemoteApp keresztül.

Ha például a UPN-tartomány utótag AAD, mint a contoso.com beállítása, de néhány felhasználó, a helyiségek és AD meg vannak beállítva, hogy jelentkezzen be az @contoso.uk, , majd ezek a felhasználók nem tudnak megfelelően bejelentkezik a ARA gyűjteményben. Felhasználók UPN AAD és AD meg kell egyezniük, lehetséges, hogy a bejelentkezés"

### <a name="create-objects-for-azure-remoteapp"></a>Objektumok létrehozása az Azure RemoteApp
Meg kell a következő a helyszíni Active Directory-objektumok létrehozása:

- A tartomány erőforrások RemoteApp programok hozzáférést biztosít a helyszíni tartomány RDSH végpontjait összefűzésével szolgáltatási fiók.
- Szervezeti egység (szervezeti egység) RemoteApp gépi objektumokat tartalmaz. A szervezeti használata ajánlott (de nem kötelező) a partnerek és a használandó RemoteApp házirendek elkülönítése.

Van szüksége a ezeknek az objektumoknak a RemoteApp webhelycsoport létrehozásakor ügyeljen először kövesse az alábbi lépéseket.