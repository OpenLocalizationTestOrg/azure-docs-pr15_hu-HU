<properties
   pageTitle="Az ajánlat telepítse az Azure piactéren elérhető |} Microsoft Azure"
   description="Megtudhatja, és az utasításokat követve az ajánlat – üzembe virtuális gép képe, a Fejlesztőeszközök szolgáltatás, a adatszolgáltatás, a stb. – végigvezetik az Azure Piactérhez."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/02/2016"
   ms.author="hascipio" />

# <a name="deploy-your-offer-to-the-azure-marketplace"></a>Az ajánlat telepítse az Azure piactérről
Ha elégedett az ajánlat (Ez azt jelenti, hogy ellenőrizte ügyfél esetben marketing tartalmat, stb.), és készen áll a indítása, kérjen **termelési leküldéses** a **Közzététel** lapon.  

1. Kattintson a közzététel lap alapján a forgatókönyv négy lépése portál kell lennie, kész és zöld. A virtuális gép ajánlatok győződjön meg arról, hogy az alábbi útmutatásokat betartják.

    ![rajz][img-pubportal-walkthru-checked]

2. Jelölje ki a **Közzététel** lap bal szélén a listából.

    ![rajz][img-pubportal-menu-publish]

3. Kattintson a **kérelem jóváhagyása termelési leküldéses**gombra. A kérelmet, ha a jóváhagyás csapatának végrehajtja a végleges véleményt, és kattintson az ajánlat érhetők el a Microsoft Azure piactéren található.

    ![rajz][img-pubportal-publish-pushproduction]

>[AZURE.IMPORTANT] Abban az esetben, ha virtuális gépeken futó a kérelem jóváhagyása termelési, leküldéses gombra kattintva az alábbi lépésekkel megtörténik a jelenet mögött. Fogja tudni a közzététel lapon minden egyes lépés végrehajtásának tekintheti meg a közzétételi portál. Először vegye ezen az oldalon rendszeres időközönként (mindaddig, amíg az Állapot oszlopban a "Szerepel a listában") van szüksége a végéről korrekciós hiba információkat.

> - Először gyártási kérését a hitelesítő csapatának ki a virtuális érvényesítése ugrik. Jó helyen jár Ha frissít a már felsorolt ajánlatot, és a kérelem van kapott, csak marketing módosítása, majd a tanúsítvány lépés kimarad.
> - A következő lépésben a kérelmet a tartalom érvényességi csapatának ki az ajánlat marketingtevékenység tartalmának ellenőrzése állnak.
> - Ha a fenti lépéseket a sikeresek, majd az ajánlat jóváhagyja gyártási. Ekkor az állapot válnak "megtalálható" a közzétételi portál. Jó helyen jár ez az állapot "Szerepel a listában" nem jelenti azt, hogy helyesek-e a folyamat. Az alábbi lépéseket kell az ajánlat érhető el a Microsoft Azure piactéren ahhoz, hogy.
> - Miután az ajánlatra vonatkozó jóváhagyják a fenti lépésben gyártási, az ajánlat replikációs megkezdhetik az végig az Azure adatközpontokkal. Általában kerül 24-48hours a replikáció végrehajtása, de a virtuális méretétől függően egy hétig is eltarthat. Ha frissít a már felsorolt ajánlatot, és rendelkezik rendelkezik, csak marketing módosítása, majd a replikáció viszont gyorsabb.
> - Amikor befejeződött a replikáció, majd az ajánlat érhetők el a Microsoft Azure piactéren található.

> Az ajánlat mindig, törölheti, a **Piszkozat** állapota (tehát soha nem **szeretne átmeneti leküldéses** vagy **termelési leküldéses**). Kattintson az **Előzmények** lapot a **Piszkozat elvetése** gombra, ha törölni szeretne egy piszkozatot a lap alján.


## <a name="production-checklist-for-all-virtual-machine-offers"></a>Az összes virtuális gép ajánlatok gyártási ellenőrzőlistája

- Ellenőrizze, hogy a Microsoft Azure tanúsítvánnyal partnernek
- Az termékváltozatok lapon a beállítás "elrejtése a Termékváltozat a piactérről mert meg kell mindig vásárolható keresztül megoldás sablon" kell megjelölni Igen csak akkor, ha a Termékváltozat része megoldás sablon. Minden egyéb esetben ezt a beállítást kell mindig lesznek olvasottként megjelölve, nem értékre.
- Ne feledje: Nem módosítania kell a Termékváltozat láthatósági beállítás után a Termékváltozat szerepel a listában. Ez a funkció nem támogatjuk.
- Győződjön meg arról, hogy a emblémák igazodjon az Azure piactéren elérhető embléma útmutatásokat, az alábbiakban.
- Ajánlat és Termékváltozat leírás kerülni a Webhelyfiókok lehet azonos.
- Termékváltozat a cím és kínálnak hosszú összefoglaló kerülni a Webhelyfiókok lehet azonos.
- Termékváltozat cím és kínálnak összefoglaló kerülni a Webhelyfiókok lehet azonos.
- Termékváltozat címek nem lehet azonos felajánlásáról több SKU.

**Azure Piactér embléma útmutató**

- Az Azure tervezés egyszerű színpalettájának tartalmaz. Tartsa az száma az elsődleges és másodlagos színek az embléma a alacsony.
- A téma színei a Azure portál fehérek és fekete. Így elkerülheti, ezeket a színeket a használják a emblémák háttérszínének. Használja, néhány szín, amely a emblémák hangjelzéseket az Azure-portálon tenné. Azt javasoljuk, hogy egyszerű alapszínek. Ha átlátszósággal használ, akkor győződjön meg róla, hogy a embléma szöveg nem fehér vagy fekete.
- Az embléma a színátmenetes háttér ne használjon.
- Ne helyezzen szöveg, akár a vállalata vagy márka neve, az embléma.
- Az embléma kinézetét és hangulatát strukturálatlan"kell lennie, és kerülje színátmenetek.
- Az embléma nem kell kiterjesztett.

**A fő embléma irányelveket:**

- A fő embléma nem kötelező. A publisher dönthet úgy, hogy nem a fő embléma feltöltése. **Azonban egyszer feltöltött a fő ikon nem lehet törölni a közzétételi portál. Ekkor a partner kell kövesse a Microsoft Azure piactéren profilképek fő ikonok egyéb az ajánlat nem lehet jóváhagyni gyártási.**
- A Publisher megjelenítendő nevet, Termékváltozat cím és a hosszú összefoglaló ajánlatot fehér betűszín jelenik meg. Így ne bármilyen világos színű tartása a háttérben a fő ikonra. Fekete, háttér fehér vagy átlátszó visszacsatolás nem engedélyezett a fő ikonok megjelenítéséhez.
- A publisher jelenít meg, név, Termékváltozat cím, a hosszú összefoglaló ajánlatot, és a Létrehozás gombra vannak beágyazva programozás útján a fő embléma után az ajánlatra vonatkozó Ugrás felsorolt. Így nem kell megadni szöveget a fő embléma tervezésekor is. Hagyja üres területre a jobb oldalon, mert a szöveget (azaz a Publisher megjelenítési neve, Termékváltozat címet, a hosszú összefoglaló ajánlatot) szerepelni fog programozás útján amerikai ott fölé. A szöveg az üres terület 415 x 100 a jobb oldalon (és kell azt az eltolás által 370px balról).


## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>További gyártási feladatlista már felsorolt virtuális gép kínál

- Ellenőrizze, hogy már nem a vállalaton kívüli ajánlat névvel felajánlás. Ha igen, majd adja a Termékváltozat új verziója helyett egy új ismétlődő ajánlatot a meglévő lehetőség.
- Adatok lemez ne módosítsa az azonos Termékváltozat két verziója között.
- A Microsoft Azure piactéren nem támogatja a felsorolt Termékváltozatok árak módosítása, akkor hatással van a meglévő ügyfelek számlázás. Győződjön meg arról, hogy ne módosítsa a hol érhető el a Termékváltozat régiókban a felsorolt termékváltozatok árak. Azonban adja hozzá az új termékváltozatok, vagy új régiók hozzáadása egy meglévő Termékváltozat.


## <a name="next-steps"></a>Következő lépések
Miután az ajánlatra vonatkozó élő kerül, tesztelje az ügyfél jelenik meg, hogy a szerződési és funkció működni az éles üzemi környezetben, mint az összes vizsgálni, és a fejlesztői környezet érvényesített érvényesítéséhez.

## <a name="see-also"></a>Lásd még:
- [Első lépések: a Microsoft Azure piactéren felajánlás közzététele](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
