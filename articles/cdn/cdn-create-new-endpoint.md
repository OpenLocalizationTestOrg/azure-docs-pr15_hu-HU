<properties
     pageTitle="Azure CDN használatával |} Microsoft Azure"
     description="Ez a témakör bemutatja, hogyan engedélyezhető az Azure tartalom kézbesítési hálózati (CDN). Az alábbiakban az oktatóprogram végpontot, illetve új CDN-profil létrehozása."
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="07/28/2016" 
     ms.author="casoper"/>

# <a name="using-azure-cdn"></a>Azure CDN használatával  

Ez a témakör az alábbiakban Azure CDN engedélyezése: hozzon létre egy új CDN-profil és végpontot.

>[AZURE.IMPORTANT] CDN működését, szolgáltatások listáját és bemutatása a a [CDN – áttekintés](./cdn-overview.md)című témakörben találhat.

## <a name="create-a-new-cdn-profile"></a>Hozzon létre egy új CDN-profil

CDN-profil CDN-végpontok gyűjteménye.  Az egyes profilok tartalmaz egy vagy több CDN-végpontok.  Előfordulhat, hogy használni kívánt több profil a CDN-végpontok internetes tartomány, webalkalmazás, vagy néhány egyéb feltétel szerinti rendszerezésére.

> [AZURE.NOTE] Alapértelmezés szerint nyolc CDN-profilok korlátozva egyetlen Azure előfizetést. CDN-profilokhoz tíz CDN-végpontok korlátozódik.
>
> A CDN-profil szintjén érvényes árak CDN. Ha ki szeretne árak Azure CDN-meghatározási kombinálja, több CDN-profil szüksége lesz.

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Hozzon létre egy új CDN-végpontot

**Új CDN végpont létrehozása**

1. Az [Azure-portálon](https://portal.azure.com)nyissa meg a CDN-profil.  Előfordulhat, hogy rendelkezik a kiemelt azt az előző lépésben az irányítópult.  Ha nem, akkor megtalálja: kattintson a **Tallózás gombra**, majd a **CDN-profilokat**, és a profiljában szeretne felvenni a végpontot.

    A CDN-profil lap jelenik meg.

    ![CDN-profil][cdn-profile-settings]

2. **Végpont hozzáadása** gombra.

    ![Végpont gomb felvétele][cdn-new-endpoint-button]

    A **Hozzáadás zárólap** lap jelenik meg.

    ![Végpont lap hozzáadása][cdn-add-endpoint]

3. Írja be egy **nevet** a CDN-végpont.  Ez a név szolgálnak a tartományban a gyorsítótárban tárolt forrásokat `<endpointname>.azureedge.net`.

4. Az **Origin típusa** legördülő adja meg a origin típusát.  Válassza ki a **tárhely** Azure tárterület-fiókkal, a **Felhőbeli szolgáltatástól** az Azure felhőalapú szolgáltatás, a **Web App** -Azure Web Appot, vagy az **egyéni origin** értéket más nyilvánosan hozzáférhető webhely kiszolgálói eredetű (Azure-ban vagy máshol üzemeltetett).

    ![CDN-origin típusa](./media/cdn-create-new-endpoint/cdn-origin-type.png)
        
5. A **Origin hostname (állomásnév)** tartalmazó legördülő listára válassza ki vagy írja be a origin tartománya.  A legördülő menü az összes elérhető forrásokból lépés: 4 megadott típusú felsorolásban.  Ha *egyéni origin* választotta, a **származási írja be**, a tartomány az egyéni származási adhatja.

6. **Origin elérési út** mezőbe írja be az elérési út az erőforrások gyorsítótár szeretne, vagy hagyja üresen a mezőt a 5 lépésben megadott tartomány minden erőforrás lehetővé teszi a gyorsítótár.

7. Az **Origin host fejléc**a host fejléc azt szeretné, hogy az egyes kérelem küldése a CDN, és az alapértelmezett távozó.

    > [AZURE.WARNING] Azure-tárhely és a Web Apps alkalmazások, például a forrásokból bizonyos típusú megkövetelése a host fejléc, a tartomány, a származási megfelelően. Csak ha rendelkezik az origin eltér a saját tartomány host élőfej igénylő, hagyja az alapértelmezett értéket.

8. **Protokoll** és **Origin port**adja meg a protokollok és az erőforrások, a származási helyen eléréséhez használható portok.  Legalább egy protocol (HTTP vagy HTTPS) kell kijelölni.
    
    > [AZURE.NOTE] Az **Origin port** csak hatással van, mit port az adatok kinyerése a origin végpont segítségével.  A végpont, magát csak akkor érhető el az alapértelmezett HTTP vége ügyfelek és HTTPS portokat (80 és 443-as), a **származási port**függetlenül.  
    >
    > **Azure CDN Akamai a** végpontokat nem engedélyezik a teljes TCP porttartományt forrásokból.  Az origin portok nem áll rendelkezésre, című témakör [Akamai engedélyezett Origin portokat az Azure CDN-t](https://msdn.microsoft.com/library/mt757337.aspx).  
    >
    > CDN elérése HTTPS tartalomhoz rendelkezik az alábbi korlátozások:
    > 
    > - Az a CDN által biztosított SSL-tanúsítvány kell használnia. Harmadik fél tanúsítványok nem támogatottak.
    > - A CDN által biztosított tartományt kell használnia (`<endpointname>.azureedge.net`) access HTTPS-tartalom. HTTPS-támogatás nem érhető el egyéni tartománynevek (CNAME), mivel az a CDN nem támogatja az egyéni tanúsítványok adott időben.

9. A **Hozzáadás** gombra kattintva hozza létre az új végpontot.

10. Amikor létrejött a végpontot, jelennek meg a profil végpontok listáját. A lista nézet eléréséhez a gyorsítótárban tárolt tartalmat, valamint az origin tartományt használni az URL-cím látható.

    ![CDN-végpontok][cdn-endpoint-success]

    > [AZURE.IMPORTANT] Az végpontot nem azonnal lesz elérhetővé válik felhasználásra, mint a szülőtől keresztül a CDN regisztráció időt vesz igénybe.  <b>Azure CDN Akamai a</b> profilok propagálása általában befejezi egy percen belül.  <b>Azure CDN Verizon a</b> profilok propagálása 90 percen belül általában befejeződik, de bizonyos esetekben több időt vesz igénybe.
    >    
    > A felhasználók, akik a CDN tartománynév használatára, a végpontok konfigurációja propagálása a POP-okhoz előtt próbálkozzon a HTTP 404-es választ kódok kap.  Ha több óra már létrehozott a végpontot, és továbbra is érkeznek 404-es válaszokat, című [404-es állapotok visszaadó hibaelhárítási CDN végpontok](cdn-troubleshoot-endpoint.md).


##<a name="see-also"></a>Lásd még:
- [A lekérdezési karakterlánc kérések gyorsítótárazási viselkedését szabályozása](cdn-query-string.md)
- [Hogyan CDN-tartalom hozzárendelése az egyéni tartomány](cdn-map-content-to-custom-domain.md)
- [Azure CDN-végpont eszközök előre betöltése](cdn-preload-endpoint.md)
- [Azure CDN zárólap törlése](cdn-purge-endpoint.md)
- [Hibaelhárítás 404-es állapotok visszaadó CDN-végpontok](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
