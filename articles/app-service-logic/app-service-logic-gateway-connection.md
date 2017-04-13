<properties
   pageTitle="Alkalmazások logika a helyszíni átjáró adatkapcsolat |} Microsoft Azure"
   description="Kapcsolat létrehozása az átjáró a helyszíni logika alkalmazásból olvashat."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="connect-to-the-on-premises-data-gateway-for-logic-apps"></a>Csatlakozás az átjáró a helyszíni logika alkalmazásai

Támogatott érték alkalmazások összekötők konfigurálása a kapcsolat a hozzáférést a helyszíni adatok keresztül az átjáró a helyszíni teszi lehetővé.  Az alábbi lépésekkel végigvezeti telepítése és konfigurálása a helyszíni adatok átjárót egy logikai alkalmazás használata.

## <a name="prerequisites"></a>Előfeltételek

* El kell használata a munkahelyi vagy iskolai e-mail címét a fiók (Azure Active Directory-alapú fiók) társíthatja az átjáró a helyszíni Azure-ban
    * Ha egy Microsoft Account használ (például @outlook.com, @live.com) az Azure-fiók használatával hozzon létre egy munkahelyi vagy iskolai e-mail címét [az alábbi lépéseket](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal) követve

> [AZURE.WARNING] Nincs korlátozás jelenleg, hogy a helyszíni átjáró telepítése csak befejeződik, ha van regisztrálva a Power BI-fiókkal.  Regisztráljon időközben minden fiók "A Power BI ingyenes" a telepítés elvégzéséhez.

* A helyszíni adatok átjáró [egy helyi számítógépre telepítve](app-service-logic-gateway-install.md)kell lennie.
* Átjáró kell nem rendelkezik kérte egy másik Azure a helyszíni adatok átjáró ([formál fordul elő, ha a következő lépés: 2 alatti létrehozása](#2-create-an-azure-on-premises-data-gateway-resource)) – telepítés átjárók erőforráshoz társított állhat.

## <a name="installing-and-configuring-the-connection"></a>Való telepítéséről és konfigurálásáról a kapcsolat

### <a name="1-install-the-on-premises-data-gateway"></a>1. a helyszíni adatok átjáró telepítése

Az átjáró a helyszíni telepítésével kapcsolatos információ [Ebben a cikkben](app-service-logic-gateway-install.md)található.  A további műveletek elvégzéséhez a folytatás előtt egy helyszíni gépen telepítenie kell az átjárót.

### <a name="2-create-an-azure-on-premises-data-gateway-resource"></a>2. a Azure a helyszíni adatok átjáró erőforrás létrehozása

A telepítés után az átjáró a helyszíni össze kell kapcsolnia az Azure előfizetés.

1. Jelentkezzen be az azonos munkahelyi vagy iskolai e-mail címét az átjáró telepítéskor használt használatával Azure
1. **Új** erőforrás gombra
1. Keresse meg és jelölje ki az **átjáró a helyszíni adatok**
1. Töltse ki az adatokat az átjáró társítani a - fiókjából, beleértve a ki a megfelelő **Telepítési neve**

    ![A helyszíni átjáró adatkapcsolat][1]
1. Kattintson a **Létrehozás** gombra az erőforrás létrehozása

### <a name="3-create-a-logic-app-connection-in-the-designer"></a>3. logika alkalmazás kapcsolat létrehozása a tervezőben

Most, hogy az Azure-előfizetése társítva az átjáró a helyszíni egy példányát, létrehozhat egy kapcsolatot vele az összefüggés-alkalmazásból.

1. Nyissa meg a logika alkalmazást, és válasszon egy összekötőre, amely támogatja a helyszíni kapcsolódási (kezdve az írás SQL Server)
1. Jelölje be az **átjáró a helyszíni adatok keresztül**

    ![Logika alkalmazás Designer átjáró létrehozása][2]
1. Jelölje ki az **átjáró** csatlakozni kíván, és töltse ki a többi kapcsolat adatait szükséges
1. Kattintson a **Create** a kapcsolat létrehozása

A kapcsolat kell most sikeresen konfigurálhatók a logika alkalmazásban használható.  

## <a name="next-steps"></a>Következő lépések
- [Gyakori példák és -esetek logika alkalmazásai](app-service-logic-examples-and-scenarios.md)
- [Nagyvállalati integrációs szolgáltatások](app-service-logic-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/app-service-logic-gateway-connection/createblade.PNG
[2]: ./media/app-service-logic-gateway-connection/blankconnection.PNG
[3]: ./media/app-service-logic-gateway-connection/checkbox.PNG