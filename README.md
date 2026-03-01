# ram-slot-diagnostics
Ellenőrzőlista-jellegű jegyzetek RAM slot / dual-channel stabilitási hibák diagnosztikájához. Nyilvános, adatvédelmi okból naplók és kimenetek nélkül.

# RAM slot diagnosztika (nyilvános, max privacy)

Ez a repó egy RAM-slot / dual-channel / POST és stabilitási hibakeresési folyamat dokumentációja.  
Nyilvános repóként készült, ezért **nem tartalmaz semmilyen nyers logot vagy rendszer-kimenetet**.

A cél, hogy a diagnosztikai gondolkodás és lépések másoknak is használhatóak legyenek, miközben semmi olyan adat nem kerül ki, ami alapján a gép vagy a tulajdonos beazonosítható.

---

## Adatvédelem (szándékosan nincs log)

Ebben a repóban **nem publikálok**:

- teljes vagy részleges `dmesg`, `journalctl`, `inxi`, `dmidecode` kimeneteket
- képernyőképeket terminálról
- útvonalakat (pl. `/home/...`), hostnevet, felhasználónevet
- UUID / MAC / sorozatszám / asset tag jellegű azonosítókat
- időbélyegeket (dátum/idő/időzóna)

Ha célzott hibakereséshez valakinek mégis szüksége van logokra, azt **privát módon** lehet egyeztetni.

---

## Környezet (általános)

- Platform: Intel-alapú asztali PC (7. generációs szint)
- Chipset: B-szériás (B2xx jelleg)
- Memória: 16 GB DDR4 (2×8 GB), ~2400 MHz kategória
- OS: Linux Mint XFCE (Debian/Ubuntu-alapú)

---

## Történet (általánosítva)

- A memória eredetileg nem optimális slot-párban futott (single-channel jelleg).
- Fizikai átrendezés közben egy DIMM rögzítő elem megsérült.
- Ezt követően az egyik DIMM slot gyanússá vált (POST/boot instabil), míg egy másik slot megbízhatóan működik.

**Cél:** stabil konfiguráció kialakítása, lehetőleg dual-channel módon, de elsődlegesen stabilitásra optimalizálva.

---

## Fogalmak / alapelv

A legtöbb DDR4-es alaplapon a “javasolt” dual-channel párosítás:

- **A2 + B2** (a CPU-tól számított 2. slotok csatornánként)

> Megjegyzés: slotkiosztás modellenként eltérhet, mindig ellenőrizd az alaplap kézikönyvét is.

---

## Diagnosztikai célok (mit akarunk kizárni)

- RAM modul hiba: egy konkrét stick hibás
- Slot / kontakt / mechanikai hiba: egy adott foglalat instabil
- BIOS / beállítás instabilitás: XMP / órajel / feszültség / tréning
- Ritkább: platform-specifikus memóriavezérlő érzékenység

---

## Diagnosztikai stratégia (lépésről lépésre)

### 1) Minimál-konfig
- 1 db RAM modul
- alap beállítások (BIOS default jelleg)
- cél: stabil POST + boot

### 2) Boot-mátrix (modulonként / slotonként)
Egy modullal indíts minden releváns slotban, majd ismételd a másik modullal.

**Értelmezés:**
- Ha egy modul minden slotban rossz → modul gyanús
- Ha minden modul ugyanabban a slotban rossz → slot gyanús

### 3) Stabil slot megtalálása
Amelyik kombináció stabilan bootol, az lesz a “baseline”.

### 4) Dual-channel próbálkozás (ha a baseline stabil)
- A javasolt dual-channel pár (általában A2+B2)
- ha instabil: vissza baseline-ra, és előbb modul/slot kizárás

### 5) Terheléses teszt
- rövid stressz: gyors jelzés instabilitásra
- hosszabb RAM teszt: célzott memória-ellenőrzés

---

## Boot-mátrix (összefoglaló táblázat)

A táblázat **nem tartalmaz logokat**, csak eredményt.

| Modul | Slot | POST/boot | Rövid stressz | Megjegyzés |
|------:|:----:|:---------:|:-------------:|------------|
| #1    | A2   | ✅ / ❌    | ✅ / ❌        |            |
| #1    | B2   | ✅ / ❌    | ✅ / ❌        |            |
| #2    | A2   | ✅ / ❌    | ✅ / ❌        |            |
| #2    | B2   | ✅ / ❌    | ✅ / ❌        |            |

**Dual-channel próbák (ha releváns):**

| Slot-pár | POST/boot | Stabilitás | Megjegyzés |
|:--------:|:---------:|:----------:|------------|
| A2+B2    | ✅ / ❌    | ✅ / ❌     |            |

---

## Használt parancsok (kimenet nélkül)

### RAM slot / modul azonosítás
```bash
sudo dmidecode -t memory | egrep -i "Size:|Locator:" -n
Memória / hiba-jellegű kernel sorok szűrése
dmesg -T | egrep -i "edac|mce|machine check|ecc|memory|dimm|ram|segfault|error|fail" -n
Figyelmeztetések az aktuális boottól
journalctl -b -0 -p warning..alert --no-pager
stress-ng gyors VM teszt
stress-ng --vm 2 --vm-bytes 75% --timeout 5m --metrics-brief
memtester (opcionális)
sudo memtester 6G 3
BIOS beállítások (log nélkül rögzítve)

XMP: ON / OFF

RAM órajel: Auto / kézi (pl. 2400 kategória)

RAM feszültség: Auto / kézi

Megjegyzés: bármilyen változtatás után csak egy dolgot módosíts egyszerre, hogy a hatás visszakövethető legyen.

Következtetések (összefoglaló, beazonosítás nélkül)

Egy slot gyanús (mechanikai/kontakt instabilitás valószínű)

Egy másik slot stabil (baseline)

A végcél: stabil baseline → dual-channel kísérlet → hosszabb terheléses verifikáció

Licenc / felhasználás

Szabadon felhasználható diagnosztikai jegyzetként. Nyilvános repóban szándékosan nincs rendszerazonosító vagy nyers log.


---

## ✅ SANITIZE.md (másold be 1:1)

```markdown
# Adatvédelem

Ez a repó nyilvános, ezért **nem publikálok logokat vagy rendszer-kimeneteket**, még anonimizálva sem.

Nem kerül ki például: `dmesg`, `journalctl`, `inxi`, `dmidecode`, képernyőképek terminálról, útvonalak, hostnév, felhasználónév, UUID/MAC/sorozatszám, időbélyegek.

A repó célja a diagnosztikai folyamat (lépések + döntési logika) dokumentálása.  
Részletes logok csak **privát egyeztetéssel** adhatók át célzott hibakereséshez.

logs/
public_logs/
*.log
*.txt
*.out
