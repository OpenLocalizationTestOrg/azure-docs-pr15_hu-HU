1. A portálon nyissa meg az **Új**. Írja be a "Virtuális hálózati átjáró" szót a keresés. Keresse meg a **virtuális hálózati átjáró** a Keresés a feladó, és kattintson a bejegyzésre. Ekkor megnyílik a **Létrehozás virtuális hálózati átjáró** lap.
2. Kattintson a **Create** a **virtuális hálózati átjáró** a lap alján. A **Létrehozás virtuális hálózati átjáró** lap nyílik meg. Töltse ki a virtuális hálózati átjáró értékeket.

    ![Létrehozás virtuális hálózati átjárók lap mezők] (./media/vpn-gateway-add-gw-rm-portal-include/createvnetgw300.png "Létrehozás virtuális hálózati átjárók lap mezők")

3. **Név**: az átjáró neve. Ez nem ugyanaz, mint egy átjáró alhálózat elnevezési. Érdemes hoz létre az átjáró-objektum nevét.

4. **Átjáró típusa**: jelölje be a **VPN Használatát**. Virtuális Magánhálózati átjárók használja a virtuális hálózati átjáró **VPN**. 

5. **VPN típusa**: válassza ki a virtuális Magánhálózati típusát a konfigurációban megadott. A legtöbb konfigurációk szükség egy útvonal alapú VPN-típust.

6. **Termékváltozat**: jelölje be az átjáró Termékváltozat a legördülő listából. A legördülő menü szerepel a termékváltozatok VPN típustól függ.

7. **Hely**: módosítsa a **hely** mezőben azt a helyet, ahol a virtuális hálózat található mutassanak.
 
8. Válassza ki a virtuális hálózat, amelyhez szeretne ezt az átjárót. Kattintson a **virtuális hálózati** a **Válassza ki a virtuális hálózati** lap megnyitásához. Jelölje ki a VNet. Ha nem látja a VNet, ellenőrizze, hogy a **hely** mezőben a virtuális hálózat a régió mutat.

9. Válasszon egy nyilvános IP-címet. Kattintson a **nyilvános IP-cím** a **választható nyilvános IP-címét** a lap megnyitásához. Válassza a **+ Új** **létrehozása a nyilvános IP-cím lap**megnyitásához. Adja meg a nyilvános IP-cím nevét. Ez a lap egy nyilvános IP cím objektumra, amelyhez egy nyilvános IP-cím dinamikusan osztják hoz létre.<br>Kattintson az **OK gombra** a módosítások mentéséhez, hogy ez a lap.

10. **Előfizetés**: Ellenőrizze, hogy be van jelölve a megfelelő előfizetést.

11. **Erőforráscsoport**: Ez a beállítás határozza meg, hogy ki a virtuális hálózati. 

12. Nem állítsa be a **hely** után a korábbi beállításokhoz korábban megadott.

13. Kérdezze meg a beállításokat. Választhat a **PIN-kód irányítópult** a lap alján, ha azt szeretné, hogy az átjáró jelenjen meg az irányítópulton.

14. Kattintson a **Létrehozás** az átjáró létrehozása a kezdéshez. A beállítások érvényesíteni kell, és megjelenik a "üzembe helyezés virtuális hálózati átjáró" csempe az irányítópulton. Az átjárók létrehozása 45 percet is igénybe vehet. Előfordulhat, hogy a portáloldalon a kész állapotának megtekintéséhez frissítéséhez.

    ![Virtuális üzembe helyezése hálózati átjáró] (./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png "Virtuális üzembe helyezése hálózati átjáró")

11. Az átjáró létrehozása után megtekintése a portálon a virtuális hálózat megjeleníti a rá kiosztott IP-címét. Az átjáró megjelennek egy csatlakoztatott eszközt. További információk megjelenítéséhez a csatlakoztatott eszköz (a virtuális hálózati átjáró) kattinthat.



