##<a name="highly-available-solutions-with-azure-traffic-manager"></a>Az Azure forgalom-kezelővel könnyen hozzáférhető megoldások

Meg kell határoznia, hogy a terhelést magas elérhetősége követelményeknek is teljesülniük kezelővel Azure forgalom egyedül, vagy ha más DNS-megoldásokat vagy a folyamatok forgalom manager egyesítéséhez szükséges. Attól függően, hogy az igényeinek használhatja:

- **Adatforgalom manager egyedül**. Ha 99.99 %-os felüli időpontokat a terhelést elegendő, a forgalom manager saját maga is használhatja. Az adatforgalom-kezelő szolgáltatás meghibásodása felhasználók nem tudnak a terhelést eléréséhez, amíg az adatforgalom-kezelő szolgáltatás másolást.

- A **forgalom manager egy másik megoldást, Azure forgalom manager együtt használja**. Abban az esetben a forgalom kezelő szolgáltatás módosíthatja a többi forgalom kezelő szolgáltatás mutasson a CNAME rekordot. A terhelést a hozzáférést továbbra is elérhető, de a terhelést szolgáltatója összes helyekre elosztott. A legtöbb drága megoldás, de van szükség egy újabb SLA munkaterhelésekből szükség lehet.
