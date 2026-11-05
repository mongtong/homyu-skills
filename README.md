# homyu-skills

**繁體中文** · [English](#english)

Hom Yu 在開發 **HY Humanizer**（把一軌錄音變成多次真實重錄的音訊外掛）過程中整理出來的 agent skill。

Skill 是一份給 AI 代理看的規範：安裝之後，Claude Code、Claude.ai、Cursor、Codex 等支援 [agent-skills](https://skills.sh) 格式的工具，在做相關任務時會自動載入並照著做，不用每次重講。

## Skills

| Skill | 做什麼 |
|---|---|
| [`daw-plugin-ui`](skills/daw-plugin-ui/SKILL.md)（[繁體中文對照](skills/daw-plugin-ui/SKILL.zh-TW.md)） | 音訊外掛與音樂工具的 UI 規範：音樂人習慣的旋鈕與推桿操作（拖曳、Shift 微調、雙擊歸零）、帶 peak hold 與 clip 鎖定的電表、音量對齊的 A/B、Seed／隨機／最愛的呈現方式、外掛視窗排版、深色視覺語言、預設值與狀態規則、出貨前 10 點檢查，以及 JUCE 與網頁原型各自的實作要點。 |

## 安裝

```bash
npx skills add mongtong/homyu-skills --skill daw-plugin-ui
```

或直接把 `skills/daw-plugin-ui/SKILL.md` 複製到你的 agent 的 skills 資料夾。

## 什麼時候會觸發

設計、檢查或撰寫外掛編輯器、mixer、參數面板、音訊測試頁的時候。描述裡包含「audio plugin UI、DAW plugin、knob、fader、meter、外掛介面、旋鈕、推桿、電表」等關鍵字就會被選到。

## 作者

Hom Yu

## 授權

MIT

---

## English

Agent skills for building music software, written by **Hom Yu** while developing **HY Humanizer** (an audio plugin that turns one recorded take into several realistic re-performances).

Skills work with Claude Code, Claude.ai, Cursor, Codex and any agent that reads the [agent-skills](https://skills.sh) format: once installed, the agent loads the skill automatically for matching tasks.

| Skill | What it does |
|---|---|
| [`daw-plugin-ui`](skills/daw-plugin-ui/SKILL.md) ([Traditional Chinese](skills/daw-plugin-ui/SKILL.zh-TW.md)) | Conventions for audio-plugin and music-tool UI: knob/fader behaviour musicians expect (drag, Shift fine-tune, double-click reset), meters with peak hold and clip latch, loudness-matched A/B, seed / randomize / favourites controls, plugin-window layout, dark visual language, preset and state rules, a 10-point pre-ship checklist, plus JUCE and web-prototype implementation notes. |

```bash
npx skills add mongtong/homyu-skills --skill daw-plugin-ui
```

Author: Hom Yu · License: MIT
