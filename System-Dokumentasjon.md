# System Dokumentasjon

Dette er systemdokumentasjonen til fagprøve oppgaven min som er å lage en Utgifts Tracker. Her dekkes hvilke teknologier som blir brukt, hvordan datamodellen ser ut, hvordan sikkerheten fungerer, hvordan frontenden fungerer, hvilke problemstillinger som ble møtt på, planen videre, og hvilke avvik jeg gjorde fra den originale planen.

## Innhold
- Oppsummering applikasjon
- Teknologier
- Database modell
- Sikkerhet
- Frontend
- Problemstillinger
- Planen videre
- Avvik fra plan

<details>
<summary>
    Oppsummering applikasjon
</summary>
<p>Oppgaven denne applikasjonen skal la bruker legge inn transaksjoner ( inntekt og utgifter ) for og ha oversikt over personlig økonomi og gis mulighet til og opprette budsjetter for og holde styr på egne inntekter og utgifter.</p>
</details>
<details>
    <summary>
        Teknologier
    </summary>
    <ul>
        <li>Omega 365 CTP</li>
        <li>Microsoft SQL Server</li>
        <li>Vue.js 3</li>
        <li>Bootstrap 5.3</li>
        <li>HighCharts</li>
    </ul>
</details>
<details>
    <summary>
        Database modell
    </summary>
  <img width="1884" height="1007" alt="image" src="https://github.com/user-attachments/assets/34d40606-ce70-4797-ab45-041b5b23f26c" />
  <p>Datamodellen her er egentlig ganske selvforklarende og rett frem. Den består av 9 custom tabeller og 1 Omega 365 system tabeller.</p>
    <!-- <small>FK - Foreign Key </small> -->
    <details>
        <summary>Tabeller</summary>
        <ul>
            <li>Budgets - dette er tabellen der alle .</li>
            <li>Transactions - dette er historikk tabellen der alle budsjettene man oppretter blir lagret med til og fra dato, tittel, description, total estimat/budget, valuta og mulighet til og kategorisere budsjettet ditt.</li>
            <li>Transactions - dette er historikk tabellen der alle transaksjonene man oppretter blir lagret ned med dato transaksjons dato, type, categori, kost, valuta og hvilken konto det kommer ut/ inn ifra.</li>
            <li>TransactionsTEMP - dette er historikk tabellen der alle transaksjonene blir lagret midlertidig før de blir opprettet på sikkelig og sent til Transactions tabellen. Denne tabellen er identisk til Transactions tabellen .</li>
            <li>Accounts - dette er historikk tabellen der alle Bank kontoene til brukeren ligger, dataen som er lagret her er Personen det gjelder og hvilken kontoer en har.</li>
            <li>TransactionTypes - dette er historikk tabellen der alle transaksjons typene ligger ( income og expense ).</li>
            <li>BudgetCategories - dette er historikk tabellen der alle budsjett kategoriene ligger lagret.</li>
            <li>BudgetTransactionCategories - dette er historikk tabellen der alle transaksjons kategoriene blir lagret ned tilhørende til en budjsett kategory .</li>
            <li>Currencies - dette er historikk tabellen der alle de forskjellige valutaene ligger lagret ned med deres valutakurs.</li>
            <li>Persons - dette er tabellen der alle brukere i Omega 365 instanset er lagret.</li>
        </ul>
    </details>
    <details>
        <summary>Views</summary>
        <ul>
            <li>BudgetsRegister - view som henter ut metadata til Budgets og joiner på linke tabeller for å hente data som budsjettkategori, total brukt i løp av det budsjette og hvor mye du har resterende av det budsjette du har satt ( oversteget / Underskredet ). </li>
            <li>TransactionsOverview - view som henter ut metadata til transaksjonen og joinser seg med linke tabellene for å hente ut data som transaksjons kategorien, transaksjons typen og valutaen.  </li>
            <li>AccountOverview - view som henter metadata til Accounts og joiner på linke tabeller for å hente ut valutaen, total inntekt, total trekk og balansen til brukeren ( inntekt - trekk ). </li>
            <li>MonthlyTransactionsHighChart - view som henter ut kosten i måneden som blir da brukt i line charten. </li>
            <li>BudgetTransactionCategories - view som henter ut metadata om hvilken transaksjons kategori tilhører budsjett kategorien er. </li>
            <li>TransactionsTEMP - view som er direkte kopi av TransactionsOverview viewet men istedefor at den bruker Transactions tabellen så henter den dataen fra TransactionsTEMP. </li>
        </ul>
    </details>
    <details>
        <summary>Procedures</summary>
        <ul>
            <li>CreateTransactions - dette er prosedyren som kjøres når en oppretter transaksjoner, den tar alle radene som ligger i TransactionsTemp tabellen og inserter de i den sikkelige Transactions tabellen. </li>
            <li>CreateBudget - dette er prosedyren som kjøres når en oppretter budsjetter, den tar inn parameterene: Tittel, Description, From Date og To Date, TotalBudget, Currency, Person_ID og BudgetCategory </li>
        </ul>
    </details>
</details>
<details>
    <summary>
        Sikkerhet
    </summary>
      <p>Hele løsningen støtter seg på sikkerheten i Omega 365 CTP. Hoved konseptet i sikkerheten er moduler, capabilities, og roller. En modul en der man definerer tilgang på tabeller og apper. Roller kan da videre kobles opp til disse modulene, men en rolle er ikke begrenset til bare en modul, den kan ha flere forskjellige moduler på en rolle. Capabilities er "spesialtilganger" som blir gitt til noen individe roller for enda strengere sjekker. </p>
  <p>I Omega 365 CTP har vi flere måter å autentisere på, mest vanlig er SQL login eller Microsoft login. Med en SQL login som består av ett brukernavn og passord, kjøres ett sikkert API kall til SQL serveren for å autentisere brukeren. Med Microsoft login blir dette sendt til Microsoft og blir sjekket av dem. Om du blir autentisert av Microsoft returnerer de en token slik at Omega 365 vet at du er autentisert.</p>
  <p>Utover dette er to faktor autentisering (2FA) høyst anbefalt. Det er flere forskjellige tilgjengelige valg som: SMS, Email, Time-based One Time Password (TOTP), og Passkey.</p>
  <p>Sikkerheten i views er egentlig ganske enkel siden den sjekker bare om du lesetilgang på tabellen. For økt sikkerhet har brukere aldri direkte tilgang til tabeller. I stedet brukes views, ofte via en atbv som er automatisk generert for alle nye tabeller. "atbv" står for Application Table View og brukes for å kontrollere hva en bruker kan se.</p>
  <p>Sikkerheten i triggere er litt mer avansert, siden her må det sjekkes på om du har redigering/slette tilganger. </p>
  <br/>
  <details>
      <summary>Triggers</summary>
      <ul>
          <li>Standard Insert Permission Sjekk - dette er prosedyren som kjøres når en oppretter transaksjoner, den tar alle radene som ligger i TransactionsTemp tabellen og inserter de i den sikkelige Transactions tabellen. </li>
          <img width="800" height="360" alt="image" src="https://github.com/user-attachments/assets/24a9761d-0630-4bb4-a0d1-84326029317f" />
          <li>Standard Update Permission Sjekk - dette er prosedyren som kjøres når en oppretter transaksjoner, den tar alle radene som ligger i TransactionsTemp tabellen og inserter de i den sikkelige Transactions tabellen. </li>
          <img width="881" height="341" alt="image" src="https://github.com/user-attachments/assets/82a24c40-ea22-459b-8eb1-3c9714b3e218" />
          <li>Standard Delete Permission Sjekk - dette er prosedyren som kjøres når en oppretter budsjetter, den tar inn parameterene: Tittel, Description, From Date og To Date, TotalBudget, Currency, Person_ID og BudgetCategory </li>
          <img width="832" height="296" alt="image" src="https://github.com/user-attachments/assets/4104e22c-ad0d-4572-8524-2fbfd7482356" />
      </ul>
      <br/>
      <br/>
      <ul>
          <li>Person Insert Sjekk - Denne sjekker om du oppretter budsjetter for en person som ikke er deg selv og vil da ikke la deg gjøre det og throwe en feil melding, den har også standard sjekken </li>
          <img width="840" height="482" alt="image" src="https://github.com/user-attachments/assets/afc117f0-7ba6-4476-ae45-c1c43b97873a" />
          <li>Person Update Sjekk - Denne sjekker på det samme som insert triggeren når det kommer til personer og vanlig tabell tilgangs sjekk </li>
          <img width="993" height="538" alt="image" src="https://github.com/user-attachments/assets/1cb7ba83-fc1e-4c46-b4cc-1f073ea8c4e3" />
          <li>Person Delete Sjekk - Denne sjekker på akkurat det samme som de andre om personen som prøver og slette budsjette sletter sitt eget eller noen andre sin  </li>
          <img width="1107" height="538" alt="image" src="https://github.com/user-attachments/assets/f1e9536d-7d2b-4669-a5e4-f036ed6cd3cd" />
      </ul>
  </details>
  <details>
      <summary>Roles</summary>
      <ul>
          <img width="1093" height="613" alt="image" src="https://github.com/user-attachments/assets/f53b8f63-48f4-4ee9-81e2-893b4a506fb1" />
          <li>Her er et bilde av hvordan strukturen på vår access modell i Omega 365, En rolle kan ha flere moduler og capabilities på seg, moduler er hvor du kan definere app tilganger og tabell permissions på mens capabilities blir brukt til og definere mer spesifike tilgangs sjekker. </li>
          <li>Siden appen min er en utgifts tracker har jeg bestemt meg for at alt skal være filtrert på person nivå ( altså at ud kun skal se din egen data og ingen andre enn deg selv skal ha tilgang til dette ) </li>
          <li>I Utgifts Trackeren har jeg laget til 2 roller, en for vanlige brukerer som kun skal ha tilgang til og kunne bruke appen og inserte, redigere og delete på sin egen data og en rolle for utviklerer som da har delete og insert permissions på alle tabeller ( f.eks kategori tabellene og type tabellene er det bare utviklere som har edit permissions på ) </li>
      </ul>
  </details>
</details>
<details>
    <summary>
        Frontend
    </summary>
    <p>
        Denne løsningen består av 2 skjermbilder; Utgifts Register, Budget Categories.
    </p>
    <ul>
        <img width="2557" height="1170" alt="image" src="https://github.com/user-attachments/assets/83f08dec-b66a-47c7-b862-d60b0debfbb7" />
        <li>Utgifts Tracker (Overview Tab) - Denne tabben viser deg et rask overblikk over økonomien på personen, her ser du hvilken Account som du er på hvor mye penger du har på konto, hvilken valuta. Den viser også total inntekt fra når du har lagt inn din første transaksjon til din siste og total utgifter og til slutt totalen ( inntekt - utgift ). Under der igjen så ser du en månedlig oversikt over alle inntekter den måneden og ved siden av ser du den dataen i en chart</li>
        <img width="2546" height="1271" alt="image" src="https://github.com/user-attachments/assets/b7fc9a86-53ff-4fc0-b0a2-e8b8f2b4ab20" />
        <li>Utgifts Tracker (Transactions Tab) - Denne tabben viser oversikt over alle transaksjoner du har hatt i total, siden dette er en grid så er det filtrerings mulighet direkte på grid men også en grupperings funksjon hvor du kan f.eks gruppere på transaksjons type/ kategori og de andre kolonnene. I denne tabben er også hvor man oppretter transaksjonene på knappen der det står Create New Transactions</li>
        <img width="2542" height="1270" alt="image" src="https://github.com/user-attachments/assets/94dad0c1-57b5-4fb0-9f7f-556ba67fceb0" />
        <li>Utgifts Tracker (Budgets Tab) - Denne tabben viser deg oversikt over alle budsjettene du har opprettet med et register grid som viser deg et rask overblikk over alle budsjettene dine men også et detalje view der du får se mer informasjon om budsjette du står på men også de transaksjonene som har skjedd mellom from and to daten som du har satt, og viss du har satt en kategori så filtrerer det også ned hvilken transaksjoner som blir vist.</li>
        <img width="2548" height="1167" alt="image" src="https://github.com/user-attachments/assets/a7f98cb6-0a18-4cc7-bbd0-62abfea73e75" />
        <li>Budget Categories - denne appen er en setup app som ligger i O365 Setup som vil da bare si en app for og se informasjon/ redigere ( dette er noe som bare de me rollen Utgifts Tracker Developer skal kunne redigere, readonly for brukere ). I denne appen ser du oversikt over hvilket transaksjons kategorier som eksisterer og hvilken budsjetts kategorier som eksisterer og hvor transaksjons kategorien går under hvilken budsjett kategori .</li>
        <br/>
        <br/>
        <p>Siden disse appen bruker Omega 365 CTP, håndteres mye av kommunikasjonen med SQL Server via et mellomlag ved hjelp av DataSources. Enkelt forklart er DataSources JSON-objekter som definerer et view. Disse sendes til .NET Core API-et for verifisering. Hvis verifiseringen godkjennes, sendes forespørselen videre til SQL Server. Ved å bruke dette konseptet håndterer API-et sikkerhetstiltak som beskyttelse mot SQL Injection, slik at dette ikke trenger å implementeres direkte i frontend.</p>
   </ul>
</details>
<!-- <details>
    <summary>
        Info Til Neste Utvikler
    </summary>
    <ul>
        <li>Koden er vell strukturert så det skal være lett og finne fram hvilken fil som hører til hvor og følger naming på variabler er utrolig viktig at du fortsetter med slik</li>
    </ul>
</details> -->
<details>
    <summary>
        Problemstillinger under utvikling
    </summary>
    <ol>
        <li>Monthly Balance Overview Chart - Charten var litt av en challenge siden jeg ikke har vært borti det så mye, så og formattere datoer og verdiene som skulle bli sendt in var en ting jeg brukte en del tid på</li>
        <li>Overview Layout - Dette var noe jeg slet litt med og endret et par ganger med tanke på hva som hadde blitt mest intuitivt for bruker så det ble et par endringer her og endring på formattering og hadde en del problemer med reaktivitet når det kom til charten siden den ikke ville endre seg basert på screen width, til slutt fikk jeg det til men det tok litt tid på og debugge dette og landet til slutt på en layout som jeg syntes var mest oversiktlig for bruker.  </li>
    </ol>
</details>
<details>
    <summary>
        Planen videre
    </summary>
    <ul>
        <p>Her er tingene jeg hadde planlagt videre, men som jeg ikke fikk tid til:</p>
        <li>Muligheten for og legge til flere kategorier på et budsjett ( per nå så er det bare mulig og legge til en kategori på et budsjett ) </li>
        <li>Fjerning av transaksjoner ( dette er egentlig relativt lett men var usikker på om jeg ville la bruker få muligheten til og slette transaksjoner men heller få dem til og legge inn en linje som en korrigering av feil transaksjoner ) </li>
        <li>Legging inn av transaksjoner bak i tid ( dette ville krevd omskriving i flere views og direkte i app og procedure og endring av flere tabeller men er en drit fin funksjon siden per nå så baserer den seg på Created feltet som er da transaksjonen blir lagt inn ( opprettet ) men vil da si at det ikke er mulighet for å legge inn en bak i tid eller fram i tid ) dette tenkte jeg egentlig på og endre siste dagen men dersom det ble for kort tid så legger jeg det heller til i lista som en ToDo </li>
        <li>Mulighet til og legge inn flere Accounts typ Sparekonto, Brukskonto BSU konto. Det dette ville innebært er og lagd en ny tab for administrering av Kontoer ( dette er noe som tabellstrukturen min støtter men har ikke fått tid til og implementere )</li>
        <li>Mulighet til og endre valuta og basert på valutaen skal exchangeraten endre prisen ( dette er noe datamodellen min støtter per nå men har ikke lagt mulighet for convertering men skal ikke være så altfor vanskelig og implementere ) </li>
    </ul>
</details>
<details>
    <summary>
        Avvik fra plan
    </summary>
    <ul>
        <p>Her er avvikene jeg har hatt fra den originale planen med begrunnelse:</p>
        <li>De to første dagene så planla jeg appen men tror jeg forvirret meg veldig i starten så fikk meg til og revurdere hele appen som da ledet til at jeg planla en ny app ( ikke fra scratch men heller la til ekstra på den første planen jeg hadde ) og endret på tabell strukturen og logikk på hvordan appen skal fungere. Da ble dette egentlig et stort avvik dersom det ble omskriving av oppsett/ logikk men også layout så da begynte jeg og revisere på søndagen som da betyr at jeg fikk mindre tid til og holde på med apputvikling enn det jeg ville</li>
        <li>Når det kommer til oppretting av transaksjoner så får man ikke sette opp hvilken dato dette skjedde selv men går heller basert på created dato så et avvik der er at den funksjonen ikke ble lagt til/ endret i tide siden datamodellen min ikke håndterte det og det ble for sent og implementere det nå dersom det created feltet blir brukt alle steder men viss jeg hadde fått tid så hadde jeg lagt til støtte for dette.</li>
        </ul>
</details>
