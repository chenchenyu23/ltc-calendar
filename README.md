[index.html.html](https://github.com/user-attachments/files/30569800/index.html.html)
# ltc-calendar<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>LTC Calendar Pro｜長照服務月曆</title>
<style>
  :root{
    --bg:#f3f0eb;--panel:#faf8f4;--text:#494844;--muted:#817d76;--line:#d8d1c7;
    --primary:#7e8f82;--primary-soft:#e4e9e3;--ok:#6f8575;--warn:#a38b68;--danger:#9b716d;
    --rose:#b7928d;--blue:#8797a5;--taupe:#9b8e82;--sage:#879888;
    --shadow:0 8px 24px rgba(80,72,62,.08);--radius:16px;
  }
  *{box-sizing:border-box}
  body{margin:0;background:var(--bg);color:var(--text);font-family:-apple-system,BlinkMacSystemFont,"Segoe UI","Noto Sans TC",Arial,sans-serif}
  .wrap{max-width:1440px;margin:0 auto;padding:24px}
  h1{margin:0;font-size:28px}
  h2{font-size:19px;margin:0 0 14px}
  .sub{color:var(--muted);margin-top:6px}
  .panel{background:var(--panel);border:1px solid var(--line);border-radius:var(--radius);box-shadow:var(--shadow);padding:20px;margin-top:18px}
  textarea{width:100%;min-height:320px;border:1px solid var(--line);border-radius:12px;padding:14px;font:14px/1.55 ui-monospace,SFMono-Regular,Menlo,Consolas,monospace;resize:vertical}
  .toolbar{display:flex;justify-content:flex-end;gap:10px;margin-top:18px}
  .modal{position:fixed;inset:0;z-index:1000;display:none;align-items:center;justify-content:center;padding:20px;background:rgba(45,43,40,.48);backdrop-filter:blur(3px)}
  .modal.open{display:flex}
  .modal-card{width:min(900px,100%);max-height:92vh;overflow:auto;background:var(--panel);border:1px solid var(--line);border-radius:18px;box-shadow:0 24px 70px rgba(40,36,31,.24);padding:20px}
  .modal-head{display:flex;justify-content:space-between;align-items:center;gap:12px;margin-bottom:14px}
  .modal-title{font-size:20px;font-weight:800}
  .icon-btn{width:38px;height:38px;justify-content:center;padding:0;border-radius:50%;background:#e9e4dc;color:var(--text);font-size:20px}
  body.modal-open{overflow:hidden}
  button,.file-label{border:0;border-radius:10px;padding:10px 15px;font-weight:700;cursor:pointer;display:inline-flex;align-items:center;gap:6px}
  .primary{background:var(--primary);color:#fff}.secondary{background:#e9e4dc;color:var(--text)}.ghost{background:#faf8f4;color:var(--primary);border:1px solid #b7c0b8}.danger{background:#eee2df;color:var(--danger)}
  .actions{display:flex;flex-wrap:wrap;gap:10px;margin-top:12px}
  .hint{font-size:13px;color:var(--muted);margin-top:10px}
  .grid{display:grid;grid-template-columns:repeat(6,minmax(0,1fr));gap:14px}
  .card{border:1px solid var(--line);border-radius:14px;padding:16px;background:var(--panel)}
  .card .label{font-size:13px;color:var(--muted)}.card .value{font-size:25px;font-weight:800;margin-top:7px}.card .small{font-size:12px;color:var(--muted);margin-top:5px}
  .filters{display:grid;grid-template-columns:minmax(180px,320px);gap:10px}
  select,input{width:100%;border:1px solid var(--line);border-radius:10px;padding:9px 10px;background:#fff;color:var(--text)}
  .table-wrap{overflow:auto;border:1px solid var(--line);border-radius:12px}
  table{border-collapse:collapse;width:100%;min-width:1050px;background:var(--panel)}
  th,td{border-bottom:1px solid var(--line);padding:10px 11px;text-align:left;vertical-align:top;font-size:13px}
  th{position:sticky;top:0;background:#eeeae3;z-index:1;white-space:nowrap}
  tr:hover td{background:#f7f4ef}
  .num{text-align:right;white-space:nowrap}
  .pill{display:inline-block;padding:3px 8px;border-radius:999px;background:#e9e4dc;font-size:12px;white-space:nowrap}
  .ok{background:#e2e9e3;color:var(--ok)}.warn{background:#eee7dc;color:var(--warn)}
  .price-table{min-width:650px}.price-table input{max-width:140px}
  .unpriced{color:var(--danger);font-weight:700}
  .tabs{display:flex;gap:8px;margin-bottom:12px}.tab{padding:8px 12px;border-radius:10px;background:#e9e4dc;cursor:pointer;font-weight:700}.tab.active{background:var(--primary-soft);color:var(--primary)}
  .empty{padding:28px;text-align:center;color:var(--muted)}
  .footer{font-size:12px;color:var(--muted);text-align:center;padding:20px 0}
  .mini-btn{padding:6px 9px;font-size:12px}
  .calendar-head{display:flex;justify-content:space-between;align-items:center;gap:12px;margin-bottom:12px;flex-wrap:wrap}
  .calendar-title{font-size:20px;font-weight:800}
  .calendar-grid{display:grid;grid-template-columns:repeat(7,minmax(0,1fr));border:1px solid var(--line);border-radius:14px;overflow:hidden;background:var(--panel)}
  .weekday{padding:9px;text-align:center;font-size:13px;font-weight:800;background:#eeeae3;border-right:1px solid var(--line)}
  .weekday:nth-child(7){border-right:0}
  .weekday.weekend{background:#eee3e1;color:#8e6965}
  .day{min-height:150px;padding:8px;border-top:1px solid var(--line);border-right:1px solid var(--line);background:var(--panel);overflow:hidden;cursor:pointer;transition:background .15s ease}
  .day:hover{background:#f5f1eb}.day.weekend{background:#fbf3f1}.day.weekend:hover{background:#f7ebe8}
  .day:nth-child(7n){border-right:0}.day.other{background:#eeeae3;color:#98a2b3;cursor:default}.day.other.weekend{background:#ece5e3}.day.today{box-shadow:none}
  .day-num{font-weight:800;margin-bottom:6px}.events{display:flex;flex-direction:column;gap:5px}
  .event{border:1px solid var(--line);border-left:4px solid var(--primary);border-radius:8px;padding:6px 7px;background:#f7f4ef;font-size:12px;line-height:1.35}
  .event.cat-BA{border-left-color:#7e8f82;background:#eef2ed}.event.cat-BB{border-left-color:#8797a5;background:#eef1f4}
  .event.cat-C{border-left-color:#a58d72;background:#f3eee7}.event.cat-DA{border-left-color:#b7928d;background:#f4eceb}
  .event.cat-GA-SC{border-left-color:#8f829d;background:#f1edf4}
  .event.cat-OT{border-left-color:#9b8e82;background:#f1efec}
  .event-time{font-weight:800;color:var(--text);font-size:12px}.event-duration{display:block;color:var(--muted);font-weight:700;margin-top:2px}.event-agency{display:block;font-weight:800;color:var(--text);margin-top:3px}.event-code{display:block;font-weight:800;margin-top:2px;color:var(--text);line-height:1.45}
  .event-meta{color:var(--muted);margin-top:2px;font-size:11px}.event-amount{white-space:nowrap;font-weight:700;color:var(--muted)}
  .alias-input{min-width:150px}.agency-full{max-width:700px;word-break:break-all}.legend{display:flex;flex-wrap:wrap;gap:8px;margin-top:10px}.legend .pill{border:1px solid var(--line);background:var(--panel)}

  .summary-bottom{margin-top:18px}.category-summary{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:12px;margin-bottom:16px}
  .care-summary{grid-column:1/-1;border:1px solid #bfc8c0;border-radius:16px;background:#edf1ec;overflow:hidden}
  .care-summary-head{display:flex;justify-content:space-between;align-items:center;gap:16px;padding:16px 18px;border-bottom:1px solid #cdd5ce}
  .care-summary-title{font-size:17px;font-weight:900}.care-summary-total{text-align:right}.care-summary-total span{display:block;font-size:12px;color:var(--muted);font-weight:700}.care-summary-total strong{font-size:28px;line-height:1.1}
  .care-subgrid{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:10px;padding:12px}
  .category-card{border:1px solid var(--line);border-radius:14px;background:#f7f4ef;overflow:hidden;cursor:pointer;transition:.15s ease}.category-card:hover{transform:translateY(-1px);box-shadow:0 5px 14px rgba(80,74,66,.10)}.category-card.active{outline:3px solid rgba(104,122,107,.28);border-color:var(--primary)}
  .care-summary .category-card{background:#fafbf9}
  .category-head{display:flex;justify-content:space-between;align-items:center;padding:11px 12px;background:#eeeae3;border-bottom:1px solid var(--line)}
  .category-name{font-weight:800}.category-total{font-size:18px;font-weight:800}
  .category-body{padding:8px 10px}.category-agency{padding:8px 0;border-bottom:1px dashed var(--line)}.category-agency:last-child{border-bottom:0}
  .category-agency-top{display:flex;justify-content:space-between;gap:8px;font-size:13px;font-weight:800}.category-items{font-size:12px;color:var(--muted);margin-top:4px;line-height:1.45}
  .category-empty{padding:14px 10px;color:var(--muted);font-size:12px;text-align:center}
  @media(max-width:760px){.category-summary,.care-subgrid{grid-template-columns:1fr}.care-summary-head{align-items:flex-start}.care-summary-total strong{font-size:24px}}
  @media(max-width:900px){.calendar-grid{grid-template-columns:repeat(7,minmax(110px,1fr));min-width:770px}.calendar-scroll{overflow:auto}.day{min-height:135px}}
  @media(max-width:1000px){.grid{grid-template-columns:repeat(2,1fr)}.filters{grid-template-columns:repeat(2,1fr)}}
  @media(max-width:620px){.wrap{padding:12px}.grid,.filters{grid-template-columns:1fr}h1{font-size:23px}.panel{padding:15px}}
  @media print{body{background:var(--panel)}.no-print,.actions,.filters,.tabs{display:none!important}.panel{box-shadow:none;border:0;padding:0;margin-top:10px}.wrap{max-width:none;padding:0}table{min-width:0}th,td{font-size:10px;padding:5px}.footer{display:none}}
</style>
</head>
<body>
<div class="wrap">
  <header>
    <h1>LTC Calendar Pro｜長照服務月曆</h1>
    <div class="sub">貼上照管系統服務紀錄，自動整理服務單位、服務項目、申報狀態與預估申報金額。</div>
  </header>

  <div class="toolbar no-print">
    <button class="primary" id="openInputBtn">貼上服務紀錄</button>
  </div>

  <div class="modal no-print" id="inputModal" role="dialog" aria-modal="true" aria-labelledby="inputModalTitle">
    <div class="modal-card">
      <div class="modal-head">
        <div class="modal-title" id="inputModalTitle">貼上服務紀錄</div>
        <button class="icon-btn" id="closeInputBtn" aria-label="關閉">×</button>
      </div>
      <textarea id="rawInput" placeholder="請直接貼上包含欄位標題與資料列的內容，例如：項次、服務日期、服務項目、類型、服務數量……"></textarea>
      <div class="actions">
        <button class="primary" id="parseBtn">產生月曆</button>
        <button class="danger" id="modalClearBtn">一鍵清除</button>
        <button class="secondary" id="cancelInputBtn">取消</button>
      </div>
      <div class="hint">資料只在目前瀏覽器中處理，不會上傳。</div>
    </div>
  </div>

  <div class="modal" id="dayModal" role="dialog" aria-modal="true" aria-labelledby="dayModalTitle">
    <div class="modal-card" style="width:min(720px,100%)">
      <div class="modal-head">
        <div class="modal-title" id="dayModalTitle">當日服務紀錄</div>
        <button class="icon-btn" id="closeDayBtn" aria-label="關閉">×</button>
      </div>
      <div id="dayModalBody"></div>
    </div>
  </div>

  <section class="panel no-print">
    <h2>篩選年月</h2>
    <div class="filters">
      <select id="monthFilter"><option value="">全部月份</option></select>
    </div>
  </section>

  <section class="panel">
    <div class="calendar-head">
      <div>
        <div class="calendar-title" id="calendarTitle">請先貼上資料</div>
        <div class="hint" style="margin-top:4px">同一服務人員於同一單位、前後時間連續的服務會合併顯示；不同人員則分開。</div>
      </div>
      <div class="actions no-print" style="margin:0">
        <button class="secondary mini-btn" id="prevMonthBtn">上一月</button>
        <button class="secondary mini-btn" id="nextMonthBtn">下一月</button>
        <button class="ghost mini-btn" id="printCalendarBtn">列印月曆</button>
      </div>
    </div>
    <div class="calendar-scroll"><div class="calendar-grid" id="calendarGrid"></div></div>

    <div class="summary-bottom">
      <div class="category-summary" id="categorySummary"></div>
    </div>
  </section>

  <div class="footer">本工具僅供內部資料整理與預估，正式申報金額以主管機關核定、支付標準及核銷系統結果為準。</div>
</div>
<script>
const defaultPrices={
  // BA 居家照顧服務（一般地區支付價格）
  BA01:260,BA02:195,BA03:35,BA04:130,BA05:310,'BA05-1':310,'BA05-2':310,BA07:325,BA08:500,
  BA09:2200,BA09a:2500,BA10:155,BA11:195,BA12:130,BA13:195,BA14:685,
  BA15:195,'BA15-1':195,'BA15-2':195,BA16:130,'BA16-1':130,'BA16-2':130,BA17A:75,BA17B:65,BA17C:50,
  BA17D1:50,BA17D2:50,BA17E:50,BA18:200,BA20:175,BA22:130,BA23:200,BA24:220,

  // BB 日間照顧（一般地區支付價格）
  BB01:675,BB02:340,BB03:840,BB04:420,BB05:920,BB06:460,BB07:1045,
  BB08:525,BB09:1130,BB10:565,BB11:1210,BB12:605,BB13:1285,BB14:645,

  // BC 家庭托顧（一般地區支付價格）
  BC01:625,BC02:315,BC03:760,BC04:380,BC05:785,BC06:395,BC07:880,
  BC08:440,BC09:960,BC10:480,BC11:980,BC12:490,BC13:1040,BC14:520,

  // BD 社區式附加服務（一般地區支付價格）
  BD01:200,BD02:150,BD03:100,

  // C 專業服務（一般地區支付價格）
  CA07:4500,CA08:6000,
  CB01:4000,CB02:9000,CB03:4500,CB04:9000,
  CC01:2000,CD02:6000
};
let activeCategory='';
const storedPrices=JSON.parse(localStorage.getItem('ltc_price_map')||'null')||{};
const savedPrices=Object.fromEntries(Object.entries(storedPrices).filter(([,v])=>v!==''&&v!==null&&Number.isFinite(Number(v))).map(([k,v])=>[k,Number(v)]));
let priceMap={...defaultPrices,...savedPrices};
let aliasMap={};
let rows=[];
const sample=`項次\t服務日期\t服務項目\t類型\t服務數量\t服務區間起訖\t長照機構\t服務人員\t狀態\tAA10申請狀態\t系統識別碼
1\t115/07/21\tDA01[交通接送]\t補助\t1\t14:33~14:42[9分]\t(臺北市政府交通接送)接受派案帳號\tU12*****85\t未核銷\t-\t9031500454
2\t115/06/30\tBB09[日間照顧（全日）--第5型]\t補助\t1\t11:55~17:50[355分]\t臺北市政府社會局委託財團法人恆安社會福利慈善事業基金會經營管理臺北市文山社區式長期照顧服務機構\tF12*****48、A13*****76\t已於核銷系統確認申報\t未申請確認\t8989064786
3\t115/06/29\tBA20[陪伴服務](10712)\t補助\t1\t16:05~16:35[30分]\t財團法人恆安社會福利慈善事業基金會附設臺北市私立恆安居家式服務類長期照顧服務機構\tA22*****31\t已於核銷系統確認申報\t未申請確認\t8990907393
4\t115/06/29\tBA07[協助沐浴及洗頭]\t補助\t1\t15:35~16:05[30分]\t財團法人恆安社會福利慈善事業基金會附設臺北市私立恆安居家式服務類長期照顧服務機構\tA22*****31\t已於核銷系統確認申報\t未申請確認\t8990907388`;

function escapeHtml(v=''){return String(v).replace(/[&<>'"]/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;',"'":'&#39;','"':'&quot;'}[c]));}
function money(v){return new Intl.NumberFormat('zh-TW',{style:'currency',currency:'TWD',maximumFractionDigits:0}).format(Number(v)||0);}
function parseService(raw){const m=String(raw).trim().match(/^([A-Z]{2}\d+(?:-[A-Z0-9]+|[A-Z]\d*)?)\[([^\]]+)\]/i);return m?{code:m[1].toUpperCase(),name:m[2]}:{code:String(raw).trim().toUpperCase(),name:''};}
function rocMonth(date){const m=String(date).match(/^(\d{2,3})\/(\d{1,2})\/\d{1,2}$/);return m?`${m[1]}/${String(m[2]).padStart(2,'0')}`:'';}
function rocToDate(date){const m=String(date).match(/^(\d{2,3})\/(\d{1,2})\/(\d{1,2})$/);return m?new Date(Number(m[1])+1911,Number(m[2])-1,Number(m[3])):null;}
function autoAlias(name=''){
  const normalized=String(name).replace(/臺/g,'台').replace(/\s+/g,'');
  // DA01 系統帳號不是實際服務單位，不以「交通」作為單位名稱。
  if(normalized.includes('交通接送')&&normalized.includes('接受派案帳號')) return '';

  // OT01 目前已知服務單位。
  if(normalized.includes('紅心')) return '紅心';
  if(normalized.includes('恆安')) return '恆安';

  // 居家：只抓「居家」前方最近的實際單位名稱。
  const homePatterns=[
    /(?:私立|附設)([一-鿿]{2,12}?)(?=居家式服務類|居家長照機構|居家服務|居家)/,
    /([一-鿿]{2,12}?)(?=居家式服務類|居家長照機構|居家服務|居家)/
  ];
  for(const re of homePatterns){
    const m=normalized.match(re);
    if(m){
      const label=m[1].replace(/^(台北市|新北市|桃園市|基隆市)/,'').replace(/^私立/,'');
      return label.slice(-8);
    }
  }

  // 日照／BB 類：抓「日照、日間照顧、社區長照機構、社區式長照機構」前方最近的單位名稱，
  // 排除基金會、慈善事業等法人名稱，只保留實際日照單位名稱。
  const dayPatterns=[
    /(?:私立|附設)([一-鿿]{2,12}?)(?=日照中心|日間照顧中心|日照|日間照顧|社區長照機構|社區式長期照顧服務機構)/,
    /(?:台北市|新北市|桃園市|基隆市)([一-鿿]{2,12}?)(?=日照中心|日間照顧中心|日照|日間照顧|社區長照機構|社區式長期照顧服務機構)/,
    /([一-鿿]{2,12}?)(?=日照中心|日間照顧中心|日照|日間照顧|社區長照機構|社區式長期照顧服務機構)/
  ];
  for(const re of dayPatterns){
    const m=normalized.match(re);
    if(m){
      let label=m[1]
        .replace(/^(台北市|新北市|桃園市|基隆市)/,'')
        .replace(/財團法人|社團法人|基金會|慈善事業|社會福利|經營管理|委託/g,'');
      label=label.slice(-8);
      return label.endsWith('日照')?label:label+'日照';
    }
  }

  // 其他類型採保守簡稱，避免抓到「機構」、「慈善事業」等泛稱。
  let core=normalized
    .replace(/[（(][^）)]*[）)]/g,'')
    .replace(/財團法人|社團法人|基金會|慈善事業|社會福利|附設|私立|長期照顧服務機構|長期照顧服務|居家式服務類|社區式|日間照顧中心|日間照顧|日照中心|日照|社區長照機構|台北市|新北市|桃園市|基隆市|政府|社會局|衛生局|委託|經營管理|接受派案帳號|機構/g,'')
    .trim();
  const chinese=core.match(/[一-鿿]{2,}/);
  return chinese?chinese[0].slice(-8):(core.slice(0,8)||'未命名');
}
function agencyAlias(name){return autoAlias(name);}
function splitLine(line){
  let cols=line.split('\t').map(s=>s.trim()).filter((s,i,a)=>!(s===''&&(i===0||i===a.length-1)));
  if(cols.length<10) cols=line.trim().split(/\s{2,}/).map(s=>s.trim());
  return cols;
}
function parseInput(text){
  const lines=text.replace(/\r/g,'').split('\n').map(s=>s.trimEnd()).filter(s=>s.trim());
  const data=[];
  for(const line of lines){
    const c=splitLine(line);
    if(!/^\d+$/.test(c[0]||'')) continue;
    if(c.length<10) continue;
    const service=parseService(c[2]);
    data.push({
      index:Number(c[0]),date:c[1]||'',month:rocMonth(c[1]),rawService:c[2]||'',code:service.code,name:service.name,
      type:c[3]||'',qty:Number(c[4])||0,time:c[5]||'',agency:c[6]||'',staff:c[7]||'',status:c[8]||'',aa10:c[9]||'',systemId:c[10]||''
    });
  }
  return data;
}
function filters(){return {month:monthFilter.value};}
function filteredRows(){const f=filters();return rows.filter(r=>!f.month||r.month===f.month);}
function uniq(arr){return [...new Set(arr.filter(Boolean))].sort((a,b)=>a.localeCompare(b,'zh-Hant'));}
function fillSelect(el,values,label){const old=el.value;el.innerHTML=`<option value="">${label}</option>`+values.map(v=>`<option>${escapeHtml(v)}</option>`).join('');if(values.includes(old))el.value=old;}
function updateFilters(){fillSelect(monthFilter,uniq(rows.map(r=>r.month)),'全部月份');}
function normalizeCode(code=''){return String(code).trim().toUpperCase();}
function unitPrice(code=''){
  const c=normalizeCode(code);
  const direct=Number(priceMap[c]);
  if(Number.isFinite(direct)) return direct;
  // 相容系統新舊寫法，例如 BA05、BA05-1、BA05-2 均屬餐食照顧。
  const baseFallback={BA05:310,'BA05-1':310,'BA05-2':310,BA15:195,'BA15-1':195,'BA15-2':195,BA16:130,'BA16-1':130,'BA16-2':130};
  const fallback=Number(baseFallback[c]);
  return Number.isFinite(fallback)?fallback:null;
}
function amount(r){const p=unitPrice(r.code);return p==null?0:p*r.qty;}
function render(){
  const data=filteredRows();
  renderCategorySummary(data);
  const calendarData=activeCategory?data.filter(r=>inCategory(r.code,activeCategory)):data;
  renderCalendar(calendarData);
}
function categoryOf(code=''){
  if(code.startsWith('BA')) return 'BA';
  if(code.startsWith('BB')||code.startsWith('BC')||code.startsWith('BD')) return 'BB';
  if(code.startsWith('C')) return 'C';
  if(code.startsWith('DA')) return 'DA';
  if(code.startsWith('GA')||code.startsWith('SC')) return 'GA/SC';
  return 'OT';
}
function inCategory(code,prefix){return categoryOf(code)===prefix;}
function categoryCardHtml(prefix,data){
  const subset=data.filter(r=>inCategory(r.code,prefix));
  const total=subset.reduce((s,r)=>s+amount(r),0);
  const agencies=new Map();
  for(const r of subset){
    if(!agencies.has(r.agency))agencies.set(r.agency,{amount:0,codes:new Map()});
    const a=agencies.get(r.agency);a.amount+=amount(r);
    if(!a.codes.has(r.code))a.codes.set(r.code,{qty:0,amount:0});
    const c=a.codes.get(r.code);c.qty+=r.qty;c.amount+=amount(r);
  }
  const body=[...agencies.entries()].sort((a,b)=>b[1].amount-a[1].amount).map(([agency,v])=>{
    const items=[...v.codes.entries()].sort((a,b)=>a[0].localeCompare(b[0])).map(([code,x])=>`${escapeHtml(code)} ×${x.qty}${unitPrice(code)==null?'・未設定單價':'・'+money(x.amount)}`).join('、');
    return `<div class="category-agency"><div class="category-agency-top"><span>${escapeHtml(agencyAlias(agency)||'未標示單位')}</span><span>${money(v.amount)}</span></div><div class="category-items">${items}</div></div>`;
  }).join('');
  return `<section class="category-card ${activeCategory===prefix?'active':''}" data-category="${prefix}" title="點擊篩選月曆；再次點擊取消"><div class="category-head"><span class="category-name">${prefix}</span><span class="category-total">${money(total)}</span></div><div class="category-body">${body||'<div class="category-empty">本月無資料</div>'}</div></section>`;
}
function renderCategorySummary(data){
  const combined=data.filter(r=>['BA','BB','C'].includes(categoryOf(r.code))).reduce((s,r)=>s+amount(r),0);
  const careCards=['BA','BB','C'].map(prefix=>categoryCardHtml(prefix,data)).join('');
  const otherCards=['DA','GA/SC','OT'].map(prefix=>categoryCardHtml(prefix,data)).join('');
  categorySummary.innerHTML=`<section class="care-summary"><div class="care-summary-head"><div class="care-summary-title">照顧及專業服務使用額度</div><div class="care-summary-total"><span>BA＋BB＋C 總計</span><strong>${money(combined)}</strong></div></div><div class="care-subgrid">${careCards}</div></section>${otherCards}`;
  document.querySelectorAll('[data-category]').forEach(card=>card.addEventListener('click',()=>{
    activeCategory=activeCategory===card.dataset.category?'':card.dataset.category;
    render();
  }));
}

function parseTimeRange(value=''){
  const m=String(value).match(/(\d{1,2}:\d{2})\s*[~～-]\s*(\d{1,2}:\d{2})/);
  return m?{start:m[1].padStart(5,'0'),end:m[2].padStart(5,'0')}:null;
}
function timeToMinutes(value=''){
  const m=String(value).match(/^(\d{1,2}):(\d{2})$/);
  return m?Number(m[1])*60+Number(m[2]):null;
}
function formatDuration(start='',end=''){
  let a=timeToMinutes(start),b=timeToMinutes(end);
  if(a==null||b==null) return '';
  if(b<a) b+=24*60;
  const mins=Math.max(0,b-a),h=Math.floor(mins/60),m=mins%60;
  if(h&&m) return `${h}小時${m}分鐘`;
  if(h) return `${h}小時`;
  return `${m}分鐘`;
}
function displayClock(value=''){return String(value).replace(':','');}
function displayTimeRange(start='',end=''){return start&&end?`${displayClock(start)}-${displayClock(end)}`:'';}
function displayRawTime(value=''){
  const main=String(value).split('[')[0].trim();
  const tr=parseTimeRange(main);
  return tr?displayTimeRange(tr.start,tr.end):main.replace(/:/g,'').replace(/[~～]/g,'-');
}
function mergeContinuousServices(dayRows){
  const buckets=new Map();
  for(const r of dayRows){
    const key=`${r.agency}||${r.staff}`;
    if(!buckets.has(key)) buckets.set(key,[]);
    buckets.get(key).push(r);
  }
  const groups=[];
  for(const sameProviderRows of buckets.values()){
    sameProviderRows.sort((a,b)=>(parseTimeRange(a.time)?.start||a.time).localeCompare(parseTimeRange(b.time)?.start||b.time));
    for(const r of sameProviderRows){
      const tr=parseTimeRange(r.time);
      const prev=groups[groups.length-1];
      const sameProvider=prev&&prev.staff===r.staff&&prev.agency===r.agency;
      if(sameProvider&&tr&&prev.end===tr.start){
        prev.rows.push(r);prev.end=tr.end;prev.amount+=amount(r);
      }else{
        groups.push({agency:r.agency,staff:r.staff,start:tr?.start||'',end:tr?.end||'',rows:[r],amount:amount(r)});
      }
    }
  }
  return groups.sort((a,b)=>(a.start||'99:99').localeCompare(b.start||'99:99')||a.staff.localeCompare(b.staff));
}
function renderAgency(data){const m=new Map();for(const r of data){if(!m.has(r.agency))m.set(r.agency,{count:0,qty:0,confirmed:0,unclaimed:0,amount:0});const x=m.get(r.agency);x.count++;x.qty+=r.qty;x.amount+=amount(r);if(r.status.includes('確認申報'))x.confirmed++;if(r.status.includes('未核銷'))x.unclaimed++;}
 agencyBody.innerHTML=[...m.entries()].sort((a,b)=>b[1].amount-a[1].amount).map(([k,v])=>`<tr><td><strong>${escapeHtml(agencyAlias(k))}</strong></td><td>${escapeHtml(k)}</td><td class="num">${v.count}</td><td class="num">${v.qty}</td><td class="num">${v.confirmed}</td><td class="num">${v.unclaimed}</td><td class="num"><strong>${money(v.amount)}</strong></td></tr>`).join('')||`<tr><td colspan="7" class="empty">尚無資料</td></tr>`;}
function renderCalendar(data){
  const months=uniq(rows.map(r=>r.month));
  let month=monthFilter.value||months[months.length-1]||'';
  if(month&&!monthFilter.value){monthFilter.value=month;data=filteredRows();}
  if(!month){calendarTitle.textContent='請先貼上資料';calendarGrid.innerHTML='<div class="empty" style="grid-column:1/-1">尚無月曆資料</div>';return;}
  const [ry,mm]=month.split('/').map(Number), y=ry+1911, m=mm-1;
  calendarTitle.textContent=`民國 ${ry} 年 ${mm} 月服務月曆${activeCategory?'｜'+activeCategory:''}`;
  const first=new Date(y,m,1), last=new Date(y,m+1,0);
  const mondayOffset=(first.getDay()+6)%7;
  const start=new Date(y,m,1-mondayOffset);
  const weekdays=['一','二','三','四','五','六','日'];
  let html=weekdays.map((x,i)=>`<div class="weekday ${i>=5?'weekend':''}">${x}</div>`).join('');
  const byDay=new Map();for(const r of data.filter(r=>r.month===month)){const d=rocToDate(r.date);if(!d)continue;const key=d.toISOString().slice(0,10);if(!byDay.has(key))byDay.set(key,[]);byDay.get(key).push(r);}
  for(let i=0;i<42;i++){const d=new Date(start);d.setDate(start.getDate()+i);const key=d.toISOString().slice(0,10), inMonth=d.getMonth()===m;const groups=mergeContinuousServices(byDay.get(key)||[]);
    const isWeekend=d.getDay()===0||d.getDay()===6;
    html+=`<div class="day ${inMonth?'':'other'} ${isWeekend?'weekend':''}" ${inMonth?`data-date="${key}"`:''}><div class="day-num">${d.getDate()}</div><div class="events">${groups.map(g=>{
      const codeQty=new Map();
      for(const r of g.rows) codeQty.set(r.code,(codeQty.get(r.code)||0)+r.qty);
      const codeSort=(a,b)=>a.localeCompare(b,'en',{numeric:true,sensitivity:'base'});
      const codes=[...codeQty.entries()].sort((a,b)=>codeSort(a[0],b[0])).map(([code,qty])=>`${escapeHtml(code)}${qty>1?' ×'+qty:''}`).join('<br>');
      const time=g.start&&g.end?displayTimeRange(g.start,g.end):g.rows.map(r=>escapeHtml(displayRawTime(r.time))).join('<br>');
      const duration=g.start&&g.end?formatDuration(g.start,g.end):'';
      const eventCategory=categoryOf(g.rows[0]?.code||'').replace('/','-');
      return `<div class="event cat-${eventCategory}"><div class="event-time">${time}</div>${duration?`<span class="event-duration">${escapeHtml(duration)}</span>`:''}${agencyAlias(g.agency)?`<span class="event-agency">${escapeHtml(agencyAlias(g.agency))}</span>`:''}<span class="event-code">${codes}</span>${g.staff?`<div class="event-meta">${escapeHtml(g.staff)}</div>`:''}</div>`;
    }).join('')}</div></div>`;
  }
  calendarGrid.innerHTML=html;
  calendarGrid.querySelectorAll('.day[data-date]').forEach(day=>day.addEventListener('click',()=>openDayModal(day.dataset.date,byDay.get(day.dataset.date)||[])));
}
function openDayModal(dateKey,dayRows){
  const d=new Date(dateKey+'T00:00:00');
  const week=['日','一','二','三','四','五','六'][d.getDay()];
  dayModalTitle.textContent=`${d.getMonth()+1}月${d.getDate()}日（${week}）服務紀錄`;
  const groups=mergeContinuousServices(dayRows);
  dayModalBody.innerHTML=groups.length?groups.map(g=>{
    const codeQty=new Map();
    for(const r of g.rows) codeQty.set(r.code,(codeQty.get(r.code)||0)+r.qty);
    const codes=[...codeQty.entries()].sort((a,b)=>a[0].localeCompare(b[0],'en',{numeric:true})).map(([code,qty])=>`<div class="event-code">${escapeHtml(code)}${qty>1?' ×'+qty:''}</div>`).join('');
    const time=g.start&&g.end?displayTimeRange(g.start,g.end):g.rows.map(r=>escapeHtml(displayRawTime(r.time))).join('<br>');
    const duration=g.start&&g.end?formatDuration(g.start,g.end):'';
    const eventCategory=categoryOf(g.rows[0]?.code||'').replace('/','-');
    return `<div class="event cat-${eventCategory}" style="padding:12px 14px;margin-bottom:10px"><div class="event-time" style="font-size:15px">${time}</div>${duration?`<span class="event-duration" style="font-size:13px">${escapeHtml(duration)}</span>`:''}${agencyAlias(g.agency)?`<span class="event-agency" style="font-size:14px">${escapeHtml(agencyAlias(g.agency))}</span>`:''}${codes}${g.staff?`<div class="event-meta" style="font-size:12px">${escapeHtml(g.staff)}</div>`:''}</div>`;
  }).join(''):'<div class="empty">當日無服務紀錄</div>';
  dayModal.classList.add('open');document.body.classList.add('modal-open');
}
function closeDayModal(){dayModal.classList.remove('open');if(!inputModal.classList.contains('open'))document.body.classList.remove('modal-open');}
closeDayBtn.onclick=closeDayModal;
dayModal.addEventListener('click',e=>{if(e.target===dayModal)closeDayModal();});
function openInputModal(){inputModal.classList.add('open');document.body.classList.add('modal-open');setTimeout(()=>rawInput.focus(),0);}
function closeInputModal(){inputModal.classList.remove('open');document.body.classList.remove('modal-open');}
openInputBtn.onclick=openInputModal;
closeInputBtn.onclick=closeInputModal;
cancelInputBtn.onclick=closeInputModal;
inputModal.addEventListener('click',e=>{if(e.target===inputModal)closeInputModal();});
document.addEventListener('keydown',e=>{if(e.key!=='Escape')return;if(dayModal.classList.contains('open'))closeDayModal();else if(inputModal.classList.contains('open'))closeInputModal();});
parseBtn.onclick=()=>{rows=parseInput(rawInput.value);updateFilters();render();localStorage.setItem('ltc_raw_input',rawInput.value);if(!rows.length){alert('未辨識到資料列。請確認貼上的內容包含項次及以 Tab 分隔的欄位。');return;}closeInputModal();};
modalClearBtn.onclick=()=>{rawInput.value='';rows=[];activeCategory='';localStorage.removeItem('ltc_raw_input');updateFilters();render();rawInput.focus();};
monthFilter.addEventListener('change',render);
printCalendarBtn.onclick=()=>window.print();
function shiftMonth(delta){const months=uniq(rows.map(r=>r.month));if(!months.length)return;let cur=monthFilter.value||months[months.length-1];let [ry,mm]=cur.split('/').map(Number);let d=new Date(ry+1911,mm-1+delta,1);const target=`${d.getFullYear()-1911}/${String(d.getMonth()+1).padStart(2,'0')}`;if(!months.includes(target)){alert('該月份沒有資料');return;}monthFilter.value=target;render();}
prevMonthBtn.onclick=()=>shiftMonth(-1);nextMonthBtn.onclick=()=>shiftMonth(1);
const saved=localStorage.getItem('ltc_raw_input');if(saved){rawInput.value=saved;rows=parseInput(saved);updateFilters();render();}
</script>
</body>
</html>
