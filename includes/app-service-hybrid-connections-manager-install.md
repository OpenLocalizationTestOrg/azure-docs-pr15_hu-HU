
1. A **hibrid kapcsolatok** lap kattintson az imént létrehozott hibrid kapcsolatra, majd kattintson a **Figyelő beállítása**.
    
    ![Kattintson a figyelő beállítása](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
    
4. A **hibrid kapcsolat tulajdonságai** lap megnyitása A **Helyszíni hibrid kapcsolatkezelő** **Töltse le és állítsa be a kézi**kiválasztása, mentse a letöltött HybridConnectionManager.msi csomagot és másolja a vágólapra az átjáró csatlakozási karakterlánc.
    
    ![Ide kattintva telepítése](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
    
5. A rendszergazdai parancssort írja be a telepítő indítsa el a következő parancsot:

        start HybridConnectionManager.msi
 
7. A telepítő futtatása után **nem most**gombra, majd keresse meg a %ProgramFiles%\Microsoft\HybridConnectionManager mappát, HCMConfigWizard.exe futtatása és kattintson az **Igen** gombra a **Felhasználói fiókok felügyelete** párbeszédpanel.
        
7. Illessze be a korábban kimásolt hibrid-kapcsolati karakterláncot, és kattintson az **OK gombra**. 
    
    ![Telepítése](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
    
8. A telepítés befejezése után kattintson a **Bezárás**gombra.
    
    ![Kattintson a Bezárás gombra](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
    
    A **hibrid kapcsolatok** lap **állapot** oszlopában most jeleníti **csatlakoztatva**. 
    
    ![Csatlakoztatott állapota](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)