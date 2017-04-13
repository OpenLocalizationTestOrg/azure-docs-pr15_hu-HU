<properties
    pageTitle="Magas elérhetősége határokon földrajzi AD FS telepítési Azure forgalom Manager Azure-ban |} Microsoft Azure"
    description="A dokumentumban megtanulhatja, hogyan telepítse az Azure Active Directory összevonási szolgáltatások magas availablity."
    keywords="Active Directory összevonási szolgáltatások Azure forgalom manager, adfs Azure forgalom Manager, földrajzi, több adatközponthoz, földrajzi adatközpontokkal, több földrajzi adatközpont esetén az azure Active Directory összevonási szolgáltatások üzembe, azure adfs, az azure adfs, az azure Active Directory összevonási szolgáltatások üzembe, telepítése az adfs, üzembe az Active Directory összevonási szolgáltatások, az azure-adfs telepítése az adfs azure-ban, üzembe AD FS azure, az adfs azure, az Active Directory összevonási szolgáltatások, a Azure, az Azure-iaas az Active Directory összevonási szolgáltatások – bevezetés , ADFS, azure adfs áthelyezése"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="anandy;billmath"/>
    
#<a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Magas elérhetősége határokon földrajzi AD FS telepítési Azure forgalom Manager Azure-ban

[Azure Active Directory összevonási szolgáltatások bevezetésének](active-directory-aadconnect-azure-adfs.md) nyújt részletes iránymutatást, hogy miként telepítheti egy egyszerű AD FS-infrastruktúrát Azure-ban a szervezet számára. Ebben a cikkben vektoriális földrajzi telepítési, az Active Directory összevonási szolgáltatások létrehozása az Azure a következő lépésekkel [Azure forgalom](../traffic-manager/traffic-manager-overview.md)segítségével. Azure forgalom Manager segít földrajzilag várható magas rendelkezésre állásának és a szervezet Active Directory összevonási szolgáltatások infrastruktúra nagy teljesítményű létrehozása azáltal, hogy útválasztási módszerekkel érhető el a különböző módon az infrastruktúra a tartomány használatára.

Egy könnyen hozzáférhető határokon földrajzi AD FS-infrastruktúrát lehetővé teszi, hogy:

* **Egyetlen pont hiba eltávolítás:** Feladatátvevő funkciókhoz Azure forgalom Manager érhet el egy könnyen hozzáférhető AD FS-infrastruktúrát akkor is, ha megszakad a adatközpontokban a földgömb részében közül
* **Teljesítménynövekedés:** Ez a cikk a javasolt példányban segítségével adja meg a nagy teljesítményű AD FS infrastruktúra felhasználók gyorsabb hitelesítést végezni. 

##<a name="design-principles"></a>Tervező elvek

![Általános tervezési](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

Az alapvető tervezési elvek ugyanaz, mint a tervezés elvek Azure Active Directory összevonási szolgáltatások bevezetésének cikkben felsorolt lesz. A fenti ábrán egy egyszerű kiterjesztése a egyszerű telepítésben szeretne egy másik földrajzi területhez tartozik. Az alábbiakban néhány tudnivaló a üzembe az új földrajzi régiók bővítésekor

* **Virtuális hálózati:** A földrajzi régióban szeretné telepíteni az Active Directory összevonási szolgáltatások infrastruktúrájának bővítéseit hozzunk létre egy új virtuális hálózathoz. A diagramban látni Geo1 VNET és Geo2 VNET minden földrajzi régióban két virtuális hálózatból.

* **Tartomány vezérlők, és az új földrajzi VNET AD FS-kiszolgálók:** A tartomány új földrajzi régióban vezérlők, hogy az AD FS-kiszolgálók új régióban nem kell a tartományvezérlőnek a másik kéznél forduljon a hálózati hitelesítés és egyúttal gördülékenyebbé tételéhez elvégzéséhez üzembe ajánlott.

* **Tárolás fiókok:** Tárterület-fiókok társítva egy területet. Mivel fog egy új földrajzi régióban gépek telepít, akkor hozhat létre új tárterület-fiókot kell használni a tartományban lévő.  

* **Hálózati biztonsági csoportok:** Ahogy a tárterület-fiók esetén hálózati biztonsági csoportokat létrehozni egy tartomány nem használható más földrajzi területhez tartozik. Szüksége lesz így hozhat létre új hálózati biztonsági csoportok hasonlóak az első földrajzi területhez tartozik INT és DMZ alhálózat új földrajzi régióban.

* **Nyilvános IP-címek DNS címkék:** Azure forgalom Manager végpontokat is hivatkozhat csak DNS címkék keresztül. Ezért szükségesek a külső terheléselosztókkal nyilvános IP-címek DNS címkék létrehozása.

* **Azure forgalom Manager:** Microsoft Azure-forgalmat kezelő lehetővé teszi a felhasználó-alapú forgalmat a szolgáltatási végpontok fut a világ különböző adatközpont esetén terjesztését. Azure forgalom Manager működik, a DNS-szintjén. Irányítsa át a végfelhasználói forgalom globálisan elosztott végpontok DNS válaszok használ. Ügyfelek csatlakozhat ezeket a végpontok közvetlenül. Beállításokkal különböző útválasztási teljesítményét, Weighted és prioritását egyszerűen választhatja a legjobban a szervezete igényeihez útválasztási lehetőséget. 

* **Két sorterület V-nettó V hálózat összekapcsolását:** Nem kell lennie a virtuális hálózatok közötti kapcsolatot magát. Mivel minden virtuális hálózati fér hozzá a tartomány vezérlők és az Active Directory összevonási szolgáltatások és WAP kiszolgáló önmagában, azok a virtuális hálózatok különböző területei között minden kapcsolat nélkül is dolgozhat. 

##<a name="steps-to-integrate-azure-traffic-manager"></a>Integrálása az Azure forgalom vezető lépések

###<a name="deploy-ad-fs-in-the-new-geographical-region"></a>Az új földrajzi területhez tartozik az AD FS terjesztése
Kövesse a lépéseket, és [Azure Active Directory összevonási szolgáltatások bevezetésének](active-directory-aadconnect-azure-adfs.md) bevezetését tervezi a azonos topológia új földrajzi régióban útmutatását.

###<a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>Nyilvános IP-címét az Internet szemben lévő (nyilvános) terheléselosztókkal DNS címkék
Fent említett az Azure-forgalmat Manager csak is hivatkozhat, a végpontok DNS címkéket, és ezért fontos, hogy a külső terheléselosztókkal nyilvános IP-címek DNS címkék létrehozása. Képernyőfelvétel alatt bemutatja, hogyan konfigurálható a DNS-címkének az nyilvános IP-cím. 

![DNS-címke](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

###<a name="deploying-azure-traffic-manager"></a>Azure forgalom Manager üzembe helyezése

Kövesse az alábbi lépéseket követve forgalom manager profil létrehozása. További információ akkor is érdemes lehet [az Azure forgalom Manager profilok](../traffic-manager/traffic-manager-manage-profiles.md)kezelése.

1. **Forgalom Manager profil létrehozása:** Adjon egyedi nevet az adatforgalom-kezelő profilját. Ez a név, a profil része a DNS-nevét, és működik-e a forgalom Manager tartomány neve címke előtaggal. A név / előtag kerül. trafficmanager.net manager a forgalmához DNS címke létrehozásához. Az az alábbi képernyőképen a forgalom kezelő DNS előtag mysts beállítása alatt, és eredményül kapott DNS-címkének mysts.trafficmanager.net. 

    ![Adatforgalom Manager profil létrehozása](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
 
2. **Adatforgalmat útválasztási módszer:** Háromféleképpen útválasztási érhető el az adatforgalom-kezelőben:

    * Prioritás 
    * Teljesítményét
    * Súlyozott
    
    **Teljesítmény** módszer az ajánlott erősen válaszol AD FS-infrastruktúrát eléréséhez. Bármely igényeknek leginkább megfelelő útválasztási módszert is választhat. Az Active Directory összevonási szolgáltatások funkciókat nincs hatással van a útválasztási beállítást. További információt talál [forgalom Manager forgalom útválasztási módszereket](../traffic-manager/traffic-manager-routing-methods.md) . A minta kép felett, láthatja a kijelölt **Teljesítmény** módszert.
   
3.  **Végpontok konfigurálása:** A forgalom manager lapon végpontok kattintson, és válassza a Hozzáadás gombra. Ekkor megnyílik egy hozzáadása végpont lap hasonlóan, mint a képernyőképet, az alábbi
 
    ![Állítsa be a végpontok](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
 
    A különböző bemeneti adatok alapján kövesse az alábbi iránymutatást:

    **Típusa:** Azure végpont válassza azt fogja mutat, az Azure nyilvános IP-címére.

    **Neve:** Adjon nevet a végponton társítani kívánt. Ez jelenleg nem a DNS-nevét, és nincs hatással van a DNS-rekordok.

    **Megcélozhat erőforrástípust:** Jelölje be a tulajdonság értékét nyilvános IP-cím. 

    **Erőforrás cél:** Meg fogalmat ad a különböző DNS címkék közül választhat, elérhető van az előfizetés a lehetőségek közül. Válassza a DNS-címkének.

    Adja hozzá azt szeretné, hogy a forgalom átirányítása Azure forgalom kezelője földrajzi területenként végpontja.
    További információt és részletes lépései hozzáadása / végpontok konfigurálása a forgalom manager tekintse át a [hozzáadásához letiltása, engedélyezése és törlése a végpontok](../traffic-manager/traffic-manager-endpoints.md)
    
4. **Konfigurálása vizsgálati:** A forgalom manager lapon kattintson a konfiguráción. Konfigurációs lapon módosítania kell a 80-as port HTTP és a relatív elérési út /adfs/probe probe képernyő-beállítások

    ![Vizsgálati konfigurálása](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 

    >[AZURE.NOTE] **Győződjön meg arról, hogy a végpontok állapotának ONLINE után a konfigurálás befejeződött**. Ha az összes végpontok csökkentett"állapotú, a Azure forgalom kezelő és irányítja a forgalmat feltételezve, hogy a diagnosztika helytelen, és az összes végpontok érhetők el a legjobb próbálkozásra hajt végre.

5. **Módosítása DNS-rekordok Azure forgalom Manager:** Az összevonási szolgáltatás a CNAME Azure forgalom Manager DNS-nevének kell lennie. Hozzon létre egy CNAME nyilvános DNS-rekordok, hogy a személy, aki partnere kapcsolatba próbál elérni az összevonási szolgáltatás ténylegesen elérné a Azure-forgalmat kezelő.

    Például az összevonási szolgáltatás fs.fabidentity.com a forgalom kezelő mutassanak volna módosítania kell a DNS-erőforrásrekord a következő lesz:

    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

##<a name="test-the-routing-and-ad-fs-sign-in"></a>A továbbítás és AD FS bejelentkezés   

###<a name="routing-test"></a>Továbbítás tesztelése

A továbbítás legalapvetőbb vizsgálat a teendő az összevonási szolgáltatás DNS neve minden földrajzi régióban számítógépről ping lenne. Attól függően, hogy a választott útválasztási módszer a ténylegesen pingelése végpont jelennek meg a ping megjelenítési. Például ha lehetőséget választotta, a teljesítmény útválasztás, majd a legközelebb a régió, az ügyfél végpont lejár. Az alábbi képen a két pingelése, a két különböző régió ügyfél gép pillanatképét egy EastAsia régió és egy nyugati USA-beli. 

![Továbbítás tesztelése](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

###<a name="ad-fs-sign-in-test"></a>Active Directory összevonási szolgáltatások bejelentkezési tesztelése

Az Active Directory összevonási szolgáltatások tesztelése legegyszerűbben a IdpInitiatedSignon.aspx lapján. Annak érdekében, hogy elvégeztetni, hogy az szükséges ahhoz, hogy az Active Directory összevonási szolgáltatások tulajdonságainak IdpInitiatedSignOn. Hajtsa végre az alábbi lépésekkel ellenőrizze az Active Directory összevonási szolgáltatások beállítása
 
1. Futtassa az alábbi parancsmag, a PowerShell használatával állítsa be engedélyezve van az AD FS-kiszolgálón. Set-AdfsProperties - EnableIdPInitiatedSignonPage $true
2. A minden olyan külső gépi access https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. Meg kell jelennie a Active Directory összevonási szolgáltatások lap alatt hasonlóan:

    ![ADFS-próba - hitelesítés beavatkozás igazolására szolgáló eljárás](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)

    és a sikeres-bejelentkezés, fog szolgálni, egy üzenetet az alább látható módon:

    ![ADFS-próba - hitelesítés sikeres](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)
 
##<a name="related-links"></a>Kapcsolódó hivatkozások
* [Egyszerű AD FS-telepítés Azure-ban](active-directory-aadconnect-azure-adfs.md)
* [Microsoft Azure forgalom Manager](../traffic-manager/traffic-manager-overview.md)
* [Adatforgalom Manager forgalom útválasztási módszerek](../traffic-manager/traffic-manager-routing-methods.md)

##<a name="next-steps"></a>Következő lépések
* [Az Azure forgalom Manager profilok kezelése](../traffic-manager/traffic-manager-manage-profiles.md)
* [Adja hozzá, letiltása, engedélyezése és törlése a végpontok](../traffic-manager/traffic-manager-endpoints.md) 

