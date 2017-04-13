<properties
    pageTitle="Az Azure eszközkészlet Holdas telepítése |} Microsoft Azure"
    description="Megtudhatja, hogyan telepítheti az Azure eszközkészlete Holdas."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

# <a name="installing-the-azure-toolkit-for-eclipse"></a>Az Azure eszközkészlet Holdas telepítése

Az Azure eszközkészlete Holdas biztosít a sablonok és funkciókat, amelyek lehetővé teszik, hogy könnyen létrehozása, kialakítása és tesztelése és helyezhetnek üzembe a Holdas fejlesztői környezet segítségével Azure alkalmazásokat. Az Azure eszközkészlete Holdas Megnyitás projektben, amelynek forráskód részére elérhető a a project-webhely a következő URL-címen GitHub a MIT-licenc:

<https://github.com/Microsoft/Azure-Tools-for-Java>

Az alábbi lépéseket megtudhatja, hogyan telepítheti az Azure eszközkészlete Holdas.

[AZURE.INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>Az Azure eszközkészlete Holdas telepítése

1. Indítsa el a Holdas.

1. Amikor megnyílik a Holdas, kattintson a **Súgó** menü, és kattintson az **Új szoftverek telepítése**, az alábbi ábrán látható módon.

    ![Az Azure eszközkészlet Holdas telepítése][01]

1. Írja be a **Rendelkezésre álló szoftvert** párbeszédpanel **ismerni a** szövegdobozon belüli **http://dl.microsoft.com/eclipse** , majd az **Enter** billentyűt.

1. **Név** mezőre jelölje be az **Azure eszközkészlete Holdas**, és törölje a jelet **az összes frissítése során webhelyek partner telepítése szükséges szoftvert**. A képernyő jelenjen meg az alábbihoz hasonló:

    ![Az Azure eszközkészlet Holdas telepítése][02]

1. Ha kibontja az **Azure eszközkészlete Holdas**, az alábbi elemek jelennek meg:

    * **Java alkalmazás háttérismeretek beépülő modul**: az összetevő Azure a telemetriai naplózás és elemzése az alkalmazások és-kiszolgálói példány szolgáltatások használatát teszi.
    * **Azure Access vezérlő szolgáltatások szűrő**: Ez az összetevő támogatja az Azure ACS alkalmazás használatához, az egyszeri bejelentkezési forgatókönyvek engedélyezése és externalizing identitás logika az alkalmazásból.
    * **Azure közös beépülő modul**: Ez az összetevő biztosít a leggyakoribb funkciókat, szükség szerint más eszközkészlet összetevők.
    * **Azure Explorer Holdas**: Ez az összetevő biztosít a leggyakoribb funkciókat, szükség szerint más eszközkészlet összetevők.
    * **Java Holdas Azure beépülő modul**: az összetevő fejlesztésével projektek, amelyek segítségével összeállítása, tesztelése és helyezhetnek üzembe a Microsoft Azure felhő Holdas, és a parancssorból Java-alkalmazásokat támogatja.
    * **Azure Web Apps alkalmazások beépülő modul Java**: Ez az összetevő támogatja az üzembe helyezése a Microsoft Azure Web App tárolók Java webalkalmazások.
    * **A Microsoft JDBC illesztőprogram 4.2 az SQL Server**: Ez az összetevő biztosítja JDBC API az SQL Server és a Microsoft Azure SQL-adatbázis Java Platform vállalati Edition 8.
    * **Apache Qpid ügyfél-tárak JMS csomag**: Ez az összetevő biztosítja a JMS ügyfél összetevő ahhoz, hogy az alkalmazás használata a Microsoft Azure üzenetkezelés AMQP Apache Qpid projektből.
    * **Microsoft Azure-tárak Java csomag**: Ez az összetevő biztosítja API-khoz eléréséhez a Microsoft Azure-szolgáltatásokkal, például a tárhely, service bus, szolgáltatás futtatókörnyezet és stb.

1. Kattintson a **Tovább**gombra. (Ha az eszközkészlet telepítésekor szokatlan késések tapasztal, győződjön meg arról, hogy **az összes frissítése során webhelyek partner telepítéséhez szükséges szoftverek keresése** nincs bejelölve.)

1. A **Részletek telepítése** párbeszédpanelen kattintson a **Tovább**gombra.

    ![Tekintse át a telepítési információk][03]

1. **Tekintse át a licencek** párbeszédpanelen olvassa el a licencszerződést feltételeit. Ha fogadja el a licencszerződést feltételeit, kattintson **az elfogadom a licencszerződést** , és kattintson a **Befejezés gombra**. (A hátralévő lépések feltételezik, fogadja el a licencszerződést feltételeit. Ha nem fogadja el a licencszerződést feltételeit, lépjen ki a telepítési folyamatot.)

    ![Tekintse át a licencek][04]

    Holdas letölti és telepíti a kapcsolódó csomagok.

    ![Telepítés állapota][05]

1. Indítsa újra a telepítés befejezéséhez Holdas kéri, kattintson az **Igen**gombra.

    ![Indítsa újra a figyelmeztető üzenet][06]

## <a name="see-also"></a>Lásd még:

Java IDEs az Azure eszközök gazdag kapcsolatban további információt az alábbi helyeken találhat:

- [Azure eszközkészlete Holdas]
  - *Az Azure eszközkészlet telepítésével Holdas (Ez a cikk)*
  - [Helló világ webalkalmazást Holdas az Azure létrehozása]
  - [Az Azure eszközkészlete Holdas újdonságai]
- [Azure eszközkészlete IntelliJ]
  - [Az Azure eszközkészlet IntelliJ telepítése]
  - [Helló világ webalkalmazást IntelliJ az Azure létrehozása]
  - [Az Azure eszközkészlete IntelliJ újdonságai]

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center]című témakört.

<!-- URL List -->

[Azure eszközkészlete Holdas]: ./azure-toolkit-for-eclipse.md
[Azure eszközkészlete IntelliJ]: ./azure-toolkit-for-intellij.md
[Helló világ webalkalmazást Holdas az Azure létrehozása]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Helló világ webalkalmazást IntelliJ az Azure létrehozása]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Az Azure eszközkészlet IntelliJ telepítése]: ./azure-toolkit-for-intellij-installation.md
[Az Azure eszközkészlete Holdas újdonságai]: ./azure-toolkit-for-eclipse-whats-new.md
[Az Azure eszközkészlete IntelliJ újdonságai]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

