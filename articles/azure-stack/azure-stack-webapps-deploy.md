<properties
    pageTitle="Web Apps erőforrás szolgáltató hozzáadása Azure jegyzettömbhöz |} Microsoft Azure"
    description="Részletes útmutatást Azure egymást fedő Web Apps alkalmazások telepítése"
    services="azure-stack"
    documentationCenter=""
    authors="ccompy, apwestgarth"
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

# <a name="add-a-web-apps-resource-provider-to-azure-stack"></a>Azure jegyzettömbhöz Web Apps alkalmazások erőforrás szolgáltató hozzáadása

> [AZURE.NOTE] Az alábbi információk csak az Azure Papírhalom TP1 telepítések vonatkozik.

A Web Apps erőforrás szolgáltató felvétele Azure Papírhalom hét lépéseket foglalja magában:

1.  Töltse le a szükséges összetevőket.
2.  Azure Papírhalom Webappok által használt tanúsítványok létrehozása.
3.  A telepítő segítségével letöltése, szakaszban és Azure Papírhalom Web Apps alkalmazások telepítése. 
4.  Érvényesítse a Web Apps telepítését.
5.  DNS-rekordok létrehozása az előtér és a Management Server terheléselosztókkal.
6.  Az imént telepített Web Apps alkalmazások erőforrás szolgáltató regisztrálása ARM.
7.  A Web Apps erőforrás szolgáltató tesztelése.

## <a name="download-required-components"></a>Töltse le a szükséges összetevők

1.  Töltse le az [Azure Papírhalom alkalmazás szolgáltatás előzetes telepítőt](http://aka.ms/azasinstaller). 
2.  Töltse le az [Azure Papírhalom alkalmazás szolgáltatás telepítési segítő parancsfájlok](http://aka.ms/azashelper). 
3.  A fájlok kinyerése a segítő parancsfájlok zip-fájl, három parancsfájlok kell:
    - Hozzon létre AppServiceCerts.ps1
    - Hozzon létre AppServiceDnsRecords.ps1
    - Register-AppServiceResourceProvider.ps1 

## <a name="create-certificates-to-be-used-by-azure-stack-web-apps"></a>Azure Papírhalom Webappok által használt tanúsítványok létrehozása

Ez a első parancsfájl működik-e a Web Apps alkalmazások által használt 3 tanúsítványok létrehozása az Azure Papírhalom hitelesítésszolgáltató. Futtassa a parancsfájlt a ClientVM, mint azurestack\administrator futtatja a PowerShell biztosítása:
1.  A **azurestack\administrator**futtatott PowerShell-munkamenetet hajtsa végre a **Létrehozás-AppServiceCerts.ps1** parancsfájl.  Ezzel három tanúsítványok, a parancsfájlt, esetleg szükséges Webappok által ugyanabban a mappában jön létre.
2.  Adja meg egy jelszót a pfx fájlok biztonságos, és jegyezze le azt, írja be az Azure Papírhalom Web Apps telepítőt kell.

## <a name="use-the-installer-to-download-and-install-azure-stack-web-apps"></a>Töltse le és telepítse az Azure Papírhalom Web Apps alkalmazások használata a telepítő

A appservice.exe telepítő lesz:
1.  Kérdezze meg a felhasználót, hogy a Microsoft és harmadik fél EULA elfogadásához.
2.  Azure Papírhalom telepítési vonatkozó információk gyűjtését.
3.  Hozzon létre egy blob-tárolóhoz megadott Azure Papírhalom tároló fiók.
4.  Töltse le a fájlokat, telepítse az Azure Papírhalom Web App erőforrás-szolgáltató szükséges.
5.  Felkészülés a Web App erőforrás-szolgáltató a Papírhalom Azure környezetben üzembe a telepítést.
6.  A fájlok feltöltése az alkalmazás szolgáltatásfiók tároló.
7.  A bemutató elindításához az erőforrás-kezelő Azure-sablon szükséges adatokat.

Az alábbi lépéseket a telepítés szakaszokra végigvezeti Önt:

>[AZURE.NOTE] A telepítő végrehajtásához jogú fiók (helyi vagy tartományi rendszergazdája) kell használnia. Ha azurestack\azurestackuser mint bejelentkezni a rendszer kéri, emelt hitelesítő adatokat. 

1.  **Azurestack\administrator**appservice.exe futnak. 
2.  Kattintson a **központi telepítés Azure erőforrás-kezelő használatával**.

![Azure Papírhalom szolgáltatás App Technical Preview 1 Installer][1]

3.  Olvassa el és fogadja el a Microsoft előzetes verziójú szoftverlicenc-szerződést, és kattintson a **Tovább gombra**.
4.  Olvassa el és fogadja el a harmadik partylicense szerződést, és kattintson a **Tovább gombra**.
5.  Tekintse át az alkalmazás Felhőszolgáltatásba konfigurációs információkat, és kattintson a **Tovább**gombra.

![Azure Papírhalom App Technical Preview 1 alkalmazás szolgáltatás felhő konfigurációjának][2]

6. Kattintson a **Csatlakozás** (az Azure Papírhalom előfizetések mező mellett).

![Azure Papírhalom alkalmazás szolgáltatás Technical Preview 1 alkalmazás szolgáltatás felhő a beállítások képernyőn két][3]

7.  Az Azure Papírhalom hitelesítési ablakban adja meg a **fiók Azure Active Directory szolgáltatás-rendszergazda** , és a **jelszavát**, és kattintson a **Sign In**.
**Megjegyzés:** Ez a az Azure Active Directory-fiókját, telepítette az Azure Papírhalom megadott.
    - Kattintson a **Lefelé mutató nyílra** a jelölőnégyzetét, **Azure Papírhalom előfizetések** jobb oldalán, és válassza az előfizetés.

![Azure Papírhalom alkalmazás szolgáltatás Technical Preview 1 előfizetés kiválasztása][5]

8.  Kattintson a **Lefelé mutató nyílra** a jelölőnégyzetét, **Azure Papírhalom helyek**jobb oldalán.
    - Válassza a **helyi**.
9. Adja meg a rendszergazda **nevét** .
10. Írja be a **jelszót** a rendszergazdák számára.
11. Tekintse át **az SQL Server részleteit** , és szükség esetén végezze el a módosításokat.
12. Tekintse át a **Rendszergazdák bejelentkezési fiók** , és szükség esetén végezze el a módosításokat.
13. Írja be a **Rendszergazdák jelszavát**.
14. Kattintson a **Tovább**gombra.  Ezen a ponton a telepítő most ellenőrzi a kapcsolat adatai az SQL Server megadva.

![Azure Papírhalom alkalmazás szolgáltatás Technical Preview 1 előfizetés kiválasztása][4]    

15. Kattintson a **Tallózás gombra** a **Web Apps alkalmazások alapértelmezett SSL Biztonságitanúsítvány-fájl** mellett, és keresse meg a **webalkalmazás. AzureStack.Local** [korábban létrehozott](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps)tanúsítvány.
16. Írja be a tanúsítványok létrehozásakor beállított **tanúsítvány jelszava** .
17. Kattintson a **Tallózás gombra** az **Erőforrás szolgáltató SSL Biztonságitanúsítvány-fájl** mellett, és keresse meg a **kezelése. AzureStack.Local** [korábban létrehozott](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps)tanúsítvány.
18. Írja be a tanúsítványok létrehozásakor beállított **tanúsítvány jelszava** .
19. Kattintson a **Tallózás gombra** az **Erőforrás szolgáltató legfelső szintű tanúsítvány fájl** mellett, és keresse meg a **kezelése. AzureStack.Local** [korábban létrehozott](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps)tanúsítvány.
20. Kattintson a **Tovább gombra** a telepítő ellenőrzi a tanúsítvány megadott jelszót.

![Azure Papírhalom alkalmazás szolgáltatás Technical Preview 1 tanúsítvány adatai][6]

A telepítési lép, hogy körülbelül 45-60 perces befejezéséhez.

![Azure Papírhalom alkalmazás szolgáltatás Technical Preview 1 telepítés állapota][7]

21. A telepítő sikeresen befejezése után kattintson a **Kilépés**gombra.

## <a name="validate-web-apps-installation"></a>Web Apps alkalmazások telepítési ellenőrzése

1.  Nyissa meg az **Azure Papírhalom Állomásgép** a **Hyper-V kezelője**.
2.  Keresse meg a **CN0-virtuális** és **Csatlakozás** a virtuális.
![Azure Papírhalom App Technical Preview 1 a Hyper-V szolgáltatáskezelő][8]
3.  A virtuális asztali kattintson duplán a **Webes felhő kezelőkonzol**.
![Azure Papírhalom szolgáltatás App Technical Preview 1 Management Console][9]
4.  Nyissa meg azt a **kezelt kiszolgálók**.
5.  Minden gép esetén **készen áll arra,** folytassa a következő lépéssel. 
![Azure Papírhalom App Technical Preview 1 kezelt kiszolgálók szolgáltatásállapot][10]

## <a name="create-dns-records-for-the-management-server-and-front-end-load-balancers"></a>A Management Server és a előtér terheléselosztókkal DNS-rekordok létrehozása
1.  Egy PowerShell-példány **azurestack\administrator**nyílnak meg.
2.  Nyissa meg azt a helyet, a parancsfájlok letöltése és a [kapcsolatban előzetesen szükséges lépés](#Download-Required-Components)kicsomagolása.
3.  Futtassa a **Létrehozás-AppServiceDnsRecords.ps1** , ezzel létrehozott DNS-bejegyzések ahhoz, hogy e-mailként első vége kiszolgálókon való portál és -web app kérések.  ARM a Web Apps, két szoftver terheléselosztókkal (SLBs), a telepítés során létrehozott a hálózati erőforrás szolgáltató. Az adatkezelési és az előtér-kiszolgálók mutatnak. A portál és az Azure Papírhalom alkalmazás erőforrás szolgáltatónak ARM-alapú kérelmek a Management Server megnyitásához.

## <a name="register-the-newly-deployed-azure-stack-web-apps-provider-with-arm"></a>ARM regisztrálja az imént telepített Azure Papírhalom Web Apps-szolgáltató
1.  Egy PowerShell-példány **azurestack\administrator**nyílnak meg.
2.  Nyissa meg azt a helyet, a parancsfájlok letöltése és a [kapcsolatban előzetesen szükséges lépés](#Download-Required-Components)kicsomagolása.
3.  Futtassa a **Register-AppServiceResourceProvider.ps1** . 

>[AZURE.NOTE] Írja be a felhasználónév és jelszó **pontosan (beleértve a felső és alsó eset)** , a megadott azt a **Virtuális gép(ek) rendszergazda** és a **jelszó** mezője a telepítés, vagy az erőforrás-szolgáltató regisztrációs sikertelen lesz.

## <a name="test-drive-azure-stack-web-apps"></a>Teszt meghajtó Azure Papírhalom Web Apps alkalmazások

Most, hogy telepítve, és a Web Apps alkalmazások erőforrás szolgáltató regisztrált, ellenőrizheti, hogy győződjön meg arról, hogy a bérlők telepítheti a web Apps alkalmazások.

1.  Az Azure Papírhalom-portálon kattintson az új gombra, válassza a webhely + Mobile, és kattintson a Web App.
2.  A Web App alkalmazásban a lap írja be egy nevet a Web app mezőbe.
3.  Erőforrás csoportban kattintson az új gombra, és írjon be egy nevet a erőforráscsoport mezőbe. 
4.  Kattintson az alkalmazás terv/SRV, és új létrehozása gombjára.
5.  Az alkalmazás szolgáltatás a terv lap írja be egy nevet az alkalmazás szolgáltatás terv mezőbe.
6.  Kattintson a árak réteg, kattintson a megosztott ingyenes vagy a megosztott megosztott, kattintson a kijelölés kattintson az OK gombra, és kattintson a Létrehozás gombra.
7.  A percet, a mozaik az új webalkalmazás fog megjelenni az irányítópulton. Kattintson a csempére.
8.  A web app lap, a Tallózás gombra kattintva megnézheti az alkalmazás az alapértelmezett webhelyéhez.


**(Nem kötelező) WordPress, DNN vagy Django webhely terjesztése**

1. Az Azure Papírhalom-portálon kattintson a "+", lépjen az Azure Piactérhez üzembe Django webhelye, és várja meg a művelet sikeresen befejeződött. A Django webes platform fájl rendszer-alapú adatbázis használ, és nem csak olyan további erőforrás szolgáltatót, például a MySQL- vagy SQL.  

2. Ha a MySQL-erőforrás szolgáltató is telepítette, a piactérről WordPress webhelye telepítheti. Az adatbázis-paraméterek rákérdez, amikor a szövegbeviteli a felhasználó nevét, mint *User1@Server1* (a felhasználónév és a lehetőség a kiszolgáló neve).

3. Ha is telepíti az SQL Server erőforrás szolgáltató, telepítheti egy DNN webhely a piactérről. Amikor adatbázis paraméterekkel rákérdez, válasszon egy adatbázist, amelyhez csatlakozik az erőforrás-szolgáltató SQL Servert futtató számítógép.

## <a name="next-steps"></a>Következő lépések

Más [platform szolgáltatást (PaaS) szerint](azure-stack-tools-paas-services.md), például az [erőforrás-szolgáltató SQL Server](azure-stack-sql-rp-deploy-short.md) és a [MySQL-erőforrás szolgáltató](azure-stack-mysql-rp-deploy-short.md)is próbálkozhat.

<!--Image references-->
[1]: ./media/azure-stack-webapps-deploy/AppService_exe_Start.png
[2]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep1.png
[3]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2.png
[4]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_populated.png
[5]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_SubscriptionSelection.png
[6]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep3_Certificates.png
[7]: ./media/azure-stack-webapps-deploy/AppService_exe_InstallationProgress.png
[8]: ./media/azure-stack-webapps-deploy/HyperV.png
[9]: ./media/azure-stack-webapps-deploy/MMC.png
[10]: ./media/azure-stack-webapps-deploy/ManagedServers.png


<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
