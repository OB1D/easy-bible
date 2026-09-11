# MATRIX PURGE PLAN — linguistics sources, AI-interp papers, lexicon sources

Drafted 2026-09-11 for Mal. Status: READY FOR DISPATCH TO SEQUENCE. Nothing below
has been written to C:\CHIP or D:\Atlas. Every step is a proposal for the
review gate; none of it runs until the gate says so.

Three files travel with this plan:
- `MATRIX_PURGE_PLAN.md` (this file)
- `ai_interp_curriculum_draft.json` (the ready-to-paste curricula section and
  watchlist rows for batch B, already sent to Mal earlier tonight)
- `linguistics_watchlist_draft.json` (batch A data-node rows, batch C lexicon
  rows, and the three disables; every URL in it carries its probe result)

---

## 0. Standing constraints this plan obeys

| Rule | Source | What it means here |
|---|---|---|
| NO DELETIONS AT ALL | HOLD 2026-08-31 | Nothing is removed. Misfiled watchlist rows are flipped `enabled:false`, not deleted. |
| Do not write over old yet | Mal, 2026-09-11 | Every change is a NEW file, a NEW vessel, or an APPEND. No Drive file is rewritten in place. |
| A vessel is versioned, never mutated | MM_VERSION_0017 | MATRIX-0 stays sealed. New material gets a new MATRIX. |
| CORE locked first | DEC-0002 | English, Latin and the linguistics taxonomy come before any other language sector. Batch A serves CORE; nothing here opens another language. |
| Expert data teaches as cited data | DEC-0004 | The AI-interp papers are cited sources, not fleet knowledge. They do not enter a lattice or A-Z. |
| Review-gated, append-only, measured not assumed | doctrine | Intake plans before apply; ledger line per book; no number in this plan is a guess, unverified items are marked. |
| Every URL verified live before writing | curricula_pinned rule | The sandbox could not reach arxiv / aclanthology / openreview / gutenberg / kaikki. Each URL below carries VERIFIED or UNVERIFIED. UNVERIFIED rows ship with `enabled:false` until CHIP probes them. |
| Widening the shelf admits content | GL_EXHAUSTED_0010 / STANDING_DELEGATIONS | Adding a lexicon source to `chip_source_matrix.SOURCES` is Mal's gate, not a script's. |

---

## 1. What was found (the audit)

**None of the 17 proposed sources exist anywhere in the system.** Checked:
`sources_watchlist.json` (421 rows), `curricula_pinned.json` (6 sections),
`library_catalog.json` (193 books), `library_built.json`, `chip_source_matrix.SOURCES`,
`grounding/linguistics_truths.json`, `grounding/linguistics_core.json`,
`gold_candidates.jsonl` (783 rows, 0 linguistics), `scout_journal.jsonl`,
`intake_journal.jsonl`, `discovered_data.jsonl`.

**Linguistics is thin on disk, and part of what is there is not catalogued.**

| On disk | Path | Catalogued | Note |
|---|---|---|---|
| Sapir, *Language* (1921) | `library/txt/linguistics/sapir_language.txt` (491 KB) | NO | Real linguistics primer, sitting uncatalogued and never turned on. |
| Shea, *Grammatical Sketch of the Heve Language* | `library/txt/linguistics/gut_grammatical_sketch_of_the_heve_language_.txt` | yes | Scout pick, "thin domain (8 real concepts)". |
| Sweet, *Practical Study of Languages* (1899) | `library/txt/language pedagogy/` | yes | Pedagogy, not theory. |
| Roget (alt + classified), Wilkins 1668, Ranganathan, Porphyry | `library/txt/classification/` | yes | Classification, feeds EN-ROGET lattice. |
| Allen & Greenough, Bennett | `library/txt/latin/` | yes | CORE Latin. |
| Boole (.tex), Aristotle *Poetics*, Shakespeare | `library/txt/language and logic/` | yes | |
| 30 language pair files, babel.jsonl, lexicons.jsonl | `nodes/languages/` | n/a | Data nodes, not books. |

**Two watchlist rows are misfiled into CORE corpora by the scout's "thin domain" rule.**

| Row | corpus | What it actually is | Proposed |
|---|---|---|---|
| `gh_awesome_README.md` | linguistics | sindresorhus/awesome, a generic link list | `enabled:false` |
| `gh_awesome-go_README.md` | latin | Go language link list | `enabled:false` |
| `gut_the_hunniwell_boys_and_the_platinum_myst.txt` | latin | boys' adventure novel | `enabled:false` |

**The 232 linguistics_core entries are all labelled P22.** Page 22 is
ELECTRONICS in the current `atlas_pagemap.json` (v2). LINGUISTICS is page 12
and `atlas_stage_linguistics.py` already writes its 55 truths to page 12. The
P22 label looks like a stale v1 page number that was never migrated. Flagged
for Mal, not fixed here (fixing means rewriting an old file).

**Lexicon lineages are declared but not on disk.** In `chip_source_matrix.SOURCES`:

| id | lineage | on disk | verdict today |
|---|---|---|---|
| OEWN-2025 | princeton | yes | REDUNDANT with WN-3.1 by rule (same lineage), but it is the A-Z co-confirmer |
| WN-3.1 | princeton | yes | the A-Z co-confirmer |
| kaikki-IPA | wiktionary | yes | WORTHY candidate |
| GCIDE | webster-1913 | yes | WORTHY candidate |
| wiktionary-defs | wiktionary | NO | not fetched |
| dwyl-479k | dwyl | yes | word list only, no senses |
| CMU-dict | cmu | NO | not fetched |
| ELP / SUBTLEX / Morpholex | independent | 1 KB `.md` stubs | PRESENT, UNPARSED, loader `lines` returns empty |

`chip_lexicon.py` in the C:\CHIP Drive mirror is 392 bytes (a truncated
docstring, no code). Either the mirror is stale or the file is broken on CHIP.
Dispatch should check the real file on CHIP before anything that imports it
(`chip_bank.py`, `chip_matrix_trgb.py` SHELF lattices) is exercised.

---

## 2. The doors (how anything gets into a MATRIX)

```
sources_watchlist.json        the ONLY download gate. Row: {name,url,dest,corpus,enabled,trust,why,title,proposed}
   |  chip_fetch (ring)        downloads enabled rows to data/intake
   v
chip_intake.py <folder>       plan only. Text -> learn + frames; .dic/.aff -> lexpack;
chip_intake.py <folder> --apply   .json/.csv/.tsv/.yaml -> "node-candidate" in data/intake, NOT learned
   v
library/txt/<corpus>/         the shelf of books (chip_shelf, library_catalog.json)
   v
chip_matrix_trgb.py --new MATRIX-X --why "..."
chip_matrix_trgb.py --recipe MATRIX-X --add <book | manifest.books.json>   order IS the recipe
chip_matrix_trgb.py --on MATRIX-X --apply        one book, one ledger line (before/after/flips)
chip_matrix_trgb.py --advance                    the ring's lap; skips sealed vessels
chip_matrix_trgb.py --seal MATRIX-X --why "..."  when the recipe is complete
```

Two other doors matter for this plan and are NOT the book pipeline:

- **curricula_pinned.json** → `chip_curricula.py <section>` appends a section's
  sources to the watchlist with trust `verified-direct`. Requires every URL
  live-checked first. This is the door for batch B.
- **chip_source_matrix.SOURCES** + **chip_dict A-Z confirmation**
  (DC_CONFIRMED_0010: a word enters A-Z only when two parsed dictionaries agree
  on POS). This is the door for batch C. Lexicon sources are SHELF components
  (read by every matrix, owned by none). They never go through a recipe.

---

## 3. Batch A — linguistics sources → new vessel MATRIX-LING

### A1. Disposition of the 7 googled sources

| Source | Fetchable as a plain file? | Door | Verdict |
|---|---|---|---|
| Glottolog | yes. `glottolog/glottolog-cldf` on GitHub: `cldf/languages.csv`, `cldf/values.csv` (CC BY 4.0). raw.githubusercontent reachable. | watchlist → intake as **data node** (`.csv` → node-candidate, `data/intake`) | ADD, corpus `linguistics`, dest `intake`. Not a book. Feeds a future languages node, not MATRIX-LING. |
| WALS | yes. `cldf-datasets/wals` on GitHub: `cldf/values.csv`, `cldf/parameters.csv`, `cldf/languages.csv` (CC BY 4.0). | same as Glottolog | ADD as data node. WALS `parameters.csv` is the one file here that reads as typology *terms*; it can also seed `linguistics_truths` after review. |
| ACL Anthology | catalogue site, no single file. Individual PDFs fetchable. | per-paper watchlist rows | Do not add the site. Papers from it appear in batch B. |
| Lingbuzz | HTML index of preprints, no licence, no stable file | none | SKIP. Record in `scout_journal` as seen-and-declined. |
| *Language* (LSA journal) | paywalled (Project MUSE) | none | SKIP. Open-access LSA items live on aclanthology-style PDFs case by case. |
| MIT CogNet | paywalled, retired to MIT Press Direct | none | SKIP. |
| Lingthusiasm | podcast, transcripts are HTML pages, CC BY-NC-SA | none for now | SKIP as a source. Could be a `wordcraft` intake later if Mal wants spoken-register English; not linguistics knowledge. |

Net: **2 adds (both data nodes), 0 books, 5 skips.** The googled list does not
supply books for MATRIX-LING. The books are already on disk.

### A2. The recipe for MATRIX-LING (books, in order)

Order is the recipe. Primer first so every later sense lands on fixed ground.

1. `library/txt/linguistics/sapir_language.txt` — Sapir, *Language* (must be catalogued first, see A4)
2. `library/txt/language pedagogy/<sweet_1899>.txt` — Sweet
3. `library/txt/classification/<wilkins_1668>.txt` — Wilkins, *Essay towards a Real Character*
4. `library/txt/classification/<roget_classified>.txt` — Roget classified
5. `library/txt/latin/<allen_greenough>.txt` — CORE Latin grammar
6. `library/txt/latin/<bennett>.txt`
7. `library/txt/linguistics/gut_grammatical_sketch_of_the_heve_language_.txt` — last; one exotic sketch after the framework is fixed

Exact file names for 2–6 are in `library_catalog.json`; Dispatch fills them
from the catalog rather than from this list.

Manifest form (new file, `recipes/MATRIX-LING.books.json`):

```json
{"files": ["library/txt/linguistics/sapir_language.txt", "..."]}
```

`chip_matrix_trgb._book_files` accepts a `.books.json` manifest, but a
manifest is ONE book to the ledger (one before/after line). Seven separate
`--add` calls give seven ledger lines. **Use seven `--add` calls**, not the
manifest; the measurement between books is the point.

### A3. Commands (run on CHIP, in this order)

```
python chip_matrix_trgb.py --status                              # confirm MATRIX-0 SEALED, note open vessels
python chip_matrix_trgb.py --new MATRIX-LING --why "CORE linguistics: Sapir first, then classification, then Latin"
python chip_matrix_trgb.py --recipe MATRIX-LING --add library/txt/linguistics/sapir_language.txt
   ... one --add per book in A2 order ...
python chip_matrix_trgb.py --on MATRIX-LING --apply              # first book, by hand, read the ledger line
python chip_matrix_trgb.py --advance                             # then let the ring carry the rest
```

Gate after book 1: read `MATRIX/MATRIX-LING/mind_ledger.jsonl`. If
`documents_read` is 0 or `refused` is non-empty, stop and report; do not advance.

### A4. Pre-steps for batch A

- **Catalogue Sapir.** `sapir_language.txt` is on the shelf but not in
  `library_catalog.json`. Run the shelf's catalogue pass (`chip_shelf.py`) so
  it gets a `CHIP-BK-` id. Append, do not regenerate the catalog.
- **Watchlist appends** (new rows, `enabled:true` only after CHIP probes the URL):

```json
{"name": "glottolog_languages.csv", "url": "https://raw.githubusercontent.com/glottolog/glottolog-cldf/master/cldf/languages.csv",
 "dest": "intake", "corpus": "linguistics", "enabled": true, "trust": "github:cc-by-4.0", "bytes": 2477017,
 "why": "Glottolog languoid catalogue (glottocode, ISO 639-3, family, macroarea); data node, not a book", "title": "Glottolog CLDF languages", "proposed": "2026-09-11"}
{"name": "wals_parameters.csv", "url": "https://raw.githubusercontent.com/cldf-datasets/wals/master/cldf/parameters.csv",
 "dest": "intake", "corpus": "linguistics", "enabled": true, "trust": "github:cc-by-4.0",
 "why": "WALS typological feature names (192 chapters); data node; candidate seed for linguistics_truths", "title": "WALS CLDF parameters", "proposed": "2026-09-11"}
{"name": "wals_values.csv", "url": "https://raw.githubusercontent.com/cldf-datasets/wals/master/cldf/values.csv",
 "dest": "intake", "corpus": "linguistics", "enabled": true, "trust": "github:cc-by-4.0", "bytes": 4641862,
 "why": "WALS feature values per language; data node", "title": "WALS CLDF values", "proposed": "2026-09-11"}
```

  All three URLs VERIFIED live from the sandbox on 2026-09-11 (HTTP 206 on a
  range request, CSV headers read: Glottolog `ID,Name,Macroarea,...,Glottocode,
  ISO639P3code,Level,...`; WALS `ID,Name,Description,ColumnSpec,Chapter_ID`).
  Branch is `master` on both repos; `main` returns 404. Rows are also in
  `linguistics_watchlist_draft.json`. Whether `enabled` starts true is Mal's
  call; the URLs themselves are proven.

- **Watchlist disables** (three rows in section 1, `enabled:false`, reason
  recorded in the row's `why`). This is a change to existing rows: Mal's gate.

- **P22 → P12 question** for Mal: is `linguistics_core.json`'s `P22` a stale
  page label? If yes, the fix is a new `linguistics_core.v3.json` with `P12`,
  and `atlas_stage_linguistics.py` pointed at it. Not done here.

---

## 4. Batch B — AI-interp papers → curricula section `ai-interp`, corpus `science`

These are expert data under DEC-0004. They are read as cited sources for the
professor and for retrieval; they do not enter A-Z, a lattice, or MATRIX-LING.
MATRIX-LING is a mind about language; a mind that read Sapir then Belinkov is
a different mind, and not the one CORE asked for.

The full draft is `ai_interp_curriculum_draft.json`. Summary:

| # | File name | Status | Enable |
|---|---|---|---|
| 1 | cur_interp_belinkov_probing_classifiers.pdf (arxiv 2102.12452) | known-real, not probed | after probe |
| 2 | cur_interp_hewitt_manning_structural_probe.pdf (N19-1419) | known-real, substitutes for the OpenReview item | after probe |
| 3 | cur_interp_openreview_probing_syntax.pdf | UNVERIFIED id | false |
| 4 | cur_interp_rai_mechanistic_interp_review.pdf (arxiv 2407.02646) | known-real | after probe |
| 5 | cur_interp_brinkmann_symbolic_multistep.pdf (2024.findings-acl.242) | title real, id UNVERIFIED | false |
| 6 | cur_interp_nagar_embedding_compositions.pdf (2025.findings-acl.1104) | id UNVERIFIED | false |
| 7 | cur_interp_preprints_mi_language_abilities.pdf (preprints.org) | UNVERIFIED, not peer reviewed | false |
| 8 | cur_interp_aaai_systematic_compositionality.pdf | id UNVERIFIED | false |
| 9 | cur_interp_cunningham_sparse_autoencoders.pdf (arxiv 2309.08600) | known-real, open stand-in for paywalled ACM survey | after probe |
| 10 | gh_TransformerLens_README.md | VERIFIED live (HTTP 206 from sandbox) | true |

Skipped: mbrenndoerfer.com blog (gloss on #1), learnmechinterp.com tutorial
(replaced by #10), ACM survey 10.1145/3787104 (paywalled, replaced by #9).

Steps:

1. New file: append section `ai-interp` to `curricula_pinned.json` — as an
   append to `sections`, or as a sibling file `curricula_pinned.ai-interp.json`
   if Mal prefers no edit to the old file. Either way `verified` for this
   section stays false until step 2.
2. On CHIP: probe each URL (HEAD or first bytes). Fill `bytes`. Flip
   `enabled` only on a 200/206 with a PDF or text body.
3. `python chip_curricula.py ai-interp --dry` then without `--dry`. That
   appends the watchlist rows with trust `verified-direct`.
4. Ring fetches. `chip_intake.py data/intake` plan, review, `--apply`. PDFs
   go to `library/pdf/science/`.
5. Optional later: a separate vessel `MATRIX-INTERP` with these as its recipe,
   if Mal wants a mind that has read them. Not part of CORE. Not in this plan's
   critical path.

---

## 5. Batch C — lexicon sources ("build my corpus in the best direction")

The lexicon question is not the watchlist. It is `chip_source_matrix` and the
A-Z confirmation rule. Today A-Z holds 67,846 words: the set OEWN-2025 and
WN-3.1 agree on. Both are Princeton lineage, so today's A-Z is one lineage
agreeing with itself. **The best direction is a third, independent lineage
that confirms or contests those 67,846**, and a first source for what A-Z
does not hold (pronunciation, morphology, frequency).

Priority order, by independence and by what is already declared:

| Priority | Source | Lineage | State | What it adds | Door |
|---|---|---|---|---|---|
| 1 | GCIDE (Webster 1913 + supplements) | webster-1913 | ON DISK, declared | Independent definitions and POS. The one independent confirmer already on the shelf. | Run `chip_source_matrix.py --matrix` for the verdict; if WORTHY, propose to Mal as the third A-Z confirmer. Requires a `chip_dict` loader for GCIDE's format. |
| 2 | kaikki-IPA | wiktionary | ON DISK, declared | Pronunciation per headword | already the headword gate (`knowledge/english_headwords.txt`, 1.35M). Verdict run only. |
| 3 | wiktionary-defs (kaikki `kaikki.org-dictionary-English.jsonl`) | wiktionary | declared, NOT on disk | Independent senses + POS for ~1.3M headwords | Watchlist row (kaikki.org, UNVERIFIED from sandbox, large: >1 GB). Mal's gate: shelf widening. |
| 4 | CMU-dict (`cmudict.dict`) | cmu | declared, NOT on disk | ARPAbet pronunciations, ~134k entries | Watchlist row: `https://raw.githubusercontent.com/cmusphinx/cmudict/master/cmudict.dict` VERIFIED live, 3,618,488 bytes, body reads `'bout B AW1 T`. BSD licence. `chip_source_matrix` already expects it at `library/txt/dictionary/cmudict.dict` with loader `cmu`, so corpus hint `dictionary`. |
| 5 | Morpholex | independent | 1 KB stub | Morphological segmentation (MorphoLex-en) | `https://raw.githubusercontent.com/hugomailhot/MorphoLex-en/master/MorphoLEX_en.xlsx` VERIFIED live, 6,767,171 bytes. `.xlsx` is not a `_DATA_EXT` in `chip_intake`, so it needs a loader (openpyxl → csv) before it is more than a file on disk. |
| 6 | SUBTLEX-US, ELP | independent | 1 KB stubs | Frequency, lexical decision norms | Licensed for research; download is form-gated, not a plain URL. Mal fetches by hand to `library/txt/dictionary/`, then a `lines`/`csv` loader. |

What NOT to do: none of these go into MATRIX-LING. They are shelf components.
`chip_matrix_trgb` hashes the shelf into every ledger line (MT_REPRO_0011), so
changing the shelf after MATRIX-LING opens changes what its later ledger lines
mean. **Order: batch C shelf changes before MATRIX-LING's first `--on`, or
after it is sealed. Not during.**

Commands (on CHIP, after Mal's gate on rows 3–6):

```
python chip_source_matrix.py --matrix                 # verdict per source against the A-Z book
python chip_dict.py --stats                           # A-Z count before
   ... loader work, then re-run chip_dict's confirmation pass ...
python chip_dict.py --stats                           # A-Z count after; the delta is the report
```

---

## 6. Sequenced task list for Dispatch

Each row: who acts, what the gate is, what "done" looks like.

| # | Task | Actor | Gate | Done when |
|---|---|---|---|---|
| 1 | Check `chip_lexicon.py` on CHIP is the real file (Drive mirror shows 392 bytes) | Dispatch | none | size and `python -m py_compile` reported |
| 2 | Catalogue `sapir_language.txt` | Dispatch on CHIP | none (append) | `CHIP-BK-` id exists in `library_catalog.json` |
| 3 | Three watchlist disables (section 1) | Mal | Mal | rows carry `enabled:false` and a `why` |
| 4 | Three Glottolog/WALS watchlist rows, `enabled:false` | Dispatch appends | URL probe on CHIP → Mal flips enabled | rows appended, probe result logged |
| 5 | `curricula_pinned` section `ai-interp` (new section or sibling file) | Dispatch | URL probes on CHIP; Mal for `verified:true` | section present, `bytes` filled |
| 6 | `chip_curricula.py ai-interp --dry` → apply | Dispatch | after 5 | watchlist rows appended with `verified-direct` |
| 7 | `chip_source_matrix.py --matrix` verdict run | Dispatch | none (read-only) | verdict table reported to Mal |
| 8 | Lexicon shelf widening: CMU, wiktionary-defs, Morpholex rows | Mal | Mal (GL_EXHAUSTED_0010) | rows appended; not before 7's report |
| 9 | Open MATRIX-LING, add 7 books, `--on` book 1 by hand | Dispatch | after 2; after 8 is decided (shelf frozen) | ledger line 1 read and sane |
| 10 | Ring `--advance` carries books 2–7 | ring | after 9 | 7 ledger lines |
| 11 | `--seal MATRIX-LING` | Mal | Mal | sealed; `--status` shows it |
| 12 | P22 → P12 decision on `linguistics_core.json` | Mal | Mal | decision recorded in `decisions.jsonl` |
| 13 | Intake plan then apply for fetched batch A/B files | Dispatch | plan reviewed | files on shelf, node-candidates in `data/intake` |

Tasks 1, 2, 3, 4, 5, 7, 12 are independent of each other and can run in
parallel with whatever else the fleet is doing. Task 9 is the only one that
must wait on a shelf decision, because of MT_REPRO_0011.

---

## 7. What I could not verify from the sandbox

- Any URL outside raw.githubusercontent.com. Every arxiv / aclanthology /
  openreview / preprints / aaai / kaikki link is UNVERIFIED and ships disabled.
- VERIFIED live (HTTP 206, headers read, sizes from Content-Length):
  Glottolog languages.csv, WALS parameters.csv and values.csv, cmudict.dict,
  MorphoLEX_en.xlsx, TransformerLens README. All on branch `master`.
- Whether `chip_lexicon.py` on CHIP is intact (task 1).
- The real state of `chip_fetch.py`, `chip_lexpack.py`, `chip_filer.py`,
  `chip_spine.py`, `manifest.json`: the Drive decode of these was still
  running when this plan was written. The door descriptions in section 2 come
  from `chip_intake.py`, `chip_curricula.py`, `chip_shelf.py`,
  `chip_matrix_trgb.py`, `chip_source_matrix.py`, `chip_dict.py` and
  `grow_lexicon.py`, all read in full.
