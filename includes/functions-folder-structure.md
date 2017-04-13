
Az összes egy adott funkció alkalmazásban a függvény a kód abban a legfelső szintű mappa, amely tartalmazza a host konfigurációs fájl és egy vagy több almappák külön függvény, ahogy az alábbi példa kódot, amelyek tartalmazzák

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

A legfelső szintű mappa függvény alkalmazás helyezkedik el a *host.json* fájl bizonyos futtatókörnyezet konfigurációs tartalmaz A rendelkezésre álló beállítások a további tudnivalókért lásd [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) a tárházba WebJobs.Script wiki.

Minden egyes függvénynek egy egy vagy több kódot fájlt, function.json beállításait, illetve más függőségek tartalmazó mappára.