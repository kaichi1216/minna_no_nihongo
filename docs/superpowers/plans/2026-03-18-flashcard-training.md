# Flashcard 單字互動訓練 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add per-lesson flashcard vocabulary training to the Minna no Nihongo learning site, with localStorage-based progress tracking.

**Architecture:** Each of the two volume files (`shokyuu1/index.html`, `shokyuu2/index.html`) gets identical CSS and JS additions. The JS dynamically generates the flashcard UI from existing DOM vocab data, using event delegation. No new files are created.

**Tech Stack:** Vanilla HTML/CSS/JS, localStorage, Web Speech API (existing)

**Spec:** `docs/superpowers/specs/2026-03-18-flashcard-training-design.md`

---

## File Map

Both modifications are identical in structure:

- **Modify:** `shokyuu1/index.html`
  - CSS: insert before `</style>` (line 300)
  - HTML: insert practice button before each `<div class="section-title">単語</div>` (25 lessons)
  - JS: insert before `</script>` (line 2969)
- **Modify:** `shokyuu2/index.html`
  - CSS: insert before `</style>` (line 300)
  - HTML: insert practice button before each `<div class="section-title">単語</div>` (25 lessons)
  - JS: insert before `</script>` (line 3049)

---

### Task 1: Add Flashcard CSS to shokyuu1

**Files:**
- Modify: `shokyuu1/index.html:297-300` (insert before `</style>`)

- [ ] **Step 1: Add all flashcard-related CSS**

Insert the following CSS before the `</style>` tag (after the `@media (min-width: 769px)` block at line 299):

```css
/* === Flashcard Practice === */
.practice-btn-wrapper {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  gap: 10px;
}
.practice-btn {
  padding: 8px 20px;
  background: #c0392b;
  color: white;
  border: none;
  border-radius: 20px;
  font-size: 13px;
  cursor: pointer;
  transition: background 0.2s;
}
.practice-btn:hover { background: #a93226; }
.practice-last-score {
  font-size: 12px;
  color: #888;
}

.practice-area {
  display: none;
  margin-bottom: 24px;
}
.practice-area.active { display: block; }

.practice-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
  flex-wrap: wrap;
  gap: 8px;
}
.practice-header-left {
  display: flex;
  align-items: center;
  gap: 8px;
}
.practice-header-left .practice-label {
  font-size: 13px;
  color: #c0392b;
  font-weight: bold;
}
.practice-header-left .practice-direction {
  font-size: 12px;
  color: #888;
}
.practice-header-right {
  display: flex;
  gap: 8px;
}
.practice-header-right button {
  padding: 4px 12px;
  font-size: 11px;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.2s, color 0.2s;
}
.practice-switch-btn {
  border: 1px solid #c0392b;
  background: transparent;
  color: #c0392b;
}
.practice-switch-btn:hover { background: #c0392b; color: white; }
.practice-end-btn {
  border: 1px solid #999;
  background: transparent;
  color: #666;
}
.practice-end-btn:hover { background: #999; color: white; }

.practice-progress {
  margin-bottom: 20px;
}
.practice-progress-bar {
  background: #e8ddd0;
  border-radius: 10px;
  height: 6px;
  overflow: hidden;
}
.practice-progress-fill {
  height: 100%;
  background: #c0392b;
  border-radius: 10px;
  transition: width 0.3s;
}
.practice-progress-text {
  text-align: center;
  font-size: 12px;
  color: #888;
  margin-top: 6px;
}

.flashcard {
  background: white;
  border: 2px solid #e8ddd0;
  border-radius: 12px;
  padding: 48px 32px;
  text-align: center;
  margin: 0 auto 20px;
  max-width: 400px;
  cursor: pointer;
  min-height: 160px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  transition: box-shadow 0.2s;
  user-select: none;
  position: relative;
}
.flashcard:hover { box-shadow: 0 4px 16px rgba(0,0,0,0.06); }
.flashcard .fc-main {
  font-size: 32px;
  font-weight: bold;
  color: #333;
  margin-bottom: 8px;
}
.flashcard .fc-sub {
  font-size: 16px;
  color: #888;
}
.flashcard .fc-hint {
  font-size: 12px;
  color: #bbb;
  margin-top: 16px;
}
.flashcard .fc-front,
.flashcard .fc-answer {
  transition: opacity 0.25s;
}
.flashcard .fc-answer {
  opacity: 0;
  position: absolute;
}
.flashcard.flipped .fc-front { opacity: 0; position: absolute; }
.flashcard.flipped .fc-answer { opacity: 1; position: static; }
.flashcard.flipped .fc-hint { display: none; }

.practice-actions {
  display: none;
  gap: 12px;
  justify-content: center;
  margin-bottom: 20px;
}
.practice-actions.visible { display: flex; }
.practice-actions button {
  flex: 1;
  max-width: 150px;
  padding: 12px;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 14px;
  cursor: pointer;
  transition: opacity 0.2s;
}
.practice-actions button:hover { opacity: 0.85; }
.practice-actions .btn-unknown { background: #e74c3c; }
.practice-actions .btn-known { background: #27ae60; }

.practice-result {
  text-align: center;
  padding: 20px 0;
}
.practice-result h3 {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 16px;
}
.practice-result-stats {
  display: flex;
  justify-content: center;
  gap: 32px;
  margin-bottom: 20px;
}
.practice-result-stats .stat-num {
  font-size: 28px;
  font-weight: bold;
}
.practice-result-stats .stat-num.known { color: #27ae60; }
.practice-result-stats .stat-num.unknown { color: #e74c3c; }
.practice-result-stats .stat-num.rate { color: #333; }
.practice-result-stats .stat-label {
  font-size: 12px;
  color: #888;
}
.practice-result-actions {
  display: flex;
  gap: 12px;
  justify-content: center;
  flex-wrap: wrap;
}
.practice-result-actions button {
  padding: 10px 20px;
  border-radius: 20px;
  font-size: 13px;
  cursor: pointer;
  transition: background 0.2s, color 0.2s;
}
.practice-retry-btn {
  background: #c0392b;
  color: white;
  border: none;
}
.practice-retry-btn:hover { background: #a93226; }
.practice-back-btn {
  background: transparent;
  color: #666;
  border: 1px solid #ddd;
}
.practice-back-btn:hover { background: #f0f0f0; }

@media (max-width: 768px) {
  .flashcard {
    max-width: 100%;
    padding: 36px 20px;
  }
  .flashcard .fc-main { font-size: 26px; }
  .practice-actions button { max-width: none; }
  .practice-result-actions { flex-direction: column; align-items: center; }
  .practice-result-actions button { width: 80%; max-width: 280px; }
}
```

- [ ] **Step 2: Verify by opening the file in browser**

Open `shokyuu1/index.html` in browser. The page should look identical to before (no visible changes yet, CSS only applies to elements that don't exist yet).

- [ ] **Step 3: Commit**

```bash
git add shokyuu1/index.html
git commit -m "feat(shokyuu1): add flashcard practice CSS styles"
```

---

### Task 2: Add practice buttons to all 25 lessons in shokyuu1

**Files:**
- Modify: `shokyuu1/index.html` — insert HTML before each `<div class="section-title">単語</div>`

- [ ] **Step 1: Insert practice button wrapper before every 単語 section title**

For each of the 25 lessons, insert the following HTML immediately before `<div class="section-title">単語</div>`:

```html
  <div class="practice-btn-wrapper"><button class="practice-btn">&#128221; 開始練習</button><span class="practice-last-score"></span></div>
```

The pattern to find-and-replace (in every lesson):

**Before:**
```html
  <div class="section-title">単語</div>
```

**After:**
```html
  <div class="practice-btn-wrapper"><button class="practice-btn">&#128221; 開始練習</button><span class="practice-last-score"></span></div>
  <div class="section-title">単語</div>
```

There are exactly 25 occurrences in `shokyuu1/index.html` (one per lesson). Use `replace_all` on the Edit tool.

- [ ] **Step 2: Verify buttons appear**

Open `shokyuu1/index.html` in browser. Each lesson should now show a red "開始練習" button above the 単語 heading. The buttons don't do anything yet.

- [ ] **Step 3: Commit**

```bash
git add shokyuu1/index.html
git commit -m "feat(shokyuu1): add practice buttons to all 25 lessons"
```

---

### Task 3: Add Flashcard JavaScript to shokyuu1

**Files:**
- Modify: `shokyuu1/index.html` — insert JS before `</script>` (line 2969 area, will have shifted due to Task 2 insertions)

- [ ] **Step 1: Add all flashcard JavaScript**

Insert the following JavaScript before the closing `</script>` tag:

```javascript
// === Flashcard Practice ===
(function() {
  // Fisher-Yates shuffle
  function shuffle(arr) {
    for (var i = arr.length - 1; i > 0; i--) {
      var j = Math.floor(Math.random() * (i + 1));
      var tmp = arr[i]; arr[i] = arr[j]; arr[j] = tmp;
    }
    return arr;
  }

  function getWordKey(word) {
    return word.kana + '|' + word.translation;
  }

  function loadRecord(lessonId) {
    try {
      var data = localStorage.getItem('minna-practice-' + lessonId);
      return data ? JSON.parse(data) : null;
    } catch(e) { return null; }
  }

  function saveRecord(lessonId, words) {
    try {
      localStorage.setItem('minna-practice-' + lessonId, JSON.stringify({
        lastDate: new Date().toISOString().slice(0, 10),
        words: words
      }));
    } catch(e) { /* QuotaExceededError — silent fail */ }
  }

  function extractWords(lesson) {
    var words = [];
    lesson.querySelectorAll('.vocab-row').forEach(function(row) {
      words.push({
        kana: row.querySelector('.word').textContent,
        kanji: row.querySelector('.kanji').textContent,
        translation: row.querySelector('.translation').textContent
      });
    });
    return words;
  }

  function updateLastScore(lesson) {
    var lessonId = lesson.id;
    var record = loadRecord(lessonId);
    var scoreEl = lesson.querySelector('.practice-last-score');
    if (!scoreEl) return;
    if (!record || !record.words) { scoreEl.textContent = ''; return; }
    var total = 0, known = 0;
    for (var k in record.words) {
      total++;
      if (record.words[k]) known++;
    }
    if (total > 0) {
      scoreEl.textContent = '上次：' + known + '/' + total + ' 認識';
    }
  }

  function startPractice(lesson, wordSubset) {
    var lessonId = lesson.id;
    var lessonNum = lesson.querySelector('.lesson-number').textContent;
    var allWords = extractWords(lesson);
    var words = wordSubset || allWords;
    words = shuffle(words.slice());

    // Hide lesson content except header and practice area
    Array.prototype.forEach.call(lesson.children, function(child) {
      if (!child.classList.contains('lesson-header') && !child.classList.contains('practice-area')) {
        child.setAttribute('data-practice-hidden', '');
        child.style.display = 'none';
      }
    });

    var area = lesson.querySelector('.practice-area');
    if (!area) {
      area = document.createElement('div');
      area.className = 'practice-area';
      lesson.appendChild(area);
    }
    area.classList.add('active');

    var currentIndex = 0;
    var direction = 'jp2zh'; // 'jp2zh' or 'zh2jp'
    var results = {}; // key -> true/false

    function renderCard() {
      if (currentIndex >= words.length) {
        renderResult();
        return;
      }
      var word = words[currentIndex];
      var dirLabel = direction === 'jp2zh' ? '日文 → 中文' : '中文 → 日文';

      var front, frontSub, answer, answerSub;
      if (direction === 'jp2zh') {
        front = word.kana;
        frontSub = word.kanji || '';
        answer = word.translation;
        answerSub = '';
      } else {
        front = word.translation;
        frontSub = '';
        answer = word.kana;
        answerSub = word.kanji || '';
      }

      area.innerHTML =
        '<div class="practice-header">' +
          '<div class="practice-header-left">' +
            '<span class="practice-label">' + lessonNum + ' 練習</span>' +
            '<span class="practice-direction">' + dirLabel + '</span>' +
          '</div>' +
          '<div class="practice-header-right">' +
            '<button class="practice-switch-btn">切換方向</button>' +
            '<button class="practice-end-btn">✕ 結束</button>' +
          '</div>' +
        '</div>' +
        '<div class="practice-progress">' +
          '<div class="practice-progress-bar"><div class="practice-progress-fill" style="width:' + Math.round(currentIndex / words.length * 100) + '%"></div></div>' +
          '<div class="practice-progress-text">第 ' + (currentIndex + 1) + ' / ' + words.length + ' 張</div>' +
        '</div>' +
        '<div class="flashcard">' +
          '<div class="fc-front">' +
            '<div class="fc-main">' + front + '</div>' +
            (frontSub ? '<div class="fc-sub">' + frontSub + '</div>' : '') +
          '</div>' +
          '<div class="fc-answer">' +
            '<div class="fc-main">' + answer + '</div>' +
            (answerSub ? '<div class="fc-sub">' + answerSub + '</div>' : '') +
          '</div>' +
          '<div class="fc-hint">&#128070; 點擊翻牌</div>' +
        '</div>' +
        '<div class="practice-actions">' +
          '<button class="btn-unknown">&#10060; 不認識</button>' +
          '<button class="btn-known">&#9989; 認識</button>' +
        '</div>';

      // Event: flip card
      var card = area.querySelector('.flashcard');
      card.addEventListener('click', function() {
        if (!card.classList.contains('flipped')) {
          card.classList.add('flipped');
          area.querySelector('.practice-actions').classList.add('visible');
        }
      });

      // Event: known / unknown
      area.querySelector('.btn-known').addEventListener('click', function() {
        results[getWordKey(words[currentIndex])] = true;
        currentIndex++;
        renderCard();
      });
      area.querySelector('.btn-unknown').addEventListener('click', function() {
        results[getWordKey(words[currentIndex])] = false;
        currentIndex++;
        renderCard();
      });

      // Event: switch direction
      area.querySelector('.practice-switch-btn').addEventListener('click', function() {
        direction = direction === 'jp2zh' ? 'zh2jp' : 'jp2zh';
        var dirEl = area.querySelector('.practice-direction');
        dirEl.textContent = direction === 'jp2zh' ? '日文 → 中文' : '中文 → 日文';
      });

      // Event: end practice
      area.querySelector('.practice-end-btn').addEventListener('click', function() {
        endPractice();
      });
    }

    function renderResult() {
      var knownCount = 0, unknownCount = 0;
      var unknownWords = [];
      for (var i = 0; i < words.length; i++) {
        var key = getWordKey(words[i]);
        if (results[key]) {
          knownCount++;
        } else {
          unknownCount++;
          unknownWords.push(words[i]);
        }
      }
      var rate = words.length > 0 ? Math.round(knownCount / words.length * 100) : 0;

      // Merge results into full record
      var record = loadRecord(lessonId);
      var fullWords = (record && record.words) ? record.words : {};
      // If this was a full practice, replace all; if retry, merge
      if (!wordSubset) {
        fullWords = results;
      } else {
        for (var k in results) {
          fullWords[k] = results[k];
        }
      }
      saveRecord(lessonId, fullWords);

      var retryBtn = unknownCount > 0
        ? '<button class="practice-retry-btn">&#128260; 只練不認識的 (' + unknownCount + ')</button>'
        : '';

      area.innerHTML =
        '<div class="practice-result">' +
          '<h3>練習完成！</h3>' +
          '<div class="practice-result-stats">' +
            '<div><div class="stat-num known">' + knownCount + '</div><div class="stat-label">認識</div></div>' +
            '<div><div class="stat-num unknown">' + unknownCount + '</div><div class="stat-label">不認識</div></div>' +
            '<div><div class="stat-num rate">' + rate + '%</div><div class="stat-label">正確率</div></div>' +
          '</div>' +
          '<div class="practice-result-actions">' +
            retryBtn +
            '<button class="practice-back-btn">&#128203; 回到單字列表</button>' +
          '</div>' +
        '</div>';

      // Event: retry unknown
      var retryEl = area.querySelector('.practice-retry-btn');
      if (retryEl) {
        retryEl.addEventListener('click', function() {
          startPractice(lesson, unknownWords);
        });
      }

      // Event: back to list
      area.querySelector('.practice-back-btn').addEventListener('click', function() {
        endPractice();
      });

      updateLastScore(lesson);
    }

    function endPractice() {
      area.classList.remove('active');
      area.innerHTML = '';
      Array.prototype.forEach.call(lesson.children, function(child) {
        if (child.hasAttribute('data-practice-hidden')) {
          child.removeAttribute('data-practice-hidden');
          child.style.display = '';
        }
      });
    }

    renderCard();
  }

  // Event delegation for practice buttons
  document.addEventListener('click', function(e) {
    var btn = e.target.closest('.practice-btn');
    if (!btn) return;
    var lesson = btn.closest('.lesson');
    if (!lesson) return;
    startPractice(lesson, null);
  });

  // Initialize last scores on page load
  document.querySelectorAll('.lesson').forEach(function(lesson) {
    updateLastScore(lesson);
  });
})();
```

- [ ] **Step 2: Test the complete flashcard flow in browser**

Open `shokyuu1/index.html` in browser. For lesson 1:
1. Click "開始練習" — vocab list hides, flashcard appears with first random word
2. Click the card — it flips to show the answer
3. Click "認識" or "不認識" — advances to next card
4. Click "切換方向" — direction label changes, next card shows in new direction
5. Complete all cards — result screen shows with stats
6. Click "只練不認識的" — restarts with only unknown words
7. Click "回到單字列表" — vocab list reappears
8. The "上次" score appears next to the button
9. Refresh the page — score persists from localStorage

- [ ] **Step 3: Commit**

```bash
git add shokyuu1/index.html
git commit -m "feat(shokyuu1): add flashcard practice JavaScript logic"
```

---

### Task 4: Apply identical changes to shokyuu2

**Files:**
- Modify: `shokyuu2/index.html`
  - CSS: insert before `</style>` (line 300)
  - HTML: insert practice buttons before each `<div class="section-title">単語</div>` (25 lessons)
  - JS: insert before `</script>`

- [ ] **Step 1: Add the same CSS block to shokyuu2**

Insert the exact same CSS block from Task 1 before `</style>` in `shokyuu2/index.html`.

- [ ] **Step 2: Add practice buttons to all 25 lessons in shokyuu2**

Same as Task 2 — use `replace_all` on the Edit tool to insert the practice button wrapper before every `<div class="section-title">単語</div>`.

- [ ] **Step 3: Add the same JavaScript block to shokyuu2**

Insert the exact same JS block from Task 3 before `</script>` in `shokyuu2/index.html`.

- [ ] **Step 4: Test in browser**

Open `shokyuu2/index.html` in browser. Repeat the same manual test as Task 3 Step 2, using lesson 26. Verify:
1. Flashcard flow works end to end
2. localStorage key is `minna-practice-lesson26` (not `lesson1`)
3. Last score displays correctly

- [ ] **Step 5: Commit**

```bash
git add shokyuu2/index.html
git commit -m "feat(shokyuu2): add flashcard practice (CSS, buttons, JS)"
```

---

### Task 5: Cross-volume verification and cleanup

- [ ] **Step 1: Verify localStorage isolation**

Open both `shokyuu1/index.html` and `shokyuu2/index.html`. Complete a practice in lesson 1 and lesson 26. Open browser DevTools → Application → localStorage. Confirm:
- `minna-practice-lesson1` exists with correct data
- `minna-practice-lesson26` exists with correct data
- Keys use `kana|translation` composite format

- [ ] **Step 2: Verify responsive design**

Use browser DevTools to toggle mobile view (375px width). Confirm:
- Flashcard fills full width
- Buttons are comfortably tappable
- Progress bar and result screen layout correctly

- [ ] **Step 3: Add .superpowers to .gitignore if not present**

Check if `.gitignore` exists and contains `.superpowers/`. If not, add it.

```bash
echo ".superpowers/" >> .gitignore
git add .gitignore
git commit -m "chore: add .superpowers to gitignore"
```

- [ ] **Step 4: Final commit (if any cleanup needed)**

Verify `git status` is clean. If any remaining changes, commit them.
