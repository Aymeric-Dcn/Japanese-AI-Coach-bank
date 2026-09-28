# Japanese AI Coach — exercise bank

*[Version française](README.fr.md)*

**Reviewed Japanese exercises built from real sentences**: particles, conjugations and JLPT-style questions (漢字読み, 表記, 文脈規定, 文法形式, 並べ替え ★), each with its sentence, answer, translations in French and English, and explanation. This is the shared bank of [Japanese AI Coach](https://github.com/Aymeric-Dcn/Japanese-AI-Coach): every copy of the app downloads it at startup, so the app works without a local LLM.

Current content: see **[STATS.md](STATS.md)** (regenerated at every publication).

## Why a separate bank

A local model can generate exercises, but it cannot be trusted to judge them: in our tests Qwen 3 14B accepted every JLPT question it was asked to check, including ones where two answers were right. So an exercise only enters this bank after three filters:

1. **Built by rules, not by the model.** Sentences come from [Tatoeba](https://tatoeba.org); the blank, the answer and the readings come from a morphological analyzer. Wrong choices are built like the real test (another reading of the kanji, long ↔ short vowel, voicing, same-sound kanji that do not form a word).
2. **Ambiguity rules.** Particle pairs that are often both right (は/が, に/へ, と/や…) are never offered together, は/が exercises are kept only where the grammar decides, 並べ替え keeps only pieces whose order is fixed, conjugations that would also fit are left out…
3. **Human-level review.** Batches are reviewed by Claude (Anthropic); rejected exercises are listed in `rejected.jsonl` with the reason, and apps retire them too.

Reports from users (« ⚑ Report a mistake » in the app) feed the next review.

## Layout

```
manifest.json            version, date, count, one entry per file (path, sha256, count, topic)
STATS.md                 what the bank holds, per topic
exercises/<topic>.jsonl  one reviewed exercise per line
rejected.jsonl           keys removed after review, with the reason
inbox/                   contributions waiting for review (unreviewed)
```

One line of `exercises/*.jsonl`:

```json
{"key": "tatoeba:234114:particle:が", "topic": "Particules は / が", "kind": "particle",
 "data": {"sentence": "あなた___学校に遅れた理由を言いなさい。", "answers": ["が"], "allowed_answers": ["は", "が"],
          "full_sentence": "あなたが学校に遅れた理由を言いなさい。", "reading": "…",
          "translation": "Dis-moi la raison pour laquelle tu étais en retard à l'école.", "translation_en": "…",
          "explanation": "…", "explanation_en": "…", "hint": "…", "hint_en": "…",
          "source": "Tatoeba #234114", "source_url": "https://tatoeba.org/fr/sentences/show/234114",
          "review": {"by": "Claude", "date": "2026-09-28"}}}
```

JLPT questions also have `question` (the word tested in 【】, or （　　） for the blank), `choices` and `answer_index`. `key` is stable: the same exercise is never added twice.

## Using it

In the app, nothing to do: the bank is downloaded at startup (Progress → Shared bank → « Sync now » to force it). From the command line: `python bank_sync.py pull`. Your answers, progress and Anki data are never sent anywhere.

## Contributing

Apps with a local model generate exercises; « Send my exercises for review » puts them in `inbox/` (with a GitHub token allowed to write to this repository), or `python bank_sync.py contribute --dest <folder>` writes a file you can send. The maintainer imports the inbox, reviews it, and publishes what passes.

## Sources and licence

The bank is published under **[CC BY-SA 4.0](LICENSE.md)**. It is built from:

- **Tatoeba** sentences and translations — [CC BY 2.0 FR](https://creativecommons.org/licenses/by/2.0/fr/); each exercise links to its sentence and its authors on tatoeba.org (`source_url`).
- **JLPT word levels** from [open-anki-jlpt-decks](https://github.com/jamsinclair/open-anki-jlpt-decks) (MIT), based on Jonathan Waller's lists on [tanos.co.uk](http://www.tanos.co.uk/jlpt/) (CC BY).
- **Kanji readings and short meanings** from the [Full Japanese Study Deck](https://github.com/Ronokof/Full-Japanese-Study-Deck) (CC BY-SA 4.0). Its non-commercial parts (jpdb.io data, audio) are not used.
- **Readings and word boundaries** from [SudachiPy](https://github.com/WorksApplications/SudachiPy) and its dictionary (Apache 2.0).
- Explanations written by Qwen 3 (Apache 2.0) and Claude, reviewed.
