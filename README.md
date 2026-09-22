# P+P Revit Tools – BBR Arealfordeling

**Aktuel version:** 0.6.0 (beta)  
**Understøttede Revit-versioner:** 2024, 2025, 2026 og 2027  
**Udviklerkontakt:** [nbb@pplusp.dk](mailto:nbb@pplusp.dk)

## Download

Installationsfilen udgives som GitHub Release to steder:

- **Brugere:** det offentlige downloads-repository  
  **[PP_RevitTools_BBR-downloads › Releases](https://github.com/niciolajboholt/PP_RevitTools_BBR-downloads/releases)**
- **Udviklere med adgang:** dette private repository  
  **[PP_RevitTools_BBR › Releases](https://github.com/niciolajboholt/PP_RevitTools_BBR/releases)**

Hent `PP_RevitTools_BBR_v0.6.0_Setup.exe`. `.sha256`-filen kan bruges til at
kontrollere, at filen er hentet korrekt. Versioner før v1.0.0 udgives som beta
(pre-release).

## Formål

P+P Revit Tools – BBR Arealfordeling hjælper med at kontrollere, beregne og
fordele adgangs- og fællesboligarealer på boliger i Autodesk Revit. Værktøjet er
lavet til P+P's BIM-arbejdsgang og reducerer manuelt arbejde, uens
parameteropsætning og risikoen for regnefejl.

Add-in'et arbejder kun med `Areas` i det arealskema, brugeren vælger. Før noget
skrives til modellen, vises datagrundlag, fejl, forslag, grupper og beregnede
resultater til kontrol. Værktøjet er et projekterings- og
kvalitetssikringsværktøj; den faglige vurdering og det endelige
myndighedsgrundlag er fortsat brugerens ansvar.

## Hovedfunktioner

- Kontrollerer de seks nødvendige P+P Shared Parameters ud fra navn, GUID,
  datatype, instancebinding og kategorien `Areas`.
- Kan erstatte en ugyldig parameter med forkert GUID, datatype eller binding
  med den officielle P+P-parameter (**Erstat**) efter tydelig advarsel.
- Opretter manglende Shared Parameters direkte fra add-in'ets indbyggede
  parameterkontrakt.
- Kræver ikke, at brugeren har `A_DK_Shared Parameters.txt` valgt eller liggende
  lokalt.
- Læser og validerer Areas i det valgte arealskema.
- Fordeler adgangsarealer på boliger og erhvervsenheder og
  fællesboligarealer på boliger.
- Understøtter typen `Erhverv`, som modtager adgangsareal, men ikke
  fællesboligareal.
- Foreslår klassifikation ud fra Area-navne som eksempelvis trapperum,
  altangang, fællessal, cykelparkering, butik og bolig.
- Genkender betegnelser som `Opgang X` og `Trappe Y` og kan foreslå et passende
  `A_House Number`.
- Analyserer fælles Area-grænser og samler geometrisk sammenhørende Areas i
  nabogrupper.
- Samler fejl for samme Area i én redigerbar række.
- Bevarer modelspecifikke rettelseskladder, hvis arbejdet ikke afsluttes med det
  samme.
- Viser alle resultater i et preview med Etage og opdelt adgangs- og
  fællesboligtillæg, før de skrives.
- Eksporterer alle Areas med input, fordeling, resultat og fejl til en
  Excel-venlig CSV-fil.
- Skriver resultatparametrene samlet i én Revit-transaktion, så hele handlingen
  rulles tilbage, hvis en fejl opstår.
- Indeholder knappen **Se vejledning**, som åbner den installerede PDF-manual.

## Typisk arbejdsgang

1. Åbn Revit-modellen og gem den, før en ny version af add-in'et afprøves.
2. Vælg `P+P > Arealer > BBR Arealfordeling`.
3. Kontrollér Shared Parameters, og vælg **Add** eller **Add all**, hvis noget
   mangler.
4. Vælg det korrekte arealskema.
5. Gennemgå valideringsfejl og add-in'ets forslag.
6. Ret værdier enkeltvis, anvend sikre forslag eller kopier en værdi til en
   nabogruppe.
7. Vælg **Gem og genberegn**, og kontrollér derefter gruppeoversigt og
   bolig- og erhvervspreview.
8. Gem eventuelt beregningsgrundlaget med **Eksportér CSV**.
9. Vælg **Skriv værdier til Revit**, når resultatet er godkendt.
10. Kontrollér de skrevne værdier i et Revit Area Schedule.

Ingen beregnede værdier skrives, før brugeren aktivt godkender handlingen.

## Parametre

### Input

| Parameter | Type | Formål |
|---|---|---|
| `A_House Number` | Text | Kobler boliger og fordelbare fællesarealer til samme gruppe. |
| `A_Shared Space` | Yes/No | Angiver, om Area'et indgår som et fællesareal. |
| `A_Shared Space Type` | Text | Klassificerer Area'et, fx boligareal, erhverv, adgangsareal eller fællesboligareal. |
| `A_Distribution Factor` | Number | Angiver hvor stor en andel af Area'et der fordeles. |

### Resultater

| Parameter | Type | Formål |
|---|---|---|
| `A_Shared Space Area_Added` | Area | Boligens/erhvervsenhedens præcise beregnede andel af adgangs- og fællesarealet. |
| `A_BBR Area_Rounded` | Area | Enhedens eget areal plus tillæg, afrundet til hele positive m². |

`A_BBR Area_Rounded` anvender normal positiv 0,5-oprunding. Adgangsarealer og
fællesboligarealer bruges som beregningsgrundlag, men får ikke selv et positivt
BBR-resultat. Areas, der hverken er bolig eller erhverv, får eksplicit `0 m²` i
begge resultatparametre.

## Automatisk oprettelse af Shared Parameters

Ved opstart kontrollerer add-in'et, om hver parameter:

- er en Shared Parameter,
- har P+P's officielle GUID,
- har korrekt datatype,
- er en instanceparameter,
- og er bundet til kategorien `Areas`.

Manglende definitioner kan oprettes enkeltvis med **Add** eller samlet med
**Add all**. Revit API'et kræver teknisk en Shared Parameters-fil under selve
oprettelsen. Add-in'et laver derfor en unik midlertidig fil, anvender den og
gendanner derefter automatisk den Shared Parameters-fil, brugeren eventuelt
havde valgt i forvejen. Den oprindelige fil ændres ikke.

En eksisterende parameter med korrekt navn, men forkert GUID, datatype eller
binding ændres ikke automatisk. Den markeres som ugyldig og får handlingen
**Erstat**. Før erstatning tælles udfyldte Area-værdier, og datatab kræver en
ekstra bekræftelse. Sletning og genoprettelse sker i én transaktion med
rollback. **Add all** erstatter aldrig automatisk.

## Klassifikation og fordeling

Værktøjet understøtter blandt andet følgende Area-typer:

- `Boligareal`
- `Erhverv`
- `Adgangsareal`
- `Fællesboligareal`
- `Udenomsrum`
- `Ikke-boligareal`
- `Udendørsareal`

Adgangsareal fordeles på både boliger og erhvervsenheder i samme
`A_House Number`-gruppe. Fællesboligareal fordeles kun på boliger.

Adgangsareal kan normalt fordeles med faktor `1,00` eller efter en dokumenteret
faglig vurdering med faktor `0,50`. Fællesboligareal fordeles normalt med faktor
`1,00`. Udenomsrum, ikke-boligareal og udendørsareal er ikke fordelbare og skal
have faktor `0`.

Boliger og deres relevante adgangs- eller fællesarealer skal have samme
`A_House Number`. Brug stabile og forståelige betegnelser som `Opgang X` eller
`Trappe Y`. Bindestreger accepteres ikke som gyldige gruppenavne.

Den reserverede værdi `ALLE` må kun bruges på fællesboligareal, når alle boliger
i det valgte arealskema reelt har brugsret. Arealet fordeles da ligeligt på alle
bolig-Areas. Valget kræver altid faglig kontrol.

## Forslag, nabogrupper og rettelser

Add-in'et analyserer både Area-navne og geometri. Navnebaserede forslag kan
angive type, Shared-status, faktor og House Number. Geometrisk analyse
sammenligner Area-grænser på samme niveau og i samme arealskema.

Hvis en gruppe har én kendt House-værdi, kan den foreslås til manglende naboer.
Flere forskellige kendte værdier vises som en konflikt. En gruppe uden en kendt
værdi kan få et midlertidigt forslag som `Nabogruppe 1`, som brugeren erstatter
med projektets vedtagne betegnelse.

Forslag kan anvendes på én række, på alle sikre forslag eller på alle forslag,
der fortsat er fagligt mulige. Et eksisterende `A_House Number` overskrives
aldrig automatisk. Ændringer forbliver et preview, indtil brugeren vælger
**Gem og genberegn**.

## Sikkerhed ved skrivning

- Tomme obligatoriske parametre blokerer skrivning.
- En fordelingsgruppe uden modtagende boliger vises som en redigerbar fejl.
- Resultater genberegnes efter inputrettelser.
- Begge resultatparametre skrives i samme transaktion.
- En fejl under skrivning ruller hele transaktionen tilbage.
- Revit Undo kan fortryde den samlede resultatskrivning.

## Installation for brugere

1. Luk alle åbne Revit-versioner.
2. Hent `PP_RevitTools_BBR_v0.6.0_Setup.exe` fra
   [downloads-repositoryets Releases](https://github.com/niciolajboholt/PP_RevitTools_BBR-downloads/releases).
3. Start installationsfilen.
4. Vælg de Revit-versioner, add-in'et skal installeres til.
5. Start Revit og vælg `P+P > Arealer > BBR Arealfordeling`.

Installeren kan håndtere Revit 2024–2027 i samme brugerflow. Installerede
Revit-versioner er forudvalgt, men de andre kan vælges manuelt. Brugeren behøver
ikke Visual Studio, .NET SDK, Inno Setup eller separate Revit API-filer.

Add-in'et installeres pr. bruger i:

```text
%APPDATA%\Autodesk\Revit\Addins\<år>
```

PDF-vejledningen installeres ved siden af den versionsspecifikke add-in-DLL som:

```text
PP.RevitTools\BBR_Arealfordeling_Vejledning.pdf
```

Programmet kan fjernes igen under **Windows > Installerede apps**.

## Byg installationsfilen

Den letteste udviklerarbejdsgang er:

1. Hent eller klon repositoryet på en Windows-pc.
2. Luk Revit.
3. Dobbeltklik på `RUN_ME.bat`.
4. Følg build- og installationsvinduet.

`RUN_ME.bat` kontrollerer de nødvendige udviklingskrav, bygger alle fire
Revit-årgange, kører smoke tests og opretter installationsfilen. Hvis Revit er
lukket, starter installationen automatisk. Hvis Revit kører, færdiggøres
`.exe`-filen stadig, men installationen springes over, indtil brugeren har
lukket Revit og starter filen manuelt.

Udviklingsmaskinen skal have:

- Windows,
- .NET Framework 4.8 Developer Pack,
- .NET 8 SDK til Revit 2025–2026,
- .NET 10 SDK til Revit 2027,
- Inno Setup 6 eller 7.

Revit 2024 bygges til `net48`, Revit 2025–2026 til `net8.0-windows` og Revit
2027 til `net10.0-windows`. Buildet anvender låste compile-only
Revit-API-referencepakker fra Nice3point; API-DLL'erne følger ikke med
installationen.

Manuelt build kan køres med:

```powershell
Set-ExecutionPolicy -Scope Process Bypass
.\build-installer.ps1
```

Resultatet placeres i:

```text
release\PP_RevitTools_BBR_v0.6.0_Setup.exe
release\PP_RevitTools_BBR_v0.6.0_Setup.exe.sha256
release\BBR_Arealfordeling_Vejledning_v0.6.0.pdf
```

Kun `.exe`-filen er nødvendig for kollegaerne. PDF-vejledningen er indlejret i
installationsfilen. Den separate PDF og SHA-256-fil er valgfrie reference- og
kontrolfiler.

## Test før distribution

Et vellykket compile og beståede smoke tests er ikke alene en godkendt
Revit-kompatibilitetstest. Før en release distribueres bredt, skal den:

1. installeres med den færdige `.exe`,
2. åbnes i hver relevant Revit-version,
3. testes i en kopi af en repræsentativ model,
4. gennemgå parameteroprettelse, forslag, preview og skrivning,
5. og afinstalleres eller opdateres via Windows.

Den detaljerede testplan findes i [`docs/TESTPLAN.md`](docs/TESTPLAN.md).

## Kendte begrænsninger

- Add-in'et er ikke digitalt kodesigneret endnu, så Windows kan vise en
  sikkerhedsadvarsel.
- **Erstat** fjerner en ugyldig parameter fra alle kategorier, den er bundet
  til, men tæller kun udfyldte værdier på Areas.
- Hver Revit-årgang anvender sin egen assembly og skal funktionstestes særskilt.
- Der er endnu ikke ribbon-ikon eller permanent revisionslog.
- Releasefiler skal fortsat bygges på en Windows-udviklingsmaskine.

## Dokumentation

- [Brugermanual](docs/BRUGERMANUAL.md)
- [PDF-vejledning v0.6.0](docs/BBR_Arealfordeling_Vejledning_v0.6.0.pdf)
- [Dokumentationsoversigt](docs/DOKUMENTATIONSOVERSIGT.md)
- [Teknisk beskrivelse](docs/TEKNISK_BESKRIVELSE.md)
- [Parameterkontrakt](docs/PARAMETERKONTRAKT.md)
- [Arkitektur](docs/ARKITEKTUR.md)
- [Testplan](docs/TESTPLAN.md)
- [Release-test](docs/RELEASE_TEST.md)
- [Versionsmatrix](docs/REVIT_VERSION_MATRIX.md)
- [Release notes](RELEASE_NOTES.md)

## Support

Ved fejl bør brugeren sende:

- Revit-version,
- en kort beskrivelse af den præcise handling,
- teksten fra fejlmeddelelsen,
- og gerne et skærmbillede.

Kontakt: [nbb@pplusp.dk](mailto:nbb@pplusp.dk)
