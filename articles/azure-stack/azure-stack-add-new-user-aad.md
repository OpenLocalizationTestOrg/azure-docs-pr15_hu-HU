<properties
    pageTitle="Azure Papírhalom bérlői új fiók felvétele az Azure Active Directory |} Microsoft Azure"
    description="Microsoft Azure Papírhalom ez telepít, után kell legalább egy bérlői felhasználói fiók létrehozása, így megismerheti, hogy a bérlői portálon."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Azure Papírhalom bérlői új fiók felvétele az Azure Active Directory

Miután [az Azure Papírhalom ez üzembe helyezése](azure-stack-run-powershell-script.md)bérlői felhasználói fiók, ismerje meg a bérlői portál, és tesztelje a ajánlatok és a csomagok lesz szüksége. Bérlői fiók hozhat létre, [az Azure portálon](#create-an-azure-stack-tenant-account-using-the-azure-portal) vagy [a PowerShell](#create-an-azure-stack-tenant-account-using-powershell)használatával.

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Az Azure portálon Azure Papírhalom bérlői fiók létrehozása

Az Azure portál használata az Azure előfizetéssel kell rendelkeznie.

1. Bejelentkezés az [Azure](http://manage.windowsazure.com).

2.  Microsoft Azure-bal oldali navigációs területen kattintson **Az Active Directory**.

3.  A címtár listában kattintson az Azure Papírhalom használni kívánt könyvtárra, vagy hozzon létre egy újat.

4.  A címtár-lapot kattintson a **felhasználók**elemre.

5.  Kattintson a **felhasználó hozzáadása**gombra.

6.  A **felhasználó hozzáadása** varázslót, a **felhasználó típusa** listában válassza ki **a szervezet új felhasználót**.

7.  A **felhasználónév** mezőbe írja be a felhasználó nevét.

8.  Az a **@** mezőbe, válassza ki a megfelelő bejegyzést.

9.  Kattintson a következő nyílra.

10.  A varázsló a **felhasználói profil** lapon írja be a **Keresztnév**és **vezetéknevet**, valamint **megjelenítendő nevet**.

11. A **szerepkör** listában jelölje ki a **felhasználót**.

12. Kattintson a következő nyílra.

13. Az **első ideiglenes jelszót** lapon kattintson a **Létrehozás**gombra.

14. Másolja az **új jelszót**.

15. Jelentkezzen be az új fiókot a Microsoft Azure. Módosítsa a jelszót, amikor a rendszer kéri.

16. Jelentkezzen be az `https://portal.azurestack.local` az új fiókkal, a bérlői portál megjelenítéséhez.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>A PowerShell használatával Azure Papírhalom bérlői fiók létrehozása

Ha nem egy Azure-előfizetést, az Azure portálon nem használható a bérlői felhasználói fiók felvétele. Ebben az esetben is használhatja az Azure Active Directory modul Windows Powershellhez helyette.

> [AZURE.NOTE] Ha a Microsoft Account (Live ID) üzembe Azure Papírhalom ez használ, bérlői fiók létrehozása AAD PowerShell nem használható. 

1.  Telepítse a [Microsoft Online Services – bejelentkezési segéd RTW informatikai szakembereknek](https://www.microsoft.com/en-us/download/details.aspx?id=41950).

2.  Telepítse az [Azure Active Directory modul a Windows Powershellhez (64 bites verzió)](http://go.microsoft.com/fwlink/p/?linkid=236297) , és nyissa meg.

3.  A következő parancsmagok futtatásával:




    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack PoC
   
            $msolcred = get-credential
    
    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".
    
            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId
    
    ```

4.  Jelentkezzen be az új fiók Microsoft Azure. Módosítsa a jelszót, amikor a rendszer kéri.

5.  Jelentkezzen be a `https://portal.azurestack.local` az új fiókkal, a bérlői portál megjelenítéséhez.



