<properties 
    pageTitle="Azure-alkalmazás visszaállítása" 
    description="További információ az alkalmazás visszaállítása biztonsági másolatból." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/06/2016" 
    ms.author="cephalin"/>

# <a name="restore-an-app-in-azure"></a>Azure-alkalmazás visszaállítása

Ez a cikk bemutatja, hogyan-alkalmazás [Azure alkalmazás szolgáltatás](../app-service/app-service-value-prop-what-is.md) , amely a korábban mentésben (lásd: [az alkalmazás az Azure feljebb](web-sites-backup.md)) visszaállítására. Az alkalmazás, a csatolt adatbázisokat (SQL-adatbázis vagy MySQL) igény szerinti az előző állapotának visszaállításához, vagy hozzon létre egy új alkalmazást, az eredeti alkalmazás biztonsági mentése alapján. A legújabb verzióra párhuzamosan futó új alkalmazás létrehozása akkor lehet hasznos, az A és B tesztelése.

Biztonsági mentés visszaállítása **normál** és **prémium** réteg futó alkalmazás érhető el. Tudni az alkalmazás mentése átméretezés olvassa el a [méretezni Azure-alkalmazás mentése](web-sites-scale.md)című témakört. **Prémium** réteg lehetővé teszi, hogy a napi biztonsági másolatok végre kell hajtani, mint **normál** réteg nagyobb számú.

<a name="PreviousBackup"></a>
## <a name="restore-an-app-from-an-existing-backup"></a>Az alkalmazás vissza egy meglévő biztonsági másolatból

1. Kattintson a **Beállítások** lap, az alkalmazás az Azure-portálon, a **biztonsági másolatok** a **biztonsági másolatok** lap megjelenítéséhez. Ezután kattintson **Visszaállítása gombra** a parancssávon. 
    
    ![Válassza a Visszaállítás gombra][ChooseRestoreNow]

3. A **Visszaállítás** lap jelölje be a biztonsági másolat forrás. 

    ![](./media/web-sites-restore/021ChooseSource.png)
    
    Az **alkalmazás biztonsági mentése** beállítást választja, megjelenik az aktuális alkalmazást a meglévő másolatait, és egyszerűen válasszon. 
    A **tároló** beállítással választhat a minden olyan biztonsági ZIP-fájl minden meglévő Azure tárterület-fiók és az előfizetés tároló. 
    Ha egy másik alkalmazás biztonsági másolatának visszaállítása próbálja, használja a **tárolási** lehetőség.

4. Ezután adja meg az alkalmazás visszaállítása a **cél visszaállítása**.

    ![](./media/web-sites-restore/022ChooseDestination.png)
    
    >[AZURE.WARNING] Ha úgy dönt, hogy a **felülírási**, az aktuális alkalmazás az összes meglévő adatok törlődnek. Kattintson az **OK gombra**, mielőtt győződjön meg arról, hogy az pontosan mit szeretne tenni.
    
    Választhatja a **Meglévő alkalmazást** az app biztonsági másolatának visszaállítása egy másik alkalmazásban az azonos resoure csoportba. Mielőtt használni ezt a lehetőséget, kell már létrehozott egy másik alkalmazásban az erőforráscsoport egy definiált az alkalmazás biztonsági mentése az adatbázis konfigurációs tükrözése a. 
    
5. Kattintson az **OK gombra**.

<a name="StorageAccount"></a>
## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Töltse le vagy a biztonsági törlése egy tárterület-fiókból
    
1. A fő lap **tallózással keresse meg** a Azure portál válassza a **tárterület-fiókok**.
    
    A meglévő tárolás partnerlista jelennek meg. 
    
2. Válassza a tárterület-fiókot, amely tartalmazza a biztonsági másolat letöltése vagy törölni kívánt.
    
    A tárterület-fiókom a lap jelenik meg.

3. A tároló accountn lap jelölje ki a kívánt tároló
    
    ![Tárolók megtekintése][ViewContainers]

4. Jelölje be a biztonsági másolatnak, töltse le vagy törölni szeretné.

    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)

5. Kattintson a **letöltése** és **törlése** , attól függően, hogy mit szeretne tenni.  

<a name="OperationLogs"></a>
## <a name="monitor-a-restore-operation"></a>A visszaállítási művelet figyelése
    
1. A sikeres vagy sikertelen az alkalmazás visszaállítási művelet adatainak megtekintéséhez nyissa meg a **Napló** lap az Azure-portálon. 
    
    A **naplókat** lap a műveleteket a szint, állapot, erőforrás és idő részletek együtt jeleníti meg.
    
2. Görgessen lefelé a kívánt művelet visszaállítása, és kattintással jelölje ki.

A Részletek lap a visszaállítási művelet a rendelkezésre álló információk jelennek meg.
    
## <a name="next-steps"></a>Következő lépések

Is biztonsági mentése és visszaállítása a REST API-t használ szolgáltatási alkalmazás alkalmazások (lásd: a [Biztonsági mentése és visszaállítása alkalmazás szolgáltatás alkalmazások használata többi](websites-csm-backup.md)).

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók kezdéshez Azure alkalmazás szolgáltatással, nyissa meg a [Próbálja alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban. Nem kötelező, hitelkártyák Nincs nyilatkozatát.


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
 
