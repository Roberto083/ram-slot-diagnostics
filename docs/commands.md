# Használt parancsok (kimenet nélkül)

## RAM slot / modul azonosítás
sudo dmidecode -t memory | egrep -i "Size:|Locator:" -n

## Memória / hiba-jellegű kernel sorok szűrése
dmesg -T | egrep -i "edac|mce|machine check|ecc|memory|dimm|ram|segfault|error|fail" -n

## Figyelmeztetések az aktuális boottól
journalctl -b -0 -p warning..alert --no-pager

## stress-ng gyors VM teszt
stress-ng --vm 2 --vm-bytes 75% --timeout 5m --metrics-brief

## memtester (opcionális)
sudo memtester 6G 3
