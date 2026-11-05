# Resultat och lärdomar

Utfall, tider och tekniska lärdomar från återställningarna i labbet: Del 2 (ransomware, procedur A) och Del 3 (ny värd, procedur B).

## Tider

| Mått | Mål | Utfall |
| --- | --- | --- |
| RPO | Högst 24 timmar | Uppfyllt – den dagliga backupen gjorde att ingen viktig data gick förlorad |
| RTO | Högst 4 timmar | Uppfyllt med god marginal |
| Bare Metal Recovery | – | Cirka 20 minuter per virtuell maskin |
| System State-återställning | – | Cirka 40 minuter i våra tester |

## Utvärdering

**Tappade de återställda maskinerna nätverket?**
Nej, men på ny hårdvara satte Windows ibland nätverksprofilen till standard och nätverkskortet till DHCP. Det rättades snabbt.

**Var Windows Server Backup pålitligt och tillräckligt snabbt?**
Ja. Bare Metal Recovery tog cirka 20 minuter och System State cirka 40 minuter, väl inom RTO på 4 timmar.

**Vilka steg i planen behöver förtydligas till nästa test?**
Gruppens svar var att inga steg behövde förtydligas, eftersom processen flöt på utan oväntade problem.

> **Tillägg vid portföljarbetet:** två förbättringar framgår ändå av testerna. Varje servers statiska IP-inställningar bör stå som en färdig checklista, eftersom nätverkskorten hamnar på DHCP efter återställning. Och planen bör vara entydig om vilken server som delar ut DHCP.

**Hur fungerade nätverkskorten i pfSense på ny hårdvara?**
Smidigt, utan manuell ompekning av MAC-adresser, eftersom `config.xml` lästes in rent. Om pfSense-VM:en får WAN-kortet först och LAN-kortet sedan, och switcharna på värden heter rätt, mappar pfSense alltid WAN till `hn0` och LAN till `hn1`.

**Vilket moment tog längst tid?**
Scenario B, eftersom den nya värden måste konfigureras. I scenario A påverkas inte värden.

**Saknades någon kritisk information i planen?**
Nej, allt som behövdes fanns i dokumentationen.

## Tekniska upptäckter

**Systemvolymen kan inte återställas inifrån Windows.**
Windows Server Backup kan inte skriva över den aktiva systemvolymen i ett operativsystem som körs. Därför måste återställningen startas från installationsmediet (ISO) i återställningsmiljön, som Bare Metal Recovery.

**Versionen av Windows Server spelar roll.**
När vi angav inloggningsuppgifter i återställningsmiljön (**Reparera datorn**) märkte vi en skillnad:

| Version | Format som fungerade |
| --- | --- |
| Windows Server 2022 | `domän\användare` |
| Windows Server 2025 | UPN, `användare@domän` |

**Felsökningstips:** `Shift + F10` i återställningsmiljön öppnar kommandotolken. Med `dir \\server\share` kontrollerade vi att nätverksåtkomsten och inloggningen mot den delade backupmappen fungerade innan återställningen startades.
