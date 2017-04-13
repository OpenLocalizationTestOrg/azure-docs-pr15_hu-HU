<properties 
   pageTitle="Azure Active Directory hozzáadása csatlakoztatott szolgáltatások az Visual Studio |} Microsoft Azure"
   description="Azure Active Directory felvétele a Visual Studio csatlakoztatott-szolgáltatás hozzáadása párbeszédpanel használatával"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="active-directory"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Azure Active Directory hozzáadása a Visual Studio segítségével a csatlakoztatott szolgáltatások 

##<a name="overview"></a>– Áttekintés
Azure Active Directory (Azure Active Directory) használatával az ASP.NET MVC webalkalmazások vagy az Active Directory Authentication a webes API-szolgáltatások támogatja a egyszeri bejelentkezés (SSO). Az Azure Active Directory-hitelesítés a felhasználók segítségével Azure AD át a fiókok csatlakoztatása a webes alkalmazásokat. Azure Active Directory Authentication webes API-val előnyei többek között nagyobb adatbiztonság, amikor egy API-t egy webalkalmazás ki. Azure Active Directory, a nem rendelkezik a saját fiók és a felhasználó-ös külön hitelesítési rendszer kezeléséhez.

## <a name="supported-project-types"></a>Támogatott projekttípus létrehozása

A csatlakoztatott szolgáltatások párbeszédpanelen az alábbi projekttípusok az Azure Active Directory csatlakozhat is használhatja.

- ASP.NET MVC projektek

- ASP.NET webes API projektek


### <a name="connect-to-azure-ad-using-the-connected-services-dialog"></a>Kapcsolódás az Azure Active Directory, a csatlakoztatott szolgáltatások párbeszédpanelen

1. Győződjön meg arról, hogy fiókja van, az Azure. Ha nincs Azure-fiók, jelentkezzen az [ingyenes próbaverziót](http://go.microsoft.com/fwlink/?LinkId=518146).

1. A Visual Studióban nyissa meg a helyi menü **hivatkozás** csomópont a projektben, és válassza a **Csatlakoztatott szolgáltatások hozzáadása**.
1. Jelölje ki az **Azure Active Directory Authentication** , és válassza a **beállítás**.

    ![Válassza a Azure Active Directory Authentication hozzáadása](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. Jelölje be az első oldalon a **konfigurálása Azure Active Directory Authentication**, **állítsa be az egyszeri bejelentkezés használata az Azure Active Directory**.

    Ha a projekt egy másik hitelesítési beállításokkal van konfigurálva, a varázsló figyelmeztet, hogy a Folytatás letiltja az előző konfiguráció.

    ![Azure Active Directory konfigurálása varázsló](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1.  A második oldalon jelölje ki a tartományt a **tartomány** legördülő listából. A tartományok listájában a Fiókbeállítások párbeszédpaneljén fiókjaihoz által elérhető összes tartományok tartalmazza. Alternatívájaként adhat meg a tartomány nevét, ha nem találja, amit keres, például mydomain.onmicrosoft.com azzal. Megadhatja, hogy a beállítás, hozzon létre egy új Azure AD-alkalmazást vagy a beállítások egy meglévő Azure AD-alkalmazásból. 

    ![Azure Active Directory konfigurálása varázsló](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)


1. A varázsló harmadik lapja ellenőrizze, hogy **olvassa el a címtár-adatok** be van jelölve. A varázsló ekkor kitölti az **ügyfél titkos**. 

    ![Azure Active Directory konfigurálása varázsló](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Kattintson a **Befejezés** gombra. A párbeszédpanel összeadja a szükséges konfigurációs kód és a hivatkozások ahhoz, hogy a projekt a Azure AD-hitelesítést. Az Active Directory tartományi megtekintheti az [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Tekintse át a következő lépésekkel használatára a böngészőben megjelenő első lépések lap és a Mi történt a lapon, a projekt módosításának módját megjelenítéséhez. Ha azt szeretné, jelölje be a mindent működésének, nyissa meg az egyik módosított konfigurációs fájl, és ellenőrizze, hogy mi történt az említett beállításainak van. A fő web.config ASP.NET MVC projektben például választania kell hozzá ezeket a beállításokat:

        <appSettings> 
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:AADInstance" value="https://login.windows.net/" />
            <add key="ida:Domain" value="Your selected domain" />
            <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
            <add key="ida:PostLogoutRedirectUri" value="The default redirect URI from the project" />
        </appSettings>

## <a name="how-your-project-is-modified"></a>Hogyan a projekt módosul.

A varázsló futtatásakor a Visual Studio összeadja az Azure Active Directory és hivatkozásokat a projekthez tartozó. Konfigurációs és a projekt kód fájlok is módosított Azure Active Directory támogatásának hozzáadására. A megadott módosítások, amely lehetővé teszi a Visual Studio attól függenek, hogy a projekt típusa. Hogyan ASP.NET MVC projektek módosulnak részletes információt talál az [Milyen történt – MVC projektek](http://go.microsoft.com/fwlink/p/?LinkID=513809). Webes API-projektekhez megtudhatja, [Mi történt – webes API projektek](http://go.microsoft.com/fwlink/p/?LinkId=513810).

##<a name="next-steps"></a>Következő lépések

Kérdések és segítség kérése.

 - [Fórum az MSDN webhelyen: Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)

 - [Azure AD-dokumentáció](https://azure.microsoft.com/documentation/services/active-directory/)

 - [Blogbejegyzés: Bevezetés az Azure Active Directory](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

