# Controle: werkt alles nu correct?

Doel: één volledige controleronde over de database, de mailverzending en de opmaak (sjablonen) van elke mail, met een kort verslag per onderdeel.

## Wat ik al zag bij het nalezen van de code

- Het afzenderadres komt overal uit de instellingen, met vaste terugval `no-reply@send.maximilien.site`. Het oude `noreply@maximilien.site` staat nergens meer.
- Contactformulieren, bestelbonnen, boekingen, peterschap en inlogmails gebruiken allemaal dezelfde huisstijl-opmaak.
- Inlogmails gaan uitdrukkelijk via Brevo; de meeste andere mails kiezen automatisch Brevo zodra de sleutel aanwezig is.
- Adressen als `contact@maximilien.brussels` staan nog in zichtbare paginateksten. Dat is bewust (contactadres voor bezoekers), geen afzender.

## Wat ik ga controleren

1. Database: verbinding, alle tabellen aanwezig, geen openstaande migraties.
2. Verzendroute: één echte testmail per soort (contact, inlogcode/magic link, bestelbevestiging, mededeling) en nakijken dat elke mail via Brevo vertrekt met het juiste afzenderadres.
3. Opmaak: elke sjabloon in de drie talen (NL/FR/EN) bekijken via de ingebouwde voorbeeldweergave — logo, titel, knop, voettekst, en de platte-tekstversie.
4. Links: controleren dat knoppen in inlogmails naar het juiste adres wijzen voor zowel maximilien.site als maximilien.app.
5. Foutafhandeling: nagaan dat een mislukte verzending netjes in het logboek belandt met de exacte foutcode, en dat de bezoeker een duidelijke melding krijgt.
6. Alle geautomatiseerde tests opnieuw draaien.

## Wat ik daarna doe

- Alles wat afwijkt, herstel ik meteen (opmaak, ontbrekende taal, verkeerde link, ontbrekende foutmelding).
- Je krijgt een kort overzicht per onderdeel: goed / hersteld / vraagt jouw actie.

## Technisch

- Migratie- en gezondheidscontrole via de bestaande ontwikkelroutes (`/api/dev/run-migrations`, `/api/dev/health`).
- Sjabloonweergave via `src/lib/email-previews.server.ts` en `src/lib/transactional-templates.ts`.
- Verzendpaden: `email.server.ts` (Brevo-payload en logging), `email-service.server.ts`, `email-engine.server.ts`, `auth-email.server.ts`.
- Testmails naar een adres dat jij aangeeft; standaard gebruik ik het beheerdersadres.
