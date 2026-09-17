# PENROSE GEOMETRY AS A FORMAL COMPUTATIONAL SYSTEM — assessment note

Drafted 2026-09-17 for Mal, from the one-line prompt "Penrose geometry can
become a formal computational system." Revised the same day after an online
check of GitHub and the literature (section 8). Status: ASSESSMENT, nothing
dispatched. Nothing here touches C:\CHIP or D:\Atlas. Section 6 is a proposal
for the review gate; it does not run until the gate says so.

Standing rule applied throughout: measured, not assumed. Every claim below is
tagged KNOWN (standard textbook result), VERIFIED (citation confirmed online
today, see section 7 for what "confirmed" means under the sandbox's limits),
RECALLED (I believe it is in the literature, not confirmed today), or MINE
(my own reasoning, not a published result).

---

## 0. Which Penrose

"Penrose geometry" names at least three things Roger Penrose built. Only one is
usually meant, but the other two are worth a line because they are *already*
formal computational systems, and that changes what "become" means.

| Object | What it is | Formal status today |
|---|---|---|
| **Penrose tilings** (P1 pentagons, P2 kite/dart, P3 thick/thin rhombs) | aperiodic tilings of the plane forced by local matching rules | Formal in four independent ways (section 2); universal computation on them is proven (section 5c); already used as a quantum error-correcting code (section 3). **This note assumes this is what was meant.** |
| **Spin networks** (Penrose 1971) | trivalent graphs labelled by half-integers; the binor calculus evaluates them combinatorially | Already a formal calculus. Evaluation is a well-defined recursive algorithm. Basis of loop quantum gravity. |
| **Penrose graphical (tensor) notation** | diagrams for tensor contraction | Already a formal notation; the modern form is string diagrams for monoidal categories, which is a full rewriting calculus. |

Name collision to avoid in any search: "Penrose" is also a diagram-drawing
language from CMU (penrose.cs.cmu.edu), unrelated. KNOWN.

If Mal meant spin networks or the diagram notation, the answer is "it already
is one; the work is picking a rewriting engine". The rest of this note is
about the tilings.

---

## 1. Verdict in five lines

1. **Yes, as a formal system.** The tilings have an alphabet, axioms, inference
   rules, theorems, a decision procedure for theoremhood, and a canonical
   address for every symbol. Section 4 writes that skeleton down. Nothing
   needs inventing.
2. **No, as a computer in the geometry alone.** A Penrose tiling carries
   essentially zero information: its entropy is zero, and the whole tiling is
   fixed by a handful of real parameters. A program has to be *placed on* the
   tiling, it cannot be *the* tiling. Section 3.
3. **Yes, as a substrate, and this is settled.** Universal cellular automata
   on the kite-and-dart tiling were published in 2012–2013 (Imai, Hatsuda,
   Poupet, Sato; Sato, Imai, Iwamoto), and a Life-isomorphic automaton on any
   multigrid tiling in 2017 (Bailey, Lindsey). VERIFIED. Section 5c.
4. **The address is the interesting formal object, and it is already built.**
   Every tile has an infinite hierarchical address; Penrose tilings are
   classified by 0/1 index sequences with no two consecutive 1s, which is
   exactly the Zeckendorf constraint. Simon Tatham has implemented
   "combinatorial coordinates" with finite-state transducers for Penrose, hat
   and spectre (2024–2025). VERIFIED. Section 5b.
5. **The standout use is the one where the "defect" is the feature.** Li and
   Boyle (2023) showed the Penrose tiling is a quantum error-correcting code
   precisely because no local measurement can distinguish two tilings and
   any bounded erasure is recoverable from the outside. VERIFIED. Section 3.

---

## 2. What is already formal about Penrose tilings

Four formalisations exist. They are provably equivalent descriptions of the
same set of tilings (KNOWN, de Bruijn 1981 for 2↔3; Grünbaum & Shephard ch. 10
for 1↔2).

### 2a. Matching rules — a constraint system

Tiles carry edge markings (arrows, or Ammann bars). A tiling is *legal* iff
every shared edge has matching marks. Only 7 vertex configurations occur in
the kite/dart tiling (sun, star, ace, deuce, jack, queen, king) and 8 in the
rhombus tiling. KNOWN.

This is a constraint-satisfaction system with a finite local rule set, i.e.
exactly the form of a Wang tile set. Any marked tile set converts to Wang
tiles (KNOWN, Grünbaum & Shephard 11.1), so Penrose tilings are a
sub-language of the Wang-tile language. In symbolic-dynamics terms the
Penrose tilings form a *sofic* shift: Goodman-Strauss proved that almost
every substitution tiling admits local matching rules, and Vereshchagin
(2026) reduced the hypothesis to finite local complexity. VERIFIED (abstract).

### 2b. Substitution (inflation / deflation) — a rewriting system

Each tile rewrites to a fixed patch of smaller tiles (inflation); the
inverse, composition, is *unique* on legal patches (KNOWN; this uniqueness
is the "recognizability" property, and it is what forces aperiodicity).

This is a 2-D L-system: a deterministic parallel rewriting system on a
2-letter alphabet with geometric placement. Its 1-D shadow is the Fibonacci
substitution `a → ab, b → a`, and the tile-count matrix is the Fibonacci
matrix `[[1,1],[1,0]]` with Perron eigenvalue φ. KNOWN. Several GitHub
generators are literally L-systems (section 8).

### 2c. Cut-and-project — an arithmetic semantics

De Bruijn: every Penrose rhombus tiling is the projection of a 2-plane slice
through the integer lattice Z^5 (equivalently a "pentagrid" of five families of
parallel lines with offsets γ_0..γ_4 summing to 0). KNOWN.

Consequence for computation: given the offsets to enough precision, the tile
at any position is computable by rounding and comparing — polynomial time, no
search, no backtracking. The tiling is a computable object in the same sense
a Sturmian word is: `floor((n+1)α) − floor(nα)` in 1-D, its 5-dimensional
analogue here. MINE as a statement; the underlying construction is KNOWN.

### 2d. Tiling space as a dynamical system / C*-algebra

The set of all Penrose tilings, modulo translation, is a compact space with an
R^2 (translation) action. It is strictly ergodic, has zero topological
entropy, and is an almost 1:1 extension of a minimal rotation on the 4-torus;
inflation is an almost 1:1 extension of a hyperbolic toral automorphism.
VERIFIED (E. A. Robinson Jr, Trans. AMS 348 (1996) 4447–4464). Connes uses
the same space as his opening example of a non-commutative space: the AF
C*-algebra built from the substitution's Bratteli diagram, with K_0 ≅ Z + Zφ
ordered by the golden ratio (KNOWN, Connes 1994, ch. II).

That last object is a purely combinatorial formal system (a Bratteli diagram
is a graded graph; the AF algebra is its inductive limit) and its K-theory is
computed by a finite-matrix calculation. It is the most "algebraic" face of
the geometry.

---

## 3. What the geometry cannot be on its own — and where that is the point

**Zero entropy.** The number of distinct legal patches of radius r grows
polynomially in r, not exponentially (KNOWN, finite local complexity plus
substitution). A universal computer's configuration space grows
exponentially. So a Penrose tiling cannot encode an arbitrary computation in
its tile pattern the way a Wang tiling can encode a Turing machine history
(Berger 1966, KNOWN). Contrast Kari's 14 aperiodic Wang tiles (KNOWN, Kari
1996): those are aperiodic *because* each row performs a multiplication on a
Beatty sequence. Kari's tiles compute; Penrose's tiles are computed. Labbé
(2024) pushes the Kari side further: a family of aperiodic Wang tile sets
defined as instances of a "square-shaped computer chip" whose inputs and
outputs are 3-vectors of integers, covering every metallic mean, with the
golden-ratio case containing Ammann's 16 tiles. VERIFIED (abstract).

**Uncountably many models, all locally identical.** Every finite patch that
appears in one Penrose tiling appears in every other, within a distance
proportional to the patch diameter (KNOWN, Conway's local isomorphism
theorem; the constant is about φ³/2 · diameter, RECALLED). So no finite
observation distinguishes two tilings. As a "formal system" it has one
theory and uncountably many models, all elementarily equivalent at every
finite radius. That is a feature for a coordinate scheme (every address
scheme works everywhere) and a defect for a memory (you cannot store a bit
in the geometry).

**And that defect is exactly what Li and Boyle used.** "The Penrose Tiling
is a Quantum Error-Correcting Code" (arXiv 2311.13040, Nov 2023): code
states are superpositions over an equivalence class of tilings; local
indistinguishability means no bounded measurement can read the logical
information, and *local recoverability* (the tiles deleted from any bounded
region are uniquely determined by the tiles outside it) means any bounded
erasure, however large, is correctable. Variants on Ammann–Beenker and
Fibonacci tilings live on finite tori and in discrete spin systems. A 2026
follow-up does the same for the hat and spectre monotiles (arXiv
2607.15326). VERIFIED (abstracts, Quanta coverage Feb 2024, Error Correction
Zoo entry). This is the strongest existing sense in which Penrose geometry
*is* a computational system: not a processor, a memory whose protection is
the geometry.

**Growth is not local.** A tiling cannot be grown one tile at a time by a
rule that looks only at nearby tiles: there exist "deceptions" of arbitrary
order, patches that satisfy the matching rules everywhere but cannot be
extended (KNOWN, Penrose; Grünbaum & Shephard 10.5; a deception can be as
small as three tiles and exists at every inflation scale, VERIFIED via
Onoda et al.). Vertex-rule growth with local rules is possible if the rules
are richer than the arrow-matching rules (VERIFIED, Onoda, Steinhardt,
DiVincenzo, Socolar, PRL 60 (1988) 2653–2656). This is the point Penrose
himself leaned on in *The Emperor's New Mind* (KNOWN that he argued it;
whether the argument holds is contested).

**But legality of a finite patch is decidable.** Compose (deflate) the patch
repeatedly. Composition on a legal patch is unique and always succeeds; on an
illegal one it eventually fails. Because composition shrinks the patch by φ
each step, the patch reduces in O(log r) steps to something of bounded size
that can be checked against a finite table. RECALLED as a published result;
the reasoning is MINE and I am confident in it, but Dispatch should check
Grünbaum & Shephard 10.5 before it is cited. The contrast with the general
domino problem (undecidable) is the sharp fact: **Penrose tilings are a
decidable fragment of an undecidable system.**

---

## 4. The formal-system skeleton

Written out so it can be compared to any other system the fleet holds. Rhombus
version (P3); kite/dart is the same with a different table.

| Component | Penrose instance | Status |
|---|---|---|
| Alphabet | two tile types T (thick, 72°) and t (thin, 36°), each with an arrowed-edge marking and one of 10 orientations | KNOWN |
| Well-formed strings | finite patches: sets of placed tiles, edge-to-edge, no overlap | KNOWN |
| Axioms | the 8 legal vertex configurations | KNOWN |
| Inference rule 1 | inflation: T ↦ patch(T), t ↦ patch(t), applied in parallel | KNOWN |
| Inference rule 2 | extension: add a tile whose every shared edge matches | KNOWN (this rule alone does not preserve legality; see deceptions) |
| Theorems | legal patches (those extendable to an infinite legal tiling) | KNOWN |
| Decision procedure | repeated composition until bounded size, then table lookup | RECALLED, see 3 |
| Canonical form / address | for a tile x: the sequence (type of x, type of parent(x), type of parent²(x), …) with the position of each in its parent; a Penrose tiling is determined by a 0/1 index sequence with no two consecutive 1s, and two sequences give the same tiling iff they agree from some position on | VERIFIED (Conway index sequence, Grünbaum & Shephard; Robinson 1996; Tatham's combinatorial coordinates) |
| Semantics | cut-and-project: model = a choice of offsets γ in the internal space; interpretation of a tile = a lattice point of Z^5 whose projection lands in the window | KNOWN |
| Language class | sofic shift (local matching rules exist for the substitution) | VERIFIED (Goodman-Strauss; Vereshchagin 2026) |
| Invariants | tile-frequency ratio φ:1; inflation matrix [[2,1],[1,1]] for rhombs; K_0 = Z + Zφ | KNOWN |
| Complexity | patch counting polynomial; entropy 0; local isomorphism radius linear in patch size | KNOWN |

Read as a logic: it is a *complete, decidable, categorical-at-every-finite-
level* theory with no room for undecidable sentences. That is the opposite of
the usual "formal system" a fleet wants when it says computation.

---

## 5. Three ways it becomes computational

### 5a. The substitution machine (the tiling as the program)

Run inflation as the step function of a rewriting system. Inputs: a seed
patch and a level n. Output: the level-n patch. This is a real, if narrow,
computer: it computes exactly the Penrose patches and nothing else. Its
value is as a generator and as a *test oracle* for 5b and 5c, not as a
general engine.

### 5b. The address system (the tiling as memory layout) — already built

Treat the Conway address as a numeration. In 1-D the analogue is exact:
positions in the Fibonacci word are Zeckendorf numerals (sums of non-adjacent
Fibonacci numbers), and the substitution is the carry rule. In 2-D the index
sequence of a Penrose tiling is *literally* a 0/1 sequence with no two
consecutive 1s (the Zeckendorf condition), so the analogy is a theorem, not
a metaphor. VERIFIED (ICERM Solomyak tutorial; neverendingbooks summary of
Grünbaum & Shephard).

The engineering is done. Simon Tatham's "combinatorial coordinates" give
every tile its hierarchy address without geometry, and finite-state
transducers walk from a tile to its neighbours by a bounded carry across
levels. Covered: Penrose P2/P3, hat, spectre. Two substitution systems for
the hat are "unambiguous": a single tile address determines the whole plane.
Blog series 2024 ("Beyond the wall", "Combinatorial coordinates for the
aperiodic Spectre tiling", "Aperiodic Tilings V: the Refinable Frontier"),
paper "Finite-state transducers for substitution tilings" arXiv 2512.16595
(Dec 2025). VERIFIED (abstract and search snippets; the pages themselves are
egress-blocked from the sandbox). The code ships in his puzzle collection's
Penrose, hat and spectre grid generators, RECALLED.

What this buys: a self-similar, non-periodic, repetitive addressing scheme
where "zoom out by one level" is a single symbol drop, with a regular
language for neighbourhood. That is closer to what the fleet's lattices and
vessels already do (hierarchical, versioned, never mutated) than any
computation-on-the-tiles idea. If Penrose enters the system anywhere, this
is the door I would point at, and the transducer approach is the one to
copy, not reinvent.

### 5c. The substrate (the tiling as the machine's tape) — universality proven

Put a finite state on every tile and update by a local rule over the tile's
neighbours. The history, all VERIFIED:

| Year | Who | Result |
|---|---|---|
| 2010 | Owens & Stepney | Life rules on P2 and P3; still lifes and oscillators; statistics differ between kite/dart and rhombs; no glider found |
| 2012 | Goucher | first glider on an aperiodic tiling: 4-state outer-totalistic CA, works on generic quadrilateral tilings. J. Cellular Automata 7(5–6) 385–392 |
| 2012–13 | Imai, Hatsuda, Poupet, Sato | semi-totalistic CA on kite/dart simulating any boolean circuit, hence any Turing machine (AUTOMATA/JAC 2012; Fundamenta Informaticae 2013; a 6-state version, HAL lirmm-01476788). Key move: quasi-periodicity gives a constant N such that the circuit pattern appears in every N×N window |
| 2013 | Sato, Imai, Iwamoto | rotation-symmetric von Neumann neighbourhood: a 5-state universal kite/dart CA (asynchronous circuits) and two 3-state CAs simulating universal logic elements (synchronous). CANDAR 2013 |
| 2017 | Bailey & Lindsey | CA on any multigrid tiling that is *isomorphic* to Conway's Life, so gliders, signal delivery, universal computation and reproduction carry over; next state is a local computation. arXiv 1708.09301 |
| 2020 | — | sandpile toppling on Penrose tilings (arXiv 2006.06254) |
| 2023 | — | Life on the Robinson-triangle Penrose tiling: still lifes (arXiv 2302.10157) |

So the question I left OPEN in the first draft is closed: **cellular
automata on Penrose tilings are computationally universal**, by two
independent routes (direct circuit embedding, and isomorphism to Life).

The algorithmic self-assembly model (aTAM, Winfree 1998, KNOWN) is the other
substrate: Wang-like tiles with glue strengths that attach only when
sufficiently bound. aTAM is Turing universal. A small JavaScript simulation
of P3 self-assembly under Socolar's growth rules exists on GitHub
(section 8), but I found no aTAM-universality result specific to Penrose
tiles; that remains a research question, not a build task.

---

## 6. What could be built first (proposal, gated)

Revised after section 8. Smallest experiments with a measured outcome. None
of them touch existing vessels. Each is one new file, except where an
existing open-source tool is the better base.

| # | Task | Measures | Gate |
|---|---|---|---|
| 1 | `penrose_sub.py`: rhombus inflation, N levels, exact coordinates in Z[φ] (no floats) | tile counts per level equal Fibonacci-matrix prediction; patch legality by vertex table = 100% | none (pure, new) |
| 2 | Composition (deflation) on a patch; decide legality; feed it deliberate deceptions | every deception rejected, every inflated patch accepted; depth at rejection recorded | after 1 |
| 3 | Combinatorial coordinates per tile, neighbour-by-transducer, following Tatham's construction rather than my sketch; check two independently generated tilings agree on all radius-r patches | local-isomorphism radius observed vs the φ³/2 bound; transducer state count | after 2; needs Tatham's paper fetched (arXiv is egress-blocked here) |
| 4 | Cut-and-project generator (de Bruijn pentagrid) with rational offsets; cross-check against 1. `pywonderland` and `byewokko/penrose` already do this in Python and can be the reference | identical patch statistics; per-tile agreement inside the window | after 1 |
| 5 | CA on the tiling: reproduce Goucher's glider, then Bailey–Lindsey's Life-isomorphic rule. `Grgs/cellular-automaton-lab` already runs rules on Penrose P3, hat and spectre patches and may be the cheaper base than writing a simulator | glider period and displacement match the paper; a Life glider survives on the Bailey–Lindsey rule | after 3; papers fetched and verified |

If 1–3 pass, the fleet has a working formal system (alphabet, rules,
decision procedure, canonical address) in a few hundred lines, and a
verified statement of exactly what it can and cannot do. Task 5 now has a
known answer to reproduce rather than an open question to settle.

What NOT to do: do not put Penrose structure into any lattice or A-Z. It is
not knowledge and it is not a source (DEC-0004 by analogy). It is, at most,
an addressing scheme; and that is a MM_VERSION-level decision, not a script's.

---

## 7. Sources

Verification status is about the *citation*, not the claim. What "VERIFIED"
means today: the sandbox egress proxy blocks arxiv.org, semanticscholar.org,
wikipedia.org, chiark.greenend.org.uk, cp4space.hatsya.com and
oldcitypublishing.com; github.com is reachable. So VERIFIED below means the
citation and abstract were confirmed through search-engine snippets of those
pages, or by fetching a reachable mirror (HAL, LIRMM, ADS, AMS, PubMed,
Steinhardt's site), not by reading the paper. Nothing below was read in full.

| Ref | Status |
|---|---|
| R. Penrose, "The rôle of aesthetics in pure and applied mathematical research", Bull. IMA 10 (1974) | KNOWN (the original) |
| R. Penrose, "Pentaplexity", Math. Intelligencer 2 (1979) | KNOWN |
| N. G. de Bruijn, "Algebraic theory of Penrose's non-periodic tilings of the plane I, II", Indag. Math. 43 (1981) | KNOWN |
| B. Grünbaum & G. C. Shephard, *Tilings and Patterns* (1987), ch. 10 (Penrose, deceptions, composition, index sequences), ch. 11 (Wang tiles) | KNOWN |
| R. Berger, "The undecidability of the domino problem", Mem. AMS 66 (1966) | KNOWN |
| R. M. Robinson, "Undecidability and nonperiodicity for tilings of the plane", Invent. Math. 12 (1971) | KNOWN |
| J. Kari, "A small aperiodic set of Wang tiles", Discrete Math. 160 (1996) | KNOWN |
| A. Connes, *Noncommutative Geometry* (1994), ch. II §3, Penrose tilings | KNOWN |
| E. A. Robinson Jr, "The dynamical properties of Penrose tilings", Trans. AMS 348(11) (1996) 4447–4464 | VERIFIED (AMS page) |
| G. Onoda, P. Steinhardt, D. DiVincenzo, J. Socolar, "Growing perfect quasicrystals", PRL 60(25) (1988) 2653–2656 | VERIFIED (PubMed, Steinhardt site) |
| N. Owens & S. Stepney, "Investigations of Game of Life cellular automata rules on Penrose tilings: lifetime, ash, and oscillator statistics", J. Cellular Automata (2010); and "The Game of Life rules on Penrose tilings: still life and oscillators", in Adamatzky (ed.) *Game of Life Cellular Automata*, Springer (2010) | VERIFIED (York research database, Springer) |
| A. P. Goucher, "Gliders in cellular automata on Penrose tilings", J. Cellular Automata 7(5–6) (2012) 385–392 | VERIFIED (publisher index; one index lists 2013) |
| K. Imai, T. Hatsuda, V. Poupet, K. Sato, "A universal semi-totalistic cellular automaton on kite and dart Penrose tilings", AUTOMATA & JAC 2012; Fundamenta Informaticae (2013); arXiv 1208.2771; 6-state version HAL lirmm-01476788 | VERIFIED (HAL, LIRMM, ADS) |
| K. Sato, K. Imai, C. Iwamoto, "Universal von Neumann neighborhood cellular automata on Penrose tilings", CANDAR 2013, IEEE | VERIFIED (IEEE Xplore 6726954) |
| D. A. Bailey & K. A. Lindsey, "A Game of Life on Penrose tilings", arXiv 1708.09301 (2017) | VERIFIED (ADS, Complexity Digest) |
| Z. Li & L. Boyle, "The Penrose tiling is a quantum error-correcting code", arXiv 2311.13040 (2023) | VERIFIED (Quanta 2024-02-23; Error Correction Zoo) |
| "Quantum error-correcting codes from aperiodic monotiles: the hat and the spectre", arXiv 2607.15326 (2026) | VERIFIED (title only) |
| S. Labbé, "Metallic mean Wang tiles II: the dynamics of an aperiodic computer chip", Forum of Math. Sigma (2025); arXiv 2403.03197 | VERIFIED (Cambridge Core) |
| S. Tatham, "Beyond the wall: working with aperiodic tilings using finite-state transducers" and related posts (2024); "Finite-state transducers for substitution tilings", arXiv 2512.16595 (2025) | VERIFIED (search snippets; site blocked) |
| N. Vereshchagin, "Matching rules for substitution and hierarchical tilings for any substitution with finite local complexity", arXiv 2606.25005 (2026) | VERIFIED (abstract) |
| F. D'Andrea, "A guide to Penrose tilings", arXiv 2310.18950 (2023) | VERIFIED (title); modern survey, good first fetch for Dispatch |
| E. Winfree, *Algorithmic Self-Assembly of DNA*, PhD thesis, Caltech (1998) | KNOWN |
| M. Baake & U. Grimm, *Aperiodic Order*, vol. 1 (2013) | KNOWN (the modern reference for 2b–2d) |
| M. Senechal, *Quasicrystals and Geometric Order* (1995) | KNOWN |
| R. Penrose, *The Emperor's New Mind* (1989), ch. 4, tilings and non-computability | KNOWN |
| R. Penrose, "Angular momentum: an approach to combinatorial space-time" (1971), spin networks | KNOWN |
| D. Smith, J. S. Myers, C. Kaplan, C. Goodman-Strauss, "An aperiodic monotile", Combinatorial Theory 4(1) (2024); "A chiral aperiodic monotile", 4(2) (2024) | VERIFIED (via the Lean repo README) |

---

## 8. Prior art found online, 2026-09-17

Question asked: is anyone using this idea, on GitHub or elsewhere? Answer:
yes, in every direction this note proposes, and one direction it did not
anticipate (error correction). Nobody found is doing it for the fleet's
purpose (addressing for a versioned knowledge store), and nothing found is
positioned as "Penrose as a formal system"; the pieces are spread across
CA people, dynamicists, a puzzle author, a Lean formaliser and quantum
information theorists.

### 8a. GitHub — computation or formalism, not just drawing

| Repo | What | Language | Signal |
|---|---|---|---|
| `Grgs/cellular-automaton-lab` | "topology-first" CA playground: one rule engine over 68 tiling families incl. Penrose P3, pinwheel, hat, turtle, spectre, Taylor–Socolar; 15 built-in rules (Life-like, excitable, signal) | Python + TypeScript, MIT | 1,079 commits, v0.5.0, 0 stars. Active, unknown. The closest thing to task 5's base |
| `IntKecsk/Apery` | Penrose P3 generator by triple deflation plus CA simulator (Life, von Neumann CA planned) | C++/Qt5, GPL-3 | 14 commits, 1 star, early |
| `jsm28/AperiodicMonotilesLean` | Lean formalisation of the hat and spectre papers by their second author, staging for mathlib | Lean | 180 commits, steps 1–2 of 24 in progress. Penrose tilings not covered. The only *formal-proof* work found |
| `kosh90/self-assembly_penrose_tiles-check` | self-assembly of P3 under Socolar's growth rules, browser demo | JavaScript/Paper.js | 5 commits, toy |
| `neozhaoliang/pywonderland` | de Bruijn pentagrid Penrose among many maths demos | Python | 4.2k stars; reference implementation for task 4 |
| `byewokko/penrose` | de Bruijn multigrid generator | Python | 17 stars |
| `cimi/penrose-tiling`, `luke-r-mills/L_System_Penrose_Generator` | Penrose via L-systems | JS; Processing | small; confirm 2b is how people actually build it |
| `PenroseRhombus_PCB` | PCBs tiling a P3 with addressable LEDs | hardware | physical substrate for a CA, 2 stars |
| `QuasiCrystal.jl` | Fibonacci chain, Penrose, Ammann–Beenker for physics | Julia | 4 stars |

Plain generators (drawing only): `xnx/penrose`, `JesusFreke/pynrose`,
`samm00/penrose`, `bpandreotti/rose` (Rust), `roothch/TilingGallery` (Rust,
pentagrid), `apaleyes/penrose-tiling` (JS), `mathworks/penrose-tiling`,
`arnfred/Penrose`, `cole-k/Penrose-Tiling`, `daneroo/im-penrose`, and a
dozen more under the `penrose-tilings` topic. None carries matching-rule
checking or composition as a decision procedure, as far as the READMEs
show.

Not found on GitHub: a composition-based legality checker; Tatham's
transducer code as a standalone library (it lives inside his puzzle
collection); any Lean or Coq formalisation of Penrose tilings themselves.

### 8b. Literature — who has done what

| Idea in this note | Already done by |
|---|---|
| 5c universality on the substrate | Imai–Hatsuda–Poupet–Sato 2012/13; Sato–Imai–Iwamoto 2013; Bailey–Lindsey 2017 |
| 5b hierarchical addressing with a regular neighbour language | Tatham 2024–25 (Penrose, hat, spectre) |
| 3 "cannot store a bit in the geometry" | inverted into a QECC by Li–Boyle 2023; extended to hat/spectre 2026 |
| 2a Penrose as sofic / Wang sub-language | Goodman-Strauss; Vereshchagin 2026 |
| 3 "Kari's tiles compute, Penrose's are computed" | Labbé 2024 builds the computing side out for every metallic mean |
| 2d dynamics | Robinson 1996; a 2026 Markov-partition construction for hat tilings (arXiv 2604.20964) |
| Local growth | Onoda et al. 1988; "Growing perfect decagonal quasicrystals by local rules" (arXiv 0704.0848) |

### 8c. What this changes

- Task 5 is a reproduction, not a research question.
- Task 3 should follow Tatham, whose construction is published and tested
  on three tilings, instead of my sketch.
- The one genuinely under-explored corner I can see is the *decision
  procedure* (composition-based legality with deception detection) as a
  standalone, tested artefact; nobody on GitHub ships one. That is task 2,
  and it is small.
- The error-correction result is the strongest argument that Penrose
  geometry is a computational system in its own right, and it is the one
  that maps least onto anything the fleet does. Worth knowing, not worth
  building on here.

---

## 9. Gödel and geometry: is a piece missing? (added 2026-09-17, second question)

Question asked: Gödel's incompleteness theorem; is there a piece of the
puzzle missing with geometry? Short answer: no piece is missing, but
geometry is where the puzzle's edge is visible, and it is the same edge
sections 3 and 5c keep hitting.

**What Gödel needs.** The first theorem applies to a theory only if it is
consistent, recursively axiomatised, and interprets enough arithmetic to
encode its own sentences (Robinson's Q suffices). Remove any one and the
theorem is silent. Geometry fails the third. KNOWN.

**Plain geometry is complete.** Tarski (1951): first-order Euclidean
geometry, i.e. the theory of real closed fields, is complete and decidable
by quantifier elimination. Hilbert's second-order axioms (1899, with the
completeness axiom) are categorical. So geometry is not a hole in Gödel's
picture; it is the textbook example of a mathematics Gödel leaves alone.
KNOWN.

**The price.** Real closed fields cannot say "x is an integer". The
continuum offers no place to write an unbounded string of discrete symbols,
so no sentence can refer to itself. Presburger arithmetic escapes the same
way by dropping multiplication. Completeness is bought by giving up the
capacity to store arbitrary finite information inside the theory. That is
the zero-entropy fact of section 3 in another dress: a Penrose tiling
cannot hold a bit, and patch legality is decidable. MINE as the framing;
each half KNOWN.

**Where geometry lets Gödel back in.** As soon as an unbounded discrete
structure is laid on the plane, arithmetic returns:

| Discrete structure on a geometric substrate | Status |
|---|---|
| Wang's domino problem (does a finite tile set tile the plane) | undecidable, Berger 1966. KNOWN |
| Domino problem in the hyperbolic plane | undecidable, Margenstern 2008 and Kari. RECALLED |
| Homeomorphism of manifolds, dimension ≥ 4 | undecidable, Markov 1958, via the word problem for groups. KNOWN |
| Halting of a configuration on a universal Penrose-substrate CA (section 5c) | undecidable. MINE, immediate from universality |
| Legality of a finite Penrose patch (section 3) | decidable. RECALLED |

The boundary runs through the Penrose tiling itself: the geometry is on
the decidable side, the geometry plus states on the tiles is on the other.

**The structural view.** If the popular telling is missing a piece, it is
that incompleteness is not about numbers. Lawvere's fixed-point theorem
(1969; Yanofsky 2003 for the survey) shows Cantor's diagonal, Gödel's
sentence, Tarski's undefinability of truth, Turing's halting problem and
Russell's paradox are one theorem about self-reference in any cartesian
closed category with a point-surjective map onto its own function space.
Numbers are the cheapest carrier. Geometry neither adds nor removes the
phenomenon; it only decides whether a carrier is available. KNOWN.

**On Penrose's own use of Gödel.** *The Emperor's New Mind* (1989) and
*Shadows of the Mind* (1994) argue from Gödel that mathematical insight is
non-computable, with tilings as the illustration. Putnam, Feferman, Davis
and Franzén rejected the argument: Gödel shows a consistent system cannot
prove its own consistency, not that a human can see truths no system can.
KNOWN that the critiques exist; the fleet should not build doctrine on the
argument.

**For the formal-system thread.** The knob is information capacity.

| Below the line: complete, decidable, cannot count | Above the line: expressive, incomplete, undecidable |
|---|---|
| real closed fields (Tarski) | Peano arithmetic |
| Presburger arithmetic | Wang tilings |
| Rabin's monadic theory of the infinite binary tree (S2S) | any universal substrate, Penrose CA included |
| Penrose patch legality | Penrose CA halting |

A Penrose tiling is interesting here because it sits on the line and one
steps over it by adding states to the tiles. Nothing is missing. The
puzzle has an edge, and geometry is where a finger can be put on it.

Sources for this section, all KNOWN unless tagged: A. Tarski, *A Decision
Method for Elementary Algebra and Geometry* (1951); D. Hilbert,
*Grundlagen der Geometrie* (1899); M. Presburger (1929); R. Berger (1966);
M. Margenstern, "The domino problem of the hyperbolic plane is
undecidable", Theor. Comp. Sci. 407 (2008), RECALLED; A. A. Markov,
"Insolubility of the problem of homeomorphy" (1958); F. W. Lawvere,
"Diagonal arguments and cartesian closed categories" (1969); N. Yanofsky,
"A universal approach to self-referential paradoxes, incompleteness and
fixed points", Bull. Symb. Logic 9 (2003); M. O. Rabin, "Decidability of
second-order theories and automata on infinite trees", Trans. AMS 141
(1969); T. Franzén, *Gödel's Theorem: An Incomplete Guide to Its Use and
Abuse* (2005); H. Putnam, review of *Shadows of the Mind*, Bull. AMS 32
(1995); S. Feferman, "Penrose's Gödelian argument", Psyche 2 (1995).

---

## 10. GAR and the Penrose system (added 2026-09-17, third question)

Question asked: does this mean GAR can embed the Penrose system into the
MATRIX? Source for GAR: the work order filed 2026-09-16 for GAR, the
Geometry Algebra Reasoner (`chip_gar_20260916.py`, GA_0001), nine parts BB
to PP. Not in this repo; read from Mal's upload. What it fixes:

- product bar: "every answer a plan of exact operations over sets,
  sequences and counts, each step with its size and addresses; the same
  question gives the same answer; the eval grades by rule and a drop refuses
  the change";
- operations: intersect, union, minus, follows, contrast, slice,
  read-paragraph, ratio, closure (with depth), similarity as an exact
  fraction, absence, as-of;
- plans are data, stored, replayed, re-graded (BB, CC); memory keeps exact
  sets (EE); the sequence index runs over every shelf (GG); coverage stated
  before every answer (MM); faults refused before serving (NN); one voice,
  same plan and addresses for two wordings (PP).

None of the nine parts contains a geometric operation. "Geometry" in the
name is not yet cashed. That is the opening.

### 10a. Three readings, three answers

| "Embed" means | Answer | Door |
|---|---|---|
| **Knowledge.** Penrose texts on a shelf, a MATRIX that read them, GAR's sequence index over them, GAR answering "which papers on Penrose tilings cite Berger and state universality" | Yes, today, no new code | shelf → recipe → `--on` → GG indexes the shelf. Ordinary. Mal's gate to open the vessel |
| **Algebra.** The Penrose formal system's own operations (inflate, compose, legal, address, neighbour, patch-at-address, exact φ-ratios) added to GAR's operation set, so tiles become addressed elements GAR's set algebra already handles | Yes, and it fits the product bar exactly; one new work-order step | a step in the GAR order, selftest acceptance, same form as FF (`garops`). Draft below |
| **Substrate.** A universal cellular automaton on the tiling (section 5c) running inside GAR or a MATRIX | No | a CA run has no size bound and its halting is undecidable (section 9). It cannot be "a plan with its size" and "the same answer every time" is not guaranteed. Above the line; GAR is built to stay below it |

### 10b. Why the second reading fits

Match the product bar to the Penrose system, item by item:

| Product bar | Penrose instance | Section |
|---|---|---|
| exact operations | inflate, compose, legality are integer-exact in Z[φ]; no floats | 2b, 4 |
| each step with its size | tile counts per inflation level are the Fibonacci-matrix prediction, known before the step runs | 2b |
| each step with its addresses | combinatorial coordinates give every tile a finite address; neighbours by finite-state transducer | 5b |
| the same question gives the same answer | local isomorphism: the radius-r patch at an address is the same in every model, by theorem, not by seed | 3 |
| graded by rule, a drop refuses | legality is decidable, so every graded answer has a ground truth the eval can compute, not a judgment | 3, 9 |
| coverage stated before answering (MM) | a patch either composes to bounded size or fails at a known depth; coverage is the depth reached | 3 |
| faults refused before serving (NN) | a deception is a planted fault the system detects by construction | 3 |
| the ratio operation | φ:1 tile frequency, the inflation matrix, exact in Z[φ]; the golden ratio is the first ratio GAR would hold exactly rather than as a count fraction | 4 |

Everything on the right terminates, has a size, and is replayable. That is
the Gödel line of section 9 stated as an engineering rule: GAR embeds the
decidable geometry (5a, 5b, the decision procedure) and does not embed the
universal substrate (5c).

### 10c. Draft work-order step

Same form as the nine steps in the order. File
`gar_step_gargeom_draft.json` travels with this note. Not filed; Mal's gate.

```json
{
 "id": "QQ",
 "verb": "gargeom",
 "title": "the first geometric algebra in GAR: Penrose rhombus tiles as addressed elements - inflate(patch,n), compose(patch), legal(patch), address(tile), neighbours(address), patch(address,r), ratio in Z[phi] exact - every op with its size (Fibonacci-matrix count) and its addresses (combinatorial coordinates, Tatham's transducers); GAR's set ops (intersect, minus, contrast, slice) apply unchanged to sets of tile addresses; no cellular automaton on the tiling, ever (no size bound, halting undecidable)",
 "needs": ["BB", "FF"],
 "accept": {"kind": "selftest", "script": "chip_gargeom_YYYYMMDD.py"},
 "why": "GAR is named for geometry and holds none; the Penrose system is the one geometry whose every question is decidable, so it can be graded by rule",
 "selftest": [
  "counts per level equal the inflation-matrix prediction for n = 1..8",
  "every inflated patch is legal; every planted deception (3-tile and one per scale up to level 5) is refused with its depth",
  "two independently generated tilings agree on every radius-r patch at matching addresses, r up to the local-isomorphism bound",
  "the same question, two wordings, two sessions: identical plan and addresses (the PP test on geometric asks)",
  "coverage stated before each answer equals the composition depth reached",
  "no floating point anywhere in the module: all coordinates in Z[phi]"
 ],
 "excluded": "any CA or growth-by-local-rule on the tiling; any patch that is not an inflation image or a checked input"
}
```

### 10d. What I could not check

The twelve shapes, how FF registers a new operation, and whether GAR's
plans can carry a tuple-valued element (an address is a sequence, not a
token). If plans hold only sets of tokens, addresses serialise to strings
with a fixed alphabet, which is what Tatham does, and nothing is lost. The
selftest is the truth either way, per the order's own rule.

---

## 11. GAR and transformers (added 2026-09-17, fourth question)

Question asked: when an AI walks its weights and tensors, can GAR work as
a transformer? Answer: not as a transformer, by doctrine (II, "the learner
without weights"); but as the exact counterpart of what a transformer does
approximately, and as the target language for what interpretability reads
back out of one. This ties GAR to batch B of the MATRIX purge plan
(`ai-interp` curriculum).

**Correspondence.** KNOWN as a description, MINE as the mapping to GAR.

| Transformer | GAR |
|---|---|
| embedding (position in R^d) | address, exact |
| attention (retrieval weighted by similarity) | similarity as an exact fraction, then intersect / slice |
| layers (composition) | plan steps steering steps (CC) |
| residual stream | exact sets in memory (EE) |
| gradient descent | precedent rank by replay and grade (II) |

GAR is a hard-attention, weightless transformer.

**The bridge, compile direction.** RASP (Weiss, Goldberg, Yahav, "Thinking
Like Transformers", ICML 2021): a language of select / aggregate /
elementwise operations whose programs are transformers by construction.
Tracr (Lindner et al., DeepMind, 2023): compiles RASP programs into actual
transformer weights. A GAR plan is a RASP program with exact set semantics,
so a plan can be compiled to weights that compute it. RECALLED for exact
venues; the tools are real and public.

**The bridge, decompile direction.** Mechanistic interpretability recovers
circuits, i.e. discrete operations over addressed features, from trained
weights. That is batch B: Belinkov (probing), Hewitt & Manning (structural
probe), Cunningham et al. (sparse autoencoders), TransformerLens. GAR's
plan language is a place to write a recovered circuit so it can be
replayed, graded and refused. This is the concrete use. MINE.

**The Gödel line, again.** A single forward pass is a bounded-depth
circuit and provably limited in the languages it recognises (Hao, Angluin,
Frank 2022, hard-attention transformers; Merrill & Sabharwal 2023, TC0 and
chain of thought; RECALLED). Only the unbounded autoregressive loop lifts a
transformer to universality. Same split as inflate-to-level-n (section 5a)
versus a CA on the tiling (5c). GAR keeps the loop bounded by depth (FF
closure "with depth"), which is why it stays gradeable.

**Penrose here.** Only as an address scheme for positions, if at all.
Positional encodings are already geometric (rotations by angle). An
aperiodic address scheme for a residual stream is an idea, not a result;
nothing found published. MINE, thin. Not proposed.

**Not to do.** Do not read this as licence to put weights in GAR, or to
put GAR in the loop of a model. The doctrine's line is the value.
