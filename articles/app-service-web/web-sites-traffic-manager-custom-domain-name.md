<properties
    pageTitle="Azure alkalmazás szolgáltatásban, a terheléselosztás forgalom Manager használó egyéni tartománynevet a webalkalmazás konfigurálása"
    description="Az egyéni tartománynév használata az Azure alkalmazás szolgáltatás, amely tartalmazza a forgalom Manager terheléselosztás webalkalmazást."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Egyéni tartománynevet a webalkalmazás konfigurálása forgalom-kezelővel Azure App szolgáltatásban

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[AZURE.INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Ez a cikk ismerteti általános egyéni tartománynév használata az Azure alkalmazás szolgáltatás, amelyek a forgalom kezelővel terheléselosztás.

[AZURE.INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>
## <a name="understanding-dns-records"></a>DNS-rekordok ismertetése

[AZURE.INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>
## <a name="configure-your-web-apps-for-standard-mode"></a>A web Apps alkalmazások szabványos üzemmódban konfigurálása

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>
## <a name="add-a-dns-record-for-your-custom-domain"></a>Az egyéni tartományához tartozó DNS-rekord hozzáadása

> [AZURE.NOTE] Ha vásárolt tartomány Azure alkalmazás szolgáltatás Web Apps alkalmazások – ugorja át, az alábbi lépések, és a [Tartomány vásárlása Web Apps alkalmazások](custom-dns-web-site-buydomains-web-app.md) cikk utolsó lépéseként hivatkozik.

Az egyéni tartomány társítása webalkalmazást Azure App szolgáltatásban, fel kell vennie egy új bejegyzés a DNS-táblázatban az egyéni tartományához tartozó a tartományregisztrálónál, a tartomány nevét a megvásárolt termékkulcsot által biztosított eszközök segítségével. Kövesse az alábbi lépéseket a megkereséséhez, és a DNS-eszközöket használja.

1. Bejelentkezés a fiókjába a tartományregisztrálónál, és keresse meg a DNS-rekordok kezelésére szolgáló lap. Keresse meg a hivatkozások vagy a **Tartománynevet**, a **DNS-**vagy a **Név kiszolgáló kezelése**felirata webhely területeket. Erre a lapra mutató hivatkozás található gyakran a fiókadatok megtekintésre és egy hivatkozást, például **a tartományok**majd keres.

1. Miután megtalálta a kezelése lap a tartomány neve, keresse meg a hivatkozás, amellyel módosíthatja a DNS-rekordjait. Ez lehet, hogy szerepel, a **Zone file** **DNS-rekordjait**, vagy egy **Speciális** beállításai hivatkozásra.

    * Az oldal valószínűleg lesz hozta létre, például egy bejegyzés társítása néhány rekordot "**@**"vagy"\*" egy "tartomány rögzítő" lappal. Rekordok közös alszint tartományok, például a **www-t**is tartalmazhat.
    * Az oldal említeni **További CNAME rekordot**, vagy adja meg a legördülő listában jelölje ki a rekord típusát. Azt is említeni más rekordok, például **A rekordok** és az **MX rekordok**is. Egyes esetekben CNAME rekordok más nevek, például az **Alias rekord**neve.
    * A lap is, amely lehetővé teszi a **térképre** **állomásnév** vagy **tartománynév** egy másik tartománynév mezőket.

1. Részletei határozzák meg, minden egyes tartományregisztrálója változnak, miközben az általános leképezheti *az* egyéni tartománynevét (például **contoso.com**,) *szeretné* a forgalmat Manager tartománynév (**contoso.trafficmanager.net**), amellyel a web App alkalmazásban.

    > [AZURE.NOTE] Azt is megteheti Ha egy bejegyzéshez már használatban van, és preemptively kötést létrehozni az alkalmazások rá szüksége, létrehozhat egy további CNAME rekordot. Ha például **www.contoso.com** preemptively kapcsolni a web App alkalmazásban, hozzon létre egy CNAME rekordot **awverify.www** **contoso.trafficmanager.net**. Majd felveheti "www.contoso.com" a Web App alkalmazásban a "www" CNAME rekord módosítása nélkül. További tudnivalókért lásd: a [DNS-rekordok létrehozása az egyéni tartomány egy webalkalmazás][CREATEDNS].

1. Amikor befejezte a hozzáadása vagy módosítása a DNS-rekordjait a tartományregisztrálónál, mentse a módosításokat.

<a name="enabledomain"></a>
## <a name="enable-traffic-manager"></a>Adatforgalom Manager engedélyezése

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Következő lépések

További tudnivalókért lásd: a [Node.js Developer Center](/develop/nodejs/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
