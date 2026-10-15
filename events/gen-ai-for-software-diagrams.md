# Generative AI for Software Architecture Diagrams

## Exercises

### PlantUML
- [PlantText.com](https://planttext.com/)
- [PlantUML Original](https://www.plantuml.com/plantuml/) | [PlantUML New Web Editor](https://editor.plantuml.com/)
- [Sequence diagram exercise image](../exercises/sequence-exercise.png)
- [PlantUML Sequence diagrams](https://plantuml.com/sequence-diagram)
- [PlantUML colours](https://plantuml.com/color)
  - Colours used in the diagram are: #d4e3e2 pale blue, #599fb0 dark blue, #faa634 yellow, #eb4234 red


### Mermaid
- [Mermaid Live](https://mermaid.live)
- [Mermaid Chart](https://www.mermaidchart.com/play)
- [Sequence diagram exercise image](../exercises/sequence-exercise.png)
- [Mermaid Sequence diagrams](https://mermaid.js.org/syntax/sequenceDiagram.html)
- [Customise Mermaid theme](https://mermaid.js.org/config/theming.html#customizing-themes-with-themevariables)
  - For this exercise you can optionally look at changing *actorBkg*, *actorBorder*, *actorLineColor*, *activationBkgColor*, and *activationBorderColor* (colours from the original diagram are defined as hex values above)

### Structurizr
- [Structurizr Playground](https://playground.structurizr.com/) (no install - use this for the exercise)
- Or run it locally with Docker: `docker run -it --rm -p 8080:8080 structurizr/structurizr playground`
- [What replaced Structurizr Cloud, Lite and the CLI](https://docs.structurizr.com/eol)
- [Context diagram exercise image](../exercises/context-exercise.png)
- [Container diagram exercise image](../exercises/container-exercise.png)


## Further Links
- [JacquiRead.com](https://jacquiread.com)
- [The KataLog](https://github.com/TheKataLog)
- [C4 Model](https://c4model.com)
- [List of text-to-diagram tools](https://xosh.org/text-to-diagram/)
- [Kroki (API endpoint for diagrams-as-code)](https://kroki.io/)
- [D2 Diagrams-as-code](https://d2lang.com/)

### PlantUML
- [PlantUML website](https://plantuml.com/)
- [Render PlantUML on GitHub](https://stackoverflow.com/questions/32203610/how-to-integrate-uml-diagrams-into-gitlab-or-github/32771815#32771815)
- [PlantUML for GitHub browser extension](https://github.com/plantuml/plantuml-for-github) - official, renders PlantUML on GitHub ([Chrome](https://chromewebstore.google.com/detail/plantuml-for-github/lbokhidfopkdehkmlmpaabacljoediic), [Firefox](https://addons.mozilla.org/en-US/firefox/addon/plantuml-for-github/))
- [PlantUML MCP server (`@plantuml/mcp-js`)](https://www.npmjs.com/package/@plantuml/mcp-js) - official, runs locally in Node.js without Java
- [C4-PlantUML](https://github.com/plantuml-stdlib/C4-PlantUML)
- [PlantUML Standard Libraries](https://github.com/plantuml/plantuml-stdlib)
- Using legends in PlantUML:
  - [PlantUML legends](https://plantuml.com/commons#3b05da228ffe6d52)
  - [Table legend hack](https://www.hascode.com/snippet-adding-legends-to-plantuml-diagrams/)
  - [Stack Overflow table legend hack](https://stackoverflow.com/questions/30999290/how-to-generate-a-legend-with-colors-in-plantuml/44217931#44217931)
- [PlantUML styling (replacing skin params)](https://plantuml.com/style-evolution)
- [PlantUML Skin Params](https://plantuml-documentation.readthedocs.io/en/latest/formatting/all-skin-params.html#)
- [PlantUML Icons](https://plantuml.com/creole#041a1eb0031c373d)
- [PlantUML Activity diagram colours](https://plantuml.com/activity-diagram-beta#5d50889672f6f860)
- [HTML colour names](https://www.w3schools.com/colors/colors_names.asp)
- [CSS colour names](https://www.w3schools.com/cssref/css_colors.php)

### Mermaid
- [Mermaid website](https://mermaid.js.org/)
- [Mermaid diagram breaking](https://mermaid.js.org/intro/syntax-reference.html#diagram-breaking) (scroll down a little)
- [Mermaid Flowchart styling](https://mermaid.js.org/syntax/flowchart.html#styling-and-classes)
- [Mermaid styling issue on GitHub](https://github.com/mermaid-js/mermaid/issues/523)
- [Mermaid (horrible) hack for styling sequence diagrams](https://stackoverflow.com/questions/63587556/color-change-of-one-element-in-a-mermaid-sequence-diagram)
- [Mermaid Theming](https://mermaid.js.org/config/theming.html)

### Structurizr
- [Structurizr website](https://structurizr.com/) | [Structurizr documentation](https://docs.structurizr.com/)
- Run Structurizr locally against your own workspace: `docker run -it --rm -p 8080:8080 -v /path/to/workspace:/usr/local/structurizr structurizr/structurizr local`
- [Structurizr DSL basics](https://docs.structurizr.com/dsl/basics)
- [Structurizr DSL language reference](https://docs.structurizr.com/dsl/language)
- Styling:
  - Elements: [DSL Cookbook](https://docs.structurizr.com/dsl/cookbook/element-styles/) | [DSL language reference](https://docs.structurizr.com/dsl/language#element-style)
  - Relationships: [DSL Cookbook](https://docs.structurizr.com/dsl/cookbook/relationship-styles/) | [DSL language reference](https://docs.structurizr.com/dsl/language#relationship-style)
- [Structurizr workspace scope](https://docs.structurizr.com/workspaces/scope)
- [Structurizr exporter comparison](https://docs.structurizr.com/export/comparison)
- [Structurizr Docs](https://docs.structurizr.com/ui/documentation/)
- [Structurizr ADRs](https://docs.structurizr.com/ui/decisions/)

### AI and diagrams-as-code
- [AI patterns and antipatterns for diagrams-as-code](gen-ai-patterns.md) - the patterns and antipatterns from the course, with the conventions file
- [diagrams-as-code skill](https://github.com/the-ministry-of-software-design/ai-skill-diagrams) - Agent Skill for PlantUML, Mermaid and Structurizr ([Agent Skills spec](https://agentskills.io/))
- [Structurizr and AI, including the MCP server](https://docs.structurizr.com/ai)
- PlantUML MCP server: see PlantUML above

### Credits
Credits for the patterns and antipatterns, with full links, are on the [patterns page](gen-ai-patterns.md#credits) and under each pattern.

### Draw.io & Diagrams-as-code
- [Draw.io](https://app.diagrams.net/)
- [Draw.io keyboard shortcuts](https://viewer.diagrams.net/#Uhttps%3A%2F%2Fviewer.diagrams.net%2Fshortcuts.svg%23%7B%22pageId%22%3A%22150dc974-5404-6732-309c-fd6db42db779%22%7D)
- [PlantUML in Draw.io - removed from app.diagrams.net end of 2025](https://www.drawio.com/blog/plantuml-to-mermaid)
- [Mermaid in Draw.io](https://www.drawio.com/blog/mermaid-diagrams)

---
[> Home](../README.md)