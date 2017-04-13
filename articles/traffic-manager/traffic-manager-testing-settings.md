<properties
    pageTitle="Adatforgalom kezelő beállításainak tesztelése |} Microsoft Azure"
    description="Ez a cikk segít forgalom kezelő beállításainak tesztelése"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="test-your-traffic-manager-settings"></a>Tesztelje a forgalom Manager

Tesztelje a forgalom Manager, szüksége van több ügyfél, a különböző helyeken, amelyből futtatását is lehetővé teszi a vizsgálatok. Ezután hozása a forgalom Manager profil lefelé egyesével a végpontok.

* A DNS-TTL értékének beállítása alacsony, hogy a módosítások propagálása gyorsan (például az 30 másodperc)
* Tudnivalók az Azure felhőszolgáltatások és a webhelyek a profil tesztelése a IP-címét.
* Az eszközöket, amelyekkel DNS nevét úgy, hogy IP-címet, és megjeleníti, hogy a cím használhatja.

Látható, hogy a DNS-nevek úgy, hogy a profil végpont IP-címek ellenőrizni. A neveket a forgalmat a forgalom Manager profilban lévő definiált útválasztási módszer megfelel módon megoldható. Az eszközök, például **az nslookup** , illetve **alaposabban meg** DNS-névfeloldás is használhatja.

Az alábbi példák tesztelje a forgalom Manager profil nyújt segítséget.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Jelölje be az nslookup és az ipconfig használata Windows forgalom Manager profil

1. Nyisson meg egy parancs vagy a Windows PowerShell-parancssorában rendszergazdaként.
2. Típus `ipconfig /flushdns` a DNS-feloldó gyorsítótárának kiürítése.
3. Típus `nslookup <your Traffic Manager domain name>`. Annak vizsgálata, például a következő parancsot a tartománynevét az előtag *myapp.contoso*

        nslookup myapp.contoso.trafficmanager.net

    Egy tipikus eredmény az alábbi információkat jeleníti meg:

    * A DNS-nevét és IP-címet a DNS-kiszolgáló a forgalom Manager tartománynév feloldásához használatban.
    * A forgalom Manager tartománynév adta meg a parancssorban "nslookup" és az IP-címet, amelyre a forgalom Manager tartomány úgy oldja fel az után. Ellenőrizze, hogy fontos a második IP-címe lesz. Meg kell egyeznie a felhőszolgáltatások és a webhelyek tesztelése forgalom Manager profilban egyik nyilvános virtuális IP (virtuális) címet.

## <a name="how-to-test-the-failover-traffic-routing-method"></a>Hogyan ellenőrizheti a feladatátvevő forgalom útválasztási módszer

1. Minden végpontok hagyja meg.
2. Egyetlen ügyfelet használ, kérheti a DNS-feloldási az nslookup vagy hasonló segédprogram, amely a vállalat tartománynevét.
3. Győződjön meg arról, hogy a megfeleltetett IP-címek megfelel-e az elsődleges végpontot.
4. Az elsődleges végpont leállíthatja, vagy úgy, hogy a forgalmat Manager lapon, hogy az alkalmazás nem működik, távolítsa el a megfigyeléssel fájlt.
5. Várja meg a DNS Time-to-Live (TTL) a forgalom Manager profil plusz két perc további. Ha például ha a DNS-TTL 300 másodperc (5 perc), meg kell várnia hét perc.
6. Ürítse ki a DNS ügyfél gyorsítótár és kérelem DNS-feloldási nslookup használatával. A Windows rendszerben ürítse ki a DNS-gyorsítótárat a ipconfig/flushdns paranccsal.
7. Győződjön meg arról, hogy a megfeleltetett IP-címek megfelel-e a másodlagos végpontot.
8. Ismételje meg a folyamat leállítása az egyes végpontot. Győződjön meg arról, hogy a DNS adja eredményül a következő végpont IP-címét a listában. Ha az összes végpontok lefelé, újra kell beszerzése az elsődleges végpontot IP-címét.

## <a name="how-to-test-the-weighted-traffic-routing-method"></a>Hogyan ellenőrizheti a súlyozott forgalom útválasztási módszer

1. Minden végpontok hagyja meg.
2. Egyetlen ügyfelet használ, kérheti a DNS-feloldási az nslookup vagy hasonló segédprogram, amely a vállalat tartománynevét.
3. Győződjön meg arról, hogy a megfeleltetett IP-címek megegyezik egy, a végpontok.
4. A DNS-ügyfél gyorsítótárának kiürítése, és ismételje meg a 2 és 3 minden végpontot. Meg kell jelennie a végpontok vissza különböző IP-címeket.

## <a name="how-to-test-the-performance-traffic-routing-method"></a>Hogyan ellenőrizheti a teljesítmény forgalom útválasztási módszer

A teljesítmény forgalom útválasztási mód hatékony teszteléséhez, amely a világ különböző részein ügyfelek kell rendelkeznie. Tesztelje a szolgáltatások használható különböző Azure régiókban ügyfelek hozhat létre. Ha globális hálózaton, távolról jelentkezzen be a világ más részein ügyfelek és a vizsgálatok futtatása onnan.

Másik lehetőségként vannak ingyenes, webalapú DNS keresése és alaposabban elérhető szolgáltatásokat. Egyes eszközök adni, akkor jelölje be a DNS-névfeloldás a világ különböző helyről. Végezze el a Keresés a "DNS keresés" példákat. Külső szolgáltatások, például Gomez vagy konferencián ellenőrizze, hogy a profilok vannak való terjesztésére forgalom is használható.

## <a name="next-steps"></a>Következő lépések

* [Adatforgalom Manager forgalom útválasztási módjairól](traffic-manager-routing-methods.md)
* [Adatforgalom Manager teljesítménnyel kapcsolatos szempontok](traffic-manager-performance-considerations.md)
* [Hibaelhárítási forgalom Manager csökkent állam](traffic-manager-troubleshooting-degraded.md)




