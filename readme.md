# GCP Migreringsguide

## Administrativt
1. Alle applikasjoner i NAV skal ha en PVK. 
Her må noen fylle inn mer informasjon om hva man skal gjøre og hvem man kan snakke med.

## Tekniske forutsetninger
###Teamet må legges til i https://github.com/navikt/teams
* Det opprettes en gruppe i Azure AD. Kan verifiseres [her](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ManagedAppMenuBlade/Users/appId/5cbaf0ba-4d99-48a1-acf5-cca701361fd2/objectId/4c5e3226-106e-404d-81be-d02f31104b5a)
* Det opprettes et team i github. Kan verifiseres [her](https://github.com/orgs/navikt/teams)
* Deploy-nøkler for teamet opprettes og hentes [her](https://deploy.nais.io/)
* Brukere i teamet gis tilgang til GCP. Logg på med nav-epost [her](https://console.cloud.google.com/)
* Teamet har fått et eget dev- og prod-prosjekt i GCP: https://console.cloud.google.com/home/dashboard?project=<dev-ditteamnavn>
* Teamet har fått et eget namespace i clusterne: `labs-gcp`, `dev-gcp` og `prod-gcp`
Følg instruksjonene [her](https://doc.nais.io/basics/access) for å installere og sette opp kubectl

### Sikkerhet
I GCP-clusterne våre er det lagt opp til zero-trust, som betyr at applikasjonen må uttrykke hvilke apper den skal snakke med, og hvilke apper som skal snakke med den.
Dette må applikasjonen utrykke i form av en [AccessPolicy](https://doc.nais.io/gcp/access-policy)
I tillegg til å åpne opp for kommunikasjon til og fra andre applikasjoner i samme cluster, åpner dette for kommunikasjon med applikasjoner i andre cluster med access-tokens utstedt av `tokendings`.
(Dette forutsetter at applikasjonen du skal konsumere har implementert validering av tokendings-tokens.)

### Deploy
Man kan bruke samme [deploymekanismer](https://doc.nais.io/deployment) som vanlig, men erstatter `CLUSTER` med hhv `labs-gcp`,`dev-gcp` eller `prod-gcp`

### Ingress
For å nå applikasjonen via ingress, er følgende domener tilgjengelige:

#### Labs-gcp
* labs.nais.io - tilgjengelig fra internett.

#### Dev-gcp
* dev-adeo.no - tilgjengelig kun fra nav-interne nett, scaleft og naisdevice
* dev-nav.no - tilgjengelig kun fra nav-interne nett, scaleft og naisdevice
* dev-gcp.nais.io - tilgjengelig kun fra nav-interne nett, scaleft og naisdevice

#### Prod-gcp
* adeo.no - tilgjengelig kun fra nav-interne nett, scaleft og naisdevice. Hostnavnet må legges inn i DNS for adeo.no og pekes på den interne lastbalansereren: 10.7.8.100
* nav.no - tilgjengelig fra internett. Hostnavnet må legges inn i DNS for nav.no og peke på Googles lastbalanserer: 35.244.216.34
* prod-gcp.nais.io - tilgjengelig fra internett.


