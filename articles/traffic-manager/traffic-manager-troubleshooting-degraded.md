<properties
    pageTitle="Hibaelhárítás a csökkent Azure forgalom Manager állapota"
    description="Hibák elhárítása forgalom Manager profilok azt mutatja, mint amikor csökkent állapotát."
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

# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Hibaelhárítás a csökkent Azure forgalom Manager állam

Ez a cikk ismerteti, hogyan kapcsolatos hibák elhárítása az Azure forgalom Manager profil csökkentett állapotot megjelenítő. Ebben az esetben érdemes megfontolni, hogy a szolgáltatott cloudapp.net szolgáltatások mutató forgalom Manager profil állította. Ha bejelöli forgalmának feletteséhez állapotának, láthatja, hogy az állapot teljesítménye csökkent.

![csökkentett állam](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degraded.png)

Ha megnyitja az adott profil végpontok lapja jelenik meg egy vagy több, a végpontok egy Offline állapotú:

![kapcsolat nélkül](./media/traffic-manager-troubleshooting-degraded/traffic-manager-offline.png)

## <a name="understanding-traffic-manager-probes"></a>Ellenőrzi a forgalom-kezelő ismertetése

- Adatforgalom menedzser zárólap ONLINE kell, csak akkor, ha a vizsgálati HTTP 200 választ kap vissza a vizsgálati elérési útját. Bármely más nem 200 válasz hiba.
- A 30 x átirányítást nem sikerül, még akkor is, ha az átirányított URL-cím egy 200 adja eredményül.
- A HTTPs szondákat tanúsítvány hibák figyelmen kívül hagyja.
- A tényleges tartalmat a vizsgálati elérési út nem számít, feltéve, hogy egy 200 értéket adja vissza. Egy URL-CÍMÉT, például néhány statikus tartalommá számlálása "/ favicon.ico" gyakori technika. Dinamikus tartalom, például a ASP lapok, előfordulhat, hogy nem mindig ad vissza 200, akkor is, ha az alkalmazás megfelelő.
- Ajánlott a vizsgálati elérési útjának beállítása olyanra, amely még elegendő logika megállapítható, hogy a webhely-e a felfelé vagy lefelé. Az előző példában, az elérési út beállításával "/ favicon.ico", csak Ön válaszol tesztelés, hogy w3wp.exe. Ez a vizsgálati is jelzi, hogy a webes alkalmazás megfelelő. Jobb választás lenne, például egy valamire adhatja meg egy elérési utat "/ Probe.aspx", amelynek logika a szolgáltatás állapotát jelző határozza meg. Például használhatja a Processzor kihasználtsági teljesítmény számláló vagy sikertelen kérések száma mérjük. Vagy az adatbázis-erőforrások vagy győződjön meg arról, hogy működik-e a webes alkalmazás a munkamenet állapota eléréséhez próbálkozhatnak.
- A profil összes végponton csökkent vannak, ha majd forgalom Manager kezeli minden, a megfelelő végpontok és útvonalak forgalom összes végpontokhoz. Ez a probléma biztosítja, hogy a vizsgálathoz használt mechanizmusa problémáit nem eredményeznek egy teljes üzemszünetek a szolgáltatás.

## <a name="troubleshooting"></a>Hibaelhárítás

Vizsgálati hiba elhárításához van szüksége, amely tartalmazza a HTTP-állapotkód visszatérési vizsgálati URL-CÍMÉT az eszköz. Vannak sok eszközök érhető el, amely mutatja, hogy a nyers HTTP választ.

* [Fiddler](http://www.telerik.com/fiddler)
* [curl](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Is használhatja az F12 hibakeresési eszközök Network lap az Internet Explorerben a HTTP válaszok megtekintése.

Ez a példa azt szeretné, hogy a válasz, a vizsgálati URL-: http://watestsdp2008r2.cloudapp.net:80/vizsgálati. Az alábbi PowerShell-példa bemutatja a problémát.

```powershell
    Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Példa kimenet:

```text
    StatusCode StatusDescription
    ---------- -----------------
            301 Moved Permanently
```

Figyelje meg, hogy az átirányítás választ kapott. Korábban már említettük, bármilyen StatusCode eltérő módon 200 tekintendő hibát. Adatforgalom Manager végpont állapotát Offline értékűre változik. A probléma megoldásához, jelölje be a webhely konfiguráció győződjön meg arról, hogy a megfelelő StatusCode a vizsgálati elérési adhatók vissza. A forgalom Manager vizsgálati mutasson egy elérési utat, amely visszaadja a 200 átkonfigurálása.

Ha a vizsgálati a HTTPS protokollt használ, előfordulhat, letiltása és a vizsgálat alatt az SSL/TLS-hibák elkerülése érdekében ellenőrzése tanúsítvány. Az alábbi PowerShell-kimutatások tanúsítvány érvényességi az aktuális PowerShell-munkamenet letiltása:

```powershell
    add-type @"
    using System.Net;
    using System.Security.Cryptography.X509Certificates;
    public class TrustAllCertsPolicy : ICertificatePolicy {
        public bool CheckValidationResult(
        ServicePoint srvPoint, X509Certificate certificate,
        WebRequest request, int certificateProblem) {
        return true;
        }
    }
    "@
    [System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a>Következő lépések

[Adatforgalom Manager forgalom útválasztási módjairól](traffic-manager-routing-methods.md)

[Mi az forgalom Manager](traffic-manager-overview.md)

[Cloud Services](http://go.microsoft.com/fwlink/?LinkId=314074)

[Azure Web Apps alkalmazások](https://azure.microsoft.com/documentation/services/app-service/web/)

[Műveletek a forgalom Manager (REST API-referencia)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure forgalom kezelő parancsmagok][1]

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
