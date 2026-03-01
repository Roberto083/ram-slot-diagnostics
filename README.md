# ram-slot-diagnostics
Ellenőrzőlista-jellegű jegyzetek RAM slot / dual-channel stabilitási hibák diagnosztikájához. Nyilvános, adatvédelmi okból naplók és kimenetek nélkül.

📌 **Adatvédelem:** lásd `SANITIZE.md` (a repó nyilvános, nincs nyers log/azonosító).

# RAM slot diagnosztika (nyilvános, max privacy)

Ellenőrzőlista-jellegű jegyzetek RAM slot / dual-channel / POST és stabilitási hibák diagnosztikájához Linux (Linux Mint XFCE) környezetben.

**Cél:** a hibakeresési gondolkodás és lépések dokumentálása **reprodukálható** formában (tünet → teszt → következtetés).  
**Privacy:** a repó szándékosan **nem tartalmaz** nyers logokat vagy rendszerazonosítókat.

## Tartalom
- `SANITIZE.md` – adatvédelmi szabályok (mit nem publikálunk)
- `docs/strategy.md` – diagnosztikai stratégia (minimál-konfig → boot-mátrix → baseline → dual-channel)
- `docs/matrix.md` – boot-mátrix táblák (kimenet nélkül, csak eredmények)
- `docs/commands.md` – használt parancsok (kimenet nélkül)
- `docs/bios-notes.md` – BIOS opciók (log nélkül)

## Környezet (általánosítva)
- Platform: Intel-alapú asztali PC (7. generációs szint)
- Chipset: B-szériás (B2xx jelleg)
- Memória: 16 GB DDR4 (2×8 GB), ~2400 MHz kategória
- OS: Linux Mint XFCE (Debian/Ubuntu-alapú)

## Fogalmak / alapelv
A legtöbb DDR4-es alaplapon a javasolt dual-channel párosítás: **A2 + B2** (CPU-tól számított 2. slotok csatornánként).  
Megjegyzés: modellenként eltérhet – mindig ellenőrizd az alaplap kézikönyvét.

## Következtetés (összefoglaló, beazonosítás nélkül)
- Egy slot gyanús (mechanikai/kontakt instabilitás valószínű)
- Egy másik slot stabil (baseline)
- Végcél: stabil baseline → dual-channel kísérlet → hosszabb terheléses verifikáció

## Licenc / felhasználás
Szabadon felhasználható diagnosztikai jegyzetként. Nyilvános repóban szándékosan nincs rendszerazonosító vagy nyers log.
