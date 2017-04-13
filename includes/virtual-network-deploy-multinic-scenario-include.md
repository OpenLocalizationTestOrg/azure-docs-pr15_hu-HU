## <a name="scenario"></a>Eset

A telepítési használó több VMs az adott helyzetben azt ismerteti, ezt a dokumentumot. Ebben az esetben, ha egy kétszintű IaaS terhelést Azure-ban is. Minden réteg telepítve van a saját alhálózat virtuális hálózatban (VNet). Az előtér-réteg csoportosítva egy nagy elérhetőségének beállítása terheléselosztó több webkiszolgálón tevődik össze. A visszahívási vége réteg több adatbázis-kiszolgálók tevődik össze. Ezek az adatbázis-kiszolgálók két kártyát minden, egy adatbázis eléréséhez, a másik kezelése a fog telepíthető. Az alkalmazási példát is szabályozhatja, hogy milyen forgalom minden alhálózathoz hozhatnak hálózati biztonsági csoportok (NSGs) és a hálózati kártya a környezetben. Az alábbi ábrán ebben az esetben az egyszerű architektúrája.  

![MultiNIC eset](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

