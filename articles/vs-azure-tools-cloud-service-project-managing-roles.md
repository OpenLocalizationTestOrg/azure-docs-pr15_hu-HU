<properties
   pageTitle="A Visual Studio projektek az Azure felhőben szerepkörök kezelése szolgáltatások |} Microsoft Azure"
   description="Megtudhatja, hogy miként adhat hozzá új szerepkört az Azure felhőalapú szolgáltatás project vagy a meglévő szerepkörök eltávolítása abból a Visual Studio használatával."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-roles-in-the-azure-cloud-services-projects-with-visual-studio"></a>A Visual Studio Azure cloud services projektekben szerepkörök kezelése

Miután létrehozta a Azure felhőalapú szolgáltatás projekt, új szerepkörök hozzáadása, vagy meglévő szerepkörök eltávolítása. Projekt importálása is, és alakítsa át a szerepkör. Például egy olyan ASP.NET webalkalmazást importálása, és jelölje ki azt a webes szerep.

## <a name="adding-or-removing-roles"></a>Hozzáadás és eltávolítás szerepkörök

**Szerepkör hozzáadása**

**Megoldás Explorer**nyissa meg a helyi menü, a **szerepkörök** csomópont a felhőalapú szolgáltatás projektben, és válassza a **Hozzáadás gombra**. Jelöljön ki egy meglévő webes szerepkör vagy dolgozó szerepkör az aktuális megoldást, vagy új webhely vagy dolgozó szerepkör projekt létrehozása. Vagy válassza ki a megfelelő projektben, például egy ASP.NET webes alkalmazás projekten, és társítása szerepkör projekt.

**Ha el szeretné távolítani a szerepkör-hozzárendelés**

Az a felhő szolgáltatási projekt megoldás Explorer **szerepkörök** csomópontra nyissa meg a helyi menü, távolítsa el, és válassza az **Eltávolítás**a szerepkörhöz.

## <a name="removing-and-adding-roles-in-your-cloud-service"></a>Eltávolítása, és a felhőszolgáltatásában szerepkörök hozzáadása

Ha szerepkörbe eltávolítása a felhőalapú szolgáltatás projekt, de úgy dönt, hogy állítsa vissza a szerepkör a projekt, csak a deklarálási szerepkörök és a végpontok és diagnosztikai adatok, például a egyszerű attribútumok kerülnek. Nincsenek további forrásokat vagy hivatkozások felvétele a ServiceDefinition.csdef fájlt, vagy a ServiceConfiguration.cscfg fájlt Ha szeretne hozzáadni, ezt az információt, akkor be kell manuálisan adhatja hozzá ezeket a fájlokat az.

Előfordulhat, hogy távolítsa el a webes szolgáltatás szerepkörbe és a később úgy dönt, hogy a szerepkör jeleníteni az a megoldás. Ha bejelöli ezt a hiba akkor fordul elő. Ez a hiba elkerülése érdekében fel kell venni a `<LocalResources>` látható a következő XML vissza az ServiceDefinition.csdef fájlba elemet. A nevet a név attribútum részeként megadott állapotfrissítéseket a project web szolgáltatás szerepkör a **<LocalStorage>** elemet. Ebben a példában az a webes szolgáltatás szerepkör neve **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Következő lépések

Tudnivalók a szerepkörök beállításáról a Visual Studióban [konfigurálása a szerepkörök az Azure felhőalapú szolgáltatás a Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md)elolvasásával.
