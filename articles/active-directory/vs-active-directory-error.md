<properties 
    pageTitle="Hitelesítés észlelési során hiba" 
    description="Az active directory-kapcsolat varázsló észlelt-kompatibilis hitelesítés típusa" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="error-during-authentication-detection"></a>Hitelesítés észlelés során hiba

Észlelését előző hitelesítési kódot, miközben a varázsló észlelt-kompatibilis hitelesítés típusa.   

##<a name="what-is-being-checked"></a>Mi az ellenőrzendő?

**Megjegyzés:** Annak érdekében, hogy helyesen észleli az előző hitelesítési kód projekt, a projekt kell felépíteni.  Ha ezt a hibát, és a projekt nincs egy előző hitelesítési kódot, újjáépítése, és próbálkozzon újra.

###<a name="project-types"></a>A projekttípusok

A varázsló ellenőrzi, hogy milyen típusú projektet, így a projektbe megváltoztathatják a megfelelő hitelesítési logika fejleszt.  Ha bármelyik vezérlő, amely származik `ApiController` a projektben, a projekt számítanak WebAPI projekt.  Ha csak a származtatása vezérlők `MVC.Controller` a projektben, a projekt számítanak MVC projektben.  Bármely más a varázsló által nem támogatott.  Fejlesztő projektek jelenleg nem támogatott.

###<a name="compatible-authentication-code"></a>Kompatibilis hitelesítési kódot.

A varázsló is ellenőrzi, hogy a varázsló korábban beállított vagy a varázsló alkalmazással kompatibilis hitelesítési beállítások.  Ha minden beállítás, célszerű párhuzamosan eset és a varázsló megnyitásához, és a beállítások megjelenítése.  Ha csak a jelen bizonyos beállításokat, egy hiba esetben célszerű.

MVC projektben a varázsló ellenőrzi a varázsló előző felhasználás eredménye a következő beállítások közül:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Ezenkívül a varázsló ellenőrzi a webes API projektben, a következő beállításokat, a varázsló előző felhasználása eredményeként bármelyikét:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

###<a name="incompatible-authentication-code"></a>Nem kompatibilis hitelesítési kódot.

Végül a varázsló megkísérli észleli a Visual Studio korábbi verzióival konfigurált hitelesítési kód verziója. Ha ezt a hibát, akkor a projekt-kompatibilis hitelesítés típusa tartalmazza. A varázsló észleli az alábbi típusú Visual Studio korábbi verziókból származó hitelesítés:

* Windows-hitelesítés 
* Egyéni felhasználói fiókok 
* Szervezeti fiókok 
 

Windows-hitelesítés feltárása MVC projektben, a varázsló megkeresi a `authentication` elem a **web.config** fájlból.

<pre>
    &lt;konfigurációs&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;hitelesítési mód = "Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/Configuration&gt;
</pre>

A webes API-projekt feltárása a Windows-hitelesítés, a varázsló megkeresi a `IISExpressWindowsAuthentication` elem a projekt **.csproj** fájlból:

<pre>
    &lt;A Project&gt;
&lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;engedélyezett&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup > &lt;/projekt        &gt;
</pre>

Egyes felhasználói fiókok hitelesítési feltárása, a varázsló megvizsgálja a csomag elem a **Packages.config** fájlból.

<pre>
    &lt;csomagok&gt;
        <span style="background-color: yellow">&lt;id="Microsoft.AspNet.Identity.EntityFramework csomag" verzió = "2.1.0" targetFramework = "net45" /&gt;</span>
    &lt;/csomagok&gt;
</pre>

Egy régi szervezeti fiók hitelesítési mód feltárása, a varázsló megkeresi a következő elem **web.config**a:

<pre>
    &lt;konfigurációs&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;kulcs hozzáadása = "ida: tartomány" érték = "x" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/Configuration&gt;
</pre>

Ha módosítani szeretné a hitelesítés típusa, távolítsa el a nem kompatibilis a hitelesítés típusa, és futtassa újra a varázslót.

További tudnivalókért lásd: [Az Azure Active Directory Authentication esetek](active-directory-authentication-scenarios.md).