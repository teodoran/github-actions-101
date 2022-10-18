GitHub Actions 101
==================
_Et begynnerkurs i GitHub Actions, hvor du lærer å skrive en CI/CD-pipeline som bygger, tester og deployer en applikasjon._

💡 Hva er dette for noe?
------------------------
Flere ønsker å lære mer om CI/CD-verktøy generelt, og [GitHub Actions](https://docs.github.com/en/actions/learn-github-actions) spesielt. Derfor er dette kurset laget, hvor man ser på CI/CD fra A til Å, med utgangspunkt i GitHub Actions.

Dette er et begynnerkurs i GitHub Actions, og passer for deg som har jobbet lite eller ingenting med dette fra før. Her starter du med en eksempelapplikasjon, og i løpet av 2 timer får du prøve deg på å skrive en CI/CD-pipeline i GitHub Actions som bygger, tester og deployer applikasjonen. I tillegg inneholder repoet en kort presentasjon av de viktigste temaene innen CI/CD generelt.

🚦Hvordan kommer jeg i gang?
----------------------------
Før du kan kjøre applikasjonen lokalt, trenger du å installere [.NET 6.0 SDK](https://dotnet.microsoft.com/en-us/download), og et passende verktøy for å editere kode. Hvis du ikke har noen spesielle preferanser, er [Visual Studio Code](https://code.visualstudio.com/) et greit valg.

For å klone koden, trenger du [Git](https://git-scm.com/downloads). I tillegg trenger du en konto på [GitHub](https://github.com/join) for å kunne bruke [Actions](https://docs.github.com/en/actions/learn-github-actions).

Når vi kommer så langt i kurset at man skal begynne å installere applikasjonen i forskjellige miljøer, trenger du [Docker](https://docs.docker.com/get-docker/) eller [Podman](https://podman.io/getting-started/installation) for å bygge kontainere, og [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) for å orkestrere kontainerene du bygger i Kubernetes.

🐙 Kort om CI/CD
----------------
For at applikasjonene vi lager skal kunne brukes av noen andre enn oss, må vi typisk få de ut et eller annet sted hvor noen andre enn oss kan bruke de. Dette andre stedet kaller vi gjerne produksjon, og på veien ut kan det skje mye rart.

![](Images/fra-utvikler-til-produksjon.png)

_**Spørsmål:** Hva må til for at en applikasjon vi har laget kommer seg ut i produksjon?_

### En ordliste for CI/CD
- **CI:** Kontinuering Integrering (Continuous Integration). Praksis hvor man forsøker å samle kode-endringer fra flere bidragsytere hyppig, ved å automatisk merge inn og teste små endringer kontinuerlig.
- **CD:** Kontinuerlig Leveranse (Continuous Delivery). Tilnærming hvor man blant annet ønsker å redusere risiko for alvorlige feil i produksjon, ved å levere hyppige små endringer.
- **CI/CD-pipeline:** Den automatiske prosessen kode må igjennom for å komme seg fra en utvikler til produksjon.
- **Ressurs:** Noe som applikasjonen vår er avhengig av for å kjøre. Noen typiske eksempler er: En datamaskin (eller tilsvarende) som kan kjøre den ferdige applikasjonen vår. Databaser, køer og filer for å holde på tilstand. Nettverk for å snakke med omverdenen. Andre applikasjoner som vår applikasjon bruker.
- **Delte ressurser:** Ressurser som brukes av andre applikasjoner enn den applikasjonen vi jobber med. Dette eksempelvis være delte servere, nettverk, byggesystemer, API, med mer.
- **Miljø:** En samling av alle ressursene som trengs for å kjøre applikasjonen vi jobber med. Det miljøet som brukerne av applikasjonen bruker, kalles typisk produksjon. Andre viktige miljøer er testmiljøer, som brukes for å sjekke at nye versjoner av applikasjonen fungerer før de installers i produksjonsmiljøet, og lokalt miljø, som brukes av utviklere for å sjekke at applikasjonen fungerer mens man utvikler.
- **Infrastruktur:** Samlingen av alle ressursene og miljøene en applikasjon bruker.
- **Infrastruktur som Kode:** En tilnærming hvor man automatiserer oppsett av infrastruktur ved å uttrykke den som kode.

_**Spørsmål:** Har du hørt noen andre rare CI/CD-ord du lurer på hva betyr? Er det forklaringer i ordlisten over du er uenig i?_

🎬 Litt om GitHub Actions
-------------------------
Actions er et verktøy for å bygge CI/CD-pipelines. Det er tilgjengelig direkte i GitHub, og settes opp ved at man legger inn spesielle YAML-filer i mappen `.github/workflows`.

Hver fil definerer en [workflow](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#workflows). En workflow er en automatisk prosess som vi ønsker at skal kjøre når en spesiell hendelse skjer. Dette kan eksempelvis være at man bygger og tester koden automatisk når det kommer inn nye endringer i en pull-request, eller at man bygger, tester og installerer koden i et miljø når nye endringer kommer inn på main-branchen i repoet.

Hendelsene som starter en workflow kaller vi en [event](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#events). Dette er ofte hendelser knyttet til koderepoet, f.eks. at ny kode kommer inn på en spesiell branch, eller at noe nytt skjer i en pull-request, men det kan også være at man manuelt kan lage en event, eller at man kan trigge eventer jevnlig, for f.eks. å oppdatere pakker som applikasjonen er avhengig av.

Hver workflow består av en eller flere [jobber](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#jobs), som igjen kan bestå av ett eller flere steg. Eksempelvis kan man ha en jobb som bygger applikasjonen, som igjen innholder ett steg som installerer pakker som applikasjonen trenger, og ett steg som kjører en kommando for å bygge koden.

For å gjøre det lettere å lage worksflows som trenger å gjennomføre komplekse, men mye brukte oppgaver, kan man bruke [actions](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#actions). Det finnes mange [ferdige actions](https://docs.github.com/en/actions/learn-github-actions/finding-and-customizing-actions) man kan bruke, og man kan [lage sine egne actions](https://docs.github.com/en/actions/creating-actions) hvis man trenger det.

Når en workflow skal kjøre, trenger den et sted å kjøre. Dette kaller man [runners](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#runners). GitHub leverer noen ferdige runnere som man kan bruke, og det går også ann å sette opp sine egne.

### Hei på deg GitHub Actions!
Start med å forke repoet [github-actions-101](https://github.com/teodor-elstad/github-actions-101), sånn at du får en kopi av repoet knyttet til din egen GitHub-bruker. Deretter er det bare å klone din kopi av repoet lokalt på maskinen din.

Lag en fil som heter `hello-actions.yml` under mappen `.github/workflows`. Lim in koden under, og sjekk den inn. Gå til Actions-fanen i din klone av "github-actions-101"-repoet, og se hva som skjer her.

```yaml
name: "Hello Actions!"

on:
  workflow_dispatch:
  push:

jobs:
  say-hello:
    runs-on: ubuntu-latest
    steps:
      - name: "Echo Greeting"
        run: "echo 'Hei på deg GitHub Actions!'"
      - name: "Echo Goodnight"
        run: "echo 'Natta!'"
```

🏗️ Vi bygger en CI/CD-pipeline!
-------------------------------
Nå skal vi ta ibruk GitHub Actions til å lage en enkel CI/CD-pipeline som bygger, tester og installerer _Sticky Notes_ applikasjonen som finnes i dette repoet.

![](Images/masse-fra-utvikler-til-produksjon.png)

_Herfra blir det code-along med workshop-verten._

Notater
-------

### Vi setter opp AKS
Hente nøkler for å koble på klusteret `devops-101-cluster` (krever at man har både kubectl og Azure CLI installert):

Forteller Azure CLI hvilken subscription man skal jobbe med.
```shell
$> az login
$> az account set --subscription 3a72c7bc-dae2-4dd1-8fed-89110003b86a
```

Forteller Azure CLI at man skal hente ned nøkler til kubectl for klusteret `devops-101-cluster`.
```shell
$> az aks get-credentials --resource-group tae-devops-101-workshop --name devops-101-cluster
```

Noen ting man skal kunne gjøre når dette er på plass:
```shell
# List all deployments in all namespaces
$> kubectl get deployments --all-namespaces=true

# List all deployments in a specific namespace
# Format :kubectl get deployments --namespace <namespace-name>
$> kubectl get deployments --namespace kube-system

# List details about a specific deployment
# Format :kubectl describe deployment <deployment-name> --namespace <namespace-name>
$> kubectl describe deployment my-dep --namespace kube-system

# List pods using a specific label
# Format :kubectl get pods -l <label-key>=<label-value> --all-namespaces=true
$> kubectl get pods -l app=nginx --all-namespaces=true

# Get logs for all pods with a specific label
# Format :kubectl logs -l <label-key>=<label-value>
$> kubectl logs -l app=nginx --namespace kube-system
```

Nyttige lenker:
- [kubectl overview](https://kubernetes.io/docs/reference/kubectl/)
- [kubectl getting started guide](https://kubernetes.io/docs/setup/)
- [kubectl cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- [kubectl reference](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)

### Vi setter opp ACR
Registry heter `devops101registry.azurecr.io`.

[Denne guiden](https://learn.microsoft.com/en-us/azure/container-registry/container-registry-get-started-portal?tabs=azure-cli#log-in-to-registry) er fin.

#### Vi kobler sammen ACR og AKS
Her er det greit å se på [denne guiden](https://learn.microsoft.com/en-us/azure/aks/cluster-container-registry-integration?tabs=azure-cli).

Kommandoen under fungerer ikke fordi man ikke er owner av subscription ressursen er satt opp i. Får prøve å gjøre det manuelt når den tid kommer.
```shell
$> az aks update -n devops-101-cluster -g tae-devops-101-workshop --attach-acr devops101registry
```

```shell
$> kubectl create secret docker-registry devops101registry-credentials --docker-server=devops101registry.azurecr.io --docker-username=devops101registry --docker-password={replace with password} --docker-email=Teodor.Ande.Elstad@nrk.no
```

### Vi bygger en Docker-kontainer
```shell
$> docker build -f Notes.Api/Dockerfile -t devops101registry.azurecr.io/notes-api:0.0.1 ./
```

```shell
$> docker run -it -p 8000:80 notes-api:0.0.1
```

### Vi pusher image til ACR

```shell
$> docker login devops101registry.azurecr.io
```

```shell
$> docker push devops101registry.azurecr.io/notes-api:0.0.1
```

### Vi kjører opp Notes.Api på Kubernetes
```shell
$> kubectl apply -f Notes.Api/Kubernetes.yaml
```