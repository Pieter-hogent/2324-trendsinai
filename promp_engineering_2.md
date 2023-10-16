## Prompt Engineering

De vorige les (notebooks/prompt_engineering.ipynb) hebben we een aantal prompt engineering principes gezien die bruikbaar zijn in ChatGPT. Wat nuttig en handig is, en de inzichten in prompts, wijzigen nog continue, maar toch zijn een aantal basis concepten ondertussen te ontwaren.



Alle prompt technieken vallen in vijf categorieën samen te vatten, en een goede prompt gebruikt dan één of meerdere van deze categorieën.

- context
- input data
- instructions
- examples
- constraints



### categorieën

#### context

De context van een prompt beschrijft meestal op welke manier het LLM zich dient te gedragen, "Je bent een expert JavaScript developer", "Je bent een AI assistent die boeken aanraadt op basis van gebruikers voorkeuren", "Je bent een hogeschool lector die bachelorproefvoorstellen evalueert"

#### input data

Input data is de data die beschrijft waar het over gaat bij de prompt, de tekst die moet samengevat worden, de bachelorproef die moet geëvalueerd worden, het stuk code waar uitleg over moet gegeven worden.

#### instructions

De instructions beschrijven wat het LLM moet doen. "Geef 5 aanbevelingen voor nieuwe boeken", "Beschrijf alle schrijf- en grammaticale fouten, geef twee verbeterpunten aan waar de student mee aan de slag kan", "Leg uit wat dit stuk code doet"

#### examples

Soms loont het om voorbeelden van input en output te geven, zeker als er een zekere structuur verwacht wordt. (JSON data met een specifiek formaat bijvoorbeeld)

#### constraints

De constraints dienen om af te lijnen waartoe het LLM zich moet beperken. Dit kan zowel gaan om vormelijke beperkingen "Genereer enkel een JSON file met key 'boek' en key 'author'", "Maximaal 100 woorden lang", "in de stijl van Shakespeare"

Maar ook inhoudelijk "Wees vriendelijk", "Eindig met een positieve noot", "Zorg dat de boeken van hetzelfde genre zijn".



### voorbeeld

```
Je bent aan AI assistent die gepersonaliseerde boek aanbevelingen maakt gebaseerd op gebruikersvoorkeuren.

Gebruiker: man van 46 jaar, houdt van science fiction en non-fictie en leest in het Nederlands en Engels
Favoriete boeken: "Predictably Irrational" van Dan Ariely, "Do Androids Dream of Electric Sheep" van Philip K. Dick en "A Random Walk Down Wall Street" van Malkeil Burton

Kan je vijf aanbevelingen formuleren voor deze gebruiker?

Voorbeeld uitvoer: "The Martian" van Andy Weir, omdat je van science fiction houdt

Zorg dat de aanbevelingen in lijn zijn met de genres die de gebruiker graag leest, en die aanleunen bij zijn favoriete boeken. Prijs geen boeken aan uit het voorbeeld. Wees vriendelijk en wek zin op bij de gebruiker om de boeken die je aanbeveelt te beginnen lezen.
```



## role

Als je de openai api gebruikt kan je telkens ook een `role` meegeven bij elk chatbericht: `system`, `user` of `assistant`. User en assistant zijn gewoon de gebruiker en de chatbot (of ai assistant), je kan op deze manier dus delen van eerdere conversaties terug als context meesturen bij een volgende vraag.

Maar de system role is exact waaraan je dit soort prompts zou meegeven, het legt vast hoe het systeem zich dient te gedragen. Dit heeft twee voordelen

- context: chatbots hebben een beperkte context, naarmate een conversatie vordert vergeten ze wat ze eerder zeiden 
- standvastigheid: de rol die je vastlegt blijft makkelijker behouden, de chat messages met de gebruiker nadien gaan minder snel 'afleiden'



### oefening

Zet je per twee en schrijf een uitgebreide prompt die alle categorieën gebruikt om het volgende te bekomen:

1. Bachelorproef voorstellen evalueren naar inhoud en spelling.
2. De werking van Python code uitleggen en eventueel optimalisaties aanprijzen.

(gebruik je eigen bachelorproefvoorstel / een stuk code uit de les Webservices om dit te testen)





