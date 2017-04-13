<properties
   pageTitle="Hozzáférés-vezérlés tó adattár áttekintése |} Microsoft Azure"
   description="Dátumtáblázatok ismertetése hogyan hozzáférés-vezérlés az Azure tó adattárhoz"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="access-control-in-azure-data-lake-store"></a>Azure tó adattár hozzáférés-vezérlés

Tó adattár végrehajtja a hozzáférés-vezérlési modell, amely az Fájlrendszerhez, majd a a hozzáférés-vezérlési POSIX modell. Ez a cikk a hozzáférés-vezérlési modell tó adattár alapjait foglalja össze. További tudnivalók a Fájlrendszerhez hozzáférés-vezérlés modell információ [Fájlrendszerhez engedélyek útmutató](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="access-control-lists-on-files-and-folders"></a>A fájlok és mappák hozzáférés-vezérlési listák

Vannak olyan kétféle számára a hozzáférést vezérlő vezérlési listákat – **Access hozzáférés-vezérlési listák** és **Alapértelmezett hozzáférés-vezérlési listák**.

* **Az access hozzáférés-vezérlési listák** – ezek határozzák meg, az objektumokhoz való hozzáférést. Fájlok és mappák is van az Access hozzáférés-vezérlési listák.

* **Alapértelmezett hozzáférés-vezérlési listák** – "Sablonként" hozzáférés-vezérlési listák társított egy mappát, amely meghatározza az Access hozzáférés-vezérlési listák létrehozása a mappa alatti gyermek elemeket. Fájlok nem rendelkezik alapértelmezett hozzáférés-vezérlési listák.

![Tó adattár hozzáférés-vezérlési listák](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Az Access hozzáférés-vezérlési listák és a alapértelmezett hozzáférés-vezérlési listák van ugyanazt a szerkezetet.

![Tó adattár hozzáférés-vezérlési listák](./media/data-lake-store-access-control/data-lake-store-acls-2.png)

>[AZURE.NOTE] Az alapértelmezett vezérlés a szülő a módosítás nem befolyásolja a hozzáférés-vezérlés vagy a gyermek már megtalálható elemek alapértelmezett vezérlés.

## <a name="users-and-identities"></a>Felhasználók és identitások

Minden fájl és mappa ezek identitások egyedi engedélyeket foglalja magában:

* A fájl a tulajdonos felhasználó
* A tulajdonos csoport
* Névvel ellátott felhasználók
* Névvel ellátott csoportok
* A többi felhasználó

Az azonosítási a felhasználók és csoportok úgy az Azure Active Directory (AAD) identitások példáiban "felhasználó", a tó adattár környezetében vagy jelentheti egy AAD-felhasználó vagy biztonsági csoport AAD.

## <a name="permissions"></a>Engedélyek

Az engedélyek formázáshoz objektum **olvasható**, **Írja be**, és **végrehajtás** , és használható a fájlok és mappák, az alábbi táblázatban látható módon.

|            |    Fájl     |   Mappa |
|------------|-------------|----------|
| **Read (R)** | A fájl tartalmát tudja olvasni. | **Olvasás** és a **végrehajtás** a mappa tartalmát felsoroló szükséges.|
| **Írás (W)** | Írhat vagy hozzáfűzése egy fájlhoz | **Írja be és hajtsa végre** a gyermek elemek létrehozása a mappa szükséges. |
| **Végrehajtás (X)** | Nem jelent semmit, az adatok tó áruház környezetben | A mappa alárendelt elemeinek bejárásához szükséges. |

### <a name="short-forms-for-permissions"></a>Az engedélyek rövid űrlapok

**RWX**használják, hogy **olvassa + írása + végrehajtása**. További sűrített numerikus űrlap létezik, amelyben **olvasható = 4**, **írása = 2**, és **végrehajtás = 1** és a SZUM jelöli az engedélyeket. Az alábbiakban néhány példát talál.

| Numerikus űrlap | Rövid űrlap |      A hibaüzenet jelentése:     |
|--------------|------------|------------------------|
| 7            | RWX        | További + írása + végrehajtása |
| 5            | R-X        | Olvassa el a + végrehajtása         |
| 4            | R –        | Olvasás                   |
| 0            | ---        | Nincs engedélye         |


### <a name="permissions-do-not-inherit"></a>Engedélyek nem öröklik

A modellben POSIX típusú adattárhoz tó által használt elem engedélyeinek tárolja az elemben. Más szóval elem engedélyeinek nem öröklik a szülő-elemek közül.

## <a name="common-scenarios-related-to-permissions"></a>Engedélyek kapcsolatos gyakori alkalmazási területek

Az alábbiakban néhány gyakori alkalmazási területek tó adattár fiókkal bizonyos műveletek elvégzéséhez szükséges engedélyek megértéséhez.

### <a name="permissions-needed-to-read-a-file"></a>A fájl olvasásához szükséges engedélyek

![Tó adattár hozzáférés-vezérlési listák](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* Olvasható – a fájlt a hívó **olvasási** engedéllyel kell adni
* A mappastruktúra, amelyeknél a fájl - mappák a hívó **végrehajtás** engedélyt kell adni

### <a name="permissions-needed-to-append-to-a-file"></a>Egy fájl hozzáfűzése szükséges engedélyek

![Tó adattár hozzáférés-vezérlési listák](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* A beszúrandó - fájlt a hívó **írási** engedélyekkel kell adni
* A mappák, amelyeknél a fájl - kíván a hívó féllel **végrehajtás** engedélyt kell adni

### <a name="permissions-needed-to-delete-a-file"></a>Törölhetők a fájlok szükséges engedélyek

![Tó adattár hozzáférés-vezérlési listák](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* A szülő mappából – a hívó kell adni **írása + végrehajtása** engedélyek
* Az elérési útra – az összes többi mappa a hívó **végrehajtás** engedélyt kell adni

>[AZURE.NOTE] Írja be a fájl engedélyeinek nincs szükség törölhetők a fájlok, mindaddig, amíg a fenti két feltétel teljesülése esetén.

### <a name="permissions-needed-to-enumerate-a-folder"></a>Számba veheti a webhely mappa szükséges engedélyek

![Tó adattár hozzáférés-vezérlési listák](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* Számba veheti a webhely - a mappa a hívó **olvasható + végrehajtás** engedélyek van szüksége
* Minden előd mappák – a hívó **végrehajtás** engedélyt kell adni

## <a name="viewing-permissions-in-the-azure-portal"></a>Engedélyek megtekintése az Azure-portálon

Kattintson a tó adattár fiók **Adatainak Explorer** lap, **az Access** egy fájlt vagy mappát a hozzáférés-vezérlési listák megjelenítéséhez. Az alábbi képernyőképen tekintheti meg a **katalógus** mappa a **mydatastore** fiókon hozzáférés-vezérlési listák Access kattintson.

![Tó adattár hozzáférés-vezérlési listák](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

Ezt követően az **Access** lap kattintson **Egyszerű** az egyszerűbb nézet megjelenítéséhez.

![Tó adattár hozzáférés-vezérlési listák](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Kattintson a **Speciális nézet** a speciális nézet megjelenítéséhez.

![Tó adattár hozzáférés-vezérlési listák](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="the-super-user"></a>A felügyelő felhasználó

Egy felügyelő felhasználó rendelkezik az összes felhasználót a legtöbb tartalomvédelmi az adatok tó áruházból. Felügyelő felhasználó:

* **az összes** fájlok és mappák RWX engedélyei
* módosíthatja az engedélyeket a minden fájl vagy mappa.
* módosíthatja a tulajdonos felhasználó vagy bármely fájl vagy mappa tulajdonos csoport.

Azure-ban egy tó adattár fióknak több Azure szerepkörök:

* Tulajdonosok
* Munkatársak
* Olvasók
* Stb.

Minden tagja a **tulajdonosok** szerepkör tó adattár fiókot a program automatikusan a rendszer felhasználói fiókhoz tartozó. További tudnivalók a Azure szerepkör alapján Access vezérlő (RBAC) információ [szerepköralapú hozzáférés - ellenőrzés](../active-directory/role-based-access-control-configure.md).

## <a name="the-owning-user"></a>A tulajdonos felhasználó

Az elemet létrehozó felhasználó automatikusan megtörténik a tulajdonos felhasználó annak az elemnek. Egy tulajdonos műveleteket hajthatja végre:

* Egy fájl, amely a tulajdonosa az engedélyek módosítása
* Egy fájl, amely tulajdonában van, a tulajdonos csoport mindaddig, amíg a tulajdonos felhasználó is a tagja a cél csoport módosítása

>[AZURE.NOTE] A tulajdonos felhasználói **nem lehet** módosítani a tulajdonos felhasználó másik tulajdonú fájl. Csak a rendszer felhasználója képes legyen módosítani a tulajdonos felhasználó egy fájlt vagy mappát.

## <a name="the-owning-group"></a>A tulajdonos csoport

A POSIX hozzáférés-vezérlési listák, a minden felhasználó "elsődleges csoport" társítva. Például a felhasználó "Anna" a "Pénzügy" csoport tartozhat. Anna több tartozhatnak, de saját elsődleges csoport mindig kijelölt egy csoportot. POSIX, amikor Anna létrehoz egy fájlt, a tulajdonos csoport, hogy a fájl beállítása saját elsődleges csoporthoz, amely ebben az esetben "Pénzügy".
 
Formázáshoz új elem létrehozásakor tó adattár hozzárendel egy értéket a tulajdonos csoport. 

* **Eset 1** - gyökérmappájából "/". Ebben a mappában jön létre, amikor létrejön egy tó adattár fiók. Ebben az esetben a tulajdonos csoportot a fiók létrehozó felhasználó értéke.
* **Eset 2** (minden más esetben) – új elem létrehozásakor a tulajdonos csoport másolt-e a szülő mappából.

A tulajdonos csoport módosítható:
* Azok a felhasználók rendszer
* A tulajdonos felhasználó, ha a tulajdonos felhasználót is szerepel a cél csoport tagjának.

## <a name="access-check-algorithm"></a>Az Access jelölőnégyzet algoritmus

Az alábbi ábrán az access jelölőnégyzet algoritmus tó adattár fiókok jelöli.

![Tó adattár hozzáférés-vezérlési listák algoritmus](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="the-mask-and-effective-permissions"></a>A maszk és a "hatékony engedélyek"

A **maszk** értéke RWX, amellyel a **felhasználók nevű**, a **csoport tulajdonos**és **csoportok nevű** hozzáférés korlátozása, az Access ellenőrzése algoritmus végrehajtásakor. Az alábbiakban a maszk alapfogalmak. 

* A maszk "hatékony engedélyek" hoz létre, ez azt jelenti, hogy módosítja a engedélyek hozzáférése idején.
* A maszk fájl tulajdonosa, és a rendszer felhasználókat közvetlenül szerkeszthető.
* A maszk tartalmaz, az azt jelenti, hogy a hatékony engedélyt létrehozása-engedélyek eltávolítása. A maszk **nem sikerül** engedélyek hozzáadása a hatékony engedélyt. 

Tudassa velünk tekintse meg néhány példát talál. Az alábbi a maszk értéke **RWX**, ami azt jelenti, hogy, hogy a maszk nem távolítja el a szükséges engedélyeket. Figyelje meg, hogy elnevezett felhasználói, csoport tulajdonos és elnevezett csoport hatékony engedélyeit az access-ellenőrzése során nem változnak.

![Tó adattár hozzáférés-vezérlési listák](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

Az alábbi példa a maszk **R-X**értéke. Akkor ellenőrizze a **nevű felhasználói**, **csoport tulajdonos**és **neve csoport** hozzáférési időben **, azzal kikapcsolja azokat a írási engedélye** .

![Tó adattár hozzáférés-vezérlési listák](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

A hivatkozás az alábbiakban hol jelenik meg egy fájlt vagy mappát a maszk az Azure-portálon.

![Tó adattár hozzáférés-vezérlési listák](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

>[AZURE.NOTE] Új tó adattár fiókot a hozzáférés-vezérlés és az alapértelmezett vezérlés a legfelső szintű mappa ("/") a maszk RWX az alapértelmezés szerint.

## <a name="permissions-on-new-files-and-folders"></a>Új fájlok és mappák engedélyeinek

Amikor egy új fájlt vagy mappát a meglévő mappa jön létre, az alapértelmezett vezérlés a szülő mappára határozza meg:

* A gyermek mappa alapértelmezett vezérlés és a hozzáférés-vezérlés
* A gyermek fájl hozzáférés-vezérlés (fájlok nem rendelkezik egy alapértelmezett vezérlés)

### <a name="a-child-file-or-folders-access-acl"></a>A gyermek fájl vagy mappa hozzáférés-vezérlés

A gyermek fájl vagy mappa létrehozásakor a szülő alapértelmezett vezérlés másolja a program a gyermek fájl vagy mappa hozzáférés-vezérlés. Is ha **másik** felhasználó a szülő alapértelmezett vezérlés RWX engedélyekkel rendelkezik, akkor program teljesen eltávolítja a gyermek elem hozzáférés-vezérlés.

![Tó adattár hozzáférés-vezérlési listák](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

A legtöbb esetben a fenti információk kell kell tudni a gyermek elem hozzáférés-vezérlés hogyan határozzák meg. Jó helyen jár Ha ismeri a POSIX rendszerek, és szeretné érteni meg szeretné vizsgálni hogyan érhető el ez a transzformáció, című [létrehozásakor a hozzáférés-vezérlés új fájlok és mappák Umask a szerepkör](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) a jelen cikk.
 

### <a name="a-child-folders-default-acl"></a>A gyermek mappa alapértelmezett vezérlés

A gyermek-mappa létrehozása területen a szülő mappából a szülőmappát alapértelmezett vezérlés másolja a program fölé, mivel az, hogy a gyermek mappa alapértelmezett vezérlés.

![Tó adattár hozzáférés-vezérlési listák](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a>Haladó felhasználóknak a hozzáférés-vezérlési listák adatok tó áruház ismertetése

Az alábbiakban néhány speciális témakörök útmutatóból megtudhatja, hogyan határozzák meg hozzáférés-vezérlési listák tó adattár fájlok és mappák.

### <a name="umasks-role-in-creating-the-access-acl-for-new-files-and-folders"></a>A hozzáférés-vezérlés új fájlok és mappák létrehozásával Umask's szerepkör

POSIX-kompatibilis rendszer esetén a általános fogalma adott umask értéke 9-t a engedéllyel a **tulajdonos felhasználó**, **csoport tulajdonos**és **más** új gyermek fájl vagy mappa hozzáférés-vezérlés átalakításához használt szülő mappára. Egy umask bit azonosítása: mely bittel, ha ki szeretné kapcsolni a hozzáférés-vezérlés a gyermek elemet. Így célszerű használt nyomtatásból tulajdonos felhasználó, csoport, tulajdonos engedélyeinek terjesztését és más.
  
Egy Fájlrendszerhez rendszerben a umask az általában a webhely teljes szélességén konfigurációs lehetőség a rendszergazdák által vezérelt. Tó adattár használja- **fiók szintű umask** nem módosíthatók. A következő táblázat mutatja az adatok tó áruházból umask.

| Felhasználócsoport  | A beállítás | Új gyermekelem hozzáférés-vezérlés gyakorolt hatás |
|------------ |---------|---------------------------------------|
| Tulajdonos felhasználó | ---     | Nincs hatása                             |
| Tulajdonos csoport| ---     | Nincs hatása                             |
| Más       | RWX     | Távolítsa el az olvasás + írása + végrehajtása         | 

Az alábbi ábrán a umask működését. A nettó hatása az **olvasási + írása + végrehajtása** eltávolítása **más** felhasználó. Mivel a umask a **tulajdonos felhasználó** és a **csoport tulajdonos**bittel nem adta meg, ezeket az engedélyeket nem lesznek átalakítva.

![Tó adattár hozzáférés-vezérlési listák](./media/data-lake-store-access-control/data-lake-store-acls-umask.png) 

### <a name="the-sticky-bit"></a>A Beragadó bit

A Beragadó bit speciális jellemzi a POSIX fájlrendszer. Az adattár tó környezetben valószínű, hogy a Beragadó bit lesz szükség.

Az alábbi táblázatban látható, a Beragadó bit való működésének tó adattár.

| Felhasználócsoport         | Fájl    | Mappa |
|--------------------|---------|-------------------------|
| Öntapadós bit **kikapcsolása** | Nincs hatása   | Nincs hatása           |
| **A** Beragadó bit  | Nincs hatása   | Megakadályozza, hogy bárki, kivéve a **rendszer-felhasználók** és a **tulajdonos felhasználó** törlése és átnevezése a gyermek elemre a gyermek elemek.               |

A Beragadó bit nem látható az Azure-portálra.

## <a name="common-questions-for-acls-in-data-lake-store"></a>Gyakori kérdések a hozzáférés-vezérlési listák adatok tó tárolása

Az alábbiakban néhány kérdésre, amely a hozzáférés-vezérlési listák adatok tó áruház részletez gyakran lyncen.

### <a name="do-i-have-to-enable-support-for-acls"></a>Van a hozzáférés-vezérlési listák támogatásának engedélyezése?

nem. Hozzáférés-vezérlés keresztül hozzáférés-vezérlési listák értéke mindig a tó adattár fiókot.

### <a name="what-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a>Engedélyek szükségesek rekurzív törlése a mappát és tartalmát?

* A szülő mappából rendelkeznie kell **írni + végrehajtása**.
* A törölni kívánt mappát, és minden mappái szükséges **További + írása + végrehajtása**.
>[AZURE.NOTE] A fájlok, mappák törlése nem szükséges írási meg azokat a fájlokat. Emellett a legfelső szintű mappa "/" **soha nem** lehet törölni.

### <a name="who-is-set-as-the-owner-of-a-file-or-folder"></a>Fájl vagy mappa tulajdonosa ki van beállítva?

Fájl vagy mappa készítője tulajdonosává válik.

### <a name="who-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a>Ki van beállítva a tulajdonos csoportja egy fájlt vagy mappát hoz létre?

Másolja a szülőmappát, amelyek mellett az új fájlt vagy mappát hoz létre a tulajdonos csoportját.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Állok a tulajdonos felhasználó egy fájl, de nincs már szükség RWX engedélyeket. Mit tegyek?

A tulajdonos felhasználói egyszerűen módosíthatja az engedélyekkel engedélyezhetik maguk bármely RWX szükségük van a fájl.

### <a name="does-data-lake-store-support-inheritance-of-acls"></a>Tó adattár támogatja a hozzáférés-vezérlési listák öröklődése?

nem.

### <a name="what-is-the-difference-between-mask-and-umask"></a>Mi a különbség a maszk és umask között?

| a maszk | umask|
|------|------|
| A **maszk** tulajdonságra a minden fájl és mappa érhető el. | A **umask** tulajdonság tó adattár fiók. Igen van csak egyetlen umask az adatok tó áruházból.    |
| A maszk tulajdonságra, kattintson a fájl vagy mappa is változtatható meg a tulajdonos felhasználónak vagy egy fájlt vagy a rendszer felhasználói tulajdonos csoportját. | Minden olyan felhasználó, felügyelő felhasználó által a umask tulajdonsága nem módosítható. Azt értéke nem módosítható, állandó.|
| A maszk tulajdonságra szolgál a hozzáférése algoritmus futásidőben során határozza meg, hogy egy felhasználó rendelkezik-e a jobbra lévő fájl vagy mappa művelet végrehajtásához. A szerepkör a maszk "hatékony engedélyek" létrehozására, a hozzáférés-ellenőrzés idején. | A umask nem használatos Access-ellenőrzése során egyáltalán. A umask azt határozza meg, az Access vezérlés az új alárendelt elemeinek mappában. |
| A maszk értéke 3 bites RWX elnevezett felhasználói, az elnevezett csoport és a tulajdonos felhasználói hozzáférés-ellenőrzés alkalmával vonatkozik.| A umask 9 bit érték a tulajdonos felhasználói, csoport tulajdonos és más új gyermek vonatkozik.| 

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>Hol találok további információ a hozzáférés-vezérlési modell POSIX?

* [http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.HTML](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [Fájlrendszerhez jogosultsági útmutató](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) 

* [POSIX – GYAKORI KÉRDÉSEK](http://www.opengroup.org/austin/papers/posix_faq.html)

* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [POSIX 1003.1e 1997.](http://users.suse.com/~agruen/acl/posix/Posix_1003.1e-990310.pdf)

* [Linux POSIX vezérlés](http://users.suse.com/~agruen/acl/linux-acls/online/)

* [Használja a hozzáférés-vezérlési listákat Linux vezérlés](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Lásd még:

* [Azure tó adattár áttekintése](data-lake-store-overview.md)

* [Azure adatok tó Analytics – első lépések](../data-lake-analytics/data-lake-analytics-get-started-portal.md)





