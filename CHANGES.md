# Ändringar vid portföljarbetet

Repot byggdes i oktober 2026 av gruppens Disaster Recovery Plan, version 4.0 från maj 2026. Den här filen visar vad som är originalarbete och vad som ändrats efteråt.

## 1. Omskrivning och sanering

- Planen skrevs om från PDF till Markdown och delades upp i plan, två procedurer och utvärdering.
- Gruppmedlemmarnas namn ersattes med roller.
- Märkningen "Konfidentiellt (internt bruk)" togs bort, eftersom företaget är fiktivt.
- Lärarens uppgiftstext ingår inte. Uppgiften beskrivs med egna ord i README.
- Nätverksdiagrammet och incidentflödet ritades om i Mermaid. Övriga diagram är nya.
- Filserverns IP-adress, som var trasig i PDF:en, är 192.168.21.10.

## 2. Rättelser

| Var | Original | Rättelse |
| --- | --- | --- |
| Backupstrategi | VSS fryser databasen "i en millisekund" och "skyddar helt mot USN rollback" | VSS ger en konsekvent ögonblicksbild. Skyddet mot USN rollback kommer av att DC:n återställs från en backup som Windows känner igen, inte från en kopierad VHDX. |
| Backupstrategi | "Full syntetisk backup" varje vecka och retention på 7 dagar | Windows Server Backup gör blockbaserade inkrementella backuper och rensar gamla versioner själv. Inställningarna finns inte i verktyget. |
| Underhåll | Angriparen har ingen åtkomst om alla användarlösenord byts | Återställ från en punkt före intrånget och byt lösenord för administratörer, tjänstekonton och `krbtgt` (två gånger) |
| Verifiering | Klienten får IP från pfSense DHCP-server | DHCP-scopet ligger på SRV-DC-01 enligt IP-planen |
| Utvärdering | Inga steg behöver förtydligas | Tillägg: en checklista för statisk IP per server och en entydig beskrivning av vem som delar ut DHCP |

Varje rättelse är också märkt i texten.

## 3. Nytt vid portföljarbetet

- README med sammanfattning, arkitektur, resultat, felsökning, säkerhet, lärdomar och en guide för att återskapa labbet.
- Avsnittet om att `config.xml` inte ska ligga i ett git-repo.
