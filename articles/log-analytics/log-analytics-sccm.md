<properties
    pageTitle="Csatlakozás Konfigurációkezelő napló Analytics |} Microsoft Azure"
    description="Ez a cikk bemutatja a lépéseket a napló Analytics Configuration Manager csatlakozás és adatok elemzése."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="banders"/>

# <a name="connect-configuration-manager-to-log-analytics"></a>Log Analytics Konfigurációkezelő csatlakoztatása

System Center Configuration Manager csatlakozhat a MOBILE napló Analytics Szinkronizáló eszköz webhelycsoport adatokat. Így a Configuration Manager üzembe adatainak MOBILE érhető el.

Számos való csatlakozáshoz szükséges Configuration Manager MOBILE, így egy rövid lefutása a teljes folyamat az alábbi lépéseket:

1. Az Azure adatkezelési portálon Configuration Manager regisztrálni webalkalmazás és/vagy a webes API alkalmazásként, és győződjön meg arról, hogy rendelkezik-e az ügyfél-azonosító és az ügyfél titkos kulcs az Azure Active Directoryból a regisztráció gombra. Lásd: [használata portál létrehozása az Active Directory-alkalmazás vagy szolgáltatás fő érheti el, amely erőforrásait](../resource-group-create-service-principal-portal.md) részletes információt elvégezheti az ezt a lépést.
2. Az Azure adatkezelési portálon [Configuration Manager (a regisztrált web app) biztosítson MOBILE eléréséhez szükséges engedély](#provide-configuration-manager-with-permissions-to-oms).
3. Az Konfigurációkezelőjében [kapcsolat létrehozása a MOBILE kapcsolat hozzáadása varázsló segítségével](#add-an-oms-connection-to-configuration-manager).
4. A Konfigurációkezelő is [frissíteni a kapcsolat tulajdonságai parancsra](#update-oms-connection-properties) , ha a jelszó és az ügyfél titkos kulcs bármikor lejár, vagy nem vesznek el.
5. A MOBILE portálról adatokkal, [Töltse le és telepítse a Microsoft figyelése Agent](#download-and-install-the-agent) a Configuration Manager szolgáltatás kapcsolat futtató számítógépen pont webhely rendszer szerepköre. A agent Configuration Manager adatokat küld a MOBILE.
6. A [webhelycsoportok Configuration Manager importálása](#import-collections) számítógép csoportok MOBILE.
7. A MOBILE, az adatok megtekintése Configuration Manager [számítógép csoportok](log-analytics-computer-groups.md)szerint

Erről további Configuration Manager kapcsolódás MOBILE [szinkronizálási](https://technet.microsoft.com/library/mt757374.aspx)adatait a Configuration Manager a műveleteket a Microsoft adatkezelési programcsomagban.



## <a name="provide-configuration-manager-with-permissions-to-oms"></a>Az engedélyek biztosítása az Configuration Manager MOBILE

Az alábbi eljárásból megismerheti azokat a Azure Kezelőportálja MOBILE hozzáférési jogosultsággal rendelkező. Kifejezetten meg kell a *munkatársi szerepkörök* felhasználóknak biztosítani kívánt az erőforráscsoport. Viszont, amely lehetővé teszi a Azure Kezelőportálja MOBILE Configuration Manager csatlakozni.

>[AZURE.NOTE] Meg kell adnia engedéllyel MOBILE Configuration Manager. Egyéb esetben kapni fog egy hibaüzenet Configuration Manager a konfigurálása varázsló használatakor.


1. Nyissa meg az [Azure-portálra](https://portal.azure.com/) , és kattintson a **Tallózás** > **Napló Analytics (MOBILE)** kattintva nyissa meg a napló Analytics (MOBILE) lap.  
2. Kattintson a **Log Analytics (MOBILE)** lap **hozzáadása** gombra a **MOBILE munkaterület** lap megnyitásához.  
  ![MOBILE lap](./media/log-analytics-sccm/sccm-azure01.png)
3. A **MOBILE munkaterület** lap a következő adatokat, és kattintson **az OK**gombra.
  - **Munkaterület MOBILE**
  - **Előfizetés**
  - **Erőforráscsoport**
  - **Hely**
  - **Réteg árak**  
    ![MOBILE lap](./media/log-analytics-sccm/sccm-azure02.png)  

    >[AZURE.NOTE] A fenti példa létrehoz egy új erőforráscsoport. Az erőforráscsoport Configuration Manager biztosítson ebben a példában a MOBILE munkaterületre vonatkozó engedélyt csak szolgál.

4. Kattintson a **Tallózás** > **erőforrás csoportok** , az **erőforrás csoportoknak** lap megnyitásához.
5. Az erőforráscsoport létrehozott feletti megnyitásához kattintson az **erőforrás csoportoknak** lap a &lt;erőforráscsoport neve&gt; beállítások lap.  
  ![erőforrás-csoport beállításai lap](./media/log-analytics-sccm/sccm-azure03.png)
6. Az a &lt;erőforráscsoport neve&gt; beállítások lap, kattintson a hozzáférés-vezérlés (IAM) kattintva nyissa meg a &lt;erőforráscsoport neve&gt; felhasználók lap.  
  ![erőforrás csoport a felhasználók lap](./media/log-analytics-sccm/sccm-azure04.png)  
7. Az a &lt;erőforráscsoport neve&gt; felhasználók lap, kattintson a **Hozzáadás gombra** kattintva nyissa meg az **access hozzáadása** lap.
8. Kattintson a **Hozzáadás az access** a lap kattintson a **Jelölje ki azt a szerepkört**, és válassza a vonatkozó **munkatársi** szerepkörök.  
  ![Jelölje ki azt a szerepkört](./media/log-analytics-sccm/sccm-azure05.png)  
9. Kattintson a **felhasználók hozzáadása**, jelölje ki a Configuration Manager felhasználót, kattintson a **Kijelölés**gombra, és kattintson **az OK**gombra.  
  ![felhasználók hozzáadása](./media/log-analytics-sccm/sccm-azure06.png)  


## <a name="add-an-oms-connection-to-configuration-manager"></a>Egy MOBILE kapcsolatot Configuration Manager hozzáadása

MOBILE-kapcsolat létesítése a Configuration Manager-környezet [szolgáltatás csatlakozási pont](https://technet.microsoft.com/library/mt627781.aspx) állítva az online módban kell rendelkeznie.

1. A **felügyeleti** munkaterületi Configuration Manager jelölje ki a **MOBILE összekötőt**. Ekkor megnyílik a **MOBILE kapcsolat hozzáadása varázslót**. Kattintson a **Tovább gombra**.

2. Az **Általános** képernyőn győződjön meg arról, hogy elvégezte a következő műveleteket, és a részleteket az egyes elemekre, majd kattintson a **Tovább gombra**.
  1. Az Azure-felügyeleti portálon a regisztráció, hogy rendelkezik-e [a regisztráció az ügyfél-azonosító](../active-directory/active-directory-integrating-applications.md), és Configuration Manager webalkalmazás és/vagy a webes API alkalmazásként.
  2. Az Azure adatkezelési portálon az alkalmazás titkos kulcs az Azure Active Directory regisztrált alkalmazáshoz létrehozott.  
  3. Az Azure-felügyeleti portálon a regisztrált web app által ellátni MOBILE eléréséhez szükséges engedély.  
  ![MOBILE varázsló az Általános lap kapcsolat](./media/log-analytics-sccm/sccm-console-general01.png)

3. **Azure Active Directory** képernyőn konfigurálja a csatlakozási beállításokat MOBILE, mert a **bérlői** , az **Ügyfél-azonosító** , és az **Ügyfél titkos kulcs** , majd kattintson a **Tovább gombra**.  
  ![Kapcsolat MOBILE varázsló Azure Active Directory-lapra](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)

4. Az egyéb eljárások sikeresen hajtható végre, ha majd az információ **MOBILE kapcsolatot a beállítások** képernyőn automatikusan megjelenik ezen a lapon. A kapcsolatbeállításokat az adatokat a **Azure előfizetés** , az **Azure erőforráscsoport** és a **Műveletek kapcsolatkezelés csomagja munkaterület**jelenjenek meg.  
  ![Kapcsolat MOBILE varázsló MOBILE kapcsolat lapra](./media/log-analytics-sccm/sccm-wizard-configure04.png)

5. A varázsló csatlakozik az információkat, amelyeket a szövegbeviteli MOBILE szolgáltatás. Jelölje ki az eszközt csoportokat használni kívánt MOBILE szinkronizálni, és kattintson a **Hozzáadás**gombra.  
  ![Jelölje ki a gyűjtemények](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)

6. Ellenőrizze az **összefoglaló** képernyőn a csatlakozási beállításokat, majd kattintson a **Tovább gombra**. A **haladás** képernyő állapota a csatlakozási, majd a **kész**kell.

>[AZURE.NOTE] A felső szintű webhely a hierarchia MOBILE kell csatlakozni. Ha MOBILE csatlakozás elsődleges különálló webhelyre, majd adjon hozzá egy központi felügyeleti webhelyhez a környezetben, törlése és újbóli létrehozása a MOBILE kapcsolatot az új hierarchián belül, be kell.

MOBILE Configuration Manager van csatolva, miután hozzáadása vagy eltávolítása a webhelycsoportok, és a MOBILE kapcsolat tulajdonságainak megtekintése.

## <a name="update-oms-connection-properties"></a>Frissítse a MOBILE kapcsolat tulajdonságai

Ha a jelszó és az ügyfél titkos kulcs bármikor lejár, vagy nem vesznek el, kell manuálisan frissíteni az a MOBILE kapcsolat tulajdonságai parancsra.

1. Konfigurációkezelő, nyissa meg azt a **Felhőszolgáltatások** , majd válassza a **MOBILE összekötő** , nyissa meg a **MOBILE kapcsolat tulajdonságai** lapot.
2. Ezen a lapon kattintson az **Azure Active Directory** lapján megtekintheti a **bérlői**és az **Ügyfél-azonosító**, **ügyfél titkos kulcs lejárati**. **Annak ellenőrzése** , ha már lejárt a **ügyfél titkos kulcs** .


## <a name="download-and-install-the-agent"></a>Töltse le és telepítse a agent

1. A MOBILE portálon, [Töltse le a MOBILE ügynök telepítőfájl](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. Telepítése és beállítása a agent a Configuration Manager szolgáltatás csatlakozási pontot webhely rendszer szerepkör futtató számítógépen használja az alábbi lehetőségek közül:
  - [Ügynököt a beállítás használatával](log-analytics-windows-agents.md#install-the-agent-using-setup)
  - [A parancssorból ügynököt](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
  - [Telepítse az Azure automatizálási DSC használata ügynök](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)


## <a name="import-collections"></a>Webhelycsoportok importálása

Miután hozzáadott egy MOBILE kapcsolatot Configuration Manager, és telepítve van a agent a Configuration Manager szolgáltatás kapcsolat futtató számítógépen, majd a webhely rendszer szerepkör pontra, a következő lépés a Configuration Manager a számítógép csoportként MOBILE gyűjtemények importálandó.

Miután importáló engedélyezve van, a webhelycsoport tagsági információk veszi 3 óránként naprakészen tartása a webhelycsoport tagságot. Megadhatja, hogy bármikor importáló letiltása.

1. A MOBILE portálon kattintson a **Beállítások**gombra.
2. Kattintson a **Számítógép csoportok** fülre, majd a a **SCCM** fülre.
3. Jelölje ki az **Importálás Configuration Manager webhelycsoport tagságot** , és kattintson a **Mentés**gombra.  
  ![Számítógép-csoportok - SCCM lap](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Configuration Manager adatainak megtekintése

Miután hozzáadott egy MOBILE kapcsolatot Configuration Manager, és telepítve van a agent a Configuration Manager szolgáltatás csatlakozási pontot webhely rendszer szerepkör futtató számítógépen, a agent adatainak MOBILE küldi. A MOBILE a Configuration Manager gyűjtemények [számítógép csoportok](log-analytics-computer-groups.md)jelennek meg. A **Beállítások**megtekintése a csoportok a **Számítógép csoportok** **Configuration Manager** lapról.

Miután importálta az csoportokat, megjelenik a webhelycsoport tagságot hány számítógépen észlelt. A webhelycsoportok importált számát is láthatja.

![Számítógép-csoportok - SCCM lap](./media/log-analytics-sccm/sccm-computer-groups02.png)

Amikor egy gombra kattint, keresés megnyílik, és megjeleníti az importált csoportokat mindegyike vagy minden egyes csoporthoz tartozó összes számítógépre történő. [Log](log-analytics-log-searches.md)keresőfunkcióval Configuration Manager adatok mélyreható elemzés elindíthatja.

## <a name="next-steps"></a>Következő lépések

- [Log](log-analytics-log-searches.md) kereséssel a Configuration Manager adatok részletes információinak megtekintése.
