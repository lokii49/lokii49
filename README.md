### Hi, I'm Lokesh 👋

I'm a software engineer in Hyderabad (5 years of experience, 3+ on its native iOS SDK). I've shipped three apps of my own on the App Store.
Most of my recent work is about measuring whether model output is true. I write the rubric before the run, keep held-out cases, and only ship a change if it passes the gate.

---

#### 🧪 LLM evaluation: [`mirror/tools/llmrig`](https://github.com/lokii49/mirror/tree/main/tools/llmrig)

This test rig sits behind the on-device AI in [MirrorNotes](https://mirrornotes.org), a journaling app with no server.

- **Exact copy of the app's model path, run off the device.** It uses the same Gemma 3 1B GGUF and the same patched llama.cpp, and checks the chat template token by token. Prompts are dumped byte-for-byte from the app's own test harness. This turns one generation per ~25s on the simulator into N≥10 runs per variant on Metal.
- **Rubric written before any run.** An output passes only if it invents nothing, swaps no one's actions, doesn't state plans as done, and gets the format right. All cases are synthetic; no real journal text is ever used.
- **Root cause, then fix.** With the production prompt, ~0/40 outputs were faithful: the model invented "The rain outside…" openers. Prompting alone couldn't fix it. **GBNF grammar-constrained decoding** that forces a verbatim quote, plus a sentence-level guard, could.
- **12 numbered rounds for Apple Foundation Models**, each with a ship rule fixed in advance: pass ≥ 95%, no case below 8/10, refusals ≤ 5%. Shipped at **98% pass**. Fresh held-out cases later raised the fallback rate from 3.6% to 19%, and I wrote that up rather than tuning against the test set. Variants that failed the gate (rounds 10 and 12) are recorded and didn't ship.
- **Retrieval benchmark** ([`retrieval/`](https://github.com/lokii49/mirror/tree/main/tools/llmrig/retrieval)). Compares keyword search, Apple `NLContextualEmbedding` and EmbeddingGemma v1/v2 (Q4/Q8). Measures R@10 on lexical, paraphrase and abstract questions in 5 languages, with trap entries mixed in. Models are compared question by question, since single cells vary by ±0.2.

#### ✅ Test engineering in shipped apps

- **[sharedTimer](https://github.com/lokii49/sharedTimer)**: one shared model compiled into 7 targets (app, App Clip, iMessage, widgets/Live Activity, watchOS).
  - 135 Swift Testing unit tests, plus 27 tests against real AlarmKit on a device, which sweep up stray alarms afterwards.
  - UI tests for the shared-sequence import → pause → extend → resume → delete flow.
  - GitHub Actions CI; the flaky Spotlight-index test now waits at most 30 seconds instead of trying a fixed number of times.
  - `fastlane release` handles the whole release.
- **MirrorNotes**: 750+ Swift Testing tests, plus opt-in end-to-end tests against the real Foundation Models, enabled with an environment flag.
- **TinyAct** (iOS + Android + Firebase): **shared JSON test vectors** prove the streak, XP and award logic gives the same results in Swift, Kotlin and TypeScript.

#### 🛠️ Stack

`Swift` `SwiftUI` `UIKit` `Swift Testing / XCTest` `llama.cpp` `Apple Foundation Models` `GBNF` `Python` `TypeScript` `Kotlin` `Firebase` `CloudKit` `GitHub Actions` `Fastlane` `MCP`

#### 📫 Reach me

pudari.lokesh@icloud.com · [mirrornotes.org](https://mirrornotes.org) · [tinyact.app](https://tinyact.app)
