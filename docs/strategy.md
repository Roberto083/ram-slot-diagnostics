# Diagnosztikai stratégia (lépésről lépésre)

## 1) Minimál-konfig
- 1 db RAM modul
- BIOS default jelleg (Auto / XMP OFF)
- cél: stabil POST + boot

## 2) Boot-mátrix (modulonként / slotonként)
Egy modullal indíts minden releváns slotban, majd ismételd a másik modullal.

**Értelmezés:**
- Ha egy modul minden slotban rossz → modul gyanús
- Ha minden modul ugyanabban a slotban rossz → slot gyanús

## 3) Stabil slot (baseline) meghatározása
Amelyik kombináció stabilan bootol, az lesz a baseline.

## 4) Dual-channel próbálkozás (ha a baseline stabil)
- javasolt párosítás (gyakran A2 + B2)
- ha instabil: vissza baseline-ra, és előbb modul/slot kizárás

## 5) Terheléses teszt
- rövid stressz: gyors jelzés instabilitásra
- hosszabb RAM teszt: célzott memória-ellenőrzés
