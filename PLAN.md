# Plan — IALA Metanorma flavor (`metanorma-iala`) + samples repo (`mn-samples-iala`)

Status: updated 2026-09-14. Grounded in local `reference-docs/` sources, the two
template repos (`mn-samples-iho`, `metanorma-iho`), and the supporting data
projects (`relaton-data-iala`, `pubid`, `edoxen`, `iala-vocab`, `uniword`).

## 0. Goals

1. **`metanorma-iala`** — a new Metanorma flavor gem for IALA publications.
   Greenfield: no gem on rubygems, no repo under ribose/metanorma orgs.
2. **`mn-samples-iala`** (this repo) — AsciiDoc sources of IALA publications,
   organized like `mn-samples-iho`, published as a mini-site via GitHub Pages.
3. **Import pipeline** — `bin/import-docx` (done, v1) converts IALA Word
   documents into `sources/<id>/` Metanorma layout using uniword.
4. **Resolutions as a doctype** — resolution documents carry their source
   resolution(s) in **Edoxen format** (decisions model, cf. bipm-resolutions).

Constraint: `reference-docs/` holds copyrighted IALA PDFs/DOCX used as local
reference only — never committed to git, never deleted.

## 1. IALA domain model (from reference docs)

### 1.1 Organization

Four Technical Committees (source: `reference-docs/iala-structure.adoc`):

| Committee | Working groups |
|---|---|
| ARM — AtoN Requirements and Management | WG1 Navigational Requirements; WG2 Information Services and Portrayal; WG3 Risk management |
| ENG — Engineering and Sustainability | WG1 Visual and physical AtoN; WG2 Radionavigation services; WG3 Heritage and culture; WG4 Sustainability (new) |
| DTEC — Digital Technologies | WG1 Digital information system; WG2 Emerging digital technology; WG3 Digital communication system; WG4 IMT (new) |
| VTS — Vessel Traffic Services | WG1 Operations; WG2 Technology; WG3 VTS Training |

Above them: General Assembly → Council. Technical documents are approved by
Council (R, G, C) or General Assembly (S — see S1040 recitals).

Org name transition matters for rendering:
- ≤ 2023: "International Association of Marine Aids to Navigation and Lighthouse
  Authorities / Association Internationale de Signalisation Maritime",
  www.iala-aism.org (S1040, June 2023)
- 2025+: "International Organization for Marine Aids to Navigation", www.iala.int
  (R1026 Dec 2025, G1199 June 2026)

### 1.2 Document types

Confirmed type space from `Pubid::Iala` (`~/src/pubid/pubid`, already complete):
identifier grammar `[IALA ]{S|R|G|M|C|X|P}<4-digit>[-subpart][ Ed <edition>][(<Lang>)]`
plus the GA resolution series; identifier classes exist for standard (S),
recommendation (R), guideline (G), manual (M), model-course (C), advice (X),
resolution (P, reserved), general-assembly, letter, annex, report.

Flavor doctypes (first release): `standard`, `recommendation`, `guideline`,
`model-course`, `resolution`. Later: `manual`, `advice`, `report`, `letter`.

| Type | Prefix | Definition / use | Approval | Template |
|---|---|---|---|---|
| Standard | S | part of a framework whose implementation harmonizes AtoN world-wide; non-mandatory | General Assembly | recitals page + 8 fixed sections |
| Recommendation | R | specifies what practices *shall* be carried out; may be referenced by a standard | Council | "THE COUNCIL" recitals + optional annex |
| Guideline | G | describes how to implement practices normally specified in a recommendation | Council | TOC + free main body + back matter |
| Model course | C | training courses; C01xx = VTS, C1xxx = Level 1, C2xxx = Level 2, often multi-part (C0103-1) | Council | course/module layout (source: C0103-1, C2007-1) |
| Resolution | P/GA01-Res | General Assembly / Council resolutions (GA01-Res.01/.02/.12, A12-01, A13-01) | GA / Council | resolution statements; source decisions in Edoxen format (§1.6) |

### 1.3 Identity and numbering

- Reference = prefix + 4 digits: `S1040`, `G1199`, `R1026`, `C0103-1`; legacy ids
  in parentheses: `R0141 (E-141)`.
- Filename syntax: `[p][nnnn] [title] [(legacy)] [Edx.x] [date]`.
- Edition: major `Ed1.0/2.0` = Council/Assembly-approved revision; minor `Ed1.1`
  = editorial. Edition date = month + year in full.
- MRN on cover and footer: `urn:mrn:iala:pub:s1040:ed2.0`
  (variants in the wild: `r1026:ed.1.0` — render canonical form; G1143).
- Document revision table (front matter): Date / Details / Approval, where
  Approval = "Council 03", "Council 04" (Council session number).
- Identifiers are parsed/rendered by the existing `Pubid::Iala` — the flavor
  must depend on it rather than hand-roll docid logic.

### 1.4 Standard template skeleton (from S1040 Ed 2.0)

Cover → "THE GENERAL ASSEMBLY" recital page (BEARING IN MIND / RECOGNIZING /
RECALLING / RECALLING ALSO / HAVING CONSIDERED / APPROVES / INVITES …) →
Contents → exactly 8 sections:

1. Introduction
2. Purpose
3. Application
4. Scope
5. Referenced documents
6. Supplementary elements
7. Adoption of and amendments to standards
8. Document history

Recommendation template (R1026): cover (may carry an "(INFORMATIVE)"
qualifier) → "THE COUNCIL" recital page (first RECALLING paragraph is
mandatory and non-removable) → optional annex laid out like a guideline.

### 1.5 Style essentials (G1115)

- UK English (OED + G1115 preferred-spelling appendix; -ize verbs, -yse;
  gender-neutral; no first-person pronouns; 24h times; "17 June 2020" dates).
- Font: Calibri throughout; headings and table text in blue RGB(0, 85, 140)
  (#00558C); covers color-coded per document type (Brand Guidelines palette —
  exact per-type colors need the Brand Guidelines PDF, see §5).
- 5 heading levels; TOC shows levels 1–3 only.
- Annexes = self-contained (A, A.1, A.1.1 …) vs appendices = attached to main
  body (Appendix 1, 1.1, 1.1.1 …); annex figures/tables numbered separately.
- Back matter order: Definitions → Abbreviations → References → Further reading
  → Appendices/Annexes. (G1199 in fact carries only Abbreviations + References.)
- Definitions section is *fixed boilerplate* pointing to the IALA Dictionary.
- References: sequential `[n]` markers, entries like
  `IALA. (2013) G1081 Provision of Virtual Aids to Navigation.`
- Equations centred, right-justified `(n)` number.

### 1.6 Supporting data infrastructure (all already existing)

| Project | Path | Role |
|---|---|---|
| `pubid` (Pubid::Iala) | ~/src/pubid/pubid | parse/render IALA identifiers incl. URN (urn_parser/urn_generator) |
| `relaton-data-iala` | ~/src/relaton/relaton-data-iala | 863 Relaton YAML records (schema v1.5.6) incl. resolutions (a12-01, a13-01/02), `ext.doctype`, `flavor: iala`; docidentifier `IALA <id>` |
| `iala-vocab` | ~/src/glossarist/iala-vocab | IALA Dictionary (glossarist dataset) — the Definitions boilerplate target |
| `edoxen` (+ edoxen-model, edoxen-js, browser) | ~/src/edoxen | generic decisions/meetings information model; Ruby gem `edoxen`; JSON Schemas `decision-collection.yaml`, `meeting.yaml` |
| `bipm-resolutions` | ~/src/edoxen/bipm-resolutions | canonical Edoxen dataset example to mirror |
| `uniword` | ~/src/mn/uniword | full-coverage OOXML (DOCX) reader used by `bin/import-docx` |

Edoxen resolution format (from edoxen-model + bipm-resolutions):
- One `DecisionCollection` YAML per meeting: `resolutions/{body}-meeting-{N}.yaml`
  (metadata + `decisions[]`), plus `edoxen-data/meetings/{body}-{N}.yaml`
  (Meeting records) and `edoxen-data/registers/bodies.yaml` (body register).
- Decision fields: `identifier` (StructuredIdentifier prefix/number), `kind`
  (resolution/recommendation/…), `status`, `urn`, `dates[]`, `meeting`,
  `relations[]`, `urls[]`, `title`/`subject`/`message`/`considering`
  (LocalizedString), `considerations`, `approvals`, `actions`.
- Proposed IALA layout: `resolutions/ga-meeting-1.yaml` with
  `identifier: [{prefix: GA01, number: '01'}]`; legacy `A13-01` maps to
  `{prefix: A13, number: '01'}`; URN style `urn:iala:v1:ga:1:2024:resolution:01`.

## 2. `mn-samples-iala` organization (mirror of `mn-samples-iho`)

```
mn-samples-iala/
├── Gemfile                  # uniword now; + metanorma-cli/metanorma-iala later
├── bin/import-docx          # uniword-based DOCX → sources/<id>/ importer (v1 done)
├── metanorma.yml            # source file list + collection (organization, name)
├── README.adoc
├── .gitignore               # reference-docs/ must never be committed
├── sources/<id>/            # one dir per publication: s1040, g1199, r1026, c0103-1
│   ├── document.adoc        # header attrs + include::sections/NNN-*.adoc[]
│   ├── sections/*.adoc      # one file per level-1 section / annex
│   └── images/
├── resolutions/             # Edoxen-format source decisions (proposed, §3.10)
├── site/                    # committed build outputs (html/doc/pdf/xml/rxl + index)
├── common-images/
├── relaton/cache/           # citation cache (committed, as in mn-samples-iho)
└── reference-docs/          # LOCAL ONLY (gitignored)
```

**Import pipeline (implemented)**: `bin/import-docx "reference-docs/G1199 ….docx"`
parses the IALA filename (prefix/number/edition/title), walks the uniword
document model in true document order (`body.element_order` markers driving
queues over `paragraphs`/`tables`, since `Body#elements` is type-grouped),
classifies IALA template styles (French-Word style ids: Titre1–4, Corpsdetexte,
Bullet1/List1, Figurecaption, TM1–3 TOC, Document* cover styles), skips
cover/TOC/revision-table front matter, and emits: header attributes
(doctype/series/docnumber/edition/published-date/urn + approval note),
level-1 sections as files, headings h2–h5, bullets/numbered lists, figures
with captions attached above, abbreviations as definition lists, References as
`[bibliography]` with sequential keys + TODO to relaton-ify, images extracted
to `images/`. Status: G1199 fully imported (6 sections, 8 images, 264 lines)
as the golden sample. Known v1 gaps (flagged as TODOs in output): table-heavy
documents untested (G1199 body has no tables), hyperlinks, footnotes, OMML
equations, charts, model-course layout.

Header pattern produced (final attribute names settle when the flavor lands):

```asciidoc
= VTS Digital Communications
:doctype: guideline
:series: G
:docnumber: 1199
:edition: 1.0
:published-date: 2026-06
:mn-document-class: iala
:urn: urn:mrn:iala:pub:g1199:ed1.0
```

CI (copy the cimas-generated set from `mn-samples-iho`): `generate.yml` using
`metanorma/ci/.github/workflows/sample-gen.yml@main`, plus `test.yml`,
`automerge.yml`, `docker.yml`. Enable GitHub Pages.

Sample priority from `reference-docs/`:
1. **G1199** — done via import (docx available; newest guideline template)
2. **S1040** (Standard template + General Assembly recitals; PDF-only → style
   transcription, or request docx)
3. **R1026** (Recommendation + informative qualifier)
4. **S1050 / S1060**, **G1195 / G1201**
5. **C0103-1 / C2007-1** (model courses — template not yet analyzed)
6. Resolutions GA01-Res.01/.02/.12, A12-01, A13-01 (Edoxen transcription, §3.10)

## 3. `metanorma-iala` gem — architecture copied from `metanorma-iho` v1.3.x

metanorma-iho is built entirely on **metanorma-generic** (single runtime dep
`metanorma-generic ~> 3.4`); all flavor config lives in a root `metanorma.yml`.
Copy that pattern. (`metanorma-model-iho` is only a PlantUML *design* repo, not
a gem — the Lutaml::Model classes live inside the flavor gem under
`lib/metanorma/iho/document/`; same for IALA.)

File-by-file checklist:

```
metanorma-iala.gemspec            # deps: metanorma-generic, pubid-iala
metanorma.yml                     # all config (below)
lib/metanorma-iala.rb             # Configuration < Generic::Configuration; Registry.register(Processor)
lib/metanorma/iala/
  processor.rb                    # output html/doc/pdf(+presentation); version string
  converter.rb                    # register_for "iala"
  document.rb + document/{root.rb, metadata/*.rb}   # Lutaml::Model classes
  {front,cleanup,validate,log,version}.rb, boilerplate.adoc
  iala.rng (+ copied standoc/isodoc/biblio/reqt rngs)
lib/isodoc/iala/
  {init,metadata,xref,base_convert,html_convert,word_convert,
   pdf_convert,presentation_xml_convert}.rb
  i18n-en.yaml
  iala.{standard,recommendation,guideline,model-course,resolution}.xsl
  html/ (scss, cover/word templates, logo/)
lib/relaton/render/{general,fields,parse}.rb + config.yml   # "IALA. (2013) G1081 …" citations
spec/ (namespace, roundtrip, metanorma/*, isodoc/* + fixtures)
.github/workflows/{rake,release,automerge}.yml
```

`metanorma.yml` keys to decide: `metanorma_name: iala`, organization names
short/long (both eras? see §5), `document_namespace`/`xml_root_tag`,
`doctypes: [standard, recommendation, guideline, model-course, resolution]`,
`committee_types: [arm, eng, dtec, vts]`, `docid_template` (delegate to
Pubid::Iala), stages ↔ edition semantics, `fonts_manifest` (Calibri for
DOC/PDF), cover/word templates, `validate_rng_file`, `i18nyaml`.

Flavor-specific work items (the real engineering):

1. **Docid**: delegate to `Pubid::Iala` (prefix + number + legacy + part +
   edition + language); render `S1040`, `R0141 (E-141)`, `C0103-1`.
2. **Cover + footer fields**: type banner (color-coded), reference, edition,
   date, MRN — footer replicates cover fields on every page (Word StyleRef
   mechanism → XSLT running headers/footers).
3. **Recital pages**: Standard → "THE GENERAL ASSEMBLY" boilerplate;
   Recommendation → "THE COUNCIL" recitals with mandatory first RECALLING
   paragraph. Model as boilerplate sections with editable clause content.
4. **Document revision table**: Date/Details/Approval with Council session
   numbers — from document-history data.
5. **Fixed Definitions boilerplate** (IALA Dictionary pointer; link target to
   be confirmed with iala-vocab / iala.int).
6. **Brand**: IALA logo variants + per-type cover colors (needs Brand
   Guidelines assets); Calibri; #00558C headings.
7. **Back matter ordering** and Reference/Further-reading list styles.
8. **Citations**: consume `relaton-data-iala` as the Relaton dataset (no new
   relaton-iala fetcher gem needed initially — the dataset repo is the source;
   add `Relaton::Render::Iala` for G1115 reference syntax).
9. **Validation RNG**: iala.rng extending standoc with committee/WG,
   approval-body, urn/mrn, edition attributes.
10. **Resolutions doctype**: Edoxen-format decision data is the source of
    truth; resolution documents in `sources/` keep/derive their content from
    Edoxen records (e.g. `resolutions/ga-meeting-1.yaml` proposed at repo
    root, mirroring bipm-resolutions layout + `registers/bodies.yaml`).
    Flavor renders the resolution statements (considering/approvals/actions)
    from the decision fields; URN per `urn:iala:v1:…` style. Open: exact
    placement of the dataset (this repo vs separate `edoxen-data-iala` repo)
    and single-source direction (adoc from YAML, or YAML alongside adoc).

## 4. Milestones

- **M0 (this repo)**: `.gitignore`, PLAN.md + CLAUDE.md, `bin/import-docx` v1,
  G1199 imported. **Done (uncommitted).** Remaining: README.adoc,
  metanorma.yml skeleton, CI workflows.
- **M0.5 — `iala` taste (done)**: metanorma-taste `data/iala/` (base flavor
  `generic`) makes `:mn-document-class: iala` / `-t iala` work today:
  config.yaml (publisher IALA, doctypes, #00558C, Carlito/Calibri, IALA-first
  bibliography sort), metanorma.yml (namespace, docid G1199 form),
  i18n.yaml (doctype labels + IALA Dictionary Definitions boilerplate),
  copyright.adoc, IALA logo SVGs. G1199 compiles to xml/html/doc/rxl/
  presentation with plain `:mn-output-extensions: xml,html,doc,rxl` —
  presentation is an implicit dependency handled by the compile layer.
  Upstream fixes produced along the way (uncommitted, PR candidates):
  metanorma compile_options (extension_keys mirror + non-mutating
  output_formats filter — path-pinned here until pushed) and
  metanorma-generic (formats-table hardening + dup on output_formats).
  Known upstream issue: on the metanorma-core `feat/flavor-table` pin
  (metanorma-core#18), taste registration runs before `Core::Flavors` is
  defined; taste-side hardening in `TasteRegister#aliases` compensates.
  The taste is replaced by the full metanorma-iala gem at M1+.
- **M0.6 — relaton-v3 / pubid-v2 stack (done, 2026-09-15)**: per the
  migration directive, the repo runs the full v3 set via temporary git pins
  (pubid 2.0.0.pre.alpha.10 via pubid#380, relaton 3.0.0.pre.alpha.1
  monogem, metanorma-document 0.5.1, standoc/core/metanorma@main,
  generic@fix/output-formats-merge incl. #128, taste@feat/flavor-registry-
  integration; metanorma-cli intentionally absent until the flavor gems
  migrate). G1199 compiles with 0 errors to all five outputs including a
  cold-cache relaton v3 fetch. Release checklist: metanorma#604. The moxml
  monkeypatch breakage (metanorma#603) is fixed by document 0.5.1 in this
  set — no patches anywhere.
- **M1**: metanorma-iala skeleton on metanorma-generic + pubid-iala; plain
  `:doctype: guideline` document compiles to XML + unstyled HTML.
- **M2**: G1199 compiles through the flavor; structure (cover, revision
  table, back matter) renders correctly in HTML.
- **M3**: Standard + Recommendation templates (recitals, 8-section skeleton,
  informative qualifier); S1040 + R1026 samples (transcribed from PDF).
- **M4**: Word + PDF via iala XSLTs (brand colors, footer MRN); visual diff
  against reference PDFs.
- **M5**: citations via relaton-data-iala + Relaton::Render::Iala;
  References/Further reading in G1115 syntax.
- **M6**: Resolutions doctype: Edoxen dataset for GA01-Res/A12/A13; rendering
  of resolution documents from decision data.
- **M7**: spec suite, validation RNG, i18n, gem release, CI green, Pages live.

## 5. Open questions

1. Which organization name renders by default (Association vs Organization)?
   Parameterize by published-date, or fix on the current IGO name?
2. Edoxen data placement for resolutions: `resolutions/` in this repo, or a
   separate dataset repo (like bipm-resolutions)? And is the adoc generated
   from the Edoxen YAML (single source) or authored with YAML kept alongside?
3. Model Course template specifics — need TOC analysis of C0103-1/C2007-1
   before designing that doctype.
4. Brand assets: logo files + exact per-type cover colors (Brand Guidelines
   PDF not in reference-docs; only the color-palette extract in G1115 app. 3).
5. MRN canonical form: sources are inconsistent (`s1040:ed2.0` vs
   `r1026:ed.1.0`); propose always `urn:mrn:iala:pub:<ref>:ed<x.x>`, lowercase.
   (Pubid::Iala has urn_generator — check its canonical form and align.)
6. Dictionary URL for the Definitions boilerplate (iala-aism.org/wiki vs
   iala.int; iala-vocab dataset is the glossarist instantiation).
7. Docx availability: only G1199 has a docx; are docx sources obtainable for
   S/R/C documents, or are those transcribed from PDF?
8. Hosting/ownership: which GitHub org gets metanorma-iala, and who owns the
   first release version number (user decides).
