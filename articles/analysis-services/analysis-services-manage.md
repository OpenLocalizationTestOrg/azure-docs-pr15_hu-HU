<properties
   pageTitle="Azure Analysis Services kezelése |} Microsoft Azure"
   description="Megtudhatja, hogy miként kezelheti egy Analysis Services-kiszolgáló Azure-ban."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="manage-analysis-services"></a>Az Analysis Services kezelése

Miután létrehozta az Analysis Services-kiszolgáló Azure-ban, felügyelete és kezelése tevékenységek végrehajtásához azonnal vagy valamikor utazás közben lenyomva lehet. Adatok frissítése, hogy kik is elérheti a modelleket a kiszolgálón, vagy ha a kiszolgáló rendszerállapot figyelése feldolgozás például fut. Csak bizonyos felügyeleti feladatok hajtható végre az Azure-portálon, mások az SQL Server Management Studio (SSMS), és bizonyos feladatok végezhetők vagy.

## <a name="azure-portal"></a>Azure portál
Az [Azure portál](http://portal.azure.com/) , ahol létrehozása és törlése a kiszolgálókat, Lync server erőforrásaira, méretének módosítása, és ki fér hozzá a kiszolgálók kezelése.  Ha problémákat tapasztal problémákat, is küldhet támogatási kérelmet.

![Kiszolgálónév Ismerkedés az Azure-ban](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Csatlakozás a kiszolgálóhoz Azure-ban olyan, mint a kiszolgálói példány saját szervezet kapcsolódás. A SSMS elvégezhetők ugyanazokat a műveleteket, például a folyamat adatainak vagy hozzon létre egy feldolgozás parancsfájlt, szerepkörök kezeléséhez és PowerShell-lel.

![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

 A nagyobb különbségek egyik a hitelesítés, a kiszolgálóhoz való csatlakozáshoz használhatja. Az Azure Analysis Services-kiszolgálóhoz való kapcsolódás szüksége jelölje ki **Az Active Directory jelszó-hitelesítést**.

### <a name="to-connect-with-ssms"></a>Az SSMS való csatlakozáskor
1. Mielőtt csatlakoztatná, kell a kiszolgáló nevét. Az **Azure-portálon** > kiszolgáló > **Áttekintés** > **kiszolgáló nevét**, másolja a kiszolgáló nevét.

    ![Kiszolgálónév Ismerkedés az Azure-ban](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

2. A SSMS > **Objektum Intéző**, kattintson a **Csatlakozás** > **Analysis Services**.

3. **Csatlakozás a kiszolgálóhoz** párbeszédpanelen illessze be a kiszolgáló nevét, majd a **hitelesítés**, válassza az alábbiak egyikét:

    **Active Directory beépített hitelesítése** az Active Directoryval, az Azure Active Directory összevonási egyszeri bejelentkezés használata.

    **Az active Directory jelszó-hitelesítést** szervezeti fiókkal. Például amikor egy tartományon kívüli csatlakozás csatlakozott a számítógépen.

    Megjegyzés: Ha nem látja az Active Directory-hitelesítés, szükség lehet az [Azure Active Directory-hitelesítés engedélyezése](#enable-azure-active-directory-authentication) az SSMS.

    ![Csatlakozás a SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

Mivel a server Azure-ban kezelése SSMS használatával sokkal ugyanaz, mint egy helyszíni kiszolgálót kezelése, nem fogunk mappába érkeznek, az alábbi részleteket. Az összes Súgó meg kell megtalálható a [Analysis Services példány Management](https://msdn.microsoft.com/library/hh230806.aspx) MSDN webhelyen.

## <a name="server-administrators"></a>Kiszolgálók rendszergazdái számára
Segítségével **Analysis Services-rendszergazdák** az ellenőrzési lap az Azure portál vagy SSMS a kiszolgáló rendszergazdáinak kezelése. Analysis Services-rendszergazdák adatbázis kiszolgálók rendszergazdái számára a gyakori adatbázis felügyeleti feladatok például hozzáadása és eltávolítása az adatbázisok és a felhasználók kezelése engedéllyel. Alapértelmezés szerint Analysis Services-rendszergazdaként automatikusan hozzáadja a felhasználót, hogy a kiszolgáló létrehozása az Azure-portálon

Meg kell még figyelembe venni:

-   Windows Live ID nem Azure Analysis Services támogatott identitás típusát.  
-   Analysis Services-rendszergazdák érvényes Azure Active Directory-felhasználók kell lennie.
-   Az Azure Analysis Services-kiszolgálón keresztül Azure erőforrás-kezelő sablonok létrehozásáról, ha az Analysis Services-rendszergazdák felhasználóknak, hogy fel kell venni, mint rendszergazdák JSON tömbje vesz igénybe.

Analysis Services-rendszergazdák Azure erőforrás-rendszergazdák kezelheti az Azure előfizetések erőforrások eltérő lehet. A meglévő XMLA és TSML való kompatibilitás az Analysis Services viselkedése kezelése, és lehetővé teszi a feladatok Azure erőforrás-kezelés és elemzési között újból Services-adatbázis kezelése fenntartja.

Összes szerepkörének megtekintéséhez és elérni a Azure Analysis Services-erőforrás típusa (IAM) hozzáférés-vezérlés az ellenőrzési lap a használata.

## <a name="database-users"></a>Adatbázis-felhasználók
Azure Analysis Services modell adatbázis-felhasználók az Azure Active Directory kell lennie. A modell adatbázis megadott felhasználónevét szervezeti e-mail címét vagy az egyszerű Felhasználónévi kell lennie. Ez különbözik a helyszíni modell adatbázisok, amely támogatja a felhasználók által Windows tartomány felhasználónevét.

Bármikor felvehet felhasználókat, vagy [Táblázatos modell parancsfájlok nyelvet](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) használ az SQL Server Management Studio [szerepkör-hozzárendelések az Azure Active Directory](../active-directory/role-based-access-control-configure.md) segítségével.

**TMSL mintaparancsfájl**

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Users"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@contoso.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="enable-azure-active-directory-authentication"></a>Azure Active Directory-hitelesítés engedélyezése
A beállításjegyzékben SSMS az Azure Active Directory authentication szolgáltatás engedélyezéséhez hozzon létre egy szövegfájlt, névvel ellátott EnableAAD.reg, majd másolja a vágólapra, és illessze be a következőt:


```
Windows Registry Editor Version 5.00
[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\Microsoft Analysis Services\Settings]
"AS AAD Enabled"="True"
```

Mentse és futtassa a fájlt.



## <a name="next-steps"></a>Következő lépések
Ha az új kiszolgálóhoz már még nem telepítette az táblázatos modellből, ezt követően egy időben. További tudnivalókért olvassa el a [Deploy Azure Analysis Services rendszerhez](analysis-services-deploy.md)című témakört.

Ha már telepítette a modell a kiszolgálóhoz, készen áll egy ügyfél vagy a böngésző használatával kapcsolódik hozzá. További tudnivalókért olvassa el a [Azure Analysis Services-kiszolgáló adatok beolvasása](analysis-services-connect.md)című témakört.
