<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>Threads 投稿 AI</title>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@300;400;500;700;900&family=Space+Mono:wght@700&display=swap" rel="stylesheet">
<style>
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent;}
:root{
  --bg:#f2f2f7;--surface:#fff;--accent:#7c6af7;--accent2:#f76a8a;--accent3:#00c896;
  --text:#1c1c1e;--text2:#3a3a3c;--muted:#8e8e93;--border:#e5e5ea;
  --tab-h:68px;--safe-b:env(safe-area-inset-bottom,16px);
}
html,body{height:100%;font-family:'Noto Sans JP',sans-serif;background:var(--bg);color:var(--text);overflow:hidden;}
.app{display:flex;flex-direction:column;height:100vh;max-width:430px;margin:0 auto;background:var(--bg);position:relative;}

/* Statusbar */
.statusbar{height:44px;background:var(--surface);display:flex;align-items:center;justify-content:space-between;padding:0 20px;flex-shrink:0;border-bottom:1px solid var(--border);}
.statusbar-time{font-size:15px;font-weight:700;letter-spacing:-0.3px;}
.statusbar-icons{display:flex;gap:6px;align-items:center;}

/* Nav */
.nav-header{background:var(--surface);padding:12px 20px 14px;display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid var(--border);flex-shrink:0;}
.nav-title{font-size:18px;font-weight:700;letter-spacing:-0.3px;}
.nav-badge{display:flex;align-items:center;gap:6px;background:rgba(124,106,247,0.1);border-radius:100px;padding:5px 12px;font-size:11px;font-weight:700;color:var(--accent);font-family:'Space Mono',monospace;}
.nav-badge .dot{width:5px;height:5px;border-radius:50%;background:var(--accent3);animation:pulse 2s infinite;}

/* Scroll */
.scroll-area{flex:1;overflow-y:auto;overflow-x:hidden;-webkit-overflow-scrolling:touch;padding-bottom:calc(var(--tab-h) + var(--safe-b) + 16px);}
.scroll-area::-webkit-scrollbar{display:none;}

/* Sections */
.section{display:none;padding:16px 16px 0;animation:fadeIn 0.25s ease;}
.section.active{display:block;}
@keyframes fadeIn{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:translateY(0)}}

/* Cards */
.card{background:var(--surface);border-radius:16px;margin-bottom:12px;overflow:hidden;box-shadow:0 1px 4px rgba(0,0,0,0.06);}
.card-header{padding:16px 16px 0;font-size:11px;font-weight:700;letter-spacing:0.1em;text-transform:uppercase;color:var(--muted);margin-bottom:10px;}

/* Form */
.form-group{padding:0 16px 14px;}
.form-group label{display:block;font-size:12px;font-weight:600;color:var(--muted);margin-bottom:6px;text-transform:uppercase;letter-spacing:0.08em;}
select,input[type=text],textarea{width:100%;background:var(--bg);border:none;border-radius:10px;padding:12px 14px;font-family:'Noto Sans JP',sans-serif;font-size:15px;color:var(--text);outline:none;appearance:none;-webkit-appearance:none;}
textarea{resize:none;font-size:14px;line-height:1.6;}
.select-wrap{position:relative;}
.select-wrap::after{content:'›';position:absolute;right:14px;top:50%;transform:translateY(-50%) rotate(90deg);font-size:18px;color:var(--muted);pointer-events:none;}

/* Segment */
.segment{display:flex;background:var(--bg);border-radius:10px;padding:3px;margin:0 16px 14px;}
.seg-btn{flex:1;border:none;background:transparent;border-radius:8px;padding:8px 4px;font-family:'Noto Sans JP',sans-serif;font-size:12px;font-weight:500;color:var(--muted);cursor:pointer;transition:all 0.2s;}
.seg-btn.active{background:var(--surface);color:var(--text);font-weight:700;box-shadow:0 1px 4px rgba(0,0,0,0.12);}

/* Chips */
.chips{display:flex;flex-wrap:wrap;gap:8px;padding:0 16px 16px;}
.chip{border:1.5px solid var(--border);background:transparent;border-radius:100px;padding:7px 14px;font-family:'Noto Sans JP',sans-serif;font-size:13px;color:var(--muted);cursor:pointer;transition:all 0.18s;}
.chip.active{border-color:var(--accent);background:rgba(124,106,247,0.1);color:var(--accent);font-weight:600;}

/* Buttons */
.btn-gen{display:flex;align-items:center;justify-content:center;gap:8px;width:calc(100% - 32px);margin:0 16px 16px;padding:17px;border:none;border-radius:14px;background:linear-gradient(135deg,#7c6af7,#f76a8a);color:white;font-family:'Noto Sans JP',sans-serif;font-size:16px;font-weight:700;cursor:pointer;transition:transform 0.15s,opacity 0.15s;box-shadow:0 4px 20px rgba(124,106,247,0.3);}
.btn-gen:active{transform:scale(0.97);}
.btn-gen:disabled{opacity:0.5;pointer-events:none;}

/* Loading */
.loading-wrap{display:none;flex-direction:column;align-items:center;padding:40px 20px;gap:12px;}
.loading-wrap.show{display:flex;}
.spinner{width:32px;height:32px;border:3px solid var(--border);border-top-color:var(--accent);border-radius:50%;animation:spin 0.7s linear infinite;}
.spinner.sm{width:20px;height:20px;border-width:2.5px;}
.loading-wrap p{font-size:14px;color:var(--muted);}
@keyframes spin{to{transform:rotate(360deg)}}

/* Result cards */
.result-card{background:var(--surface);border-radius:16px;margin-bottom:10px;overflow:hidden;box-shadow:0 1px 4px rgba(0,0,0,0.06);animation:fadeIn 0.3s ease both;}
.result-top{display:flex;align-items:center;gap:8px;padding:14px 16px 10px;}
.result-num{font-family:'Space Mono',monospace;font-size:10px;color:var(--muted);}
.type-pill{font-size:10px;font-weight:700;letter-spacing:0.06em;padding:3px 9px;border-radius:100px;border:1px solid;}
.tp-hook{color:#7c6af7;border-color:rgba(124,106,247,0.3);background:rgba(124,106,247,0.08);}
.tp-story{color:#f76a8a;border-color:rgba(247,106,138,0.3);background:rgba(247,106,138,0.08);}
.tp-value{color:#00c896;border-color:rgba(0,200,150,0.3);background:rgba(0,200,150,0.08);}
.tp-question{color:#f7a600;border-color:rgba(247,166,0,0.3);background:rgba(247,166,0,0.08);}
.result-text{padding:0 16px 14px;font-size:14px;line-height:1.75;color:var(--text2);white-space:pre-wrap;}
.result-footer{border-top:1px solid var(--border);padding:10px 16px;display:flex;gap:8px;flex-wrap:wrap;}
.btn-sm{display:flex;align-items:center;gap:5px;padding:7px 12px;border-radius:8px;border:1px solid var(--border);background:transparent;font-family:'Noto Sans JP',sans-serif;font-size:12px;color:var(--muted);cursor:pointer;transition:all 0.18s;white-space:nowrap;}
.btn-sm:active{transform:scale(0.95);}
.btn-sm.ok{color:var(--accent3);border-color:var(--accent3);background:rgba(0,200,150,0.08);}
.btn-sm.comment-btn{color:var(--accent2);border-color:rgba(247,106,138,0.4);background:rgba(247,106,138,0.05);}
.btn-sm.comment-btn:active,.btn-sm.comment-btn.loading-c{opacity:0.7;}

/* Comment section inside result card */
.comment-section{border-top:1px solid var(--border);padding:12px 16px;background:rgba(247,106,138,0.02);}
.comment-section-header{font-size:11px;font-weight:700;letter-spacing:0.08em;text-transform:uppercase;color:var(--accent2);margin-bottom:10px;display:flex;align-items:center;gap:6px;}
.comment-item{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:12px 14px;margin-bottom:8px;position:relative;animation:fadeIn 0.25s ease both;}
.comment-item:last-child{margin-bottom:0;}
.comment-meta{display:flex;align-items:center;justify-content:space-between;margin-bottom:6px;}
.comment-style{font-size:10px;font-weight:700;padding:2px 8px;border-radius:100px;border:1px solid;}
.cs-empathy{color:#7c6af7;border-color:rgba(124,106,247,0.3);background:rgba(124,106,247,0.06);}
.cs-question{color:#f7a600;border-color:rgba(247,166,0,0.3);background:rgba(247,166,0,0.06);}
.cs-praise{color:#00c896;border-color:rgba(0,200,150,0.3);background:rgba(0,200,150,0.06);}
.cs-value{color:#f76a8a;border-color:rgba(247,106,138,0.3);background:rgba(247,106,138,0.06);}
.comment-text{font-size:13px;line-height:1.65;color:var(--text2);}
.btn-copy-sm{border:none;background:transparent;color:var(--muted);cursor:pointer;padding:2px 6px;font-size:11px;border-radius:6px;transition:color 0.15s;}
.btn-copy-sm:hover{color:var(--accent3);}
.comment-loading{display:flex;align-items:center;gap:8px;padding:12px 0;color:var(--muted);font-size:13px;}

/* Comment tab */
.comment-tab-intro{text-align:center;padding:32px 20px 20px;}
.comment-tab-intro .big-icon{font-size:48px;margin-bottom:12px;}
.comment-tab-intro h2{font-size:18px;font-weight:800;margin-bottom:8px;}
.comment-tab-intro p{font-size:13px;color:var(--muted);line-height:1.65;}

.cmt-input-card{background:var(--surface);border-radius:16px;margin-bottom:12px;overflow:hidden;box-shadow:0 1px 4px rgba(0,0,0,0.06);}
.cmt-input-card .card-header{padding:14px 16px 0;}
.cmt-post-area{padding:0 16px 14px;}
.cmt-post-area textarea{background:var(--bg);border-radius:10px;min-height:90px;}

.cmt-style-row{display:flex;flex-wrap:wrap;gap:8px;padding:0 16px 14px;}
.cmt-style-chip{border:1.5px solid var(--border);background:transparent;border-radius:100px;padding:6px 12px;font-family:'Noto Sans JP',sans-serif;font-size:12px;color:var(--muted);cursor:pointer;transition:all 0.18s;}
.cmt-style-chip.active{border-color:var(--accent2);background:rgba(247,106,138,0.1);color:var(--accent2);font-weight:600;}

.btn-gen-comment{display:flex;align-items:center;justify-content:center;gap:8px;width:calc(100% - 32px);margin:0 16px 16px;padding:15px;border:none;border-radius:14px;background:linear-gradient(135deg,#f76a8a,#f7a600);color:white;font-family:'Noto Sans JP',sans-serif;font-size:15px;font-weight:700;cursor:pointer;transition:transform 0.15s,opacity 0.15s;box-shadow:0 4px 16px rgba(247,106,138,0.3);}
.btn-gen-comment:active{transform:scale(0.97);}
.btn-gen-comment:disabled{opacity:0.5;pointer-events:none;}

.cmt-results{}
.cmt-result-item{background:var(--surface);border-radius:14px;padding:14px 16px;margin-bottom:10px;box-shadow:0 1px 4px rgba(0,0,0,0.06);animation:fadeIn 0.25s ease both;position:relative;}
.cmt-result-item .comment-meta{margin-bottom:8px;}
.cmt-result-item .comment-text{font-size:14px;line-height:1.7;color:var(--text2);margin-bottom:10px;}
.cmt-result-item .copy-row{display:flex;justify-content:flex-end;}

/* History */
.history-empty{text-align:center;padding:60px 20px;color:var(--muted);}
.history-empty .icon{font-size:40px;margin-bottom:12px;}
.history-empty p{font-size:14px;line-height:1.6;}
.hist-card{background:var(--surface);border-radius:16px;padding:16px;margin-bottom:10px;box-shadow:0 1px 4px rgba(0,0,0,0.06);cursor:pointer;transition:transform 0.15s;}
.hist-card:active{transform:scale(0.98);}
.hist-meta{display:flex;align-items:center;justify-content:space-between;margin-bottom:8px;}
.hist-genre{font-size:12px;font-weight:700;color:var(--accent);}
.hist-date{font-size:11px;color:var(--muted);}
.hist-preview{font-size:13px;color:var(--text2);line-height:1.5;display:-webkit-box;-webkit-line-clamp:2;-webkit-box-orient:vertical;overflow:hidden;}

/* Settings */
.settings-row{display:flex;align-items:center;justify-content:space-between;padding:14px 16px;border-bottom:1px solid var(--border);}
.settings-row:last-child{border-bottom:none;}
.settings-label{font-size:15px;}
.settings-value{font-size:14px;color:var(--muted);}
.toggle{width:50px;height:30px;border-radius:15px;background:var(--border);position:relative;cursor:pointer;transition:background 0.2s;}
.toggle.on{background:var(--accent3);}
.toggle::after{content:'';position:absolute;width:26px;height:26px;border-radius:50%;background:white;top:2px;left:2px;transition:transform 0.2s;box-shadow:0 1px 4px rgba(0,0,0,0.2);}
.toggle.on::after{transform:translateX(20px);}

/* Results header */
.results-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:12px;}
.results-header span{font-size:12px;font-weight:700;color:var(--muted);text-transform:uppercase;letter-spacing:0.1em;}
.results-count{background:rgba(124,106,247,0.1);color:var(--accent);font-family:'Space Mono',monospace;font-size:11px;font-weight:700;padding:3px 10px;border-radius:100px;}

/* Tab bar */
.tab-bar{position:absolute;bottom:0;left:0;right:0;height:calc(var(--tab-h) + var(--safe-b));padding-bottom:var(--safe-b);background:rgba(255,255,255,0.92);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);border-top:1px solid var(--border);display:flex;align-items:flex-start;padding-top:8px;z-index:100;}
.tab-item{flex:1;display:flex;flex-direction:column;align-items:center;gap:3px;cursor:pointer;padding:4px 0;border:none;background:transparent;transition:transform 0.15s;}
.tab-item:active{transform:scale(0.9);}
.tab-icon{width:26px;height:26px;border-radius:8px;display:flex;align-items:center;justify-content:center;transition:background 0.2s;}
.tab-item.active .tab-icon{background:rgba(124,106,247,0.12);}
.tab-item svg{transition:all 0.2s;}
.tab-item.active svg{color:var(--accent);}
.tab-item:not(.active) svg{color:var(--muted);}
.tab-label{font-size:10px;font-weight:600;color:var(--muted);letter-spacing:0.03em;}
.tab-item.active .tab-label{color:var(--accent);font-weight:700;}
.page-title{font-size:22px;font-weight:900;letter-spacing:-0.5px;padding:8px 16px 12px;}

@keyframes pulse{0%,100%{opacity:1}50%{opacity:0.3}}
</style>
</head>
<body>
<div class="app">

  <!-- Status bar -->
  <div class="statusbar">
    <span class="statusbar-time" id="clock">9:41</span>
    <div class="statusbar-icons">
      <svg width="16" height="12" viewBox="0 0 16 12" fill="currentColor"><rect x="0" y="4" width="3" height="8" rx="1"/><rect x="4.5" y="2.5" width="3" height="9.5" rx="1"/><rect x="9" y="0.5" width="3" height="11.5" rx="1"/><rect x="13.5" y="0" width="2.5" height="12" rx="1" opacity="0.3"/></svg>
      <svg width="16" height="12" viewBox="0 0 24 12" fill="none" stroke="currentColor" stroke-width="2"><path d="M1 4C4.5.5 9.5.5 12 3C14.5.5 19.5.5 23 4"/><path d="M4 7C6.5 4.5 9.5 4.5 12 7C14.5 4.5 17.5 4.5 20 7"/><circle cx="12" cy="10" r="1.5" fill="currentColor"/></svg>
      <svg width="25" height="12" viewBox="0 0 25 12" fill="none"><rect x=".5" y=".5" width="21" height="11" rx="3.5" stroke="currentColor" stroke-opacity=".35"/><rect x="2" y="2" width="16" height="8" rx="2" fill="currentColor"/><path d="M23 4.5V7.5C23.8 7.2 24.5 6.5 24.5 6C24.5 5.5 23.8 4.8 23 4.5Z" fill="currentColor" opacity=".4"/></svg>
    </div>
  </div>

  <!-- Nav header -->
  <div class="nav-header">
    <span class="nav-title" id="nav-title">投稿生成</span>
    <div class="nav-badge"><span class="dot"></span>AI</div>
  </div>

  <!-- Scroll area -->
  <div class="scroll-area">

    <!-- ① 生成タブ -->
    <div class="section active" id="tab-generate">
      <p class="page-title">投稿アイデアを<br>生成する</p>

      <div class="card">
        <div class="card-header">ジャンル &amp; 目的</div>
        <div class="form-group">
          <label>ジャンル</label>
          <div class="select-wrap">
            <select id="genre">
              <option>副業・フリーランス</option><option>デザイン・クリエイター</option>
              <option>動画・YouTube</option><option>ライティング・ブログ</option>
              <option>プログラミング・エンジニア</option><option>SNSマーケティング</option>
              <option>写真・カメラ</option><option>イラスト・アート</option>
            </select>
          </div>
        </div>
        <div class="form-group">
          <label>目的</label>
          <div class="select-wrap">
            <select id="goal">
              <option>フォロワーを増やす</option><option>信頼・権威性を築く</option>
              <option>集客・売上につなげる</option><option>エンゲージメントを高める</option>
              <option>ブランドを認知させる</option>
            </select>
          </div>
        </div>
        <div class="form-group">
          <label>自己紹介（任意）</label>
          <input type="text" id="profile" placeholder="例：会社員しながら週末に副業デザイナー" />
        </div>
      </div>

      <div class="card">
        <div class="card-header">投稿のトーン</div>
        <div class="chips" id="tone-chips">
          <button class="chip active" data-val="共感・親近感">共感・親近感</button>
          <button class="chip" data-val="プロ・権威">プロ・権威</button>
          <button class="chip" data-val="ユーモア">ユーモア</button>
          <button class="chip" data-val="挑発・インパクト">挑発・強め</button>
          <button class="chip" data-val="教育・有益">教育・有益</button>
        </div>
      </div>

      <button class="btn-gen" id="gen-btn" onclick="generate()">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2.5"><path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"/></svg>
        投稿アイデアを生成する
      </button>

      <div class="loading-wrap" id="loading"><div class="spinner"></div><p>AIが考えています…</p></div>
      <div id="results"></div>
    </div>

    <!-- ② コメントタブ -->
    <div class="section" id="tab-comment">
      <p class="page-title">自動コメント<br>生成する</p>

      <div class="cmt-input-card">
        <div class="card-header">他者の投稿文を入力</div>
        <div class="cmt-post-area">
          <textarea id="cmt-post-input" placeholder="コメントしたい投稿文をここに貼り付け…&#10;&#10;例：「副業を始めて3ヶ月で月5万円達成しました！」" rows="4"></textarea>
        </div>
        <div class="card-header" style="padding-top:0">コメントスタイル（複数選択可）</div>
        <div class="cmt-style-row" id="cmt-style-chips">
          <button class="cmt-style-chip active" data-val="共感">共感</button>
          <button class="cmt-style-chip active" data-val="質問">質問</button>
          <button class="cmt-style-chip" data-val="賞賛">賞賛</button>
          <button class="cmt-style-chip" data-val="価値追加">価値追加</button>
          <button class="cmt-style-chip" data-val="ユーモア">ユーモア</button>
        </div>
      </div>

      <button class="btn-gen-comment" id="cmt-btn" onclick="generateComments()">
        <svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2.5"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg>
        コメント候補を生成する
      </button>

      <div class="loading-wrap" id="cmt-loading"><div class="spinner"></div><p>コメントを考えています…</p></div>
      <div id="cmt-results"></div>
    </div>

    <!-- ③ 履歴タブ -->
    <div class="section" id="tab-history">
      <p class="page-title">生成履歴</p>
      <div id="history-list">
        <div class="history-empty"><div class="icon">📝</div><p>まだ履歴がありません。<br>生成すると自動保存されます。</p></div>
      </div>
    </div>

    <!-- ④ 設定タブ -->
    <div class="section" id="tab-settings">
      <p class="page-title">設定</p>
      <div class="card">
        <div class="settings-row">
          <span class="settings-label">絵文字を使用する</span>
          <div class="toggle on" id="toggle-emoji" onclick="this.classList.toggle('on');settings.emoji=this.classList.contains('on')"></div>
        </div>
        <div class="settings-row">
          <span class="settings-label">生成数</span>
          <div class="segment" style="width:160px;margin:0">
            <button class="seg-btn" data-n="3" onclick="setCount(this)">3件</button>
            <button class="seg-btn active" data-n="5" onclick="setCount(this)">5件</button>
            <button class="seg-btn" data-n="8" onclick="setCount(this)">8件</button>
          </div>
        </div>
        <div class="settings-row">
          <span class="settings-label">文字数</span>
          <div class="segment" style="width:160px;margin:0">
            <button class="seg-btn active" data-len="短め" onclick="setLen(this)">短め</button>
            <button class="seg-btn" data-len="普通" onclick="setLen(this)">普通</button>
            <button class="seg-btn" data-len="長め" onclick="setLen(this)">長め</button>
          </div>
        </div>
        <div class="settings-row">
          <span class="settings-label">コメント数</span>
          <div class="segment" style="width:160px;margin:0">
            <button class="seg-btn" data-cn="3" onclick="setCmtCount(this)">3件</button>
            <button class="seg-btn active" data-cn="5" onclick="setCmtCount(this)">5件</button>
          </div>
        </div>
        <div class="settings-row">
          <span class="settings-label">履歴をクリア</span>
          <button onclick="clearHistory()" style="border:none;background:none;color:#f76a8a;font-size:14px;cursor:pointer;font-family:'Noto Sans JP',sans-serif;">クリア</button>
        </div>
      </div>
      <div class="card">
        <div class="settings-row">
          <span class="settings-label" style="font-size:13px;color:var(--muted)">Threads AI Generator v3.0</span>
          <span class="settings-value">Powered by Claude</span>
        </div>
      </div>
    </div>

  </div><!-- /scroll-area -->

  <!-- Tab bar (4 tabs) -->
  <div class="tab-bar">
    <button class="tab-item active" data-tab="generate" onclick="switchTab(this)">
      <div class="tab-icon">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"/></svg>
      </div>
      <span class="tab-label">生成</span>
    </button>
    <button class="tab-item" data-tab="comment" onclick="switchTab(this)">
      <div class="tab-icon">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg>
      </div>
      <span class="tab-label">コメント</span>
    </button>
    <button class="tab-item" data-tab="history" onclick="switchTab(this)">
      <div class="tab-icon">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><path d="M3 12a9 9 0 1 0 9-9 9.75 9.75 0 0 0-6.74 2.74L3 8"/><path d="M3 3v5h5"/><polyline points="12 7 12 12 15 15"/></svg>
      </div>
      <span class="tab-label">履歴</span>
    </button>
    <button class="tab-item" data-tab="settings" onclick="switchTab(this)">
      <div class="tab-icon">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1-2.83 2.83l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-4 0v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83-2.83l.06-.06A1.65 1.65 0 0 0 4.68 15a1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1 0-4h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 2.83-2.83l.06.06A1.65 1.65 0 0 0 9 4.68a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 4 0v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 2.83l-.06.06A1.65 1.65 0 0 0 19.4 9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 0 4h-.09a1.65 1.65 0 0 0-1.51 1z"/></svg>
      </div>
      <span class="tab-label">設定</span>
    </button>
  </div>

</div>

<script>
// Clock
function updateClock(){const n=new Date();document.getElementById('clock').textContent=n.getHours()+':'+String(n.getMinutes()).padStart(2,'0');}
updateClock();setInterval(updateClock,10000);

let settings={emoji:true,count:5,length:'短め',cmtCount:5};
let historyData=[];
try{historyData=JSON.parse(localStorage.getItem('th_history')||'[]');}catch(e){}

// Tab
const tabTitles={generate:'投稿生成',comment:'コメント生成',history:'生成履歴',settings:'設定'};
function switchTab(btn){
  document.querySelectorAll('.tab-item').forEach(t=>t.classList.remove('active'));
  document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
  btn.classList.add('active');
  const tab=btn.dataset.tab;
  document.getElementById('tab-'+tab).classList.add('active');
  document.getElementById('nav-title').textContent=tabTitles[tab];
  if(tab==='history')renderHistory();
}

// Chips
document.querySelectorAll('#tone-chips .chip').forEach(c=>{
  c.addEventListener('click',()=>{document.querySelectorAll('#tone-chips .chip').forEach(x=>x.classList.remove('active'));c.classList.add('active');});
});
document.querySelectorAll('#cmt-style-chips .cmt-style-chip').forEach(c=>{
  c.addEventListener('click',()=>c.classList.toggle('active'));
});

// Settings
function setCount(btn){btn.closest('.segment').querySelectorAll('.seg-btn').forEach(b=>b.classList.remove('active'));btn.classList.add('active');settings.count=parseInt(btn.dataset.n);}
function setLen(btn){btn.closest('.segment').querySelectorAll('.seg-btn').forEach(b=>b.classList.remove('active'));btn.classList.add('active');settings.length=btn.dataset.len;}
function setCmtCount(btn){btn.closest('.segment').querySelectorAll('.seg-btn').forEach(b=>b.classList.remove('active'));btn.classList.add('active');settings.cmtCount=parseInt(btn.dataset.cn);}

// History
function saveHistory(genre,tone,ideas){
  historyData.unshift({genre,tone,ideas,date:new Date().toLocaleDateString('ja-JP')});
  if(historyData.length>20)historyData=historyData.slice(0,20);
  try{localStorage.setItem('th_history',JSON.stringify(historyData));}catch(e){}
}
function renderHistory(){
  const el=document.getElementById('history-list');
  if(!historyData.length){el.innerHTML='<div class="history-empty"><div class="icon">📝</div><p>まだ履歴がありません。<br>生成すると自動保存されます。</p></div>';return;}
  el.innerHTML=historyData.map((h,i)=>`<div class="hist-card" onclick="showHistDetail(${i})"><div class="hist-meta"><span class="hist-genre">${h.genre} · ${h.tone}</span><span class="hist-date">${h.date}</span></div><div class="hist-preview">${escHtml(h.ideas[0]?.text||'')}</div></div>`).join('');
}
function showHistDetail(i){
  const h=historyData[i];renderResults(h.ideas);
  document.querySelectorAll('.tab-item').forEach(t=>t.classList.remove('active'));
  document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
  document.querySelector('[data-tab="generate"]').classList.add('active');
  document.getElementById('tab-generate').classList.add('active');
  document.getElementById('nav-title').textContent='投稿生成';
  document.querySelector('.scroll-area').scrollTo({top:300,behavior:'smooth'});
}
function clearHistory(){
  if(confirm('履歴をすべて削除しますか？')){historyData=[];try{localStorage.removeItem('th_history');}catch(e){}renderHistory();}
}

// ── 投稿生成 ──
async function generate(){
  const genre=document.getElementById('genre').value;
  const goal=document.getElementById('goal').value;
  const profile=document.getElementById('profile').value;
  const tone=document.querySelector('#tone-chips .chip.active')?.dataset.val||'共感・親近感';
  const btn=document.getElementById('gen-btn');
  const loading=document.getElementById('loading');
  const results=document.getElementById('results');
  btn.disabled=true;loading.classList.add('show');results.innerHTML='';
  const lenNote=settings.length==='短め'?'100文字以内':settings.length==='長め'?'280文字前後':'150〜200文字';
  const emojiNote=settings.emoji?'絵文字を適度に使用':'絵文字なし';
  const prompt=`あなたはThreads（Meta）の投稿専門家です。条件に合う投稿アイデアを${settings.count}つ生成してください。
ジャンル:${genre}/目的:${goal}/投稿者:${profile||'クリエイター・副業ワーカー'}/トーン:${tone}/文字数:${lenNote}/${emojiNote}
JSON配列のみ返してください（説明文・マークダウン不要）:
[{"type":"hook|story|value|question","typeLabel":"フック|ストーリー|価値提供|問いかけ","text":"投稿文"}]
Threadsらしい自然な日本語、いいね・コメントしたくなる内容`;
  try{
    const res=await fetch('https://api.anthropic.com/v1/messages',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({model:'claude-sonnet-4-20250514',max_tokens:1500,messages:[{role:'user',content:prompt}]})});
    const data=await res.json();
    const ideas=JSON.parse(data.content.map(b=>b.text||'').join('').replace(/```json|```/g,'').trim());
    saveHistory(genre,tone,ideas);
    loading.classList.remove('show');btn.disabled=false;
    renderResults(ideas);
    document.querySelector('.scroll-area').scrollTo({top:300,behavior:'smooth'});
  }catch(e){loading.classList.remove('show');btn.disabled=false;results.innerHTML='<div style="text-align:center;padding:32px;color:#f76a8a;font-size:14px">生成に失敗しました。もう一度お試しください。</div>';}
}

function renderResults(ideas){
  const results=document.getElementById('results');
  results.innerHTML=`<div class="results-header"><span>生成されたアイデア</span><span class="results-count">${ideas.length} POSTS</span></div>`+
  ideas.map((idea,i)=>`
    <div class="result-card" style="animation-delay:${i*0.06}s">
      <div class="result-top">
        <span class="result-num">#${String(i+1).padStart(2,'0')}</span>
        <span class="type-pill tp-${idea.type||'hook'}">${idea.typeLabel}</span>
      </div>
      <div class="result-text">${escHtml(idea.text)}</div>
      <div class="result-footer">
        <button class="btn-sm" onclick="copyText(this,\`${escAttr(idea.text)}\`)">
          <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="9" y="9" width="13" height="13" rx="2"/><path d="M5 15H4a2 2 0 01-2-2V4a2 2 0 012-2h9a2 2 0 012 2v1"/></svg>コピー
        </button>
        <button class="btn-sm comment-btn" onclick="genInlineComments(this, \`${escAttr(idea.text)}\`)">
          <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg>コメント生成
        </button>
      </div>
      <div class="comment-section" id="cs-${i}" style="display:none"></div>
    </div>`).join('');
}

// ── インラインコメント生成（投稿カードから） ──
async function genInlineComments(btn, postText){
  const card=btn.closest('.result-card');
  const idx=Array.from(document.querySelectorAll('.result-card')).indexOf(card);
  const csEl=document.getElementById('cs-'+idx);
  if(!csEl)return;

  // Toggle off if already shown
  if(csEl.style.display==='block'&&csEl.innerHTML.trim()){csEl.style.display='none';return;}

  csEl.style.display='block';
  csEl.innerHTML='<div class="comment-loading"><div class="spinner sm"></div>コメント候補を生成中…</div>';
  btn.disabled=true;

  const prompt=`以下のThreads投稿に対する自然で効果的な返信コメントを3つ生成してください。
投稿文:「${postText}」
JSON配列のみ返してください（説明文・マークダウン不要）:
[{"style":"共感|質問|賞賛|価値追加","styleLabel":"共感|質問|賞賛|価値追加","text":"コメント文（50文字以内、自然な日本語）"}]
- 投稿者との関係を深める内容
- スパムっぽくない自然なコメント
- 絵文字を1〜2個使用可`;

  try{
    const res=await fetch('https://api.anthropic.com/v1/messages',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({model:'claude-sonnet-4-20250514',max_tokens:600,messages:[{role:'user',content:prompt}]})});
    const data=await res.json();
    const comments=JSON.parse(data.content.map(b=>b.text||'').join('').replace(/```json|```/g,'').trim());
    const styleClass={共感:'cs-empathy',質問:'cs-question',賞賛:'cs-praise',価値追加:'cs-value'};
    csEl.innerHTML=`<div class="comment-section-header"><svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg>コメント候補</div>`+
    comments.map((c,j)=>`<div class="comment-item" style="animation-delay:${j*0.07}s"><div class="comment-meta"><span class="comment-style ${styleClass[c.style]||'cs-empathy'}">${c.styleLabel}</span><button class="btn-copy-sm" onclick="copyText(this,\`${escAttr(c.text)}\`)">コピー</button></div><div class="comment-text">${escHtml(c.text)}</div></div>`).join('');
    btn.disabled=false;
  }catch(e){csEl.innerHTML='<div style="font-size:13px;color:#f76a8a;padding:8px 0">生成に失敗しました</div>';btn.disabled=false;}
}

// ── コメントタブ：独立コメント生成 ──
async function generateComments(){
  const postText=document.getElementById('cmt-post-input').value.trim();
  if(!postText){alert('投稿文を入力してください');return;}
  const styles=[...document.querySelectorAll('#cmt-style-chips .cmt-style-chip.active')].map(c=>c.dataset.val);
  if(!styles.length){alert('スタイルを1つ以上選択してください');return;}
  const btn=document.getElementById('cmt-btn');
  const loading=document.getElementById('cmt-loading');
  const results=document.getElementById('cmt-results');
  btn.disabled=true;loading.classList.add('show');results.innerHTML='';

  const prompt=`以下のThreads投稿に対する返信コメントを${settings.cmtCount}つ生成してください。
投稿文:「${postText}」
使用するスタイル:${styles.join('、')}
JSON配列のみ返してください（説明文・マークダウン不要）:
[{"style":"スタイル名","styleLabel":"スタイル名","text":"コメント文（60文字以内）"}]
- 投稿者との関係を深めるコメント
- スパムっぽくない自然な日本語
- 絵文字は1〜2個使用可
- 各スタイルをバランスよく使う`;

  try{
    const res=await fetch('https://api.anthropic.com/v1/messages',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({model:'claude-sonnet-4-20250514',max_tokens:800,messages:[{role:'user',content:prompt}]})});
    const data=await res.json();
    const comments=JSON.parse(data.content.map(b=>b.text||'').join('').replace(/```json|```/g,'').trim());
    loading.classList.remove('show');btn.disabled=false;
    const styleClass={共感:'cs-empathy',質問:'cs-question',賞賛:'cs-praise',価値追加:'cs-value',ユーモア:'cs-empathy'};
    results.innerHTML=`<div class="results-header"><span>コメント候補</span><span class="results-count">${comments.length} CMTS</span></div>`+
    comments.map((c,i)=>`<div class="cmt-result-item" style="animation-delay:${i*0.06}s"><div class="comment-meta"><span class="comment-style ${styleClass[c.style]||'cs-empathy'}">${c.styleLabel}</span></div><div class="comment-text">${escHtml(c.text)}</div><div class="copy-row"><button class="btn-sm ok" onclick="copyText(this,\`${escAttr(c.text)}\`)"><svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="9" y="9" width="13" height="13" rx="2"/><path d="M5 15H4a2 2 0 01-2-2V4a2 2 0 012-2h9a2 2 0 012 2v1"/></svg>コピー</button></div></div>`).join('');
    document.querySelector('.scroll-area').scrollTo({top:400,behavior:'smooth'});
  }catch(e){loading.classList.remove('show');btn.disabled=false;results.innerHTML='<div style="text-align:center;padding:32px;color:#f76a8a;font-size:14px">生成に失敗しました。もう一度お試しください。</div>';}
}

function copyText(btn,text){
  navigator.clipboard.writeText(text).then(()=>{
    const orig=btn.innerHTML;
    btn.classList.add('ok');
    btn.innerHTML=btn.innerHTML.replace(/コピー/,'コピー済み');
    setTimeout(()=>{btn.classList.remove('ok');btn.innerHTML=orig;},2000);
  });
}
function escHtml(s){return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');}
function escAttr(s){return s.replace(/`/g,'\\`').replace(/\$/g,'\\$');}
</script>
</body>
</html>
