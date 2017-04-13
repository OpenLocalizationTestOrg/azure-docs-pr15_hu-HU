<properties
   pageTitle="Erőforrás-kezelő sablonok VIEWBEN kód használata |} Microsoft Azure"
   description="Szemlélteti, hogyan állíthatja be a Visual Studio kód létrehozása az Azure erőforrás-kezelő sablonok."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="cmatskas"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="chmatsk;tomfitz"/>

# <a name="working-with-azure-resource-manager-templates-in-visual-studio-code"></a>Azure erőforrás-kezelő sablonok a Visual Studio kód használata

Azure erőforrás-kezelő sablonok, amelyek bemutatják, erőforrás és a kapcsolódó függőségek JSON-fájlokat. Ezek a fájlok esetenként is nagy és bonyolultan, ezért fontos támogatási szerszámok. Visual Studio Code egy új, könnyű, Megnyitás-forrást, platformok kód szerkesztése. Támogatja az erőforrás-kezelő sablonok között egy [Új bővítmény](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)készítése és módosítása. VIEWBEN kód mindenhol fut, és nem szükséges internetkapcsolat, kivéve, ha szeretné az erőforrás-kezelő sablonok telepítése.

Ha még nem rendelkezik VIEWBEN kód, a [https://code.visualstudio.com/](https://code.visualstudio.com/)telepítheti.

## <a name="install-the-resource-manager-extension"></a>Az erőforrás-kezelő bővítmény telepítése

A JSON-sablonok VIEWBEN kódban dolgozni, egy bővítmény telepítéséhez szükséges. Az alábbi lépésekkel töltse le és telepítse az erőforrás-kezelő JSON sablonok nyelvek támogatásának:

1. Indítsa el a VIEWBEN kódot. 
2. Gyors megnyitásához (Ctrl + P) 
3. A következő parancsot: 

        ext install azurerm-vscode-tools

4. Indítsa újra a VIEWBEN kódot, amikor a rendszer kéri a bővítmény engedélyezése. 

 A feladat kész!

## <a name="set-up-resource-manager-snippets"></a>Erőforrás-kezelő kódrészletek beállítása

Az előző lépéseket telepítve a szerszámok támogatás, de most VIEWBEN kódot, és használja a JSON-sablon kódrészletek konfigurálása szükség.

1. A fájl tartalmát a vágólapra másolja az [azure-xplat-arm-szerszámok](https://raw.githubusercontent.com/Azure/azure-xplat-arm-tooling/master/VSCode/armsnippets.json) tárházba.
2. Indítsa el a VIEWBEN kódot. 
3. A kód VIEWBEN, megnyithatja a JSON kódrészletek fájl navigálással vagy **fájl** -> **Beállítások** -> **Felhasználói kódrészletek** -> **JSON**, vagy ha kijelöli **az F1** , és írja be a **Beállítások** is jelölje ki **beállítások: kódrészletek**.

    ![preferencia-sorrend kódrészletek](./media/resource-manager-vs-code/preferences-snippets.png)

    A beállítások csoportban kattintson a **JSON**.

    ![Jelölje ki a json](./media/resource-manager-vs-code/select-json.png)

4. A fájlt a lépés: 1 a felhasználó kódrészletek fájlba az utolsó előtt tartalmának beillesztése "}" 
5. Ellenőrizze, hogy a JSON néz ki az OK gombra, és nincs történő bárhol is meg vannak. 
6. Mentse és zárja be a felhasználó kódrészletek fájlt.

Ez az összes van szükség az erőforrás-kezelő kódrészletek használatának megkezdéséhez. Ezután azt fog elhelyezni ezzel a beállítással a tesztet.

## <a name="work-with-template-in-vs-code"></a>Sablon VIEWBEN kód használata

A sablon használata indítása legegyszerűbben fogjon meg egy rövid indítása sablonokkal [Github](https://github.com/Azure/azure-quickstart-templates) , vagy egy saját használja. Könnyen [sablon exportálása](resource-manager-export-template.md) egy erőforrás csoportot a portálon keresztül. 

1. Ha erőforráscsoport exportált egy sablont, nyissa meg a kibontott fájlok VIEWBEN kódot.

    ![fájlok megjelenítése](./media/resource-manager-vs-code/show-files.png)

2. Nyissa meg a template.json fájlt, így Ön szerkesztheti és néhány további információforrásokat. Miután a **"erőforrások": [** nyomja le az enter új sort szeretne kezdeni. **Arm**írja be, ha a fogja bemutatni, és a beállítások. Ezek a lehetőségek állnak a sablon kódrészletek telepítette. Ez így néz ki: 

    ![kódrészletek megjelenítése](./media/resource-manager-vs-code/type-snippets.png)

3. Válassza ki a kívánt kódtöredékének. Ez a cikk lehet vagyok megválasztása a **arm-ip** hozhat létre egy új nyilvános IP-címet. Egy vesszőt a záró zárójel után szeretné elhelyezni "}" kattintva győződjön meg arról, hogy a sablon az újonnan létrehozott erőforrás szintaxisa érvényes.

     ![vesszővel hozzáadása](./media/resource-manager-vs-code/add-comma.png)

4. VIEWBEN kódot tartalmaz beépített IntelliSense. A sablonok szerkesztés során VIEWBEN kód felajánlja a rendelkezésre álló értékek. Változók szakasz hozzáadása a sablonhoz, például vegye fel a **""** (két dupla idézőjel), és válassza a **Ctrl + szóköz** e idézőjelek között. Választhat a lehetőségek **változók**például.

    ![változók hozzáadása](./media/resource-manager-vs-code/add-variables.png)

5. Az IntelliSense is is felajánlja az elérhető értékek vagy függvények. A tulajdonság beállítása a paraméter értéke, a **"[]"** és a **Ctrl + szóköz**kifejezés létrehozása. Írja be a nevét, a függvény elindíthatja. Amikor megtalálta a kívánt függvényre, válassza a **lapot** .

    ![paraméter felvétele](./media/resource-manager-vs-code/select-parameters.png)

6. **Ctrl + szóköz** parancsra a rendelkezésre álló paraméterek belül a sablon listájának megtekintéséhez a függvényen belül.

    ![paraméter felvétele](./media/resource-manager-vs-code/select-avail-parameters.png)

7. A sablon Ha séma ellenőrzési problémák megoldásához, látni fogja a a már jól ismert történő szerkesztőben. Írja be a **Ctrl + Shift + M** vagy a bal alsó állapotsorban a Karaktertábla útmutatója listája tekinthet meg.

    ![hibák](./media/resource-manager-vs-code/errors.png)

    A sablon érvényességi segítséget nyújtanak szintaxis problémák; észlelésére azonban figyelmen kívül hagyhatja hibákról is látható. Bizonyos esetekben a szerkesztő hasonlít a sablon egy, amely nem naprakész, és ezért jelentések hibát jelez, annak ellenére, hogy tudja, hogy helyes séma alapján. Tegyük fel például, a függvény a legutóbb hozzá lett adva erőforrás-kezelő, de nem lettek frissítve a sémában. A szerkesztő jelentések annak ellenére telepítése során megfelelően működik a függvény hibát jelez.

    ![hibaüzenet jelenik meg](./media/resource-manager-vs-code/unrecognized-function.png)

## <a name="deploy-your-new-resources"></a>Az új erőforrások terjesztése

Amikor készen áll a sablont, az új erőforrások, az alábbi útmutatást követve telepítheti: 

### <a name="windows"></a>A Windows

1. Nyisson meg egy PowerShell parancssor 
2. Bejelentkezés típusát: 

        Login-AzureRmAccount 

3. Ha több előfizetéssel rendelkezik, az előfizetések és listájának megjelenítése:

        Get-AzureRmSubscription

    És válassza az előfizetés használni.
   
        Select-AzureRmSubscription -SubscriptionId <Subscription Id>

4. A paraméterek parameters.json fájljait frissítése
5. Futtassa a Deploy.ps1 az Azure a sablon terjesztése

### <a name="osxlinux"></a>OSX vagy Linux rendszerhez

1. Terminálszolgáltatások ablak megnyitása 
2. Bejelentkezés típusát:

        azure login 

3. Ha több előfizetéssel rendelkezik, válassza ki a megfelelő előfizetéssel:

        azure account set <subscriptionNameOrId> 

4. Frissítse a paraméterek a parameters.json fájlt.
5. A sablon üzembe helyezéséhez futtatása:

        azure group deployment create -f <PathToTemplate> 

## <a name="next-steps"></a>Következő lépések

- Sablonok kapcsolatos további tudnivalókért lásd: [Azure erőforrás-kezelő szerzői sablonok](resource-group-authoring-templates.md).
- Sablon függvényekkel kapcsolatos további tudnivalókért lásd: [Azure erőforrás-kezelő sablon függvények](resource-group-template-functions.md).
- További példák a Visual Studio kód használata című [Build felhő Visual Studio kóddal](https://github.com/Microsoft/HealthClinic.biz/wiki/Build-cloud-apps-with-Visual-Studio-Code) a [bemutató](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 csatlakozás. További QuickStarts HealthClinic.biz bemutatója a csomagban a [Azure fejlesztői eszközökkel QuickStarts csomagban](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts)című témakör tartalmaz.
