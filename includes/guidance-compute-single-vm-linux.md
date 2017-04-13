Ez a cikk a Azure-figyelmet méretezhetőség, elérhetőségét, kezelhetőség és biztonsági ismertet a Linux virtuális gépet (virtuális) futtató igazolt tanácsok csoportja. Azure támogatja a különböző népszerű Linux forgalom termékkategóriákban, beleértve a CentOS, Debian, vörös kalap vállalati, Ubuntu és FreeBSD futtatása. További tudnivalókért lásd: [Azure és Linux][azure-linux].

> [AZURE.NOTE] Azure van két különböző telepítési modellek: [Erőforrás-kezelő] [ resource-manager-overview] és klasszikus. Ez a cikk az erőforrás-kezelő Microsoft javasolja új telepítési használja.

Nem javasoljuk egy egyetlen virtuális használatának gyártási munkaterhelésekből, mert nincs felfelé idejű szolgáltatásiszint-szerződés (SLA) egyetlen VMs Azure a. Úgy juthat az SLA, telepítenie kell az [elérhetőség beállítása]több VMs[availability-set]. További tudnivalókért lásd: a [több VMs Azure a futó][multi-vm]. 

## <a name="architecture-diagram"></a>Architektúra diagramja

Csak a maga virtuális-nél több mozgó részek Azure-ban egy virtuális kiépítési magába foglalja. Nincsenek számítási, a hálózathasználatra és a tárhely olyan elemeket, amelyek a kell vennie.

> A Visio-dokumentum, amely tartalmazza a architektúra diagram letölthető a [Microsoft letöltőközpontból][visio-download]. Ez az ábra be van kapcsolva a "számítási - egyetlen virtuális" lapot.

![[0]][0]

- **Erőforráscsoport.** [_Erőforráscsoport_] [ resource-manager-overview] van olyan tároló, amely kapcsolódó erőforrásokat. Az erőforrások tartása a virtuális erőforráscsoport létrehozása.

- **VM**. A közzétett képeket listájából, vagy töltse fel a Azure blob-tárolóhoz (virtuális) virtuális merevlemez-fájlból egy virtuális is kiépítése.

- **Operációs rendszer lemez.** Operációs rendszer lemez egy virtuális [Azure-tárolóban]lévő[azure-storage]. Ez azt jelenti, hogy továbbra is fennáll, akkor is, ha megszakad az állomásgép. Az operációs rendszer lemez `/dev/sda1`.

- **Ideiglenes lemez.** A virtuális ideiglenes lemezen jön létre. Ez a lemez tárolja az állomásgép fizikai meghajtót. Még _nem_ Azure tárolási mentve, és előfordulhat, hogy válassza a nem vagyok a gépnél újraindítása és más virtuális életciklus-események során. Ez a lemez csak ideiglenes adatokat, például az oldal vagy felcserélése fájlok használja. Az ideiglenes lemez `/dev/sdb1` és csatlakoztatva `/mnt/resource` vagy `/mnt`.

- **Adatok lemezt.** [Adatok lemez] [ data-disk] egy alkalmazás adatokhoz használt állandó virtuális. Adatok lemezt Azure tárolására, például az operációs rendszer lemez tárolja.

- **Virtuális hálózati (VNet) és alhálózat.** Minden az Azure virtuális telepítik be egy (VNet), amely további oszlik alhálózat.

- **Nyilvános IP-címet.** Egy nyilvános IP-cím használatával kommunikál a virtuális van szükség&mdash;például SSH fölé.

- A **hálózati kapcsolat (hálózati)**. A hálózati kártya lehetővé teszi, hogy a virtuális használatával kommunikál a virtuális hálózathoz.

- **Hálózati biztonsági csoport (NSG)**. A [NSG] [ nsg] hálózati forgalmának engedélyezésére alhálózathoz engedélyezése és tiltása szolgál. Az egyes hálózati vagy alhálózat egy NSG társíthat. Az alhálózathoz társítani, ha a NSG szabályokat alkalmazni az adott alhálózat összes VMs.
 
- **Diagnosztikai.** Diagnosztikai naplózás elengedhetetlen kezelésével, és a virtuális hibaelhárítási.

## <a name="recommendations"></a>Javaslatok

Azure felajánlja a különféle számos különböző erőforrásokat és erőforrástípus, így a hivatkozás architektúra lehet kiépítve számos különböző módon. A fenti ajánlást követő hivatkozás architektúra telepíteni egy Azure erőforrás-kezelő sablon nyújtott. Ha úgy dönt, hogy hozzon létre saját hivatkozás architektúrája, kivéve, ha rendelkezik az adott követelmény, hogy nem férnek el a ajánlást célszerű követnie az alábbi javaslatokat. 

### <a name="vm-recommendations"></a>Virtuális javaslatok

A Tartományi - és GS-sorozat azt javasoljuk, mert ezek gépi méretű támogatja a [Prémium tároló][premium-storage]. Jelölje ki azt a gép méretű, kivéve, ha rendelkezik egy speciális terhelést, például a nagy teljesítményű számítások. További részleteket találhat a [virtuális gép méretű][virtual-machine-sizes]. Amikor egy meglévő terhelést Azure helyez át, indítsa el a virtuális mérete, amely a legközelebb áll a helyszíni kiszolgálókhoz. Ezután mérjük a Processzor vonatkozó tényleges terhelést teljesítményének, memória lemez bemeneti és kimeneti műveletek / szekundum (IOPS), és szükség esetén módosítsa a méretét. Ha több van szüksége, is érdemes szem előtt tartania minden méretét a hálózati kártya korlátját.  

Akkor kiépítése a virtuális és más erőforrások, ha meg kell adnia egy helyen. Általánosságban elmondható válasszon egy helyet, amelyek az ügyfelek és belső felhasználók legközelebb. Azonban nem az összes virtuális méretű feltétlenül vehető igénybe az összes hely. További részleteket találhat a [régió szerint szolgáltatások][services-by-region]. Az adott helyen elérhető virtuális méretű listában a következő Azure parancssori kezelőfelületről parancsot:

```
    azure vm sizes --location <location>
```

Kapcsolatos tudnivalók a közzétett virtuális képként további információért olvassa el a [választó Azure virtuális gép képek és a Navigálás][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Lemez- és javaslatok

Az lemeztevékenység jobb teljesítmény elérése érdekében azt javasoljuk [Prémium tároló][premium-storage], amely tárolja az adatokat a félvezető meghajtókon (SSD). Költség a kiépített lemez méretének alapul. IOPS és az átvitel (Ez azt jelenti, hogy adatátviteli ráta) is függ a lemez méretét, így kiépítése lemezen, akkor vegye figyelembe az összes három tényezőket (kapacitással, IOPS és az átvitt). 

Tárterület fiókok támogatják az 1-20 VMs is.

Egy vagy több adat lemez hozzáadása. Amikor létrehoz egy virtuális, formázva. Jelentkezzen be a virtuális formázni. Az adatok lemezt a Megjelenítés témákba `/dev/sdc`, `/dev/sdd`és így tovább. Futtatását is lehetővé teszi `lsblk` a továbbfejlesztett fájlblokkolás eszközről, többek között a lemezt listáját. Adatok lemezen, hozzon létre egy partíciót és fájl rendszer, és csatlakoztassa a lemez. Példa:

```bat
    # Create a partition.
    sudo fdisk /dev/sdc     # Enter 'n' to partition, 'w' to write the change.     
    
    # Create a file system.
    sudo mkfs -t ext3 /dev/sdc1
    
    # Mount the drive.
    sudo mkdir /data1
    sudo mount /dev/sdc1 /data1
```

Ha sok adatok lemezt, a felhasználónevek a teljes I/O korlátozások a tárterület-fiók. További információ című cikkben találhat [virtuális gép lemez][vm-disk-limits].

Adatok lemezen hozzáadásakor egy logikai egység szám (logikai) Azonosítóját a lemez van rendelve. Tetszés szerint megadhatja a logikai azonosító &mdash; , ha például lemezen cserélni, és szeretné tartani az azonos logikai azonosító vagy egy alkalmazás, amely egy adott logikai azonosítóját. Ne feledje azonban, hogy a logikai azonosítók minden lemezre egyedinek kell lennie.

Előfordulhat, hogy meg szeretné változtatni az I/O ütemező optimalizálása a teljesítmény elérése érdekében meghajtókon félvezető (SSD) (prémium tároló által használt). Közös ajánlása az NOOP ütemező SSD használni, de figyelheti a lemez I/O teljesítményt az adott terhelés [iostat] például egy eszközt kell használnia.

- A legjobb teljesítmény elérése érdekében a diagnosztikai naplók tartása külön tárterület-fiók létrehozása. Helyi meghajtóra felesleges tároló (LRS) fióktulajdonos elegendő a diagnosztikai naplók.


### <a name="network-recommendations"></a>Hálózati javaslatok

A nyilvános IP-cím lehet dinamikus vagy statikus. Az alapértelmezett érték dinamikus.

- A [statikus IP-cím] lefoglalása[ static-ip] szüksége van-e egy rögzített IP-címet, amely nem változik &mdash; tegyük fel például, létre kell hoznia egy A rekordot a DNS-ben vagy kell az IP-cím whitelisted kell.

- Létrehozhat egy teljes tartománynevét (FQDN) az IP-cím is. Ezután rögzítheti egy [CNAME rekordot] [ cname-record] a DNS-ben, amely a teljesen minősített tartománynév mutat. További tudnivalókért lásd: a [teljesen minősített tartománynév az Azure-portálon létrehozása][fqdn].

Az összes NSGs tartalmazza az [alapértelmezett]szabályhalmaz[nsg-default-rules], például egy szabályt, amely letiltja az összes bejövő internetes forgalmat. Az alapértelmezett szabályokat nem törölhető, de más szabályok felülbírálhatja őket. Ahhoz, hogy az internetforgalom, szabályok létrehozása, amelyek adott portokra bejövő forgalmának engedélyezésére &mdash; például http a 80-as port.  

Ahhoz, hogy a SSH, szabály hozzáadása a NSG, amely lehetővé teszi, hogy a TCP-port 22 bejövő forgalmat.

## <a name="scalability-considerations"></a>Méretezhetőség kapcsolatos szempontok

Méretezheti egy virtuális felfelé vagy lefelé [a virtuális méret]módosításával[vm-resize]. 

Ha át kívánja méretezni vízszintesen, az elérhetőség mögött egy terheléselosztó beállítása két vagy több VMs helyezni. Részletekért olvassa el [a Azure több VMs futó][multi-vm].

## <a name="availability-considerations"></a>Elérhetőség kapcsolatos szempontok

Fent leírt módon nem egy egyetlen virtuális nincs SLA. Úgy juthat az SLA, telepítenie kell az több VMs be egy elérhetőségének beállítása.

A virtuális hatással lehet által [tervezett karbantartás] [ planned-maintenance] vagy a [tervezett karbantartás][manage-vm-availability]. Használhatja a [virtuális indítsa újra a naplók] [ reboot-logs] határozza meg, hogy a virtuális újra kell indítani a tervezett karbantartás okozta.

[Azure]tároló biztonsági VHD[azure-storage], amelynek van replikált tartóssági és elérhetősége.

És vírusvédelmet véletlen adatvesztés szokásos műveletek során (például mert felhasználói hiba), célszerű is alkalmazhat pont és az idő biztonsági másolatok [blob pillanatképek] használata[ blob-snapshot] vagy egy másik eszközt.

## <a name="manageability-considerations"></a>Kezelhetőség kapcsolatos szempontok

**Erőforrás-csoportokat.** Helyezze az azonos életciklusának megosztása az azonos [erőforráscsoport]szorosan összekapcsolt erőforrások[resource-manager-overview]. Az erőforrás csoportok üzembe figyelemmel az erőforrások csoportként és számlázási erőforráscsoport költségek Felvetítés teszi lehetővé. Erőforrások beállított, amelyek rendkívül hasznos próba telepítésekhez is törölheti. Erőforrások érthető neveket ad. Amely megkönnyíti az adott erőforrás keresse meg és szerepének bemutatása. Lásd: [Azure erőforrások ajánlott elnevezési szabályai][naming conventions].

**SSH**. Mielőtt egy Linux virtuális hoz létre, készítése 2048 bites RSA nyilvános-titkos kulcs két. A virtuális létrehozásához kövesse a nyilvános kulcs fájlt. További információért megtudhatja, [hogy miként használata SSH Linux és Azure a Mac][ssh-linux].

**Virtuális diagnosztika.** Engedélyezés figyelése és diagnosztika, például egyszerű állapot mértékek, diagnosztika infrastruktúra naplók és [indítási diagnosztika][boot-diagnostics]. Indítási diagnosztika segíthetnek diagnosztizálása indítási sikertelen, ha nem indítható állapotba lekéri a virtuális. További tudnivalókért lásd: a [Figyelés és diagnosztika engedélyezése][enable-monitoring].  

A következő CLI parancs lehetővé teszi, hogy a diagnosztika:

```
    azure vm enable-diag <resource-group> <vm-name>
```

**A virtuális leállítása.** Azure "Leállt" és "Deallocated" államok közötti különbséget tesz. Amikor a virtuális állapot "leállt" előfizetést terhelő. Nem fel, ha a virtuális felszabadítása.

A következő CLI paranccsal felszabadítása egy virtuális:

```
    azure vm deallocate <resource-group> <vm-name>
```

- A **Leállítás** gombra az Azure-portálon is felszabadítja a virtuális. Jó helyen jár Ha kikapcsolja az operációs rendszer keresztül bejelentkezett, a virtuális leállítja, de _nem_ felszabadítása, így akkor továbbra is megterheljük.

**A virtuális törlése.** Ha töröl egy virtuális, a VHD nem törlődnek. Ez azt jelenti, hogy a virtuális törölhető a adatvesztés nélkül. Jó helyen jár akkor továbbra is ráterheljük tároló. A virtuális törléséhez törölje a fájlt a [blob-tárolóhoz][blob-storage].

Véletlen törlését elkerülése érdekében használja az [erőforrás-zárolás] [ resource-lock] az erőforrás teljes csoport vagy lock egyes erőforrások, például a virtuális zárolásához. 

## <a name="security-considerations"></a>Biztonsági megfontolások

Operációs rendszer frissítések automatizálhatja a [OSPatching] virtuális bővítmény használatával. A bővítmény telepítésekor, a virtuális kiépítése. Megadhatja, hogy milyen gyakran szeretné telepíteni a javítást, illetve e javítása után indítsa újra.

[Szerepköralapú hozzáférés-vezérlés] használata[ rbac] (RBAC) való hozzáférés korlátozása a Azure erőforrásokat rendszerbe. RBAC lehetővé teszi, hogy engedélyt szerepköröket rendelhet DevOps csapata tagjainak. Például az Olvasó szerepkör is Azure-erőforrások megtekintése, de nem létrehozása, kezelése, és törölje őket. Adott Azure erőforrástípus néhány szerepkörök vonatkoznak. Például a virtuális gép munkatársi szerepkörök is indítsa újra a vagy felszabadítása egy virtuális, a rendszergazdai jelszó alaphelyzetbe állítása, hozzon létre egy virtuális és így tovább. Más [beépített RBAC szerepkörök] [ rbac-roles] , amely hasznos lehet a hivatkozás architektúra [DevTest Labs felhasználói] tartalmazza a[ rbac-devtest] és [Hálózati munkatársi][rbac-network]. 

Felhasználó rendelhetők több szerepkört, és hozhat létre egyéni szerepkörök még több az egyedi engedélyek.

> [AZURE.NOTE] RBAC nem korlátozza a műveleteket, amelyeket egy virtuális bejelentkezett felhasználó hajthatják végre. Ezeket az engedélyeket az operációs rendszer vendégként a típusa határozza meg.   

Használja a [Naplók] [ audit-logs] műveletek és más virtuális események kiépítési megjelenítéséhez.

- Fontolja meg az [Azure lemez titkosítási] [ disk-encryption] Ha módosítani szeretné az operációs rendszer és az adatok lemezt titkosítása. 

## <a name="solution-deployment"></a>Megoldás üzembe helyezése

A [telepítési] [ github-folder] a hivatkozást, amely bemutatja az alábbi gyakorlati tanácsokat architektúra érhető el. A hivatkozás architektúra egy virtuális hálózati (VNet), a hálózati biztonsági csoport (NSG) és a egyetlen virtuális gép (virtuális) tartalmazza.

A hivatkozás architektúra üzembe többféle módon lehet. A legegyszerűbben kövesse az alábbi lépéseket: 

1. Kattintson a jobb gombbal az alábbi gombra, és válassza a bármelyik "hivatkozás megnyitása új lapon" vagy "A hivatkozás megnyitása új ablakban".
[![Azure telepítése](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)

2. Miután a hivatkozásra az Azure-portálon megnyílt, értéket kell megadnia az egyes beállításokat: 
    - Az **erőforráscsoport** neve már van megadva a paraméter fájlt, így válassza az **Új létrehozása** , és adja meg `ra-single-vm-rg` a szövegmezőbe.
    - A **hely** legördülő listából válassza ki a régió.
    - Ne módosítsa a **Sablon legfelső szintű Uri** vagy a **Paraméter legfelső szintű Uri** mezőbe.
    - A legördülő listából, **linux** **Os típusának** kiválasztása
    - Tekintse át a szerződési feltételeket, majd jelölje be az **elfogadom a fenti feltételek** jelölőnégyzetet.
    - Kattintson a **vásárlás** gombra.

3. Várja meg, a telepítés befejezéséhez.

4. A paraméter fájloknak csomagolásukkor rendszergazda felhasználónevet és jelszót, és azt ajánljuk, hogy azonnal módosítsa is. Kattintson a virtuális nevű `ra-single-vm0 `az Azure-portálon. Kattintson a **jelszó alaphelyzetbe állítása** **támogatási + hibaelhárítási** szakaszában. Jelölje be a **jelszó alaphelyzetbe állítása** **üzemmód** legördülő mezőben, majd jelölje be az új **felhasználó nevét** és **jelszavát**. A **frissítés** gombra kattintva továbbra is fennáll, az új felhasználó nevét és jelszavát.

További módszerek, a hivatkozás architektúra üzembe a további tudnivalókért lásd a [útmutatást-egyetlen-virtuális] fontos fájl[ github-folder] Github mappát.

## <a name="customize-the-deployment"></a>A telepítési testreszabása

A telepítési az igényeinek megfelelően módosításához kövesse a a [útmutatást-egyetlen-virtuális] [ github-folder] lapot.

## <a name="next-steps"></a>Következő lépések

Ahhoz, hogy a [SZOLGÁLTATÁSISZINT-virtuális gépeken futó] [ vm-sla] szeretné alkalmazni, telepítenie kell a két vagy több példányok egy elérhetőségének beállítása. További tudnivalókért lásd: a [több VMs Azure a futó][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: ../articles/virtual-machines/virtual-machines-windows-create-availability-set.md
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-linux]: ../articles/virtual-machines/virtual-machines-linux-azure-overview.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-linux-about-disks-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-linux-portal-create-fqdn.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm/
[iostat]: https://en.wikipedia.org/wiki/Iostat
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[OSPatching]: https://github.com/Azure/azure-linux-extensions/tree/master/OSPatching
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-linux-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[select-vm-image]: ../articles/virtual-machines/virtual-machines-linux-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssh-linux]: ../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-linux-sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_0/
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[components]: #Solution-components
[blocks]: https://github.com/mspnp/template-building-blocks
[0]: ./media/guidance-blueprints/compute-single-vm.png "Egyetlen Linux virtuális architektúra Azure-ban"

