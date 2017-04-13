
<properties 
    pageTitle="Azure RemoteApp használata az Office 365 felhasználói fiókok |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure RemoteApp használata az Office 365-felhasználói fiókok"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-azure-remoteapp-with-office-365-user-accounts"></a>Azure RemoteApp használata az Office 365 felhasználói fiókok

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Ha van Office 365-előfizetést, az Azure Active Directory, a felhasználóneveket és a szolgáltatások az Office 365 eléréséhez használt jelszót tároló van. Például a felhasználók aktiválása az Office 365 ProPlus azok hitelesítést Azure AD a licencek ellenőrzéséhez. A legtöbb ügyfelek szeretné az Azure RemoteApp ugyanabban a könyvtárban használja.

Azure RemoteApp telepítése esetén az Azure előfizetéssel társított egy másik Azure Active Directory valószínűleg használ. Az Office 365-ben címtár használatához, szüksége lesz az Azure előfizetés áthelyezése könyvtárra.

Az Office 365-ügyfélalkalmazásokban telepítéséről című témakörben [használata az Office 365-előfizetés Azure RemoteApp](remoteapp-officesubscription.md).
 
## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a>1 fázis: Regisztráljon az ingyenes Office 365 Azure Active Directory-előfizetés
Ha az Azure klasszikus portálon használja, ismertetett lépésekkel hozhatja létre [Regisztráljon az ingyenes Azure Active Directory-előfizetés](https://technet.microsoft.com/library/dn832618.aspx) felügyeleti férhet hozzá az Azure AD az Azure adatkezelési portálon keresztül. Ezt a folyamatot, eredménye jelentkezzen be az Azure-portálra, és lásd: a címtárában van – ezen a ponton, akkor nem látható sokkal több mivel a teljes Azure előfizetés a Azure RemoteApp használja egy másik könyvtár kell lennie.

Ne feledje, hogy a nevét és jelszavát a rendszergazdai fiók ebben a lépésben a létrehozott – ezek a fázis 2 szükség lesz.

Ha az Azure portálon használja, olvassa el a [hogyan regisztrálhatja és aktiválása az Office 365 portálon, ingyenes Azure Active Directory](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).

## <a name="phase-2-change-the-azure-ad-associated-with-your-azure-subscription"></a>2 fázis: Módosíthatja az Azure Active Directory az Azure-előfizetéséhez társított.
Lépés az aktuális könyvtár az Azure előfizetés átalakítása az Office 365-ben címtár azt időadatok használata a fázis 1.

Kövesse a [módosítása az Azure Active Directory-bérlői az Azure RemoteApp](remoteapp-changetenant.md)leírt utasításokat. Adott figyelmet fordítani az alábbi lépéseket:

- Lépés a #1: Azure RemoteApp (ARA) telepítette az előfizetéshez, győződjön meg arról, előtt távolítsa el az összes Azure AD-fiókokkal bármely ARA gyűjtemények először próbálja bármely más. Azt is megteheti megpróbálhatja, bármelyik meglévő webhelycsoportok törlése.
- A #2: Ez a lépés álljanak. Akkor használja a Microsoft-fiókkal (például @outlook.com) a szolgáltatás rendszergazdájaként az előfizetéshez tartozó; ennek oka, hogy azt nem lehet a meglévő Azure AD az előfizetés – csatolva az összes olyan felhasználói fiók azt elvégezni, ha azt nem tudja áthelyezheti egy másik Azure AD.
- #4 lépésben: Hozzáadása meglévő könyvtárat, a rendszer megkérdezi, hogy az adott könyvtár, jelentkezzen be a rendszergazdai fiókjával. Ellenőrizze, hogy a rendszergazdai fiók fázis 1.
- #5 lépés: Az előfizetés a szülő könyvtár módosítsa az Office 365-ben címtár. A végeredmény kell, hogy a beállítások -> előfizetések az előfizetést az Office 365-ben címtár sorolja fel. 
![Az előfizetés a szülő könyvtár módosítása](./media/remoteapp-o365user/settings.png)
 

Ezen a ponton Azure RemoteApp előfizetése társítva az Office 365 Azure Active Directory; Azure RemoteApp használhatja a meglévő Office 365 felhasználói fiókok!




