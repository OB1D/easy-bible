# PENROSE GEOMETRY AS A FORMAL COMPUTATIONAL SYSTEM — assessment note

Drafted 2026-09-17 for Mal, from the one-line prompt "Penrose geometry can
become a formal computational system." Status: ASSESSMENT, nothing dispatched.
Nothing here touches C:\CHIP or D:\Atlas. Section 6 is a proposal for the
review gate; it does not run until the gate says so.

Standing rule applied throughout: measured, not assumed. Every claim below is
tagged KNOWN (standard textbook result, citation in section 7), RECALLED
(I believe it is in the literature, citation should be checked before it is
relied on), or MINE (my own reasoning, not a published result).

---

## 0. Which Penrose

"Penrose geometry" names at least three things Roger Penrose built. Only one is
usually meant, but the other two are worth a line because they are *already*
formal computational systems, and that changes what "become" means.

| Object | What it is | Formal status today |
|---|---|---|
| **Penrose tilings** (P1 pentagons, P2 kite/dart, P3 thick/thin rhombs) | aperiodic tilings of the plane forced by local matching rules | Formal in four independent ways (section 2), but not by themselves a universal computer (section 3). **This note assumes this is what was meant.** |
| **Spin networks** (Penrose 1971) | trivalent graphs labelled by half-integers; the binor calculus evaluates them combinatorially | Already a formal calculus. Evaluation is a well-defined recursive algorithm. Basis of loop quantum gravity. |
| **Penrose graphical (tensor) notation** | diagrams for tensor contraction | Already a formal notation; the modern form is string diagrams for monoidal categories, which is a full rewriting calculus. |

If Mal meant spin networks or the diagram notation, the answer is "it already
is one; the work is picking a rewriting engine". The rest of this note is
about the tilings, where the question is real.

---

## 1. Verdict in four lines

1. **Yes, as a formal system.** The tilings have an alphabet, axioms, inference
   rules, theorems, a decision procedure for theoremhood, and a canonical
   address for every symbol. Section 4 writes that skeleton down. KNOWN parts
   throughout; nothing needs inventing.
2. **No, as a computer in the geometry alone.** A Penrose tiling carries
   essentially zero information: its entropy is zero, and the whole tiling is
   fixed by a handful of real parameters. A program has to be *placed on* the
   tiling, it cannot be *the* tiling. Section 3.
3. **Yes, as a substrate.** Cellular automata on Penrose tilings support
   gliders (RECALLED, Goucher 2012), which is the usual first step toward
   universality. Self-assembly models in which tiles compute are Turing
   universal in general (KNOWN, Winfree). Section 5.
4. **The interesting formal object is the address, not the tile.** Every tile
   in a Penrose tiling has an infinite hierarchical address (its chain of
   parents under composition). Two tilings are the same up to translation iff
   their addresses agree eventually. That is a 2-D numeration system, the
   plane analogue of Zeckendorf / Fibonacci coding. If anything in the fleet
   should use Penrose structure, it is that: an aperiodic, self-similar,
   repetitive coordinate scheme. Section 5b.

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
sub-language of the Wang-tile language.

### 2b. Substitution (inflation / deflation) — a rewriting system

Each tile rewrites to a fixed patch of smaller tiles (inflation); the
inverse, composition, is *unique* on legal patches (KNOWN; this uniqueness
is the "recognizability" property, and it is what forces aperiodicity).

This is a 2-D L-system: a deterministic parallel rewriting system on a
2-letter alphabet with geometric placement. Its 1-D shadow is the Fibonacci
substitution `a → ab, b → a`, and the tile-count matrix is the Fibonacci
matrix `[[1,1],[1,0]]` with Perron eigenvalue φ. KNOWN.

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

The set of all Penrose tilings, modulo translation, is a compact space with a
Z^2 (translation) action. It is minimal, uniquely ergodic, has zero
topological entropy, and is almost-everywhere conjugate to a rotation on a
4-torus (RECALLED, E. A. Robinson Jr 1996). Connes uses the same space as his
opening example of a non-commutative space: the AF C*-algebra built from the
substitution's Bratteli diagram, with K_0 ≅ Z + Zφ ordered by the golden
ratio (KNOWN, Connes 1994, ch. II).

That last object is a purely combinatorial formal system (a Bratteli diagram
is a graded graph; the AF algebra is its inductive limit) and its K-theory is
computed by a finite-matrix calculation. It is the most "algebraic" face of
the geometry.

---

## 3. What the geometry cannot be on its own

**Zero entropy.** The number of distinct legal patches of radius r grows
polynomially in r, not exponentially (KNOWN, finite local complexity plus
substitution). A universal computer's configuration space grows
exponentially. So a Penrose tiling cannot encode an arbitrary computation in
its tile pattern the way a Wang tiling can encode a Turing machine history
(Berger 1966, KNOWN). Contrast Kari's 14 aperiodic Wang tiles (KNOWN, Kari
1996): those are aperiodic *because* each row performs a multiplication on a
Beatty sequence. Kari's tiles compute; Penrose's tiles are computed.

**Uncountably many models, all locally identical.** Every finite patch that
appears in one Penrose tiling appears in every other, within a distance
proportional to the patch diameter (KNOWN, Conway's local isomorphism
theorem; the constant is about φ³/2 · diameter, RECALLED). So no finite
observation distinguishes two tilings. As a "formal system" it has one
theory and uncountably many models, all elementarily equivalent at every
finite radius. That is a feature for a coordinate scheme (every address
scheme works everywhere) and a defect for a memory (you cannot store a bit
in the geometry).

**Growth is not local.** A tiling cannot be grown one tile at a time by a
rule that looks only at nearby tiles: there exist "deceptions" of arbitrary
order, patches that satisfy the matching rules everywhere but cannot be
extended (KNOWN, Penrose; Grünbaum & Shephard 10.5). Vertex-rule growth needs
some non-local information (RECALLED, Onoda–Steinhardt–DiVincenzo–Socolar
1988). This is the point Penrose himself leaned on in *The Emperor's New
Mind* (KNOWN that he argued it; whether the argument holds is contested).

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
| Canonical form / address | for a tile x: the sequence (type of x, type of parent(x), type of parent²(x), …) with the position of each in its parent; two tilings coincide up to translation iff addresses agree past some level | KNOWN (Conway index sequence; Robinson 1996) |
| Semantics | cut-and-project: model = a choice of offsets γ in the internal space; interpretation of a tile = a lattice point of Z^5 whose projection lands in the window | KNOWN |
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

### 5b. The address system (the tiling as memory layout)

Treat the Conway address as a numeration. In 1-D the analogue is exact:
positions in the Fibonacci word are Zeckendorf numerals (sums of non-adjacent
Fibonacci numbers), and the substitution is the carry rule. In 2-D the
address gives every tile a unique hierarchical name, neighbours are found by
a bounded "carry" across levels, and the local-isomorphism theorem
guarantees the same naming works in every model. MINE as a design; the
pieces are KNOWN.

What this buys: a self-similar, non-periodic, repetitive addressing scheme
where "zoom out by one level" is a single symbol drop. That is closer to
what the fleet's lattices and vessels already do (hierarchical, versioned,
never mutated) than any computation-on-the-tiles idea. If Penrose enters
the system anywhere, this is the door I would point at.

### 5c. The substrate (the tiling as the machine's tape)

Put a finite state on every tile and update by a local rule over the tile's
neighbours. Owens & Stepney (RECALLED, 2010) ran Life-like rules on Penrose
tilings; Goucher (RECALLED, 2012) found gliders on the kite/dart tiling. A
glider plus a gun plus collisions is the standard road to a universal CA,
and nothing in the geometry forbids it (the tiling is repetitive, so a
construction that works in one place works everywhere; the varying
neighbourhood sizes are the only obstacle). Whether universality has been
*proven* on a Penrose substrate I do not know; treat as OPEN.

The algorithmic self-assembly model (aTAM, Winfree 1998, KNOWN) is the other
substrate: Wang-like tiles with glue strengths that attach only when
sufficiently bound. aTAM is Turing universal. Penrose tiles in aTAM would
need the glue-strength rule to do the non-local work that deceptions show
is required; that is a real research question, not a build task.

---

## 6. What could be built first (proposal, gated)

Smallest experiments with a measured outcome. None of them touch existing
vessels. Each is one new file.

| # | Task | Measures | Gate |
|---|---|---|---|
| 1 | `penrose_sub.py`: rhombus inflation, N levels, exact coordinates in Z[φ] (no floats) | tile counts per level equal Fibonacci-matrix prediction; patch legality by vertex table = 100% | none (pure, new) |
| 2 | Composition (deflation) on a patch; decide legality; feed it deliberate deceptions | every deception rejected, every inflated patch accepted; depth at rejection recorded | after 1 |
| 3 | Conway address per tile; neighbour-by-carry; check two independently generated tilings agree on all radius-r patches | local-isomorphism radius observed vs the φ³/2 bound | after 2 |
| 4 | Cut-and-project generator (de Bruijn pentagrid) with rational offsets; cross-check against 1 | identical patch statistics; per-tile agreement inside the window | after 1 |
| 5 | CA on the tiling: reproduce a Goucher glider | glider period and displacement match the paper | after 3; needs the paper fetched and verified |

If 1–3 pass, the fleet has a working formal system (alphabet, rules,
decision procedure, canonical address) in a few hundred lines, and a
verified statement of exactly what it can and cannot do. Task 5 is where
"computational" would start to mean universal, and it is the one with an
open literature question behind it.

What NOT to do: do not put Penrose structure into any lattice or A-Z. It is
not knowledge and it is not a source (DEC-0004 by analogy). It is, at most,
an addressing scheme; and that is a MM_VERSION-level decision, not a script's.

---

## 7. Sources

Verification status is about the *citation*, not the claim. The sandbox has
no library access; nothing below was fetched today.

| Ref | Status |
|---|---|
| R. Penrose, "The rôle of aesthetics in pure and applied mathematical research", Bull. IMA 10 (1974) | KNOWN (the original) |
| R. Penrose, "Pentaplexity", Math. Intelligencer 2 (1979) | KNOWN |
| N. G. de Bruijn, "Algebraic theory of Penrose's non-periodic tilings of the plane I, II", Indag. Math. 43 (1981) | KNOWN |
| B. Grünbaum & G. C. Shephard, *Tilings and Patterns* (1987), ch. 10 (Penrose, deceptions, composition), ch. 11 (Wang tiles) | KNOWN |
| R. Berger, "The undecidability of the domino problem", Mem. AMS 66 (1966) | KNOWN |
| R. M. Robinson, "Undecidability and nonperiodicity for tilings of the plane", Invent. Math. 12 (1971) | KNOWN |
| J. Kari, "A small aperiodic set of Wang tiles", Discrete Math. 160 (1996) | KNOWN |
| A. Connes, *Noncommutative Geometry* (1994), ch. II §3, Penrose tilings | KNOWN |
| E. A. Robinson Jr, "The dynamical properties of Penrose tilings", Trans. AMS 348 (1996) | RECALLED, check volume/year |
| G. Onoda, P. Steinhardt, D. DiVincenzo, J. Socolar, "Growing perfect quasicrystals", PRL 60 (1988) | RECALLED |
| N. Owens & S. Stepney, "Investigations of Game of Life cellular automata rules on Penrose tilings", J. Cellular Automata (2010) | RECALLED |
| A. P. Goucher, "Gliders in cellular automata on Penrose tilings", J. Cellular Automata 7 (2012) | RECALLED |
| E. Winfree, *Algorithmic Self-Assembly of DNA*, PhD thesis, Caltech (1998) | KNOWN |
| M. Baake & U. Grimm, *Aperiodic Order*, vol. 1 (2013) | KNOWN (the modern reference for 2b–2d) |
| M. Senechal, *Quasicrystals and Geometric Order* (1995) | KNOWN |
| R. Penrose, *The Emperor's New Mind* (1989), ch. 4, tilings and non-computability | KNOWN |
| R. Penrose, "Angular momentum: an approach to combinatorial space-time" (1971), spin networks | KNOWN |
| D. Smith, J. S. Myers, C. Kaplan, C. Goodman-Strauss, "An aperiodic monotile" (2023) | KNOWN; relevant if a one-tile alphabet is ever wanted |
