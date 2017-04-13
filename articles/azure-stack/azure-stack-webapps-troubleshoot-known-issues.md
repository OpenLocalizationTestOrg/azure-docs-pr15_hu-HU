<properties
    pageTitle="Azure Papírhalom – ismert problémák és hibaelhárítási alkalmazások webes |} Microsoft Azure"
    description="Részletes útmutatást Azure egymást fedő Web Apps alkalmazások telepítése"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="web-apps-resource-provider---known-issues-and-troubleshooting"></a>Web Apps erőforrás szolgáltató – ismert problémák és hibaelhárítás

> [AZURE.NOTE] Az alábbi információk csak az Azure Papírhalom TP1 telepítések vonatkozik.

Ha problémákat tapasztal az üzembe helyezése vagy a Azure Papírhalom Web Apps használata közben olvassa el az alábbi útmutatást.

## <a name="azure-stack-web-apps-not-available-in-the-portal"></a>Azure Papírhalom Web Apps alkalmazások nem érhető el a portálon

![Azure Papírhalom Web Apps alkalmazások létrehozása új Web App alkalmazásban][1]

Miután befejezte a [regisztrációs az Azure Papírhalom Web Apps-szolgáltató](azure-stack-webapps-deploy.md#register-the-newly-deployed-azure-stack-web-apps-provider-with-arm) , ha nem találja a webes + mobil lap, majd próbálkozzon a következőkkel:
* **A portál jelentkezzen ki** , és **Zárja be a böngészőt** , és kattintson a egy **új böngésző ablakának jelentkezzen be a portáljára** , és próbálkozzon újra.

Ha még nem látható a webhely és a mobil lap, próbálkozzon a következőkkel:

1.  Keresse meg a **Hyper-V kezelője** **Azure Papírhalom Állomásgép** **xRPVM** és **Csatlakozás** a virtuális.
2.  Nyissa meg egy **parancssort rendszergazdaként** , és **IISRESET** futtatása
3.  Térjen vissza az **ClientVM** , és nyisson meg egy **új böngészőablakban**, **Jelentkezzen be a portáljára** , és próbálkozzon újra.

## <a name="deployment-fails-when-creating-a-web-app-in-a-new-resource-group"></a>Telepítési sikertelen, amikor az új erőforráscsoport a webalkalmazás létrehozása

Az első előzetes verzió a Web Apps alkalmazások, az **Összes Web Apps alkalmazások** kell létrehozni az azonos erőforráscsoport szolgáltatóként Web Apps erőforrás (**Helyi AppService**).  Ismert probléma, és fog oldható meg a jövőbeli előnézetét.

## <a name="other-issues"></a>Egyéb kérdések

Ha más Azure Papírhalom a Web Apps kapcsolatos problémák kérjük bejegyzése [az Azure Papírhalom fórum] (https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) hol csoportunk tagjai bejegyzések figyelése.


<!--Image references-->
[1]: ./media/azure-stack-webapps-troubleshoot-known-issues/NewWebandMobile.png



<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
