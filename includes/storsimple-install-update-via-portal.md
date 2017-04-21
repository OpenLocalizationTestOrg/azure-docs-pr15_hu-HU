<!--author=SharS last changed: 01/15/2016-->

#### <a name="to-install-update-12-from-the-azure-classic-portal"></a>A frissítés 1.2-es telepítse az Azure klasszikus portálról

1. A szolgáltatás StorSimple lapon kattintson az eszközére. Nyissa meg az **eszközök** > **karbantartási**.

2. A lap alján kattintson a **Frissítések beolvasása**gombra. A feladat hoz létre, hogy az elérhető frissítések keresése hivatkozásra. Kap értesítést jelenít meg, amikor a feladat sikeresen befejeződött.

3. **Szoftverfrissítések** szakaszában ugyanazon az oldalon jelenik meg, hogy új szoftverfrissítések állnak rendelkezésre. Azt javasoljuk, olvassa el a kibocsátási megjegyzések az eszközön 1.2-es frissítés alkalmazása előtt.

    ![Szoftverfrissítések telepítése](./media/storsimple-install-update-via-portal/InstallUpdate12_11M.png)

4. A lap alján kattintson a **Frissítések telepítése**gombra.

5. Megerősítését kéri. Kattintson az **OK gombra**.

6. A **Frissítések telepítése** párbeszédpanel formában jelennek meg. Az eszköz kell felelnie szerepel az ezen a párbeszédpanelen. A frissítés előtt volt elvégezni ezeket a lépéseket. Jelölje ki **lehet megérteni a fenti követelmény, és készen állok a frissítse az eszközt**. Kattintson a jelölőnégyzet ikonra.

    ![Megerősítést kérő üzenet](./media/storsimple-install-update-via-portal/InstallUpdate12_2M.png)

7. Egy sor olyan automatikus előtti ellenőrzések most elindítja. Ezek a következők:

    - A **vezérlő állapot ellenőrzi,** győződjön meg róla, hogy mindkét eszköz vezérlők megfelelő és online.
    
    - A **hardver összetevő állapota ellenőrzi,** győződjön meg róla, hogy az StorSimple eszközön hardver-összetevő megfelelő.
    
    - **Adatok 0 ellenőrzi** , hogy a ellenőrizze, hogy az adatok 0 engedélyezve van-e az eszközön. Ha a kapcsolat nincs engedélyezve, szüksége lesz kapcsolhatja be, majd próbálkozzon újra.
    
    - Az **adatok 2 és 3 adatok vizsgálata** ellenőrizze, hogy az adatok 2 és 3 adatok hálózati kapcsolatok nem engedélyezettek. Ha ezek felületek engedélyezve vannak, majd szüksége lesz le őket, és próbálja meg frissíteni az eszközt. Ez az ellenőrzés történik, csak akkor, ha olyan eszközről, ám szoftvert futtat frissít. Ez az ellenőrzés nem szükséges verziók 0,1 értéket, 0,2 vagy 0,3 rendszerű eszközökön.
    
    - Az **átjáró ellenőrzése** bármilyen eszközről, 1 frissítés előtti verzióját futtató. Ez az ellenőrzés megtörténik a frissítés előtti 1 szoftver fut az összes eszközön, de nem sikerül az eszközök, amelyek az átjárók be van állítva a hálózati kapcsolat eltérő adatok 0.
 
    Frissítés 1.2-es csak lesz érvényes, ha a fenti frissítés előtti ellenőrzések sikeresen. Ön értesítést kap, hogy vannak-e a frissítés előtti ellenőrzések folyamatban.
  
    ![Értesítés előre ellenőrzése](./media/storsimple-install-update-via-portal/InstallUpdate12_3M.png)

    Az alábbi képen a frissítés előtti ellenőrzés sikertelen volt. Meg kell győződjön meg róla, hogy mindkét eszköz vezérlők megfelelő és online. Akkor is kell a hardver-összetevő állapotának ellenőrzése. Ebben a példában a vezérlő 0 és 1 vezérlő összetevők szükséges figyelmet. Ha saját maga által meg nem cím ezeket a problémákat, lépjen kapcsolatba a Microsoft Support szeretne.

     ![Nem sikerült előtti ellenőrzése](./media/storsimple-install-update-via-portal/HCS_PreUpgradeChecksFailed-include.png)

    > [AZURE.NOTE] Frissítés 1.2-es StorSimple eszközén alkalmazása után a adatok 2 és 3 adatok ellenőrzése és az átjáró jelölőnégyzet már nem szükséges a jövőbeli frissítéseit. Az egyéb előtti ellenőrzések továbbra is szükséges.


8. A frissítés előtti ellenőrzések sikeres befejezését követően egy frissítési feladat jön létre. Kap értesítést jelenít meg, amikor a frissítés feladat sikeresen jön létre.
 
    ![Feladat létrehozása frissítése](./media/storsimple-install-update-via-portal/InstallUpdate12_44M.png)

    A frissítés majd az eszközön fog vonatkozni.
 
9. A frissítés feladat állapotának nyomon követéséhez, kattintson a **Feladat megtekintése**gombra. A **feladatok** lapon megjelenik a frissítés előrehaladását. 

    ![Projekt előrehaladásának frissítése](./media/storsimple-install-update-via-portal/InstallUpdate12_5M.png)

10. A frissítés elvégzéséhez néhány órát felvételére. A részletek, a feladat bármikor tekinthet meg.

    ![Feladat részletei frissítése](./media/storsimple-install-update-via-portal/InstallUpdate12_6M.png)

11. A feladat befejeződése után nyissa meg a **Karbantartás** lapot, és görgessen le a **Szoftverfrissítések**.

12. Győződjön meg arról, hogy eszköze problémamentesen működik **StorSimple 8000 sorozat frissítés 1.2 (6.3.9600.17584)**. **Utolsó frissítés dátum** módosítani kell a is.

    ![Karbantartása lap](./media/storsimple-install-update-via-portal/InstallUpdate12_10M.png)

13. Most láthatja, hogy a karbantartás mód frissítések is elérhetők. A frissítések eszköz legrövidebb leállás eredményez, és csak az eszközön a Windows PowerShell felületén keresztül vonatkozni zavaró frissítések is. Kövesse a [Karbantartás mód frissítések telepítése](storsimple-update-device.md#install-maintenance-mode-updates-via-windows-powershell-for-storsimple) a Windows PowerShell-e a frissítések telepítéséhez StorSimple.

> [AZURE.NOTE] Bizonyos esetekben a karbantartás mód frissítések jelző üzenet jelenik meg felfelé 24 óra után a karbantartás mód frissítéseket sikeresen alkalmazza az eszközön.  


