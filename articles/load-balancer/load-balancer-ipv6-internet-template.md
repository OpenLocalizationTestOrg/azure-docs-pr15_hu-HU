<properties
    pageTitle="Megoldást üzembe helyez az internetes terheléselosztás és az IPv6 sablon használatával |} Microsoft Azure"
    description="Az IPv6 támogatása az Azure betöltése terheléselosztó és terheléselosztás VMs telepítéséről."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="az IPv6, azure terheléselosztó, kettős Papírhalom, nyilvános ip, natív ipv6, Mobiltelefonról, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Megoldást üzembe helyez az internetes terheléselosztó és az IPv6 sablon használatával

> [AZURE.SELECTOR]
- [A PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Sablon](./load-balancer-ipv6-internet-template.md)

Az Azure terheléselosztó az egy réteg-4-es (TCP, UDP) terheléselosztó. A terheléselosztó magas elérhetősége biztosítja terjesztése bejövő forgalom megfelelő szolgáltatás példányainak cloud Services vagy a betöltés terheléselosztó meg virtuális gépeken futó között. Azure terheléselosztó is bemutathatja több portok vagy több IP-címek szolgáltatások.

## <a name="example-deployment-scenario"></a>Példa telepítéshez

Az alábbi ábra szemlélteti a terheléselosztási megoldást üzembe helyezéséhez a jelen cikkben ismertetett példa sablon segítségével.

![Betöltési terheléselosztó eset](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

Ebben az esetben a következő Azure erőforrások létrehoznia:

- minden egyes virtuális társított IPv4 és az IPv6-címek a virtuális hálózati felhasználói felülete
- az internetes terheléselosztó IPv4- és az IPv6 nyilvános IP-címet a
- két töltse be a nyilvános VIP hozzárendelése a magánjellegű végpontok terheléselosztó szabályok
- az elérhetőség beállítása, amely tartalmazza a két VMs
- két virtuális gépeken futó (VMs)

## <a name="deploying-the-template-using-the-azure-portal"></a>A sablon az Azure portálon üzembe helyezése

Ez a cikk az [Azure quickstart útmutató](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) sablongyűjteményben közzétett sablonra hivatkozik. Töltse le a sablont a gyűjteményből, vagy indítsa el a telepítést, közvetlenül a gyűjteményben az Azure-ban. Ez a cikk tartalma feltételezi, hogy a sablon a helyi számítógépre letöltött.

1. Nyissa meg az Azure portálját, és be kell jelentkeznie hozhat létre VMs és a hálózati erőforrások belül az Azure előfizetéssel engedélyekkel rendelkező fiókkal. Is kivéve, ha meglévő erőforrásokat használata esetén a a fiók erőforráscsoport és a tárterület-fiók létrehozása engedélyre van szüksége.

2. Kattintson a "+ új" a a menüre, majd írja be a keresőmezőbe a "sablon". A keresési eredmények közül válassza ki a "Sablon telepítésének".

    ![LB-ipv6-portálon-2.lépésben](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. A minden lap, kattintson a "Telepítés sablon."

    ![LB-ipv6-portálon-3. lépés](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Kattintson a "Létrehozása".

    ![LB-ipv6-portálon-step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Kattintson a "Sablon szerkesztése." Meglévő tartalmának törlése a sablonfájlt (a kezdete és vége {}) teljes tartalmának a másolás, beillesztés, majd kattintson a "Mentés."

    > [AZURE.NOTE] Ha Microsoft Internet Explorer használja, beillesztésekor megjelenik egy párbeszédpanel kéri, hogy engedélyezze a Windows vágólap elérését. Kattintson a "Hozzáférés engedélyezése."

    ![LB-ipv6-portálon-step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Kattintson a "Szerkesztése paramétereinek." A Paraméterek lap sablon paraméterek szakaszában adja meg az útmutató per értékeket, majd a "Mentés" gombra kattintva zárja be a Paraméterek lap. Az egyéni telepítés lap jelölje ki azt az előfizetést, meglévő erőforráscsoport, vagy hozzon létre egy újat. Ha egy erőforrás csoportot szeretne létrehozni, jelölje be az erőforráscsoport helyét. Ezután kattintson a **jogi feltételeket**, majd a jogi adatokra kattintson a **vásárlás** gombra. Azure megkezdi az erőforrások telepítésével. Néhány percig, amíg az összes erőforrás telepítéséhez szükséges időt.

    ![LB-ipv6-portálon-step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    További információt a következő paraméterek című [sablon paramétereket és a változók](#template-parameters-and-variables) a jelen cikk.

7. A sablon alapján létrehozott forrásokban talál, kattintson a Tallózás gombra, és görgessen lefelé a listában, amíg "Erőforrás csoport" megjelenítéséhez, majd kattintson rá.

    ![LB-ipv6-portálon-step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Kattintson az erőforrás a csoportok lap, a csoport nevét, az erőforrás 6 lépésben megadott. Megjelenik a bevezetett erőforrások listája. Az összes találja a hiba is, ha a "Utolsó telepítési." kell mondjuk "Sikerült" Ha nem, gondoskodjon arról, hogy a fiókot használ a szükséges erőforrások létrehozása engedélyeket.

    ![LB-ipv6-portálon-step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [AZURE.NOTE] Az erőforrás-csoportokat közvetlenül azután, hogy 6 lépés végrehajtásával Tallózás gombra, ha "Az utolsó telepítési" "Kialakítása" állapotának jelennek meg az erőforrások telepített billentyűt.

9. Kattintson az erőforrások listája "myIPv6PublicIP". Látni, hogy rendelkezik-e az IPv6-címek a IP-cím, és hogy a DNS-nevére a 6 dnsNameforIPv6LbIP paraméterhez megadott érték. Ez az erőforrás az a nyilvános IPv6 címét és a host név Internet-ügyfelek számára elérhető.

    ![LB-ipv6-portálon-step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Kapcsolat ellenőrzése

A sablon a sikeres elkezdte, ellenőrizheti kapcsolódási hajtsa végre az alábbi műveleteket:

1. Jelentkezzen be az Azure-portálra, és a VMs hozta létre a sablon telepítése kapcsolódni. Ha telepítette a Windows Server virtuális, futtassa az ipconfig/összes a parancssorból. Láthatja, hogy a VMs IPv4 és az IPv6-címei van-e. Ha Linux VMs telepítette, állítsa be a dinamikus IPv6-címei az utasításokat követve a Linux eloszlás fogadásához Linux-OS szeretne.
2. Az IPv6 internetkapcsolattal rendelkező ügyfélprogramból nyilvános IPv6-címe kapcsolat a terheléselosztó kezdeményezhet. Győződjön meg arról, hogy a terheléselosztó terheléselosztás-e a két VMs között, hogy az egyes a VMs webkiszolgálóra például a Microsoft Internet Information Services (IIS) telepítheti. Az alapértelmezett weblap-kiszolgálókon egyedileg azonosító "Server0" vagy "Kiszolgáló1" szöveget tartalmazó. Ezután nyissa meg az IPv6 internetkapcsolattal rendelkező ügyfélen böngészőt, és tallózással keresse meg a hostname (állomásnév), a végpontok közötti IPv6-kapcsolat a minden virtuális megerősítéséhez terheléselosztó dnsNameforIPv6LbIP paraméterhez megadott. Ha csak a weblap, csak egy kiszolgálóról látható, szükség lehet a böngésző gyorsítótárának kiürítése. Nyissa meg a több privát böngészési munkamenetet. Ekkor megjelennek az egyes server választ.
3. IPv4-internetkapcsolattal rendelkező ügyfélprogramból kezdeményezése nyilvános IPv4-címe kapcsolat a terheléselosztó. Erősítse meg, hogy a terheléselosztó terheléselosztási a két VMs, tesztelni sikerült, használja az IIS, a 2 részletes.
4. Az egyes a virtuális kezdeményezése egy kimenő kapcsolódási IPv6-ot vagy az interneten IPv4-kompatibilis eszközre. Mindkét esetben a forrás IP-Címének a cél eszköz által látott a terheléselosztó nyilvános IPv4- vagy IPv6-címét.

>[AZURE.NOTE]
IPv4 és az IPv6-ICMP le van tiltva a Azure hálózaton. Eredményt adja mint mindig ping ICMP eszközök sikertelen lesz. A kapcsolat teszteléséhez használja például TCPing vagy a próba-NetConnection PowerShell-parancsmag egy TCP alternatív. Ne feledje, hogy az IP-címek a diagramon látható értékek, előfordulhat, hogy példák. Mivel az IPv6-címei dinamikusan van hozzárendelve, a címeket kap függvényében eltérőek, és régió változhat. Azt is közös az a nyilvános IPv6 address a terheléselosztó, mint a háttéradatbázist készletben magánjellegű IPv6-címei különböző előtaggal kezdődik.

## <a name="template-parameters-and-variables"></a>Sablon paramétereket és a változók

Az erőforrás-kezelő Azure-sablon változói és a paraméterek, amelyet testre szabhat igény tartalmazza. Rögzített értékek, amelyek nem szeretné, hogy a felhasználó megváltoztathassa változók segítségével. Paraméterek segítségével értékeket, amelyet a felhasználónak meg kell adnia a sablon telepítésekor. A példa sablon van konfigurálva, a jelen cikkben ismertetett eset. A környezet igényeihez szabhatja.

A jelen cikkben használt példa sablont tartalmaz, a következő változók és a Paraméterek:

| Paraméter / változó | Jegyzetek |
|-----------|-------|
| adminUsername | Adja meg, jelentkezzen be a virtuális gépeken futó a rendszergazdai fiók nevére. |
| adminPassword | Adja meg, jelentkezzen be a virtuális gépeken futó a rendszergazdai fiók jelszava. |
| dnsNameforIPv4LbIP | Adja meg a DNS állomásnév szeretne hozzárendelni a terheléselosztó nyilvános nevet. Ez a név oldja fel a terheléselosztó nyilvános IPv4-címet. A név kisbetűre cseréli le kell és felel meg a regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP | Adja meg a DNS állomásnév szeretne hozzárendelni a terheléselosztó nyilvános nevet. Ez a név oldja fel a terheléselosztó nyilvános IPv6-címet. A név kisbetűre cseréli le kell és felel meg a regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. Ez lehet nevével megegyező IPv4-címét. Amikor egy ügyfél küld Azure ad vissza ilyen nevű DNS-lekérdezést az A és a AAAA rögzíti a Ha meg van-e osztva a nevét. |
| vmNamePrefix | Adja meg a virtuális neve előtagot. A sablon hozzáfűzi szám (0; 1, stb.) a VMs létrehozásakor nevére. |
| nicNamePrefix | Adja meg a hálózati kapcsolat neve előtagot. A sablon hozzáfűzi szám (0; 1, stb.) a hálózati kapcsolatok létrehozásakor nevére. |
| storageAccountName | Írja be a meglévő tároló fiók nevét, vagy adjon meg egy új hozza létre a sablon nevét. |
| availabilitySetName | Az elérhetőség használható együtt a VMs beállítása majd megadása |
| addressPrefix | A cím előtagját használta a virtuális hálózat címtartományokat |
| subnetName | Az alhálózathoz a nevét a VNet készült |
| subnetPrefix | A cím előtagját használta az alhálózathoz cím cellatartomány |
| vnetName | Adja meg a VNet a VMs által használt nevét. |
| ipv4PrivateIPAddressType | A magánjellegű IP-cím (statikus vagy dinamikus) módját terhelés |
| ipv6PrivateIPAddressType | A felosztás módszerét a magánjellegű IP-cím (dinamikus). Az IPv6 csak a dinamikus terhelés támogatja. |
| numberOfInstances | A sablon alapján rendszerbe terheléselosztása példányok száma |
| ipv4PublicIPAddressName | Adja meg, hogy a tartománynév használva kommunikálhat a terheléselosztó nyilvános IPv4-címét szeretne. |
| ipv4PublicIPAddressType | A nyilvános IP-cím (statikus vagy dinamikus) módját terhelés |
| Ipv6PublicIPAddressName | Adja meg, hogy a tartománynév használva kommunikálhat a terheléselosztó nyilvános IPv6-címét szeretne. |
| ipv6PublicIPAddressType | A felosztás módját nyilvános IP-címe (dinamikus). Az IPv6 csak a dinamikus terhelés támogatja. |
| lbName | Adja meg a terheléselosztó nevét. Ez a név jelenik meg a portálon vagy CLI vagy PowerShell paranccsal hivatkozó használt. |

A hátralévő változók a sablonban származtatott értéket, ha Azure hoz létre az erőforrásokhoz rendelt tartalmaz. Ezek a változók nem változik.
