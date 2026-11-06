# Research Integrity Compass (RIC)
### የምርምር ታማኝነት ኮምፓስ

**Publication quality, integrity and reward screening for universities.**

**Live app:** https://muler-21.github.io/Research-Integrity-Compass-/ · **Author:** Mulusew Kassa Bitew ([ORCID 0000-0001-6223-0037](https://orcid.org/0000-0001-6223-0037)) · **Licence:** MIT · **Cite:** see [How to cite](#how-to-cite)

RIC is a single-file web app for university research-management teams. It does five things:

- collects all of an institution's publications
- rates them against international research-assessment standards: field-normalised citation impact, top-10% papers, the Nordic (JUFO / Norwegian Register) channel levels, and integrity checks
- applies the institution's own publication-reward rules, with Debre Markos University as the built-in example
- ranks publishers and journals under both views
- flags where they disagree

It uses the free [OpenAlex](https://openalex.org) API and runs entirely in the browser, with no server or install.

## Run it

- **Locally:** open `index.html` in Chrome, Firefox or Edge.
- **Online:** use the live app link above (hosted on GitHub Pages). To host your own copy, fork this repository and turn on Settings → Pages → Deploy from branch.

**No account or API key is needed.** OpenAlex gives every visitor a small free daily allowance and up to 10,000 results per search, which covers a typical university dashboard. Heavy users can paste a free OpenAlex API key into **Setup** for ten times the allowance and no 10,000 limit. The key is stored only in that user's browser.

## Management dashboard (start here)

For a university's research-management team. Pick the institution and a range of years. The dashboard collects **all** of its publications, using OpenAlex's institution links plus a text search of raw affiliations. It then reports:

- **Headline indicators** (in the style of the CWTS Leiden Ranking):
  - number of publications
  - share in the world's top 10% / 1% most-cited papers for their field and year
  - mean and median field-weighted citation impact (FWCI)
  - international co-authorship
  - institution-led papers
  - open access
  - share in indexed or accredited venues
  - integrity-risk papers
  - papers that would qualify under the institution's reward rules
- **Quality bands** for every paper, checked in this order: ⚠ Integrity risk (retracted, watchlist, warning signals) → Excellent (top 10% cited) → Strong (high-impact or established journal, or FWCI ≥ 1) → Standard (indexed or accredited venue) → Unverified.
- **Breakdowns:** a chart of output by year and band; a table by research field; a **By department** table; a researchers table; an integrity-risk register.
- **Departments:** OpenAlex doesn't record departments, so RIC reads each person's department and college from the addresses they print on their papers (e.g. *"Department of Animal Science, College of Agriculture and Natural Resources, Debre Markos University"*) and uses the one they give most often. It handles "Dept." abbreviations, all-capitals addresses and the university name written in front of a unit. People who never name a department show as "not stated". Departments also appear in the Staff list, in CSV exports and on the reward verification form.
- **Papers table:** every paper with its band, its metrics and its status under the institution's rules (e.g. DMU), downloadable as CSV.
- **Methodology note:** explains the indicators and follows DORA / Leiden Manifesto practice. Use the figures to start conversations and to review integrity, not to rank individuals.

## International standard vs local rules: publisher and journal ranking

Inside the dashboard, every publisher (or journal) the institution has published with gets **two separate ratings**:

- **International score (0–100, grades A–E).** Built from four parts:
  - **Channel level**, on the Nordic 0/1/2 scale (2 = leading, 1 = approved, 0 = not approved). It comes from the official **JUFO** (Finland) or **Norwegian Register** lists when you upload their CSV export in Settings, matched by ISSN. Otherwise an open estimate is used, with level **X** for watchlist and warning journals.
  - The publisher's or journal's **global 2-year citedness** (OpenAlex).
  - The **field-weighted citation impact** of the institution's papers.
  - **Integrity**: retractions, watchlist and warning signals.

  The weights are editable. A publisher on the watchlist is always graded E.
- **Local rules (e.g. DMU).** The share of papers that qualify for reward under the institution's rule set, and the estimated reward value.

Each rating produces its own rank. An **Assessment** column flags where they disagree:

- *rewarded locally but below international standard* (grade D/E): a candidate for tightening the policy
- *high international standard but mostly not rewarded* (grade A/B): a candidate for widening it

You can switch between ranking publishers and ranking journals, group imprints under their parent publisher, and download the ranking as CSV.

## Publication incentive screening

This tab is for a university research office that pays staff for publications. It:

1. Fetches every paper carrying the institution's affiliation in the claim window. It also searches raw affiliation text, which catches papers where OpenAlex didn't link the institution.
2. Checks each paper:
   - Is it inside the claim window?
   - Does at least one author carry the institution's affiliation?
   - Is the journal indexed? Scopus and PubMed are checked automatically. Web of Science needs a pasted list, otherwise the paper goes to manual review.
   - Is the journal on the institution's accredited list?
   - Journal integrity: is it on the watchlist, or does it show warning signals?
   - Is the paper retracted?
   - Has its DOI already been paid (duplicate claim)?
   - Is the document type one that earns a reward?
3. Gives each paper a status: **Eligible**, **Needs review** (approve or reject it in the table; decisions are saved in the browser and printed on the form) or **Not eligible**, with reasons.
4. Computes payments from the rate table and the author-share table, applying the per-author caps for each category.
5. Prints a pre-filled **verification form** for each paper with these sections: claim, publication details, eligibility checks with evidence, authors and payment, author and journal metrics, declarations, directorate sign-off.

The rules (window, rates, caps, author shares, lists, how strict each check is) are editable. You can export them as JSON and import them at another institution. **Debre Markos University 2026/27** is included as a preset, built from the Directorate's call letter (Ref. 048/1.1/19) and its Article 8 form.

**Limits:**
- Web of Science and the Journal Impact Factor are proprietary. The form leaves the JIF blank and shows OpenAlex's open 2-year citedness alongside it.
- The use of an institutional email has to be checked on the PDF.
- Very recent papers can take a few weeks to appear in OpenAlex.

## Other views

| Tab | Input | Output |
|---|---|---|
| **Organization staff** | An organization; current or current + former staff; minimum profile size | Every researcher at the organization, with a count of their papers (under this affiliation, in the year range) in each tier. Filter to people with Watchlist/Warning papers or to one tier; **Profile** opens a person's journal list |
| **Scan** | Institution and/or country (ISO-2) and/or keyword, plus a year range | Share of output by journal tier, the journal table, and **List researchers** for any tier (with an option to keep only researchers affiliated with the institution) |
| **Researcher → journals** | One author (name or ORCID) | That person's journals, grouped by tier |
| **Journals → researchers** | One or more journals (e.g. a top journal and a suspect one) | Authors of each journal, with overlap between journals |

Every table sorts when you click a column header and downloads as CSV.

## Tiers (thresholds are editable in Settings)

| Tier | Rule |
|---|---|
| On watchlist | ISSN, exact title or exact publisher matches the loaded list (this overrides every other rule) |
| High impact | OpenAlex 2-yr mean citedness ≥ 5 |
| Established | In CWTS-core, Scopus or DOAJ, and citedness ≥ 2 |
| Indexed, lower impact | In CWTS-core, Scopus or DOAJ |
| Warning signals | In no index **and** at least one of: fully OA but not in DOAJ, mass output with very low citations, APC with very low citations |
| Not in any index | None of the above |
| Repository / conf. / book | Source type is not `journal` |

## Watchlist

No journal is flagged until you load a watchlist. You can:

- load the community *Stop Predatory Journals* list (last maintained around 2021) from GitHub,
- upload a CSV or TXT file, or
- paste entries one per line: a title, an ISSN, or `publisher: Name`.

The hijacked-journals list is left out on purpose, because it names the legitimate journals that were cloned.

## Caveats

- These tiers are signals, not verdicts. Exact-title matching can hit the wrong journal, and authors in flagged journals may have been deceived.
- OpenAlex author profiles sometimes merge or split people with similar names.
- Check each case before acting on it, and don't publish named lists of researchers based on this tool alone.

## How to cite

If you use RIC in your work, please cite it. GitHub's **"Cite this repository"** button, which reads [`CITATION.cff`](CITATION.cff), gives these formats ready to copy.

### APA

> Bitew, M. K. (2026). *Research Integrity Compass (RIC) — የምርምር ታማኝነት ኮምፓስ* (Version 1.0.0) [Computer software]. GitHub. https://github.com/Muler-21/Research-Integrity-Compass-

### BibTeX

```bibtex
@software{bitew_ric_2026,
  author  = {Bitew, Mulusew Kassa},
  title   = {Research Integrity Compass (RIC) — የምርምር ታማኝነት ኮምፓስ},
  version = {1.0.0},
  year    = {2026},
  url     = {https://github.com/Muler-21/Research-Integrity-Compass-}
}
```

<!-- After archiving a release on Zenodo, add: DOI badge and "https://doi.org/10.5281/zenodo.XXXXXXX" to the citations above. -->

## References and data sources

RIC builds on the following open data, standards and policies. Please acknowledge them as well.

- **OpenAlex** (publication, author, journal and publisher data, CC0): Priem, J., Piwowar, H., & Orr, R. (2022). *OpenAlex: A fully-open index of scholarly works, authors, venues, institutions, and concepts.* arXiv:2205.01833. <https://openalex.org>
- **Leiden Manifesto:** Hicks, D., Wouters, P., Waltman, L., de Rijcke, S., & Rafols, I. (2015). Bibliometrics: The Leiden Manifesto for research metrics. *Nature*, 520, 429–431. <https://doi.org/10.1038/520429a>
- **DORA:** San Francisco Declaration on Research Assessment (2012). <https://sfdora.org>
- **CWTS Leiden Ranking:** the model for the indicator definitions (top-10% share, collaboration, open access). <https://www.leidenranking.com>
- **Publication Forum (JUFO), Finland:** channel levels, used when you upload their export. <https://julkaisufoorumi.fi>
- **Norwegian Register for Scientific Journals, Series and Publishers (HK-dir):** channel levels, used when you upload their export. <https://kanalregister.hkdir.no>
- **Stop Predatory Journals** (MIT): the optional watchlist. <https://github.com/stop-predatory-journals/stop-predatory-journals.github.io>
- **Debre Markos University**, Research, Publication, Ethics & Dissemination Directorate: the publication reward guideline (approved Hamle 16, 2014 E.C.) and the 2026/27 call letter (Ref. 048/1.1/19), used for the built-in rule-set example.

## Author

**Mulusew Kassa Bitew**, Postdoctoral Research Fellow, University of Molise, Italy.
ORCID: [0000-0001-6223-0037](https://orcid.org/0000-0001-6223-0037) · GitHub: [@Muler-21](https://github.com/Muler-21)

Feedback, bug reports and new institutional rule sets are welcome through [GitHub Issues](https://github.com/Muler-21/Research-Integrity-Compass-/issues).

## AI-assistance statement

RIC was developed with AI assistance (Claude, Anthropic). The concept, requirements, institutional rules and validation are by the author. The code was generated and tested iteratively under the author's direction.

## Licence

Released under the [MIT License](LICENSE) © 2026 Mulusew Kassa Bitew. Data retrieved through the app remains under its providers' terms: OpenAlex data is CC0; JUFO and Norwegian Register lists are subject to their own terms of use.
