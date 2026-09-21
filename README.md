# WikiSlider

WikiSlider maps Wikipedia's "first eligible link" graph: starting from an article, follow the first internal article link in the readable lead text that is not inside parentheses or italics, and repeat. Many chains converge on **Philosophy**; others terminate or enter loops.

The project has two parts:

- a reproducible Python crawler that samples English Wikipedia, caches article parsing, follows first-link chains, detects loops, and aggregates edge/node traffic;
- a static React + TypeScript visualization that renders the resulting directed graph, highlights the Philosophy basin, shows non-Philosophy loops, and lets you inspect paths and funnel statistics.

## What the visualization answers

WikiSlider is designed to show both the structure of the first-link graph and the *frequency* with which sampled starting pages pass through its nodes and edges. Edge thickness and node size reflect observed traffic in the generated sample. Filters let you separate chains that reach Philosophy from loops, dead ends, and max-depth traces.

The crawler uses a documented approximation of the classic "Getting to Philosophy" rule. It ignores links in parentheses, italicized links, references, tables/infoboxes/navigation chrome, non-article namespaces, red links, and self-links.

## Local development

```bash
npm install
npm run dev
```

To regenerate the graph dataset:

```bash
python -m pip install -r scripts/requirements.txt
python scripts/crawl.py --seeds 750 --popular 250 --output public/data/graph.json
```

Run checks with:

```bash
npm test
npm run build
python -m unittest discover -s scripts/tests -v
```

## Data generation

The crawler mixes random main-namespace pages with highly viewed pages, then reuses cached first-link results so convergent chains are not repeatedly fetched. The output records sampling method, crawler version, generation time, per-node visit counts, per-edge traversal counts, outcome counts, and representative loops.

Automated GitHub Actions run tests/builds on every push. A scheduled workflow can refresh the Wikipedia graph and deploy the current static site to GitHub Pages.

## Responsible API use

WikiSlider identifies itself to Wikimedia, serializes requests, caches fetched pages, observes backoff responses, and keeps the default scheduled sample deliberately moderate. For genuinely exhaustive research over all English Wikipedia articles, Wikimedia database/XML dumps are a better data source than crawling the live API.

## License

MIT for WikiSlider source code. Wikipedia article content and metadata remain subject to Wikimedia/Wikipedia licensing and terms.
