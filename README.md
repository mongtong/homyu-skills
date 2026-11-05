# hy-skills

Agent skills for building music software, written while developing **HY Humanizer** (an audio-plugin that turns one recorded take into several realistic re-performances).

Skills work with Claude Code, Claude.ai, Cursor, Codex and any agent that reads the [agent-skills](https://skills.sh) format.

## Skills

| Skill | What it does |
|---|---|
| [`daw-plugin-ui`](skills/daw-plugin-ui/SKILL.md) | Conventions for audio-plugin and music-tool UI: knob/fader behaviour musicians expect (drag, Shift fine-tune, double-click reset), meters with peak hold and clip latch, loudness-matched A/B, seed / randomize / favourites controls, plugin-window layout, dark visual language, preset and state rules, a 10-point pre-ship checklist, plus JUCE and web-prototype implementation notes. |

## Install

```bash
npx skills add mongtong/hy-skills --skill daw-plugin-ui
```

Or copy `skills/daw-plugin-ui/SKILL.md` into your agent's skills folder.

## 中文說明

這是在開發 **HY Humanizer**（把一軌錄音變成多次真實重錄的外掛）過程中整理出來的 agent skill。

`daw-plugin-ui` 把音訊外掛 UI 的慣例固定下來：旋鈕與推桿的操作方式、電表與 peak hold、A/B 要音量對齊、Seed 與隨機控制怎麼呈現、外掛視窗排版、深色視覺語言、預設值與狀態、出貨前檢查清單，以及 JUCE 和網頁原型各自的實作要點。

歡迎 PR。

## Author

Hom Yu

## License

MIT
