<properties
   pageTitle="Piros kalap frissítési infrastruktúra (RHUI) |} Microsoft Azure"
   description="További tudnivalók a piros kalap frissítés infrastruktúra (RHUI) igény szerinti Red Hat Enterprise Linux előfordulását Microsoft Azure-ban"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="BorisB2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="borisb"/>

# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Piros kalap frissítési infrastruktúra (RHUI) igény szerinti piros kalap vállalati Linux VMs Azure-ban

Elérheti a piros kalap frissítés infrastruktúra (RHUI) telepítését az Azure virtuális gépeken futó igény szerinti piros kalap vállalati Linux (RHEL) képének érhető el a Microsoft Azure piactéren létrehozott regisztrált.  Az igény szerinti RHEL példányokat egy területi yum tárházba és egyre növekvő tendenciát mutat frissítések érkeznek hozzáférést.

A tárházba RHUI kezeli, yum listában kiépítési során a RHEL-példányban van beállítva. Nem kell tennie bármilyen további beállítási - futtatása `yum update` után a RHEL példány készen áll a legújabb frissítéseket.

> [AZURE.NOTE] Azure RHUI infrastruktúra nemrégiben frissítette (szeptember 2016), és zavartalan a hozzáférést az Azure RHUI megköveteli a módosításokat a konfigurációban a meglévő RHEL példányainak. Keresse meg a RHUI Azure infrastruktúrájának szakaszban további információt.


## <a name="rhui-azure-infrastructure-update"></a>Azure infrastruktúrájának RHUI
Azure kezdve szeptember 2016, új piros kalap frissítés infrastruktúra (RHUI) kiszolgálók rendelkezik. Ezeket a kiszolgálókat az [Azure-forgalmat kezelővel]( https://azure.microsoft.com/services/traffic-manager/) telepítik, hogy egyetlen végpont (rhui-1.micrsoft.com) bármely virtuális régió függetlenül is használhatják. Is használhatja az SSL-tanúsítvány, amely láncolva van-e a jól ismert hitelesítésszolgáltató (Egerben root). A frissítés automatikus végrehajtása lenne veszélyes egyes ügyfelek, amelyben hozzáférés-vezérlési listák vagy egyéni útválasztási táblázatok RHUI frissítés kiszolgálók, tehát a frissítés "kiválaszthatják-." Az alábbi új kiszolgálókon bevezetési manuális erre a lapra, és egy teljes parancsfájlt bevezetési (esetén az egyes lépések ellenőrzés) automatikus módon elérhetők. A Microsoft Azure piactéren (dátummal szeptember 2016 vagy újabb verzió) új RHEL PAYG képek fog automatikusan mutasson az új Azure RHUI kiszolgálók, és nem szükséges bármely további művelet.

### <a name="the-new-azure-rhui-infrastructure-onboarding-timeline"></a>Az új Azure RHUI infrastruktúra bevezetési ütemterv

| Dátum | Megjegyzés: |
| --- | --- |
|2016 szeptember 22 | RHUI kiszolgálók és a rendelkezésre álló telepítési utasításokat. Használja az új (szeptember 2016 dátummal) rendszerbe VMs RHEL PAYG piactér képek automatikusan használja, az új RHUI kiszolgálók, azonban meglévő VMs "részvétel"
|November 1 2016 | A Microsoft Azure piactéren gyűjtemény kikerül régebbi RHEL PAYG virtuális-képek (amely a régi Azure RHUI kiszolgálók használata)
|2017 január 16 | A régi Azure RHUI kiszolgálók fog kell leszerelése. Frissítse az összes a szóban forgó PAYG RHEL VMs ez alkalommal Azure RHUI való hozzáférés kezelése

### <a name="the-ips-for-the-new-rhui-content-delivery-servers-are"></a>Az IP-címei az új RHUI tartalomkézbesítési kiszolgálók vannak

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193
```

### <a name="manual-update-procedure-to-use-the-new-azure-rhui-servers"></a>Kézi frissítés eljárással az új Azure RHUI kiszolgálók használata

A nyilvános kulcs aláírás (keresztül curl) letöltése

```
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Ellenőrizze a letöltött billentyűt

```
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

Ellenőrizze, jelölje be a kimenet `keyid` és `user ID packet`:

```
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

A nyilvános kulcs telepítése

```
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

Töltse le a ellenőrzése és RPM ügyfélcsomag telepítése

Letöltés: Az RHEL 6

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

A 7 RHEL

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

Ellenőrzése:

```
rpm -Kv azureclient.rpm
```

Jelölje be a kimeneti, hogy a csomag aláírás rendben

```
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

A RPM telepítése

```
sudo rpm -U azureclient.rpm
```

Befejezésekor ellenőrizze, hogy a virtuális Azure RHUI képernyő érhető el

### <a name="all-in-one-script-for-automating-the-above-task"></a>A fenti feladatok automatizálása teljes körű parancsfájl
Használja az alábbi parancsprogramot szükség szerint automatizálhatja a feladatot a szóban forgó VMs frissítés új Azure RHUI kiszolgálókhoz.

```
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a>RHUI – áttekintés
[Piros kalap frissítési infrastruktúra](https://access.redhat.com/products/red-hat-update-infrastructure) erősen méretezhető megoldást Red Hat Enterprise Linux felhő példányok piros kalap tanúsítvánnyal felhő szolgáltatók által futtatott yum tárházba tartalmak kezelésére nyújt. A felsőbb szintű cellulóz projekt alapján, RHUI lehetővé teszi a felhőben szolgáltatók helyileg tárházba piros kalap tárolt tartalom tükrözni, egyéni tárházakban létrehozása a saját tartalom és ezek tárházakban végfelhasználók tartalom kézbesítési terheléselosztás rendszer nagy csoport számára elérhetővé.

## <a name="regions-where-rhui-is-available"></a>Hol érhető el RHUI területek
RHUI RHEL igény szerinti képek esetén érhető el minden régióban érhető el. Jelenleg az [Azure állapot](https://azure.microsoft.com/status/) Irányítópultlap és Azure US kormányzati régiók felsorolt összes nyilvános régiókban tartalmazza. Az ár a RHEL igény szerinti képek kiépítve VMs RHUI hozzáférés szerepel. További területi/nemzeti felhő elérhetősége frissülnek, ahogy azt RHEL igény szerinti elérhetősége bontsa ki a jövőben.

> [AZURE.NOTE] Azure által üzemeltetett RHUI a hozzáférést a VMs belül [a Microsoft Azure adatközpont IP-címtartományai](https://www.microsoft.com/download/details.aspx?id=41653)korlátozódik.

## <a name="get-updates-from-another-update-repository"></a>A frissítések beszerzése a más frissítés tárházba

Ha módosítani szeretné a frissítések beszerzése a különböző frissítési összegyűjti (helyett Azure által üzemeltetett RHUI) szüksége lesz a RHUI példányok unregister parancsot, és újraregisztrálása őket a kívánt frissítési infrastruktúra (például piros kalap műholdas vagy piros kalap ügyfél portál CDN). Szüksége lesz megfelelő piros kalap előfizetéseket az alábbi szolgáltatások és a regisztrációs [piros kalap felhő](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure)eléréséhez az Azure.

Unregister RHUI, és a frissítés infrastruktúra követés való újraregisztrálása az alábbi lépéseket.

1.  /Etc/yum.repos.d/rh-cloud.repo szerkesztése és módosítása az összes `enabled=1` való `enabled=0`. Példa:

        # sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo

2.  /Etc/yum/pluginconf.d/rhnplugin.conf szerkesztheti és módosíthatja a `enabled=0` való `enabled=1`.
3.  Kattintson a kívánt infrastruktúra – például piros kalap ügyfél portál regisztrálása. Kövesse a piros kalap megoldási útmutató [regisztrálja](https://access.redhat.com/solutions/253273), és a rendszer a piros kalap ügyfél portálra előfizetés.

> [AZURE.NOTE] Az Azure által üzemeltetett RHUI való hozzáférést is tartalmazni fogja RHEL kirovó (PAYG) kép árát. Egy PAYG RHEL virtuális az Azure által üzemeltetett RHUI a regisztrációjának nem konvertálása a virtuális gép Előrehozás-a-saját-licenc (BYOL) típusú virtuális és így, előfordulhat, hogy kell hozni dupla díjakat, ha a azonos virtuális regisztrálása más forrásból újdonságai. 
> 
> Ha egységes kell egy frissítési infrastruktúra eltérő használata Azure által üzemeltetett RHUI fontolja meg, hozhat létre és üzembe helyezése a saját (BYOL típusú) képek [létrehozása és Azure virtuális gép feltöltése piros kalap-alapú](virtual-machines-linux-redhat-create-upload-vhd.md) a cikkben leírt módon.

## <a name="next-steps"></a>Következő lépések
Hozzon létre egy piros kalap vállalati Linux virtuális az Azure piactéren elérhető kirovó kép és emelés Azure által üzemeltetett RHUI lépjen [Azure piactérről](https://azure.microsoft.com/marketplace/partners/redhat/). Fogja tudni használni `yum update` a RHEL példány bármilyen további beállítási nélkül.