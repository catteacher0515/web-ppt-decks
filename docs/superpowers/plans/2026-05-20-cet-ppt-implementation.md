# CET Resource Deck Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the approved 8-slide single-file HTML swipe deck in `ppt/index.html`, rename the screenshot assets to semantic filenames, and ship a browsable first version that matches the signed-off CET design spec.

**Architecture:** Use `guizang-ppt-skill` style A by copying `assets/template.html` into `ppt/index.html`, keeping the default `🖋 墨水经典` theme, and replacing `<!-- SLIDES_HERE -->` with eight hand-authored `<section>` blocks. Keep screenshot "light packaging" in HTML through the template's frame components instead of raster redrawing, so the original website UI stays readable while the deck remains visually consistent.

**Tech Stack:** Static HTML, template CSS/JS from `guizang-ppt-skill`, Git, shell validation, local HTTP preview

---

## File Structure

- `ppt/index.html`
  Single-file horizontal swipe deck. Holds the `<title>`, eight slide sections, and uses the existing template CSS/JS without adding new external files.
- `ppt/images/04-homepage.png`
  Renamed homepage screenshot used on the solution reveal slide.
- `ppt/images/05-paper-list.png`
  Renamed paper list screenshot used in the proof grid.
- `ppt/images/05-batch-download.png`
  Renamed batch download screenshot used in the proof grid.
- `ppt/images/05-site-philosophy.png`
  Renamed site philosophy screenshot used in the proof grid.
- `ppt/images/06-paper-preview.png`
  Renamed preview screenshot used on the preview/listening slide.

## Task 1: Scaffold the Deck and Normalize Screenshot Names

**Files:**
- Create: `ppt/index.html`
- Modify: `ppt/images/微信图片_20260520184548_1296_17.png`
- Modify: `ppt/images/微信图片_20260520184559_1297_17.png`
- Modify: `ppt/images/微信图片_20260520184608_1298_17.png`
- Modify: `ppt/images/微信图片_20260520184620_1299_17.png`
- Modify: `ppt/images/微信图片_20260520184630_1300_17.png`

- [ ] **Step 1: Verify the deck file is absent and the raw screenshots still exist**

```bash
test ! -f /Users/huapingyu/dev/guizang-test/ppt/index.html
printf '%s\n' /Users/huapingyu/dev/guizang-test/ppt/images/*.png
```

Expected:
- `test` exits `0`
- The printed filenames are the five `微信图片_*.png` screenshots

- [ ] **Step 2: Copy the approved template and rename the screenshots to semantic filenames**

```bash
cp /Users/huapingyu/.claude/skills/guizang-ppt-skill/assets/template.html \
  /Users/huapingyu/dev/guizang-test/ppt/index.html

mv '/Users/huapingyu/dev/guizang-test/ppt/images/微信图片_20260520184548_1296_17.png' \
  '/Users/huapingyu/dev/guizang-test/ppt/images/04-homepage.png'

mv '/Users/huapingyu/dev/guizang-test/ppt/images/微信图片_20260520184559_1297_17.png' \
  '/Users/huapingyu/dev/guizang-test/ppt/images/05-paper-list.png'

mv '/Users/huapingyu/dev/guizang-test/ppt/images/微信图片_20260520184608_1298_17.png' \
  '/Users/huapingyu/dev/guizang-test/ppt/images/05-batch-download.png'

mv '/Users/huapingyu/dev/guizang-test/ppt/images/微信图片_20260520184620_1299_17.png' \
  '/Users/huapingyu/dev/guizang-test/ppt/images/06-paper-preview.png'

mv '/Users/huapingyu/dev/guizang-test/ppt/images/微信图片_20260520184630_1300_17.png' \
  '/Users/huapingyu/dev/guizang-test/ppt/images/05-site-philosophy.png'
```

- [ ] **Step 3: Verify the scaffold is in place before writing slides**

```bash
rg -n '\[必填\]|<!-- SLIDES_HERE -->' /Users/huapingyu/dev/guizang-test/ppt/index.html
ls -1 /Users/huapingyu/dev/guizang-test/ppt/images
```

Expected:
- `rg` prints exactly two matches: the `<title>` placeholder and `<!-- SLIDES_HERE -->`
- `ls` prints:

```text
04-homepage.png
05-batch-download.png
05-paper-list.png
05-site-philosophy.png
06-paper-preview.png
```

- [ ] **Step 4: Commit the scaffold and normalized asset names**

```bash
git -C /Users/huapingyu/dev/guizang-test add ppt/index.html ppt/images
git -C /Users/huapingyu/dev/guizang-test commit -m "chore: scaffold CET deck and normalize image names"
```

## Task 2: Build the Deck Metadata and Slides 1-4

**Files:**
- Modify: `ppt/index.html`

- [ ] **Step 1: Confirm the deck still has no real title and no slides**

```bash
rg -n '<title>获取四六级真题，不要再在评论区刷 ×× 啦！</title>' \
  /Users/huapingyu/dev/guizang-test/ppt/index.html

rg -c '<section class="slide' \
  /Users/huapingyu/dev/guizang-test/ppt/index.html
```

Expected:
- The first `rg` returns no matches
- The second command prints `0`

- [ ] **Step 2: Replace the title placeholder and insert slides 1-4 plus a continuation marker**

Replace the template title:

```html
<title>获取四六级真题，不要再在评论区刷 ×× 啦！</title>
```

Replace `<!-- SLIDES_HERE -->` with:

```html
<section class="slide hero dark">
  <div class="chrome">
    <div>CET Papers · Resource Deck</div>
    <div>Vol.01</div>
  </div>
  <div class="frame" style="display:grid;gap:4vh;align-content:center;min-height:80vh">
    <div class="kicker" data-anim>花萍雨 · CET RESOURCE</div>
    <h1 class="h-hero" style="font-size:8.8vw" data-anim>获取四六级真题</h1>
    <h2 class="h-sub" data-anim>不要再在评论区刷 ×× 啦！</h2>
    <p class="lead" style="max-width:58vw" data-anim>
      真正想要的，不过是一份能直接拿到、能直接开始做的真题。
    </p>
    <div class="meta-row" data-anim>
      <span>花萍雨</span><span>·</span><span>CET4 / CET6 免费真题网站</span>
    </div>
  </div>
  <div class="foot">
    <div>一个更直接的真题获取方式</div>
    <div>— 2026 —</div>
  </div>
</section>

<section class="slide hero light">
  <div class="chrome">
    <div>痛点 · Pain Point</div>
    <div>01 / 08</div>
  </div>
  <div class="frame" style="display:grid;gap:7vh;align-content:center;min-height:80vh">
    <div class="kicker" data-anim>The Problem</div>
    <h1 class="h-hero" style="font-size:6.9vw;line-height:1.15">
      <span data-anim style="display:block">找一套真题，</span>
      <span data-anim style="display:block">为什么总要</span>
      <span data-anim style="display:block">多绕一步？</span>
    </h1>
    <p class="lead" style="max-width:56vw" data-anim>
      不是去评论区回复口令，就是先登录、先看广告、先跳转。
      真正想要的资源，反而总被中间流程拖慢。
    </p>
  </div>
  <div class="foot">
    <div>痛点不是没有资源，而是多走了一步</div>
    <div>Page 01</div>
  </div>
</section>

<section class="slide light" data-animate="directional">
  <div class="chrome">
    <div>旧方式 vs 新方式</div>
    <div>02 / 08</div>
  </div>
  <div class="frame" style="padding-top:5vh">
    <div class="kicker" data-anim>Before / After</div>
    <h2 class="h-xl" style="margin-bottom:4vh" data-anim>你要的是真题，不是流程。</h2>

    <div class="grid-2-6-6" style="gap:5vw 4vh">
      <div data-anim="left" style="padding:3vh 2vw;border-left:3px solid currentColor;opacity:.58">
        <div class="kicker" style="opacity:.9">Before · 常见路径</div>
        <h3 class="h-md" style="margin-top:2vh">先经过一堆中间步骤</h3>
        <ul style="margin-top:3vh;padding-left:1.2em;display:flex;flex-direction:column;gap:1.4vh;font-family:var(--sans-zh);font-size:max(14px,1.1vw);line-height:1.55">
          <li>评论区回复关键词</li>
          <li>登录注册之后再拿资源</li>
          <li>广告、网盘、跳转反复打断</li>
        </ul>
      </div>

      <div data-anim="right" style="padding:3vh 2vw;border-left:3px solid currentColor">
        <div class="kicker" style="opacity:.9">After · 这个网站</div>
        <h3 class="h-md" style="margin-top:2vh">直接给你真题本身</h3>
        <ul style="margin-top:3vh;padding-left:1.2em;display:flex;flex-direction:column;gap:1.4vh;font-family:var(--sans-zh);font-size:max(14px,1.1vw);line-height:1.55">
          <li>免费使用</li>
          <li>无广告打断</li>
          <li>免登录、免注册</li>
          <li>打开就能直接获取</li>
        </ul>
      </div>
    </div>
  </div>
  <div class="foot">
    <div>旧方式卖流程，新方式只给结果</div>
    <div>Page 02</div>
  </div>
</section>

<section class="slide dark">
  <div class="chrome">
    <div>方案 · Direct Access</div>
    <div>03 / 08</div>
  </div>
  <div class="frame grid-2-7-5" style="padding-top:6vh">
    <div style="display:flex;flex-direction:column;justify-content:space-between;gap:3vh">
      <div>
        <div class="kicker" data-anim>So I Built It</div>
        <h2 class="h-xl" style="font-size:6.6vw" data-anim>所以我做了一个更直接的网站。</h2>
        <p class="lead" style="margin-top:3vh" data-anim>
          一个免费、无广、免登录的四六级真题站。
          你不需要先完成一堆动作，再换一份真正想要的资源。
        </p>
      </div>

      <div class="callout" data-anim>
        四级：
        <span style="font-family:var(--mono);font-size:.86em;letter-spacing:0;text-transform:none;">
          https://catteacher0515.github.io/cet4-download/
        </span>
        <br>
        六级：
        <span style="font-family:var(--mono);font-size:.86em;letter-spacing:0;text-transform:none;">
          https://catteacher0515.github.io/cet6-download/
        </span>
        <div class="callout-src">Desktop first · Tablet / Mobile available</div>
      </div>
    </div>

    <figure class="frame-img r-16x10 fit-contain" data-anim>
      <img src="images/04-homepage.png" alt="四六级真题网站首页">
      <figcaption class="img-cap">首页 · 打开就能进入资源入口</figcaption>
    </figure>
  </div>
  <div class="foot">
    <div>先把网站拿出来，再谈为什么值得用</div>
    <div>Page 03</div>
  </div>
</section>

<!-- SLIDES_CONTINUE -->
```

- [ ] **Step 3: Verify the first half of the deck was inserted correctly**

```bash
rg -n '<title>获取四六级真题，不要再在评论区刷 ×× 啦！</title>' \
  /Users/huapingyu/dev/guizang-test/ppt/index.html

rg -c '<section class="slide' \
  /Users/huapingyu/dev/guizang-test/ppt/index.html

rg -n '04-homepage.png|The Problem|Before · 常见路径|SLIDES_CONTINUE' \
  /Users/huapingyu/dev/guizang-test/ppt/index.html
```

Expected:
- The title `rg` returns exactly one match
- The slide count prints `4`
- The final `rg` finds all four markers, confirming the right section landed

- [ ] **Step 4: Commit slides 1-4**

```bash
git -C /Users/huapingyu/dev/guizang-test add ppt/index.html
git -C /Users/huapingyu/dev/guizang-test commit -m "feat: add CET deck opening sequence"
```

## Task 3: Build Slides 5-8 and Close the Deck

**Files:**
- Modify: `ppt/index.html`

- [ ] **Step 1: Confirm only the first four slides exist before adding the back half**

```bash
rg -c '<section class="slide' \
  /Users/huapingyu/dev/guizang-test/ppt/index.html

rg -n 'SLIDES_CONTINUE' \
  /Users/huapingyu/dev/guizang-test/ppt/index.html
```

Expected:
- The slide count prints `4`
- `SLIDES_CONTINUE` still exists exactly once

- [ ] **Step 2: Replace the continuation marker with slides 5-8**

Replace `<!-- SLIDES_CONTINUE -->` with:

```html
<section class="slide light">
  <div class="chrome">
    <div>真实页面 · Proof</div>
    <div>04 / 08</div>
  </div>
  <div class="frame" style="padding-top:5vh">
    <div class="kicker" data-anim>Proof · 截图实证</div>
    <h2 class="h-xl" data-anim>不是一句口号，是直接可用。</h2>

    <div class="grid-3" style="margin-top:4vh;align-items:start">
      <figure class="frame-img h-26 fit-contain" data-anim>
        <img src="images/05-paper-list.png" alt="四六级真题列表页">
        <figcaption class="img-cap">真题列表 · 直接选年份与套题</figcaption>
      </figure>

      <figure class="frame-img h-26 fit-contain" data-anim>
        <img src="images/05-batch-download.png" alt="批量下载弹窗">
        <figcaption class="img-cap">批量下载 · 一次拿走需要的资源</figcaption>
      </figure>

      <figure class="frame-img h-26 fit-contain" data-anim>
        <img src="images/05-site-philosophy.png" alt="站点说明页">
        <figcaption class="img-cap">站点说明 · 免费、无广、免登录</figcaption>
      </figure>
    </div>
  </div>
  <div class="foot">
    <div>真实截图比口号更有说服力</div>
    <div>Page 04</div>
  </div>
</section>

<section class="slide dark">
  <div class="chrome">
    <div>预览与听力 · Preview</div>
    <div>05 / 08</div>
  </div>
  <div class="frame grid-2-8-4" style="padding-top:6vh">
    <div>
      <div class="kicker" data-anim>Preview · 不只是下载链接</div>
      <h2 class="h-xl" style="margin-top:1vh;margin-bottom:3vh" data-anim>可以先看，再决定怎么用。</h2>

      <p class="lead" style="margin-bottom:3vh" data-anim>
        点进单套真题之后，可以直接在网页里预览内容，
        不必先经历一轮跳转和拆文件。
      </p>

      <p data-anim style="font-family:var(--sans-zh);font-size:max(14px,1.15vw);line-height:1.75;opacity:.78;margin-bottom:2.4vh">
        如果你需要听力，页面也预留了对应入口。对临近考试的人来说，
        能更快开始做题，比“资源到底藏在哪里”更重要。
      </p>

      <div class="callout" style="margin-top:3vh" data-anim>
        重点不是把资源包装得多复杂，<br>
        而是让你少折腾一步，直接进入备考。
        <div class="callout-src">CET Preview · Fast Access</div>
      </div>
    </div>

    <figure class="frame-img r-16x10 fit-contain" data-anim>
      <img src="images/06-paper-preview.png" alt="真题预览页">
      <figcaption class="img-cap">真题预览 · 先看内容，再进入使用</figcaption>
    </figure>
  </div>
  <div class="foot">
    <div>下载之外，也要让使用本身更顺</div>
    <div>Page 05</div>
  </div>
</section>

<section class="slide light" data-animate="quote">
  <div class="chrome">
    <div>Takeaway · 金句</div>
    <div>06 / 08</div>
  </div>
  <div class="frame" style="display:grid;gap:5vh;align-content:center;min-height:80vh">
    <div class="kicker" data-anim>Quote · 核心一句</div>
    <blockquote style="font-family:var(--serif-zh);font-weight:700;font-size:5.6vw;line-height:1.2;letter-spacing:-.01em;max-width:72vw">
      <span data-anim="line" style="display:block">“如果你只是想</span>
      <span data-anim="line" style="display:block">快速拿到真题，</span>
      <span data-anim="line" style="display:block">这个站就是为你准备的。”</span>
    </blockquote>
    <p class="lead" style="max-width:55vw;opacity:.68" data-anim>
      少一点中间流程，多一点真正开始做题的时间。
    </p>
    <div class="meta-row" data-anim>
      <span>花萍雨</span><span>·</span><span>From the original article</span>
    </div>
  </div>
  <div class="foot">
    <div>把真正有用的话留给最后</div>
    <div>Page 06</div>
  </div>
</section>

<section class="slide hero light">
  <div class="chrome">
    <div>Resources · CET4 / CET6</div>
    <div>08 / 08</div>
  </div>
  <div class="frame" style="display:grid;gap:5vh;align-content:center;min-height:80vh">
    <div class="kicker" data-anim>Direct Links</div>
    <h1 class="h-hero" style="font-size:6.6vw" data-anim>直接拿走就好。</h1>

    <div class="col" style="gap:2vh;max-width:72vw">
      <div class="callout" data-anim>
        四级：
        <span style="font-family:var(--mono);font-size:.86em;letter-spacing:0;text-transform:none;">
          https://catteacher0515.github.io/cet4-download/
        </span>
      </div>

      <div class="callout" data-anim>
        六级：
        <span style="font-family:var(--mono);font-size:.86em;letter-spacing:0;text-transform:none;">
          https://catteacher0515.github.io/cet6-download/
        </span>
      </div>
    </div>

    <div class="meta-row" data-anim>
      <span>花萍雨</span><span>·</span><span>如果这份资源对你有帮助，欢迎关注我</span>
    </div>
  </div>
  <div class="foot">
    <div>CET Resource Deck · End</div>
    <div>— Thank You —</div>
  </div>
</section>
```

- [ ] **Step 3: Verify the full eight-slide deck and all image references**

```bash
rg -c '<section class="slide' \
  /Users/huapingyu/dev/guizang-test/ppt/index.html

rg -n '05-paper-list.png|05-batch-download.png|05-site-philosophy.png|06-paper-preview.png|欢迎关注我' \
  /Users/huapingyu/dev/guizang-test/ppt/index.html

rg -n 'SLIDES_CONTINUE' \
  /Users/huapingyu/dev/guizang-test/ppt/index.html
```

Expected:
- The slide count prints `8`
- The second `rg` finds all five markers
- The final `rg` returns no matches

- [ ] **Step 4: Commit slides 5-8**

```bash
git -C /Users/huapingyu/dev/guizang-test add ppt/index.html
git -C /Users/huapingyu/dev/guizang-test commit -m "feat: complete CET swipe deck content"
```

## Task 4: Run Deck QA and Ship the First Browser-Verified Version

**Files:**
- Modify if needed: `ppt/index.html`

- [ ] **Step 1: Run placeholder and theme-sequence checks**

```bash
rg -n '\[必填\]|SLIDES_HERE|SLIDES_CONTINUE' \
  /Users/huapingyu/dev/guizang-test/ppt/index.html

rg -o 'class="slide[^"]*"' -n \
  /Users/huapingyu/dev/guizang-test/ppt/index.html
```

Expected:
- The first `rg` returns no matches
- The second command prints this exact sequence:

```text
class="slide hero dark"
class="slide hero light"
class="slide light"
class="slide dark"
class="slide light"
class="slide dark"
class="slide light"
class="slide hero light"
```

- [ ] **Step 2: Start a local preview server**

```bash
python3 -m http.server 4173 --directory /Users/huapingyu/dev/guizang-test/ppt
```

Expected:
- The command prints `Serving HTTP on ... port 4173`
- Keep the server running while checking the deck in the browser

- [ ] **Step 3: Perform the visual QA pass in a browser**

Open:

```text
http://127.0.0.1:4173/index.html
```

Verify all of the following:
- Slide count is 8 and arrow/scroll navigation works
- Page 1 cover has the correct title and no placeholder text
- Page 3 comparison slide stays text-only and readable
- Page 5 grid loads all three screenshots and none are obviously cropped wrong
- Page 6 preview screenshot stays legible with `fit-contain`
- Page 8 shows both URLs and the CTA line in one pass without awkward wrapping

If any item fails, fix `ppt/index.html` before committing.

- [ ] **Step 4: Commit the browser-verified first version**

```bash
git -C /Users/huapingyu/dev/guizang-test add ppt/index.html
git -C /Users/huapingyu/dev/guizang-test commit -m "feat: ship browser-verified CET resource deck"
```

- [ ] **Step 5: Push the final deck**

```bash
git -C /Users/huapingyu/dev/guizang-test push origin main
```
