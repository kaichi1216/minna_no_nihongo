# みんなの日本語 初級1 單字與句型視覺化 Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single interactive HTML file that organizes vocabulary and grammar patterns from Minna no Nihongo Elementary 1 (Lessons 1-6) with a textbook-style visual design.

**Architecture:** Single self-contained HTML file with inlined CSS and JavaScript. Sidebar navigation on the left, main content scrolling on the right. All interactivity (translation toggle, search, speech synthesis, scroll tracking) implemented in vanilla JS.

**Tech Stack:** Pure HTML5, CSS3, vanilla JavaScript, Web Speech API

**Spec:** `docs/superpowers/specs/2026-03-13-minna-no-nihongo-vocab-grammar-design.md`

**Output file:** `/Users/kai/Desktop/minna-no-nihongo/index.html`

---

## Chunk 1: HTML/CSS Foundation + Lesson 1

### Task 1: Create HTML skeleton with all CSS

**Files:**
- Create: `/Users/kai/Desktop/minna-no-nihongo/index.html`

This task creates the complete HTML structure and all CSS styles. No lesson content yet — just the skeleton with sidebar, header, and placeholder main area.

- [ ] **Step 1: Write the base HTML file**

Create `index.html` with:
- `<!DOCTYPE html>` with `lang="ja"`
- `<meta charset="UTF-8">`, viewport meta
- `<title>みんなの日本語 初級I — 単語と文型</title>`
- Complete `<style>` block (all CSS below)
- Sidebar structure with 6 lesson links, search box, translation toggle buttons
- Empty `<main>` with `id="main-content"`

CSS must include:

```css
/* === Base === */
* { margin: 0; padding: 0; box-sizing: border-box; }
body {
  font-family: "Hiragino Sans", "Noto Sans JP", "Yu Gothic", sans-serif;
  background: #fdf8f0;
  color: #333;
  display: flex;
  min-height: 100vh;
}

/* === Sidebar === */
.sidebar {
  width: 200px;
  background: #f5ede0;
  border-right: 1px solid #e0d5c5;
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  z-index: 10;
}
.sidebar-header {
  padding: 20px 16px;
  font-size: 15px;
  font-weight: bold;
  color: #c0392b;
  border-bottom: 1px solid #e0d5c5;
}
.sidebar-nav { padding: 8px 0; }
.sidebar-nav a {
  display: block;
  padding: 10px 16px;
  font-size: 14px;
  color: #555;
  text-decoration: none;
  transition: background 0.2s, color 0.2s;
}
.sidebar-nav a:hover { background: #ece3d4; }
.sidebar-nav a.active {
  background: #c0392b;
  color: white;
  font-weight: bold;
}
.sidebar-search {
  padding: 12px 16px;
  border-top: 1px solid #e0d5c5;
}
.sidebar-search input {
  width: 100%;
  padding: 8px 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 13px;
  background: white;
  outline: none;
}
.sidebar-search input:focus { border-color: #c0392b; }
.sidebar-actions {
  padding: 12px 16px;
  border-top: 1px solid #e0d5c5;
}
.sidebar-actions button {
  display: block;
  width: 100%;
  padding: 8px;
  margin-bottom: 6px;
  font-size: 12px;
  color: #c0392b;
  background: transparent;
  border: 1px solid #c0392b;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.2s;
}
.sidebar-actions button:hover {
  background: #c0392b;
  color: white;
}

/* === Main Content === */
.main-content {
  margin-left: 200px;
  flex: 1;
  padding: 32px 40px;
  max-width: 900px;
}

/* === Lesson Section === */
.lesson {
  margin-bottom: 60px;
  scroll-margin-top: 20px;
}
.lesson-header {
  border-left: 4px solid #c0392b;
  padding-left: 14px;
  margin-bottom: 24px;
}
.lesson-header .lesson-number {
  font-size: 13px;
  color: #c0392b;
  font-weight: bold;
}
.lesson-header .lesson-title {
  font-size: 20px;
  font-weight: bold;
  color: #333;
  margin-top: 2px;
}

/* === Section Titles (単語, 文型) === */
.section-title {
  font-size: 15px;
  font-weight: bold;
  color: #333;
  border-bottom: 2px solid #c0392b;
  padding-bottom: 4px;
  margin-bottom: 14px;
  display: inline-block;
}

/* === Vocabulary Row === */
.vocab-row {
  display: flex;
  align-items: center;
  padding: 8px 0;
  border-bottom: 1px dotted #ddd;
  gap: 12px;
}
.vocab-row .word {
  font-size: 17px;
  min-width: 120px;
  color: #333;
}
.vocab-row .kanji {
  font-size: 13px;
  color: #888;
  min-width: 60px;
}
.vocab-row .pos-tag {
  padding: 2px 8px;
  border-radius: 4px;
  font-size: 11px;
  white-space: nowrap;
}
/* Part-of-speech tag colors */
.pos-noun { background: #fef3e8; color: #8b5e3c; }
.pos-verb1 { background: #e8f0fe; color: #3a6bb5; }
.pos-verb2 { background: #e8f0fe; color: #3a6bb5; }
.pos-verb3 { background: #e8f0fe; color: #3a6bb5; }
.pos-i-adj { background: #e8f4f0; color: #2e7d5e; }
.pos-na-adj { background: #f0e8f4; color: #6b3a9e; }
.pos-adv { background: #fef8e8; color: #8b7a3c; }
.pos-other { background: #f0f0f0; color: #666; }
.pos-greeting { background: #fce8e8; color: #a0522d; }
.pos-counter { background: #e8eef8; color: #4a6a8b; }
.pos-particle { background: #f0f0f0; color: #666; }

.vocab-row .translation {
  margin-left: auto;
  font-size: 14px;
  color: #666;
  cursor: pointer;
  transition: filter 0.2s;
  user-select: none;
}
.vocab-row .translation.hidden {
  filter: blur(5px);
}
.vocab-row .speak-btn {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 16px;
  padding: 2px 4px;
  opacity: 0.6;
  transition: opacity 0.2s;
}
.vocab-row .speak-btn:hover { opacity: 1; }

/* === Grammar Card === */
.grammar-section { margin-top: 28px; }
.grammar-card {
  background: white;
  border: 1px solid #e8ddd0;
  border-radius: 8px;
  padding: 18px;
  margin-bottom: 14px;
}
.grammar-card .pattern {
  font-size: 16px;
  font-weight: bold;
  color: #c0392b;
  margin-bottom: 6px;
}
.grammar-card .pattern-desc {
  font-size: 13px;
  color: #666;
  margin-bottom: 12px;
}
.grammar-card .example {
  background: #fdf8f0;
  padding: 10px 14px;
  border-radius: 6px;
  margin-bottom: 8px;
}
.grammar-card .example .jp {
  font-size: 14px;
  color: #333;
  margin-bottom: 3px;
  display: flex;
  align-items: center;
  gap: 6px;
}
.grammar-card .example .zh {
  font-size: 12px;
  color: #888;
  cursor: pointer;
  user-select: none;
  transition: filter 0.2s;
}
.grammar-card .example .zh.hidden {
  filter: blur(5px);
}
.grammar-card .speak-btn {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 14px;
  padding: 0;
  opacity: 0.6;
  transition: opacity 0.2s;
}
.grammar-card .speak-btn:hover { opacity: 1; }

/* === Variation Table === */
.variation-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 13px;
  margin-top: 10px;
}
.variation-table td {
  padding: 7px 12px;
  border: 1px solid #e8ddd0;
}
.variation-table tr:nth-child(odd) { background: #f5ede0; }
.variation-table .label-cell {
  font-weight: bold;
  width: 70px;
  text-align: center;
}
.variation-table .positive { color: #c0392b; font-weight: bold; }
.variation-table .negative { color: #2e7d5e; font-weight: bold; }
.variation-table .question { color: #4A90D9; font-weight: bold; }

/* === Search highlight === */
mark {
  background: #fff3a8;
  padding: 0 2px;
  border-radius: 2px;
}

/* === Responsive (< 768px) === */
@media (max-width: 768px) {
  .sidebar {
    position: fixed;
    width: 100%;
    height: auto;
    max-height: 50px;
    overflow: hidden;
    transition: max-height 0.3s;
    border-right: none;
    border-bottom: 1px solid #e0d5c5;
  }
  .sidebar.open { max-height: 100vh; overflow-y: auto; }
  .sidebar-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .menu-toggle {
    display: block;
    background: none;
    border: none;
    font-size: 20px;
    cursor: pointer;
    color: #c0392b;
  }
  .main-content {
    margin-left: 0;
    margin-top: 50px;
    padding: 20px 16px;
  }
  .vocab-row .word { min-width: 80px; }
  .vocab-row .kanji { min-width: 40px; }
}
@media (min-width: 769px) {
  .menu-toggle { display: none; }
}
```

HTML sidebar structure:

```html
<aside class="sidebar" id="sidebar">
  <div class="sidebar-header">
    <span>みんなの日本語 初級I</span>
    <button class="menu-toggle" id="menu-toggle">☰</button>
  </div>
  <nav class="sidebar-nav" id="sidebar-nav">
    <a href="#lesson1" class="active">第1課</a>
    <a href="#lesson2">第2課</a>
    <a href="#lesson3">第3課</a>
    <a href="#lesson4">第4課</a>
    <a href="#lesson5">第5課</a>
    <a href="#lesson6">第6課</a>
  </nav>
  <div class="sidebar-search">
    <input type="text" id="search-input" placeholder="🔍 搜尋單字...">
  </div>
  <div class="sidebar-actions">
    <button id="btn-show-all">顯示全部翻譯</button>
    <button id="btn-hide-all">隱藏全部翻譯</button>
  </div>
</aside>
<main class="main-content" id="main-content">
  <!-- Lesson content goes here -->
</main>
```

- [ ] **Step 2: Open in browser and verify**

Open `index.html` in browser. Verify:
- Sidebar renders on the left with correct styling
- Main content area is to the right of sidebar
- Sidebar header shows みんなの日本語 初級I in red
- 6 lesson links appear in sidebar
- Search box and translation toggle buttons appear
- Warm background color (#fdf8f0) renders correctly

---

### Task 2: Add Lesson 1 content

**Files:**
- Modify: `/Users/kai/Desktop/minna-no-nihongo/index.html` (inside `<main>`)

Add Lesson 1 vocabulary and grammar inside the `<main>` element.

- [ ] **Step 1: Add Lesson 1 vocabulary HTML**

Insert inside `<main>`:

```html
<section class="lesson" id="lesson1">
  <div class="lesson-header">
    <div class="lesson-number">だい１か</div>
    <div class="lesson-title">わたしは マイク・ミラーです</div>
  </div>

  <div class="section-title">単語</div>
  <div class="vocab-list">
    <div class="vocab-row"><span class="word">わたし</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">我</span><button class="speak-btn" data-text="わたし">🔊</button></div>
    <div class="vocab-row"><span class="word">あなた</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">你</span><button class="speak-btn" data-text="あなた">🔊</button></div>
    <div class="vocab-row"><span class="word">あのひと</span><span class="kanji">あの人</span><span class="pos-tag pos-noun">名詞</span><span class="translation">那個人</span><button class="speak-btn" data-text="あのひと">🔊</button></div>
    <div class="vocab-row"><span class="word">あのかた</span><span class="kanji">あの方</span><span class="pos-tag pos-noun">名詞</span><span class="translation">那位（敬稱）</span><button class="speak-btn" data-text="あのかた">🔊</button></div>
    <div class="vocab-row"><span class="word">みなさん</span><span class="kanji">皆さん</span><span class="pos-tag pos-noun">名詞</span><span class="translation">大家、各位</span><button class="speak-btn" data-text="みなさん">🔊</button></div>
    <div class="vocab-row"><span class="word">せんせい</span><span class="kanji">先生</span><span class="pos-tag pos-noun">名詞</span><span class="translation">老師</span><button class="speak-btn" data-text="せんせい">🔊</button></div>
    <div class="vocab-row"><span class="word">きょうし</span><span class="kanji">教師</span><span class="pos-tag pos-noun">名詞</span><span class="translation">教師、教員</span><button class="speak-btn" data-text="きょうし">🔊</button></div>
    <div class="vocab-row"><span class="word">がくせい</span><span class="kanji">学生</span><span class="pos-tag pos-noun">名詞</span><span class="translation">學生</span><button class="speak-btn" data-text="がくせい">🔊</button></div>
    <div class="vocab-row"><span class="word">かいしゃいん</span><span class="kanji">会社員</span><span class="pos-tag pos-noun">名詞</span><span class="translation">公司職員</span><button class="speak-btn" data-text="かいしゃいん">🔊</button></div>
    <div class="vocab-row"><span class="word">しゃいん</span><span class="kanji">社員</span><span class="pos-tag pos-noun">名詞</span><span class="translation">～公司的職員</span><button class="speak-btn" data-text="しゃいん">🔊</button></div>
    <div class="vocab-row"><span class="word">ぎんこういん</span><span class="kanji">銀行員</span><span class="pos-tag pos-noun">名詞</span><span class="translation">銀行員</span><button class="speak-btn" data-text="ぎんこういん">🔊</button></div>
    <div class="vocab-row"><span class="word">いしゃ</span><span class="kanji">医者</span><span class="pos-tag pos-noun">名詞</span><span class="translation">醫生</span><button class="speak-btn" data-text="いしゃ">🔊</button></div>
    <div class="vocab-row"><span class="word">けんきゅうしゃ</span><span class="kanji">研究者</span><span class="pos-tag pos-noun">名詞</span><span class="translation">研究員</span><button class="speak-btn" data-text="けんきゅうしゃ">🔊</button></div>
    <div class="vocab-row"><span class="word">エンジニア</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">工程師</span><button class="speak-btn" data-text="エンジニア">🔊</button></div>
    <div class="vocab-row"><span class="word">だいがく</span><span class="kanji">大学</span><span class="pos-tag pos-noun">名詞</span><span class="translation">大學</span><button class="speak-btn" data-text="だいがく">🔊</button></div>
    <div class="vocab-row"><span class="word">びょういん</span><span class="kanji">病院</span><span class="pos-tag pos-noun">名詞</span><span class="translation">醫院</span><button class="speak-btn" data-text="びょういん">🔊</button></div>
    <div class="vocab-row"><span class="word">でんき</span><span class="kanji">電気</span><span class="pos-tag pos-noun">名詞</span><span class="translation">電氣、電力</span><button class="speak-btn" data-text="でんき">🔊</button></div>
    <div class="vocab-row"><span class="word">だれ</span><span class="kanji"></span><span class="pos-tag pos-other">疑問詞</span><span class="translation">誰</span><button class="speak-btn" data-text="だれ">🔊</button></div>
    <div class="vocab-row"><span class="word">どなた</span><span class="kanji"></span><span class="pos-tag pos-other">疑問詞</span><span class="translation">哪位（敬稱）</span><button class="speak-btn" data-text="どなた">🔊</button></div>
    <div class="vocab-row"><span class="word">～さい</span><span class="kanji">～歳</span><span class="pos-tag pos-counter">助數詞</span><span class="translation">～歲</span><button class="speak-btn" data-text="さい">🔊</button></div>
    <div class="vocab-row"><span class="word">なんさい</span><span class="kanji">何歳</span><span class="pos-tag pos-other">疑問詞</span><span class="translation">幾歲</span><button class="speak-btn" data-text="なんさい">🔊</button></div>
    <div class="vocab-row"><span class="word">おいくつ</span><span class="kanji"></span><span class="pos-tag pos-other">疑問詞</span><span class="translation">幾歲（敬稱）</span><button class="speak-btn" data-text="おいくつ">🔊</button></div>
    <div class="vocab-row"><span class="word">はい</span><span class="kanji"></span><span class="pos-tag pos-greeting">應答</span><span class="translation">是</span><button class="speak-btn" data-text="はい">🔊</button></div>
    <div class="vocab-row"><span class="word">いいえ</span><span class="kanji"></span><span class="pos-tag pos-greeting">應答</span><span class="translation">不是</span><button class="speak-btn" data-text="いいえ">🔊</button></div>
    <div class="vocab-row"><span class="word">はじめまして</span><span class="kanji"></span><span class="pos-tag pos-greeting">寒暄</span><span class="translation">初次見面</span><button class="speak-btn" data-text="はじめまして">🔊</button></div>
    <div class="vocab-row"><span class="word">どうぞ よろしく</span><span class="kanji"></span><span class="pos-tag pos-greeting">寒暄</span><span class="translation">請多指教</span><button class="speak-btn" data-text="どうぞ よろしく">🔊</button></div>
    <div class="vocab-row"><span class="word">しつれいですが</span><span class="kanji">失礼ですが</span><span class="pos-tag pos-greeting">寒暄</span><span class="translation">冒昧請問</span><button class="speak-btn" data-text="しつれいですが">🔊</button></div>
    <div class="vocab-row"><span class="word">おなまえは？</span><span class="kanji">お名前は？</span><span class="pos-tag pos-greeting">寒暄</span><span class="translation">請問貴姓？</span><button class="speak-btn" data-text="おなまえは">🔊</button></div>
    <div class="vocab-row"><span class="word">こちらは ～さんです</span><span class="kanji"></span><span class="pos-tag pos-greeting">寒暄</span><span class="translation">這位是～</span><button class="speak-btn" data-text="こちらは さんです">🔊</button></div>
    <div class="vocab-row"><span class="word">～からきました</span><span class="kanji">～から来ました</span><span class="pos-tag pos-other">句型</span><span class="translation">從～來的</span><button class="speak-btn" data-text="からきました">🔊</button></div>
  </div>
```

- [ ] **Step 2: Add Lesson 1 grammar HTML**

Append after the vocab-list `</div>` but still inside the lesson `<section>`:

```html
  <div class="grammar-section">
    <div class="section-title">文型</div>

    <div class="grammar-card">
      <div class="pattern">N1 は N2 です</div>
      <div class="pattern-desc">N1 是 N2</div>
      <div class="example">
        <div class="jp"><span>わたしは マイク・ミラーです。</span><button class="speak-btn" data-text="わたしは マイク・ミラーです">🔊</button></div>
        <div class="zh">我是 Mike Miller。</div>
      </div>
      <div class="example">
        <div class="jp"><span>サントスさんは ブラジルじんです。</span><button class="speak-btn" data-text="サントスさんは ブラジルじんです">🔊</button></div>
        <div class="zh">Santos 先生是巴西人。</div>
      </div>
      <table class="variation-table">
        <tr><td class="label-cell">肯定</td><td>N1 は N2 <span class="positive">です</span></td></tr>
        <tr><td class="label-cell">否定</td><td>N1 は N2 <span class="negative">じゃ ありません</span></td></tr>
        <tr><td class="label-cell">疑問</td><td>N1 は N2 です<span class="question">か</span></td></tr>
      </table>
    </div>

    <div class="grammar-card">
      <div class="pattern">N1 は N2 じゃ ありません</div>
      <div class="pattern-desc">N1 不是 N2</div>
      <div class="example">
        <div class="jp"><span>わたしは がくせいじゃ ありません。</span><button class="speak-btn" data-text="わたしは がくせいじゃ ありません">🔊</button></div>
        <div class="zh">我不是學生。</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">N1 は N2 ですか</div>
      <div class="pattern-desc">N1 是 N2 嗎？</div>
      <div class="example">
        <div class="jp"><span>ミラーさんは アメリカじんですか。</span><button class="speak-btn" data-text="ミラーさんは アメリカじんですか">🔊</button></div>
        <div class="zh">Miller 先生是美國人嗎？</div>
      </div>
      <div class="example">
        <div class="jp"><span>…はい、アメリカじんです。</span><button class="speak-btn" data-text="はい、アメリカじんです">🔊</button></div>
        <div class="zh">…是的，是美國人。</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">N1 も N2 です</div>
      <div class="pattern-desc">N1 也是 N2</div>
      <div class="example">
        <div class="jp"><span>ミラーさんは かいしゃいんです。グプタさんも かいしゃいんです。</span><button class="speak-btn" data-text="ミラーさんは かいしゃいんです。グプタさんも かいしゃいんです。">🔊</button></div>
        <div class="zh">Miller 是公司職員。Gupta 也是公司職員。</div>
      </div>
    </div>

  </div>
</section>
```

- [ ] **Step 3: Open in browser and verify**

Verify:
- Lesson 1 header shows with red left border
- Vocabulary rows display correctly with dotted separators
- Part-of-speech tags show with correct colors
- Grammar cards render with white background and rounded borders
- Variation table shows 肯定/否定/疑問 with colored text
- 🔊 buttons display (not functional yet)

---

### Task 3: Add Lesson 2 content

**Files:**
- Modify: `/Users/kai/Desktop/minna-no-nihongo/index.html` (append inside `<main>` after Lesson 1)

- [ ] **Step 1: Add Lesson 2 vocabulary and grammar HTML**

```html
<section class="lesson" id="lesson2">
  <div class="lesson-header">
    <div class="lesson-number">だい２か</div>
    <div class="lesson-title">これは 本です</div>
  </div>

  <div class="section-title">単語</div>
  <div class="vocab-list">
    <div class="vocab-row"><span class="word">これ</span><span class="kanji"></span><span class="pos-tag pos-other">指示詞</span><span class="translation">這個</span><button class="speak-btn" data-text="これ">🔊</button></div>
    <div class="vocab-row"><span class="word">それ</span><span class="kanji"></span><span class="pos-tag pos-other">指示詞</span><span class="translation">那個（離聽者近）</span><button class="speak-btn" data-text="それ">🔊</button></div>
    <div class="vocab-row"><span class="word">あれ</span><span class="kanji"></span><span class="pos-tag pos-other">指示詞</span><span class="translation">那個（離兩者都遠）</span><button class="speak-btn" data-text="あれ">🔊</button></div>
    <div class="vocab-row"><span class="word">この</span><span class="kanji"></span><span class="pos-tag pos-other">連體詞</span><span class="translation">這個～</span><button class="speak-btn" data-text="この">🔊</button></div>
    <div class="vocab-row"><span class="word">その</span><span class="kanji"></span><span class="pos-tag pos-other">連體詞</span><span class="translation">那個～</span><button class="speak-btn" data-text="その">🔊</button></div>
    <div class="vocab-row"><span class="word">あの</span><span class="kanji"></span><span class="pos-tag pos-other">連體詞</span><span class="translation">那個～（遠）</span><button class="speak-btn" data-text="あの">🔊</button></div>
    <div class="vocab-row"><span class="word">ほん</span><span class="kanji">本</span><span class="pos-tag pos-noun">名詞</span><span class="translation">書</span><button class="speak-btn" data-text="ほん">🔊</button></div>
    <div class="vocab-row"><span class="word">じしょ</span><span class="kanji">辞書</span><span class="pos-tag pos-noun">名詞</span><span class="translation">字典</span><button class="speak-btn" data-text="じしょ">🔊</button></div>
    <div class="vocab-row"><span class="word">ざっし</span><span class="kanji">雑誌</span><span class="pos-tag pos-noun">名詞</span><span class="translation">雜誌</span><button class="speak-btn" data-text="ざっし">🔊</button></div>
    <div class="vocab-row"><span class="word">しんぶん</span><span class="kanji">新聞</span><span class="pos-tag pos-noun">名詞</span><span class="translation">報紙</span><button class="speak-btn" data-text="しんぶん">🔊</button></div>
    <div class="vocab-row"><span class="word">ノート</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">筆記本</span><button class="speak-btn" data-text="ノート">🔊</button></div>
    <div class="vocab-row"><span class="word">てちょう</span><span class="kanji">手帳</span><span class="pos-tag pos-noun">名詞</span><span class="translation">記事本</span><button class="speak-btn" data-text="てちょう">🔊</button></div>
    <div class="vocab-row"><span class="word">めいし</span><span class="kanji">名刺</span><span class="pos-tag pos-noun">名詞</span><span class="translation">名片</span><button class="speak-btn" data-text="めいし">🔊</button></div>
    <div class="vocab-row"><span class="word">カード</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">卡片</span><button class="speak-btn" data-text="カード">🔊</button></div>
    <div class="vocab-row"><span class="word">えんぴつ</span><span class="kanji">鉛筆</span><span class="pos-tag pos-noun">名詞</span><span class="translation">鉛筆</span><button class="speak-btn" data-text="えんぴつ">🔊</button></div>
    <div class="vocab-row"><span class="word">ボールペン</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">原子筆</span><button class="speak-btn" data-text="ボールペン">🔊</button></div>
    <div class="vocab-row"><span class="word">シャープペンシル</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">自動鉛筆</span><button class="speak-btn" data-text="シャープペンシル">🔊</button></div>
    <div class="vocab-row"><span class="word">かぎ</span><span class="kanji">鍵</span><span class="pos-tag pos-noun">名詞</span><span class="translation">鑰匙</span><button class="speak-btn" data-text="かぎ">🔊</button></div>
    <div class="vocab-row"><span class="word">とけい</span><span class="kanji">時計</span><span class="pos-tag pos-noun">名詞</span><span class="translation">時鐘、手錶</span><button class="speak-btn" data-text="とけい">🔊</button></div>
    <div class="vocab-row"><span class="word">かさ</span><span class="kanji">傘</span><span class="pos-tag pos-noun">名詞</span><span class="translation">雨傘</span><button class="speak-btn" data-text="かさ">🔊</button></div>
    <div class="vocab-row"><span class="word">かばん</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">包包</span><button class="speak-btn" data-text="かばん">🔊</button></div>
    <div class="vocab-row"><span class="word">テレビ</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">電視</span><button class="speak-btn" data-text="テレビ">🔊</button></div>
    <div class="vocab-row"><span class="word">ラジオ</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">收音機</span><button class="speak-btn" data-text="ラジオ">🔊</button></div>
    <div class="vocab-row"><span class="word">カメラ</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">相機</span><button class="speak-btn" data-text="カメラ">🔊</button></div>
    <div class="vocab-row"><span class="word">コンピューター</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">電腦</span><button class="speak-btn" data-text="コンピューター">🔊</button></div>
    <div class="vocab-row"><span class="word">じどうしゃ</span><span class="kanji">自動車</span><span class="pos-tag pos-noun">名詞</span><span class="translation">汽車</span><button class="speak-btn" data-text="じどうしゃ">🔊</button></div>
    <div class="vocab-row"><span class="word">つくえ</span><span class="kanji">机</span><span class="pos-tag pos-noun">名詞</span><span class="translation">桌子</span><button class="speak-btn" data-text="つくえ">🔊</button></div>
    <div class="vocab-row"><span class="word">いす</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">椅子</span><button class="speak-btn" data-text="いす">🔊</button></div>
    <div class="vocab-row"><span class="word">チョコレート</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">巧克力</span><button class="speak-btn" data-text="チョコレート">🔊</button></div>
    <div class="vocab-row"><span class="word">コーヒー</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">咖啡</span><button class="speak-btn" data-text="コーヒー">🔊</button></div>
    <div class="vocab-row"><span class="word">えいご</span><span class="kanji">英語</span><span class="pos-tag pos-noun">名詞</span><span class="translation">英語</span><button class="speak-btn" data-text="えいご">🔊</button></div>
    <div class="vocab-row"><span class="word">にほんご</span><span class="kanji">日本語</span><span class="pos-tag pos-noun">名詞</span><span class="translation">日語</span><button class="speak-btn" data-text="にほんご">🔊</button></div>
    <div class="vocab-row"><span class="word">なん</span><span class="kanji">何</span><span class="pos-tag pos-other">疑問詞</span><span class="translation">什麼</span><button class="speak-btn" data-text="なん">🔊</button></div>
    <div class="vocab-row"><span class="word">そう</span><span class="kanji"></span><span class="pos-tag pos-adv">副詞</span><span class="translation">那樣</span><button class="speak-btn" data-text="そう">🔊</button></div>
    <div class="vocab-row"><span class="word">ちがいます</span><span class="kanji">違います</span><span class="pos-tag pos-verb1">動詞I</span><span class="translation">不對</span><button class="speak-btn" data-text="ちがいます">🔊</button></div>
    <div class="vocab-row"><span class="word">おみやげ</span><span class="kanji">お土産</span><span class="pos-tag pos-noun">名詞</span><span class="translation">禮物、伴手禮</span><button class="speak-btn" data-text="おみやげ">🔊</button></div>
  </div>

  <div class="grammar-section">
    <div class="section-title">文型</div>

    <div class="grammar-card">
      <div class="pattern">これ/それ/あれ は N です</div>
      <div class="pattern-desc">這個/那個/那個（遠）是 N</div>
      <div class="example">
        <div class="jp"><span>これは テレビです。</span><button class="speak-btn" data-text="これは テレビです">🔊</button></div>
        <div class="zh">這是電視。</div>
      </div>
      <div class="example">
        <div class="jp"><span>それは わたしの かばんです。</span><button class="speak-btn" data-text="それは わたしの かばんです">🔊</button></div>
        <div class="zh">那是我的包包。</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">この/その/あの N1 は N2 です</div>
      <div class="pattern-desc">這個/那個/那個～是 N2</div>
      <div class="example">
        <div class="jp"><span>この ほんは わたしのです。</span><button class="speak-btn" data-text="この ほんは わたしのです">🔊</button></div>
        <div class="zh">這本書是我的。</div>
      </div>
      <div class="example">
        <div class="jp"><span>あの かたは だれですか。</span><button class="speak-btn" data-text="あの かたは だれですか">🔊</button></div>
        <div class="zh">那位是誰？</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">そうです / そうじゃ ありません</div>
      <div class="pattern-desc">是的 / 不是的</div>
      <div class="example">
        <div class="jp"><span>それは ノートですか。…はい、そうです。</span><button class="speak-btn" data-text="それは ノートですか。はい、そうです。">🔊</button></div>
        <div class="zh">那是筆記本嗎？…是的。</div>
      </div>
      <div class="example">
        <div class="jp"><span>それは シャープペンシルですか。…いいえ、そうじゃ ありません。</span><button class="speak-btn" data-text="それは シャープペンシルですか。いいえ、そうじゃ ありません。">🔊</button></div>
        <div class="zh">那是自動鉛筆嗎？…不，不是的。</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">N1 の N2</div>
      <div class="pattern-desc">N1 的 N2（所屬、產地、類別）</div>
      <div class="example">
        <div class="jp"><span>これは にほんの カメラです。</span><button class="speak-btn" data-text="これは にほんの カメラです">🔊</button></div>
        <div class="zh">這是日本的相機。</div>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Verify in browser**

Verify Lesson 2 renders below Lesson 1 and clicking 第2課 in sidebar scrolls to it.

---

### Task 4: Add Lessons 3 and 4 content

**Files:**
- Modify: `/Users/kai/Desktop/minna-no-nihongo/index.html` (append inside `<main>`)

- [ ] **Step 1: Add Lesson 3 HTML**

```html
<section class="lesson" id="lesson3">
  <div class="lesson-header">
    <div class="lesson-number">だい３か</div>
    <div class="lesson-title">ここは 食堂です</div>
  </div>

  <div class="section-title">単語</div>
  <div class="vocab-list">
    <div class="vocab-row"><span class="word">ここ</span><span class="kanji"></span><span class="pos-tag pos-other">指示詞</span><span class="translation">這裡</span><button class="speak-btn" data-text="ここ">🔊</button></div>
    <div class="vocab-row"><span class="word">そこ</span><span class="kanji"></span><span class="pos-tag pos-other">指示詞</span><span class="translation">那裡</span><button class="speak-btn" data-text="そこ">🔊</button></div>
    <div class="vocab-row"><span class="word">あそこ</span><span class="kanji"></span><span class="pos-tag pos-other">指示詞</span><span class="translation">那裡（遠）</span><button class="speak-btn" data-text="あそこ">🔊</button></div>
    <div class="vocab-row"><span class="word">どこ</span><span class="kanji"></span><span class="pos-tag pos-other">疑問詞</span><span class="translation">哪裡</span><button class="speak-btn" data-text="どこ">🔊</button></div>
    <div class="vocab-row"><span class="word">こちら</span><span class="kanji"></span><span class="pos-tag pos-other">指示詞</span><span class="translation">這邊（敬稱）</span><button class="speak-btn" data-text="こちら">🔊</button></div>
    <div class="vocab-row"><span class="word">そちら</span><span class="kanji"></span><span class="pos-tag pos-other">指示詞</span><span class="translation">那邊（敬稱）</span><button class="speak-btn" data-text="そちら">🔊</button></div>
    <div class="vocab-row"><span class="word">あちら</span><span class="kanji"></span><span class="pos-tag pos-other">指示詞</span><span class="translation">那邊（遠、敬稱）</span><button class="speak-btn" data-text="あちら">🔊</button></div>
    <div class="vocab-row"><span class="word">どちら</span><span class="kanji"></span><span class="pos-tag pos-other">疑問詞</span><span class="translation">哪邊（敬稱）</span><button class="speak-btn" data-text="どちら">🔊</button></div>
    <div class="vocab-row"><span class="word">きょうしつ</span><span class="kanji">教室</span><span class="pos-tag pos-noun">名詞</span><span class="translation">教室</span><button class="speak-btn" data-text="きょうしつ">🔊</button></div>
    <div class="vocab-row"><span class="word">しょくどう</span><span class="kanji">食堂</span><span class="pos-tag pos-noun">名詞</span><span class="translation">食堂</span><button class="speak-btn" data-text="しょくどう">🔊</button></div>
    <div class="vocab-row"><span class="word">じむしょ</span><span class="kanji">事務所</span><span class="pos-tag pos-noun">名詞</span><span class="translation">辦公室</span><button class="speak-btn" data-text="じむしょ">🔊</button></div>
    <div class="vocab-row"><span class="word">かいぎしつ</span><span class="kanji">会議室</span><span class="pos-tag pos-noun">名詞</span><span class="translation">會議室</span><button class="speak-btn" data-text="かいぎしつ">🔊</button></div>
    <div class="vocab-row"><span class="word">うけつけ</span><span class="kanji">受付</span><span class="pos-tag pos-noun">名詞</span><span class="translation">接待處</span><button class="speak-btn" data-text="うけつけ">🔊</button></div>
    <div class="vocab-row"><span class="word">ロビー</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">大廳</span><button class="speak-btn" data-text="ロビー">🔊</button></div>
    <div class="vocab-row"><span class="word">へや</span><span class="kanji">部屋</span><span class="pos-tag pos-noun">名詞</span><span class="translation">房間</span><button class="speak-btn" data-text="へや">🔊</button></div>
    <div class="vocab-row"><span class="word">トイレ</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">廁所</span><button class="speak-btn" data-text="トイレ">🔊</button></div>
    <div class="vocab-row"><span class="word">かいだん</span><span class="kanji">階段</span><span class="pos-tag pos-noun">名詞</span><span class="translation">樓梯</span><button class="speak-btn" data-text="かいだん">🔊</button></div>
    <div class="vocab-row"><span class="word">エレベーター</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">電梯</span><button class="speak-btn" data-text="エレベーター">🔊</button></div>
    <div class="vocab-row"><span class="word">エスカレーター</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">電扶梯</span><button class="speak-btn" data-text="エスカレーター">🔊</button></div>
    <div class="vocab-row"><span class="word">おくに</span><span class="kanji">お国</span><span class="pos-tag pos-noun">名詞</span><span class="translation">您的國家（敬稱）</span><button class="speak-btn" data-text="おくに">🔊</button></div>
    <div class="vocab-row"><span class="word">かいしゃ</span><span class="kanji">会社</span><span class="pos-tag pos-noun">名詞</span><span class="translation">公司</span><button class="speak-btn" data-text="かいしゃ">🔊</button></div>
    <div class="vocab-row"><span class="word">うち</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">家</span><button class="speak-btn" data-text="うち">🔊</button></div>
    <div class="vocab-row"><span class="word">でんわ</span><span class="kanji">電話</span><span class="pos-tag pos-noun">名詞</span><span class="translation">電話</span><button class="speak-btn" data-text="でんわ">🔊</button></div>
    <div class="vocab-row"><span class="word">くつ</span><span class="kanji">靴</span><span class="pos-tag pos-noun">名詞</span><span class="translation">鞋子</span><button class="speak-btn" data-text="くつ">🔊</button></div>
    <div class="vocab-row"><span class="word">ネクタイ</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">領帶</span><button class="speak-btn" data-text="ネクタイ">🔊</button></div>
    <div class="vocab-row"><span class="word">ワイン</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">葡萄酒</span><button class="speak-btn" data-text="ワイン">🔊</button></div>
    <div class="vocab-row"><span class="word">なんかい</span><span class="kanji">何階</span><span class="pos-tag pos-other">疑問詞</span><span class="translation">幾樓</span><button class="speak-btn" data-text="なんかい">🔊</button></div>
    <div class="vocab-row"><span class="word">～かい/がい</span><span class="kanji">～階</span><span class="pos-tag pos-counter">助數詞</span><span class="translation">～樓</span><button class="speak-btn" data-text="かい">🔊</button></div>
  </div>

  <div class="grammar-section">
    <div class="section-title">文型</div>

    <div class="grammar-card">
      <div class="pattern">ここ/そこ/あそこ は N です</div>
      <div class="pattern-desc">這裡/那裡/那裡（遠）是 N</div>
      <div class="example">
        <div class="jp"><span>ここは しょくどうです。</span><button class="speak-btn" data-text="ここは しょくどうです">🔊</button></div>
        <div class="zh">這裡是食堂。</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">N は ここ/そこ/あそこ です</div>
      <div class="pattern-desc">N 在這裡/那裡/那裡</div>
      <div class="example">
        <div class="jp"><span>トイレは あそこです。</span><button class="speak-btn" data-text="トイレは あそこです">🔊</button></div>
        <div class="zh">廁所在那裡。</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">N は どこ ですか</div>
      <div class="pattern-desc">N 在哪裡？</div>
      <div class="example">
        <div class="jp"><span>エレベーターは どこですか。</span><button class="speak-btn" data-text="エレベーターは どこですか">🔊</button></div>
        <div class="zh">電梯在哪裡？</div>
      </div>
      <div class="example">
        <div class="jp"><span>…あちらです。</span><button class="speak-btn" data-text="あちらです">🔊</button></div>
        <div class="zh">…在那邊。</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">N は N（国）の N（製品）です</div>
      <div class="pattern-desc">N 是某國的產品</div>
      <div class="example">
        <div class="jp"><span>これは にほんの ワインです。</span><button class="speak-btn" data-text="これは にほんの ワインです">🔊</button></div>
        <div class="zh">這是日本的葡萄酒。</div>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add Lesson 4 HTML**

```html
<section class="lesson" id="lesson4">
  <div class="lesson-header">
    <div class="lesson-number">だい４か</div>
    <div class="lesson-title">今 何時ですか</div>
  </div>

  <div class="section-title">単語</div>
  <div class="vocab-list">
    <div class="vocab-row"><span class="word">おきます</span><span class="kanji">起きます</span><span class="pos-tag pos-verb2">動詞II</span><span class="translation">起床</span><button class="speak-btn" data-text="おきます">🔊</button></div>
    <div class="vocab-row"><span class="word">ねます</span><span class="kanji">寝ます</span><span class="pos-tag pos-verb2">動詞II</span><span class="translation">睡覺</span><button class="speak-btn" data-text="ねます">🔊</button></div>
    <div class="vocab-row"><span class="word">はたらきます</span><span class="kanji">働きます</span><span class="pos-tag pos-verb1">動詞I</span><span class="translation">工作</span><button class="speak-btn" data-text="はたらきます">🔊</button></div>
    <div class="vocab-row"><span class="word">やすみます</span><span class="kanji">休みます</span><span class="pos-tag pos-verb1">動詞I</span><span class="translation">休息</span><button class="speak-btn" data-text="やすみます">🔊</button></div>
    <div class="vocab-row"><span class="word">べんきょうします</span><span class="kanji">勉強します</span><span class="pos-tag pos-verb3">動詞III</span><span class="translation">學習</span><button class="speak-btn" data-text="べんきょうします">🔊</button></div>
    <div class="vocab-row"><span class="word">おわります</span><span class="kanji">終わります</span><span class="pos-tag pos-verb1">動詞I</span><span class="translation">結束</span><button class="speak-btn" data-text="おわります">🔊</button></div>
    <div class="vocab-row"><span class="word">デパート</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">百貨公司</span><button class="speak-btn" data-text="デパート">🔊</button></div>
    <div class="vocab-row"><span class="word">ぎんこう</span><span class="kanji">銀行</span><span class="pos-tag pos-noun">名詞</span><span class="translation">銀行</span><button class="speak-btn" data-text="ぎんこう">🔊</button></div>
    <div class="vocab-row"><span class="word">ゆうびんきょく</span><span class="kanji">郵便局</span><span class="pos-tag pos-noun">名詞</span><span class="translation">郵局</span><button class="speak-btn" data-text="ゆうびんきょく">🔊</button></div>
    <div class="vocab-row"><span class="word">としょかん</span><span class="kanji">図書館</span><span class="pos-tag pos-noun">名詞</span><span class="translation">圖書館</span><button class="speak-btn" data-text="としょかん">🔊</button></div>
    <div class="vocab-row"><span class="word">びじゅつかん</span><span class="kanji">美術館</span><span class="pos-tag pos-noun">名詞</span><span class="translation">美術館</span><button class="speak-btn" data-text="びじゅつかん">🔊</button></div>
    <div class="vocab-row"><span class="word">いま</span><span class="kanji">今</span><span class="pos-tag pos-noun">名詞</span><span class="translation">現在</span><button class="speak-btn" data-text="いま">🔊</button></div>
    <div class="vocab-row"><span class="word">～じ</span><span class="kanji">～時</span><span class="pos-tag pos-counter">助數詞</span><span class="translation">～點</span><button class="speak-btn" data-text="じ">🔊</button></div>
    <div class="vocab-row"><span class="word">～ふん/ぷん</span><span class="kanji">～分</span><span class="pos-tag pos-counter">助數詞</span><span class="translation">～分</span><button class="speak-btn" data-text="ふん">🔊</button></div>
    <div class="vocab-row"><span class="word">はん</span><span class="kanji">半</span><span class="pos-tag pos-noun">名詞</span><span class="translation">半（30分）</span><button class="speak-btn" data-text="はん">🔊</button></div>
    <div class="vocab-row"><span class="word">なんじ</span><span class="kanji">何時</span><span class="pos-tag pos-other">疑問詞</span><span class="translation">幾點</span><button class="speak-btn" data-text="なんじ">🔊</button></div>
    <div class="vocab-row"><span class="word">なんぷん</span><span class="kanji">何分</span><span class="pos-tag pos-other">疑問詞</span><span class="translation">幾分</span><button class="speak-btn" data-text="なんぷん">🔊</button></div>
    <div class="vocab-row"><span class="word">ごぜん</span><span class="kanji">午前</span><span class="pos-tag pos-noun">名詞</span><span class="translation">上午</span><button class="speak-btn" data-text="ごぜん">🔊</button></div>
    <div class="vocab-row"><span class="word">ごご</span><span class="kanji">午後</span><span class="pos-tag pos-noun">名詞</span><span class="translation">下午</span><button class="speak-btn" data-text="ごご">🔊</button></div>
    <div class="vocab-row"><span class="word">あさ</span><span class="kanji">朝</span><span class="pos-tag pos-noun">名詞</span><span class="translation">早上</span><button class="speak-btn" data-text="あさ">🔊</button></div>
    <div class="vocab-row"><span class="word">ひる</span><span class="kanji">昼</span><span class="pos-tag pos-noun">名詞</span><span class="translation">中午</span><button class="speak-btn" data-text="ひる">🔊</button></div>
    <div class="vocab-row"><span class="word">ばん/よる</span><span class="kanji">晩/夜</span><span class="pos-tag pos-noun">名詞</span><span class="translation">晚上</span><button class="speak-btn" data-text="ばん">🔊</button></div>
    <div class="vocab-row"><span class="word">おととい</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">前天</span><button class="speak-btn" data-text="おととい">🔊</button></div>
    <div class="vocab-row"><span class="word">きのう</span><span class="kanji">昨日</span><span class="pos-tag pos-noun">名詞</span><span class="translation">昨天</span><button class="speak-btn" data-text="きのう">🔊</button></div>
    <div class="vocab-row"><span class="word">きょう</span><span class="kanji">今日</span><span class="pos-tag pos-noun">名詞</span><span class="translation">今天</span><button class="speak-btn" data-text="きょう">🔊</button></div>
    <div class="vocab-row"><span class="word">あした</span><span class="kanji">明日</span><span class="pos-tag pos-noun">名詞</span><span class="translation">明天</span><button class="speak-btn" data-text="あした">🔊</button></div>
    <div class="vocab-row"><span class="word">あさって</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">後天</span><button class="speak-btn" data-text="あさって">🔊</button></div>
    <div class="vocab-row"><span class="word">まいあさ</span><span class="kanji">毎朝</span><span class="pos-tag pos-noun">名詞</span><span class="translation">每天早上</span><button class="speak-btn" data-text="まいあさ">🔊</button></div>
    <div class="vocab-row"><span class="word">まいばん</span><span class="kanji">毎晩</span><span class="pos-tag pos-noun">名詞</span><span class="translation">每天晚上</span><button class="speak-btn" data-text="まいばん">🔊</button></div>
    <div class="vocab-row"><span class="word">まいにち</span><span class="kanji">毎日</span><span class="pos-tag pos-noun">名詞</span><span class="translation">每天</span><button class="speak-btn" data-text="まいにち">🔊</button></div>
    <div class="vocab-row"><span class="word">げつようび</span><span class="kanji">月曜日</span><span class="pos-tag pos-noun">名詞</span><span class="translation">星期一</span><button class="speak-btn" data-text="げつようび">🔊</button></div>
    <div class="vocab-row"><span class="word">かようび</span><span class="kanji">火曜日</span><span class="pos-tag pos-noun">名詞</span><span class="translation">星期二</span><button class="speak-btn" data-text="かようび">🔊</button></div>
    <div class="vocab-row"><span class="word">すいようび</span><span class="kanji">水曜日</span><span class="pos-tag pos-noun">名詞</span><span class="translation">星期三</span><button class="speak-btn" data-text="すいようび">🔊</button></div>
    <div class="vocab-row"><span class="word">もくようび</span><span class="kanji">木曜日</span><span class="pos-tag pos-noun">名詞</span><span class="translation">星期四</span><button class="speak-btn" data-text="もくようび">🔊</button></div>
    <div class="vocab-row"><span class="word">きんようび</span><span class="kanji">金曜日</span><span class="pos-tag pos-noun">名詞</span><span class="translation">星期五</span><button class="speak-btn" data-text="きんようび">🔊</button></div>
    <div class="vocab-row"><span class="word">どようび</span><span class="kanji">土曜日</span><span class="pos-tag pos-noun">名詞</span><span class="translation">星期六</span><button class="speak-btn" data-text="どようび">🔊</button></div>
    <div class="vocab-row"><span class="word">にちようび</span><span class="kanji">日曜日</span><span class="pos-tag pos-noun">名詞</span><span class="translation">星期日</span><button class="speak-btn" data-text="にちようび">🔊</button></div>
    <div class="vocab-row"><span class="word">なんようび</span><span class="kanji">何曜日</span><span class="pos-tag pos-other">疑問詞</span><span class="translation">星期幾</span><button class="speak-btn" data-text="なんようび">🔊</button></div>
  </div>

  <div class="grammar-section">
    <div class="section-title">文型</div>

    <div class="grammar-card">
      <div class="pattern">今 N時 N分 です</div>
      <div class="pattern-desc">現在是 N 點 N 分</div>
      <div class="example">
        <div class="jp"><span>いま ７じ１０ぷんです。</span><button class="speak-btn" data-text="いま ７じ１０ぷんです">🔊</button></div>
        <div class="zh">現在是 7 點 10 分。</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">V ます / V ません / V ました / V ませんでした</div>
      <div class="pattern-desc">動詞的禮貌形：肯定/否定/過去肯定/過去否定</div>
      <div class="example">
        <div class="jp"><span>まいにち べんきょうします。</span><button class="speak-btn" data-text="まいにち べんきょうします">🔊</button></div>
        <div class="zh">每天學習。</div>
      </div>
      <table class="variation-table">
        <tr><td class="label-cell">肯定</td><td>V <span class="positive">ます</span></td></tr>
        <tr><td class="label-cell">否定</td><td>V <span class="negative">ません</span></td></tr>
        <tr><td class="label-cell">過去肯定</td><td>V <span class="positive">ました</span></td></tr>
        <tr><td class="label-cell">過去否定</td><td>V <span class="negative">ませんでした</span></td></tr>
      </table>
    </div>

    <div class="grammar-card">
      <div class="pattern">N時 に V ます</div>
      <div class="pattern-desc">在 N 點做某事</div>
      <div class="example">
        <div class="jp"><span>６じに おきます。</span><button class="speak-btn" data-text="６じに おきます">🔊</button></div>
        <div class="zh">6 點起床。</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">N から N まで</div>
      <div class="pattern-desc">從 N 到 N</div>
      <div class="example">
        <div class="jp"><span>９じから ５じまで はたらきます。</span><button class="speak-btn" data-text="９じから ５じまで はたらきます">🔊</button></div>
        <div class="zh">從 9 點工作到 5 點。</div>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Verify in browser**

Verify lessons 3 and 4 render correctly.

---

### Task 5: Add Lessons 5 and 6 content

**Files:**
- Modify: `/Users/kai/Desktop/minna-no-nihongo/index.html` (append inside `<main>`)

- [ ] **Step 1: Add Lesson 5 HTML**

```html
<section class="lesson" id="lesson5">
  <div class="lesson-header">
    <div class="lesson-number">だい５か</div>
    <div class="lesson-title">甲賀へ 行きます</div>
  </div>

  <div class="section-title">単語</div>
  <div class="vocab-list">
    <div class="vocab-row"><span class="word">いきます</span><span class="kanji">行きます</span><span class="pos-tag pos-verb1">動詞I</span><span class="translation">去</span><button class="speak-btn" data-text="いきます">🔊</button></div>
    <div class="vocab-row"><span class="word">きます</span><span class="kanji">来ます</span><span class="pos-tag pos-verb3">動詞III</span><span class="translation">來</span><button class="speak-btn" data-text="きます">🔊</button></div>
    <div class="vocab-row"><span class="word">かえります</span><span class="kanji">帰ります</span><span class="pos-tag pos-verb1">動詞I</span><span class="translation">回去</span><button class="speak-btn" data-text="かえります">🔊</button></div>
    <div class="vocab-row"><span class="word">がっこう</span><span class="kanji">学校</span><span class="pos-tag pos-noun">名詞</span><span class="translation">學校</span><button class="speak-btn" data-text="がっこう">🔊</button></div>
    <div class="vocab-row"><span class="word">スーパー</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">超市</span><button class="speak-btn" data-text="スーパー">🔊</button></div>
    <div class="vocab-row"><span class="word">えき</span><span class="kanji">駅</span><span class="pos-tag pos-noun">名詞</span><span class="translation">車站</span><button class="speak-btn" data-text="えき">🔊</button></div>
    <div class="vocab-row"><span class="word">ひこうき</span><span class="kanji">飛行機</span><span class="pos-tag pos-noun">名詞</span><span class="translation">飛機</span><button class="speak-btn" data-text="ひこうき">🔊</button></div>
    <div class="vocab-row"><span class="word">ふね</span><span class="kanji">船</span><span class="pos-tag pos-noun">名詞</span><span class="translation">船</span><button class="speak-btn" data-text="ふね">🔊</button></div>
    <div class="vocab-row"><span class="word">でんしゃ</span><span class="kanji">電車</span><span class="pos-tag pos-noun">名詞</span><span class="translation">電車</span><button class="speak-btn" data-text="でんしゃ">🔊</button></div>
    <div class="vocab-row"><span class="word">バス</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">公車</span><button class="speak-btn" data-text="バス">🔊</button></div>
    <div class="vocab-row"><span class="word">タクシー</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">計程車</span><button class="speak-btn" data-text="タクシー">🔊</button></div>
    <div class="vocab-row"><span class="word">じてんしゃ</span><span class="kanji">自転車</span><span class="pos-tag pos-noun">名詞</span><span class="translation">腳踏車</span><button class="speak-btn" data-text="じてんしゃ">🔊</button></div>
    <div class="vocab-row"><span class="word">あるいて</span><span class="kanji">歩いて</span><span class="pos-tag pos-adv">副詞</span><span class="translation">走路</span><button class="speak-btn" data-text="あるいて">🔊</button></div>
    <div class="vocab-row"><span class="word">ひと</span><span class="kanji">人</span><span class="pos-tag pos-noun">名詞</span><span class="translation">人</span><button class="speak-btn" data-text="ひと">🔊</button></div>
    <div class="vocab-row"><span class="word">ともだち</span><span class="kanji">友達</span><span class="pos-tag pos-noun">名詞</span><span class="translation">朋友</span><button class="speak-btn" data-text="ともだち">🔊</button></div>
    <div class="vocab-row"><span class="word">かれ</span><span class="kanji">彼</span><span class="pos-tag pos-noun">名詞</span><span class="translation">他、男朋友</span><button class="speak-btn" data-text="かれ">🔊</button></div>
    <div class="vocab-row"><span class="word">かのじょ</span><span class="kanji">彼女</span><span class="pos-tag pos-noun">名詞</span><span class="translation">她、女朋友</span><button class="speak-btn" data-text="かのじょ">🔊</button></div>
    <div class="vocab-row"><span class="word">かぞく</span><span class="kanji">家族</span><span class="pos-tag pos-noun">名詞</span><span class="translation">家人</span><button class="speak-btn" data-text="かぞく">🔊</button></div>
    <div class="vocab-row"><span class="word">ひとりで</span><span class="kanji">一人で</span><span class="pos-tag pos-adv">副詞</span><span class="translation">一個人</span><button class="speak-btn" data-text="ひとりで">🔊</button></div>
    <div class="vocab-row"><span class="word">せんしゅう</span><span class="kanji">先週</span><span class="pos-tag pos-noun">名詞</span><span class="translation">上週</span><button class="speak-btn" data-text="せんしゅう">🔊</button></div>
    <div class="vocab-row"><span class="word">こんしゅう</span><span class="kanji">今週</span><span class="pos-tag pos-noun">名詞</span><span class="translation">這週</span><button class="speak-btn" data-text="こんしゅう">🔊</button></div>
    <div class="vocab-row"><span class="word">らいしゅう</span><span class="kanji">来週</span><span class="pos-tag pos-noun">名詞</span><span class="translation">下週</span><button class="speak-btn" data-text="らいしゅう">🔊</button></div>
    <div class="vocab-row"><span class="word">せんげつ</span><span class="kanji">先月</span><span class="pos-tag pos-noun">名詞</span><span class="translation">上個月</span><button class="speak-btn" data-text="せんげつ">🔊</button></div>
    <div class="vocab-row"><span class="word">こんげつ</span><span class="kanji">今月</span><span class="pos-tag pos-noun">名詞</span><span class="translation">這個月</span><button class="speak-btn" data-text="こんげつ">🔊</button></div>
    <div class="vocab-row"><span class="word">らいげつ</span><span class="kanji">来月</span><span class="pos-tag pos-noun">名詞</span><span class="translation">下個月</span><button class="speak-btn" data-text="らいげつ">🔊</button></div>
    <div class="vocab-row"><span class="word">きょねん</span><span class="kanji">去年</span><span class="pos-tag pos-noun">名詞</span><span class="translation">去年</span><button class="speak-btn" data-text="きょねん">🔊</button></div>
    <div class="vocab-row"><span class="word">ことし</span><span class="kanji">今年</span><span class="pos-tag pos-noun">名詞</span><span class="translation">今年</span><button class="speak-btn" data-text="ことし">🔊</button></div>
    <div class="vocab-row"><span class="word">らいねん</span><span class="kanji">来年</span><span class="pos-tag pos-noun">名詞</span><span class="translation">明年</span><button class="speak-btn" data-text="らいねん">🔊</button></div>
    <div class="vocab-row"><span class="word">いつ</span><span class="kanji"></span><span class="pos-tag pos-other">疑問詞</span><span class="translation">什麼時候</span><button class="speak-btn" data-text="いつ">🔊</button></div>
    <div class="vocab-row"><span class="word">たんじょうび</span><span class="kanji">誕生日</span><span class="pos-tag pos-noun">名詞</span><span class="translation">生日</span><button class="speak-btn" data-text="たんじょうび">🔊</button></div>
  </div>

  <div class="grammar-section">
    <div class="section-title">文型</div>

    <div class="grammar-card">
      <div class="pattern">N（場所）へ 行きます/来ます/帰ります</div>
      <div class="pattern-desc">去/來/回去某個地方</div>
      <div class="example">
        <div class="jp"><span>きょう がっこうへ いきます。</span><button class="speak-btn" data-text="きょう がっこうへ いきます">🔊</button></div>
        <div class="zh">今天去學校。</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">N（乗り物）で 行きます</div>
      <div class="pattern-desc">搭乘某交通工具去</div>
      <div class="example">
        <div class="jp"><span>でんしゃで いきます。</span><button class="speak-btn" data-text="でんしゃで いきます">🔊</button></div>
        <div class="zh">搭電車去。</div>
      </div>
      <div class="example">
        <div class="jp"><span>あるいて いきます。</span><button class="speak-btn" data-text="あるいて いきます">🔊</button></div>
        <div class="zh">走路去。</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">N（人）と 行きます</div>
      <div class="pattern-desc">和某人一起去</div>
      <div class="example">
        <div class="jp"><span>かぞくと きょうとへ いきました。</span><button class="speak-btn" data-text="かぞくと きょうとへ いきました">🔊</button></div>
        <div class="zh">和家人去了京都。</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">いつ V ますか</div>
      <div class="pattern-desc">什麼時候做某事？</div>
      <div class="example">
        <div class="jp"><span>いつ にほんへ きましたか。</span><button class="speak-btn" data-text="いつ にほんへ きましたか">🔊</button></div>
        <div class="zh">什麼時候來日本的？</div>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add Lesson 6 HTML**

```html
<section class="lesson" id="lesson6">
  <div class="lesson-header">
    <div class="lesson-number">だい６か</div>
    <div class="lesson-title">いっしょに 京都へ 行きませんか</div>
  </div>

  <div class="section-title">単語</div>
  <div class="vocab-list">
    <div class="vocab-row"><span class="word">たべます</span><span class="kanji">食べます</span><span class="pos-tag pos-verb2">動詞II</span><span class="translation">吃</span><button class="speak-btn" data-text="たべます">🔊</button></div>
    <div class="vocab-row"><span class="word">のみます</span><span class="kanji">飲みます</span><span class="pos-tag pos-verb1">動詞I</span><span class="translation">喝</span><button class="speak-btn" data-text="のみます">🔊</button></div>
    <div class="vocab-row"><span class="word">すいます</span><span class="kanji">吸います</span><span class="pos-tag pos-verb1">動詞I</span><span class="translation">抽（菸）</span><button class="speak-btn" data-text="すいます">🔊</button></div>
    <div class="vocab-row"><span class="word">みます</span><span class="kanji">見ます</span><span class="pos-tag pos-verb2">動詞II</span><span class="translation">看</span><button class="speak-btn" data-text="みます">🔊</button></div>
    <div class="vocab-row"><span class="word">ききます</span><span class="kanji">聞きます</span><span class="pos-tag pos-verb1">動詞I</span><span class="translation">聽</span><button class="speak-btn" data-text="ききます">🔊</button></div>
    <div class="vocab-row"><span class="word">よみます</span><span class="kanji">読みます</span><span class="pos-tag pos-verb1">動詞I</span><span class="translation">讀</span><button class="speak-btn" data-text="よみます">🔊</button></div>
    <div class="vocab-row"><span class="word">かきます</span><span class="kanji">書きます</span><span class="pos-tag pos-verb1">動詞I</span><span class="translation">寫</span><button class="speak-btn" data-text="かきます">🔊</button></div>
    <div class="vocab-row"><span class="word">かいます</span><span class="kanji">買います</span><span class="pos-tag pos-verb1">動詞I</span><span class="translation">買</span><button class="speak-btn" data-text="かいます">🔊</button></div>
    <div class="vocab-row"><span class="word">とります</span><span class="kanji">撮ります</span><span class="pos-tag pos-verb1">動詞I</span><span class="translation">拍（照片）</span><button class="speak-btn" data-text="とります">🔊</button></div>
    <div class="vocab-row"><span class="word">します</span><span class="kanji"></span><span class="pos-tag pos-verb3">動詞III</span><span class="translation">做</span><button class="speak-btn" data-text="します">🔊</button></div>
    <div class="vocab-row"><span class="word">あいます</span><span class="kanji">会います</span><span class="pos-tag pos-verb1">動詞I</span><span class="translation">見面</span><button class="speak-btn" data-text="あいます">🔊</button></div>
    <div class="vocab-row"><span class="word">ごはん</span><span class="kanji">ご飯</span><span class="pos-tag pos-noun">名詞</span><span class="translation">飯、餐</span><button class="speak-btn" data-text="ごはん">🔊</button></div>
    <div class="vocab-row"><span class="word">あさごはん</span><span class="kanji">朝ご飯</span><span class="pos-tag pos-noun">名詞</span><span class="translation">早餐</span><button class="speak-btn" data-text="あさごはん">🔊</button></div>
    <div class="vocab-row"><span class="word">ひるごはん</span><span class="kanji">昼ご飯</span><span class="pos-tag pos-noun">名詞</span><span class="translation">午餐</span><button class="speak-btn" data-text="ひるごはん">🔊</button></div>
    <div class="vocab-row"><span class="word">ばんごはん</span><span class="kanji">晩ご飯</span><span class="pos-tag pos-noun">名詞</span><span class="translation">晚餐</span><button class="speak-btn" data-text="ばんごはん">🔊</button></div>
    <div class="vocab-row"><span class="word">パン</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">麵包</span><button class="speak-btn" data-text="パン">🔊</button></div>
    <div class="vocab-row"><span class="word">たまご</span><span class="kanji">卵</span><span class="pos-tag pos-noun">名詞</span><span class="translation">雞蛋</span><button class="speak-btn" data-text="たまご">🔊</button></div>
    <div class="vocab-row"><span class="word">にく</span><span class="kanji">肉</span><span class="pos-tag pos-noun">名詞</span><span class="translation">肉</span><button class="speak-btn" data-text="にく">🔊</button></div>
    <div class="vocab-row"><span class="word">さかな</span><span class="kanji">魚</span><span class="pos-tag pos-noun">名詞</span><span class="translation">魚</span><button class="speak-btn" data-text="さかな">🔊</button></div>
    <div class="vocab-row"><span class="word">やさい</span><span class="kanji">野菜</span><span class="pos-tag pos-noun">名詞</span><span class="translation">蔬菜</span><button class="speak-btn" data-text="やさい">🔊</button></div>
    <div class="vocab-row"><span class="word">くだもの</span><span class="kanji">果物</span><span class="pos-tag pos-noun">名詞</span><span class="translation">水果</span><button class="speak-btn" data-text="くだもの">🔊</button></div>
    <div class="vocab-row"><span class="word">みず</span><span class="kanji">水</span><span class="pos-tag pos-noun">名詞</span><span class="translation">水</span><button class="speak-btn" data-text="みず">🔊</button></div>
    <div class="vocab-row"><span class="word">おちゃ</span><span class="kanji">お茶</span><span class="pos-tag pos-noun">名詞</span><span class="translation">茶</span><button class="speak-btn" data-text="おちゃ">🔊</button></div>
    <div class="vocab-row"><span class="word">こうちゃ</span><span class="kanji">紅茶</span><span class="pos-tag pos-noun">名詞</span><span class="translation">紅茶</span><button class="speak-btn" data-text="こうちゃ">🔊</button></div>
    <div class="vocab-row"><span class="word">ぎゅうにゅう/ミルク</span><span class="kanji">牛乳</span><span class="pos-tag pos-noun">名詞</span><span class="translation">牛奶</span><button class="speak-btn" data-text="ぎゅうにゅう">🔊</button></div>
    <div class="vocab-row"><span class="word">ジュース</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">果汁</span><button class="speak-btn" data-text="ジュース">🔊</button></div>
    <div class="vocab-row"><span class="word">ビール</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">啤酒</span><button class="speak-btn" data-text="ビール">🔊</button></div>
    <div class="vocab-row"><span class="word">おさけ</span><span class="kanji">お酒</span><span class="pos-tag pos-noun">名詞</span><span class="translation">酒</span><button class="speak-btn" data-text="おさけ">🔊</button></div>
    <div class="vocab-row"><span class="word">しゃしん</span><span class="kanji">写真</span><span class="pos-tag pos-noun">名詞</span><span class="translation">照片</span><button class="speak-btn" data-text="しゃしん">🔊</button></div>
    <div class="vocab-row"><span class="word">みせ</span><span class="kanji">店</span><span class="pos-tag pos-noun">名詞</span><span class="translation">店</span><button class="speak-btn" data-text="みせ">🔊</button></div>
    <div class="vocab-row"><span class="word">にわ</span><span class="kanji">庭</span><span class="pos-tag pos-noun">名詞</span><span class="translation">庭院</span><button class="speak-btn" data-text="にわ">🔊</button></div>
    <div class="vocab-row"><span class="word">しゅくだい</span><span class="kanji">宿題</span><span class="pos-tag pos-noun">名詞</span><span class="translation">作業</span><button class="speak-btn" data-text="しゅくだい">🔊</button></div>
    <div class="vocab-row"><span class="word">てがみ</span><span class="kanji">手紙</span><span class="pos-tag pos-noun">名詞</span><span class="translation">信</span><button class="speak-btn" data-text="てがみ">🔊</button></div>
    <div class="vocab-row"><span class="word">レポート</span><span class="kanji"></span><span class="pos-tag pos-noun">名詞</span><span class="translation">報告</span><button class="speak-btn" data-text="レポート">🔊</button></div>
    <div class="vocab-row"><span class="word">なに</span><span class="kanji">何</span><span class="pos-tag pos-other">疑問詞</span><span class="translation">什麼</span><button class="speak-btn" data-text="なに">🔊</button></div>
    <div class="vocab-row"><span class="word">いっしょに</span><span class="kanji">一緒に</span><span class="pos-tag pos-adv">副詞</span><span class="translation">一起</span><button class="speak-btn" data-text="いっしょに">🔊</button></div>
    <div class="vocab-row"><span class="word">ちょっと</span><span class="kanji"></span><span class="pos-tag pos-adv">副詞</span><span class="translation">稍微、一點</span><button class="speak-btn" data-text="ちょっと">🔊</button></div>
    <div class="vocab-row"><span class="word">いつも</span><span class="kanji"></span><span class="pos-tag pos-adv">副詞</span><span class="translation">總是</span><button class="speak-btn" data-text="いつも">🔊</button></div>
    <div class="vocab-row"><span class="word">ときどき</span><span class="kanji">時々</span><span class="pos-tag pos-adv">副詞</span><span class="translation">有時候</span><button class="speak-btn" data-text="ときどき">🔊</button></div>
    <div class="vocab-row"><span class="word">それから</span><span class="kanji"></span><span class="pos-tag pos-other">接續</span><span class="translation">然後</span><button class="speak-btn" data-text="それから">🔊</button></div>
    <div class="vocab-row"><span class="word">いいですね</span><span class="kanji"></span><span class="pos-tag pos-greeting">應答</span><span class="translation">好啊</span><button class="speak-btn" data-text="いいですね">🔊</button></div>
  </div>

  <div class="grammar-section">
    <div class="section-title">文型</div>

    <div class="grammar-card">
      <div class="pattern">N を V ます</div>
      <div class="pattern-desc">做某事（用助詞「を」標記受詞）</div>
      <div class="example">
        <div class="jp"><span>パンを たべます。</span><button class="speak-btn" data-text="パンを たべます">🔊</button></div>
        <div class="zh">吃麵包。</div>
      </div>
      <div class="example">
        <div class="jp"><span>コーヒーを のみます。</span><button class="speak-btn" data-text="コーヒーを のみます">🔊</button></div>
        <div class="zh">喝咖啡。</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">N で V ます</div>
      <div class="pattern-desc">在某地做某事（用助詞「で」標記地點）</div>
      <div class="example">
        <div class="jp"><span>えきで しんぶんを かいます。</span><button class="speak-btn" data-text="えきで しんぶんを かいます">🔊</button></div>
        <div class="zh">在車站買報紙。</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">V ませんか</div>
      <div class="pattern-desc">要不要一起做某事？（邀請）</div>
      <div class="example">
        <div class="jp"><span>いっしょに きょうとへ いきませんか。</span><button class="speak-btn" data-text="いっしょに きょうとへ いきませんか">🔊</button></div>
        <div class="zh">要不要一起去京都？</div>
      </div>
    </div>

    <div class="grammar-card">
      <div class="pattern">V ましょう</div>
      <div class="pattern-desc">一起做某事吧（提議）</div>
      <div class="example">
        <div class="jp"><span>ちょっと やすみましょう。</span><button class="speak-btn" data-text="ちょっと やすみましょう">🔊</button></div>
        <div class="zh">稍微休息一下吧。</div>
      </div>
      <table class="variation-table">
        <tr><td class="label-cell">肯定</td><td>V <span class="positive">ます</span></td></tr>
        <tr><td class="label-cell">否定</td><td>V <span class="negative">ません</span></td></tr>
        <tr><td class="label-cell">邀請</td><td>V <span class="question">ませんか</span></td></tr>
        <tr><td class="label-cell">提議</td><td>V <span class="positive">ましょう</span></td></tr>
      </table>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Verify in browser**

Verify all 6 lessons display. Sidebar links should scroll to each lesson.

---

## Chunk 2: JavaScript Interactivity

### Task 6: Add translation show/hide functionality

**Files:**
- Modify: `/Users/kai/Desktop/minna-no-nihongo/index.html` (add `<script>` before `</body>`)

**Important:** The `<script>` tag must be placed right before `</body>`, after all HTML content, so all DOM elements exist when the JavaScript runs.

- [ ] **Step 1: Add translation toggle JavaScript**

Add before `</body>`:

```html
<script>
// === Translation Toggle ===
document.addEventListener('click', function(e) {
  // Toggle individual translation
  if (e.target.classList.contains('translation') || e.target.classList.contains('zh')) {
    e.target.classList.toggle('hidden');
  }
});

document.getElementById('btn-hide-all').addEventListener('click', function() {
  document.querySelectorAll('.translation, .zh').forEach(function(el) {
    el.classList.add('hidden');
  });
});

document.getElementById('btn-show-all').addEventListener('click', function() {
  document.querySelectorAll('.translation, .zh').forEach(function(el) {
    el.classList.remove('hidden');
  });
});
</script>
```

- [ ] **Step 2: Verify in browser**

- Click a translation text → it blurs
- Click again → it shows
- Click 隱藏全部翻譯 → all translations blur
- Click 顯示全部翻譯 → all translations show

---

### Task 7: Add speech synthesis

**Files:**
- Modify: `/Users/kai/Desktop/minna-no-nihongo/index.html` (append to `<script>`)

- [ ] **Step 1: Add speech synthesis JavaScript**

Append inside the existing `<script>`:

```javascript
// === Speech Synthesis ===
var speechSupported = 'speechSynthesis' in window;

if (!speechSupported) {
  document.querySelectorAll('.speak-btn').forEach(function(btn) {
    btn.style.display = 'none';
  });
}

document.addEventListener('click', function(e) {
  var btn = e.target.closest('.speak-btn');
  if (!btn || !speechSupported) return;

  var text = btn.getAttribute('data-text');
  if (!text) return;

  window.speechSynthesis.cancel();
  var utterance = new SpeechSynthesisUtterance(text);
  utterance.lang = 'ja-JP';
  utterance.rate = 0.8;
  window.speechSynthesis.speak(utterance);
});
```

- [ ] **Step 2: Verify in browser**

Click any 🔊 button → should hear Japanese pronunciation.

---

### Task 8: Add search/filter functionality

**Files:**
- Modify: `/Users/kai/Desktop/minna-no-nihongo/index.html` (append to `<script>`)

- [ ] **Step 1: Add search JavaScript**

Append inside the existing `<script>`:

```javascript
// === Search/Filter ===
var searchInput = document.getElementById('search-input');

searchInput.addEventListener('input', function() {
  var query = this.value.trim().toLowerCase();

  // Remove existing highlights
  document.querySelectorAll('mark').forEach(function(mark) {
    var parent = mark.parentNode;
    parent.replaceChild(document.createTextNode(mark.textContent), mark);
    parent.normalize();
  });

  var lessons = document.querySelectorAll('.lesson');

  if (!query) {
    // Show all lessons when search is empty
    lessons.forEach(function(lesson) {
      lesson.style.display = '';
      lesson.querySelectorAll('.vocab-row, .grammar-card').forEach(function(item) {
        item.style.display = '';
      });
    });
    return;
  }

  lessons.forEach(function(lesson) {
    var hasMatch = false;

    // Search vocab rows
    lesson.querySelectorAll('.vocab-row').forEach(function(row) {
      var word = row.querySelector('.word').textContent.toLowerCase();
      var kanji = row.querySelector('.kanji').textContent.toLowerCase();
      var trans = row.querySelector('.translation').textContent.toLowerCase();
      var match = word.includes(query) || kanji.includes(query) || trans.includes(query);
      row.style.display = match ? '' : 'none';
      if (match) {
        hasMatch = true;
        highlightText(row.querySelector('.word'), query);
        highlightText(row.querySelector('.kanji'), query);
        highlightText(row.querySelector('.translation'), query);
      }
    });

    // Search grammar cards
    lesson.querySelectorAll('.grammar-card').forEach(function(card) {
      var text = card.textContent.toLowerCase();
      var match = text.includes(query);
      card.style.display = match ? '' : 'none';
      if (match) hasMatch = true;
    });

    // Hide entire lesson if no matches
    lesson.style.display = hasMatch ? '' : 'none';
  });
});

function highlightText(element, query) {
  var text = element.textContent;
  var lower = text.toLowerCase();
  var idx = lower.indexOf(query);
  if (idx === -1) return;

  var before = text.substring(0, idx);
  var match = text.substring(idx, idx + query.length);
  var after = text.substring(idx + query.length);

  element.innerHTML = '';
  if (before) element.appendChild(document.createTextNode(before));
  var mark = document.createElement('mark');
  mark.textContent = match;
  element.appendChild(mark);
  if (after) element.appendChild(document.createTextNode(after));
}
```

- [ ] **Step 2: Verify in browser**

- Type "たべ" → only Lesson 6 shows, 食べます is highlighted
- Type "老師" → Lesson 1 shows, 老師 is highlighted in translation
- Clear search → all lessons reappear

---

### Task 9: Add scroll navigation

**Files:**
- Modify: `/Users/kai/Desktop/minna-no-nihongo/index.html` (append to `<script>`)

- [ ] **Step 1: Add scroll tracking and smooth navigation JavaScript**

Append inside the existing `<script>`:

```javascript
// === Scroll Navigation ===

// Smooth scroll when clicking sidebar links
document.querySelectorAll('.sidebar-nav a').forEach(function(link) {
  link.addEventListener('click', function(e) {
    e.preventDefault();
    var targetId = this.getAttribute('href').substring(1);
    var target = document.getElementById(targetId);
    if (target) {
      target.scrollIntoView({ behavior: 'smooth' });
    }
    // Close mobile menu
    document.getElementById('sidebar').classList.remove('open');
  });
});

// Highlight current lesson in sidebar on scroll
var observer = new IntersectionObserver(function(entries) {
  entries.forEach(function(entry) {
    if (entry.isIntersecting) {
      var id = entry.target.id;
      document.querySelectorAll('.sidebar-nav a').forEach(function(a) {
        a.classList.toggle('active', a.getAttribute('href') === '#' + id);
      });
    }
  });
}, { rootMargin: '-20% 0px -70% 0px' });

document.querySelectorAll('.lesson').forEach(function(lesson) {
  observer.observe(lesson);
});

// Mobile menu toggle
document.getElementById('menu-toggle').addEventListener('click', function() {
  document.getElementById('sidebar').classList.toggle('open');
});
```

- [ ] **Step 2: Verify in browser**

- Click sidebar links → smooth scroll to lesson
- Scroll manually → sidebar highlights current lesson
- Resize to < 768px → sidebar collapses, hamburger menu works

---

### Task 10: Final verification

- [ ] **Step 1: Full functionality check**

Open `index.html` in browser and verify ALL features:

1. **Layout**: Sidebar on left, main content scrollable on right
2. **Content**: All 6 lessons display with vocabulary and grammar
3. **Vocab styling**: Part-of-speech tags have correct colors
4. **Grammar cards**: White cards with formulas, examples, variation tables
5. **Translation toggle**: Click individual → blurs/shows. Sidebar buttons → bulk toggle
6. **Speech**: 🔊 buttons speak Japanese at 0.8x speed
7. **Search**: Typing filters across all lessons, highlights matches
8. **Navigation**: Sidebar links smooth-scroll, scroll tracking highlights active lesson
9. **Responsive**: At < 768px, sidebar becomes top hamburger menu

- [ ] **Step 2: Commit**

```bash
cd /Users/kai/Desktop/minna-no-nihongo
git init
git add index.html docs/superpowers/specs/ docs/superpowers/plans/
git commit -m "feat: みんなの日本語 初級1 第1-6課 單字與句型視覺化整理

Interactive single-page HTML study tool with:
- Vocabulary with part-of-speech tags and Chinese translations
- Grammar patterns with formulas, examples, and variation tables
- Click-to-hide translations for self-testing
- Japanese speech synthesis (Web Speech API)
- Cross-lesson search with text highlighting
- Smooth scroll navigation with active lesson tracking
- Responsive design for mobile
- Design spec and implementation plan docs

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```
