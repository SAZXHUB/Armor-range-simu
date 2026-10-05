
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
  <title>ห้องทดสอบการเจาะเกราะ</title>
  <link href="https://fonts.googleapis.com/css2?family=Chakra+Petch:wght@400;600;700&display=swap" rel="stylesheet">

  <style>
    /* ==================================================================
       1. ตัวแปรสี (theme)  —  สว่างเป็นค่าเริ่มต้น · มืดตามระบบ หรือบังคับด้วย data-theme
       --bg พื้นหลัง · --pn แผง · --ln เส้นขอบ · --tx ตัวอักษร · --mu ตัวอักษรรอง
       --ac สีเน้น · --ok สำเร็จ · --bad ล้มเหลว · --cv พื้นสนาม · --gr เส้นกริด
       ================================================================== */
    :root {
      box-sizing: border-box;
      padding-top: env(safe-area-inset-top, 0px);
      padding-bottom: env(safe-area-inset-bottom, 0px);

      --bg: #e8eadf;
      --pn: #f5f6ef;
      --ln: #bfc4b0;
      --tx: #1c2216;
      --mu: #5f6a52;
      --ac: #b8630a;
      --ok: #2c7a3f;
      --bad: #c23a1f;
      --cv: #dfe3d3;
      --gr: #cfd5c0;
    }

    @media (prefers-color-scheme: dark) {
      :root:not([data-theme="light"]) {
        --bg: #13160f;
        --pn: #1c2116;
        --ln: #373f2d;
        --tx: #e6ead9;
        --mu: #98a387;
        --ac: #ffb21f;
        --ok: #7bd88f;
        --bad: #ff6a48;
        --cv: #0e100a;
        --gr: #1d2217;
      }
    }

    :root[data-theme="dark"] {
      --bg: #13160f;
      --pn: #1c2116;
      --ln: #373f2d;
      --tx: #e6ead9;
      --mu: #98a387;
      --ac: #ffb21f;
      --ok: #7bd88f;
      --bad: #ff6a48;
      --cv: #0e100a;
      --gr: #1d2217;
    }

    /* ==================================================================
       2. พื้นฐานและโครงหน้า
       ================================================================== */
    * { box-sizing: border-box; }
    html, body { height: auto; }

    body {
      margin: 0;
      background: var(--bg);
      color: var(--tx);
      font: 14px/1.45 'Chakra Petch', system-ui, 'Noto Sans Thai', sans-serif;
    }

    header { padding: 10px 12px 4px; }
    h1 { margin: 0; font-size: 18px; }
    header p { margin: 0; color: var(--mu); font-size: 12px; }

    main {
      display: grid;
      gap: 10px;
      padding: 8px 12px 16px;
    }
    @media (min-width: 900px) {
      main { grid-template-columns: minmax(0, 1fr) 340px; }
    }
    section { min-width: 0; }

    /* ==================================================================
       3. สนามทดสอบ (canvas) · แถบปุ่ม · HUD
       ================================================================== */
    canvas {
      display: block;
      width: 100%;
      height: auto;
      background: var(--cv);
      border: 1px solid var(--ln);
      border-radius: 6px;
      touch-action: none;
    }

    .bar {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
      margin: 8px 0;
      align-items: center;
    }

    button, select {
      font: inherit;
      color: var(--tx);
      background: var(--pn);
      border: 1px solid var(--ln);
      border-radius: 6px;
      padding: 6px 10px;
      min-height: 36px;
    }
    button.on {
      border-color: var(--ac);
      box-shadow: inset 0 0 0 1px var(--ac);
    }
    #fire {
      background: var(--ac);
      color: #111;
      font-weight: 700;
      border-color: var(--ac);
    }

    .hud {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 4px 12px;
      background: var(--pn);
      border: 1px solid var(--ln);
      border-radius: 6px;
      padding: 8px 10px;
    }
    .hud div {
      display: flex;
      justify-content: space-between;
      gap: 8px;
      border-bottom: 1px dashed var(--ln);
    }
    .hud span { color: var(--mu); }
    .hud .w {                       /* แถวเต็มความกว้าง: ผล · สถิติ · สาเหตุ */
      grid-column: 1 / -1;
      display: block;
      border: 0;
    }
    .hud #h_why { color: var(--tx); }

    .ok  { color: var(--ok); }
    .bad { color: var(--bad); }

    /* ==================================================================
       4. แผงควบคุมด้านข้าง
       ================================================================== */
    .box {
      background: var(--pn);
      border: 1px solid var(--ln);
      border-radius: 6px;
      padding: 8px 10px;
      margin-bottom: 10px;
    }
    .box h2 { margin: 0 0 6px; font-size: 14px; }

    select.full { width: 100%; }

    .note { font-size: 12px; color: var(--mu); }
    .mt4  { margin-top: 4px; }
    .my4  { margin: 4px 0; }

    /* สไลเดอร์: ป้ายชื่อ + ค่า อยู่แถวบน, แถบเลื่อนอยู่แถวล่าง */
    label {
      display: grid;
      grid-template-columns: 1fr auto;
      font-size: 12px;
      color: var(--mu);
      margin-top: 4px;
    }
    label input {
      grid-column: 1 / -1;
      width: 100%;
      accent-color: var(--ac);
    }
    label b { color: var(--tx); }

    /* ช่องติ๊ก: กล่อง + ข้อความอยู่แถวเดียวกัน */
    label.check {
      display: flex;
      gap: 6px;
      align-items: center;
      margin-top: 6px;
    }
    label.check.tight { margin-top: 4px; }
    label.check input { width: auto; }

    /* ปุ่มเลือกวัสดุ / โมดูล */
    .chips { display: flex; flex-wrap: wrap; gap: 5px; }
    .chips button {
      font-size: 12px;
      padding: 4px 8px;
      min-height: 32px;
      display: flex;
      align-items: center;
      gap: 5px;
    }
    .chips i {
      width: 12px;
      height: 12px;
      border-radius: 3px;
      border: 1px solid #0006;
    }

    #stt { font-size: 12px; white-space: pre-line; }

    #log { max-height: 170px; overflow: auto; font-size: 12px; }
    #log div { padding: 2px 0; border-bottom: 1px solid var(--ln); }
  </style>
</head>

<body>
  <header>
    <h1>ห้องทดสอบการเจาะเกราะ</h1>
    <p>จำลองกลไกแบบ War Thunder · ช่องกริด 1 ช่อง = 5 เมตร · ความหนาเกราะวาดขยายให้เห็นชัด · มีการกระจายกระสุน (σ) · มุมตกตามระยะ · กด Space เพื่อยิง</p>
  </header>

  <main>
    <!-- ───────────── คอลัมน์ซ้าย: สนามทดสอบ ───────────── -->
    <section>
      <canvas id="cv"></canvas>

      <div class="bar">
        <button data-m="draw" class="on">วาดเกราะ</button>
        <button data-m="erase">ลบเกราะ</button>
        <button data-m="shell">วางกระสุน/เล็ง</button>
        <button data-m="mod">วางโมดูลภายใน</button>
        <button data-m="aim">เล็งจุด</button>

        <button id="fire">ยิง</button>
        <select id="vn">
          <option>50</option>
          <option>100</option>
          <option>200</option>
        </select>
        <button id="vol">ยิงรัว</button>
        <button id="rst">ล้างรอย</button>
        <button id="clr">ล้างเกราะ</button>

        <select id="demo">
          <option value="">ตัวอย่างเกราะ…</option>
          <option value="1">เกราะเอียง 60°</option>
          <option value="2">ERA + เกราะหลัก</option>
          <option value="3">เว้นระยะหลายชั้น</option>
          <option value="4">รถถังจำลอง + โมดูลภายใน</option>
          <option value="5">HESH 8 กก. (เจาะ 85) vs RHA 200</option>
        </select>
      </div>

      <div class="hud">
        <div><span>ประเภท</span><b id="h_ty">-</b></div>
        <div><span>ความเร็ว</span><b id="h_v">-</b></div>
        <div><span>แรงเจาะคงเหลือ</span><b id="h_pen">-</b></div>
        <div><span>มุมกระทบ</span><b id="h_ang">-</b></div>
        <div><span>ความหนาประสิทธิผล</span><b id="h_eff">-</b></div>
        <div><span>เกราะต้านทาน</span><b id="h_need">-</b></div>
        <div class="w"><span>ผล: </span><b id="h_res">รอการยิง</b></div>
        <div class="w"><span>สถิติ: </span><b id="h_st">-</b></div>
        <div class="w"><span>สาเหตุ: </span><span id="h_why">-</span></div>
      </div>
    </section>

    <!-- ───────────── คอลัมน์ขวา: แผงควบคุม ───────────── -->
    <section>
      <div class="box">
        <h2>กระสุน</h2>
        <select id="ty" class="full"></select>
        <div id="sl"></div> <!-- สไลเดอร์พารามิเตอร์กระสุน สร้างจาก SD ใน JS -->
        <label>
          <span>ความเร็วภาพ (สโลว์)</span><b id="o_slo"></b>
          <input type="range" id="slo" min=".1" max="1.5" step=".05" value=".5">
        </label>
        <label class="check">
          <input type="checkbox" id="cam" checked> กล้องซูมสโลว์ตอนกระทบ
        </label>
      </div>

      <div class="box">
        <h2>เส้นโค้งความน่าจะเป็นทะลุ</h2>
        <canvas id="bl" width="300" height="110"></canvas>
      </div>

      <div class="box">
        <h2>เกราะที่จะวาด</h2>
        <div class="chips" id="mt"></div>
        <div class="note mt4" id="mi"></div>
        <label>
          <span>ความหนา (mm)</span><b id="o_th"></b>
          <input type="range" id="th" min="5" max="1000" step="5" value="100">
        </label>
        <label class="check">
          <input type="checkbox" id="snap" checked> ล็อกมุมวาดทีละ 5°
        </label>
      </div>

      <div class="box">
        <h2>ภูมิประเทศ</h2>
        <select id="ter" class="full">
          <option value="0">ราบเรียบ</option>
          <option value="1">เนินกลางทาง</option>
          <option value="2">หลุม/คูน้ำ</option>
          <option value="3">เนินสลับหลุม</option>
          <option value="4">Hull-down (เนินหน้ารถถัง)</option>
          <option value="5">สุ่มภูมิประเทศ</option>
        </select>
      </div>

      <div class="box">
        <h2>โมเดลรถถังเป้าหมาย</h2>
        <select id="tk" class="full"></select>
        <label class="check">
          <input type="checkbox" id="flip"> หันท้ายเข้าหาปืน
        </label>
        <div class="note mt4">ภาพตัดขวางตามยาว · ขยาย 3× · ความหนาเป็นค่าประมาณเพื่อการเล่น ไม่ใช่ข้อมูลจริง</div>
      </div>

      <div class="box">
        <h2>ป้อมปืนและการเล็ง</h2>
        <div class="note">โหมด "เล็งจุด": แตะหรือลากบนสนาม ป้อมจะหมุนไปหาจุดนั้น กด "ยิง" ได้เลย ถ้ายังหมุนไม่เสร็จจะยิงเองเมื่อเล็งตรง</div>
        <label>
          <span>ความเร็วหันป้อม (°/วินาที)</span><b id="o_tsp">40</b>
          <input type="range" id="tsp" min="5" max="180" step="5" value="40">
        </label>
        <label class="check">
          <input type="checkbox" id="lim"> จำกัดมุมกด/เงยลำกล้อง (-8° / +20°)
        </label>
      </div>

      <div class="box">
        <h2>โมดูลภายในรถถัง</h2>
        <div class="chips" id="mdc"></div>
        <div class="note my4">เลือกชนิด แล้วแตะสนามเพื่อวาง · โหมดลบเกราะลบโมดูลได้</div>
        <label class="check tight">
          <input type="checkbox" id="liner"> ชั้นกันสะเก็ด Spall liner (ลดสปอลล์ ~85%)
        </label>
      </div>

      <div class="box">
        <h2>สถานะภายใน</h2>
        <div id="stt"></div>
      </div>

      <div class="box">
        <h2>บันทึกการยิง</h2>
        <div id="log"></div>
      </div>
    </section>
  </main>

  <script>
    /* ================================================================
       ห้องทดสอบการเจาะเกราะ — โค้ดแบ่งเป็นหมวดตามหมายเลขด้านล่าง

       ตัวย่อที่ใช้บ่อย
       $        querySelector ย่อ                 W, H     ขนาดสนาม (px) 1200 × 600
       SC       เมตรต่อพิกเซล (0.1)               GY       ระดับพื้นดิน (แกน y)
       R        องศา → เรเดียน                     c / cv   canvas context / canvas
       MT       วัสดุเกราะ                          T        ชนิดกระสุน (ti = ชนิดที่เลือก)
       P        พารามิเตอร์ปัจจุบัน (จากสไลเดอร์)   TH / mi  ความหนา / วัสดุเกราะที่จะวาด
       PL       แผ่นเกราะทั้งหมดบนสนาม             MO       โมดูลภายในรถถัง (MD = ชนิดโมดูล)
       Sh       กระสุนที่กำลังบิน                   S        ปืน {x, y, a = มุม}
       AIM      จุดเล็ง                              DEC      ภาพประกอบรถถัง
       TG / gy  ความสูงภูมิประเทศ / ระดับพื้นที่ x
       ST       สถิติการยิง                         BT       โหมดยิงรัว (ไม่วาดภาพ ไม่บันทึกผล)
       TK       สีจาก CSS variables สำหรับ canvas   CM       กล้องซูมสโลว์ตอนกระทบ
       เอฟเฟกต์: TR วิถีกระสุน · FX วงระเบิด · PT อนุภาค · JT เจ็ต HEAT · FL เส้นสะเก็ด · LB ป้ายผล · HP จุดกระทบ (ยิงรัว)
       SK กล้องสั่น · FH แฟลชขาว · Q คิวเหตุการณ์หน่วงเวลา
       ================================================================ */

    /* ================================================================
       1. พื้นฐาน — ตัวช่วย · ขนาดสนาม · canvas
       ================================================================ */
    const $ = s => document.querySelector(s);
    const W = 1200;
    const H = 600;
    const SC = .1;
    const GY = H - 90;
    const R = Math.PI / 180;
    const cv = $('#cv');
    const c = cv.getContext('2d');
    const dpr = Math.min(devicePixelRatio || 1, 2);
    cv.width = W * dpr;
    cv.height = H * dpr;
    // แปลงสี "#rrggbb" เป็น [r, g, b]
    function hx(h) {
        return [1, 3, 5].map(i => parseInt(h.substr(i, 2), 16));
    }

    /* ================================================================
       2. ข้อมูลหลัก — วัสดุเกราะ · ชนิดกระสุน · รายการสไลเดอร์
       ================================================================ */
    // ชื่อ, สี, ต้านกระสุนทะลุ(k), ต้านเจ็ต(h), ปรับมุมแฉลบ, ERA[ลดเจ็ต mm, ลดแรงเจาะ%]
    const MT = [
        ['rha', 'เหล็กกล้า RHA', '#8a96a0', 1, 1, 0],
        ['hard', 'เหล็กแข็งผิวหน้า', '#6f7f90', 1.25, 1.1, -6],
        ['w', 'ทังสเตน', '#5b5f7a', 2.3, 1.5, 0],
        ['du', 'ยูเรเนียมเสื่อม DU', '#4d5a52', 2.9, 1.9, 0],
        ['cer', 'เซรามิก', '#d9cfae', 1.5, 2.3, 4],
        ['cmp', 'คอมโพสิต NERA', '#8f7a5a', 1.4, 2, 0],
        ['ti', 'ไทเทเนียม', '#9aa3b8', .85, .9, 0],
        ['al', 'อะลูมิเนียม', '#c3cad1', .4, .4, 0],
        ['zn', 'สังกะสี', '#b0bfb8', .25, .3, 0],
        ['rub', 'ยางกันสะเก็ด', '#3a3a3a', .05, .12, 0],
        ['sl', 'เกราะตาข่าย Slat', '#a58f5a', .1, .2, 0],
        ['e1', 'ERA Kontakt-1', '#7a8f4a', .15, .15, 0, [350, .03]],
        ['e5', 'ERA Kontakt-5', '#4f7a4a', .2, .2, 0, [650, .25]]
    ].map(a => ({
        id: a[0],
        n: a[1],
        col: a[2],
        k: a[3],
        h: a[4],
        ric: a[5],
        era: a[6],
        rgb: hx(a[2])
    }));
    // [pen mm, v m/s, TNT g, ความไวชนวน mm, หน่วง m, normalization °, มุมแฉลบ °]
    const T = [
        ['APCBC', 'ap', [180, 800, 40, 20, .5, 5, 70]],
        ['APFSDS', 'ap', [450, 1650, 0, 0, 0, 1, 78]],
        ['APCR', 'ap', [230, 1000, 0, 0, 0, 2, 68]],
        ['APHE', 'ap', [150, 780, 600, 15, .3, 4, 66]],
        ['HE', 'he', [30, 700, 1000, 0, 0, 0, 80]],
        ['HESH', 'hesh', [90, 700, 1500, 0, 0, 0, 80]],
        ['HE ตั้งเวลา', 'timed', [30, 700, 1000, 0, 15, 0, 80]],
        ['HE สแกนพื้น', 'scan', [30, 700, 1000, 0, 3, 0, 80]],
        ['HEAT', 'heat', [400, 1000, 800, 3, 0, 0, 75]],
        ['HEATFS', 'heat', [520, 1100, 700, 1, 0, 0, 75]],
        ['ATGM HEAT', 'heat', [600, 400, 1500, 3, 0, 0, 75]],
        ['ATGM HEATFS', 'heat', [850, 500, 2000, 1, 0, 0, 75]],
        ['ATGM HE', 'he', [60, 450, 5000, 1, 0, 0, 80]],
        ['ATGM Tandem HEAT', 'heat', [900, 500, 2000, 1, 0, 0, 75]]
    ];
    const DL = { ap: 'หน่วงชนวนหลังทะลุ (m)', timed: 'ระยะตั้งเวลาระเบิด (m)', scan: 'ความสูงสแกนพื้น (m)', x: 'หน่วง (m)' };
    const SD = [
        ['pen', 'อัตราการเจาะ (mm)', 0, 1200, 5],
        ['v', 'แรงยิง · ความเร็วปากกระบอก (m/s)', 100, 2000, 10],
        ['tnt', 'แรงระเบิดกระทบ (g TNT)', 0, 8000, 10],
        ['sens', 'ความไวชนวน · เกราะบางสุดที่ทำงาน (mm)', 0, 60, 1],
        ['dly', 'x', 0, 60, .1],
        ['norm', 'การปรับมุม Normalization (°)', 0, 15, .5],
        ['ric', 'มุมแฉลบ (°)', 40, 89, 1],
        ['rng', 'ระยะยิงจำลอง (m)', 10, 3000, 10],
        ['disp', 'การกระจายกระสุน σ (mrad)', 0, 5, .05],
        ['grav', 'แรงโน้มถ่วง g (m/s²)', 0, 20, .1],
        ['cal', 'ขนาดลำกล้อง (mm) · มีผลต่อ Overmatch', 20, 200, 1],
        ['so', 'ระยะระเบิดห่างเกราะของ HEAT (เท่าเส้นผ่านศูนย์กลาง)', 0, 15, .5],
        ['yaw', 'Yaw แนวยิงหันข้าง (°) · มุมประกอบ 3 มิติ', 0, 60, 1],
        ['rho', 'ความหนาแน่นอากาศ ρ (kg/m³)', .6, 1.5, .005],
        ['wx', 'ลมสวน(+)/ตาม(−) (m/s)', -20, 20, .5],
        ['wz', 'ลมพัดเฉียง (m/s)', -20, 20, .5]
    ];

    /* ================================================================
       3. สถานะของโปรแกรม (global state)
       ================================================================ */
    let P = {
        rng: 500,
        disp: .3,
        grav: 9.81,
        so: 5,
        rho: 1.225,
        wx: 0,
        wz: 0,
        yaw: 0
    };
    let VR = 800;
    let BT = 0;
    let HP = [];
    let CM = { l: 0, d: 1, x: 0, y: 0 };
    let ti = 0;
    let PL = [];
    let mode = 'draw';
    let mi = 0;
    let SL = .5;
    let Sh = null;
    let TR = [];
    let FX = [];
    let PT = [];
    let JT = [];
    let LB = [];
    let TK = {};
    let fr = 0;
    const S = { x: 70, y: 260, a: 0 };
    let MO = [];
    let Q = [];
    let FL = [];
    let SK = 0;
    let FH = 0;
    let md = 0;
    let AIM = null;
    let DEC = null;
    let ct = -1;
    const TG = new Float32Array(W + 1);
    const gy = x => GY - TG[Math.max(0, Math.min(W, Math.round(x)))];
    const KD = () => T[ti][1];
    const ST = { n: 0, p: 0 };
    const sts = () => {
        $('#h_st').textContent = ST.n ? `ยิง ${ST.n} นัด · สำเร็จ ${ST.p} (${Math.round(ST.p / ST.n * 100)}%)` : '-';
    };

    /* ================================================================
       4. ตัวควบคุมบน UI — สไลเดอร์ · ปุ่ม · เมนู
       ================================================================ */
    SD.forEach(([k, l, mn, mx, st]) => {
        $('#sl').insertAdjacentHTML('beforeend', `<label><span id="l_${k}">${l}</span><b id="o_${k}"></b><input type="range" id="i_${k}" min="${mn}" max="${mx}" step="${st}"></label>`);
        $('#i_' + k).oninput = e => {
            P[k] = +e.target.value;
            $('#o_' + k).textContent = P[k];
        };
    });
    // ซิงค์ค่าใน P ไปยังสไลเดอร์และตัวเลขบน UI
    function sync() {
        SD.forEach(([k]) => {
            $('#i_' + k).value = P[k];
            $('#o_' + k).textContent = P[k];
        });
    }
    // เลือกชนิดกระสุน i แล้วโหลดค่าเริ่มต้นของมันเข้า P
    function setTy(i) {
        ti = i;
        ['pen', 'v', 'tnt', 'sens', 'dly', 'norm', 'ric'].forEach((k, j) => P[k] = T[i][2][j]);
        VR = P.v;
        P.cal = [88, 120, 90, 75, 105, 105, 105, 105, 105, 120, 125, 140, 130, 150][i];
        $('#l_dly').textContent = DL[T[i][1]] || DL.x;
        sync();
        $('#h_ty').textContent = T[i][0];
    }
    T.forEach((t, i) => $('#ty').insertAdjacentHTML('beforeend', `<option value="${i}">${t[0]}</option>`));
    $('#ty').onchange = e => setTy(+e.target.value);
    $('#slo').oninput = e => {
        SL = +e.target.value;
        $('#o_slo').textContent = SL;
    };
    $('#o_slo').textContent = SL;
    let TH = 100;
    $('#th').oninput = e => {
        TH = +e.target.value;
        $('#o_th').textContent = TH;
    };
    $('#o_th').textContent = TH;
    MT.forEach((m, i) => {
        const b = document.createElement('button');
        b.innerHTML = `<i style="background:${m.col}"></i>${m.n}`;
        b.onclick = () => {
            mi = i;
            mark();
        };
        $('#mt').append(b);
    });
    // ไฮไลต์ปุ่มวัสดุที่เลือก และแสดงคุณสมบัติของวัสดุนั้น
    function mark() {
        [...$('#mt').children].forEach((b, i) => b.classList.toggle('on', i == mi));
        const m = MT[mi];
        $('#mi').textContent = `ต้านกระสุนทะลุ ×${m.k} · ต้านเจ็ต HEAT ×${m.h}` + (m.era ? ` · ERA ลดเจ็ต ${m.era[0]} mm / ลดแรงเจาะ ${m.era[1] * 100}% (ใช้ได้ครั้งเดียวต่อจุด)` : '');
    }
    document.querySelectorAll('[data-m]').forEach(b => b.onclick = () => {
        mode = b.dataset.m;
        document.querySelectorAll('[data-m]').forEach(x => x.classList.toggle('on', x == b));
    });

    /* ================================================================
       5. โมเดลรถถังเป้าหมาย — ตาราง TNK · tank() · drawDec()
       ================================================================ */
    // โมเดลรถถัง — แต่ละแถวเรียงตามตัวแปรที่ tank() แตกออกมา:
    //   nm ชื่อ · L ความยาว(ม.) · hh สูงตัวถัง(ม.) · gc ระยะห่างพื้น(ม.) · ga มุมเกราะหน้า(°)
    //   ป้อม: tx ตำแหน่ง · tw ยาว · th สูง · tfa มุมหน้า(°)  |  gl ลำกล้อง(ม.)
    //   ความหนาเกราะ mm: gt glacis · lt หน้าล่าง · ft พื้น · rf หลังคา · rr ท้าย · tf หน้าป้อม · ts หลังคาป้อม · tb ท้ายป้อม
    //   hm วัสดุ glacis · tm วัสดุหน้าป้อม · era รหัส ERA ('' = ไม่มี) · cr จำนวนลูกเรือในป้อม
    //   am ตู้กระสุน [['t' = ป้อม | 'h' = ตัวถัง, สัดส่วนตามยาว, สัดส่วนตามสูง]] · bo มีฝาระบายแรงระเบิด (1/0)
    const TNK = [
        ['M1A2 Abrams', 7.9, 1.1, .45, 28, 2.3, 3.5, .95, 70, 5, 320, 100, 25, 30, 30, 620, 45, 40, 'cmp', 'cmp', '', 3, [['t', .92, .35], ['t', .92, .75]], 1],
        ['Leopard 2A7', 7.7, 1.05, .45, 25, 2.1, 3.6, .95, 65, 5.2, 280, 90, 25, 30, 25, 650, 45, 40, 'cmp', 'cmp', '', 3, [['t', .92, .5], ['h', .3, .4]], 1],
        ['Challenger 2', 8.3, 1.15, .45, 30, 2.5, 3.8, 1, 68, 5.5, 300, 100, 30, 30, 30, 700, 45, 45, 'cmp', 'cmp', '', 3, [['t', .5, .8], ['h', .3, .6]], 0],
        ['T-90M', 6.9, .85, .45, 22, 1.8, 2.6, .8, 72, 5, 200, 80, 20, 20, 20, 520, 40, 30, 'cmp', 'cmp', 'e5', 2, [['h', .4, .82], ['h', .55, .82], ['t', .9, .6]], 0],
        ['T-72B', 6.9, .85, .45, 22, 1.8, 2.6, .8, 72, 4.8, 180, 80, 20, 20, 20, 480, 40, 30, 'cmp', 'cmp', 'e1', 2, [['h', .4, .82], ['h', .55, .82], ['t', .9, .6]], 0],
        ['Tiger I', 6.3, 1, .45, 81, 2.6, 2.4, 1, 80, 5.3, 100, 100, 25, 25, 82, 100, 25, 82, 'rha', 'rha', '', 3, [['h', .3, .55], ['h', .6, .55]], 0],
        ['Panther G', 6.9, 1, .5, 35, 2.7, 2.5, .95, 78, 5.2, 80, 60, 17, 16, 40, 100, 16, 45, 'rha', 'rha', '', 3, [['h', .3, .45], ['h', .55, .45]], 0],
        ['T-34-85', 5.9, .85, .4, 30, 2.3, 2.2, .85, 70, 4.2, 45, 45, 20, 20, 40, 90, 20, 52, 'rha', 'rha', '', 3, [['h', .4, .85], ['h', .5, .85]], 0],
        ['M4A3 Sherman', 5.9, 1, .43, 34, 2, 2.4, .95, 68, 3.6, 51, 51, 25, 25, 38, 76, 38, 64, 'rha', 'rha', '', 3, [['h', .35, .5], ['h', .5, .5]], 0],
        ['Panzer IV H', 5.9, .95, .4, 78, 2.1, 2, .9, 78, 3.8, 80, 80, 11, 11, 20, 50, 16, 30, 'rha', 'rha', '', 3, [['h', .3, .5], ['h', .55, .5]], 0]
    ];
    {
        const tk = $('#tk');
        tk.innerHTML = '<option value="">เลือกโมเดลรถถัง…</option><optgroup label="ยุคปัจจุบัน" id="g1"></optgroup><optgroup label="สงครามโลกครั้งที่ 2" id="g2"></optgroup>';
        TNK.forEach((t, i) => $(i < 5 ? '#g1' : '#g2').insertAdjacentHTML('beforeend', `<option value="${i}">${t[0]}</option>`));
        tk.onchange = () => {
            if (tk.value !== '') {
                tank(+tk.value, $('#flip').checked);
            }
        };
        $('#flip').onchange = () => {
            if (ct >= 0) {
                tank(ct, $('#flip').checked);
            }
        };
        $('#tsp').oninput = e => $('#o_tsp').textContent = e.target.value;
    }
    // สร้างรถถังรุ่น i จากตาราง TNK (fl = หันท้ายเข้าหาปืน): แผ่นเกราะ ERA โมดูลภายใน และภาพประกอบ
    function tank(i, fl) {
        const [nm, L, hh, gc, ga, tx, tw, th, tfa, gl, gt, lt, ft, rf, rr, tf, ts, tb, hm, tm, era, cr, am, bo] = TNK[i];
        PL = [];
        MO = [];
        ct = i;
        const U = 30;
        const yb = GY - gc * U;
        const yt = yb - hh * U;
        const yy = yt - th * U;
        const rise = hh * U * .55;
        const gdx = rise / Math.tan(ga * R);
        const LL = L * U;
        const x0 = tx * U;
        const sl = th * U / Math.tan(tfa * R);
        const x1 = x0 + tw * U;
        const fx = 720;
        const X = v => fl ? fx + LL - v : fx + v;
        const M = id => MT.findIndex(m => m.id == id);
        const A = (ax, ay, bx, by, t, m) => mk(X(ax), ay, X(bx), by, t, M(m));
        const F = (f, u, v) => f == 't' ? [x0 + sl / 2 + (x1 - x0 - sl / 2 - 4) * u, yy + (yt - yy) * v] : [gdx + 8 + (LL - gdx - 14) * u, yt + (yb - yt) * v];
        const D = (t, f, u, v, b) => {
            const p = F(f, u, v);
            addMod(t, X(p[0]), p[1]);
            if (b) {
                MO[MO.length - 1].bo = 1;
            }
        };
        A(0, yb, 0, yt + rise, lt, 'rha');
        A(0, yt + rise, gdx, yt, gt, hm);
        A(0, yb, LL, yb, ft, 'rha');
        A(gdx, yt, LL, yt, rf, 'rha');
        A(LL, yb, LL, yt, rr, 'rha');
        A(x0, yt, x0 + sl, yy, tf, tm);
        A(x0 + sl, yy, x1, yy, ts, 'rha');
        A(x1, yy, x1, yt, tb, 'rha');
        if (era) {
            A(-8, yt + rise, gdx - 8, yt, 20, era);
            A(x0 - 8, yt, x0 + sl - 8, yy, 20, era);
        }
        D(3, 'h', .05, .5);
        for (let j = 0; j < cr; j++) {
            D(3, 't', .3 + .22 * j, j % 2 ? .3 : .65);
        }
        D(1, 'h', .82, .5);
        D(2, 'h', .6, .82);
        am.forEach(a => D(0, a[0], a[1], a[2], bo && a[0] == 't'));
        const P2 = a => a.map(p => [X(p[0]), p[1]]);
        DEC = {
            n: nm,
            tr: [X(-10), X(LL + 10)],
            hull: P2([[0, yb], [0, yt + rise], [gdx, yt], [LL, yt], [LL, yb]]),
            tur: P2([[x0, yt], [x0 + sl, yy], [x1, yy], [x1, yt]]),
            gun: [X(x0 + sl / 2), yy + th * U * .45, X(x0 - gl * U)]
        };
        const ym = (yt + yy) / 2;
        S.x = 70;
        S.a = 0;
        S.y = Math.round(ym) - 30;
        AIM = { x: X(x0 + (x1 - x0) / 2), y: ym };
        reset();
        lg('โหลดโมเดล ' + nm + (fl ? ' (หันท้ายเข้าหาปืน)' : '') + ' — ป้อมกำลังหันเข้าหาเป้า');
    }
    // วาดภาพประกอบรถถัง (สายพาน ตัวถัง ป้อม ลำกล้อง) ไว้ด้านหลังแผ่นเกราะ
    function drawDec() {
        if (!DEC) {
            return;
        }
        const d = DEC;
        const g = GY;
        c.save();
        c.globalAlpha = .6;
        c.strokeStyle = TK['--mu'];
        c.lineCap = 'round';
        c.lineWidth = 20;
        c.beginPath();
        c.moveTo(d.tr[0], g - 12);
        c.lineTo(d.tr[1], g - 12);
        c.stroke();
        c.lineWidth = 1.5;
        c.fillStyle = TK['--ln'];
        for (let k = 0; k < 6; k++) {
            c.beginPath();
            c.arc(d.tr[0] + (d.tr[1] - d.tr[0]) * (k + .5) / 6, g - 11, 8, 0, 7);
            c.stroke();
        }
        c.lineWidth = 6;
        c.beginPath();
        c.moveTo(d.gun[0], d.gun[1]);
        c.lineTo(d.gun[2], d.gun[1]);
        c.stroke();
        c.lineWidth = 1.5;
        [d.hull, d.tur].forEach(a => {
            c.beginPath();
            a.forEach((p, i) => i ? c.lineTo(p[0], p[1]) : c.moveTo(p[0], p[1]));
            c.closePath();
            c.fill();
            c.stroke();
        });
        c.globalAlpha = 1;
        c.fillStyle = TK['--tx'];
        c.font = 'bold 12px sans-serif';
        c.fillText(d.n, Math.min(d.tr[0], d.tr[1]), g + 16);
        c.restore();
    }

    /* ================================================================
       6. ป้อมปืนและการเล็ง
       ================================================================ */
    // คืน [มุมที่ปืนควรหัน, มุมที่ยังต้องหมุนอีก] ไปยังจุดเล็ง
    function aimErr() {
        const ta = Math.atan2(AIM.y - S.y, AIM.x - S.x);
        const t2 = $('#lim').checked ? Math.max(-20 * R, Math.min(8 * R, ta)) : ta;
        return [t2, Math.atan2(Math.sin(t2 - S.a), Math.cos(t2 - S.a))];
    }
    // หมุนป้อมเข้าหาจุดเล็งทุกเฟรม (ช้าลงถ้าลูกเรือป้อมเสียชีวิต) และยิงเองเมื่อเล็งตรงถ้ากดยิงค้างไว้
    function aimTick(dt) {
        if (!AIM) {
            return;
        }
        const [, d] = aimErr();
        const cw = MO.filter(o => o.t == 3);
        const cf = cw.length ? Math.max(.1, cw.filter(o => !o.gone && !(o.st > 0)).length / cw.length) : 1;
        const mx = (+$('#tsp').value) * R * dt * cf;
        S.a += Math.abs(d) <= mx ? d : Math.sign(d) * mx;
        if (S.pf && Math.abs(aimErr()[1]) < .3 * R) {
            S.pf = 0;
            fire();
        }
    }
    // วาดปืน เส้นเล็ง และเครื่องหมายจุดเล็ง
    function drawGun() {
        c.save();
        c.translate(S.x, S.y);
        c.rotate(S.a);
        c.fillStyle = TK['--mu'];
        c.fillRect(0, -2.5, 44, 5);
        c.restore();
        c.fillStyle = TK['--mu'];
        c.fillRect(S.x - 24, S.y + 8, 48, 12);
        c.beginPath();
        c.arc(S.x, S.y, 10, 0, 7);
        c.fill();
        c.strokeStyle = TK['--tx'];
        c.lineWidth = 1;
        c.stroke();
        if (AIM) {
            c.strokeStyle = TK['--bad'];
            c.lineWidth = 1.5;
            c.beginPath();
            c.arc(AIM.x, AIM.y, 8, 0, 7);
            c.moveTo(AIM.x - 14, AIM.y);
            c.lineTo(AIM.x + 14, AIM.y);
            c.moveTo(AIM.x, AIM.y - 14);
            c.lineTo(AIM.x, AIM.y + 14);
            c.stroke();
            c.fillStyle = TK['--bad'];
            c.font = '11px sans-serif';
            const e = Math.abs(aimErr()[1]) / R;
            c.fillText(e < .3 ? 'เล็งตรง' : 'Δ' + e.toFixed(1) + '°', AIM.x + 10, AIM.y - 10);
        }
    }

    /* ================================================================
       7. โมดูลภายในรถถัง — ความเสียหาย · ระเบิด · สะเก็ด · ไฟ · HESH
       ================================================================ */
    const MD = [
        ['ตู้กระสุน', '#d6a21e', 9, 30, 6, 'A'],
        ['เครื่องยนต์', '#7d8fa6', 18, 120, 45, 'E'],
        ['ถังน้ำมัน', '#c4642a', 11, 40, 4, 'F'],
        ['ลูกเรือ', '#4f8fdc', 7, 20, 3, 'C']
    ].map(a => ({
        n: a[0],
        col: a[1],
        r: a[2],
        hp: a[3],
        arm: a[4],
        ch: a[5]
    }));
    MD.forEach((d, i) => {
        const b = document.createElement('button');
        b.innerHTML = `<i style="background:${d.col}"></i>${d.n}`;
        b.onclick = () => {
            md = i;
            [...$('#mdc').children].forEach((x, j) => x.classList.toggle('on', j == i));
            document.querySelector('[data-m=mod]').click();
        };
        $('#mdc').append(b);
    });
    $('#mdc').children[0].classList.add('on');
    // วางโมดูลภายในชนิด i ที่ตำแหน่ง (x, y)
    function addMod(i, x, y) {
        MO.push({
            t: i,
            x,
            y,
            hp: MD[i].hp,
            gone: 0,
            dead: 0,
            fire: 0,
            heat: 0,
            smk: 0,
            fl: 0
        });
        stat();
    }
    // ปล่อยอนุภาคควัน n ก้อน
    function smoke(x, y, n) {
        for (let i = 0; i < n; i++) {
            PT.push({
                x: x + Math.random() * 10 - 5,
                y,
                vx: (Math.random() - .5) * 40,
                vy: -20 - Math.random() * 50,
                l: 2 + Math.random() * 2,
                m: 4,
                c: '#6b6b66',
                g: -25,
                s: 6
            });
        }
    }
    // ระเบิดใหญ่: ลูกไฟ แฟลชหน้าจอ กล้องสั่น และควัน
    function bigBoom(x, y, g) {
        const Rr = boom(x, y, g);
        FX.push({
            x,
            y,
            r: 0,
            R: Rr * 1.7,
            l: 1.3,
            big: 1
        });
        FH = Math.min(.8, .2 + g / 9000);
        SK = Math.min(14, 3 + g / 700);
        sp(x, y, 50, 1, 0, '#ff8a3c', 360, 1, 3.2);
        smoke(x, y, Math.min(40, 10 + g / 250));
    }
    // โมดูล o รับความเสียหาย d
    function dmgMod(o, d) {
        if (o.gone) {
            return;
        }
        o.hp -= d;
        o.fl = 1;
        if (o.t == 3 && o.hp > 0) {
            if (d >= 6) {
                o.st = 3;
            }
            if (o.hp < MD[3].hp * .5 && !o.wd) {
                o.wd = 1;
                lg('ลูกเรือบาดเจ็บ — หันป้อมช้าลง', 'bad');
            }
        }
        if (o.hp <= 0) {
            killMod(o);
        }
    }
    // โมดูล o ถูกทำลาย: ตู้กระสุนระเบิด · ถังน้ำมันไฟลุก · เครื่องยนต์ควัน/ไฟ · ลูกเรือเสียชีวิต
    function killMod(o) {
        if (o.t == 0) {
            return detonate(o);
        }
        o.hp = 0;
        if (o.t == 3) {
            o.gone = 1;
            lg('ลูกเรือเสียชีวิต', 'bad');
        }
        else {
            o.dead = 1;
            if (o.t == 2) {
                o.fire = 14;
                lg('ถังน้ำมันแตก ไฟลุกท่วม', 'bad');
            }
            else {
                o.smk = 8;
                lg('เครื่องยนต์พัง ควันพวยพุ่ง', 'bad');
                if (Math.random() < .4) {
                    o.fire = 8;
                    lg('เครื่องยนต์ไฟไหม้', 'bad');
                }
            }
        }
    }
    // ตู้กระสุน o ระเบิด (มีฝาระบายแรงระเบิดจะเบากว่า)
    function detonate(o) {
        if (o.gone) {
            return;
        }
        o.gone = 1;
        o.hp = 0;
        const g = o.bo ? 2200 : 6000;
        bigBoom(o.x, o.y, g);
        res('ตู้กระสุนระเบิด 💥', o.bo ? 'ฝาระบายแรงระเบิด (blow-out) เปิด พลังงานพุ่งขึ้นด้านบน ลูกเรือรอดมากขึ้น' : 'กระสุนในตู้ถูกจุดชนวน (~6 กก. TNT) แรงระเบิดทำลายโมดูลรอบข้าง', 'ok', o.x, o.y);
        blastMods(o.x, o.y, g, o);
    }
    // คลื่นระเบิดที่ (x, y) ขนาด g กรัม TNT ทำความเสียหายโมดูลรอบข้าง (ลดลงถ้ามีเกราะกั้น)
    function blastMods(x, y, g, from) {
        const Rb = 20 + 40 * Math.cbrt(g / 1000);
        MO.forEach(o => {
            if (o === from || o.gone) {
                return;
            }
            const d = Math.hypot(o.x - x, o.y - y);
            if (d > Rb) {
                return;
            }
            const blk = PL.some(p => inter(x, y, o.x, o.y, p));
            const dm = (g / 40) * Math.pow(1 - d / Rb, 1.2) * (blk ? .25 : 1);
            if (dm < 1) {
                return;
            }
            if (o.t == 0 && dm > 8) {
                Q.push({ t: .1 + Math.random() * .3, f: () => detonate(o) });
            }
            else {
                dmgMod(o, dm);
            }
        });
    }
    // ยิงสะเก็ด n ชิ้นเป็นรูปกรวยจาก (x, y) ตรวจชนแผ่นเกราะ/โมดูล คืนจำนวนโมดูลที่ถูก
    function frags(x, y, dx, dy, n, spr, rng, pen) {
        const a0 = Math.atan2(dy, dx);
        const hit = new Map();
        for (let i = 0; i < n; i++) {
            const a = a0 + (Math.random() - .5) * 2 * spr * R;
            const ux = Math.cos(a);
            const uy = Math.sin(a);
            let tm = rng;
            let tg = null;
            PL.forEach(p => {
                const r = inter(x, y, x + ux * rng, y + uy * rng, p);
                if (r && r.t * rng < tm && r.t * rng > 3) {
                    tm = r.t * rng;
                }
            });
            MO.forEach(o => {
                if (o.gone) {
                    return;
                }
                const rx = o.x - x;
                const ry = o.y - y;
                const t = rx * ux + ry * uy;
                if (t > 0 && t < tm && Math.abs(rx * uy - ry * ux) < MD[o.t].r) {
                    tm = t;
                    tg = o;
                }
            });
            if (i < 26) {
                FL.push({
                    x1: x,
                    y1: y,
                    x2: x + ux * tm,
                    y2: y + uy * tm,
                    l: .6
                });
            }
            if (tg) {
                hit.set(tg, (hit.get(tg) || 0) + 1);
            }
        }
        hit.forEach((c2, o) => {
            const d = MD[o.t];
            if (pen < d.arm * .5) {
                return;
            }
            dmgMod(o, c2 * (2 + pen * .12));
            if (o.t == 0 && !o.gone && Math.random() < 1 - Math.pow(.93, c2 * (pen / 40 + .3))) {
                detonate(o);
            }
        });
        if (hit.size) {
            lg('สะเก็ดโดน ' + [...hit.keys()].map(o => MD[o.t].n).join(', '));
        }
        return hit.size;
    }
    // เจ็ต HEAT ทะลวงโมดูลตามแนวเส้น คืนแรงเจาะที่เหลือและจุดสิ้นสุดของเจ็ต
    function hitMods(x1, y1, x2, y2, pen) {
        const L = Math.hypot(x2 - x1, y2 - y1) || 1;
        const dx = (x2 - x1) / L;
        const dy = (y2 - y1) / L;
        const hs = [];
        let st = '';
        MO.forEach(o => {
            if (o.gone) {
                return;
            }
            const rx = o.x - x1;
            const ry = o.y - y1;
            const t = rx * dx + ry * dy;
            if (t > 0 && t < L && Math.abs(rx * dy - ry * dx) < MD[o.t].r) {
                hs.push({ o, t });
            }
        });
        hs.sort((a, b) => a.t - b.t);
        for (const h of hs) {
            const o = h.o;
            const d = MD[o.t];
            dmgMod(o, Math.min(250, 40 + pen * .5));
            st += ' · โดน' + d.n;
            if (o.t == 0 && !o.gone && Math.random() < .9) {
                detonate(o);
            }
            if (pen < d.arm) {
                x2 = x1 + dx * h.t;
                y2 = y1 + dy * h.t;
                st += ' (เจ็ตหมดแรง)';
                break;
            }
            pen -= d.arm;
        }
        return { pen, x: x2, y: y2, st };
    }
    // ตรวจว่ากระสุน s ที่เจาะเข้ามาแล้วชนโมดูลภายในหรือไม่
    function modHit(s) {
        for (const o of MO) {
            const d = MD[o.t];
            if (o.gone || s.hm.has(o) || Math.hypot(s.x - o.x, s.y - o.y) > d.r) {
                continue;
            }
            s.hm.add(o);
            const dm = Math.min(250, 20 + s.pen * .9);
            dmgMod(o, dm);
            lg('กระสุนเจาะ' + d.n + ' (เสียหาย ' + Math.round(dm) + ')');
            if (o.t == 0 && !o.gone && Math.random() < .85) {
                detonate(o);
            }
            if (s.pen < d.arm) {
                s.on = false;
                if (s.tm >= 0) {
                    bigBoom(s.x, s.y, P.tnt);
                    blastMods(s.x, s.y, P.tnt);
                }
                res('กระสุนหยุดที่' + d.n, 'แรงเจาะเหลือ ' + Math.round(s.pen) + ' < ' + d.arm + ' mm', 'bad', s.x, s.y);
                return;
            }
            s.pen -= d.arm;
        }
    }
    // HESH: ขีดจำกัดสปอลล์ Ts = 140·∛(กก.) mm · เกราะที่ต้านทาน < Ts จะเกิดสะเก็ดด้านหลังแม้ไม่ทะลุ
    function hesh(s, p, m, b, hx, hy, nx, ny) {
        const kg = P.tnt / 1000;
        const need = p.t * m.k;
        const Ts = 140 * Math.cbrt(kg);
        const r = need / Math.max(1, Ts);
        const perf = P.pen >= need;
        const w = Math.min(56, 3 + p.t * .09);
        const ln = $('#liner').checked;
        const ox = hx - nx * w / 2;
        const oy = hy - ny * w / 2;
        const f = Math.max(0, Math.min(1, 1 - r));
        const mf = Math.min(1, need / 40);
        boom(hx, hy, P.tnt);
        heat(p, b.u, .8);
        hud({ need: need.toFixed(0) + ' mm', pen: P.pen + ' mm', eff: p.t + ' mm' });
        SK = Math.max(SK, Math.min(10, kg * 1.5));
        const info = `ระเบิด ${kg} กก. สปอลล์ได้ถึง ~${Ts.toFixed(0)} mm · เกราะต้านทาน ${need.toFixed(0)} mm (${Math.round(r * 100)}% ของขีดจำกัด)`;
        if (!perf && r >= 1) {
            return res('ไม่ทะลุ · ไม่เกิดสปอลล์ ✗', 'แรงเจาะ ' + P.pen + ' < ' + need.toFixed(0) + ' mm และ ' + info + ' — แผ่นหนาเกินกว่าคลื่นกระแทกจะสะเก็ดด้านหลัง', 'bad', hx, hy);
        }
        const n = Math.min(70, Math.round((10 + 40 * f * Math.cbrt(kg + .3)) * mf * (ln ? .15 : 1)));
        const fp = Math.round((25 + 70 * f) * (ln ? .5 : 1));
        const brk = perf || r < .35;
        p.holes.push({ u: b.u, ok: brk ? 1 : 2 });
        sp(ox, oy, Math.min(60, n + 8), -nx, -ny, '#ffcf6a', 230, .7, .95);
        const k = frags(ox, oy, -nx, -ny, n, 55, 70 + 14 * kg, fp);
        if (perf) {
            blastMods(ox, oy, P.tnt * .4);
        }
        res(brk ? 'แผ่นเกราะแตกทะลุ ✓' : 'เกิดสปอลล์ด้านหลัง (Scabbing) ✓', (perf ? `แรงเจาะ ${P.pen} ≥ ${need.toFixed(0)} mm` : `แรงเจาะ ${P.pen} < ${need.toFixed(0)} mm แต่ ${info}`) + ` → สะเก็ด ${n} ชิ้น (เจาะได้ ~${fp} mm) โดน ${k} โมดูล` + (ln ? ' · Spall liner ลดสะเก็ด' : ''), 'ok', hx, hy);
    }
    // อัปเดตโมดูลทุกเฟรม: คิวหน่วงเวลา · ไฟ · ควัน · ความร้อนลามไปโมดูลข้างเคียง
    function modsTick(dt) {
        MO.forEach(o => {
            if (o.st > 0) {
                o.st -= dt;
            }
        });
        Q = Q.filter(q => {
            q.t -= dt;
            if (q.t <= 0) {
                q.f();
                return false;
            }
            return true;
        });
        MO.forEach(o => {
            if (o.fire > 0) {
                o.fire -= dt;
                sp(o.x, o.y - 4, 1, 0, -1, '#ff7a1a', 60, .5, .5);
                if (Math.random() < .3) {
                    smoke(o.x, o.y, 1);
                }
                MO.forEach(q => {
                    if (q === o || q.gone || Math.hypot(q.x - o.x, q.y - o.y) > 70) {
                        return;
                    }
                    q.heat += dt * (q.t == 0 ? 3 : 2);
                    if (q.t == 3 && q.heat > 3) {
                        dmgMod(q, dt * 6);
                    }
                    if (q.heat > 12 && q.t == 0) {
                        lg('ไฟลามถึงตู้กระสุน (cook-off)', 'bad');
                        detonate(q);
                    }
                    if (q.heat > 9 && q.t == 2 && !q.fire && !q.dead) {
                        q.dead = 1;
                        q.fire = 10;
                        lg('ถังน้ำมันติดไฟจากเพลิงข้างเคียง', 'bad');
                    }
                });
            }
            if (o.smk > 0) {
                o.smk -= dt;
                if (Math.random() < .3) {
                    smoke(o.x, o.y, 1);
                }
            }
        });
    }
    // วาดโมดูลภายในหนึ่งชิ้นพร้อมแถบพลังชีวิต
    function drawMod(o) {
        const d = MD[o.t];
        const r = d.r;
        c.save();
        c.translate(o.x, o.y);
        c.globalAlpha = o.gone ? .35 : 1;
        c.fillStyle = o.fl > 0 ? '#fff' : o.dead ? '#2a2a2a' : d.col;
        o.fl = Math.max(0, o.fl - .08);
        c.fillRect(-r, -r * .8, r * 2, r * 1.6);
        c.strokeStyle = TK['--tx'];
        c.lineWidth = 1;
        c.strokeRect(-r, -r * .8, r * 2, r * 1.6);
        c.fillStyle = '#000';
        c.font = 'bold ' + Math.max(9, r) + 'px sans-serif';
        c.textAlign = 'center';
        c.fillText(d.ch, 0, r * .35);
        if (!o.gone) {
            c.fillStyle = '#0008';
            c.fillRect(-r, r, r * 2, 3);
            c.fillStyle = o.hp / d.hp > .5 ? TK['--ok'] : TK['--bad'];
            c.fillRect(-r, r, r * 2 * Math.max(0, o.hp / d.hp), 3);
        }
        c.restore();
    }
    // แสดงสถานะรวมและสถานะของแต่ละโมดูลในกล่อง "สถานะภายใน"
    function stat() {
        const E = $('#stt');
        if (!E) {
            return;
        }
        if (!MO.length) {
            E.textContent = 'ยังไม่มีโมดูล — เลือกชนิดแล้ววางในโหมด "วางโมดูลภายใน"';
            return;
        }
        const cr = MO.filter(o => o.t == 3);
        const dd = cr.filter(o => o.gone).length;
        const L = MO.map(o => {
            const d = MD[o.t];
            return d.n + ': ' + (o.gone ? (o.t == 0 ? 'ระเบิด' : 'เสียชีวิต') : o.dead ? 'พัง' : 'ปกติ') + (o.fire > 0 ? ' 🔥' : '') + (o.wd && !o.gone ? ' 🩸' : '') + (o.st > 0 ? ' 💫' : '') + ' ' + Math.round(Math.max(0, o.hp) / d.hp * 100) + '%';
        });
        const sm = MO.some(o => o.t == 0 && o.gone) ? '☠ ถูกทำลาย (กระสุนระเบิด)' : cr.length && dd == cr.length ? '☠ ลูกเรือตายหมด' : MO.some(o => o.t == 1 && o.dead) ? '⚠ เคลื่อนที่ไม่ได้' : dd ? '⚠ ลูกเรือเสียชีวิต ' + dd + '/' + cr.length : 'ยังใช้งานได้';
        E.textContent = sm + '\n' + L.join('\n');
    }

    /* ================================================================
       8. แผ่นเกราะ — สร้าง · ตัวอย่าง · เรขาคณิต · ความร้อน · ระเบิดที่ผิว
       ================================================================ */
    // สร้างแผ่นเกราะจากจุด a ไป b หนา t mm วัสดุดัชนี m
    function mk(ax, ay, bx, by, t, m) {
        const L = Math.hypot(bx - ax, by - ay);
        const n = Math.max(2, Math.ceil(L / 8));
        PL.push({
            a: { x: ax, y: ay },
            b: { x: bx, y: by },
            t,
            m,
            L,
            cells: Array.from({ length: n }, () => ({ h: 0 })),
            holes: []
        });
    }
    // สร้างแผ่นเกราะจากจุดกึ่งกลาง (cx, cy) ความยาว len มุม ang (องศา) วัสดุรหัส id
    function pl(cx, cy, len, ang, t, id) {
        const d = len / 2 * Math.cos(ang * R);
        const e = len / 2 * Math.sin(ang * R);
        mk(cx - d, cy - e, cx + d, cy + e, t, MT.findIndex(m => m.id == id));
    }
    // ล้างรอยกระสุน ความร้อน สถิติ และสถานะโมดูล (แผ่นเกราะยังอยู่)
    function reset() {
        ST.n = ST.p = 0;
        sts();
        CM.l = 0;
        PL.forEach(p => {
            p.cells.forEach(q => {
                q.h = 0;
                q.used = false;
            });
            p.holes = [];
        });
        TR = [];
        FX = [];
        PT = [];
        JT = [];
        LB = [];
        Sh = null;
        MO.forEach(o => Object.assign(o, {
            hp: MD[o.t].hp,
            gone: 0,
            dead: 0,
            fire: 0,
            heat: 0,
            smk: 0,
            fl: 0,
            st: 0,
            wd: 0
        }));
        FL = [];
        Q = [];
        SK = FH = 0;
        stat();
    }
    $('#rst').onclick = reset;
    $('#clr').onclick = () => {
        PL = [];
        MO = [];
        DEC = null;
        ct = -1;
        reset();
    };
    $('#demo').onchange = e => {
        const v = e.target.value;
        if (!v) {
            return;
        }
        PL = [];
        MO = [];
        DEC = null;
        ct = -1;
        reset();
        if (v == 1) {
            pl(560, 260, 260, 30, 100, 'rha');
        }
        if (v == 2) {
            pl(470, 260, 200, 90, 20, 'e5');
            pl(520, 260, 260, 90, 150, 'rha');
        }
        if (v == 3) {
            pl(430, 260, 220, 90, 40, 'hard');
            pl(500, 260, 220, 90, 60, 'cer');
            pl(570, 260, 220, 90, 40, 'rha');
            pl(650, 260, 220, 90, 15, 'al');
        }
        if (v == 4) {
            pl(450, 250, 150, 75, 80, 'rha');
            pl(560, 178, 220, 0, 30, 'rha');
            pl(560, 322, 220, 0, 25, 'rha');
            pl(670, 250, 150, 90, 50, 'rha');
            [[3, 497, 215], [3, 497, 285], [0, 540, 292], [0, 590, 222], [1, 630, 255], [2, 652, 298]].forEach(a => addMod(...a));
        }
        if (v == 5) {
            pl(560, 260, 300, 90, 200, 'rha');
            setTy(5);
            P.pen = 85;
            P.tnt = 8000;
            sync();
            $('#ty').value = 5;
            [[3, 610, 225], [3, 610, 295], [0, 650, 260]].forEach(a => addMod(...a));
        }
        e.target.value = '';
    };
    // จุดตัดของเส้นตรงกับแผ่นเกราะ p → {t: ตำแหน่งบนเส้น, u: ตำแหน่งบนแผ่น} หรือ null
    function inter(x1, y1, x2, y2, p) {
        const rx = x2 - x1;
        const ry = y2 - y1;
        const sx = p.b.x - p.a.x;
        const sy = p.b.y - p.a.y;
        const d = rx * sy - ry * sx;
        if (!d) {
            return null;
        }
        const t = ((p.a.x - x1) * sy - (p.a.y - y1) * sx) / d;
        const u = ((p.a.x - x1) * ry - (p.a.y - y1) * rx) / d;
        return t >= 0 && t <= 1 && u >= 0 && u <= 1 ? { t, u } : null;
    }
    // ระยะจากจุด (x, y) ถึงแผ่นเกราะ p
    function dseg(x, y, p) {
        const sx = p.b.x - p.a.x;
        const sy = p.b.y - p.a.y;
        const t = Math.max(0, Math.min(1, ((x - p.a.x) * sx + (y - p.a.y) * sy) / (p.L * p.L)));
        return Math.hypot(x - p.a.x - sx * t, y - p.a.y - sy * t);
    }
    // เซลล์ความร้อนของแผ่น p ที่ตำแหน่ง u
    const cellAt = (p, u) => p.cells[Math.min(p.cells.length - 1, Math.floor(u * p.cells.length))];
    // เพิ่มความร้อน (เรืองแสง) ให้แผ่นรอบตำแหน่ง u
    function heat(p, u, a, s = 2.5) {
        const ci = u * (p.cells.length - 1);
        p.cells.forEach((q, i) => q.h += a * Math.exp(-((i - ci) ** 2) / (2 * s * s)));
    }
    // ปล่อยอนุภาคประกาย/สะเก็ด n ชิ้นไปทางทิศ (dx, dy)
    function sp(x, y, n, dx, dy, col, spd = 140, l = .5, spr = 1.2) {
        for (let i = 0; i < n; i++) {
            const a = Math.atan2(dy, dx) + (Math.random() - .5) * spr * 2;
            const v = spd * (.3 + Math.random());
            PT.push({
                x,
                y,
                vx: Math.cos(a) * v,
                vy: Math.sin(a) * v,
                l: l * (.4 + Math.random()),
                m: l,
                c: col
            });
        }
    }
    // ระเบิดที่ผิว: ลูกไฟ ประกาย และทำให้แผ่นใกล้เคียงร้อน คืนรัศมีระเบิด
    function boom(x, y, g) {
        const Rr = 5 + 9 * Math.cbrt(g / 100);
        FX.push({
            x,
            y,
            r: 0,
            R: Rr,
            l: 1
        });
        sp(x, y, Math.min(90, g / 30 + 12), 1, 0, '#ffb21f', 280, .7, 3.2);
        PL.forEach(p => {
            const n = p.cells.length;
            p.cells.forEach((q, i) => {
                const d = Math.hypot(p.a.x + (p.b.x - p.a.x) * (i + .5) / n - x, p.a.y + (p.b.y - p.a.y) * (i + .5) / n - y);
                if (d < Rr * 1.8) {
                    q.h += Math.min(.9, g / 2500) * (1 - d / (Rr * 1.8));
                }
            });
        });
        return Rr;
    }

    /* ================================================================
       9. HUD และบันทึกการยิง
       ================================================================ */
    // เขียนค่าลงช่อง HUD (คีย์ตรงกับ id h_xxx)
    function hud(o) {
        for (const k in o)
            $('#h_' + k).textContent = o[k];
    }
    // เพิ่มข้อความในบันทึกการยิง (เก็บไม่เกิน 40 รายการ)
    function lg(m, cls) {
        if (BT) {
            return;
        }
        const d = document.createElement('div');
        d.className = cls || '';
        d.textContent = m;
        $('#log').prepend(d);
        while ($('#log').children.length > 40) {
            $('#log').lastChild.remove();
        }
    }
    // รายงานผลการยิง: อัปเดต HUD บันทึก สถิติ และป้ายบนสนาม
    function res(t, w, cls, x, y) {
        if (BT) {
            if (cls == 'ok' && Sh && !Sh.cn) {
                Sh.cn = 1;
            }
            return;
        }
        if (cls == 'ok' && Sh && !Sh.cn) {
            Sh.cn = 1;
            ST.p++;
            sts();
        }
        $('#h_res').textContent = t;
        $('#h_res').className = cls || '';
        $('#h_why').textContent = w;
        lg(t + ' — ' + w, cls);
        if (x != null) {
            LB.push({
                x,
                y,
                t,
                cls,
                l: 3
            });
        }
    }

    /* ================================================================
       10. การยิงและการกระทบ — fire · hit · jet · step
       ================================================================ */
    // ยิงหนึ่งนัด: คำนวณวิถี (flight) บวกการกระจาย σ แล้วสร้างกระสุน Sh
    function fire() {
        let pen = P.pen;
        const FB = flight();
        if (KD() == 'ap') {
            pen *= Math.pow(FB.vi / VR, 1.4);
        }
        const gz = () => (Math.random() + Math.random() + Math.random() - 1.5) * 2;
        const desc = FB.ang;
        const aa = S.a + gz() * P.disp / 1000;
        const vj = 1 + gz() * .004;
        if (KD() == 'ap') {
            pen *= Math.pow(vj, 1.4) * (1 + gz() * .03);
        }
        Sh = {
            x: S.x + Math.cos(S.a) * 44,
            y: S.y + Math.sin(S.a) * 44,
            dx: Math.cos(aa),
            dy: Math.sin(aa),
            fd: desc,
            vi: FB.vi,
            yw: P.yaw * R + FB.yw,
            mz: Math.abs(FB.dz + gz() * P.disp / 1000 * P.rng) > 1.7,
            pen,
            d: 0,
            tm: -1,
            cd: 0,
            sk: null,
            hm: new Set(),
            on: true
        };
        TR = [];
        sp(Sh.x, Sh.y, 22, Sh.dx, Sh.dy, '#ffe08a', 300, .3, .5);
        FX.push({
            x: Sh.x,
            y: Sh.y,
            r: 0,
            R: 16,
            l: .6,
            big: 1
        });
        smoke(Sh.x, Sh.y, 5);
        if (ti == 1 || ti == 2) {
            sp(Sh.x + Sh.dx * 20, Sh.y + Sh.dy * 20, 8, Sh.dx, Sh.dy - .4, '#8d948a', 220, 1, 1.4);
        }
        SK = Math.max(SK, 2.5);
        ST.n++;
        sts();
        if (!BT) {
            HP = [];
        }
        lg(`ยิง ${T[ti][0]} ระยะ ${P.rng} m — แรงเจาะที่ปลายทาง ${Math.round(pen)} mm · มุมตก ${(desc / R).toFixed(2)}° · บิน ${FB.t.toFixed(2)} s · v ${Math.round(FB.vi)} m/s · ลมพัดไป ${FB.dz.toFixed(2)} m · σ ${P.disp} mrad${Sh.mz ? ' · พลาดด้านข้าง' : ''}`);
        hud({
            ty: T[ti][0],
            pen: Math.round(pen) + ' mm',
            res: 'กำลังบิน…',
            ang: '-',
            eff: '-',
            need: '-',
            why: '-'
        });
        $('#h_res').className = '';
    }
    $('#fire').onclick = () => {
        if (AIM && Math.abs(aimErr()[1]) > .3 * R) {
            S.pf = 1;
            lg('กำลังหันป้อมเข้าหาเป้า…');
        }
        else {
            fire();
        }
    };
    // กระสุนแฉลบออกจากแผ่นเกราะ
    function bounce(s, n, hx, hy, p, u) {
        const dn = s.dx * n[0] + s.dy * n[1];
        s.dx -= 2 * dn * n[0];
        s.dy -= 2 * dn * n[1];
        s.pen *= .6;
        sp(hx, hy, 18, s.dx, s.dy, '#ffd36b', 200, .5, .5);
        heat(p, u, .45);
        s.cd = 8;
        s.sk = p;
        s.x += s.dx * 4;
        s.y += s.dy * 4;
    }
    // กระสุน s ชนแผ่นเกราะ: แฉลบ / ทะลุ / ชนวน / ERA แยกตามชนิดกระสุน (AP · HEAT · HESH · HE)
    function hit(s, b, hx, hy) {
        if (BT && !s.rec) {
            s.rec = 1;
            s.hx = hx;
            s.hy = hy;
            HP.push(s);
        }
        if (!BT && !s.cm) {
            s.cm = 1;
            CM.x = hx;
            CM.y = hy;
            CM.d = CM.l = 1.1;
        }
        if (s.fd) {
            const a = Math.atan2(s.dy, s.dx) + s.fd * s.dx;
            s.dx = Math.cos(a);
            s.dy = Math.sin(a);
            s.fd = 0;
        }
        const p = b.p;
        const m = MT[p.m];
        const K = KD();
        const sx = p.b.x - p.a.x;
        const sy = p.b.y - p.a.y;
        let nx = -sy / p.L;
        let ny = sx / p.L;
        if (s.dx * nx + s.dy * ny > 0) {
            nx = -nx;
            ny = -ny;
        }
        const th = Math.acos(Math.min(1, -(s.dx * nx + s.dy * ny)) * Math.cos(s.yw || 0)) / R;
        const rc = P.ric + m.ric;
        const om = K == 'ap' ? Math.max(0, Math.min(1, (P.cal / p.t - 1) / 2)) : 0;
        const pr = (th > rc ? Math.min(1, .2 + (th - rc) / 6) : 0) * (1 - om);
        const arm = p.t >= P.sens;
        const cell = cellAt(p, b.u);
        const n = [nx, ny];
        s.x = hx;
        s.y = hy;
        hud({ ang: th.toFixed(1) + '°' });
        const pass = () => {
            s.x += s.dx * 4;
            s.y += s.dy * 4;
            s.cd = 8;
            s.sk = p;
        };
        if (K == 'ap') {
            if (Math.random() < pr) {
                bounce(s, n, hx, hy, p, b.u);
                return res('แฉลบ', 'มุมกระทบ ' + th.toFixed(0) + '° เกินมุมแฉลบ ' + rc + '° (โอกาส ' + Math.round(pr * 100) + '%)', 'bad', hx, hy);
            }
            const te = Math.max(0, th - P.norm) * (1 - om * .6);
            const eff = p.t / Math.max(.05, Math.cos(te * R));
            const need = eff * m.k;
            let ex = om > .05 ? ' · Overmatch ' + Math.round(om * 100) + '%' : '';
            if (m.era && !cell.used) {
                cell.used = true;
                s.pen *= 1 - m.era[1];
                ex = ' · ERA ทำงาน ลดแรงเจาะ ' + Math.round(m.era[1] * 100) + '%';
                FX.push({
                    x: hx,
                    y: hy,
                    r: 0,
                    R: 14,
                    l: 1
                });
            }
            hud({ eff: eff.toFixed(0) + ' mm', need: need.toFixed(0) + ' mm', pen: Math.round(s.pen) + ' mm' });
            if (s.pen >= need) {
                const d0 = Math.atan2(s.dy, s.dx);
                let df = Math.atan2(-ny, -nx) - d0;
                df = Math.atan2(Math.sin(df), Math.cos(df));
                const a1 = d0 + Math.sign(df) * Math.min(Math.abs(df), Math.min(P.norm, th) * R);
                s.dx = Math.cos(a1);
                s.dy = Math.sin(a1);
                s.pen -= need;
                p.holes.push({ u: b.u, ok: 1 });
                heat(p, b.u, .25 + .75 * Math.min(1, need / 250));
                pass();
                const spl = m.id == 'rub' ? 1 : Math.min(60, Math.round(5 + need / 6));
                sp(s.x, s.y, Math.min(spl, 40), s.dx, s.dy, '#ffcf6a', 220, .6, .6);
                frags(s.x, s.y, s.dx, s.dy, Math.min(spl, 50), 30, 70 + need * .3, Math.max(6, need * .4));
                if (P.tnt > 0 && arm) {
                    s.tm = Math.max(.1, P.dly / SC);
                    ex += ' · ชนวนทำงาน จะระเบิดใน ' + P.dly + ' m';
                }
                res('ทะลุ ✓', `แรงเจาะเกินที่ต้องการ (${need.toFixed(0)} mm) เหลือ ${Math.round(s.pen)} mm · สะเก็ด ${spl} ชิ้น` + ex, 'ok', hx, hy);
            }
            else {
                p.holes.push({ u: b.u, ok: 0 });
                heat(p, b.u, .8);
                if (P.tnt > 0 && arm) {
                    boom(hx, hy, P.tnt);
                    res('ระเบิดนอกเกราะ', 'ชนวนทำงานที่ผิวเกราะ แรงเจาะ ' + Math.round(s.pen) + ' < ' + need.toFixed(0) + ' mm' + ex, 'bad', hx, hy);
                }
                else {
                    if (s.pen >= need * .88) {
                        frags(hx + s.dx * 10, hy + s.dy * 10, s.dx, s.dy, 10, 40, 50, need * .25);
                        lg('ใกล้ขีดจำกัด — เกราะแตกสะเก็ดด้านหลังแม้ไม่ทะลุ', 'ok');
                    }
                    res('ไม่ทะลุ ✗', 'แรงเจาะ ' + Math.round(s.pen) + ' mm < เกราะต้านทาน ' + need.toFixed(0) + ' mm (มุม ' + th.toFixed(0) + '°)' + ex, 'bad', hx, hy);
                }
                s.on = false;
            }
            return;
        }
        if (!arm) {
            pass();
            return res('ชนวนไม่ทำงาน', 'เกราะหนา ' + p.t + ' mm < ความไวชนวน ' + P.sens + ' mm กระสุนทะลุผ่านไปเฉยๆ', 'bad', hx, hy);
        }
        if (Math.random() < pr) {
            bounce(s, n, hx, hy, p, b.u);
            return res('แฉลบ · ชนวนไม่ทำงาน', 'มุมกระทบ ' + th.toFixed(0) + '° เกิน ' + rc + '°', 'bad', hx, hy);
        }
        s.on = false;
        if (K == 'heat') {
            boom(hx, hy, P.tnt * .35);
            return jet(hx, hy, s.dx, s.dy, th);
        }
        if (K == 'hesh') {
            return hesh(s, p, m, b, hx, hy, nx, ny);
        }
        const need = p.t * m.k;
        const inn = P.pen >= need;
        boom(hx, hy, P.tnt);
        blastMods(hx, hy, P.tnt);
        hud({ need: need.toFixed(0) + ' mm', pen: P.pen + ' mm', eff: p.t + ' mm' });
        heat(p, b.u, .6);
        if (inn) {
            const q = K == 'hesh' ? Math.round(20 + 40 * (1 - need / P.pen)) : Math.round(4 + 10 * (1 - need / P.pen));
            sp(hx - nx * 6, hy - ny * 6, q, -nx, -ny, '#ffcf6a', 200, .6, .7);
            p.holes.push({ u: b.u, ok: 1 });
            res(K == 'hesh' ? 'สะเก็ดหลุดเข้าด้านใน ✓' : 'ระเบิดทะลุเกราะบาง ✓', `แรงระเบิดเจาะ ${P.pen} mm ≥ ${need.toFixed(0)} mm · สะเก็ดภายใน ${q} ชิ้น`, 'ok', hx, hy);
        }
        else {
            res('ไม่ทะลุ ✗', `แรงระเบิด ${P.pen} mm < เกราะ ${need.toFixed(0)} mm — เกราะร้อนที่ผิวเท่านั้น`, 'bad', hx, hy);
        }
    }
    // เจ็ต HEAT ทะลวงแผ่นเกราะทีละชั้นตามแนวยิง (เสียพลังเมื่อผ่านช่องว่าง)
    function jet(x, y, dx, dy) {
        let pen = P.pen * (P.so <= 5 ? .7 + .3 * P.so / 5 : Math.max(.3, 1 - .07 * (P.so - 5)));
        let L = 1600;
        let hs = [];
        PL.forEach(p => {
            const r = inter(x - dx * 2, y - dy * 2, x + dx * L, y + dy * L, p);
            if (r) {
                hs.push({ p, t: r.t * (L + 2), u: r.u });
            }
        });
        hs.sort((a, b) => a.t - b.t);
        let prev = 0;
        let ex = x + dx * L;
        let ey = y + dy * L;
        let cnt = 0;
        let stop = '';
        let jx = null;
        let jy = 0;
        for (let i = 0; i < hs.length; i++) {
            const h = hs[i];
            const p = h.p;
            const m = MT[p.m];
            if (i) {
                pen *= Math.max(0, 1 - .1 * (h.t - prev) * SC);
            }
            prev = h.t;
            const cs = Math.max(.05, Math.abs(-dx * (p.b.y - p.a.y) + dy * (p.b.x - p.a.x)) / p.L * Math.cos(Sh.yw || 0));
            const need = p.t / cs * m.h;
            const cl = cellAt(p, h.u);
            if (m.era && !cl.used) {
                cl.used = true;
                const td = T[ti][0].includes('Tandem');
                pen -= td ? 0 : m.era[0] * Math.min(1.8, 1 / cs);
                FX.push({
                    x: x + dx * h.t,
                    y: y + dy * h.t,
                    r: 0,
                    R: 16,
                    l: 1
                });
                lg(td ? 'ERA ถูกหัวนำจุดชนวนก่อน — เจ็ตหลักไม่เสียพลัง' : 'ERA ทำงาน — ตัดเจ็ต ' + m.era[0] + ' mm', 'ok');
            }
            if (pen < need) {
                ex = x + dx * h.t;
                ey = y + dy * h.t;
                heat(p, h.u, .7);
                p.holes.push({ u: h.u, ok: 0 });
                stop = `เจ็ตถูกหยุดที่ ${m.n} ${p.t} mm (ต้านทาน ${need.toFixed(0)} > เหลือ ${Math.max(0, pen).toFixed(0)} mm)`;
                break;
            }
            pen -= need;
            if (!cnt) {
                jx = x + dx * h.t;
                jy = y + dy * h.t;
            }
            cnt++;
            heat(p, h.u, .5);
            p.holes.push({ u: h.u, ok: 1 });
            if (i == hs.length - 1) {
                stop = 'เจ็ตทะลุทุกชั้น';
            }
        }
        if (cnt && jx != null) {
            const r = hitMods(jx, jy, ex, ey, pen);
            pen = r.pen;
            ex = r.x;
            ey = r.y;
            stop += r.st;
        }
        JT.push({
            x1: x,
            y1: y,
            x2: ex,
            y2: ey,
            l: 1
        });
        hud({ pen: Math.max(0, Math.round(pen)) + ' mm', need: '-', eff: '-' });
        if (!hs.length) {
            stop = 'เจ็ตไม่มีเกราะอยู่ข้างหน้า';
        }
        res(stop.includes('ถูกหยุด') ? 'เจ็ตไม่ทะลุ ✗' : 'เจ็ตทะลุ ✓', stop + ` · ทะลุ ${cnt} ชั้น (เจ็ตเสียพลังเมื่อผ่านช่องว่าง ~10%/ม.)`, stop.includes('ถูกหยุด') ? 'bad' : 'ok', ex, ey);
    }
    // เดินกระสุนไปข้างหน้าตาม dt ทีละก้าว ตรวจชนเกราะ โมดูล พื้น และตัวจุดชนวน
    function step(dt) {
        const s = Sh;
        if (!s || !s.on) {
            return;
        }
        let mv = 900 * SL * dt;
        let K = KD();
        while (mv > 0 && s.on) {
            const st = Math.min(mv, 3);
            mv -= st;
            const nx = s.x + s.dx * st;
            const ny = s.y + s.dy * st;
            let b = null;
            for (const p of PL) {
                if (s.mz || p === s.sk && s.cd > 0) {
                    continue;
                }
                const r = inter(s.x, s.y, nx, ny, p);
                if (r && (!b || r.t < b.t)) {
                    b = { t: r.t, u: r.u, p };
                }
            }
            if (b) {
                hit(s, b, s.x + (nx - s.x) * b.t, s.y + (ny - s.y) * b.t);
            }
            else {
                s.x = nx;
                s.y = ny;
                modHit(s);
            }
            s.d += st;
            if (s.cd > 0) {
                s.cd -= st;
            }
            TR.push({ x: s.x, y: s.y });
            if (TR.length > 90) {
                TR.shift();
            }
            if (!s.on) {
                break;
            }
            if (s.tm >= 0) {
                s.tm -= st;
                if (s.tm <= 0) {
                    bigBoom(s.x, s.y, P.tnt);
                    blastMods(s.x, s.y, P.tnt);
                    sp(s.x, s.y, 40, 1, 0, '#ff8a3c', 200, .6, 3);
                    res('ระเบิดภายในตัวถัง', 'ชนวนหน่วงทำงาน — แรงระเบิด ' + P.tnt + ' g TNT', 'ok', s.x, s.y);
                    s.on = false;
                    break;
                }
            }
            if (K == 'timed' && s.d * SC >= P.dly) {
                boom(s.x, s.y, P.tnt);
                res('ระเบิดกลางอากาศ', 'ถึงระยะตั้งเวลา ' + P.dly + ' m', 'ok', s.x, s.y);
                s.on = false;
                break;
            }
            if (K == 'scan' && (gy(s.x) - s.y) * SC <= P.dly) {
                boom(s.x, s.y, P.tnt);
                res('ระเบิดสแกนพื้น', 'สแกนเจอพื้นที่ความสูง ' + ((gy(s.x) - s.y) * SC).toFixed(1) + ' m', 'ok', s.x, s.y);
                s.on = false;
                break;
            }
            const G2 = gy(s.x);
            if (s.y >= G2) {
                sp(s.x, G2, 24, 0, -1, '#8a7f66', 130, 1.1, 1.1);
                if (P.tnt > 0 && K != 'ap') {
                    boom(s.x, G2, P.tnt);
                }
                res('ชนพื้น/ภูมิประเทศ', 'กระสุนไม่โดนเกราะ', 'bad', s.x, G2);
                s.on = false;
                break;
            }
            if (s.x < -20 || s.x > W + 20 || s.y < -20) {
                res('พ้นสนามทดสอบ', 'กระสุนไม่ถูกเกราะ', 'bad');
                s.on = false;
                break;
            }
        }
        if (s.on) {
            hud({ v: Math.round(s.vi) + ' m/s' });
        }
    }

    /* ================================================================
       11. การวาดภาพ — ลูปหลัก draw()
       ================================================================ */
    // สีเกราะตามระดับความร้อน h (เทา → ส้ม → ขาว)
    const hc = (r, h) => {
        h = Math.min(h, 1.5);
        if (h < .02) {
            return `rgb(${r})`;
        }
        const st = [[255, 90, 20], [255, 170, 40], [255, 245, 200]];
        let t = Math.min(h / .5, 1);
        let q = r.map((v, i) => v + (st[0][i] - v) * t);
        if (h > .5) {
            t = Math.min((h - .5) / .5, 1);
            q = q.map((v, i) => v + (st[1][i] - v) * t);
        }
        if (h > 1) {
            t = Math.min((h - 1) / .5, 1);
            q = q.map((v, i) => v + (st[2][i] - v) * t);
        }
        return `rgb(${q.map(Math.round)})`;
    };
    // อ่านค่าสีจาก CSS variables มาเก็บใน TK ให้ canvas ใช้
    function tok() {
        const s = getComputedStyle(document.documentElement);
        ['--cv', '--gr', '--tx', '--mu', '--ac', '--ok', '--bad'].forEach(k => TK[k] = s.getPropertyValue(k).trim());
    }
    // วาดตัวกระสุน (หัวกับหาง)
    function body(x, y, a, col) {
        c.save();
        c.translate(x, y);
        c.rotate(a);
        c.fillStyle = col;
        c.fillRect(-12, -2.5, 12, 5);
        c.beginPath();
        c.moveTo(0, -2.5);
        c.lineTo(9, 0);
        c.lineTo(0, 2.5);
        c.fill();
        c.restore();
    }
    let dr = null;
    let last = 0;
    // ลูปหลัก (requestAnimationFrame): อัปเดตสถานะทุกอย่างแล้ววาดทั้งฉาก
    function draw(t) {
        const rdt = Math.min(.05, (t - last) / 1000 || 0);
        let dt = rdt;
        last = t;
        if (CM.l > 0) {
            CM.l -= rdt;
            dt *= .3;
        }
        if (fr++ % 20 == 0) {
            tok();
            stat();
            bl();
        }
        aimTick(dt);
        step(dt);
        modsTick(dt);
        c.setTransform(dpr, 0, 0, dpr, 0, 0);
        if (CM.l > 0 && $('#cam').checked) {
            const z = 1 + .6 * Math.sin(Math.PI * Math.max(0, Math.min(1, 1 - CM.l / CM.d)));
            c.translate(CM.x, CM.y);
            c.scale(z, z);
            c.translate(-CM.x, -CM.y);
        }
        if (SK > .3) {
            c.translate((Math.random() - .5) * SK, (Math.random() - .5) * SK);
            SK *= .88;
        }
        c.fillStyle = TK['--cv'];
        c.fillRect(0, 0, W, H);
        {
            const g = c.createLinearGradient(0, 0, 0, GY);
            g.addColorStop(0, 'rgba(120,150,190,.2)');
            g.addColorStop(1, 'rgba(255,200,120,.08)');
            c.fillStyle = g;
            c.fillRect(0, 0, W, GY);
        }
        c.strokeStyle = TK['--gr'];
        c.lineWidth = 1;
        c.beginPath();
        for (let x = 0; x <= W; x += 50) {
            c.moveTo(x, 0);
            c.lineTo(x, H);
        }
        for (let y = 0; y <= H; y += 50) {
            c.moveTo(0, y);
            c.lineTo(W, y);
        }
        c.stroke();
        c.fillStyle = TK['--gr'];
        tp();
        c.fill();
        {
            const g = c.createLinearGradient(0, GY - 80, 0, H);
            g.addColorStop(0, 'rgba(0,0,0,.1)');
            g.addColorStop(1, 'rgba(0,0,0,.45)');
            c.fillStyle = g;
            tp();
            c.fill();
        }
        c.strokeStyle = TK['--mu'];
        c.lineWidth = 1.5;
        c.beginPath();
        for (let x = 0; x <= W; x += 4) {
            x ? c.lineTo(x, gy(x)) : c.moveTo(0, gy(0));
        }
        c.stroke();
        c.fillStyle = TK['--mu'];
        c.font = '10px sans-serif';
        for (let x = 100; x < W; x += 100) {
            c.fillText(x * SC + ' m', x + 3, H - 6);
        }
        drawDec();
        MO.forEach(drawMod);
        PL.forEach(p => {
            const m = MT[p.m];
            const w = Math.min(56, 3 + p.t * .09);
            const n = p.cells.length;
            const dx = (p.b.x - p.a.x) / n;
            const dy = (p.b.y - p.a.y) / n;
            c.lineWidth = w;
            c.lineCap = 'butt';
            p.cells.forEach((q, i) => {
                q.h = Math.max(0, q.h - .0007);
                c.strokeStyle = hc(q.used ? [45, 45, 40] : m.rgb, q.h);
                c.beginPath();
                c.moveTo(p.a.x + dx * i, p.a.y + dy * i);
                c.lineTo(p.a.x + dx * (i + 1.08), p.a.y + dy * (i + 1.08));
                c.stroke();
            });
            c.lineWidth = Math.max(1, w * .2);
            c.strokeStyle = 'rgba(255,255,255,.14)';
            c.beginPath();
            c.moveTo(p.a.x, p.a.y);
            c.lineTo(p.b.x, p.b.y);
            c.stroke();
            {
                const qx = -(p.b.y - p.a.y) / p.L * w / 2;
                const qy = (p.b.x - p.a.x) / p.L * w / 2;
                c.lineWidth = 1;
                c.strokeStyle = 'rgba(0,0,0,.5)';
                [1, -1].forEach(k => {
                    c.beginPath();
                    c.moveTo(p.a.x + qx * k, p.a.y + qy * k);
                    c.lineTo(p.b.x + qx * k, p.b.y + qy * k);
                    c.stroke();
                });
            }
            p.holes.forEach(h => {
                c.fillStyle = h.ok == 2 ? '#3a1d0a' : h.ok ? '#05060a' : 'rgba(0,0,0,.5)';
                c.beginPath();
                c.arc(p.a.x + (p.b.x - p.a.x) * h.u, p.a.y + (p.b.y - p.a.y) * h.u, h.ok == 2 ? w * .4 + 2 : h.ok ? w * .28 + 1.5 : Math.max(2, w * .2), 0, 7);
                c.fill();
            });
            if (DEC && p.t < 60) {
                return;
            }
            const e = p.a.y < p.b.y ? p.a : p.b;
            c.fillStyle = TK['--tx'];
            c.font = '11px sans-serif';
            c.fillText(m.n.split(' ')[0] + ' ' + p.t + 'mm', Math.max(4, Math.min(W - 80, e.x + w / 2 + 4)), Math.max(12, e.y - 6));
        });
        if (dr && dr.a) {
            c.setLineDash([6, 5]);
            c.strokeStyle = TK['--ac'];
            c.lineWidth = Math.min(40, 3 + TH * .09);
            c.globalAlpha = .5;
            c.beginPath();
            c.moveTo(dr.a.x, dr.a.y);
            c.lineTo(dr.b.x, dr.b.y);
            c.stroke();
            c.globalAlpha = 1;
            c.setLineDash([]);
        }
        if (TR.length > 1) {
            for (let i = 1; i < TR.length; i++) {
                c.strokeStyle = `rgba(255,178,31,${i / TR.length})`;
                c.lineWidth = 2;
                c.beginPath();
                c.moveTo(TR[i - 1].x, TR[i - 1].y);
                c.lineTo(TR[i].x, TR[i].y);
                c.stroke();
            }
        }
        JT.forEach(j => {
            j.l -= dt * .8;
            c.strokeStyle = `rgba(255,200,90,${Math.max(0, j.l)})`;
            c.lineWidth = 2.5;
            c.shadowColor = '#ff8a3c';
            c.shadowBlur = 10;
            c.beginPath();
            c.moveTo(j.x1, j.y1);
            c.lineTo(j.x2, j.y2);
            c.stroke();
            c.shadowBlur = 0;
        });
        JT = JT.filter(j => j.l > 0);
        FX.forEach(f => {
            f.r += (f.R - f.r) * Math.min(1, dt * 9);
            f.l -= dt * 1.4;
            if (f.big) {
                const g = c.createRadialGradient(f.x, f.y, 0, f.x, f.y, Math.max(1, f.r));
                g.addColorStop(0, `rgba(255,250,220,${Math.min(1, f.l)})`);
                g.addColorStop(.5, `rgba(255,160,40,${Math.min(1, f.l * .8)})`);
                g.addColorStop(1, 'rgba(200,60,10,0)');
                c.fillStyle = g;
            }
            else {
                c.fillStyle = `rgba(255,150,40,${Math.max(0, f.l * .45)})`;
            }
            c.beginPath();
            c.arc(f.x, f.y, f.r, 0, 7);
            c.fill();
            c.strokeStyle = `rgba(255,230,160,${Math.max(0, f.l)})`;
            c.lineWidth = 2;
            c.beginPath();
            c.arc(f.x, f.y, f.r * 1.4, 0, 7);
            c.stroke();
        });
        FX = FX.filter(f => f.l > 0);
        PT.forEach(q => {
            q.l -= dt;
            q.x += q.vx * dt;
            q.y += q.vy * dt;
            q.vy += (q.g === undefined ? 200 : q.g) * dt;
            c.globalAlpha = Math.max(0, q.l / q.m);
            c.fillStyle = q.c;
            const z = q.s || 2.5;
            c.fillRect(q.x - z / 2, q.y - z / 2, z, z);
        });
        c.globalAlpha = 1;
        PT = PT.filter(q => q.l > 0);
        HP.forEach(q => {
            c.globalAlpha = .85;
            c.fillStyle = q.cn ? TK['--ok'] : TK['--bad'];
            c.beginPath();
            c.arc(q.hx, q.hy, 2.6, 0, 7);
            c.fill();
        });
        c.globalAlpha = 1;
        drawGun();
        if (Sh && Sh.on) {
            c.shadowColor = '#ff8a3c';
            c.shadowBlur = ti == 1 ? 4 : 12;
            body(Sh.x, Sh.y, Math.atan2(Sh.dy, Sh.dx), '#fff1c9');
            c.shadowBlur = 0;
        }
        else if (!Sh || !Sh.on) {
            const a = S.a;
            const hx2 = S.x + Math.cos(a) * 50;
            const hy2 = S.y + Math.sin(a) * 50;
            c.setLineDash([4, 5]);
            c.strokeStyle = TK['--ac'];
            c.lineWidth = 1;
            c.beginPath();
            c.moveTo(S.x, S.y);
            c.lineTo(S.x + Math.cos(a) * W * 2, S.y + Math.sin(a) * W * 2);
            c.stroke();
            c.setLineDash([]);
            body(S.x, S.y, a, '#d6dccb');
            c.beginPath();
            c.arc(hx2, hy2, 7, 0, 7);
            c.stroke();
        }
        FL.forEach(f => {
            f.l -= dt * 1.5;
            c.strokeStyle = `rgba(255,220,120,${Math.max(0, f.l * 1.6)})`;
            c.lineWidth = 1;
            c.beginPath();
            c.moveTo(f.x1, f.y1);
            c.lineTo(f.x2, f.y2);
            c.stroke();
        });
        FL = FL.filter(f => f.l > 0);
        c.font = 'bold 13px sans-serif';
        LB.forEach(l => {
            l.l -= dt;
            c.globalAlpha = Math.min(1, l.l);
            c.fillStyle = l.cls == 'ok' ? TK['--ok'] : TK['--bad'];
            c.fillText(l.t, Math.min(W - 130, Math.max(4, l.x + 10)), Math.max(14, l.y - 12 - (3 - l.l) * 6));
        });
        c.globalAlpha = 1;
        LB = LB.filter(l => l.l > 0);
        if (FH > 0) {
            c.fillStyle = `rgba(255,240,200,${FH})`;
            c.fillRect(-10, -10, W + 20, H + 20);
            FH = Math.max(0, FH - dt * 2.5);
        }
        requestAnimationFrame(draw);
    }

    /* ================================================================
       12. อินพุตเมาส์/สัมผัส
       ================================================================ */
    // แปลงพิกัดเมาส์/นิ้วเป็นพิกัดสนาม
    const pos = e => {
        const r = cv.getBoundingClientRect();
        return { x: (e.clientX - r.left) * W / r.width, y: (e.clientY - r.top) * H / r.height };
    };
    // ลากปืนหรือหมุนลำกล้องตามตำแหน่งเมาส์/นิ้ว
    function mvS(q) {
        if (!dr) {
            return;
        }
        if (dr.h) {
            S.a = Math.atan2(q.y - S.y, q.x - S.x);
            AIM = null;
        }
        else {
            S.x = Math.max(10, Math.min(W - 10, q.x));
            S.y = Math.max(10, Math.min(GY - 10, q.y));
        }
    }
    cv.onpointerdown = e => {
        cv.setPointerCapture(e.pointerId);
        const q = pos(e);
        if (mode == 'draw') {
            dr = { a: q, b: q };
        }
        else if (mode == 'erase') {
            const k2 = MO.findIndex(o => Math.hypot(o.x - q.x, o.y - q.y) < MD[o.t].r + 6);
            if (k2 >= 0) {
                MO.splice(k2, 1);
                stat();
                return;
            }
            let bi = -1;
            let bd = 16;
            PL.forEach((p, i) => {
                const d = dseg(q.x, q.y, p);
                if (d < bd) {
                    bd = d;
                    bi = i;
                }
            });
            if (bi >= 0) {
                PL.splice(bi, 1);
            }
        }
        else if (mode == 'aim') {
            AIM = { x: q.x, y: q.y };
            dr = { ai: 1 };
        }
        else if (mode == 'mod') {
            addMod(md, q.x, Math.min(q.y, GY - 10));
        }
        else {
            dr = Math.hypot(q.x - (S.x + Math.cos(S.a) * 50), q.y - (S.y + Math.sin(S.a) * 50)) < 28 ? { h: 1 } : { s: 1 };
            mvS(q);
        }
    };
    cv.onpointermove = e => {
        if (!dr) {
            return;
        }
        const q = pos(e);
        if (dr.ai) {
            AIM = { x: q.x, y: q.y };
            return;
        }
        if (dr.a) {
            let b = q;
            if ($('#snap').checked) {
                const a = Math.atan2(q.y - dr.a.y, q.x - dr.a.x);
                const l = Math.hypot(q.x - dr.a.x, q.y - dr.a.y);
                const g = Math.round(a / (5 * R)) * 5 * R;
                b = { x: dr.a.x + Math.cos(g) * l, y: dr.a.y + Math.sin(g) * l };
            }
            dr.b = b;
        }
        else {
            mvS(q);
        }
    };
    cv.onpointerup = cv.onpointercancel = () => {
        if (dr && dr.a && Math.hypot(dr.b.x - dr.a.x, dr.b.y - dr.a.y) > 10) {
            mk(dr.a.x, dr.a.y, dr.b.x, dr.b.y, TH, mi);
        }
        dr = null;
    };

    /* ================================================================
       13. ภูมิประเทศ · วิถีกระสุน · กราฟความน่าจะเป็น
       ================================================================ */
    // สร้างเส้นทาง (path) ของพื้นดินสำหรับระบายสี
    function tp() {
        c.beginPath();
        c.moveTo(0, H);
        for (let x = 0; x <= W; x += 4) {
            c.lineTo(x, gy(x));
        }
        c.lineTo(W, H);
        c.closePath();
    }
    // สร้างความสูงภูมิประเทศ TG ตามรูปแบบที่เลือก (v = 0–5)
    function terr(v) {
        TG.fill(0);
        const F = {
            1: [[330, 70, 75]],
            2: [[300, 45, -40], [480, 35, -30]],
            3: [[250, 40, 50], [380, 35, -35], [520, 45, 60]],
            4: [[655, 22, 55]],
            5: Array.from({ length: 4 }, () => [150 + Math.random() * 470, 25 + Math.random() * 40, (Math.random() - .4) * 80])
        }[v] || [];
        for (let x = 0; x <= W; x++) {
            let h = 0;
            F.forEach(([x0, sg, a]) => h += a * Math.exp(-(((x - x0) / sg) ** 2)));
            TG[x] = Math.max(-70, h * (x < 640 ? 1 : Math.max(0, 1 - (x - 640) / 50)));
        }
    }
    $('#ter').onchange = e => terr(+e.target.value);
    // การบินเต็มรูปแบบ: จุดมวล 3 มิติ + แรงต้านอากาศ ρ + แรงโน้มถ่วง + ลม · หามุมเงยที่ยิงโดนจุดเล็งที่ระยะ rng
    // จำลองวิถีกระสุน 3 มิติ (แรงต้านอากาศ แรงโน้มถ่วง ลม) หามุมเงยที่ตกที่ระยะ rng คืนความเร็ว/มุมตก/การเบี่ยง/เวลาบิน
    function flight() {
        const k = .00025 * P.rho / 1.225;
        const R2 = Math.max(1, P.rng);
        const run = a => {
            let x = 0;
            let y = 0;
            let z = 0;
            let vx = P.v * Math.cos(a);
            let vy = P.v * Math.sin(a);
            let vz = 0;
            let t = 0;
            let d = .002;
            for (let i = 0; i < 30000 && x < R2; i++) {
                const rx = vx + P.wx;
                const rz = vz - P.wz;
                const vr = Math.hypot(rx, vy, rz);
                const ax = -k * vr * rx;
                const ay = -P.grav - k * vr * vy;
                const az = -k * vr * rz;
                vx += ax * d;
                vy += ay * d;
                vz += az * d;
                x += vx * d;
                y += vy * d;
                z += vz * d;
                t += d;
            }
            return {
                x,
                y,
                z,
                t,
                vx,
                vy,
                vz
            };
        };
        let lo = 0;
        let hi = .3;
        for (let i = 0; i < 14; i++) {
            const m = (lo + hi) / 2;
            run(m).y < 0 ? lo = m : hi = m;
        }
        const r = run((lo + hi) / 2);
        return {
            vi: Math.hypot(r.vx, r.vy, r.vz),
            ang: Math.atan2(-r.vy, r.vx),
            dz: r.z,
            t: r.t,
            yw: Math.atan2(r.vz, r.vx)
        };
    }
    // วาดกราฟโอกาสทะลุตามมุมกระทบ (เฉพาะกระสุนเจาะเกราะ AP)
    function bl() {
        const g = $('#bl').getContext('2d');
        const w = 300;
        const h = 110;
        const m = MT[mi];
        g.setTransform(1, 0, 0, 1, 0, 0);
        g.clearRect(0, 0, w, h);
        g.strokeStyle = TK['--ln'];
        g.strokeRect(.5, .5, w - 1, h - 1);
        g.fillStyle = TK['--mu'];
        g.font = '10px sans-serif';
        if (KD() != 'ap') {
            g.fillText('เฉพาะกระสุนเจาะเกราะ (AP): P(ทะลุ) ตามมุมกระทบ', 6, 16);
            return;
        }
        const rc = P.ric + m.ric;
        const om = Math.max(0, Math.min(1, (P.cal / TH - 1) / 2));
        const p0 = P.pen * Math.pow(flight().vi / VR, 1.4);
        g.strokeStyle = TK['--ac'];
        g.lineWidth = 2;
        g.beginPath();
        for (let a = 0; a <= 85; a++) {
            const a3 = Math.acos(Math.cos(a * R) * Math.cos(P.yaw * R)) / R;
            const te = Math.max(0, a3 - P.norm) * (1 - om * .6);
            const need = TH / Math.max(.05, Math.cos(te * R)) * m.k;
            const pp = 1 / (1 + Math.exp(-1.7 * (p0 - need) / (p0 * .03)));
            const pr = (a3 > rc ? Math.min(1, .2 + (a3 - rc) / 6) : 0) * (1 - om);
            g.lineTo(a / 85 * w, h - 4 - pp * (1 - pr) * (h - 8));
        }
        g.stroke();
        g.fillText('P(ทะลุ) vs มุมกระทบ 0–85° · ' + m.n + ' ' + TH + ' mm', 6, 12);
    }

    /* ================================================================
       14. ยิงรัว · คีย์ลัด · เริ่มต้นโปรแกรม
       ================================================================ */
    $('#vol').onclick = () => {
        const n = +$('#vn').value;
        const sl = SL;
        let pn = 0;
        let kl = 0;
        BT = 1;
        SL = 1;
        HP = [];
        for (let i = 0; i < n; i++) {
            reset();
            if (AIM) {
                S.a = aimErr()[0];
            }
            fire();
            let g = 0;
            while (Sh.on && g++ < 300) {
                step(.05);
            }
            for (g = 0; g < 50; g++) {
                modsTick(.05);
            }
            if (Sh.cn) {
                pn++;
            }
            const cr = MO.filter(o => o.t == 3);
            if (MO.some(o => o.t == 0 && o.gone) || cr.length && cr.every(o => o.gone)) {
                kl++;
            }
        }
        BT = 0;
        SL = sl;
        PT = [];
        FX = [];
        FL = [];
        LB = [];
        JT = [];
        Q = [];
        CM.l = 0;
        const w = k => {
            const p = k / n;
            const z = 1.96;
            const d = 1 + z * z / n;
            const ce = (p + z * z / 2 / n) / d;
            const h = z * Math.sqrt(p * (1 - p) / n + z * z / 4 / n / n) / d;
            return Math.round(p * 100) + '% (95%: ' + Math.round((ce - h) * 100) + '–' + Math.round((ce + h) * 100) + '%)';
        };
        res('สรุปยิงรัว ' + n + ' นัด', 'ทะลุ/ได้ผล ' + w(pn) + ' · ทำลายรถถัง ' + w(kl) + ' · σ ' + P.disp + ' mrad · ระยะ ' + P.rng + ' m', 'ok');
        ST.n = n;
        ST.p = pn;
        sts();
        lg('ยิงรัว ' + n + ' นัด → ทะลุ ' + w(pn) + ' · ทำลาย ' + w(kl), 'ok');
    };
    document.addEventListener('keydown', e => {
        if (e.code == 'Space' && !/INPUT|SELECT|BUTTON/.test(e.target.tagName)) {
            e.preventDefault();
            $('#fire').click();
        }
    });
    setTy(1);
    mark();
    pl(560, 260, 260, 90, 200, 'rha');
    tok();
    requestAnimationFrame(draw);
  </script>
