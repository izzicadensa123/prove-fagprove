# Planlegging Prøve Fagprøve
  <p>05/02/2026 - 13/02/2026</p>

## Målet med oppgaven:
Målet med denne oppgaven er å utvikle en enkel applikasjon hvor brukere kan enkelt opprette, endre, se oversikt over personlig økonomi med bruk av en Utgiftstracker.

- **Applikasjonskomponenter:** Det skal være en klient side og en server side hvor budsjettene og transaksjoner er lagret trygt, og disse blir servert til klient via sikret API.
- **Transaksjonshåndtering:** Mulighet for å legge til, vise, og slette inntekter og utgifter, hver med dato, beløp, og beskrivelse.
- **Budsjettoversikt:** Vis en enkel oversikt over total inntekt, utgift, og gjenværende budsjett.
- **Lagring:** Transaksjonene skal lagres på en måte med sikring for av bare den som laget budsjette har mulighet til å redigere eller slette.
- **Sikkerhet:** Sikre at data ikke er tilgjengelig for uvedkommende.
- **Design:** Det skal være et enkelt design som er brukervennlig og som også følger kravene for universell utforming

## Sjekkliste kjernefunksjonalitet:
- Lag datamodell ved bruk av DrawSQL
- Lag skisse av app ved bruk av Figma som designverktøy
- Lag et "Budsjett Register" med oversikt over Budsjetter
- I hoved visningen ( registeret ) skal det være mulig og opprette nytt budsjett, redigere og slette transaksjoner (om det er ditt budsjett).
  
## Sjekkliste ekstrafunksjonalitet:
- Sortering på pris


## Fremgangsmåte:
1. Jeg skal begynne ved å planlegge datamodellen, layout på app, og hvordan sikkerheten skal virke
2. Lage SQL Objekter
   - Tabeller og trigger
   - Views
   - Procedures
3. Lage app visuelt etter layout plan
4. Legge til planlagt kjernefunksjonalitet i appen, som f.eks. mulighet til å legge inn budsjett, redigere o.l.
5. Finpusse og teste at all kjernefunksjonalitet fungerer som forventet
6. Vurdere ekstra funksjonalitet og om jeg har tid til å implementere det
7. Kjøre testing av appen og lage rapport på det
9. Skrive system dokumentasjon
10. Skrive bruker dokumentasjon om hvordan appen fungerer
11. Gå gjennom presentasjon og evt presentere for andre lærlinger for å finne ut hvilken flyt som passer


## Skisse:
### Data model:
<img width="1863" height="878" alt="skissedatamodell" src="https://github.com/user-attachments/assets/e0040529-7acf-4b30-b90f-92943448d160" />

- **Universal Columns**: Standard kolonner som kommer i alle Omega365 CTP tabeller.
- **Persons**: En system tabell som kommer ferdig definert i et Omega365 instans, har en rad per bruker i systemet og lagrer diverse metadata som navn, epost etc.
- **OrgUnits**: En system tabell som kommer ferdig definert i et Omega365 instans, hjelper med tilgangsfordeling.
- **Rolle / Modul tabeller**: De er system tabeller som kommer ferdig definert i et Omega365 instans, disse blir brukt for tilgangsstyring.
- **Currencies**: En system tabell som kommer ferdig definert i et Omega365 instans, brukt for å lagre ned forskjellige valutaer.
- **Budgets**: En tabell som lagrer alle budsjettene du oppretter.
- **Transactions**: En tabell som lagrer alle transaksjonene som skjer på budskjettet du har oprettet.
- **Transaction Categories**: En tabell som lagrer Transaction Kategorier.

### Grov skisse:
<img width="2331" height="1444" alt="grovskisse2" src="https://github.com/user-attachments/assets/e9206869-2af3-4be4-89e1-14bbc033b2af" />

- Bildet på toppen til venstre er hoved register bilde som lar deg opprette budskjetter redigere budsjette og opprette transaksjoner.
- Bildet på bunn venstre er hvordan modalen til New Budget omtrent kommer til og se ut.
- Bildet på bunn høyre er hvordan modalen til New Transaction omtrent kommer til og se ut.
- Bildet til top høyre viser hvordan register gridden kommer til og se ut når den kollapser seg og da vil det komme flere kolonner inn med mer informasjon ang budsjette som gjør det lettere og få et raskt overblikk over alle budsjettene man har opprettet.
  
## Tidsskjema:
<details>
  <summary>
      Torsdag <sub>05/02</sub>
  </summary>
  <ul>
    <li> Gjennomgang fagprøve - 1t </li>
    <li> Lage datamodell, layout og skrive plandokument - 6.25t </li>
    <li> Logging - 0.25t </li>
    <li> Levere plan - Før 17:00</li>
  </ul>
</details>
<details>
  <summary>
    Fredag <sub>06/02</sub>
  </summary>
  <ul>
    <li>Fortsette med tabellene, views, og procedures - 2t</li>
    <li>Begynne på appene sin kjernefunksjonalitet - 5.25t</li>
    <li>Logging - 0.25t</li>
  </ul>
</details>
<details>
  <summary>
    Helg <sub>07/02 - 08/02</sub>
  </summary>
  <ul>
    <li>Logging - 0.25t</li>
  </ul>
</details>
<details>
  <summary>
    Mandag <sub>09/02</sub>
  </summary>
  <ul>
    <li>Fortsette med appene sin kjernefunksjonalitet - 7.25t</li>
    <li>Logging - 0.25t</li>
  </ul>
</details>
<details>
  <summary>
    Tirsdag <sub>10/02</sub>
  </summary>
  <ul>
    <li>Fullføre appene sin kjernefunksjonalitet og beynne vurdering av ekstrafunksjonalitet - 7.25t</li>
    <li>Logging - 0.25t</li>
  </ul>
</details>
<details>
  <summary>
    Onsdag <sub>11/02</sub>
  </summary>
  <ul>
    <li>Fullføre appene sin kjernefunksjonalitet og implementere ekstrafunksjonalitet - 7.25t</li>
    <li>Logging - 0.25t</li>
  </ul>
</details>
<details>
  <summary>
    Torsdag <sub>12/02</sub>
  </summary>
  <ul>
    <li>Ferdigstille appene - 2t</li>
    <li>Skriv dokumentasjon - 3t</li>
    <li>Utfør og skriv test rapport - 1.25t</li>
    <li>Lag presentasjon - 1t</li>
    <li>Logging - 0.25t</li>  </ul>
</details>
<details>
  <summary>
    Fredag <sub>13/02</sub>
  </summary>
  <ul>
    <li>Presentere løsning for sensor og prøvenemd</li>
  </ul>
</details>

## Teknologi:

- Omega 365 CTP (Core Technology Platform)
  - Jeg har valgt Omega 365-rammeverket som er mest kjent som Appframe for å bygge applikasjonen. Det som gjør Appframe så bra er at det rammeverket håndterer mye av SQL og sikkerhetsfunksjonaliteten noe som gjør det mye mer effektivt og sikrere og utvikle raskt. Omega 365 bruker SQL for databehandling, data lagring og oppbygning av tilgangsstyring. Appframe håndterer alt som har med å få data fra SQL serveren til frontend. Det som gjør Appframe enda bedre er at det har masse innebygs funksjonalitet som diverse utviklerverktøy, innebygde tabeller og innebygd sikkerhetsstyring
  - VueJS 
  - Bootstrap 
  - Microsoft SQL Server
- GitHub
  - For dokumentasjon som er lett å dele med andre
- DrawSQL
  - For visualisering av datamodell
- Figma
  - For skisse av layout på applikasjonen
- Excel
  - For skisse av layout på applikasjonen

## Kostnader:
- Timerate lærling: 161,-
  - Estimert total pris lærling: 6 arbeidsdager * 7.5 timer * 161 = 7 245,-

- Estimert pris for meg som utvikler - tilgang til SQL og Web = 12 000,-

## Kilder:
- Vidar Nordnes (Sjef)
- Tor Halvorsen Aasheim (faglig leder)
- Arie Stensland Haugen (Kollega)
- [ChatGPT](https://chatgpt.com)
- [OmegaDocs](https://docs.omega365.com/?AreaType=10001&Area-ID=10004)
