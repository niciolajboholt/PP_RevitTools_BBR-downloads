# P+P Revit Tools – BBR Arealfordeling

**Status:** Beta – endnu ikke udgivet som release
**Understøttede Revit-versioner:** 2024, 2025, 2026 og 2027
**Udviklerkontakt:** [nbb@pplusp.dk](mailto:nbb@pplusp.dk)

## Download

Den første beta-installationsfil er endnu ikke publiceret. Når den er bygget
og testet, bliver den tilgængelig under **[Releases](../../releases)** på
denne side.

## Formål

P+P Revit Tools – BBR Arealfordeling hjælper med at kontrollere, beregne og
fordele adgangs- og fællesboligarealer på boliger i Autodesk Revit. Værktøjet
er lavet til P+P's BIM-arbejdsgang og reducerer manuelt arbejde, uens
parameteropsætning og risikoen for regnefejl.

Add-in'et arbejder kun med `Areas` i det arealskema, brugeren vælger. Før
noget skrives til modellen, vises datagrundlag, fejl, forslag, grupper og
beregnede resultater til kontrol. Værktøjet er et projekterings- og
kvalitetssikringsværktøj — den faglige vurdering og det endelige
myndighedsgrundlag er fortsat brugerens ansvar.

## Hovedfunktioner

- Kontrollerer og kan oprette de nødvendige P+P Shared Parameters automatisk.
- Læser og validerer Areas i det valgte arealskema.
- Fordeler adgangsarealer og fællesboligarealer på de relevante boliger.
- Foreslår klassifikation ud fra Area-navne (trapperum, altangang, fællessal,
  cykelparkering, bolig m.fl.).
- Genkender betegnelser som `Opgang X` / `Trappe Y` og foreslår passende
  `A_House Number`.
- Samler geometrisk sammenhørende Areas i nabogrupper og foreslår fælles
  gruppering.
- Viser alle resultater i et preview, før noget skrives.
- Skriver resultater samlet i én Revit-transaktion, så hele handlingen kan
  rulles tilbage ved fejl.
- Indeholder indbygget PDF-vejledning via knappen **Se vejledning**.

## Typisk arbejdsgang

1. Vælg `P+P > Arealer > BBR Arealfordeling` i Revit.
2. Kontrollér Shared Parameters, og opret manglende med **Add** / **Add all**.
3. Vælg det korrekte arealskema.
4. Gennemgå valideringsfejl og add-in'ets forslag.
5. Ret værdier, anvend sikre forslag, eller kopiér til en nabogruppe.
6. Vælg **Gem og genberegn**, og kontrollér gruppeoversigt og bolig-preview.
7. Vælg **Skriv værdier til Revit**, når resultatet er godkendt.
8. Kontrollér de skrevne værdier i et Revit Area Schedule.

Ingen værdier skrives, før brugeren aktivt godkender handlingen.

## Installation (når en release er udgivet)

1. Luk alle åbne Revit-versioner.
2. Hent installationsfilen fra **[Releases](../../releases)**.
3. Start installationsfilen, og vælg de Revit-versioner add-in'et skal
   installeres til.
4. Start Revit, og vælg `P+P > Arealer > BBR Arealfordeling`.

Installeren understøtter Revit 2024–2027 i samme flow. Brugeren behøver ikke
Visual Studio, .NET SDK eller separate Revit API-filer.

## Kendte begrænsninger (beta)

- Add-in'et er endnu ikke digitalt kodesigneret, så Windows kan vise en
  sikkerhedsadvarsel ved installation.
- Parametre med forkert GUID, datatype eller binding skal håndteres manuelt.
- Der er endnu ikke ribbon-ikon, eksportfil eller permanent revisionslog.

## Support

Ved fejl bedes du sende:

- Revit-version,
- en kort beskrivelse af den præcise handling,
- teksten fra en eventuel fejlmeddelelse,
- og gerne et skærmbillede.

Kontakt: [nbb@pplusp.dk](mailto:nbb@pplusp.dk)
