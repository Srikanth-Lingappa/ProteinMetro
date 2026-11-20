ProteinMetro

A subway-map route advisor for protein design pipelines.

Live site: https://srikanth-lingappa.github.io/protein-metro/

If you've ever tried to design a protein, you know the hard part isn't finding tools, it's figuring out which ones to chain together, in what order, and what numbers to trust at each handoff. RFdiffusion or BindCraft? ProteinMPNN or LigandMPNN? AlphaFold2, ColabFold, or Boltz-2 to check the result? It depends on what you're designing, what you already know about your target, and what hardware you actually have.

ProteinMetro asks you a few questions about all that and draws you a route. Each stop is a real tool, each line colour is a design use-case (binder design, enzymes, antibodies, and so on), and the connections between stops tell you what to check before moving on.

This project extends protein-design-subway-map, which is where the underlying map of tools and pipelines comes from.

How to use it

Open the site and answer the questions. They change depending on what you pick first, so you'll only see what's relevant to your case. At the end you get a pipeline you can click through:

Click a station to see the tool's inputs, outputs and the score thresholds that usually separate a good result from a bad one.
Click a connector to see the gate between two tools, meaning what to check before passing the output along.
Check the alternate routes below your main one. They show how the pipeline would change if you wanted something more accessible (webservers and Colab) or the most accurate option regardless of effort. Changed stations are highlighted.
Export a PNG of the route, with or without a summary of your answers, if you want it for slides or a lab meeting.

It also covers a few things that aren't design pipelines at all: searching for similar structures or sequences, screening existing designs for developability problems, and plain structure prediction.

How it works

There's no backend and nothing is sent anywhere. It's one HTML file with everything inside it.

Your answers get turned into a set of requirements. Each step in a workflow asks for a capability (say, "ligand-aware sequence design") rather than a specific tool, and any tool tagged with that capability becomes a candidate. A list of expert rules then scores the candidates, for example "RFdiffusion's symmetry mode is the right call for symmetric assemblies" or "with no GPU, prefer ESMFold". The winner at each step becomes a station, and the rules that fired are shown under "Why this route" so you can see the reasoning rather than just take its word for it.

If you pick a tool yourself, that choice is always respected.

Adding or changing a tool

Everything lives in index.html. The relevant bits, roughly in the order you'd touch them:

NODES has one entry per tool: name, stage, inputs, outputs and thresholds.
TOOL_CAPABILITIES says what the tool can do. This is how workflows find it.
EDGES describes the handoffs between tools and the metrics to check.
EXPERT_RULES is where the scientific preferences go ("prefer X when Y").
TOOL_ACCESS marks tools that have a public webserver or Colab notebook.

A tool with no capability tags still shows up as a fallback for its stage, so adding data alone won't break anything.

To try changes locally, just open index.html in a browser. There's no build step.

A note on the recommendations

The routes come from rules I encoded based on how these tools are generally used. They're a starting point, not a verdict. Tools change quickly, thresholds depend on your target, and no computational filter replaces experimental validation. Please read each tool's own documentation and papers before you commit compute or lab time to a route.

If you spot something wrong, outdated or missing, an issue or pull request is very welcome.

Credits

All the tools in the map belong to their respective authors and teams. ProteinMetro only points to them, and none of it would exist without their work. This project builds on the protein-design-subway-map project.

Use of AI

The code for this tool was written by Claude. The design, the tool coverage and the expert rules were directed by the author.

License

MIT. See LICENSE.
