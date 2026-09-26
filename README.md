# Korean A1-B1 vocab pack

Static data pack for a language-agnostic vocab trainer (`key: "ko"`). It has
2000 words spanning A1-B1. Each word has a short English gloss. Example
sentences come with English translations. There is no recorded audio: the
trainer speaks every word and sentence with the browser's `ko-KR` voice. The
Read tab adds 60 short reading passages with comprehension questions (see
"Reading passages" below).

**Live:** https://bannerless-studio.github.io/korean/

**Script primer.** A "한글" stage now runs before A1 and teaches Hangul (47
units) with symbol-to-sound, recognition and word-reading items. It's
skippable with "I can read it" and reversible later from Progress.

This repo holds the Korean data pack and the Korean data files its build
reads. It includes [`vocab-engine`](https://github.com/Bannerless-Studio/vocab-engine)
as a git submodule at `engine/`. The engine holds the shared UI, the drill
logic and the shared pack builder, `engine/tools/packbuilder`. The builder's
Korean rules live in `engine/tools/packbuilder/langs/ko.py`.

**Scope note:** this app gives the vocabulary base for B1/TOPIK 3; an exam
also needs grammar, writing and listening practice, which this app does not
teach.

**Data quality:** Hand QA used a stratified sample of 180 words (60 per
level, seed 51) and 90 sentences (seed 52). 178 of 180 words had a clean,
correct primary sense. The other two had a cut-off gloss (눈치) and a
definitional gloss (어머님, "honorific of 어머니"). Both now have overrides, and
an audit of all 2000 glosses for those two defects found one more (문자). In
the final sentence sample, 560 of 562 links were right. One wrong link was 싸다
"cheap" in a generated sentence that meant 싸다 "to poop", and that sentence
was replaced. The other was the spaced 만 하다 "worth doing", which links
하다. Earlier QA rounds found wrong-link classes, and each was fixed by a
rule, never word by word. The fixed classes include:

- homograph verbs such as 살 (사다 "buy" or 살다 "live"), 들어 (듣다 or 들다) and
  걸어 (걷다 or 걸다);
- 네 read as "your" or as "four";
- Sino-Korean numerals before native counters (이 사람 is "this person");
- 한 before 일 or 말 ("did", never "one");
- 건, 걸 and 게 after a modifier (것);
- -고 나서 and -ㄹ지도 모르다 (grammar, never 나서다 or 지도 "map");
- X하고 (결혼하고 is 결혼하다, never 결혼 + 하고);
- ㄹ-dropped honorific readings (마시겠습니까 is 마시다, never 말다).

A check of 20 sentences with conjugated verbs found all 35 verb forms linked
to their lemma, including the ㅂ, 르, ㄹ and 으 irregulars. The top 300 words
have no wrong part of speech and no non-lemma headword. Every closed set is
complete at A1: numbers in both series, days, months, colours, pronouns,
particles, time words and greetings. Every word has at least two example
sentences. Leaving particles aside, 96.5% of word links can be blanked in a
gap drill as a whole word or a listed form.

Tatoeba had too few usable sentences for many words, so 1,692 of the
3,044 sentences were written for this pack: 312 at A1, 543 at A2 and 837 at
B1. Every word has at least two sentences, and every word except three
sensitive ones (죽다, 전쟁, 죽음) has one at its own level. Each generated
sentence is polite (해요체 or 합쇼체), has an English translation and is
marked `"src": "gen"` in `pack/sentences.json`. The count is also in
`pack/attribution.json`. They are machine-written and checked with the
builder's own analyser. They have not been reviewed by a native Korean
speaker. Sexual content and violence are kept out of A1/A2 sentences. Rape,
abuse, suicide and self-harm sentences are left out at every level, and so
are Tatoeba sentences with non-standard spelling (너가, 왔어죠, 할께, 되요 for
돼요). Sentences are chosen polite first, then plain written style, with 반말
last: 0.9% of A1, 1.1% of A2 and 12.9% of B1 sentences are 반말. Levels are
frequency bands with NIKL grade floors and ceilings, not CEFR. Rules, counts
and seeds are in `tools/REPORT.md`, and residuals are in `TODO.md`.

**Fix round (v1.1, 2026-09-25).** A second QA round found wrong-link and
level classes. Each was fixed by a rule in `langs/ko.py`:

- a frequent noun or numeral spelled like a verb form (사고, 신고는, 둘 중,
  하나) links the noun when the English does not attest the verb, and 들어 /
  들었 default to 들다;
- determiners 어떤, 이런, 그런, 저런 and 아무런 are words of their own;
- nominal alts are only the word plus a particle chain or the copula. Light
  verb, compound and bare-contraction spellings are no longer alts;
- names in the English translation (capitalised words) stop their Korean
  spelling from linking a common noun (도쿄, 대만);
- a modifier needs a noun after it, so 이를 닦다 and 그를 믿다 are noun +
  particle, and 화가 난 것 is 나다;
- -기 after a verb stem is the nominaliser (배우기 is 배우다), and 도시다 is
  도시 + copula;
- Wiktionary's ideophone "nouns" (딱, 점점, 깜짝) are adverbs;
- definitional Wiktionary glosses were replaced (그래서, 열심히, 마을 and
  about 50 more).

Fresh samples after the round (`packbuilder sample --seed 71 --words 90`,
`--seed 72 --sentences 90`): 90/90 words correct and 90/90 sentences with
every link right.

**Round 3 (v1.2, 2026-09-25).** Target-language text uses the pack font
stack "Apple SD Gothic Neo", "Noto Sans KR", "Malgun Gothic" (Noto Sans KR is
loaded from Google Fonts). Bare 거 always links 것, never 거기, and an alt is
refused when a longer headword or name covers the eojeol (대만은, 대대로 are not
대 + particles). 한 after a modifier is the noun "as long as" (가능한 한), a
fused 안 + verb follows the translation (안들어요 "don't cut" is 들다), and 다시는
is 다시 + 는. 졸리다, 이빨, 베개, 습관 and 여우 replaced five low-rank NIKL 고급
abstractions (공공, 경향, 관점, 이래, 수면). Seed-92 sample: 90/90 sentences
with every link right.

## Reading passages (Read tab)

`pack/passages.json` holds 60 short reading texts, 20 each at A1, A2 and B1,
with comprehension questions each. The format is in the engine's
`docs/PACK_SCHEMA.md`. The texts were written for this pack (`"src": "gen"`)
and their source is `tools/passages_src.json`. Rebuild from that source with:

```
PYTHONPATH=engine/tools python3 -m packbuilder passages --lang ko .    # --check: report only
python3 engine/tools/jsonify_pack.py pack                              # passages go into sentences.js
```

The builder links word ids the same way it does for the example sentences.
It enforces in-pack coverage of at least 95% at A1 and A2, and at least 93%
at B1. It also enforces a level budget: an A1 passage may use at most 3 A2
words (and no B1 words) and an A2 passage at most 3 B1 words. Per-passage
numbers and the QA notes are in `tools/REPORT_passages.md`.

A level's 20 passages unlock once the learner has learned 70% of that
level's words. Tapping any word in a passage shows its gloss, including
inflected forms, via per-sentence token spans linked to word ids.
Comprehension questions feed missed words back into the review queue as
weak words.

The passages and questions are machine-written by Claude, checked by an
automated QA pass and re-checked by hand; they have not had a native-speaker
review. 15 words (`tools/gloss_display.json`) carry a display-only gloss
addition, applied at build, so their English also shows the sense or form a
passage uses (e.g. 안 "not; inside", 나다 "to come out... (화가 나다: to get
angry)").

## Layout

```
pack/
  pack.json         trainer config (levels, placement test, function words)
  words.json        2000 word entries
  sentences.json    example sentences, each tagged with the word ids it covers
  attribution.json  per-source licence + contributor attribution
  pack.js words.js sentences.js   generated by engine/tools/jsonify_pack.py (committed, never hand-edited)
engine/             git submodule -> vocab-engine (UI, drill logic, build/validate tools,
                    tools/packbuilder = the shared pack builder, langs/ko.py = Korean rules)
tools/
  build_pack.py     shim: runs `python3 -m packbuilder build --lang ko --repo .` from engine/tools
  gloss_overrides.json  hand gloss fixes ("lemma|pos")
  forced_a1.txt     A1 core list, forced into A1 (the closed sets are in langs/ko.py)
  generated_sentences.tsv  sentences written for this pack (word, Korean, English); append new lines at the end
  id_map_v1.json    frozen "lemma|pos" -> word id (keeps learner progress across rebuilds)
  requirements.txt  engine/tools/packbuilder/requirements.txt + the spaCy Korean model
  REPORT.md         generated coverage report from the last build (manual section kept)
build.sh            builds index.html (the self-contained trainer) from pack/ + engine/
check.sh            packbuilder check + engine validator + stale-build guard, all in one
index.html          built trainer
```

## Rebuilding

```
git clone --recurse-submodules <this repo>
# or, if already cloned: git submodule update --init

cd korean
python3 -m venv .venv
source .venv/bin/activate
pip install -r tools/requirements.txt

python3 tools/build_pack.py          # rebuild pack/{pack,words,sentences,attribution}.json + tools/REPORT.md
python3 engine/tools/jsonify_pack.py pack   # regenerate pack/*.js from the .json
./build.sh                           # build index.html
./check.sh                           # pack checks + engine validation + stale-build guard
```

The `engine` submodule is pinned to a vocab-engine commit that already
contains `tools/packbuilder/langs/ko.py`, so `build.sh` and `check.sh` work
against `engine/tools` with no `PACKBUILDER_PATH` override needed.

Sources are downloaded once into `.cache/`, which is gitignored. The build is
deterministic, so re-running from cache reproduces byte-identical
`pack/*.json`. spaCy tags the 12,000 Tatoeba sentences that have an English
translation once, in under a minute on a laptop CPU. The result is cached
under `.cache/derived/`. Editing `tools/generated_sentences.tsv` changes the
corpus, so the next build tags again. QA helpers run with
`PYTHONPATH=engine/tools python3 -m packbuilder {check,scan,sample} --lang ko --repo .`.

## Korean rules

The builder's Korean module handles what the shared pipeline cannot guess.

- **An eojeol is analysed, not looked up.** spaCy splits text into spaced
  words (eojeol) and gives each a tag class. The Korean module's analyser
  then reads each eojeol as one of these:
  - a noun plus a particle chain (친구한테는 is 친구 + -한테 + -은/는);
  - a conjugated verb or adjective. Endings are generated for regular stems.
    Irregular stems (듣다 들어, 춥다 추워, 알다 아세요) come from Wiktionary's
    conjugation tables;
  - a predicative noun + 하다 or 되다;
  - a noun + the copula -이다;
  - a closed-set word.

  The tagger's class is a hint, never a verdict. Particles and the copula are
  linked but do not count toward sentence length.
- **Particles are words.** The core particles (-이/가, -은/는, -을/를, -에,
  -에서, -(으)로, -와/과, -도, -만, -까지, -부터, -한테, -에게, -처럼, -보다,
  -마다, -밖에, -씩 and others) are A1 entries named by both allomorphs. A
  particle written apart from its noun (게임 이나) is still the particle.
- **Numbers come in two series.** Sino-Korean 일 to 십, 백, 천 and 만, and
  native 하나 to 열 and 스물, are separate words. A numeral spelling is the
  number only before a counter of its own series, or when the tagger reads
  it as a numeral. So 삼 년 and 두 명 are numbers, while 이 사람 ("this
  person") and 네가 한 일 ("what you did") are not. 네 before a noun is "your"
  unless the translation says "four". 이 before a Sino-Korean counter is "two"
  only when the translation counts that unit.
- **Homographs follow the translation.** Nouns with two common meanings (눈
  "eye" or "snow", 배 "belly" or "ship", 다리 "leg" or "bridge") and verbs
  that share a spelling (살: 사다 or 살다; 들어: 듣다 or 들다; 걸어: 걷다 or
  걸다; 팔: 팔다 or 파다) link the reading whose English appears in the
  sentence's translation. A meaning that takes about 10% or less of a
  spelling's uses (말 "horse", 차 "tea", 밤 "chestnut") is not a separate word, and its
  spelling links nothing.
- **Spoken forms count toward their standard word.** 이거, 그거 and 저거 link
  이것, 그것 and 저것. 거야 and 꺼야 link 것 + -이다. 맘 links 마음 and 뭔지
  links 뭐.
- **Grammar patterns link nothing.** Patterns whose words are grammar, not
  vocabulary, link no word for those parts: -고 나서 "after", -ㄹ지도 모르다
  "might" and -에도 불구하고 "despite".
- **Honorific and humble verbs are taught through the plain verb.** 드리다
  links 주다 and 드시다 links 먹다. The shared lexicon drops Wiktionary senses
  marked form-of, and that removes 드리다's own sense. 계시다 keeps its own
  entry.
- **Font.** `pack.json` sets `fontFamily` to "Apple SD Gothic Neo", "Noto
  Sans KR", "Malgun Gothic", sans-serif and `fonts` to Noto Sans KR, which the
  engine applies to every element marked `lang="ko"`.
- **Typing is on** (`typing: {caseSensitive: false, accents: lenient,
  strictFromLevel: null}`). "Type the word" drills the written Hangul form.
  Case does not apply to Hangul, and there are no combining marks for
  lenient accents to fold, so lenient is a no-op and stays on at every
  level (`strictFromLevel: null`).

## Sources and licences

| Data | Source | Licence | Used for |
|---|---|---|---|
| Spoken/subtitle frequency | [hermitdave/FrequencyWords](https://github.com/hermitdave/FrequencyWords) (`ko_full.txt`, 2018 OpenSubtitles) | CC-BY-SA 4.0 | word ranking |
| Written/general frequency | [`wordfreq`](https://github.com/rspeer/wordfreq) Python package (Korean, mecab morphemes) | CC-BY-SA 4.0 | word ranking |
| Glosses, part of speech, conjugation tables | [kaikki.org](https://kaikki.org) Korean extract of English Wiktionary | CC-BY-SA 3.0 / GFDL (Wiktionary) | English glosses, POS, inflection map |
| POS tagging (build time only) | [spaCy](https://spacy.io) (MIT) with `ko_core_news_sm` 3.8.0, trained on UD Korean Kaist | CC BY-SA 4.0 (model) | tag class per eojeol, as a hint to the analyser. The pack ships no model files. |
| Level floors (build time only) | NIKL 한국어 학습용 어휘 목록 (2003) grades, via [julienshim/combined_korean_vocabulary_list](https://github.com/julienshim/combined_korean_vocabulary_list) | KOGL Type 1 (list); MIT (mirror code) | a word graded 중급 is never A1, and one graded 고급 is never below B1. Never shipped. |
| Example sentences | [Tatoeba](https://tatoeba.org) `kor_sentences_detailed.tsv` | CC-BY 2.0 FR | sentence text (contributor usernames in `pack/attribution.json`) |
| Sentence translations | Tatoeba `eng_sentences.tsv` + `kor-eng_links.tsv` | CC-BY 2.0 FR | English translations |
| Generated sentences | written for this pack, `tools/generated_sentences.tsv` | CC-BY-SA 4.0 | sentences for words Tatoeba covers with fewer than 2 usable sentences, marked `"src": "gen"` |

Licence: code MIT, pack data CC BY-SA 4.0, see LICENSE.

Tatoeba has no permissively licensed Korean audio, so the pack links none
and relies on TTS. No licence is non-commercial.

## Level bands

Candidate (lemma, POS) pairs are ranked by a blended frequency score: the
weighted mean of log subtitle rank, log `wordfreq` rank and log rank in the
tagged Tatoeba corpus. `wordfreq` counts Korean as mecab morphemes, so the
module maps each word to its morpheme spelling. A compound verb with no
morpheme of its own, such as 끝나다, is estimated from its subtitle count.

- **A1** (600 words): every forced item, then the highest-ranked remaining
  words. Forced items are:
  - both number series;
  - days, months and colours;
  - pronouns, demonstratives and question words;
  - particles and the copula;
  - time words;
  - greetings and set phrases (안녕하세요, 감사합니다, 만나서 반갑습니다);
  - the A1 core list in `tools/forced_a1.txt`.
- **A2**: the next 700 by rank.
- **B1**: the next 700 by rank.

A NIKL grade floor can push a word up a band. A NIKL grade ceiling keeps a
word graded only 초급 at A2 or below, and the colour adjectives (빨갛다,
파랗다, 노랗다, 하얗다, 까맣다), 남동생 and 여동생 are capped at A2 by hand. A
capped word moves into the A2 band, and the lowest-ranked uncapped A2 words
move to B1. In the QA snapshot 24 초급-only words sat at B1; none do now. A word no example sentence can
illustrate gives its place to the next word by rank. This is a simple,
reproducible proxy for CEFR level. It is not an official CEFR or TOPIK
classification.

To take an engine update, run `git submodule update --remote engine`, then
rebuild with `./build.sh`.
