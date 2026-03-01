# Adatvédelem / Sanitization

Ez a repó nyilvános, ezért **nem publikálok logokat vagy rendszer-kimeneteket**, még anonimizálva sem.

Nem kerül ki például:
- teljes vagy részleges `dmesg`, `journalctl`, `inxi`, `dmidecode` kimenet
- terminál képernyőképek
- útvonalak (pl. `/home/...`), hostname, felhasználónév
- UUID / MAC / sorozatszám / asset tag jellegű azonosítók
- időbélyegek (dátum / idő / időzóna)

A repó célja a diagnosztikai folyamat (lépések + döntési logika) dokumentálása.  
Részletes logok csak **privát egyeztetéssel** adhatók át célzott hibakereséshez.
