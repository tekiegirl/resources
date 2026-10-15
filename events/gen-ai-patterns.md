# AI Patterns and Antipatterns for Diagrams-as-Code

These patterns and antipatterns come from the O'Reilly live course *Generative AI for Software Architecture Diagrams* by Jacqui Read. They describe what helps, and what goes wrong, when you use a large language model (LLM) to create or change diagrams-as-code in PlantUML, Mermaid or Structurizr DSL. Each one works whether you use a chat window or an AI agent.

All of them come from Jacqui Read's own experiments with LLMs and diagrams-as-code since 2024. Where other people have described something similar, their work is credited on that pattern.

Every pattern fits one of two sides of communication. Some are about how you communicate **to** the LLM, because the LLM is your audience. Others are about what the LLM communicates **for** you, because the people who read the diagram are the audience.

In this page, **LLM** always means the AI. The word **model** on its own means an architecture model, such as a Structurizr workspace, which is what [Model First, Export Views](#model-first-export-views) is about.

An **agent** here means an AI tool that can read your files, run tools (such as a diagram renderer) in a loop, and load your instructions by itself. A **chat** is a chat window where you copy and paste in and out yourself.

## At a glance

| Pattern | The antipattern it prevents |
|---|---|
| [Ask for Code, Not Pictures](#ask-for-code-not-pictures) | [Pixel Diagrams](#pixel-diagrams) |
| [Write a Brief](#write-a-brief) | [Invented Syntax](#invented-syntax) |
| [Render, Then Check](#render-then-check) | [Trusting the Edges](#trusting-the-edges) |
| [Data First](#data-first) | [Trusting the Edges](#trusting-the-edges) |
| [One Level per View](#one-level-per-view) | [Mixing Levels of Abstraction](#mixing-levels-of-abstraction) |
| [Model First, Export Views](#model-first-export-views) | [Translation Drift](#translation-drift) |
| [Start from Artefacts](#start-from-artefacts) | [Generic Architecture](#generic-architecture) |
| [Encode Your Conventions](#encode-your-conventions) | [Instruction Bloat](#instruction-bloat) |

One more antipattern, [Patch Spiral](#patch-spiral), is what happens when you keep asking for fixes instead of improving your brief. That makes eight patterns and eight antipatterns.

## Patterns

### Ask for Code, Not Pictures

Ask the LLM for diagrams-as-code, not for an image. The code is the diagram; a picture is just one view of it.

**Why:** An image can't be compared with the previous version, edited, stored in version control or checked arrow by arrow. Images made by AI image generators also tend to have misspelt labels and arrows that don't join up.

**How:** Start your prompt with "Return <notation> code only", for example "Return PlantUML code only". If you get a picture back anyway, ask for the code.

**Check:** Could you commit the answer to Git and compare it with the next version?

**In a chat:** Put "Return PlantUML code only" (or Mermaid, or Structurizr DSL) at the top of the prompt, then render the code yourself in an online editor.

**With an agent:** The agent can also write the code to a file and render it for you.

**Add to your conventions file:**

```
Always return diagrams as code (PlantUML, Mermaid or Structurizr DSL), never as an image.
```

**Common questions:**

- *AI image generators draw good diagrams now, don't they?* Even a perfect picture can't be compared, edited or checked one relationship at a time, so you still want the code.
- *The chat showed me a diagram, not code. What now?* If the chat rendered the diagram itself (for example Mermaid), there is code behind it. Ask for the code and keep it.

*Credit:* Jacqui Read's experiments (Sep 2024, Feb 2026).

### Write a Brief

Give the LLM a short brief before you ask for a diagram, in the same way you would brief a person who was drawing it for you. Cover four things: the notation and its version, the level of detail, the audience, and any layout you need, described in the layout engine's own terms.

**Why:** Without a brief, the LLM fills the gaps with its own defaults. It may also invent syntax that doesn't exist, such as made-up layout "commands".

**How:** For layout, say what the diagram engine should do, for example "swap the order of these two branches" or "use left to right direction". Don't describe the result you want to see, such as "put it on the right" or "make it cleaner", because the engine has no instruction for that and the LLM will guess.

**Check:** Could a human diagrammer act on your brief without asking you a question?

**In a chat:** Put the four lines of the brief at the top of every prompt.

**With an agent:** Keep the brief in your conventions file, so the agent loads it every time.

**Add to your conventions file:**

```
Layout: say what the engine does (order, direction), not the result you want to see.
```

**Common questions:**

- *Isn't this just prompt engineering?* It is a diagram-specific version of it. The test is whether a human diagrammer could act on the brief without asking anything.
- *Don't LLMs work this out by themselves now?* Without a brief they still fill the gaps with their own choices. In the course experiments, an LLM added a parallel block to a sequence diagram that nobody had asked for.

*Credit:* Jacqui Read's experiments (Sep 2024, Sep 2025). [Oliver (Feb 2026)](https://medium.com/@davidroliver/why-your-ai-generated-c4-diagrams-look-terrible-and-how-to-fix-them-37fe56cd5171) on layout instructions.

### Render, Then Check

Two separate checks: first that the code renders, then that the diagram says what your source says. In between, ask the LLM for a list of what it changed.

**Why:** A diagram can render perfectly and still be wrong. LLMs are much better at getting the boxes right than the arrows, and they sometimes add things nobody asked for.

**How:** Render the code and give any error message back to the LLM, but only once or twice before you step in yourself. Ask it to "list every change you made". Then compare the relationships with your source, not just the boxes.

**Check:** Did it change anything you didn't ask it to?

**In a chat:** Paste the code into an online editor yourself, paste any error back into the chat once or twice, then check the result against your source.

**With an agent:** The agent can render the code and feed errors back automatically. Review the changes as a diff in Git.

**Add to your conventions file:**

```
Render before you show me. List every change. Never remove what you weren't asked to.
```

**Common questions:**

- *If I have to check everything, what's the point?* Checking a diagram is much quicker than drawing it, and you would review a colleague's diagram too.
- *Can't another LLM do the checking?* For checklist-style checks, yes. For "is this relationship real?", no, because that is exactly where LLMs are weakest.

*Credit:* Jacqui Read's experiments (Sep 2024, Sep 2025). [R2ABench (Apr 2026)](https://arxiv.org/html/2604.06683v1); [DiagramEval (2025)](https://arxiv.org/html/2510.25761v2); [GenAIScript (May 2025)](https://microsoft.github.io/genaiscript/blog/mermaids/). "Feedback Loop" by Nitsan Avni, from Augmented Coding Patterns, licensed under CC BY 4.0. https://ai-coding-patterns.dev/patterns/feedback-loop/

### Data First

Get the structure as data before you ask for the picture. Ask for the elements and relationships as a table, YAML, Markdown or CSV, check that, and then generate the diagram from it.

**Why:** It is much quicker to check a list of relationships than to trace arrows on a diagram. LLMs often miss arrows, invent them or join them to the wrong element, especially in big, dense or messy diagrams.

**How:** Use it when a diagram has more than about ten relationships, or when you start from a big, dense or messy image (such as a whiteboard photo) or from existing documents. Skip it when you can check the whole diagram at a glance: in October 2026, a small, clean hand-drawn diagram went straight to PlantUML with no mistakes.

**Check:** Count the relationships in your source, in the data and in the diagram. Do the numbers match?

**In a chat:** Ask "Give me the elements and relationships as a table first". Correct the table, then ask it to "generate the diagram from this table only".

**With an agent:** The agent can write the data to a file, so you can review it as a diff before it draws anything.

**Add to your conventions file:**

```
For more than ten relationships, give me the elements and relationships as data first (a table, YAML or CSV).
```

**Common questions:**

- *Doesn't that double the work?* Skip it for small diagrams. Use it when there are more than about ten relationships, where it saves time.
- *Isn't Structurizr DSL already a list of elements and relationships?* Yes. That is why Data First leads on to [Model First, Export Views](#model-first-export-views), where you keep that data as your source of truth.

*Credit:* Jacqui Read's experiments (Sep 2024, Oct 2026). [Mastra (Feb 2025)](https://mastra.ai/blog/whiteboard-to-excalidraw-converter); [R2ABench](https://arxiv.org/html/2604.06683v1) and [DiagramEval](https://arxiv.org/html/2510.25761v2) on why relationships are harder than elements. "Check Alignment" by Lada Kesseler, from Augmented Coding Patterns, licensed under CC BY 4.0. https://ai-coding-patterns.dev/patterns/check-alignment/

### One Level per View

For each diagram, name the C4 level (context, container or component) and the scope, and keep every element inside that scope at that level.

**Why:** If you ask for "the architecture", the LLM tends to mix levels: components drawn as containers, or everything squeezed into one view.

**How:** Ask for one view at a time, for example "a container view of the media system", not "the architecture".

**Check:** Does every element inside the scope belong at that level? People and external systems shown around the edge are fine.

**In a chat:** Send one message per view, naming the level and the scope each time.

**With an agent:** The same, with every view generated from one model file.

**Add to your conventions file:**

```
Name the C4 level and scope. One level per view. Never style one element type as another.
```

**Common questions:**

- *Real diagrams mix levels, don't they?* Supporting elements from outside the scope are fine. The elements inside the scope should all be at one level.
- *Isn't one view at a time slower?* Each view is quicker to check, and each one answers one question for one audience.

*Credit:* Jacqui Read's experiments (Sep 2025). C4 model (Simon Brown) https://c4model.com. "Semantic Zoom" by Lada Kesseler, from Augmented Coding Patterns, licensed under CC BY 4.0. https://ai-coding-patterns.dev/patterns/semantic-zoom/

### Model First, Export Views

Let the LLM edit one architecture model, such as a Structurizr workspace, and let a tool, not the LLM, produce the PlantUML and Mermaid views from it.

**Why:** When an LLM converts a diagram from one notation to another, things quietly change or disappear. An exporter gives the same output from the same architecture model every time.

**How:** Keep a Structurizr workspace as your architecture model. Use the `structurizr export` command, or the Structurizr MCP server, to produce views in other formats. Use the LLM for the jobs that need judgement, such as editing the architecture model or answering questions about it like "what depends on this service?", and ask for the view that answers the question.

In October 2026 the Structurizr MCP server exported a container view to Mermaid with every element and relationship intact. The Mermaid render was harder to read than Structurizr's own, though: some arrows merged, and the person, browser and queue shapes were lost. The export is right; how readable it is depends on the renderer, so still [render, then check](#render-then-check).

**Check:** If there is no exporter for the format you need and you have to let the LLM convert it, ask it to list everything it couldn't convert.

**In a chat:** Keep one source-of-truth file, such as your Structurizr DSL or your data file from Data First, and paste it into each new chat. Render the views in the Structurizr playground. This takes more copying and pasting than the agent route.

**With an agent:** The agent edits the workspace and runs the exporter, or the MCP server's validate and inspect tools.

**Add to your conventions file:**

```
Edit the workspace; export the views. Use tags and styles, not `containerDb`.
```

**Common questions:**

- *We only use Mermaid. Does this still work?* Yes. Structurizr can export Mermaid, or you can keep one source file and generate every view from it.
- *Isn't that lock-in?* The Structurizr DSL is open-source text, and it has several exporters.
- *Why not just let the LLM convert between notations?* Because of [Translation Drift](#translation-drift): conversions silently drop or change things.

*Credit:* Jacqui Read's experiments (Sep 2024, Sep 2025, Oct 2026). Structurizr AI documentation and MCP server https://docs.structurizr.com/ai; [libme (Aug 2026)](https://dev.to/libme/the-architecture-diagram-in-your-wiki-is-lying-how-to-generate-one-from-the-code-instead-5cc8): "a diagram you generate is a report". "Offload Deterministic" by Lada Kesseler, from Augmented Coding Patterns, licensed under CC BY 4.0. https://ai-coding-patterns.dev/patterns/offload-deterministic/

### Start from Artefacts

Generate diagrams from what already exists, such as code, infrastructure as code, OpenAPI specifications, architecture decision records (ADRs), existing diagrams or a whiteboard photo, rather than from a written description.

**Why:** From a description alone, the LLM fills the gaps with a plausible, textbook system from its training data, not your system.

**How:** Give the LLM the artefacts, and ask it to say where each element came from. If you are starting something new, the artefacts are whatever you have written down: notes, ADRs, or the elements and relationships as data.

**Check:** Can every element in the diagram be traced to one of your sources?

**In a chat:** Paste in the artefacts, but only what your organisation allows you to share with that tool.

**With an agent:** Point the agent at the repository or folder.

**Add to your conventions file:**

```
Only include elements you can trace to the inputs; list anything you assumed.
```

**Common questions:**

- *We're starting from scratch. What artefacts?* Whatever is written down: notes, ADRs, a photo of an event-storming wall, or the elements and relationships as data.
- *Tools can already reverse-engineer diagrams from code.* Use them. AI helps with the gaps they leave, such as runtime flows, views across several repositories, and documentation.
- *I can't paste our code into a public chat.* Use your organisation's approved tool, or an LLM that runs locally.

*Credit:* Jacqui Read's experiments (Sep 2024, Sep 2025). [R2ABench (Apr 2026)](https://arxiv.org/html/2604.06683v1); [libme (Aug 2026)](https://dev.to/libme/the-architecture-diagram-in-your-wiki-is-lying-how-to-generate-one-from-the-code-instead-5cc8); [Vaughan (May 2026)](https://codex.danielvaughan.com/2026/05/13/codex-cli-architecture-diagrams-mermaid-c4-plantuml-source-code-visualisation/).

### Encode Your Conventions

Write down what you keep having to tell the LLM, in a small instruction file plus a reference file for each notation, built from real failures.

**Why:** Otherwise you repeat the same corrections in every chat. But watch for **Instruction Bloat**: if every lesson goes into the file, it grows until the LLM ignores half of it.

**How:** After each diagram, write down what you wish the LLM had known. Add a rule when the same failure happens again, and delete rules that no longer change the output.

**Check:** Can you say which failure each line in the file prevents?

**In a chat:** Paste a short conventions checklist (10-15 lines) at the start of the chat, or put it in your tool's custom instructions or project settings.

**With an agent:** Use an Agent Skill or an instruction file such as `CLAUDE.md`, `AGENTS.md` or GitHub Copilot instructions, which the agent loads by itself.

**Common questions:**

- *Does it actually help?* In the course, the same tasks are compared with and without a conventions skill, labelled with the date and model used.
- *My tool doesn't support skills.* Paste the checklist into the chat, or use custom instructions. The Agent Skills format is an open standard.
- *Won't it go out of date?* Treat it like code: keep it in version control and prune it regularly.

*Credit:* Jacqui Read's experiments with the diagrams-as-code skill. Agent Skills format https://agentskills.io; [Thoughtworks Technology Radar vol 34](https://www.thoughtworks.com/radar/techniques) on instruction bloat. "Obsess Over Rules" by Gregor Riegler, from Augmented Coding Patterns, licensed under CC BY 4.0. https://ai-coding-patterns.dev/anti-patterns/obsess-over-rules/

## Antipatterns

### Pixel Diagrams

Asking for, or accepting, an image of a diagram instead of code. You can't compare it with the last version, edit it, keep it in version control or check it relationship by relationship.

**Instead:** [Ask for Code, Not Pictures](#ask-for-code-not-pictures).

### Invented Syntax

The LLM uses keywords that don't exist, or puts real ones in the wrong place. Examples: made-up layout "commands", a `containerDb` element in Structurizr, a `title` in a Mermaid diagram type that has no title, or a reserved word such as `end` used as a label.

**Instead:** [Write a Brief](#write-a-brief), then [Render, Then Check](#render-then-check).

### Trusting the Edges

Assuming the arrows are right because the boxes are. Relationships go missing, are invented, or join the wrong elements. A related problem is helpful improvisation: the LLM adds things nobody asked for.

**Instead:** [Render, Then Check](#render-then-check) and [Data First](#data-first).

### Mixing Levels of Abstraction

Different C4 levels in one view, such as components drawn as containers, or the whole system squeezed into one diagram.

**Instead:** [One Level per View](#one-level-per-view).

### Translation Drift

Asking an LLM to convert a diagram from one notation to another, and things are dropped or changed without anyone being told.

**What to do:** Use an exporter where one exists. If you do let the LLM convert, ask it to list everything it couldn't convert, then check that list. Count the elements and relationships before and after.

**Instead:** [Model First, Export Views](#model-first-export-views).

**Add to your conventions file:**

```
When converting, list everything you couldn't convert. Quote `end` and other reserved words.
```

*Credit:* Jacqui Read's experiments (Sep 2024, Sep 2025).

### Generic Architecture

From a description alone, the LLM produces a plausible, textbook system from its training data rather than your actual system.

**Instead:** [Start from Artefacts](#start-from-artefacts).

### Patch Spiral

Asking for fix after fix on a broken result, where each fix introduces the next problem, instead of starting again with a better brief.

**What to do:** After two fixes that don't work, stop. Start a new chat with a better brief, and add what you learned to your conventions file.

**Check:** Is each fix causing the next problem?

**Instead:** [Write a Brief](#write-a-brief).

*Credit:* Jacqui Read's experiments (Sep 2024). "Sunk Cost" by Ivett Ördög, from Augmented Coding Patterns, licensed under CC BY 4.0. https://ai-coding-patterns.dev/anti-patterns/sunk-cost/

### Instruction Bloat

Putting every lesson into your instruction file until it is so long that the LLM ignores half of it.

**What to do:** Add a rule only when the same failure happens again, and delete rules that no longer change the output.

**Instead:** [Encode Your Conventions](#encode-your-conventions), kept small.

*Credit:* Jacqui Read's experiments with the diagrams-as-code skill. [Thoughtworks Technology Radar vol 34](https://www.thoughtworks.com/radar/techniques) on instruction bloat. "Obsess Over Rules" by Gregor Riegler, from Augmented Coding Patterns, licensed under CC BY 4.0. https://ai-coding-patterns.dev/anti-patterns/obsess-over-rules/

## The conventions file from the course

During the course we add a line to this file at the end of each section. Paste it into your tool's instruction file (for example `CLAUDE.md`, `AGENTS.md` or GitHub Copilot instructions) or an Agent Skill, then add your own lessons as you go.

```markdown
# conventions.md
Always return diagrams as code (PlantUML, Mermaid or Structurizr DSL), never as an image.
Layout: say what the engine does (order, direction), not the result you want to see.
Render before you show me. List every change. Never remove what you weren't asked to.
When converting, list everything you couldn't convert. Quote `end` and other reserved words.
For more than ten relationships, give me the elements and relationships as data first (a table, YAML or CSV).
Name the C4 level and scope. One level per view. Never style one element type as another.
Edit the workspace; export the views. Use tags and styles, not `containerDb`.
Only include elements you can trace to the inputs; list anything you assumed.
```

## Credits

The patterns and antipatterns come from Jacqui Read's experiments with LLMs and diagrams-as-code since 2024. Related work by others is credited on each pattern.

Related coding patterns: Augmented Coding Patterns by Lada Kesseler, Ivett Ördög, and contributors, licensed under CC BY 4.0. https://ai-coding-patterns.dev/
