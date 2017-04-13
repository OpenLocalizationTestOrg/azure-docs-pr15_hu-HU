<properties
   pageTitle="Az Azure előfizetéssel tulajdonjogát átvitele |} Microsoft Azure"
   description="Az Azure előfizetéssel átvitele egy másik felhasználó, és néhány gyakran ismételt kérdések a folyamat"
   services=""
   documentationCenter=""
   authors="genlin"
   manager="stevenpo"
   editor=""
   tags="billing,top-support-issue"/>

<tags
   ms.service="billing"
   ms.workload="na"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/10/2016"
   ms.author="genli"/>

# <a name="transferring-ownership-of-an-azure-subscription"></a>Az Azure előfizetéssel tulajdonjogát átvitele

Tegye meg:

- Kell kioszthatja felett tulajdonjogát valaki másnak Azure előfizetése számlázási?
- A módosítandó regisztrálni szeretne az Azure használt fiókot? Esetleg használt Microsoft Account, de használja a munkahelyi vagy iskolai fiók inkább szólnak?
- Szeretne váltani az Azure előfizetés egyik könyvtár?
- Azure és az Office 365 a különböző bérlők és szeretné összesíteni?

Most már ehhez egyszerűen a a Microsoft Azure fiók központ - kirovó, az MSDN, a művelet csomag vagy a BizSpark előfizetések.  Hozzáadtunk, az azt jelenti, hogy az előfizetés át egy másik felhasználónak. Más szóval most módosíthatja a bármely kirovó, az MSDN webhelyen, a művelet csomag vagy BizSpark előfizetést, saját, függetlenül attól, hogy melyik országban működik a fiók rendszergazdája. Most már támogatjuk az Azure piactéren elérhető vásárlások átadása az ilyen előfizetés is.

> [AZURE.NOTE] Az előfizetés egy másik ajánlat módosításához lásd: [Váltás másik ajánlat Azure előfizetése](billing-how-to-switch-azure-offer.md) további információt. Ha bármely pontján Ez a cikk további segítségre van szüksége, kérjük, [Kapcsolatfelvétel az ügyfélszolgálattal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) veheti a probléma megoldódott gyorsan.

## <a name="how-to-transfer-ownership-of-an-azure-subscription"></a>Az Azure előfizetéssel tulajdonjogát átvitele

> [AZURE.VIDEO transfer-an-azure-subscription]

1.  Jelentkezzen be a <https://account.windowsazure.com/Subscriptions>. Akkor rendszergazdának kell lennie a fiók-tulajdonosi átadás végrehajtásához. Keresheti meg, hogy ki az előfizetés a fiók rendszergazda kapcsolatos további tudnivalókért olvassa el a [Gyakori kérdések](#faq)című témakört.

2.  Jelölje ki a továbbítani szeretné az előfizetést.

3.  Kattintson az **Előfizetés átadása** beállítás.

    ![Azure-fiók előfizetések lap](./media/billing-subscription-transfer/image1.png)

4.  Kövesse az útmutatást követve adja meg a címzettet.

    ![Átadás előfizetés párbeszédpanel](./media/billing-subscription-transfer/image2.PNG)

5.  A címzett elfogadó hivatkozást tartalmazó e-mail kap.

    ![Előfizetés átadás e-mail címzett számára](./media/billing-subscription-transfer/image3.png)

6.  A címzett a hivatkozásra kattint, és az utasításokat, beleértve a fizetési információk megadására követi.

    ![Első előfizetés átadás weblap](./media/billing-subscription-transfer/image4.png)

    ![Második előfizetés átadás weblap](./media/billing-subscription-transfer/image5.png)

7. A siker! Az előfizetés ekkor átkerülnek.

<a id="faq"></a>
## <a name="frequently-asked-questions-faq"></a>Gyakori kérdések

-   **Hogyan lehet állapíthatom meg, hogy ki-e az előfizetés a fiók rendszergazda?**

    Jóváhagyhatja ki az előfizetés a fiók rendszergazda az alábbi képlettel történik:

    1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
    2. A központi lapon válassza azt az **előfizetést**.
    3. Jelölje ki az ellenőrizni kívánt előfizetést, és válassza a **Beállítások**gombra.
    4. Válassza a **Tulajdonságok parancsot**. Az előfizetés a fiók rendszergazda jelennek meg a **Fiók rendszergazda** mezőbe.  

-   **Előfizetés áthelyezés bármely szolgáltatás legrövidebb leállás eredményeznek?**

    Nincs hatással a szolgáltatás nem. Ez hatékony megszakít az aktuális rendszergazdai fiókkal az előfizetést és hoz létre egy újat a címzett fiókon, de az alapul szolgáló Azure szolgáltatások társít az új előfizetésbe. Az előfizetés azonosítója változatlan marad.

-   **Hogyan használhatom Ez az eljárás módosítása a könyvtárat előfizetés?**-   
    Egy Azure-előfizetést, amelyhez a fiók felügyeleti tartozik a címtárban jön létre. Igen annak érdekében, hogy módosítja a címtárban, csak nem ruházhatja át az előfizetés egy felhasználói fiókot, a cél címtárban. A lépéseket követve fogadja el az átviteli befejezésekor a felhasználó számára az előfizetés automatikus áthelyezése a cél könyvtár.

-   **Ha egy másik szervezettől előfizetés számlázási tulajdonlásának átvétele lehet, azok továbbra is van hozzáférése az erőforrások?**

    Az előfizetés át egy másik bérlőjére, ha a felhasználók az előző bérlői társított elvesznek a hozzáférést az előfizetést. Még ha a felhasználó nem a szolgáltatás-rendszergazda vagy a további-rendszergazda bezárhatja, azok előfordulhat, hogy még mindig hozzáférnek más biztonsági mechanizmusok segítségével az előfizetéséhez. Ezek a következők:
    - A felhasználó engedélyeket felügyeleti előfizetés erőforrások kezelése tanúsítványok. További tudnivalókért lásd: a [tölteni a Azure tartozó adatkezelési tanúsítvány](https://msdn.microsoft.com/library/azure/gg551722.aspx)
    -   Hívóbetűk szolgáltatások, például tárhelyet. További tudnivalókért lásd: a [nézet, a másolás, és a hívóbetűk újragenerálása tárhely](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
    -   Szolgáltatások, például Azure virtuális gépeken futó távoli hozzáférés hitelesítő adatok

    Ez nem teljes listáját. A címzett figyelembe bármely titkos kulcsok, ha van szükségük az erőforrások való hozzáférés korlátozása a szolgáltatáshoz tartozó frissítése. A legtöbb erőforrás a következőképpen frissíthető:

    1.   Válassza az Azure-portálra: [ *https://portal.azure.com*](https://portal.azure.com)

    2.    Kattintson az összes - böngészése&gt; összes erőforrás

    3.    Jelölje ki az erőforrást. Ekkor megnyílik az erőforrás lap.

    4.    Az erőforrás lap kattintson a **Beállítások**gombra. Itt tekintheti meg és frissítheti a meglévő titkos kulcsok.


-   **Ha e át az előfizetés számlázási ciklustól közepén, nem a teljes számlázási címzett díja ciklus?**

    A feladó a felelős bármely használatát a pont, hogy befejeződött-e a továbbított felfelé jelentett fizetni. A címzett feladata a időpontot a-tól a hívásátadás jelentett használatát. Előfordulhat, hogy történtek átadás előtt, de később jelentett néhány használatát. Ez az a címzett számla tartalmazni fogja.

-   **Használati és számlázási előzmények hozzáféréssel rendelkezik a címzett?**

    Jelenleg csak a címzett felfedni adatai az utolsó számla (vagy az aktuális egyenleg, ha az előfizetés vitt, mielőtt az első számla jött létre) mennyiségét. A többi a használati és számlázási előzmények nem nem ruházhatja át az előfizetéssel rendelkező.

-   **Az ajánlat módosíthatják a átvitele során?**

    Az ajánlat változatlan marad. Ha módosítani szeretné az ajánlat, be kell [ügyfélszolgálatát](http://go.microsoft.com/fwlink/?LinkID=619338).

-   **Átvihetem előfizetés más országban egy felhasználói fiókot?**

    Nem, a problémának jelenleg nem támogatott. A címzett felhasználói fiókot az azonos országon kell lennie.

-   **Használhatom-e a címzett egy másik fizetési mechanizmusok?**

    igen. Az alábbi korlátozások érvényesek: most két fiókok között az előfizetés számlázási előzmények osztott. De az az előnye, hogy ezt megteheti anélkül, hogy a [Kapcsolatfelvétel az ügyfélszolgálattal](http://go.microsoft.com/fwlink/?LinkID=619338).

-   **Lesz a fizetési módot kell hatással lehet átvitele az Azure előfizetéssel után?**

    Annak érdekében, hogy az előfizetés áthelyezés elfogadásához bankkártyával vagy hasonló fizetési módot meg kell adni az előfizetés fizetési. Például ha Péter előfizetés át Verebélyi és gipsz, fogadja el a továbbított, gipsz kell adnia, hogy kikkel kell használni az előfizetésért fizethet fizetési módot. Az átadás befejeződése után Péter már nem megterheljük az előfizetés automatikus Verebélyi át.

## <a name="next-steps-after-accepting-ownership-of-a-subscription"></a>Előfizetés tulajdonosa elfogadása után a következő lépések

1. Most már a fiók rendszergazda áll. Tekintse át, és frissítse a szolgáltatás-rendszergazda, és további rendszergazdák. A rendszergazdák az [Azure klasszikus portálon](https://manage.windowsazure.com) kezelheti beállítások. [Tudjon meg többet](http://go.microsoft.com/fwlink/?LinkID=533293).
2. Szerepköralapú hozzáférés-szerepalapú az előfizetést és a szolgáltatások számára is használhatja. Látogasson el a [Azure portál](https://portal.azure.com) [További tudnivalók a RBAC](http://go.microsoft.com/fwlink/?LinkID=544802)
3. Az előfizetés szolgáltatásokhoz tartozó hitelesítő adatok frissítése Ezek a következők:
    - A felhasználó engedélyeket felügyeleti előfizetés erőforrások kezelése tanúsítványok. További tudnivalókért lásd: [Azure-tanúsítvány létrehozása és feltöltése egy kezelése](https://msdn.microsoft.com/library/azure/gg551722.aspx)
    -   Hívóbetűk szolgáltatások, például tárhelyet. További tudnivalókért lásd: a [nézet, a másolás, és a hívóbetűk újragenerálása tárhely](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
    -   Szolgáltatások, például Azure virtuális gépeken futó távoli hozzáférés hitelesítő adatok
4. Ehhez az előfizetéshez, a [Azure-fiók központ](https://account.windowsazure.com/Subscriptions)[tudjon meg többet](http://go.microsoft.com/fwlink/?LinkID=533292) a számlázási értesítések módosítása  
5.  Ha egy partnerrel dolgozik, fontolja meg, az előfizetés a partner azonosító frissítésének. Ez az [Azure-fiók központ](https://account.windowsazure.com/Subscriptions)teheti meg.

> [AZURE.NOTE] Ha továbbra is további kérdései vannak, kérjük, [Kapcsolatfelvétel az ügyfélszolgálattal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) veheti a problémát a rendszer gyorsan.
