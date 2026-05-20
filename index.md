---
layout: home
profile_picture:
  src: /assets/img/profile-pic.jpeg
  alt: website picture
---

<style>
  /* === Smooth in-page scrolling for the top nav anchor jumps === */
  html { scroll-behavior: smooth; }

  /* === Sticky top navigation bar (thinner, gray, flush with viewport top) === */
  /* Use `nav.top-nav` so specificity (tag+class) beats the page-wide
     `.content-block ul` rule that would otherwise inherit and break the
     nav's line-height / padding / margin. */
  nav.top-nav {
    position: sticky;
    top: 0;
    z-index: 50;
    /* Translucent white with a real backdrop blur — content scrolling underneath
       is visibly frosted, while the bar stays "white" against the page. */
    background: rgba(255, 255, 255, 0.78);
    backdrop-filter: blur(10px) saturate(150%);
    -webkit-backdrop-filter: blur(10px) saturate(150%);
    border-bottom: 1px solid #e5e5e5;
    margin: 0 calc(-1 * clamp(14px, 4vw, 30px)) 24px;
  }
  nav.top-nav ul {
    list-style: none;
    margin: 0;
    padding: 0 clamp(14px, 4vw, 30px);
    display: flex;
    align-items: center;
    gap: clamp(18px, 3vw, 32px);
    min-height: 40px;
    font-size: 0.92em;
    line-height: 1;
    font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", Helvetica, Arial, sans-serif;
  }
  /* Selector includes `ul` so specificity (0,1,3,0) outranks `.content-block ul li`
     and we erase its 5px margin-bottom — the cause of the optical 3-px upward shift. */
  nav.top-nav ul li {
    display: flex;
    align-items: center;
    margin: 0;
    padding: 0;
  }
  nav.top-nav a {
    color: #444 !important;
    font-weight: 500;
    text-decoration: none;
    padding: 6px 2px;
    letter-spacing: 0.01em;
    line-height: 1;
    display: inline-flex;
    align-items: center;
  }
  nav.top-nav a:hover { color: #2053AB !important; }
  /* Anchor jumps clear the sticky nav so headings aren't tucked under it. */
  #home, #join-us, #publications, #news, #activities { scroll-margin-top: 48px; }
  @media (max-width: 600px) {
    nav.top-nav ul { gap: 18px; padding: 0 14px; min-height: 36px; font-size: 0.88em; }
  }

  /* === Bold all linked entities in News (highlights paper/venue/org names). === */
  .news-list a, #hidden-news a { font-weight: 600; }

  /* === Base typography === */
  .content-block { color: #222; font-size: 17px; }
  .content-block h1.page-heading {
    font-size: clamp(1.35em, 2.6vw, 1.6em);
    font-weight: 600;
    margin: 0 0 20px 0;
    padding: 0;
    border: none;
    letter-spacing: -0.01em;
    color: #111;
    overflow-wrap: break-word;
  }
  .content-block p { line-height: 1.7; margin: 0 0 16px 0; }
  .content-block ul { line-height: 1.7; margin: 0 0 16px 0; padding-left: 22px; }
  .content-block ul li { margin-bottom: 5px; }
  /* Default link color is plain dark text — most hyperlinks on this page are
     not "highlighted". Specific elements opt back into the brand blue below
     (role affiliations, paper titles, arXiv/Code chips). */
  /* Every link gets a soft, low-contrast gray underline so it's discoverable
     without visually competing with the prose. The underline darkens slightly
     and the text picks up brand-blue on hover. */
  .content-block a {
    color: #111;
    text-decoration: underline;
    text-decoration-color: rgba(0, 0, 0, 0.18);
    text-decoration-thickness: 1px;
    text-underline-offset: 2px;
    overflow-wrap: anywhere;
  }
  .content-block a:hover {
    color: #2053AB;
    text-decoration-color: rgba(32, 83, 171, 0.5);
  }
  /* Highlighted accent links — only these stay colored. All use the same
     brand blue (#2053AB) and are intentionally NOT underlined; the color
     itself is the affordance. */
  .content-block .role-affil a,
  .content-block .pub-title,
  .content-block .pub-links a,
  .content-block .social-line a {
    color: #2053AB !important;
    text-decoration: none;
  }
  /* Hover affordance for blue accent links: a subtle brand-blue underline
     appears only when the cursor is on the link. `!important` is needed on
     `text-decoration` to override the inline `text-decoration: none` on the
     Tencent Docs Form link. */
  .content-block .role-affil a:hover,
  .content-block .pub-title:hover,
  .content-block .pub-links a:hover,
  .content-block .social-line a:hover,
  .content-block .role-incoming a:hover,
  .content-block a[href*="docs.qq.com"]:hover {
    color: #2053AB !important;
    text-decoration: underline !important;
    text-decoration-color: rgba(32, 83, 171, 0.6) !important;
    text-decoration-thickness: 1px;
    text-underline-offset: 3px;
  }
  /* Social row keeps the bold treatment (no underline). */
  .content-block .social-line a {
    font-weight: 600;
  }
  .content-block strong { font-weight: 600; color: #111; }

  /* === Language switcher === */
  .lang-zh { display: none; }
  body.zh .lang-en { display: none; }
  body.zh .lang-zh { display: revert; }
  .lang-switcher {
    /* Pinned to the right side of the sticky nav. */
    position: fixed;
    top: max(6px, env(safe-area-inset-top, 0px));
    right: max(14px, env(safe-area-inset-right, 0px));
    z-index: 100;
    font-size: 0.85em;
    background: transparent;
    border-radius: 14px;
    line-height: 1;
  }
  .lang-switcher a {
    color: #444 !important;
    display: inline-flex;
    align-items: center;
    height: 28px;
    padding: 0 12px;
    border: 1px solid #c8c8c8;
    border-radius: 14px;
    background: rgba(255,255,255,0.85);
    text-decoration: none;
    line-height: 1;
    backdrop-filter: blur(4px);
  }
  .lang-switcher a:hover { background: #f3f3f3; text-decoration: none; }
  body.zh { font-family: -apple-system, BlinkMacSystemFont, "PingFang SC", "Helvetica Neue", "Microsoft YaHei", "Source Han Sans SC", sans-serif; }

  /* === Profile header === */
  .profile-header {
    display: flex;
    align-items: center;
    gap: clamp(16px, 3vw, 28px);
    margin-top: 36px;
    margin-bottom: 28px;
    /* Reserve space at the top so the lang-switcher doesn't overlap the name on phones. */
    padding-top: 8px;
  }
  .profile-meta { flex: 1 1 auto; min-width: 0; }
  .profile-meta p { margin: 0 0 4px 0; line-height: 1.55; overflow-wrap: anywhere; }
  .profile-pic {
    /* Scales smoothly between phone and desktop. */
    width: clamp(140px, 22vw, 220px);
    height: clamp(140px, 22vw, 220px);
    object-fit: cover;
    border-radius: 18px;
    flex: 0 0 auto;
  }
  .name-heading {
    font-size: clamp(1.7em, 5.5vw, 2.3em);
    font-weight: 700;
    margin: 0 0 12px 0 !important;
    letter-spacing: -0.02em;
    color: #111;
    line-height: 1.15;
  }
  .role-now {
    font-size: 1.1em;
    font-weight: 700;
    color: #111;
    margin: 0 0 2px 0 !important;
  }
  .role-affil { color: #333; margin-bottom: 4px !important; }
  .role-incoming {
    font-size: 0.97em;
    font-weight: 600;
    color: #2053AB; /* matches the brand-blue used for every other accent. */
    margin: 0 0 12px 0 !important;
    letter-spacing: 0.005em;
  }
  .role-incoming a {
    color: inherit !important;
    text-decoration: none;
  }
  /* The hover state for `.role-incoming a` is consolidated above with the
     other blue accent links (see the `.content-block …:hover` rule). */
  .meta-line { font-size: 0.97em; color: #444; margin-bottom: 4px !important; overflow-wrap: anywhere; }
  .meta-line em { font-style: normal; color: #999; }
  .prev-label {
    margin: 14px 0 2px 0 !important;
    color: #111;
    font-size: 0.94em;
    line-height: 1.4;
  }
  .prev-label strong { font-weight: 700; }
  .prev-list {
    margin: 0 !important;
    padding-left: 20px !important;
    color: #555;
    font-size: 0.94em;
  }
  .prev-list li { margin-bottom: 2px !important; line-height: 1.5 !important; }

  /* === Intro paragraphs below header === */
  .profile-intro p { margin-bottom: 14px; }
  /* Names & organizations (all hyperlinks in the intro) appear bold for
     scannability. Pairs naturally with the existing <strong> on Shanghai
     Innovation Institute and key dates. */
  .profile-intro a { font-weight: 600; }

  /* === Open-hours callout (Join Us) ===
     Brand-blue left border + soft tint. Visually anchored to the page palette
     so the friendly invite stands out without competing with the recruiting
     pitch above. */
  .office-hours {
    margin: 18px 0;
    padding: 12px 16px;
    border-left: 3px solid #2053AB;
    background: rgba(32, 83, 171, 0.06);
    border-radius: 0 6px 6px 0;
    line-height: 1.6;
  }
  .office-hours strong { color: #2053AB; }

  /* === Section dividers === */
  .section-divider {
    border: none;
    border-top: 1px solid #e5e5e5;
    margin: clamp(28px, 5vw, 44px) 0;
  }

  /* === Professional Activities === */
  .prof-activities div { margin-bottom: 8px; line-height: 1.55; }

  /* === News toggle === */
  #news-toggle-btn { font-size: 0.9em; margin-left: 6px; display: inline-block; padding: 4px 0; }

  /* === Publications === */
  .pub-year {
    font-size: 1.2em;
    font-weight: 700;
    color: #111;
    margin: 28px 0 14px 0;
    padding-bottom: 4px;
    border-bottom: 1px solid #eee;
  }
  .pub-year:first-of-type { margin-top: 8px; }
  .pub-entry {
    display: flex;
    gap: clamp(14px, 2.4vw, 22px);
    margin-bottom: clamp(20px, 3vw, 26px);
    align-items: flex-start;
  }
  .pub-teaser { flex: 0 0 180px; width: 180px; }
  .pub-teaser img,
  .pub-teaser video {
    width: 100%;
    height: 125px;
    object-fit: cover;
    object-position: center;
    border-radius: 5px;
    border: 1px solid rgba(0,0,0,0.08);
    display: block;
  }
  .pub-teaser img.contain {
    object-fit: contain;
    background: #f5f5f5;
  }
  .pub-info { flex: 1 1 auto; min-width: 0; }
  .pub-title {
    display: block;
    font-weight: 600;
    font-size: 1.03em;
    color: #2053AB !important;
    line-height: 1.35;
    margin-bottom: 6px;
    overflow-wrap: break-word;
  }
  .pub-authors { color: #333; font-size: 0.95em; margin-bottom: 4px; line-height: 1.5; overflow-wrap: break-word; }
  .pub-authors strong { color: #000; font-weight: 600; }
  .pub-venue { font-style: italic; font-weight: 600; color: #555; font-size: 0.94em; margin-bottom: 6px; }
  .pub-links { font-size: 0.92em; line-height: 1.7; }
  .pub-links a { color: #2053AB; }
  .pub-links .sep { color: #bbb; margin: 0 2px; }
  /* GitHub star badge (shields.io) sits inline after a [Code] link. Sized in
     em so it tracks the .pub-links font on every breakpoint; nudged down a
     hair so the pill's optical center aligns with the bracket text. */
  .pub-links img.gh-stars {
    height: 1.25em;
    width: auto;
    vertical-align: middle;
    margin-left: 5px;
    position: relative;
    top: -1px;
  }

  /* === Tablet (≤900px): keep row layout but tighten the teaser column. === */
  @media (max-width: 900px) {
    .profile-header { gap: 18px; margin-top: 24px; margin-bottom: 22px; }
    .pub-teaser { flex: 0 0 150px; width: 150px; }
    .pub-teaser img,
    .pub-teaser video { height: 105px; }
  }

  /* === Phone (≤600px): stack header + publication entries, center-align profile. === */
  @media (max-width: 600px) {
    .profile-header {
      flex-direction: column;
      align-items: center;
      text-align: center;
      gap: 18px;
      margin-top: 12px;
    }
    /* On phones, photo above identity — gives the page a clear human anchor
       before the wall of text. The clamp() in .profile-pic floors at 140px
       on desktop; explicitly bump it up here for a more present portrait. */
    .profile-pic {
      order: -1;
      width: 200px;
      height: 200px;
    }
    .profile-meta { text-align: center; }
    .meta-line { font-size: 0.92em; }
    .prev-list { text-align: left; display: inline-block; padding-left: 18px !important; }
    .prev-label { text-align: left; display: inline-block; }
    .pub-entry {
      flex-direction: column;
      gap: 12px;
    }
    .pub-teaser {
      flex: 0 0 auto;
      width: 100%;
      max-width: 360px;
      margin: 0 auto;
    }
    .pub-teaser img,
    .pub-teaser video { height: auto; max-height: 220px; }
    .pub-year { margin: 22px 0 12px 0; }
  }

  /* === Small phone (≤380px): tighter spacing, smaller lang switcher. === */
  @media (max-width: 380px) {
    .lang-switcher { font-size: 0.82em; }
    .lang-switcher a { padding: 5px 10px; }
    .content-block { font-size: 16px; }
    .pub-title { font-size: 1em; }
    .pub-authors, .pub-venue, .pub-links { font-size: 0.88em; }
  }
</style>

<div class="lang-switcher">
  <a href="javascript:void(0);" onclick="toggleLang()" id="lang-toggle"><span class="lang-en">中文</span><span class="lang-zh">English</span></a>
</div>

<nav class="top-nav" aria-label="Page sections">
  <ul>
    <li><a href="#home"><span class="lang-en">Home</span><span class="lang-zh">主页</span></a></li>
    <li><a href="#join-us"><span class="lang-en">Join us</span><span class="lang-zh">加入团队</span></a></li>
    <li><a href="#publications"><span class="lang-en">Publications</span><span class="lang-zh">论文</span></a></li>
  </ul>
</nav>

<div id="home" class="profile-header">
  <div class="profile-meta">
    <h1 class="name-heading"><span class="lang-en">Hanbo Zhang</span><span class="lang-zh">张翰博</span></h1>
    <p class="role-now"><span class="lang-en">Assistant Professor</span><span class="lang-zh">助理教授</span></p>
    <p class="role-affil"><span class="lang-en"><a href="https://www.sii.edu.cn/">Shanghai Innovation Institute</a></span><span class="lang-zh"><a href="https://www.sii.edu.cn/">上海创智学院</a></span></p>
    <p class="meta-line"><span class="lang-en"><strong>Email</strong>:</span><span class="lang-zh"><strong>邮箱</strong>：</span> zhanghanbo <em>at</em> sii <em>dot</em> edu <em>dot</em> cn</p>
    <p class="meta-line social-line">
      <a href="https://scholar.google.com/citations?user=1qfEEwsAAAAJ">Google Scholar</a> &middot;
      <a href="https://www.linkedin.com/in/%E7%BF%B0%E5%8D%9A-%E5%BC%A0-2754551a2/">LinkedIn</a> &middot;
      <a href="https://github.com/ZhangHanbo">GitHub</a>
    </p>
    <p class="prev-label"><span class="lang-en"><strong>Previously:</strong></span><span class="lang-zh"><strong>过往经历：</strong></span></p>
    <ul class="prev-list">
      <li><span class="lang-en">Senior Research Fellow, <a href="https://adacomp.comp.nus.edu.sg/">AdaComp</a>, <a href="https://www.nus.edu.sg/">National University of Singapore</a> (2024&ndash;2026)</span><span class="lang-zh">高级研究员，<a href="https://adacomp.comp.nus.edu.sg/">AdaComp</a> 实验室，<a href="https://www.nus.edu.sg/">新加坡国立大学</a>（2024&ndash;2026）</span></li>
      <li><span class="lang-en">Robotics Researcher, <a href="https://www.bytedance.com/en/">ByteDance Research</a> (2022&ndash;2024)</span><span class="lang-zh"><a href="https://www.bytedance.com/">字节跳动研究院</a> 机器人研究员（2022&ndash;2024）</span></li>
      <li><span class="lang-en">Visiting PhD, <a href="https://www.nus.edu.sg/">National University of Singapore</a></span><span class="lang-zh">访问博士生，<a href="https://www.nus.edu.sg/">新加坡国立大学</a></span></li>
      <li><span class="lang-en">PhD, <a href="https://en.xjtu.edu.cn/">Xi'an Jiaotong University</a> (2017&ndash;2022)</span><span class="lang-zh">博士，<a href="https://www.xjtu.edu.cn/">西安交通大学</a>（2017&ndash;2022）</span></li>
    </ul>
  </div>
  <img src="/assets/img/zhanghanbo.jpg" alt="Hanbo Zhang" class="profile-pic">
</div>

<div class="profile-intro">
  <p class="lang-en">I am an Assistant Professor at the <strong><a href="https://www.sii.edu.cn/">Shanghai Innovation Institute</a></strong>. Before joining <strong>SII</strong>, I was a Senior Research Fellow at the <a href="https://www.nus.edu.sg/">National University of Singapore</a>, working with Prof. <a href="https://www.comp.nus.edu.sg/~dyhsu/">David Hsu</a>; earlier, I was a robotics researcher at <a href="https://www.bytedance.com/en/">ByteDance Research</a> with <a href="https://www.taokong.org/">Tao Kong</a>. I completed my PhD at <a href="https://en.xjtu.edu.cn/">Xi'an Jiaotong University</a> in 2022, advised by Prof. <a href="http://gr.xjtu.edu.cn/web/zeuslan">Xuguang Lan</a>.</p>
  <p class="lang-zh">现任 <strong><a href="https://www.sii.edu.cn/">上海创智学院</a></strong> 助理教授。在此之前，曾在<a href="https://www.nus.edu.sg/">新加坡国立大学</a>（<strong>NUS</strong>）担任高级研究员，与 <a href="https://www.comp.nus.edu.sg/~dyhsu/">David Hsu</a> 教授合作；更早曾在<a href="https://www.bytedance.com/">字节跳动研究院</a>担任机器人研究员，与 <a href="https://www.taokong.org/">Tao Kong</a> 博士合作。2022 年于<a href="https://www.xjtu.edu.cn/">西安交通大学</a>取得博士学位，导师为 <a href="http://gr.xjtu.edu.cn/web/zeuslan">兰旭光</a> 教授。</p>

  <p class="lang-en">My research studies <strong>structuring uncertainty for open-world manipulation</strong>, developing physically-grounded and reliable intelligence for long-horizon robotic tasks in dynamic environments.</p>
  <p class="lang-zh">我的研究聚焦于 <strong>面向开放世界操纵的不确定性建模</strong>，致力于为动态环境中的长时程机器人任务构建具有物理基础、可靠可信的智能系统。</p>

  <p class="office-hours"><span class="lang-en"><strong>Open hours for students.</strong> 2 hours every week (20 mins per slot) for a casual chat: research, future plans, brainstorming, honest criticism of me, or anything in your mind. I am open to all of it; just send me an email.</span><span class="lang-zh"><strong>面向学生的开放时间。</strong>每周 2 小时（每位 20 分钟）自由交流：研究、未来规划、头脑风暴、对我的坦诚批评，或任何你心里想聊的话题。我对一切持开放态度，直接发邮件给我即可。</span></p>
</div>

<hr class="section-divider">

<h1 id="join-us" class="page-heading"><span class="lang-en">Join Our Group</span><span class="lang-zh">加入我们的团队</span></h1>

<p class="lang-en">Our group launches at <strong><a href="https://www.sii.edu.cn/">Shanghai Innovation Institute</a></strong> in May 2026. We are recruiting <strong>PhD students</strong> for the Fall 2026 or Spring 2027 intake, and <strong>research interns</strong> (remote or in person). We are also hiring a <strong>research engineer</strong>, with a competitive offer for the right candidate.</p>
<p class="lang-zh">2026 年 5 月，我们将在 <strong><a href="https://www.sii.edu.cn/">上海创智学院</a></strong> 组建研究组。现招收 <strong>博士生</strong>（2026 秋季或 2027 春季入学），以及 <strong>研究实习生</strong>（线上或线下均可）。同时招聘 <strong>研究工程师</strong>，为合适人选提供具有竞争力的待遇。</p>

<p class="lang-en"><strong>Research topics.</strong> We work on uncertainty-aware task specification and planning, object-centric scene representation for mobile manipulation, and composable, scalable manipulation skills. The broader theme is <em>structuring uncertainty for open-world manipulation</em>.</p>
<p class="lang-zh"><strong>研究方向。</strong> 我们聚焦于面向不确定性的任务表达与规划，面向移动操纵的物体中心场景表征，以及可组合、可扩展的操纵技能。整体主题是 <em>为开放世界操纵建模不确定性</em>。</p>

<p class="lang-en"><strong>Who we look for.</strong> Self-motivated, with strong instincts for real-world robotics research: physical robots, contact, planning, mobility, manipulation policies, the parts LLMs cannot do for you. We <strong>ONLY</strong> focus on algorithms and systems that ground in hardware, not conceptually or virtually. Independent thinking that may comfortably disagree with ours is 100% encouraged.</p>
<p class="lang-zh"><strong>期望的学生：</strong> 自驱，独立，对真实世界机器人研究有强烈兴趣， 包括但不限于物理机器人、接触动力学、规划、移动性、操作技能学习。我们只关注真正能落到硬件上的算法与系统，而不是局限于概念上的算法创新或仿真。愿意在学术问题上和所有人坦诚讨论甚至辩论。独立思考。</p>

<p class="lang-en">Please fill in this <a href="https://docs.qq.com/form/page/DUE10d0tCVVBkSEpN" style="color: #2053AB; font-weight: 600; text-decoration: none;">Form</a>, and email me your CV, the problem you most want to work on, and one or two representative papers or projects.</p>
<p class="lang-zh">请填写 <a href="https://docs.qq.com/form/page/DUE10d0tCVVBkSEpN" style="color: #2053AB; font-weight: 600; text-decoration: none;">该表格</a>，并发送邮件附上你的 CV、最想研究的问题，以及一到两个代表工作或项目。</p>

<hr class="section-divider">

<script>
function toggleNews() {
  var hiddenNews = document.getElementById('hidden-news');
  var toggleBtn = document.getElementById('news-toggle-btn');
  var isZh = document.body.classList.contains('zh');
  if (hiddenNews.style.display === 'none') {
    hiddenNews.style.display = 'block';
    toggleBtn.innerHTML = '[' + (isZh ? '收起' : 'Show Less') + ']';
  } else {
    hiddenNews.style.display = 'none';
    toggleBtn.innerHTML = '[' + (isZh ? '展开更多' : 'Show More') + ']';
  }
}
function toggleLang() {
  document.body.classList.toggle('zh');
  try { localStorage.setItem('hz_lang', document.body.classList.contains('zh') ? 'zh' : 'en'); } catch (e) {}
}
(function(){
  try {
    if (localStorage.getItem('hz_lang') === 'zh') { document.body.classList.add('zh'); }
  } catch (e) {}
})();
</script>

<h1 class="page-heading"><span class="lang-en">News</span><span class="lang-zh">近期动态</span></h1>
<ul class="news-list">
  <li><span class="lang-en"><strong>May 2026</strong> &middot; Joining the <a href="https://www.sii.edu.cn/">Shanghai Innovation Institute</a> as an Assistant Professor.</span><span class="lang-zh"><strong>2026 年 5 月</strong> &middot; 加入<a href="https://www.sii.edu.cn/">上海创智学院</a>，出任助理教授。</span></li>
  <li><span class="lang-en"><strong>Apr 2026</strong> &middot; Hypothesis-driven Model Expansion accepted at <a href="https://roboticsconference.org/">RSS 2026</a>.</span><span class="lang-zh"><strong>2026 年 4 月</strong> &middot; Hypothesis-driven Model Expansion 论文被 <a href="https://roboticsconference.org/">RSS 2026</a> 录用。</span></li>
  <li><span class="lang-en"><strong>Apr 2026</strong> &middot; Organizing the <a href="https://adacompnus.github.io/human-centric-mobile-manipulation/">Human-centric Mobile Manipulation</a> workshop, accepted to <a href="https://roboticsconference.org/">RSS 2026</a>.</span><span class="lang-zh"><strong>2026 年 4 月</strong> &middot; 组织的 <a href="https://adacompnus.github.io/human-centric-mobile-manipulation/">Human-centric Mobile Manipulation</a> 研讨会入选 <a href="https://roboticsconference.org/">RSS 2026</a>。</span></li>
  <li><span class="lang-en"><strong>Feb 2026</strong> &middot; <a href="https://robovlms.github.io/">RoboVLMs</a> accepted at <a href="https://www.nature.com/natmachintell/"><em>Nature Machine Intelligence</em></a>.</span><span class="lang-zh"><strong>2026 年 2 月</strong> &middot; <a href="https://robovlms.github.io/">RoboVLMs</a> 论文被 <a href="https://www.nature.com/natmachintell/"><em>Nature Machine Intelligence</em></a> 录用。</span></li>
  <li><span class="lang-en"><strong>Sep 2025</strong> &middot; <a href="https://github.com/zhangj1an/ApBot">ApBot</a> and <a href="https://sites.google.com/view/mimicfunc">MimicFunc</a> accepted at <a href="https://2025.corl.org/">CoRL 2025</a>; <a href="https://chain-of-action.github.io/">Chain-of-Action</a> accepted at <a href="https://neurips.cc/Conferences/2025">NeurIPS 2025</a>.</span><span class="lang-zh"><strong>2025 年 9 月</strong> &middot; <a href="https://github.com/zhangj1an/ApBot">ApBot</a> 与 <a href="https://sites.google.com/view/mimicfunc">MimicFunc</a> 被 <a href="https://2025.corl.org/">CoRL 2025</a> 录用；<a href="https://chain-of-action.github.io/">Chain-of-Action</a> 被 <a href="https://neurips.cc/Conferences/2025">NeurIPS 2025</a> 录用。</span></li>
</ul>
<div id="hidden-news" style="display: none;">
  <ul>
    <li><span class="lang-en"><strong>May 2025</strong> &middot; <a href="https://github.com/zhangj1an/ApBot">ApBot</a> paper and code released.</span><span class="lang-zh"><strong>2025 年 5 月</strong> &middot; <a href="https://github.com/zhangj1an/ApBot">ApBot</a> 论文与代码发布。</span></li>
    <li><span class="lang-en"><strong>Dec 2024</strong> &middot; <a href="https://robovlms.github.io/">RoboVLMs</a> paper and code released.</span><span class="lang-zh"><strong>2024 年 12 月</strong> &middot; <a href="https://robovlms.github.io/">RoboVLMs</a> 论文与代码发布。</span></li>
    <li><span class="lang-en"><strong>Oct 2024</strong> &middot; <a href="https://gr2-manipulation.github.io/">GR-2</a> technical report released.</span><span class="lang-zh"><strong>2024 年 10 月</strong> &middot; <a href="https://gr2-manipulation.github.io/">GR-2</a> 技术报告发布。</span></li>
    <li><span class="lang-en"><strong>Jan 2024</strong> &middot; <a href="https://roboflamingo.github.io/">RoboFlamingo</a> accepted at <a href="https://iclr.cc/Conferences/2024">ICLR 2024</a>; <a href="https://jxu124.github.io/TiO">TiO</a> accepted at <a href="https://2024.ieee-icra.org/">ICRA 2024</a>; <a href="https://lynx-llm.github.io/">Lynx</a> accepted at <a href="https://2024.naacl.org/">NAACL 2024</a>.</span><span class="lang-zh"><strong>2024 年 1 月</strong> &middot; <a href="https://roboflamingo.github.io/">RoboFlamingo</a> 被 <a href="https://iclr.cc/Conferences/2024">ICLR 2024</a> 录用；<a href="https://jxu124.github.io/TiO">TiO</a> 被 <a href="https://2024.ieee-icra.org/">ICRA 2024</a> 录用；<a href="https://lynx-llm.github.io/">Lynx</a> 被 <a href="https://2024.naacl.org/">NAACL 2024</a> 录用。</span></li>
  </ul>
</div>
<a href="javascript:void(0);" onclick="toggleNews()" id="news-toggle-btn">[<span class="lang-en">Show More</span><span class="lang-zh">展开更多</span>]</a>

<hr class="section-divider">

{% include publications.html %}

<hr class="section-divider">

<h1 class="page-heading"><span class="lang-en">Professional Activities</span><span class="lang-zh">学术服务</span></h1>

<div class="prof-activities">
  <div><span class="lang-en"><strong>Associate Editor</strong></span><span class="lang-zh"><strong>副编辑</strong></span> &middot; <a href="https://www.ieee-ras.org/publications/ra-l">RA-L</a></div>
  <div><span class="lang-en"><strong>Journal Reviewer</strong></span><span class="lang-zh"><strong>期刊审稿人</strong></span> &middot; <a href="https://journals.sagepub.com/home/ijr">IJRR</a>, <a href="https://www.ieee-ras.org/publications/t-ro">TRO</a>, <a href="https://www.computer.org/csdl/journal/tp">TPAMI</a></div>
  <div><span class="lang-en"><strong>Conference Reviewer</strong></span><span class="lang-zh"><strong>会议审稿人</strong></span> &middot; <a href="https://roboticsconference.org/">RSS</a>, <a href="https://corl.org/">CoRL</a>, <a href="https://www.ieee-ras.org/conferences-workshops/fully-sponsored/icra">ICRA</a>, <a href="https://www.ieee-ras.org/conferences-workshops/fully-sponsored/iros">IROS</a>, <a href="https://www.ifaamas.org/">AAMAS</a></div>
</div>
