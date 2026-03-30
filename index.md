<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>地震保険損害調査支援ツール</title>
<style>
:root {
  --primary: #1a3a5c;
  --primary-light: #2a5a8c;
  --accent: #e67e22;
  --success: #27ae60;
  --danger: #c0392b;
  --warning: #f39c12;
  --bg: #f0f4f8;
  --card: #ffffff;
  --border: #ccd6e0;
  --text: #1a2638;
  --text-muted: #5a7a9a;
  --font: 'Hiragino Kaku Gothic ProN','Hiragino Sans','Meiryo',sans-serif;
}
*{box-sizing:border-box;margin:0;padding:0;}
body{font-family:var(--font);background:var(--bg);color:var(--text);min-height:100vh;padding-bottom:40px;}
header{background:linear-gradient(135deg,var(--primary) 0%,var(--primary-light) 100%);color:white;padding:14px 20px;position:sticky;top:0;z-index:100;box-shadow:0 3px 12px rgba(0,0,0,0.25);}
header h1{font-size:16px;font-weight:700;}
header p{font-size:11px;opacity:0.7;margin-top:2px;}
.tab-bar{display:flex;background:var(--primary);overflow-x:auto;-webkit-overflow-scrolling:touch;scrollbar-width:none;}
.tab-bar::-webkit-scrollbar{display:none;}
.tab-btn{flex:0 0 auto;padding:10px 15px;color:rgba(255,255,255,0.6);font-size:12px;font-family:var(--font);background:none;border:none;border-bottom:3px solid transparent;cursor:pointer;white-space:nowrap;-webkit-tap-highlight-color:transparent;}
.tab-btn.active{color:white;border-bottom-color:var(--accent);font-weight:700;}
.tab-content{display:none;padding:14px 14px;}
.tab-content.active{display:block;}
.section-card{background:var(--card);border-radius:12px;margin-bottom:14px;box-shadow:0 2px 8px rgba(0,0,0,0.08);overflow:hidden;}
.section-header{background:var(--primary);color:white;padding:10px 14px;display:flex;align-items:center;gap:8px;}
.section-header h2{font-size:13px;font-weight:700;}
.badge{background:var(--accent);color:white;font-size:10px;padding:2px 7px;border-radius:10px;font-weight:700;}
.section-body{padding:14px;}
.field-group{margin-bottom:10px;}
.field-group label{display:block;font-size:11px;font-weight:700;color:var(--primary);margin-bottom:4px;}
.field-group input,.field-group select{width:100%;padding:10px 12px;border:1.5px solid var(--border);border-radius:8px;font-size:14px;font-family:var(--font);background:#f8fbff;color:var(--text);-webkit-appearance:none;}
.field-group input:focus,.field-group select:focus{border-color:var(--primary);outline:none;background:white;}
.field-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;}
.sep{border-top:1px dashed var(--border);margin:12px 0;}
.toggle-group{display:flex;gap:6px;flex-wrap:wrap;}
.toggle-btn{padding:8px 14px;font-size:12px;font-family:var(--font);font-weight:700;background:white;border:2px solid var(--border);border-radius:8px;color:var(--text-muted);cursor:pointer;-webkit-tap-highlight-color:transparent;}
.toggle-btn.active{background:var(--primary);color:white;border-color:var(--primary);}
/* ▽▲スピナー（上下矢印ボタン）を非表示 */
input[type=number]::-webkit-outer-spin-button,
input[type=number]::-webkit-inner-spin-button{-webkit-appearance:none;margin:0;}
input[type=number]{-moz-appearance:textfield;}
/* エラー表示 */
input.input-error{border-color:var(--danger)!important;background:#fff5f5!important;}
.err-msg{display:none;font-size:11px;color:var(--danger);background:#fff0f0;border-left:3px solid var(--danger);padding:5px 8px;border-radius:4px;margin-bottom:6px;}
.err-msg.show{display:block;}
/* 計算入力 */
.calc-row{display:grid;grid-template-columns:1fr auto 1fr auto;gap:6px;align-items:center;margin-bottom:4px;}
.calc-row input{padding:10px;border:1.5px solid var(--border);border-radius:8px;font-size:15px;font-family:var(--font);background:#f8fbff;text-align:center;}
.calc-slash{font-size:18px;color:var(--text-muted);text-align:center;}
.calc-unit{font-size:11px;color:var(--text-muted);white-space:nowrap;}
.calc-label{font-size:11px;font-weight:700;color:var(--primary);margin-bottom:2px;}
.calc-hint{font-size:10px;color:var(--text-muted);margin-bottom:4px;}
.pct-result{background:var(--primary);color:white;border-radius:8px;padding:8px 12px;font-size:14px;font-weight:700;text-align:center;margin-bottom:12px;}
.pct-result span{font-size:10px;opacity:0.8;display:block;}
.verdict-box{border-radius:12px;padding:14px;text-align:center;margin:8px 0;}
.verdict-box.v-zenzon{background:#c0392b;color:white;}
.verdict-box.v-ohanzong{background:#e67e22;color:white;}
.verdict-box.v-kohanzong{background:#f39c12;color:white;}
.verdict-box.v-ichibuzon{background:#27ae60;color:white;}
.verdict-box.v-muse{background:#7f8c8d;color:white;}
.verdict-box.v-none{background:#ecf0f1;color:#999;}
.verdict-box .v-pct{font-size:22px;font-weight:800;}
.verdict-box .v-name{font-size:15px;font-weight:700;margin-top:4px;}
.verdict-box .v-sub{font-size:10px;opacity:0.85;margin-top:2px;}
.note{background:#fff8e1;border-left:3px solid var(--warning);padding:8px 10px;font-size:11px;border-radius:4px;margin-top:6px;}
.kijun-tab-bar{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:12px;}
.kijun-tab{padding:6px 12px;font-size:11px;font-weight:700;font-family:var(--font);background:white;border:1.5px solid var(--border);border-radius:20px;color:var(--text-muted);cursor:pointer;}
.kijun-tab.active{background:var(--primary);color:white;border-color:var(--primary);}
.kijun-panel{display:none;}
.kijun-panel.active{display:block;}
.kijun-subtitle{font-size:12px;font-weight:800;color:var(--primary);border-left:4px solid var(--accent);padding-left:8px;margin:12px 0 6px;}
.kijun-table{width:100%;border-collapse:collapse;font-size:11px;margin-bottom:8px;}
.kijun-table th{background:var(--primary);color:white;padding:6px 4px;text-align:center;font-size:10px;}
.kijun-table td{border:1px solid var(--border);padding:5px 4px;text-align:center;}
.kijun-table tr:nth-child(even) td{background:#f8fbff;}
.kijun-formula{background:#f0f8e8;border:1px solid #b8dca0;border-radius:8px;padding:10px 12px;font-size:12px;line-height:1.9;color:#2a4a1a;margin-bottom:8px;}
.kijun-note{font-size:10px;color:var(--text-muted);margin-bottom:6px;line-height:1.6;}
/* 印刷モーダル */
.modal-overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,0.5);z-index:200;overflow-y:auto;-webkit-overflow-scrolling:touch;}
.modal-overlay.open{display:block;}
.modal-inner{background:white;margin:10px;border-radius:12px;overflow:hidden;}
.modal-bar{position:sticky;top:0;background:var(--primary);padding:10px 14px;display:flex;gap:8px;z-index:10;}
.modal-bar button{padding:9px 16px;font-size:13px;font-weight:700;font-family:var(--font);border:none;border-radius:8px;cursor:pointer;}
.btn-print{background:var(--accent);color:white;}
.btn-close-modal{background:rgba(255,255,255,0.2);color:white;}
/* ===== 帳票スタイル（A4縦2枚・大きめ文字で高齢者でも読みやすく） ===== */
#print-area{
  font-family:'Hiragino Mincho ProN','Yu Mincho','游明朝','MS明朝',serif;
  font-size:10pt;color:#000;background:white;
}
/* A4縦1枚分のページブロック */
.page-block{
  width:190mm;
  min-height:267mm;
  padding:8mm 10mm;
  margin:0 auto 8mm auto;
  background:white;
  box-sizing:border-box;
  border:1px solid #ccc; /* 画面プレビュー用の枠（印刷時は非表示） */
}
/* 帳票テーブル共通 */
.ht{width:100%;border-collapse:collapse;margin-bottom:5px;}
.ht td,.ht th{border:1px solid #444;padding:4px 5px;font-size:10pt;vertical-align:middle;line-height:1.5;}
.ht .lbl{background:#efefef;font-weight:bold;white-space:nowrap;}
.ht .lbl2{background:#efefef;font-weight:bold;}
.ht .center{text-align:center;}
.ht .right{text-align:right;}
.ht .sm{font-size:9pt;}
.ht .xsm{font-size:8pt;}
/* ページタイトル */
.page-title{font-size:13pt;font-weight:bold;text-align:center;margin:4px 0 6px;letter-spacing:2px;}
.page-subtitle{font-size:10pt;font-weight:bold;border-left:4px solid #1a3a5c;padding-left:6px;margin:10px 0 5px;background:#f0f4f8;padding:4px 4px 4px 8px;}
/* 認定区分マーカー */
.vmark-row{display:flex;gap:4px;flex-wrap:wrap;margin:3px 0;}
.vmark{border:1.5px solid #555;padding:2px 7px;font-size:9pt;white-space:nowrap;border-radius:2px;}
.vmark.hit{background:#000;color:#fff;font-weight:bold;}
/* 特記事項欄 */
.toki-box{border:1px solid #444;min-height:50px;padding:5px 7px;margin-bottom:5px;font-size:10pt;}
/* 注記 */
.chuki{font-size:8.5pt;line-height:1.7;margin-bottom:4px;}
.footer-notes-list{font-size:8.5pt;line-height:1.8;}
/* ページ区切り */
.page-break{page-break-after:always;}
@media print {
  header,.tab-bar,.modal-bar{display:none!important;}
  .modal-overlay{position:static!important;background:none!important;}
  .modal-inner{margin:0!important;border-radius:0!important;box-shadow:none!important;}
  .page-block{border:none!important;margin:0 auto!important;padding:8mm 10mm!important;}
  body{background:white!important;}
  .page-break{page-break-after:always;}
}
.btn-accent{display:block;width:100%;padding:14px;background:var(--accent);color:white;font-size:14px;font-weight:700;font-family:var(--font);border:none;border-radius:10px;cursor:pointer;text-align:center;}
.summary-row{display:flex;justify-content:space-between;align-items:center;padding:8px 0;border-bottom:1px solid var(--border);font-size:13px;}
.summary-val{font-weight:700;color:var(--primary);}
</style>
</head>
<body>

<header>
  <h1>🏠 地震保険損害調査支援ツール</h1>
  <p>木造建物（在来軸組・枠組壁）対応 ／ オフライン動作</p>
</header>

<div class="tab-bar">
  <button class="tab-btn active" onclick="showTab('tab-basic',this)">①基本情報</button>
  <button class="tab-btn" onclick="showTab('tab-damage',this)">②建物損害</button>
  <button class="tab-btn" onclick="showTab('tab-dosan',this)">③生活動産</button>
  <button class="tab-btn" onclick="showTab('tab-special',this)">④特殊事由</button>
  <button class="tab-btn" onclick="showTab('tab-result',this)">⑤判定結果</button>
  <button class="tab-btn" onclick="showTab('tab-kijun',this)">⑥認定基準</button>
</div>

<!-- ① 基本情報 -->
<div id="tab-basic" class="tab-content active">
  <div class="section-card">
    <div class="section-header"><h2>基本情報</h2></div>
    <div class="section-body">
      <div class="field-grid">
        <div class="field-group"><label>会社名</label><input type="text" id="company" placeholder="損害保険ジャパン㈱" oninput="autoSave()"></div>
        <div class="field-group"><label>証券番号</label><input type="text" id="policy_no" placeholder="123456789" oninput="autoSave()"></div>
      </div>
      <div class="field-group"><label>契約者・被保険者名</label><input type="text" id="insured" placeholder="氏名" oninput="autoSave()"></div>
      <div class="field-group"><label>目的物所在地</label><input type="text" id="address" placeholder="都道府県　市区郡　町村" oninput="autoSave()"></div>
      <div class="field-grid">
        <div class="field-group"><label>調査日</label><input type="date" id="survey_date" oninput="autoSave()"></div>
        <div class="field-group"><label>罹災日</label><input type="date" id="quake_date" oninput="autoSave()"></div>
      </div>
      <div class="field-group"><label>調査担当者名</label><input type="text" id="surveyor" placeholder="氏名" oninput="autoSave()"></div>
      <div class="field-group"><label>所属（調査会社名）</label><input type="text" id="surveyor_org" placeholder="調査会社名" oninput="autoSave()"></div>
      <div class="field-group">
        <label>階数</label>
        <select id="floors" onchange="autoSave()">
          <option value="1">平家（1階建て）</option>
          <option value="2" selected>2階建て</option>
          <option value="3">3階建て</option>
        </select>
      </div>
      <div class="sep"></div>
      <div class="field-group">
        <label>建物構造（工法）</label>
        <div class="toggle-group">
          <button class="toggle-btn active" id="btn-zairaiku" onclick="setStructure('zairaiku')">在来軸組工法等</button>
          <button class="toggle-btn" id="btn-waku" onclick="setStructure('waku')">枠組壁工法</button>
        </div>
      </div>
      <div class="field-grid">
        <div class="field-group"><label>地震保険金額（千円）</label><input type="number" id="ins_amount" placeholder="0" inputmode="numeric" min="0" oninput="clampPositive(this);autoSave()"></div>
        <div class="field-group"><label>保険価額（千円）</label><input type="number" id="ins_price" placeholder="0" inputmode="numeric" min="0" oninput="clampPositive(this);autoSave()"></div>
      </div>
      <div class="field-group"><label>生活用動産 保険金額（千円）</label><input type="number" id="ins_amount_dosan" placeholder="0" inputmode="numeric" min="0" oninput="clampPositive(this);autoSave()"></div>
      <div class="sep"></div>
      <div class="field-group">
        <label>契約始期</label>
        <div class="toggle-group">
          <button class="toggle-btn active" id="btn-2017" onclick="setTerm('2017')">2017年1月1日以降</button>
          <button class="toggle-btn" id="btn-2016" onclick="setTerm('2016')">2016年12月31日以前</button>
        </div>
      </div>
      <div class="note">💡 契約始期により損害認定基準が変わります。正しく選択してください。</div>
    </div>
  </div>
</div>

<!-- ② 建物損害 -->
<div id="tab-damage" class="tab-content">
  <div id="sec-zairaiku">
    <div class="section-card">
      <div class="section-header"><h2>建物損害（在来軸組工法等）</h2><span class="badge">自動計算</span></div>
      <div class="section-body">
        <div class="note" style="margin-bottom:10px;">💡 マイナスは入力できません。損傷値が全体値を超えると赤で警告します。面積・長さは小数点第2位まで（例：12.34）入力できます。</div>

        <div class="calc-label">① 軸組　損傷柱本数 ／ 全柱本数</div>
        <div class="calc-hint">※整数のみ</div>
        <div class="calc-row">
          <input type="number" id="z_jiku_dam" inputmode="numeric" placeholder="0" min="0" step="1"
            oninput="clampPositive(this);checkPair('z_jiku_dam','z_jiku_total','err_z_jiku');calcZairaiku()"
            onblur="formatOnBlur(this,0);checkPair('z_jiku_dam','z_jiku_total','err_z_jiku');calcZairaiku()">
          <span class="calc-slash">/</span>
          <input type="number" id="z_jiku_total" inputmode="numeric" placeholder="0" min="0" step="1"
            oninput="clampPositive(this);checkPair('z_jiku_dam','z_jiku_total','err_z_jiku');calcZairaiku()"
            onblur="formatOnBlur(this,0);checkPair('z_jiku_dam','z_jiku_total','err_z_jiku');calcZairaiku()">
          <span class="calc-unit">本</span>
        </div>
        <div class="err-msg" id="err_z_jiku">⚠️ 損傷本数が全柱本数を超えています</div>
        <div class="pct-result" id="z_jiku_pct"><span>軸組 損傷割合</span>— %</div>

        <div class="calc-label">② 基礎　損傷コンクリート長さ ／ 外周布基礎長さ</div>
        <div class="calc-hint">※小数点第2位まで入力可（例：12.34）</div>
        <div class="calc-row">
          <input type="number" id="z_kiso_dam" inputmode="decimal" placeholder="0.00" min="0" step="0.01"
            oninput="clampPositive(this);checkPair('z_kiso_dam','z_kiso_total','err_z_kiso');calcZairaiku()"
            onblur="formatOnBlur(this,2);checkPair('z_kiso_dam','z_kiso_total','err_z_kiso');calcZairaiku()">
          <span class="calc-slash">/</span>
          <input type="number" id="z_kiso_total" inputmode="decimal" placeholder="0.00" min="0" step="0.01"
            oninput="clampPositive(this);checkPair('z_kiso_dam','z_kiso_total','err_z_kiso');calcZairaiku()"
            onblur="formatOnBlur(this,2);checkPair('z_kiso_dam','z_kiso_total','err_z_kiso');calcZairaiku()">
          <span class="calc-unit">m</span>
        </div>
        <div class="err-msg" id="err_z_kiso">⚠️ 損傷長さが全周長さを超えています</div>
        <div class="pct-result" id="z_kiso_pct"><span>基礎 損傷割合</span>— %</div>

        <div class="calc-label">③ 屋根　葺替え面積 ／ 全屋根面積</div>
        <div class="calc-hint">※小数点第2位まで入力可（例：45.67）</div>
        <div class="calc-row">
          <input type="number" id="z_yane_dam" inputmode="decimal" placeholder="0.00" min="0" step="0.01"
            oninput="clampPositive(this);checkPair('z_yane_dam','z_yane_total','err_z_yane');calcZairaiku()"
            onblur="formatOnBlur(this,2);checkPair('z_yane_dam','z_yane_total','err_z_yane');calcZairaiku()">
          <span class="calc-slash">/</span>
          <input type="number" id="z_yane_total" inputmode="decimal" placeholder="0.00" min="0" step="0.01"
            oninput="clampPositive(this);checkPair('z_yane_dam','z_yane_total','err_z_yane');calcZairaiku()"
            onblur="formatOnBlur(this,2);checkPair('z_yane_dam','z_yane_total','err_z_yane');calcZairaiku()">
          <span class="calc-unit">㎡</span>
        </div>
        <div class="err-msg" id="err_z_yane">⚠️ 葺替え面積が全屋根面積を超えています</div>
        <div class="pct-result" id="z_yane_pct"><span>屋根 損傷割合</span>— %</div>

        <div class="calc-label">④ 外壁　損傷外壁面積 ／ 全外壁面積</div>
        <div class="calc-hint">※小数点第2位まで入力可（例：23.45）</div>
        <div class="calc-row">
          <input type="number" id="z_gaiheki_dam" inputmode="decimal" placeholder="0.00" min="0" step="0.01"
            oninput="clampPositive(this);checkPair('z_gaiheki_dam','z_gaiheki_total','err_z_gaiheki');calcZairaiku()"
            onblur="formatOnBlur(this,2);checkPair('z_gaiheki_dam','z_gaiheki_total','err_z_gaiheki');calcZairaiku()">
          <span class="calc-slash">/</span>
          <input type="number" id="z_gaiheki_total" inputmode="decimal" placeholder="0.00" min="0" step="0.01"
            oninput="clampPositive(this);checkPair('z_gaiheki_dam','z_gaiheki_total','err_z_gaiheki');calcZairaiku()"
            onblur="formatOnBlur(this,2);checkPair('z_gaiheki_dam','z_gaiheki_total','err_z_gaiheki');calcZairaiku()">
          <span class="calc-unit">㎡</span>
        </div>
        <div class="err-msg" id="err_z_gaiheki">⚠️ 損傷面積が全外壁面積を超えています</div>
        <div class="pct-result" id="z_gaiheki_pct"><span>外壁 損傷割合</span>— %</div>

        <div class="sep"></div>
        <div id="z_result_box" class="verdict-box v-none">
          <div class="v-pct" id="res_building">— %</div>
          <div class="v-name" id="res_building_verdict">—</div>
          <div class="v-sub">合計損害割合（建物）</div>
        </div>
      </div>
    </div>
  </div>

  <div id="sec-waku" style="display:none;">
    <div class="section-card">
      <div class="section-header"><h2>建物損害（枠組壁工法）</h2><span class="badge">自動計算</span></div>
      <div class="section-body">
        <div class="note" style="margin-bottom:10px;">💡 マイナスは入力できません。損傷値が全体値を超えると赤で警告します。</div>

        <div class="calc-label">① 外壁　1階外壁損傷長さ ／ 1階の外周延べ長さ</div>
        <div class="calc-hint">※小数点第2位まで入力可</div>
        <div class="calc-row">
          <input type="number" id="w_gai_dam" inputmode="decimal" placeholder="0.00" min="0" step="0.01"
            oninput="clampPositive(this);checkPair('w_gai_dam','w_gai_total','err_w_gai');calcWaku()"
            onblur="formatOnBlur(this,2);checkPair('w_gai_dam','w_gai_total','err_w_gai');calcWaku()">
          <span class="calc-slash">/</span>
          <input type="number" id="w_gai_total" inputmode="decimal" placeholder="0.00" min="0" step="0.01"
            oninput="clampPositive(this);checkPair('w_gai_dam','w_gai_total','err_w_gai');calcWaku()"
            onblur="formatOnBlur(this,2);checkPair('w_gai_dam','w_gai_total','err_w_gai');calcWaku()">
          <span class="calc-unit">m</span>
        </div>
        <div class="err-msg" id="err_w_gai">⚠️ 損傷長さが全周長さを超えています</div>
        <div class="pct-result" id="w_gai_pct"><span>外壁 損傷割合</span>— %</div>

        <div class="calc-label">② 内壁　入隅の損傷数×0.5 ／ 1階の入隅全箇所数</div>
        <div class="calc-hint">※整数のみ</div>
        <div class="calc-row">
          <input type="number" id="w_nai_dam" inputmode="numeric" placeholder="0" min="0" step="1"
            oninput="clampPositive(this);checkPair('w_nai_dam','w_nai_total','err_w_nai');calcWaku()"
            onblur="formatOnBlur(this,0);checkPair('w_nai_dam','w_nai_total','err_w_nai');calcWaku()">
          <span class="calc-slash">/</span>
          <input type="number" id="w_nai_total" inputmode="numeric" placeholder="0" min="0" step="1"
            oninput="clampPositive(this);checkPair('w_nai_dam','w_nai_total','err_w_nai');calcWaku()"
            onblur="formatOnBlur(this,0);checkPair('w_nai_dam','w_nai_total','err_w_nai');calcWaku()">
          <span class="calc-unit">箇所</span>
        </div>
        <div class="err-msg" id="err_w_nai">⚠️ 損傷箇所数が全箇所数を超えています</div>
        <div class="pct-result" id="w_nai_pct"><span>内壁 損傷割合</span>— %</div>

        <div class="calc-label">③ 基礎　損傷コンクリート長さ ／ 外周布基礎長さ</div>
        <div class="calc-hint">※小数点第2位まで入力可</div>
        <div class="calc-row">
          <input type="number" id="w_kiso_dam" inputmode="decimal" placeholder="0.00" min="0" step="0.01"
            oninput="clampPositive(this);checkPair('w_kiso_dam','w_kiso_total','err_w_kiso');calcWaku()"
            onblur="formatOnBlur(this,2);checkPair('w_kiso_dam','w_kiso_total','err_w_kiso');calcWaku()">
          <span class="calc-slash">/</span>
          <input type="number" id="w_kiso_total" inputmode="decimal" placeholder="0.00" min="0" step="0.01"
            oninput="clampPositive(this);checkPair('w_kiso_dam','w_kiso_total','err_w_kiso');calcWaku()"
            onblur="formatOnBlur(this,2);checkPair('w_kiso_dam','w_kiso_total','err_w_kiso');calcWaku()">
          <span class="calc-unit">m</span>
        </div>
        <div class="err-msg" id="err_w_kiso">⚠️ 損傷長さが全周長さを超えています</div>
        <div class="pct-result" id="w_kiso_pct"><span>基礎 損傷割合</span>— %</div>

        <div class="calc-label">④ 屋根　葺替え面積 ／ 全屋根面積</div>
        <div class="calc-hint">※小数点第2位まで入力可</div>
        <div class="calc-row">
          <input type="number" id="w_yane_dam" inputmode="decimal" placeholder="0.00" min="0" step="0.01"
            oninput="clampPositive(this);checkPair('w_yane_dam','w_yane_total','err_w_yane');calcWaku()"
            onblur="formatOnBlur(this,2);checkPair('w_yane_dam','w_yane_total','err_w_yane');calcWaku()">
          <span class="calc-slash">/</span>
          <input type="number" id="w_yane_total" inputmode="decimal" placeholder="0.00" min="0" step="0.01"
            oninput="clampPositive(this);checkPair('w_yane_dam','w_yane_total','err_w_yane');calcWaku()"
            onblur="formatOnBlur(this,2);checkPair('w_yane_dam','w_yane_total','err_w_yane');calcWaku()">
          <span class="calc-unit">㎡</span>
        </div>
        <div class="err-msg" id="err_w_yane">⚠️ 葺替え面積が全屋根面積を超えています</div>
        <div class="pct-result" id="w_yane_pct"><span>屋根 損傷割合</span>— %</div>

        <div class="sep"></div>
        <div id="w_result_box" class="verdict-box v-none">
          <div class="v-pct" id="res_building_w">— %</div>
          <div class="v-name" id="res_building_verdict_w">—</div>
          <div class="v-sub">合計損害割合（建物）</div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ③ 生活用動産 -->
<div id="tab-dosan" class="tab-content">
  <div class="section-card">
    <div class="section-header"><h2>生活用動産</h2><span class="badge">自動計算</span></div>
    <div class="section-body">
      <div class="note" style="margin-bottom:10px;">💡 損傷数が存在数を超えると警告が出ます（整数で入力してください）</div>
      <div id="dosan-sections"></div>
      <div class="sep"></div>
      <div class="verdict-box v-none" id="dosan_result_box">
        <div class="v-pct" id="res_dosan">— %</div>
        <div class="v-name" id="res_dosan_verdict">—</div>
        <div class="v-sub">損害割合（生活用動産）</div>
      </div>
    </div>
  </div>
</div>

<!-- ④ 特殊事由 -->
<div id="tab-special" class="tab-content">
  <div class="section-card">
    <div class="section-header"><h2>津波による損害</h2><span class="badge">自動判定</span></div>
    <div class="section-body">
      <div class="field-group">
        <label>津波浸水の高さ（cm）※整数で入力</label>
        <input type="number" id="tsunami_cm" inputmode="numeric" placeholder="0" min="0" step="1" oninput="clampPositive(this);calcTsunami()">
      </div>
      <div class="verdict-box v-none" id="tsunami_result_box">
        <div class="v-pct" id="res_tsunami">入力してください</div>
        <div class="v-name" id="res_tsunami_verdict">—</div>
        <div class="v-sub">津波損害認定</div>
      </div>
      <div class="note">💡 浸水高さが基礎の高さ未満の場合は「無責」。浸水損害以外には適用しない。</div>
    </div>
  </div>
  <div class="section-card">
    <div class="section-header"><h2>液状化による損害</h2><span class="badge">自動判定</span></div>
    <div class="section-body">
      <div class="field-group">
        <label>傾斜（/100 rad）</label>
        <input type="number" id="eki_keisha" inputmode="decimal" placeholder="0.0" min="0" step="0.1" oninput="clampPositive(this);calcEkijo()">
      </div>
      <div class="field-group">
        <label>最大沈下量（cm）</label>
        <input type="number" id="eki_chinka" inputmode="decimal" placeholder="0.0" min="0" step="0.1" oninput="clampPositive(this);calcEkijo()">
      </div>
      <div class="verdict-box v-none" id="eki_result_box">
        <div class="v-pct" id="res_eki">入力してください</div>
        <div class="v-name" id="res_eki_verdict">—</div>
        <div class="v-sub">液状化損害認定（高い方を採用）</div>
      </div>
    </div>
  </div>
</div>

<!-- ⑤ 判定結果 -->
<div id="tab-result" class="tab-content">
  <div class="section-card">
    <div class="section-header"><h2>判定結果サマリー</h2></div>
    <div class="section-body">
      <div class="summary-row"><span>建物損害割合</span><span class="summary-val" id="sum_building">—</span></div>
      <div class="summary-row"><span>建物認定区分</span><span class="summary-val" id="sum_building_v">—</span></div>
      <div class="summary-row"><span>動産損害割合</span><span class="summary-val" id="sum_dosan">—</span></div>
      <div class="summary-row"><span>動産認定区分</span><span class="summary-val" id="sum_dosan_v">—</span></div>
      <div class="summary-row"><span>津波判定</span><span class="summary-val" id="sum_tsunami">—</span></div>
      <div class="summary-row"><span>液状化判定</span><span class="summary-val" id="sum_eki">—</span></div>
      <div class="sep"></div>
      <button class="btn-accent" onclick="openPrintModal()">📄 帳票（報告書）を出力する</button>
    </div>
  </div>
</div>

<!-- ⑥ 認定基準 -->
<div id="tab-kijun" class="tab-content">
  <div class="section-card">
    <div class="section-header"><h2>認定基準早見表</h2></div>
    <div class="section-body">
      <div class="kijun-tab-bar">
        <button class="kijun-tab active" onclick="switchKijun('kj-zairaiku',this)">建物（在来）</button>
        <button class="kijun-tab" onclick="switchKijun('kj-waku',this)">建物（枠組）</button>
        <button class="kijun-tab" onclick="switchKijun('kj-dosan',this)">生活動産</button>
        <button class="kijun-tab" onclick="switchKijun('kj-tsunami',this)">津波</button>
        <button class="kijun-tab" onclick="switchKijun('kj-eki',this)">液状化</button>
      </div>
      <div id="kj-zairaiku" class="kijun-panel active">
        <div class="kijun-subtitle">損害認定区分（2017年1月1日以降始期）</div>
        <table class="kijun-table"><tr><th>区分</th><th>合計損害割合</th></tr>
          <tr><td>全損</td><td>50%以上</td></tr><tr><td>大半損</td><td>40%以上50%未満</td></tr>
          <tr><td>小半損</td><td>20%以上40%未満</td></tr><tr><td>一部損</td><td>3%以上20%未満</td></tr>
          <tr><td>無責</td><td>3%未満</td></tr></table>
        <div class="kijun-subtitle">損害認定区分（2016年12月31日以前始期）</div>
        <table class="kijun-table"><tr><th>区分</th><th>合計損害割合</th></tr>
          <tr><td>全損</td><td>50%以上</td></tr><tr><td>半損</td><td>20%以上50%未満</td></tr>
          <tr><td>一部損</td><td>3%以上20%未満</td></tr><tr><td>無責</td><td>3%未満</td></tr></table>
        <div class="kijun-subtitle">部位別 損傷割合の求め方（在来軸組）</div>
        <div class="kijun-formula">軸組：損傷柱本数 ÷ 全柱本数 × 100（%）<br>基礎：損傷コンクリート長さ ÷ 外周布基礎長さ × 100（%）<br>屋根：葺替え面積 ÷ 全屋根面積 × 100（%）<br>外壁：損傷外壁面積 ÷ 全外壁面積 × 100（%）<br>※すべて小数点以下第1位未満切り上げ</div>
      </div>
      <div id="kj-waku" class="kijun-panel">
        <div class="kijun-subtitle">損害認定区分（2017年以降始期）</div>
        <table class="kijun-table"><tr><th>区分</th><th>合計損害割合</th></tr>
          <tr><td>全損</td><td>50%以上</td></tr><tr><td>大半損</td><td>40%以上50%未満</td></tr>
          <tr><td>小半損</td><td>20%以上40%未満</td></tr><tr><td>一部損</td><td>3%以上20%未満</td></tr>
          <tr><td>無責</td><td>3%未満</td></tr></table>
        <div class="kijun-subtitle">部位別 損傷割合の求め方（枠組壁）</div>
        <div class="kijun-formula">外壁：1階外壁損傷長さ ÷ 1階外周延べ長さ × 100（%）<br>内壁：入隅損傷数×0.5 ÷ 1階入隅全箇所数 × 100（%）<br>基礎：損傷コンクリート長さ ÷ 外周布基礎長さ × 100（%）<br>屋根：葺替え面積 ÷ 全屋根面積 × 100（%）<br>※すべて小数点以下第1位未満切り上げ</div>
      </div>
      <div id="kj-dosan" class="kijun-panel">
        <div class="kijun-subtitle">損害認定区分（2017年以降始期）</div>
        <table class="kijun-table"><tr><th>区分</th><th>損害割合</th></tr>
          <tr><td>全損</td><td>80%以上</td></tr><tr><td>大半損</td><td>60%以上80%未満</td></tr>
          <tr><td>小半損</td><td>30%以上60%未満</td></tr><tr><td>一部損</td><td>10%以上30%未満</td></tr>
          <tr><td>無責</td><td>10%未満</td></tr></table>
        <div class="kijun-subtitle">5分類と構成割合係数</div>
        <table class="kijun-table"><tr><th>分類</th><th>品目数</th><th>構成割合</th></tr>
          <tr><td>食器類</td><td>2品目</td><td>5%</td></tr>
          <tr><td>電気器具類</td><td>10品目</td><td>20%</td></tr>
          <tr><td>家具類</td><td>10品目</td><td>10%</td></tr>
          <tr><td>身の回り品</td><td>10品目</td><td>35%</td></tr>
          <tr><td>衣類</td><td>10品目</td><td>30%</td></tr></table>
      </div>
      <div id="kj-tsunami" class="kijun-panel">
        <div class="kijun-subtitle">津波損害認定（2017年以降始期）</div>
        <table class="kijun-table"><tr><th>区分</th><th>通常建物</th><th>平屋建て</th></tr>
          <tr><td>全損</td><td>180cm以上（床上）または地盤面225cm以上</td><td>100cm以上（床上）または地盤面145cm以上</td></tr>
          <tr><td>大半損</td><td>115〜180cm未満（床上）または地盤面160〜225cm未満</td><td>75〜100cm未満（床上）または地盤面80〜145cm未満</td></tr>
          <tr><td>小半損</td><td>床上浸水または地盤面45cm超〜160cm未満</td><td>床上浸水または地盤面45cm超〜80cm未満</td></tr>
          <tr><td>一部損</td><td colspan="2">基礎の高さ以上（全損・大半損・小半損を除く）</td></tr>
          <tr><td>無責</td><td colspan="2">基礎の高さ未満の浸水</td></tr></table>
      </div>
      <div id="kj-eki" class="kijun-panel">
        <div class="kijun-subtitle">傾斜による認定（2017年以降始期）</div>
        <table class="kijun-table"><tr><th>区分</th><th>傾斜（/100 rad）</th></tr>
          <tr><td>全損</td><td>1.7/100超</td></tr><tr><td>大半損</td><td>1.4/100超〜1.7/100以下</td></tr>
          <tr><td>小半損</td><td>0.9/100超〜1.4/100以下</td></tr><tr><td>一部損</td><td>0.4/100超〜0.9/100以下</td></tr>
          <tr><td>無責</td><td>0.4/100以下</td></tr></table>
        <div class="kijun-subtitle">最大沈下量による認定（2017年以降始期）</div>
        <table class="kijun-table"><tr><th>区分</th><th>最大沈下量（cm）</th></tr>
          <tr><td>全損</td><td>30cm超</td></tr><tr><td>大半損</td><td>20cm超〜30cm以下</td></tr>
          <tr><td>小半損</td><td>15cm超〜20cm以下</td></tr><tr><td>一部損</td><td>10cm超〜15cm以下</td></tr>
          <tr><td>無責</td><td>10cm以下</td></tr></table>
        <div class="kijun-note">※傾斜・最大沈下量の両方を求め、高い方を採用。加算しない。</div>
      </div>
    </div>
  </div>
</div>

<!-- 印刷モーダル -->
<div id="printModal" class="modal-overlay">
  <div class="modal-inner">
    <div class="modal-bar">
      <button class="btn-print" onclick="window.print()">🖨️ 印刷／PDF保存</button>
      <button class="btn-close-modal" onclick="closePrintModal()">✕ 閉じる</button>
    </div>
    <div id="print-area"></div>
  </div>
</div>

<script>
const state = { structure:'zairaiku', term:'2017' };

function showTab(id, btn) {
  document.querySelectorAll('.tab-content').forEach(t=>t.classList.remove('active'));
  document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  if(btn) btn.classList.add('active');
  if(id==='tab-result') updateSummary();
}
function switchKijun(id, btn) {
  document.querySelectorAll('.kijun-panel').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.kijun-tab').forEach(b=>b.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  if(btn) btn.classList.add('active');
}
function setStructure(s) {
  state.structure=s;
  document.getElementById('btn-zairaiku').classList.toggle('active',s==='zairaiku');
  document.getElementById('btn-waku').classList.toggle('active',s==='waku');
  document.getElementById('sec-zairaiku').style.display=s==='zairaiku'?'':'none';
  document.getElementById('sec-waku').style.display=s==='waku'?'':'none';
}
function setTerm(t) {
  state.term=t;
  document.getElementById('btn-2017').classList.toggle('active',t==='2017');
  document.getElementById('btn-2016').classList.toggle('active',t==='2016');
}

// ===== バリデーション =====
function clampPositive(el) {
  const v=parseFloat(el.value);
  if(!isNaN(v)&&v<0) el.value=0;
}

// フォーカスを外したとき自動で小数点整形（例: 12 → 12.00）
// decimals=0: 整数項目, decimals=2: 面積・長さ項目
function formatOnBlur(el, decimals) {
  const v = parseFloat(el.value);
  if(isNaN(v) || el.value === '') return;
  el.value = decimals === 0 ? String(Math.round(Math.abs(v))) : Math.abs(v).toFixed(decimals);
}

function checkPair(damId, totId, errId) {
  const d=parseFloat(document.getElementById(damId).value);
  const t=parseFloat(document.getElementById(totId).value);
  const over=!isNaN(d)&&!isNaN(t)&&t>0&&d>t;
  document.getElementById(errId).classList.toggle('show',over);
  document.getElementById(damId).classList.toggle('input-error',over);
  document.getElementById(totId).classList.toggle('input-error',over);
}

function ceil1(v){return Math.ceil(v*10)/10;}

function getBuildingVerdict(pct,term){
  if(term==='2017'){
    if(pct>=50)return{name:'全損',cls:'v-zenzon'};
    if(pct>=40)return{name:'大半損',cls:'v-ohanzong'};
    if(pct>=20)return{name:'小半損',cls:'v-kohanzong'};
    if(pct>=3)return{name:'一部損',cls:'v-ichibuzon'};
    return{name:'無責',cls:'v-muse'};
  }else{
    if(pct>=50)return{name:'全損',cls:'v-zenzon'};
    if(pct>=20)return{name:'半損',cls:'v-ohanzong'};
    if(pct>=3)return{name:'一部損',cls:'v-ichibuzon'};
    return{name:'無責',cls:'v-muse'};
  }
}

function getZairaikuCoeff(partIdx,physPct,floors){
  const fi=Math.min(['1','2','3'].indexOf(floors),2);
  const T={
    0:[[3,7,8,8],[5,12,14,14],[10,19,21,21],[15,25,27,27],[20,29,31,31],[25,33,36,36],[30,37,40,40],[40,41,45,45]],
    1:[[5,3,2,2],[10,7,7,8],[20,8,8,9],[30,9,10,10],[50,10,11,12],[100,Infinity,Infinity,Infinity]],
    2:[[10,1,1,1],[20,2,2,2],[30,4,2,2],[50,5,3,3],[70,7,4,4],[100,Infinity,Infinity,Infinity]],
    3:[[10,2,2,2],[20,3,3,3],[30,5,5,5],[50,7,7,7],[70,11,11,11],[100,Infinity,Infinity,Infinity]],
  };
  for(const row of T[partIdx]){
    if(physPct<=row[0]){const v=row[fi+1];return v===Infinity?null:v;}
  }
  return null;
}

function calcZairaiku(){
  const floors=document.getElementById('floors').value;
  const parts=[
    {dam:'z_jiku_dam',tot:'z_jiku_total',el:'z_jiku_pct',idx:0},
    {dam:'z_kiso_dam',tot:'z_kiso_total',el:'z_kiso_pct',idx:1},
    {dam:'z_yane_dam',tot:'z_yane_total',el:'z_yane_pct',idx:2},
    {dam:'z_gaiheki_dam',tot:'z_gaiheki_total',el:'z_gaiheki_pct',idx:3},
  ];
  let total=0,hasData=false,hasErr=false;
  for(const p of parts){
    const d=parseFloat(document.getElementById(p.dam).value);
    const t=parseFloat(document.getElementById(p.tot).value);
    const el=document.getElementById(p.el);
    if(isNaN(d)||isNaN(t)||t<=0){el.innerHTML='<span>損傷割合</span>— %';continue;}
    if(d>t){hasErr=true;el.innerHTML='<span>損傷割合</span>⚠️ 入力エラー';continue;}
    hasData=true;
    const pp=ceil1(d/t*100);
    const c=getZairaikuCoeff(p.idx,pp,floors);
    if(c===null){el.innerHTML='<span>損傷割合</span>全損';total=9999;}
    else{el.innerHTML=`<span>損傷割合（物理${pp}%）</span>${c} %`;total+=c;}
  }
  const rb=document.getElementById('z_result_box');
  const re=document.getElementById('res_building');
  const rv=document.getElementById('res_building_verdict');
  if(!hasData&&!hasErr){rb.className='verdict-box v-none';re.textContent='— %';rv.textContent='—';return;}
  if(hasErr){rb.className='verdict-box v-none';re.textContent='入力エラーあり';rv.textContent='確認してください';return;}
  const fp=total>=9999?100:ceil1(total);
  const v=getBuildingVerdict(fp,state.term);
  rb.className=`verdict-box ${v.cls}`;
  re.textContent=`${fp>=100?'100':fp} %`;
  rv.textContent=v.name;
}

function getWakuCoeff(partIdx,physPct){
  const T=[
    [[3,2],[5,16],[10,21],[15,28],[20,34],[25,39]],
    [[3,3],[5,5],[10,9],[15,16],[20,21],[25,25]],
    [[3,2],[5,3],[10,5],[15,6],[20,7],[25,8],[30,9],[35,10]],
    [[3,1],[5,2],[10,2],[15,3],[20,3],[25,4],[30,5],[40,6],[55,7]],
  ];
  for(const row of T[partIdx]){if(physPct<=row[0])return row[1];}
  return null;
}

function calcWaku(){
  const parts=[
    {dam:'w_gai_dam',tot:'w_gai_total',el:'w_gai_pct',idx:0},
    {dam:'w_nai_dam',tot:'w_nai_total',el:'w_nai_pct',idx:1},
    {dam:'w_kiso_dam',tot:'w_kiso_total',el:'w_kiso_pct',idx:2},
    {dam:'w_yane_dam',tot:'w_yane_total',el:'w_yane_pct',idx:3},
  ];
  let total=0,hasData=false,hasErr=false;
  for(const p of parts){
    const d=parseFloat(document.getElementById(p.dam).value);
    const t=parseFloat(document.getElementById(p.tot).value);
    const el=document.getElementById(p.el);
    if(isNaN(d)||isNaN(t)||t<=0){el.innerHTML='<span>損傷割合</span>— %';continue;}
    if(d>t){hasErr=true;el.innerHTML='<span>損傷割合</span>⚠️ 入力エラー';continue;}
    hasData=true;
    const pp=ceil1(d/t*100);
    const c=getWakuCoeff(p.idx,pp);
    if(c===null){el.innerHTML='<span>損傷割合</span>全損';total=9999;}
    else{el.innerHTML=`<span>損傷割合（物理${pp}%）</span>${c} %`;total+=c;}
  }
  const rb=document.getElementById('w_result_box');
  const re=document.getElementById('res_building_w');
  const rv=document.getElementById('res_building_verdict_w');
  if(!hasData&&!hasErr){rb.className='verdict-box v-none';re.textContent='— %';rv.textContent='—';return;}
  if(hasErr){rb.className='verdict-box v-none';re.textContent='入力エラーあり';rv.textContent='確認してください';return;}
  const fp=total>=9999?100:ceil1(total);
  const v=getBuildingVerdict(fp,state.term);
  rb.className=`verdict-box ${v.cls}`;
  re.textContent=`${fp>=100?'100':fp} %`;
  rv.textContent=v.name;
  document.getElementById('res_building').textContent=re.textContent;
  document.getElementById('res_building_verdict').textContent=rv.textContent;
}

const dosanCats=[
  {id:'food', name:'食器類（2品目）',    coeff:5},
  {id:'elec', name:'電気器具類（10品目）',coeff:20},
  {id:'furn', name:'家具類（10品目）',   coeff:10},
  {id:'misc', name:'身の回り品（10品目）',coeff:35},
  {id:'cloth',name:'衣類（10品目）',     coeff:30},
];

function buildDosanUI(){
  const cont=document.getElementById('dosan-sections');
  let html='';
  dosanCats.forEach(c=>{
    html+=`<div style="margin-bottom:12px;">
      <div class="calc-label">${c.name}（構成割合 ${c.coeff}%）</div>
      <div class="calc-hint">※整数のみ</div>
      <div style="display:grid;grid-template-columns:1fr auto 1fr auto;gap:6px;align-items:center;">
        <input type="number" id="ds_${c.id}_dam" inputmode="numeric" placeholder="損傷数" min="0" step="1"
          style="padding:10px;border:1.5px solid var(--border);border-radius:8px;font-size:15px;text-align:center;background:#f8fbff;"
          oninput="clampPositive(this);checkDosan('${c.id}');calcDosan()"
          onblur="formatOnBlur(this,0);checkDosan('${c.id}');calcDosan()">
        <span style="font-size:18px;color:var(--text-muted);text-align:center;">/</span>
        <input type="number" id="ds_${c.id}_tot" inputmode="numeric" placeholder="存在数" min="0" step="1"
          style="padding:10px;border:1.5px solid var(--border);border-radius:8px;font-size:15px;text-align:center;background:#f8fbff;"
          oninput="clampPositive(this);checkDosan('${c.id}');calcDosan()"
          onblur="formatOnBlur(this,0);checkDosan('${c.id}');calcDosan()">
        <span style="font-size:11px;color:var(--text-muted);">品目</span>
      </div>
      <div class="err-msg" id="err_ds_${c.id}">⚠️ 損傷数が存在数を超えています</div>
      <div class="pct-result" id="ds_${c.id}_pct" style="margin-top:4px;"><span>損傷割合</span>— %</div>
    </div>`;
  });
  cont.innerHTML=html;
}

function checkDosan(catId){
  const d=parseFloat(document.getElementById(`ds_${catId}_dam`).value);
  const t=parseFloat(document.getElementById(`ds_${catId}_tot`).value);
  const over=!isNaN(d)&&!isNaN(t)&&t>0&&d>t;
  document.getElementById(`err_ds_${catId}`).classList.toggle('show',over);
  const dEl=document.getElementById(`ds_${catId}_dam`);
  dEl.style.borderColor=over?'var(--danger)':'';
  dEl.style.background=over?'#fff5f5':'';
}

function calcDosan(){
  let total=0,hasData=false;
  for(const c of dosanCats){
    const d=parseFloat(document.getElementById(`ds_${c.id}_dam`).value);
    const t=parseFloat(document.getElementById(`ds_${c.id}_tot`).value);
    const el=document.getElementById(`ds_${c.id}_pct`);
    if(isNaN(d)||isNaN(t)||t<=0){el.innerHTML='<span>損傷割合</span>— %';continue;}
    if(d>t){el.innerHTML='<span>損傷割合</span>⚠️ エラー';continue;}
    hasData=true;
    const pp=ceil1(d/t*100);
    const contrib=ceil1(pp*c.coeff/100);
    el.innerHTML=`<span>損傷割合（物理${pp}% × 係数${c.coeff}%）</span>${contrib} %`;
    total+=contrib;
  }
  const box=document.getElementById('dosan_result_box');
  const pEl=document.getElementById('res_dosan');
  const vEl=document.getElementById('res_dosan_verdict');
  if(!hasData){box.className='verdict-box v-none';pEl.textContent='— %';vEl.textContent='—';return;}
  const fp=ceil1(total);
  const v=getDosanVerdict(fp,state.term);
  box.className=`verdict-box ${v.cls}`;
  pEl.textContent=`${fp} %`;
  vEl.textContent=v.name;
}

function getDosanVerdict(pct,term){
  if(term==='2017'){
    if(pct>=80)return{name:'全損',cls:'v-zenzon'};
    if(pct>=60)return{name:'大半損',cls:'v-ohanzong'};
    if(pct>=30)return{name:'小半損',cls:'v-kohanzong'};
    if(pct>=10)return{name:'一部損',cls:'v-ichibuzon'};
    return{name:'無責',cls:'v-muse'};
  }else{
    if(pct>=80)return{name:'全損',cls:'v-zenzon'};
    if(pct>=30)return{name:'半損',cls:'v-ohanzong'};
    if(pct>=10)return{name:'一部損',cls:'v-ichibuzon'};
    return{name:'無責',cls:'v-muse'};
  }
}

function calcTsunami(){
  const cm=parseFloat(document.getElementById('tsunami_cm').value);
  const box=document.getElementById('tsunami_result_box');
  const pEl=document.getElementById('res_tsunami');
  const vEl=document.getElementById('res_tsunami_verdict');
  const isHira=document.getElementById('floors').value==='1';
  const is2017=state.term==='2017';
  if(isNaN(cm)||cm<0){box.className='verdict-box v-none';pEl.textContent='入力してください';vEl.textContent='—';return;}
  let v;
  if(is2017){
    if(isHira){
      if(cm>=100)v={name:'全損',cls:'v-zenzon'};
      else if(cm>=75)v={name:'大半損',cls:'v-ohanzong'};
      else if(cm>0)v={name:'小半損/一部損',cls:'v-kohanzong'};
      else v={name:'無責',cls:'v-muse'};
    }else{
      if(cm>=180)v={name:'全損',cls:'v-zenzon'};
      else if(cm>=115)v={name:'大半損',cls:'v-ohanzong'};
      else if(cm>0)v={name:'小半損/一部損',cls:'v-kohanzong'};
      else v={name:'無責',cls:'v-muse'};
    }
  }else{
    if(cm>=225||(isHira&&cm>=145))v={name:'全損',cls:'v-zenzon'};
    else if(cm>45)v={name:'半損',cls:'v-ohanzong'};
    else if(cm>0)v={name:'一部損',cls:'v-ichibuzon'};
    else v={name:'無責',cls:'v-muse'};
  }
  box.className=`verdict-box ${v.cls}`;
  pEl.textContent=`${cm} cm`;
  vEl.textContent=v.name;
}

function calcEkijo(){
  const k=parseFloat(document.getElementById('eki_keisha').value);
  const c=parseFloat(document.getElementById('eki_chinka').value);
  const box=document.getElementById('eki_result_box');
  const pEl=document.getElementById('res_eki');
  const vEl=document.getElementById('res_eki_verdict');
  const is2017=state.term==='2017';
  function kV(v){
    if(is2017){
      if(v>1.7)return{name:'全損',cls:'v-zenzon',r:5};
      if(v>1.4)return{name:'大半損',cls:'v-ohanzong',r:4};
      if(v>0.9)return{name:'小半損',cls:'v-kohanzong',r:3};
      if(v>0.4)return{name:'一部損',cls:'v-ichibuzon',r:2};
      return{name:'無責',cls:'v-muse',r:1};
    }else{
      if(v>1.7)return{name:'全損',cls:'v-zenzon',r:5};
      if(v>0.9)return{name:'半損',cls:'v-ohanzong',r:4};
      if(v>0.4)return{name:'一部損',cls:'v-ichibuzon',r:2};
      return{name:'無責',cls:'v-muse',r:1};
    }
  }
  function cV(v){
    if(is2017){
      if(v>30)return{name:'全損',cls:'v-zenzon',r:5};
      if(v>20)return{name:'大半損',cls:'v-ohanzong',r:4};
      if(v>15)return{name:'小半損',cls:'v-kohanzong',r:3};
      if(v>10)return{name:'一部損',cls:'v-ichibuzon',r:2};
      return{name:'無責',cls:'v-muse',r:1};
    }else{
      if(v>30)return{name:'全損',cls:'v-zenzon',r:5};
      if(v>15)return{name:'半損',cls:'v-ohanzong',r:4};
      if(v>10)return{name:'一部損',cls:'v-ichibuzon',r:2};
      return{name:'無責',cls:'v-muse',r:1};
    }
  }
  let res=null,detail='';
  if(!isNaN(k)&&k>=0){const vk=kV(k);res=vk;detail=`傾斜${k}/100→${vk.name}`;}
  if(!isNaN(c)&&c>=0){
    const vc=cV(c);
    detail+=(detail?' / ':'')+`沈下${c}cm→${vc.name}`;
    if(!res||vc.r>res.r)res=vc;
  }
  if(!res){box.className='verdict-box v-none';pEl.textContent='入力してください';vEl.textContent='—';return;}
  box.className=`verdict-box ${res.cls}`;
  pEl.textContent=detail;
  vEl.textContent=res.name+'（高い方を採用）';
}

function updateSummary(){
  document.getElementById('sum_building').textContent=document.getElementById('res_building').textContent;
  document.getElementById('sum_building_v').textContent=document.getElementById('res_building_verdict').textContent;
  document.getElementById('sum_dosan').textContent=document.getElementById('res_dosan').textContent;
  document.getElementById('sum_dosan_v').textContent=document.getElementById('res_dosan_verdict').textContent;
  document.getElementById('sum_tsunami').textContent=document.getElementById('res_tsunami_verdict').textContent;
  document.getElementById('sum_eki').textContent=document.getElementById('res_eki_verdict').textContent;
}

function autoSave(){
  if(state.structure==='zairaiku')calcZairaiku();else calcWaku();
  calcDosan();
}

// 損傷割合表示から数値のみ取得（帳票用）
function getPctText(elId){
  const el=document.getElementById(elId);if(!el)return'';
  const t=el.textContent;
  if(t.includes('全損'))return'全損';
  const m=t.match(/(\d+\.?\d*)\s*%/g);
  return m?m[m.length-1]:'';
}

// ===== 帳票生成（A4縦2枚・大きめ文字） =====
function openPrintModal(){
  updateSummary();
  const V=id=>{const el=document.getElementById(id);return el?(el.value||''):''};
  const company=V('company'),insured=V('insured'),policy_no=V('policy_no');
  const address=V('address'),survey_date=V('survey_date'),surveyor=V('surveyor');
  const surveyor_org=V('surveyor_org'),quake_date=V('quake_date');
  const ins_amount=V('ins_amount'),ins_price=V('ins_price'),ins_amount_d=V('ins_amount_dosan');
  const floorsEl=document.getElementById('floors');
  const floorsText=floorsEl.options[floorsEl.selectedIndex].text;
  const structName=state.structure==='zairaiku'?'在来軸組工法等':'枠組壁工法';
  const is2017=state.term==='2017';
  const bPct=document.getElementById('res_building').textContent.replace(' %','').trim();
  const bV=document.getElementById('res_building_verdict').textContent.trim();
  const dPct=document.getElementById('res_dosan').textContent.replace(' %','').trim();
  const dV=document.getElementById('res_dosan_verdict').textContent.trim();

  function marks17(sel){
    return['全損','大半損','小半損','一部損','無責']
      .map(i=>`<span class="vmark${i===sel?' hit':''}">${i}</span>`).join('');
  }
  function marks16(sel){
    return['全損','半損','一部損','無責']
      .map(i=>`<span class="vmark${i===sel?' hit':''}">${i}</span>`).join('');
  }

  let damRows='';
  if(state.structure==='zairaiku'){
    const d={jD:V('z_jiku_dam'),jT:V('z_jiku_total'),kD:V('z_kiso_dam'),kT:V('z_kiso_total'),
             yD:V('z_yane_dam'),yT:V('z_yane_total'),gD:V('z_gaiheki_dam'),gT:V('z_gaiheki_total'),
             jP:getPctText('z_jiku_pct'),kP:getPctText('z_kiso_pct'),
             yP:getPctText('z_yane_pct'),gP:getPctText('z_gaiheki_pct')};
    damRows=`
<tr>
  <td class="lbl center" rowspan="4" style="width:20px;writing-mode:vertical-rl;letter-spacing:3px;">主要構造部</td>
  <td class="lbl center" style="width:24px;">軸<br>組</td>
  <td>損傷柱本数　<b>${d.jD||'　'}</b> 本　／　全柱本数　<b>${d.jT||'　'}</b> 本</td>
  <td class="center" style="width:62px;font-weight:bold;">${d.jP}</td>
</tr>
<tr>
  <td class="lbl center">基<br>礎</td>
  <td>損傷コンクリート長さ　<b>${d.kD||'　'}</b> m　／　外周布基礎長さ　<b>${d.kT||'　'}</b> m</td>
  <td class="center" style="font-weight:bold;">${d.kP}</td>
</tr>
<tr>
  <td class="lbl center">屋<br>根</td>
  <td>葺替え面積　<b>${d.yD||'　'}</b> ㎡　／　全屋根面積　<b>${d.yT||'　'}</b> ㎡</td>
  <td class="center" style="font-weight:bold;">${d.yP}</td>
</tr>
<tr>
  <td class="lbl center">外<br>壁</td>
  <td>損傷外壁面積　<b>${d.gD||'　'}</b> ㎡　／　全外壁面積　<b>${d.gT||'　'}</b> ㎡</td>
  <td class="center" style="font-weight:bold;">${d.gP}</td>
</tr>`;
  }else{
    const d={gD:V('w_gai_dam'),gT:V('w_gai_total'),nD:V('w_nai_dam'),nT:V('w_nai_total'),
             kD:V('w_kiso_dam'),kT:V('w_kiso_total'),yD:V('w_yane_dam'),yT:V('w_yane_total'),
             gP:getPctText('w_gai_pct'),nP:getPctText('w_nai_pct'),
             kP:getPctText('w_kiso_pct'),yP:getPctText('w_yane_pct')};
    damRows=`
<tr>
  <td class="lbl center" rowspan="4" style="width:20px;writing-mode:vertical-rl;letter-spacing:3px;">主要構造部</td>
  <td class="lbl center" style="width:24px;">外<br>壁</td>
  <td>1階外壁損傷長さ　<b>${d.gD||'　'}</b> m　／　1階外周延べ長さ　<b>${d.gT||'　'}</b> m</td>
  <td class="center" style="width:62px;font-weight:bold;">${d.gP}</td>
</tr>
<tr>
  <td class="lbl center">内<br>壁</td>
  <td>入隅損傷数×0.5　<b>${d.nD||'　'}</b> 箇所　／　1階入隅全箇所数　<b>${d.nT||'　'}</b> 箇所</td>
  <td class="center" style="font-weight:bold;">${d.nP}</td>
</tr>
<tr>
  <td class="lbl center">基<br>礎</td>
  <td>損傷コンクリート長さ　<b>${d.kD||'　'}</b> m　／　外周布基礎長さ　<b>${d.kT||'　'}</b> m</td>
  <td class="center" style="font-weight:bold;">${d.kP}</td>
</tr>
<tr>
  <td class="lbl center">屋<br>根</td>
  <td>葺替え面積　<b>${d.yD||'　'}</b> ㎡　／　全屋根面積　<b>${d.yT||'　'}</b> ㎡</td>
  <td class="center" style="font-weight:bold;">${d.yP}</td>
</tr>`;
  }

  let dosanRows='';
  dosanCats.forEach(c=>{
    const dv=V(`ds_${c.id}_dam`),tv=V(`ds_${c.id}_tot`);
    const pv=getPctText(`ds_${c.id}_pct`);
    dosanRows+=`<tr>
      <td>${c.name}</td>
      <td class="center">${dv||'—'}</td>
      <td class="center">${tv||'—'}</td>
      <td class="center">${c.coeff}%</td>
      <td class="center" style="font-weight:bold;">${pv}</td>
    </tr>`;
  });

  const tsunamiCm=V('tsunami_cm');
  const tsunamiVerdt=document.getElementById('res_tsunami_verdict').textContent.trim();
  const ekiDetail=document.getElementById('res_eki').textContent.trim();
  const ekiVerdt=document.getElementById('res_eki_verdict').textContent.trim();
  const hasSpecial=tsunamiCm||(ekiVerdt!=='—'&&ekiVerdt!=='入力してください');

  // ========== 1ページ目：基本情報・認定結果 ==========
  const page1=`
<div class="page-block page-break">

  <div style="display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:4px;">
    <div style="border:1px solid #888;font-size:8.5pt;padding:2px 8px;">会 社 整 理 番 号</div>
    <div style="text-align:right;font-size:8.5pt;line-height:1.8;">
      2019年1月1日以降発生地震に使用<br>
      <span style="border:2px solid #000;padding:2px 14px;font-size:11pt;font-weight:bold;display:inline-block;margin-top:2px;">木　造　建　物</span>
    </div>
  </div>
  <div style="text-align:right;font-size:8pt;margin-bottom:3px;">ボールペン等で記入（鉛筆類は使用しないこと）</div>
  <div class="page-title">地 震 保 険 損 害 調 査 書（報 告 書）</div>

  <!-- 基本情報 -->
  <table class="ht" style="margin-bottom:6px;">
    <tr>
      <td class="lbl center" style="width:70px;">会　社　名</td>
      <td colspan="3" style="font-size:12pt;font-weight:bold;">${company}</td>
      <td class="lbl center sm" style="width:55px;">調 査 日</td>
      <td style="font-size:10pt;">${survey_date}</td>
    </tr>
    <tr>
      <td class="lbl center xsm" rowspan="2" style="line-height:1.6;">契約者または<br>被 保 険 者</td>
      <td colspan="3">（建　物）${insured}</td>
      <td class="lbl center sm">調査担当者</td>
      <td></td>
    </tr>
    <tr>
      <td colspan="3">（生活動産）${insured}</td>
      <td class="lbl center xsm" style="line-height:1.6;">所　属<br>（調査会社名）</td>
      <td>${surveyor_org}</td>
    </tr>
    <tr>
      <td class="lbl center xsm">氏　　名</td>
      <td colspan="5" style="font-size:11pt;font-weight:bold;">${surveyor}　　　印</td>
    </tr>
    <tr>
      <td class="lbl center">目的所在地</td>
      <td colspan="5">${address}</td>
    </tr>
    <tr>
      <td class="lbl center sm">地物構造</td>
      <td colspan="3">木造（${structName}）　${floorsText}</td>
      <td class="lbl center xsm">延床面積</td>
      <td>　　　㎡</td>
    </tr>
    <tr>
      <td class="lbl center">罹　災　日</td>
      <td colspan="2">${quake_date}</td>
      <td class="lbl center sm">損害形態</td>
      <td colspan="2" class="xsm" style="line-height:1.6;">火災・崩壊（津波浸水・地盤液状化）・埋没・流失・床上浸水・地盤面より45cm超の浸水</td>
    </tr>
  </table>

  <!-- 保険金額・認定結果 -->
  <table class="ht" style="margin-bottom:5px;">
    <tr>
      <td style="width:60px;"></td>
      <th class="center lbl2" colspan="2">建　　　物</th>
      <th class="center lbl2" colspan="2">生 活 用 動 産（家 財）</th>
    </tr>
    <tr>
      <td class="lbl center sm">地震保険金額</td>
      <td colspan="2" style="font-size:11pt;font-weight:bold;">${ins_amount ? ins_amount+' 千円' : '　　　　千円'}</td>
      <td colspan="2" style="font-size:11pt;font-weight:bold;">${ins_amount_d ? ins_amount_d+' 千円' : '　　　　千円'}</td>
    </tr>
    <tr>
      <td class="lbl center sm">保　険価　額</td>
      <td colspan="2" class="sm">（支払保険金を保険金額に限る場合のみ記入）${ins_price ? ins_price+' 千円' : ''}</td>
      <td colspan="2" class="sm">（支払保険金を保険金額に限る場合のみ記入）</td>
    </tr>
    <tr>
      <td class="lbl center sm" rowspan="2">認　定<br>結　果</td>
      <td class="lbl2 center sm" style="width:68px;">2017年1月1日以降<br>始期契約（注1）</td>
      <td style="padding:5px 6px;">
        <div class="vmark-row">${is2017?marks17(bV):marks17('')}</div>
        <div class="xsm" style="margin-top:3px;">(100%)全損 ・ (60%)大半損 ・ (30%)小半損 ・ (5%)一部損</div>
      </td>
      <td class="lbl2 center sm" style="width:68px;">2017年1月1日以降<br>始期契約（注1）</td>
      <td style="padding:5px 6px;">
        <div class="vmark-row">${is2017?marks17(dV):marks17('')}</div>
        <div class="xsm" style="margin-top:3px;">(100%)全損 ・ (60%)大半損 ・ (30%)小半損 ・ (5%)一部損</div>
      </td>
    </tr>
    <tr>
      <td class="lbl2 center sm">2016年12月31日以前<br>始期契約（注1）</td>
      <td style="padding:5px 6px;">
        <div class="vmark-row">${!is2017?marks16(bV):marks16('')}</div>
        <div class="xsm" style="margin-top:3px;">(100%)全損 ・ (50%)半損 ・ (5%)一部損</div>
      </td>
      <td class="lbl2 center sm">2016年12月31日以前<br>始期契約（注1）</td>
      <td style="padding:5px 6px;">
        <div class="vmark-row">${!is2017?marks16(dV):marks16('')}</div>
        <div class="xsm" style="margin-top:3px;">(100%)全損 ・ (50%)半損 ・ (5%)一部損</div>
      </td>
    </tr>
    <tr>
      <td class="lbl center sm">支払保険金（注2）</td>
      <td colspan="2" style="height:24px;">　　　　　千円</td>
      <td colspan="2">　　　　　千円</td>
    </tr>
  </table>

  <div class="chuki">
    （注1）地震保険の始期日<br>
    （注2）地震保険金額には限度額が設定されており（同一敷地内に所在し、かつ、同一被保険者の所有に属する建物は50,000千円、家財は10,000千円）、この額を超える保険金は支払えない。重複契約がある場合、自社支払合計金額を記入する。
  </div>

  <!-- 支払保険金明細欄 -->
  <div class="page-subtitle">【支払保険金明細欄】</div>
  <div class="chuki">「同一敷地内に所在し、かつ、同一被保険者の所有に属する建物」「同一敷地内に所在し、かつ、同一被保険者の世帯に属する生活用動産（家財）」の有無を確認のうえ、以下の支払保険金明細を記載する。（重複契約の有無にかかわらず記載する。）</div>
  <table class="ht" style="margin-bottom:6px;">
    <tr>
      <th class="lbl2 center" colspan="4">建　　　物</th>
      <th class="lbl2 center" colspan="4">生活用動産（家財）</th>
    </tr>
    <tr>
      <th class="lbl2 center sm">会社名</th><th class="lbl2 center sm">証 券 番 号</th>
      <th class="lbl2 center sm">地震保険金額（千円）</th><th class="lbl2 center sm">支払保険金（千円）</th>
      <th class="lbl2 center sm">会社名</th><th class="lbl2 center sm">証 券 番 号</th>
      <th class="lbl2 center sm">地震保険金額（千円）</th><th class="lbl2 center sm">支払保険金（千円）</th>
    </tr>
    <tr style="height:22px;"><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
    <tr style="height:22px;"><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
    <tr style="height:22px;"><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
    <tr>
      <td class="lbl2 center sm" colspan="2">合　　計</td><td></td><td></td>
      <td class="lbl2 center sm" colspan="2">合　　計</td><td></td><td></td>
    </tr>
    <tr>
      <td class="lbl2 center sm" colspan="2">自社合計</td>
      <td class="center sm">（　　　）</td><td class="center sm">（　　　）</td>
      <td class="lbl2 center sm" colspan="2">自社合計</td>
      <td class="center sm">（　　　）</td><td class="center sm">（　　　）</td>
    </tr>
  </table>

  <!-- 特記事項欄 -->
  <div class="page-subtitle">〈 特 記 事 項 欄 〉</div>
  <div class="toki-box" style="min-height:55px;"></div>

  <!-- 協定日・閲読者 -->
  <div style="display:flex;justify-content:flex-end;margin-bottom:8px;">
    <table style="border-collapse:collapse;font-size:10pt;">
      <tr><td style="padding:3px 8px;border-bottom:1px solid #888;">協定日　　　年　　月　　日</td></tr>
      <tr><td style="padding:3px 8px;">閲読者　　　　　　　　　　</td></tr>
    </table>
  </div>

  <div class="footer-notes-list">
    1．本調査書には、下記確認資料を付する。<br>
    　（1）契約確認資料　　（2）写真<br>
    2．「全損認定地域」所在物件で調査省略の場合は、契約確認資料にその旨を明記し、本調査書の作成は不要とする。ただし、重複契約がある場合には、契約確認資料等に重複契約の有無ごとの支払保険金明細欄を明記する。<br>
    3．共同保険は幹事会社が一部作成する。（分担会社の記入は不要）
  </div>
</div>`;

  // ========== 2ページ目：損害計算明細 ==========
  const page2=`
<div class="page-block">

  <div class="page-title" style="margin-bottom:8px;">損 害 計 算 明 細　（${structName}）</div>
  <div style="font-size:9pt;text-align:right;margin-bottom:6px;">
    会社名：${company}　　証券番号：${policy_no}　　調査日：${survey_date}
  </div>

  <!-- 建物損害 -->
  <div class="page-subtitle">（１）建物の損害　【${structName}】</div>
  <table class="ht" style="margin-bottom:4px;">
    <tr>
      <th class="lbl2 center" colspan="2">主要構造部位</th>
      <th class="lbl2 center">測定内容（損傷　／　全体）</th>
      <th class="lbl2 center" style="width:62px;">物理的<br>損傷割合</th>
    </tr>
    ${damRows}
    <tr>
      <td class="lbl2" colspan="2" style="text-align:left;padding:4px 6px;">合計損害割合（建物）　　　　　　　　　　　　　　　　（小数点以下第1位未満切り上げ）</td>
      <td></td>
      <td class="center" style="font-size:13pt;font-weight:bold;color:#c0392b;">${bPct ? bPct+' %' : '—'}</td>
    </tr>
  </table>
  <div style="background:#f0f4f8;border:2px solid #1a3a5c;border-radius:6px;padding:8px 14px;margin-bottom:10px;display:flex;gap:40px;align-items:center;">
    <div style="font-size:10pt;">建物　損害割合合計：<span style="font-size:16pt;font-weight:bold;color:#c0392b;">${bPct ? bPct+' %' : '—'}</span></div>
    <div style="font-size:10pt;">認定区分：<span style="font-size:18pt;font-weight:bold;color:#c0392b;">${bV||'—'}</span></div>
  </div>

  <!-- 生活用動産 -->
  <div class="page-subtitle">（２）生活用動産の損害</div>
  <table class="ht" style="margin-bottom:4px;">
    <tr>
      <th class="lbl2 center" style="width:46%;">分　　　類</th>
      <th class="lbl2 center">損傷数</th>
      <th class="lbl2 center">存在数</th>
      <th class="lbl2 center">構成割合</th>
      <th class="lbl2 center" style="width:60px;">損傷割合</th>
    </tr>
    ${dosanRows}
    <tr>
      <td class="lbl2" colspan="4" style="text-align:left;padding:4px 6px;">損害割合合計（動産）　　　　　（小数点以下第1位未満切り上げ）</td>
      <td class="center" style="font-size:13pt;font-weight:bold;color:#c0392b;">${dPct ? dPct+' %' : '—'}</td>
    </tr>
  </table>
  <div style="background:#f0f4f8;border:2px solid #1a3a5c;border-radius:6px;padding:8px 14px;margin-bottom:10px;display:flex;gap:40px;align-items:center;">
    <div style="font-size:10pt;">動産　損害割合合計：<span style="font-size:16pt;font-weight:bold;color:#c0392b;">${dPct ? dPct+' %' : '—'}</span></div>
    <div style="font-size:10pt;">認定区分：<span style="font-size:18pt;font-weight:bold;color:#c0392b;">${dV||'—'}</span></div>
  </div>

  ${hasSpecial ? `
  <!-- 特殊事由 -->
  <div class="page-subtitle">（３）特殊事由</div>
  <table class="ht" style="margin-bottom:10px;">
    ${tsunamiCm ? `
    <tr>
      <td class="lbl center" style="width:80px;">津　　波</td>
      <td>浸水高さ　<b style="font-size:12pt;">${tsunamiCm} cm</b></td>
      <td class="lbl center" style="width:60px;">認　定</td>
      <td style="font-size:13pt;font-weight:bold;color:#c0392b;">${tsunamiVerdt}</td>
    </tr>` : ''}
    ${(ekiVerdt!=='—'&&ekiVerdt!=='入力してください') ? `
    <tr>
      <td class="lbl center">液 状 化</td>
      <td>${ekiDetail}</td>
      <td class="lbl center">認　定</td>
      <td style="font-size:13pt;font-weight:bold;color:#c0392b;">${ekiVerdt}</td>
    </tr>` : ''}
  </table>` : ''}

  <!-- 備考欄 -->
  <div class="page-subtitle">備　考　欄</div>
  <div class="toki-box" style="min-height:60px;"></div>

  <div style="font-size:8.5pt;text-align:right;color:#666;margin-top:8px;border-top:1px solid #ccc;padding-top:6px;">
    ※本ツールによる自動計算結果です。最終判断は担当者による原本確認のうえ行ってください。<br>
    ※端数処理：損害割合は小数点以下第1位未満切り上げで計算しています。
  </div>
</div>`;

  const html = page1 + page2;

  document.getElementById('print-area').innerHTML=html;
  document.getElementById('printModal').classList.add('open');
}

function closePrintModal(){
  document.getElementById('printModal').classList.remove('open');
}

document.addEventListener('DOMContentLoaded',function(){
  setStructure('zairaiku');
  setTerm('2017');
  document.getElementById('survey_date').value=new Date().toISOString().split('T')[0];
  buildDosanUI();
});
</script>
</body>
</html>
