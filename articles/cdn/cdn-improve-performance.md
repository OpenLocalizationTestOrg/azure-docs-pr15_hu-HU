<properties
    pageTitle="Teljesítmény javítása Azure CDN tömörítésével |} Microsoft Azure"
    description="Megtudhatja, hogy miként fájl továbbítása sebesség javítása és lap betöltés teljesítményét növeli az Azure CDN tömörítésével."
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
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="improve-performance-by-compressing-files"></a>Teljesítmény javítása tömörítésével

A tömörítési a fájl továbbítása sebesség javítása és a lap betöltés teljesítmény növelése érdekében a kiszolgálóról elküldés előtt fájlméretének csökkentésével egyszerű és hatékony módszer. Csökkenti a sávszélesség-költségek és a felhasználók gyorsabb felületet nyújt.

Kétféleképpen tömörítés engedélyezése:

- A tömörítési engedélyezheti a forráskiszolgálóval, amelyben esetben a CDN a tömörített fájlok közötti sikeres lesz, és tömörített kézbesítése őket kérő ügyfeleknek.
- Lehetősége van engedélyezni a tömörítési közvetlenül a CDN peremhálózat-kiszolgálói, az, amelyben a esetben a CDN tömöríti a fájlokat és szolgálnak, a felhasználóknak, még akkor is, ha a forráskiszolgálóval nem tömöríti lesz.

> [AZURE.IMPORTANT] CDN-beállítások módosítása szánjon némi időt propagálja a hálózaton keresztül.  <b>Azure CDN Akamai a</b> profilok propagálása általában egy perc alatt egészíti ki.  <b>Azure CDN Verizon a</b> profilok általában látni fogja a módosítások alkalmazása 90 percen belül.  Ha most először be van állítva a tömörítési az a CDN-végpontot, vegye figyelembe, hogy a tömörítési beállítások a POP, mielőtt hibaelhárítási van propagálja 1 – 2 órán Várakozás

## <a name="enabling-compression"></a>Tömörítés engedélyezése

> [AZURE.NOTE] A szokásos és prémium CDN-meghatározási adja meg a tömörítési ugyanazokat a szolgáltatásokat, de a felhasználói felület működik.  Normál és prémium CDN-meghatározási közötti különbségekről olvashat az [Azure CDN áttekintése](cdn-overview.md)című témakörben találhat.

### <a name="standard-tier"></a>Szabványos szint

> [AZURE.NOTE] Ez a szakasz **Azure CDN szabványos a Verizon** és **Azure CDN szabványos Akamai a** profilok vonatkozik.

1. Kattintson a CDN-profil lap a CDN-végpont kezelni szeretné.

    ![CDN-profil lap végpontok](./media/cdn-file-compression/cdn-endpoints.png)

    Ekkor megnyílik a CDN-végpontot lap.

2. Kattintson a **Konfigurálás** gombra.

    ![CDN-profil lap kezelése gomb](./media/cdn-file-compression/cdn-config-btn.png)

    Az a CDN-konfigurációs lap megnyitása

3. Kapcsolja be a **tömörítés**.

    ![CDN-tömörítési beállítások](./media/cdn-file-compression/cdn-compress-standard.png)

4. Az alapértelmezett típusok használja, vagy módosítsa a listáját, eltávolításával vagy fájltípusok hozzáadásával.
    
    > [AZURE.TIP] Lehetséges, közben nem ajánlott tömörítés alkalmazása tömörített formátumokat, például az IRÁNYÍTÓSZÁM, MP3, MP4, JPG stb.
    
5. A módosítások elvégzése után kattintson a **Mentés** gombra.

### <a name="premium-tier"></a>Réteg prémium verzió

> [AZURE.NOTE] Ez a szakasz **Azure CDN prémium Verizon a** profilok vonatkozik.

1. Az a CDN a profil lap a **kezelés** gombra.

    ![CDN-profil lap kezelése gomb](./media/cdn-file-compression/cdn-manage-btn.png)

    Ekkor megnyílik a CDN kezelőportálja segítségével.

2. Mutasson a **HTTP nagy** fülre, majd mutasson a **Gyorsítótár beállításainak** előugró.  Kattintson a **tömörítést**.

    A tömörítési beállítások is megjelennek.

    ![Fájl tömörítés](./media/cdn-file-compression/cdn-compress-files.png)

3. Tömörítés engedélyezése a **Tömörítés engedélyezve** választógomb gombra kattintva.  Írja be a MIME típusú tömörítése listaként vesszővel tagolt (szóköz nélkül) a **Fájltípus** mezőben lévő értéket szeretné.
        
    > [AZURE.TIP] Lehetséges, közben nem ajánlott tömörítés alkalmazása tömörített formátumokat, például az IRÁNYÍTÓSZÁM, MP3, MP4, JPG stb. 

4. A módosítások elvégzése után kattintson a **frissítés** gombra.


## <a name="compression-rules"></a>A tömörítési szabályok

Az alábbi táblázat az Azure CDN tömörítés viselkedése minden esetben ismertetik.

> [AZURE.IMPORTANT] Az **Azure CDN a Verizon** (normál és Premium) csak akkor jogosult, ha a fájlok vannak tömörítve.  Tömörítés vehetők fájl kell végrehajtania:
>
> - Lehet nagyobb, mint 128 bájt.
> - 1 MB-nál kisebb lehet.
> 
> **Azure CDN Akamai származó**, az összes fájl jogosultak tömörítés.
>
> Az összes Azure CDN-termékek fájl MIME típusú, amely már [be van állítva a tömörítési](#enabling-compression)kell lennie.
>
> **Azure CDN Verizon a** profilok (normál és Premium) **gzip**, **Homorú**vagy **bzip2** kódolást támogatja.  **Akamai az Azure CDN** -profilok csak akkor támogatja a **gzip** kódolást.
>
> **Azure CDN Akamai a** végpontok mindig kérése az origin, függetlenül attól, az ügyfél kérése **gzip** kódolva fájlokat.

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Tömörítés tiltva vagy a fájl nem jogosult a tömörítési

|Az ügyfél kért formátum (keresztül elfogadás kódolás fejléc)|Gyorsítótárazott fájlformátum|Az ügyfél CDN-válasz|Jegyzetek|
|----------------|-----------|------------|-----|
|Tömörített|Tömörített|Tömörített|   |
|Tömörített|Tömörítés nélkül|Tömörítés nélkül|    | 
|Tömörített|Nem gyorsítótáras|Próbálta, tömörítve vagy tömörítés nélkül|Válasz origin függ|
|Tömörítés nélkül|Tömörített|Tömörítés nélkül|    |
|Tömörítés nélkül|Tömörítés nélkül|Tömörítés nélkül|    |   
|Tömörítés nélkül|Nem gyorsítótáras|Tömörítés nélkül|     |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Tömörítés engedélyezve fájl pedig jogosult tömörítés

|Az ügyfél kért formátum (keresztül elfogadás kódolást fejléc)|Gyorsítótárazott fájlformátum|Az ügyfél CDN-válasz|Jegyzetek|
|----------------|-----------|------------|-----|
|Tömörített|Tömörített|Tömörített|CDN-transcodes támogatott formátumok között|
|Tömörített|Tömörítés nélkül|Tömörített|CDN tömörítés hajt végre.|
|Tömörített|Nem gyorsítótáras|Tömörített|CDN tömörítés hajt végre, ha origin nem tömörített adja eredményül.  **Azure CDN Verizon a** át a nem tömörített fájlt az első kérésre és majd tömörítése és a fájl ezután valaki gyorsítótár.  A fájlok `Cache-Control: no-cache` élőfej soha nem lehet tömöríteni. 
|Tömörítés nélkül|Tömörített|Tömörítés nélkül|CDN kibontási hajt végre.|
|Tömörítés nélkül|Tömörítés nélkül|Tömörítés nélkül|     |  
|Tömörítés nélkül|Nem gyorsítótáras|Tömörítés nélkül|     |

## <a name="media-services-cdn-compression"></a>Media Services CDN tömörítés

A Media Services CDN adatfolyam végpontok engedélyezve, a tömörítési a következő tartalomtípusok alapértelmezés szerint engedélyezve van-e: alkalmazás/vnd.ms-sstr xml, application/dash+xml,application/vnd.apple.mpegurl, alkalmazás/f4m + xml. Ön nem engedélyezhetik/letilthatják az Azure portálon említett fájltípusok tömörítés.  

## <a name="see-also"></a>Lásd még:
- [CDN-fájl tömörítés – hibaelhárítás](cdn-troubleshoot-compression.md)    
