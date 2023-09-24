# Trends in AI

Cursus zal in de loop van het jaar verschijnen (en wijzigen) op de github repo, markdown files, python notebooks, misschien zelfs powerpoints

## inleiding

Er zijn grosso modo twee grote delen in deze cursus, enerzijds vanalles over en met LLM’s (toch wel dé trend van het laatste jaar binnen A.I.) en anderzijds een deel dat zich meer over ethische problemen met A.I. buigt, met vooral de focus op explainability.

Dit vak wordt de eerste keer gegeven dit jaar, en dus is het cursusmateriaal nog in volle ontwikkeling. Ook is het soms wat moeilijk inschatten hoelang we nodig hebben om sommige onderwerpen te behandelen, dus alles is sowieso onder veel voorbehoud.
Maar interessant wordt het zeker, toch één zekerheid.

## onderwerpen

- prompt engineering
- vector databases
- langchain
- LLM case uitwerken
- Bias
- Saliency mapping
- Shapley values
- Adversarial AI
- ?

## evaluatie

 100% schriftelijk examen (1e en 2e zit)
(virtuele campus: 100% mondeling examen)

## nodig

### OpenAI API

In het eerste deel van de cursus, waar we LLM’s gebruiken, gaan we niet continu in een ChatGPT venstertje typen, maar vanuit Python notebooks de API aanspreken. Daarvoor hebben we een account en een API key nodig.
De ChatGPT API is gratis de eerste drie maanden, met een limiet van $18 (wat vrij veel is voor wat wij gaan doen).
Maak een account aan op [OpenAI Platform](https://platform.openai.com/) daar kan je dan ook een API key genereren ([OpenAI Platform](https://platform.openai.com/account/api-keys)), kopieer deze ergens op een veilige plaats, en hou hem geheim. (je kan hem op de site van OpenAI niet meer terug zien, enkel een nieuwe genereren)

Als je je credits al opgebruikt hebt, of na de drie maand, is er jammer genoeg geen manier meer om de API gratis te gebruiken. Dan zal je ofwel je credit card moeten bovenhalen, of zal je een gratis alternatief moeten gebruiken.
Gelukkig zijn er gratis alternatieven die de API van OpenAI implementeren, zoals [GPT4All](https://gpt4all.io/index.html), je kan deze modellen lokaal op je laptop draaien, de resultaten zijn (uiteraard?) verre van dezelfde kwaliteit dan bij ChatGPT, maar je kan tenminste zien dat alles werkt (of als je je credit card bovenhaalt voor ChatGPT, eerst lokaal alles aan de praat krijgen voor je je credits aanspreekt)

```bash
> git clone git@github.com:nomic-ai/gpt4all.git
> cd gpt4all-api
> DOCKER_BUILDKIT=1 docker build -t gpt4all_api --progress plain -f gpt4all_api/Dockerfile.buildkit .
> docker compose up --build gpt4all_api

```

Maar…
[Docker not starting on Mac · Issue \#1432 · nomic-ai/gpt4all](https://github.com/nomic-ai/gpt4all/issues/1432#issuecomment-1722590241)
(17 september… deze cursus maken is echt fun)

### docker met python libraries

We gaan met python notebooks werken, aangezien de python library wereld nog een grotere soep is dan de JavaScript nodejs verschrikking heb ik getracht om een docker te genereren met alle nodige libraries.

Creëer de folder `docker-hogent-trendsinai` en plaats daar een `Dockerfile` met de volgende inhoud
(best mogelijk dat naarmate de cursus verder uitgewerkt wordt er hier nog libraries gaan bijkomen, maar dat zien we dan wel)

```dockerfile
FROM jupyter/tensorflow-notebook:latest

ENV OPENAI_API_KEY=SET_YOUR_KEY

RUN echo "The OPENAI_API_KEY is set to $OPENAI_API_KEY"

RUN set -ex \
   && conda install --quiet --yes \
   # choose the Python packages you need
   'openai==0.27.8' \
   'python-dotenv==1.0.0' \
   'shap=0.42.0' \
   'pytorch=2.0.0' \
   'torchvision=0.15.2' \
   'langchain=0.0.239' \
   'chromadb=0.3.26' \
   # 'captum=0.6.0' \
   && pip install --no-cache-dir "git+https://github.com/pytorch/captum.git" \
   && pip install gpt4all \
   # && pip install --no-cache-dir tensorflow-metal \
   && conda clean --all -f -y \
   && pip cache purge \
   # install Jupyter Lab extensions you need
   && jupyter lab build -y \
   && jupyter lab clean -y \
   && rm -rf "/home/${NB_USER}/.cache/yarn" \
   && rm -rf "/home/${NB_USER}/.node-gyp" \
   && fix-permissions "${CONDA_DIR}" \
   && fix-permissions "/home/${NB_USER}"%
```

- De captum library (later nodig voor de explainability lessen) gaf fouten in docker, de bug was gefixt in git maar er is nog geen release gebeurd met die fix, dus we doen een rechtstreekse pip install van de github repository.
- gpt4all gaf bij mee ook hier errors (dat ding is echt nog niet stabiel), maar in de .so library (dus enkel relevant op unix systemen, hoop ik), dat zou normaal wel moeten werken op Windows.

Binnen deze folder kan je dan de docker bouwen met

```bash
docker build --rm -t docker-hogent-trendsinai .
```

Als alles goed gaat zou het volgende commando je container moeten tonen

```bash
docker images -a
```

Maak nu een folder `trendsinai` met daarin een folder `notebook` en een `.env` file
In de `.env` file moet je de OpenAPI key plaatsen

```bash
OPENAI_API_KEY=sk-XXXXXXXXXXX
```

Dan kan je de container als volgt starten.

```bash
docker run -d -p 8888:8888 -v /Users/pieter/trendsinai/notebooks:/home/jovyan/work/trendsinai --env-file /Users/pieter/trendsinai/.env docker-hogent-trendsinai
```

Als de container correct opstart staat hij in de lijst van processen

```bash
docker ps -a
```

Docker geeft elke container een naam die gevormd wordt door een paar random woorden aan elkaar te plakken

```bash
CONTAINER ID   IMAGE                              COMMAND                  CREATED        STATUS                      PORTS                              NAMES
fd089ff162a8   879856def295                       "tini -g -- start-no…"   43 hours ago   Up 43 hours (healthy)       0.0.0.0:8888->8888/tcp             funny_bartik
```

In mijn geval `funny_bartik`
Dan kan je de logs als volgt opvragen

```bash
docker logs funny_bartik
```

En daar in zou dan een url moeten staan die je kan gebruiken om de notebooks te openen in je browser.

Als je een mac gebruikt en GPT4All wilt gebruiken zal je toch nog moeten terugvallen op een anaconda environment of iets dergelijks voorlopig, maar anders staat alles nu klaar
