<properties
    pageTitle="Azure CDN fájl tömörítése hibaelhárítása |} Microsoft Azure"
    description="Azure CDN-fájl tömörítése kapcsolatos problémák megoldása."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-file-compression"></a>CDN-fájl tömörítés – hibaelhárítás

Ez a cikk segít [CDN](cdn-improve-performance.md)-fájl tömörítése kapcsolatos problémák megoldása.

Ha ez a cikk bármely pontján további segítségre van szüksége, kapcsolatba léphet az Azure szakértői [az MSDN Azure és a túlcsordulás Papírhalom fórumokon](https://azure.microsoft.com/support/forums/). Másik lehetőségként az Azure támogatási kérelmeiket is küldhet. Nyissa meg a [Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) , és kattintson a **Támogatás első**.

## <a name="symptom"></a>A jelenség

A végpont tömörítés engedélyezve van, de nem tömörített fájlok vannak visszaküldött.

>[AZURE.TIP] Annak ellenőrzéséhez, hogy a fájlok küldése a visszaadott tömörített, kell például [Fiddler](http://www.telerik.com/fiddler) vagy a böngésző [Fejlesztőeszközök](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)eszköz használata.  Jelölőnégyzet a HTTP-válasz fejlécek vissza a gyorsítótárban tárolt CDN tartalom.  Ha van-e a fejléc, más néven `Content-Encoding` **gzip**, **bzip2**vagy **Homorú**érték, a tartalom tömörítve van.
>
>![Tartalom-kódolás élőfej](./media/cdn-troubleshoot-compression/cdn-content-header.png)

## <a name="cause"></a>OK

Nincs több oka lehet, például:

- A kívánt tartalmat, nem jogosult a tömörítési.
- A tömörítési engedélyezve van a kívánt fájl típusát.
- A HTTP-kérés nem vette fel egy érvényes tömörítési típus kérő fejlécet.

## <a name="troubleshooting-steps"></a>Hibaelhárítási lépések

> [AZURE.TIP] Ahogy az új végpontok telepít, az CDN-beállítások módosítása szánjon némi időt propagálja a hálózaton keresztül.  Általában a módosítások 90 percen belül érvényesek.  Ha az első alkalommal be van állítva a tömörítési az a CDN-végpontot, vegye figyelembe, hogy a tömörítési beállítások a POP van propagálja 1 – 2 órán várakozás. 

### <a name="verify-the-request"></a>A kérés ellenőrzése

Első lépésként a kérelem gyors megerősítést ellenőrzése tegye azt.  A kérések éppen megtekintése a böngészőben [a Fejlesztőeszközök](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) is használhatja.

- Ellenőrizze, hogy a kérelem küldi el a végpontot URL `<endpointname>.azureedge.net`, és nem az origin.
- Ellenőrizze a kérelem tartalmaz egy **Elfogadás kódolást** fejléc, az értéket, hogy az élőfej **gzip**, **Homorú**vagy **bzip2**tartalmazza.

> [AZURE.NOTE] **Akamai az Azure CDN** -profilok csak akkor támogatja a **gzip** kódolást.

![CDN-kérés fejlécek](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Ellenőrizze a tömörítési beállítások (normál CDN-profil)

> [AZURE.NOTE] Ebben a lépésben csak érvényes, ha CDN-profilját az **Azure CDN szabványos a Verizon** és **Azure CDN szabványos Akamai a** profil. 

Nyissa meg azt a végpontot, az [Azure-portálra](https://portal.azure.com) , és kattintson a **Konfigurálás** gombra.

- Ellenőrizze, hogy engedélyezve van a tömörítést.
- Ellenőrizze, hogy a MIME típus tartalmát lehet tömöríteni található tömörített formátumok listája.

![CDN-tömörítési beállítások](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Ellenőrizze a tömörítési beállítások (prémium CDN-profil)

> [AZURE.NOTE] Ebben a lépésben csak érvényes, ha CDN-profilját az **Azure CDN prémium Verizon a** profil.

Nyissa meg azt a végpontot, az [Azure-portálra](https://portal.azure.com) , és kattintson a **kezelés** gombra.  A kiegészítő portál nyílik meg.  Mutasson a **HTTP nagy** fülre, majd mutasson a **Gyorsítótár beállításainak** előugró.  Kattintson a **tömörítés**gombra. 

- Ellenőrizze, hogy engedélyezve van a tömörítést.
- Ellenőrizze, hogy a **Fájltípus** listában vesszővel tagolt listáját tartalmazza (szóköz nélkül) MIME típusú.
- Ellenőrizze, hogy a MIME típus tartalmát lehet tömöríteni található tömörített formátumok listája.

![CDN prémium tömörítési beállítások](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a>Ellenőrizze, hogy a tartalom gyorsítótárazott

> [AZURE.NOTE] Ebben a lépésben csak érvényes, ha CDN-profilját az **Azure CDN Verizon a** profil (normál vagy prémium verzióban).

A böngésző fejlesztői eszközökkel, jelölje be a választ fejlécek annak érdekében, hogy a régió, ahol kérik tárolja a rendszer a fájlt.

- Jelölje be a **kiszolgáló** válasz fejléc.  A fejléc van, hogy a formátum **Platform (POP-kiszolgáló-azonosító)**, az alábbi példában látható módon.
- Jelölje be az **X-gyorsítótár** válasz fejléc.  A fejléc **TALÁLATI**szól.  

![CDN-válasz fejlécek](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a>Ellenőrizze, hogy a fájl megfelel mérete

> [AZURE.NOTE] Ebben a lépésben csak érvényes, ha CDN-profilját az **Azure CDN Verizon a** profil (normál vagy prémium verzióban).

Tömörítés jogosult fájl kell az alábbi követelményeknek mérete:

- Nagyobb, mint 128 bájt.
- 1 MB-nál kisebb.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Ellenőrizze a kérést, a származási kiszolgálón **keresztül** fejléchez

A **Via** HTTP fejléc érték azt jelzi az érintett webkiszolgálóra által a proxykiszolgáló átadott a kérést.  Alapértelmezés szerint a Microsoft IIS-webkiszolgálón tömöríti a válaszokat a kérelmet tartalmazó **keresztül** fejlécet.  Ez a működési módjának felülbírálására, tegye a következőket:

- **IIS 6**: [beállítása HcNoCompressionForProxies = "FALSE" az IIS tulajdonságot](https://msdn.microsoft.com/library/ms525390.aspx)
- **IIS 7-es és legfeljebb**: [ **noCompressionForHttp10** és a **noCompressionForProxies** értéke HAMIS, a kiszolgáló konfigurációjának beállítása](http://www.iis.net/configreference/system.webserver/httpcompression)

