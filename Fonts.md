---
lang: en
title: Font Stacks
author: nobus-1967
description: Font Stacks & CSS rules.
keywords: font, css, cjk
published: 2026-08-16
---

# Font Stacks Documentation

This document provides a summary architectural table of font fallbacks used to ensure ideal cross-platform rendering.
Fonts are grouped by operating system type, commercial vendors, and open licenses for all key `CJK` locales (China, Japan, Korea).

## Font Mapping Summary Table

| General Style | Region / Locale | Google Fonts (Noto) | Adobe                        | Apple (macOS, iOS)               | Microsoft Windows                    | Linux / OFL                       |
|:--------------:|:---------------:| ------------------- | ---------------------------- | -------------------------------- | ------------------------------------ | --------------------------------- |
| **Serif/Ming** | Global          | Noto Serif          | Source Serif                 | New York, Times New Roman, Times | Times New Roman                      | Liberation Serif, FreeSerif       |
|                | CJK JP (`ja`)   | Noto Serif CJK JP   | Source Han Serif JP          | Hiragino Mincho ProN/Pro         | MS PMincho, MS Mincho                | IPAexMincho, IPAMincho            |
|                | CJK SC (`zh-CN`)| Noto Serif CJK SC   | Source Han Serif SC/CN       | Songti SC, STSong                | SimSun                               | FandolSong, WenQuanYi Bitmap      |
|                | CJK TC (`zh-TW`)| Noto Serif CJK TC   | Source Han Serif TC/TW       | Apple LiSung, LiSong Pro         | PMingLiU, MingLiU                    | HanaMinA (花園明朝)                   |
|                | CJK HK (`zh-HK`)| Noto Serif CJK HK   | Source Han Serif HK          | Apple LiSung, LiSong Pro         | MingLiU_HKSCS, PMingLiU              | HanaMinA (花園明朝)                   |
|                | CJK KR (`ko`)   | Noto Serif CJK KR   | Source Han Serif KR          | AppleMyungjo                     | Batang                               | UnBatang (은바탕)                    |
| **Sans-Serif/Gothic** | Global | Noto Sans          | Source Sans                  | SF Pro, Helvetica, Arial         | Segoe UI, Arial                      | Liberation Sans, Arima, Open Sans |
|                | CJK JP (`ja`)   | Noto Sans CJK JP    | Source Han Sans JP           | Hiragino Kaku Gothic ProN        | Meiryo, Yu Gothic                    | IPAexGothic, IPAGothic            |
|                | CJK SC (`zh-CN`)| Noto Sans CJK SC    | Source Han Sans SC/CN        | PingFang SC, STHeiti             | Microsoft YaHei                      | FandolHei, WenQuanYi Zen Hei      |
|                | CJK TC (`zh-TW`)| Noto Sans CJK TC    | Source Han Sans TC/TW        | PingFang TC, LiHei Pro           | Microsoft JhengHei                   | Uming (文鼎自由字型)                    |
|                | CJK HK (`zh-HK`)| Noto Sans CJK HK    | Source Han Sans HK           | PingFang HK                      | Microsoft JhengHei UI                | Uming (文鼎自由字型)                    |
|                | CJK KR (`ko`)   | Noto Sans CJK KR    | Source Han Sans KR           | Apple SD Gothic Neo              | Malgun Gothic                        | UnDotum (은돋움)                     |
| **Monospace/Mono** | Global       | Noto Sans Mono       | Source Code Pro              | SF Mono, Courier New, Courier    | Cascadia Code, Courier New, Consolas | Liberation Mono, Fira Code        |
|                | CJK JP (`ja`)   | Noto Sans Mono CJK JP | Source Han Mono JP         | Osaka-Mono, Hiragino Mono        | MS Gothic, Yu Gothic UI              | IPAGothic (Mono)                  |
|                | CJK SC (`zh-CN`)| Noto Sans Mono CJK SC | Source Han Mono SC/CN     | PingFang SC (Mono)               | NSimSun, SimHei                      | FandolMono, Sazanami Mono         |
|                | CJK TC (`zh-TW`)| Noto Sans Mono CJK TC | Source Han Mono TC/TW     | PingFang TC (Mono)               | MingLiU                              | HanaMinA (Mono)                   |
|                | CJK HK (`zh-HK`)| Noto Sans Mono CJK HK | Source Han Mono HK       | PingFang HK (Mono)               | MingLiU_HKSCS                        | HanaMinA (Mono)                   |
|                | CJK KR (`ko`)   | Noto Sans Mono CJK KR | Source Han Mono KR       | Apple SD Gothic Neo (Mono)       | GulimChe, DotumChe                   | UnDotum (Mono)                    |
| **Emoji**      | Global          | Noto Color Emoji      | EmojiOne Color, Source Emoji | Apple Color Emoji                | Segoe UI Emoji, Segoe UI Symbol      | Symbola                           |

### Architectural Principles of Building Stacks

1. **Latin → Regional Hierarchy**: Global Latin fonts are always listed first in the CSS rule (`font-family: Global, CJK, ...`). Since Latin fonts do not contain Asian ideographs, the browser seamlessly hands off the rendering of CJK glyphs to the regional fonts next in the chain.
2. **Style Group Purity**: The **Serif/Ming** styles include exclusively print-style serif fonts, preventing an accidental fallback to the calligraphic (handwritten) Kaiti style on older Windows versions.
3. **Proportional vs. Monospaced Split (Windows)**:
   * Proportional-width fonts (containing the letter **"P"**, e.g. `PMingLiU`, `MS PMincho`) are chosen for body text.
   * Strictly monospaced versions without the letter "P", or fonts ending in **"Che"** (`GulimChe`), are used for code.

## Optimized CSS Rules

* The `@import` directive is used to load **Google Fonts** automatically right inside your CSS file (without the need to edit the page's HTML). Add this block at the very beginning of the main CSS file.
* **Important**: per the CSS specification, `@import` directives must be placed *strictly before any other style rules* (including variables and selectors).

```css
/* ==========================================================================
   NOTO FONTS FROM GOOGLE FONTS
   ========================================================================== */

/* 1. Global Latin Noto fonts (Serif, Sans, Mono) */
@import url('https://googleapis.com');

/* 2. Asian Noto Serif CJK packages (Japan, China, Taiwan, Hong Kong, Korea) */
@import url('https://googleapis.com');

/* 3. Asian Noto Sans CJK packages (Japan, China, Taiwan, Hong Kong, Korea) */
@import url('https://googleapis.com');

/* 4. Asian monospaced Noto Sans Mono CJK packages */
@import url('https://googleapis.com');


/* ==========================================================================
   YOUR OPTIMIZED CSS RULES GO BELOW...
   ========================================================================== */
```

Optimized CSS rules:

```css
/* ==========================================================================
   1. SERIF/MING GROUP (Print serif fonts)
   ========================================================================== */

/* Global */
.font-serif {
  font-family: "Noto Serif", "Source Serif", "New York", "Times New Roman", Times, "Liberation Serif", "FreeSerif", -apple-system-serif, serif;
}
/* CJK JP (ja) */
.font-serif [lang="ja"] {
  font-family: "Noto Serif CJK JP", "Source Han Serif JP", "Hiragino Mincho ProN", "Hiragino Mincho Pro", "MS PMincho", "MS Mincho", "IPAexMincho", "IPAMincho", serif;
}
/* CJK SC (zh-CN) */
.font-serif [lang="zh-CN"], .font-serif [lang="zh-Hans"] {
  font-family: "Noto Serif CJK SC", "Source Han Serif SC", "Source Han Serif CN", "Songti SC", "STSong", "SimSun", "FandolSong", "WenQuanYi Bitmap", serif;
}
/* CJK TC (zh-TW) */
.font-serif [lang="zh-TW"], .font-serif [lang="zh-Hant"] {
  font-family: "Noto Serif CJK TC", "Source Han Serif TC", "Source Han Serif TW", "Apple LiSung", "LiSong Pro", "PMingLiU", "MingLiU", "HanaMinA", serif;
}
/* CJK HK (zh-HK) */
.font-serif [lang="zh-HK"] {
  font-family: "Noto Serif CJK HK", "Source Han Serif HK", "Apple LiSung", "LiSong Pro", "MingLiU_HKSCS", "PMingLiU", "MingLiU", "HanaMinA", serif;
}
/* CJK KR (ko) */
.font-serif [lang="ko"] {
  font-family: "Noto Serif CJK KR", "Source Han Serif KR", "AppleMyungjo", "Batang", "UnBatang", serif;
}

/* ==========================================================================
   2. SANS-SERIF/GOTHIC GROUP (Sans-serif fonts)
   ========================================================================== */

/* Global */
.font-sans {
  font-family: "Noto Sans", "Source Sans", "SF Pro", "Segoe UI", "Open Sans", "Helvetica", Arial, "Liberation Sans", "Arima", sans-serif;
}
/* CJK JP (ja) */
.font-sans [lang="ja"] {
  font-family: "Noto Sans CJK JP", "Source Han Sans JP", "Hiragino Kaku Gothic ProN", "Meiryo", "Yu Gothic", "IPAexGothic", "IPAGothic", sans-serif;
}
/* CJK SC (zh-CN) */
.font-sans [lang="zh-CN"], .font-sans [lang="zh-Hans"] {
  font-family: "Noto Sans CJK SC", "Source Han Sans SC", "Source Han Sans CN", "PingFang SC", "STHeiti", "Microsoft YaHei", "FandolHei", "WenQuanYi Zen Hei", sans-serif;
}
/* CJK TC (zh-TW) */
.font-sans [lang="zh-TW"], .font-sans [lang="zh-Hant"] {
  font-family: "Noto Sans CJK TC", "Source Han Sans TC", "Source Han Sans TW", "PingFang TC", "LiHei Pro", "Microsoft JhengHei", "Uming", sans-serif;
}
/* CJK HK (zh-HK) */
.font-sans [lang="zh-HK"] {
  font-family: "Noto Sans CJK HK", "Source Han Sans HK", "PingFang HK", "Microsoft JhengHei UI", "Uming", sans-serif;
}
/* CJK KR (ko) */
.font-sans [lang="ko"] {
  font-family: "Noto Sans CJK KR", "Source Han Sans KR", "Apple SD Gothic Neo", "Malgun Gothic", "UnDotum", sans-serif;
}

/* ==========================================================================
   3. MONOSPACE/MONO GROUP (Monospaced fonts for code)
   ========================================================================== */

/* Global */
.font-mono {
  font-family: "Noto Sans Mono", "Source Code Pro", "SF Mono", "Cascadia Code", "Fira Code", "Courier New", Courier, "Consolas", "Liberation Mono", monospace;
}
/* CJK JP (ja) */
.font-mono [lang="ja"] {
  font-family: "Noto Sans Mono CJK JP", "Source Han Mono JP", "Osaka-Mono", "Hiragino Mono", "MS Gothic", "Yu Gothic UI", "IPAGothic", monospace;
}
/* CJK SC (zh-CN) */
.font-mono [lang="zh-CN"], .font-mono [lang="zh-Hans"] {
  font-family: "Noto Sans Mono CJK SC", "Source Han Mono SC", "Source Han Mono CN", "PingFang SC", "NSimSun", "SimHei", "FandolMono", "Sazanami Mono", monospace;
}
/* CJK TC (zh-TW) */
.font-mono [lang="zh-TW"], .font-mono [lang="zh-Hant"] {
  font-family: "Noto Sans Mono CJK TC", "Source Han Mono TC", "Source Han Mono TW", "PingFang TC", "MingLiU", "HanaMinA", monospace;
}
/* CJK HK (zh-HK) */
.font-mono [lang="zh-HK"] {
  font-family: "Noto Sans Mono CJK HK", "Source Han Mono HK", "PingFang HK", "MingLiU_HKSCS", "HanaMinA", monospace;
}
/* CJK KR (ko) */
.font-mono [lang="ko"] {
  font-family: "Noto Sans Mono CJK KR", "Source Han Mono KR", "Apple SD Gothic Neo", "GulimChe", "DotumChe", "UnDotum", monospace;
}

/* ==========================================================================
   4. EMOJI GROUP (Standalone emojis and pictograms)
   ========================================================================== */

.emoji {
  font-family: "Noto Color Emoji", "Apple Color Emoji", "Segoe UI Emoji", "EmojiOne Color", "Source Emoji", "Segoe UI Symbol", "Symbola", sans-serif;
  font-style: normal;
}
```

## Principles for Creating CSS Rules

* **Noto** family fonts (both global and regional `CJK`) are moved to the front of every CSS rule (the browser will be forced to look up and use **Google**'s fonts).
* The browser will fall back to system alternatives from **Apple** (**macOS**) or **Microsoft** (**Windows**) only if the **Noto** web fonts are not loaded via the `<link>` tag, did not download, or are not installed locally.

The table has been turned into clean CSS rules. They automatically pick up the text language via the standard `[lang="..."]` attribute.

Within each rule, fonts are ordered by the following priority:

1. Exact **localized web font names** (e.g. `"Noto Serif CJK JP"`).
2. Region-specific **Apple** system fonts.
3. Region-specific **MS Windows** system fonts (proportional first, then monospaced).
4. Free **Linux** / **OFL** fonts.
5. Generic family (`serif`, `sans-serif`, `monospace`).

[Created for markdown2html5-base, markdown2pdf-base]: #