<properties 
   pageTitle="A napló Analytics Syslog üzenetek |} Microsoft Azure"
   description="Syslog egy esemény Linux közös naplózás protokollnak.   Ez a cikk ismerteti, hogyan konfigurálása Syslog üzenetek csoportjára a napló Analytics és a rekordok hoznának létre a MOBILE adattárban részleteit."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />


# <a name="syslog-data-sources-in-log-analytics"></a>Log Analytics Syslog adatforrások

Syslog egy esemény Linux közös naplózás protokollnak.  Alkalmazások küld üzeneteket a helyi számítógépen tárolt vagy egy Syslog adatgyűjtő kézbesítve.  A MOBILE Agent Linux telepítve van, ha a helyi Syslog szolgáltatás üzenetek továbbítása a agent állítja be.  Ezután az ügynök elküldi az üzenet hol egy megfelelő rekordot hoz létre, a MOBILE adattárban napló Analytics.  

> [AZURE.NOTE]Log Analytics rsyslog vagy syslog-ng által küldött üzenetek csoportjára támogatja. Piros kalap vállalati Linux CentOS és Oracle Linux verzióját (sysklog) 5-ös verzióját alapértelmezett syslog démont syslog esemény webhelycsoport nem támogatott. Adatokat szeretne gyűjteni syslog az alábbi terjesztését verziójában, a [rsyslog démon](http://rsyslog.com) kell lennie telepítette és beállította sysklog cseréje.

![Syslog gyűjtemény](media/log-analytics-data-sources-syslog/overview.png)


## <a name="configuring-syslog"></a>Syslog konfigurálása
A MOBILE Agent Linux csak összegyűjti a felszerelések és a konfigurációban megadott severities események.  A MOBILE portálon keresztül, vagy a Linux ügynökök konfigurációs fájlok kezelése Syslog konfigurálhatja.


### <a name="configure-syslog-in-the-oms-portal"></a>A MOBILE portál Syslog beállítása

Állítsa be a [Analytics beállítások az adatok menü](log-analytics-data-sources.md#configuring-data-sources)Syslog.  Ebben a konfigurációban a konfigurációs fájl minden Linux Agent érkeznek.

Írja be a nevét, majd kattintás egy új lehetőség is hozzáadhat **+**.  Az egyes a lehetőség csak a kijelölt severities üzenetek lesznek összegyűjtve.  Jelölje be a severities az adott szolgáltatás, amellyel a gyűjtendő számára.  További feltételeket szűrése során nem adhat meg.

![Syslog konfigurálása](media/log-analytics-data-sources-syslog/configure.png)


Összes konfigurációs módosítást alapértelmezés szerint automatikusan tolni összes anyagokkal.  Syslog minden Linux Agent manuális beállítását tetszés majd törölje a jelet a *alkalmaz a Linux gépek konfigurációs alatti*mezőbe.


### <a name="configure-syslog-on-linux-agent"></a>Syslog Linux ügynökök beállítása

Ha a [MOBILE ügynök Linux ügyfél van telepítve](log-analytics-linux-agents.md), telepít egy alapértelmezett syslog konfigurációs fájl, amely meghatározza a létesítmény és az üzenetek gyűjtött súlyosságát.  Ez a fájlt, és módosíthatja a konfigurációs módosíthatók.  A beállítási fájl eltér attól függően, hogy a Syslog démon, hogy az ügyfél telepítve van.

> [AZURE.NOTE] Ha módosítja a syslog konfigurációja, a syslog démon a módosítások érvénybelépéséhez újra kell indítania.

#### <a name="rsyslog"></a>rsyslog

Rsyslog konfigurációs fájl **/etc/rsyslog.d/95-omsagent.conf**helyen található.  Az alapértelmezett tartalom alatt jelennek meg.  Ez a syslog üzeneteket a helyi ügynök az összes létesítményekhez figyelmeztetés vagy magasabb szintű gyűjti össze.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

A létesítmény eltávolíthatja a szakaszban a konfigurációs fájl eltávolításával.  Az egy adott lehetőség az, hogy létesítmény bejegyzés módosításával gyűjtött severities korlátozhatja.  Például a felhasználó létesítmény, üzenetekre a vagy újabb hiba súlyosságát korlátozni szeretné módosítania a konfigurációs fájl a következő sor:

    user.error  @127.0.0.1:25224


#### <a name="syslog-ng"></a>Syslog-ng

Rsyslog konfigurációs fájl **/etc/syslog-ng/syslog-ng.conf**helyen.  Az alapértelmezett tartalom alatt jelennek meg.  Ez a syslog üzeneteket a helyi ügynök minden lehetőséget, és az összes severities gyűjti össze.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };
    
    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };
    
    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };
    
    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };
    
    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };
    
    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };
    
    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

A létesítmény eltávolíthatja a szakaszban a konfigurációs fájl eltávolításával.  Korlátozhatja a severities, amely az egy adott létesítmény gyűjti össze a listából eltávolításukat.  Szeretné korlátozni a felhasználó funkció csak az értesítés és fontos üzeneteket, például, hogy a konfigurációs fájl a következő szakaszban módosítsa szeretné:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="changing-the-syslog-port"></a>A Syslog port módosítása

A port 25224 helyi ügyfélen Syslog üzenetek figyeli a MOBILE agent.  A port módosíthatja a következő szakasz hozzáadása a MOBILE ügynök konfigurációs fájl **/etc/opt/microsoft/omsagent/conf/omsagent.conf**helyen található.  A **port** bejegyzésben 25224 cserélje le a portszámot, amelyet.  Figyelje meg, hogy is szüksége lesz az üzeneteket küldeni a port Syslog szolgáltatás a konfigurációs fájl módosításához szükséges engedélyekkel.

    <source>
      type syslog
      port 25224
      bind 127.0.0.1
      protocol_type udp
      tag oms.syslog
    </source>


## <a name="data-collection"></a>Adatok gyűjtése

A port 25224 helyi ügyfélen Syslog üzenetek figyeli a MOBILE agent. A beállítási fájl esetében a Syslog démon Syslog üzeneteket a port, ahol azok által gyűjtött napló Analytics alkalmazásból továbbítja.


## <a name="syslog-record-properties"></a>Syslog rekord tulajdonságai párbeszédpanelen

Syslog rekordok **Syslog** típusú, és tárolja az a tulajdonságokat az alábbi táblázatban.

| A tulajdonság | Leírás |
|:--|:--|
| Számítógép | Számítógép, amely az esemény amelyet gyűjtött. |
| Létesítmény | Határozza meg, hogy a rendszer által generált az üzenet része. |
| HostIP | A rendszer az üzenetet küldő IP-címe.  |
| Hostname (állomásnév) | A rendszer az üzenetet küldő neve. |
| SeverityLevel | Az esemény súlyosságát szintjét. |
| SyslogMessage | Az üzenet szövegét. |
| ProcessID | A folyamat az üzenet létrehozott azonosítója. |
| EventTime | Dátum és idő, amely az esemény jött létre.



## <a name="log-queries-with-syslog-records"></a>Log lekérdezések Syslog rekordok

Az alábbi táblázat példákat különböző Syslog rekordjával napló lekérdezéseket.

| Lekérdezés | Leírás |
|:--|:--|
| Típus = Syslog | Az összes Syslogs. |
| Típus = Syslog SeverityLevel = hiba | Az összes Syslog rekordot hiba súlyosságát. |
| Típus = Syslog & #124; Mérje le a számítógép count() | A rekordok száma a Syslog számítógép. |
| Típus = Syslog & #124; Lehetőség szerint mérték count() | Létesítmény száma a Syslog rekordok. |

## <a name="next-steps"></a>Következő lépések

- [Log keresések](log-analytics-log-searches.md) megismerheti az adatforrások és megoldások gyűjtött adatok elemzése céljából. 
- [Egyéni mezők](log-analytics-custom-fields.md) használatával syslog rekord adatait értelmezhető egyes mezőket.
- [Állítsa be Linux ügynökök](log-analytics-linux-agents.md) más típusú adatokat gyűjt. 
