<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Student Attendance Portal</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet"/>
<style>
  :root {
    --bg: #0d0f1a;
    --surface: #141727;
    --card: #1a1f35;
    --border: #252c45;
    --accent: #4f8ef7;
    --accent2: #7c5cfc;
    --green: #22c97a;
    --red: #f75f5f;
    --amber: #f7a94f;
    --text: #e8ecf5;
    --muted: #6b7394;
    --mono: 'Space Mono', monospace;
    --sans: 'DM Sans', sans-serif;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--sans);
    min-height: 100vh;
    padding: 0;
  }

  /* Animated background */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background:
      radial-gradient(ellipse 60% 40% at 20% 10%, rgba(79,142,247,0.08) 0%, transparent 60%),
      radial-gradient(ellipse 40% 50% at 80% 80%, rgba(124,92,252,0.07) 0%, transparent 60%);
    pointer-events: none;
    z-index: 0;
  }

  /* ── Header ── */
  header {
    position: sticky; top: 0; z-index: 100;
    background: rgba(13,15,26,0.85);
    backdrop-filter: blur(12px);
    border-bottom: 1px solid var(--border);
    padding: 0 2rem;
    display: flex; align-items: center; justify-content: space-between;
    height: 64px;
  }
  .logo {
    display: flex; align-items: center; gap: 10px;
    font-family: var(--mono); font-size: 1rem; font-weight: 700;
    letter-spacing: -0.02em;
  }
  .logo-icon {
    width: 32px; height: 32px; border-radius: 8px;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    display: grid; place-items: center;
    font-size: 16px;
  }
  .header-meta {
    font-size: 0.78rem; color: var(--muted); font-family: var(--mono);
    display: flex; align-items: center; gap: 16px;
  }
  .date-badge {
    background: var(--card); border: 1px solid var(--border);
    padding: 4px 10px; border-radius: 20px;
  }

  /* ── Main Layout ── */
  main {
    position: relative; z-index: 1;
    max-width: 900px; margin: 0 auto;
    padding: 2.5rem 1.5rem;
  }

  /* ── Setup Panel ── */
  .setup-panel {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 2rem;
    margin-bottom: 2rem;
  }
  .panel-title {
    font-family: var(--mono); font-size: 0.7rem;
    text-transform: uppercase; letter-spacing: 0.15em;
    color: var(--accent); margin-bottom: 1.2rem;
  }
  .setup-grid {
    display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 1rem;
    align-items: end;
  }
  @media(max-width:600px){ .setup-grid { grid-template-columns: 1fr; } }

  .field { display: flex; flex-direction: column; gap: 6px; }
  .field label {
    font-size: 0.75rem; color: var(--muted); font-weight: 500; letter-spacing: 0.04em;
  }
  .field input, .field select {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    color: var(--text);
    font-family: var(--sans); font-size: 0.9rem;
    padding: 10px 12px;
    outline: none;
    transition: border-color 0.2s;
  }
  .field input:focus, .field select:focus {
    border-color: var(--accent);
    box-shadow: 0 0 0 3px rgba(79,142,247,0.12);
  }
  .field select option { background: var(--surface); }

  /* ── Period Tabs ── */
  .periods-header {
    display: flex; align-items: center; justify-content: space-between;
    margin-bottom: 1.2rem; flex-wrap: wrap; gap: 12px;
  }
  .page-heading {
    font-size: 1.5rem; font-weight: 600; letter-spacing: -0.03em;
  }
  .page-heading span { color: var(--accent); }

  .period-tabs {
    display: flex; gap: 6px; flex-wrap: wrap;
  }
  .period-tab {
    font-family: var(--mono); font-size: 0.72rem;
    padding: 6px 14px; border-radius: 20px;
    border: 1px solid var(--border); background: var(--card);
    color: var(--muted); cursor: pointer; transition: all 0.18s;
  }
  .period-tab:hover { border-color: var(--accent); color: var(--accent); }
  .period-tab.active {
    background: var(--accent); border-color: var(--accent);
    color: #fff; font-weight: 700;
  }
  .period-tab .dot {
    display: inline-block; width: 6px; height: 6px;
    border-radius: 50%; margin-right: 5px; vertical-align: middle;
    background: currentColor; opacity: 0.5;
  }
  .period-tab.done .dot { background: var(--green); opacity: 1; }

  /* ── Student Table ── */
  .attendance-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 16px;
    overflow: hidden;
    margin-bottom: 1.5rem;
  }
  .card-header {
    padding: 1rem 1.5rem;
    border-bottom: 1px solid var(--border);
    display: flex; align-items: center; justify-content: space-between;
    flex-wrap: wrap; gap: 10px;
  }
  .card-header-left { display: flex; align-items: center; gap: 10px; }
  .period-badge {
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    color: #fff; font-family: var(--mono); font-size: 0.72rem;
    font-weight: 700; padding: 4px 10px; border-radius: 6px;
  }
  .subject-name { font-size: 1rem; font-weight: 600; }

  .bulk-btns { display: flex; gap: 8px; }
  .bulk-btn {
    font-size: 0.75rem; font-family: var(--mono);
    padding: 5px 12px; border-radius: 6px; border: 1px solid;
    cursor: pointer; transition: all 0.15s; font-weight: 700;
  }
  .bulk-present { border-color: var(--green); color: var(--green); background: transparent; }
  .bulk-present:hover { background: var(--green); color: #0d0f1a; }
  .bulk-absent { border-color: var(--red); color: var(--red); background: transparent; }
  .bulk-absent:hover { background: var(--red); color: #fff; }

  table { width: 100%; border-collapse: collapse; }
  thead tr { background: var(--surface); }
  thead th {
    padding: 10px 16px; text-align: left;
    font-size: 0.7rem; font-family: var(--mono);
    color: var(--muted); text-transform: uppercase; letter-spacing: 0.1em;
    font-weight: 400;
  }
  tbody tr {
    border-top: 1px solid var(--border);
    transition: background 0.15s;
  }
  tbody tr:hover { background: rgba(79,142,247,0.04); }
  td {
    padding: 12px 16px; font-size: 0.88rem;
  }
  .student-id {
    font-family: var(--mono); font-size: 0.75rem; color: var(--muted);
  }
  .student-name { font-weight: 500; }

  /* Status Buttons */
  .status-btns { display: flex; gap: 6px; }
  .status-btn {
    font-size: 0.72rem; font-family: var(--mono); font-weight: 700;
    padding: 5px 11px; border-radius: 6px; border: 1px solid transparent;
    cursor: pointer; transition: all 0.15s; letter-spacing: 0.04em;
  }
  .btn-p { border-color: var(--border); color: var(--muted); background: transparent; }
  .btn-p:hover { border-color: var(--green); color: var(--green); }
  .btn-p.active { background: var(--green); color: #0d0f1a; border-color: var(--green); }
  .btn-a { border-color: var(--border); color: var(--muted); background: transparent; }
  .btn-a:hover { border-color: var(--red); color: var(--red); }
  .btn-a.active { background: var(--red); color: #fff; border-color: var(--red); }
  .btn-l { border-color: var(--border); color: var(--muted); background: transparent; }
  .btn-l:hover { border-color: var(--amber); color: var(--amber); }
  .btn-l.active { background: var(--amber); color: #0d0f1a; border-color: var(--amber); }

  /* ── Progress Bar ── */
  .progress-row {
    padding: 12px 16px;
    border-top: 1px solid var(--border);
    display: flex; align-items: center; gap: 12px;
    background: var(--surface);
  }
  .progress-label { font-size: 0.72rem; color: var(--muted); font-family: var(--mono); min-width: 90px; }
  .progress-bar-wrap {
    flex: 1; height: 6px; background: var(--border); border-radius: 99px; overflow: hidden;
  }
  .progress-bar-fill {
    height: 100%; background: linear-gradient(90deg, var(--accent), var(--green));
    border-radius: 99px; transition: width 0.4s ease;
  }
  .progress-count { font-family: var(--mono); font-size: 0.75rem; color: var(--text); min-width: 50px; text-align: right; }

  /* ── Summary ── */
  .summary-grid {
    display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px;
    margin-bottom: 1.5rem;
  }
  @media(max-width:600px){ .summary-grid { grid-template-columns: repeat(2,1fr); } }
  .stat-card {
    background: var(--card); border: 1px solid var(--border);
    border-radius: 12px; padding: 1rem;
    text-align: center;
  }
  .stat-value {
    font-family: var(--mono); font-size: 1.8rem; font-weight: 700;
    line-height: 1;
  }
  .stat-label { font-size: 0.72rem; color: var(--muted); margin-top: 4px; }
  .stat-card.total .stat-value { color: var(--accent); }
  .stat-card.present .stat-value { color: var(--green); }
  .stat-card.absent .stat-value { color: var(--red); }
  .stat-card.late .stat-value { color: var(--amber); }

  /* ── Submit ── */
  .submit-row {
    display: flex; justify-content: flex-end; gap: 10px;
    flex-wrap: wrap;
  }
  .btn-reset {
    background: transparent; border: 1px solid var(--border);
    color: var(--muted); padding: 10px 22px; border-radius: 8px;
    font-family: var(--mono); font-size: 0.8rem; cursor: pointer;
    transition: all 0.15s;
  }
  .btn-reset:hover { border-color: var(--red); color: var(--red); }
  .btn-submit {
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    border: none; color: #fff; padding: 10px 28px; border-radius: 8px;
    font-family: var(--mono); font-size: 0.82rem; font-weight: 700;
    cursor: pointer; transition: opacity 0.15s; letter-spacing: 0.04em;
  }
  .btn-submit:hover { opacity: 0.85; }

  /* ── Toast ── */
  .toast {
    position: fixed; bottom: 28px; right: 28px;
    background: var(--green); color: #0d0f1a;
    font-family: var(--mono); font-size: 0.82rem; font-weight: 700;
    padding: 12px 22px; border-radius: 10px;
    box-shadow: 0 8px 32px rgba(34,201,122,0.3);
    transform: translateY(60px); opacity: 0;
    transition: all 0.35s cubic-bezier(.34,1.56,.64,1);
    z-index: 999;
  }
  .toast.show { transform: translateY(0); opacity: 1; }

  /* ── Empty State ── */
  .empty-state {
    text-align: center; padding: 3rem 1rem;
    color: var(--muted); font-size: 0.9rem;
  }
  .empty-icon { font-size: 2.5rem; margin-bottom: 12px; }
</style>
</head>
<body>

<header>
  <div class="logo">
    <div class="logo-icon">📋</div>
    AttendX
  </div>
  <div class="header-meta">
    <span id="clockEl"></span>
    <span class="date-badge" id="dateEl"></span>
  </div>
</header>

<main>

  <!-- Setup Panel -->
  <div class="setup-panel">
    <div class="panel-title">// Session Setup</div>
    <div class="setup-grid">
      <div class="field">
        <label>Class / Section</label>
        <select id="classSelect">
          <option value="">— Select Class —</option>
          <option>Class 10-A</option>
          <option>Class 10-B</option>
          <option>Class 11-A</option>
          <option>Class 11-B</option>
          <option>Class 12-A</option>
          <option>Class 12-B</option>
        </select>
      </div>
      <div class="field">
        <label>Total Periods Today</label>
        <select id="periodsCount">
          <option value="6">6 Periods</option>
          <option value="7" selected>7 Periods</option>
          <option value="8">8 Periods</option>
        </select>
      </div>
      <div class="field">
        <label>&nbsp;</label>
        <button class="btn-submit" onclick="initPortal()" style="height:42px">Load Attendance →</button>
      </div>
    </div>
  </div>

  <!-- Portal (hidden until loaded) -->
  <div id="portal" style="display:none">

    <!-- Summary -->
    <div class="summary-grid">
      <div class="stat-card total"><div class="stat-value" id="sumTotal">0</div><div class="stat-label">Total Students</div></div>
      <div class="stat-card present"><div class="stat-value" id="sumPresent">0</div><div class="stat-label">Present</div></div>
      <div class="stat-card absent"><div class="stat-value" id="sumAbsent">0</div><div class="stat-label">Absent</div></div>
      <div class="stat-card late"><div class="stat-value" id="sumLate">0</div><div class="stat-label">Late</div></div>
    </div>

    <!-- Period Tabs + Heading -->
    <div class="periods-header">
      <div class="page-heading">Period <span id="activePeriodLabel">1</span></div>
      <div class="period-tabs" id="periodTabs"></div>
    </div>

    <!-- Attendance Card -->
    <div class="attendance-card">
      <div class="card-header">
        <div class="card-header-left">
          <span class="period-badge" id="cardPeriodBadge">P1</span>
          <span class="subject-name" id="cardSubject">—</span>
        </div>
        <div class="bulk-btns">
          <button class="bulk-btn bulk-present" onclick="markAll('P')">✓ All Present</button>
          <button class="bulk-btn bulk-absent" onclick="markAll('A')">✗ All Absent</button>
        </div>
      </div>
      <table>
        <thead>
          <tr>
            <th style="width:70px">Roll No</th>
            <th>Student Name</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody id="studentBody"></tbody>
      </table>
      <div class="progress-row">
        <span class="progress-label" id="progressLabel">0 / 0 marked</span>
        <div class="progress-bar-wrap"><div class="progress-bar-fill" id="progressFill" style="width:0%"></div></div>
        <span class="progress-count" id="progressPct">0%</span>
      </div>
    </div>

    <div class="submit-row">
      <button class="btn-reset" onclick="resetPeriod()">↺ Reset Period</button>
      <button class="btn-submit" onclick="savePeriod()">Save Period ✓</button>
    </div>

  </div>
</main>

<div class="toast" id="toast">✓ Attendance Saved!</div>

<script>
// ── Data ──────────────────────────────────────────────
const STUDENTS = [
  { id: "001", name: "Musmin taj" },
  { id: "002", name: "Mohammed Rizwan" },
  { id: "003", name: "samantha" },
  { id: "004", name: "john cena" },
  { id: "005", name: "thor" },
  { id: "006", name: "Divya Nair" },
  { id: "007", name: "Arjun Singh" },
  { id: "008", name: "Pooja Reddy" },
  { id: "009", name: "Nikhil Gupta" },
  { id: "010", name: "Ananya Krishnan" },
  { id: "011", name: "Vikram Joshi" },
  { id: "012", name: "Meera Kapoor" },
  { id: "013", name: "Rahul Desai" },
  { id: "014", name: "Sana Khan" },
  { id: "015", name: "Aditya Rao" },
];

const SUBJECTS = ["Mathematics","English","Physics","Chemistry","Biology","History","Computer Sc.","P.E."];

let activePeriod = 1;
let totalPeriods = 7;
// attendance[period][studentId] = 'P' | 'A' | 'L' | null
let attendance = {};

// ── Clock ─────────────────────────────────────────────
function updateClock() {
  const now = new Date();
  document.getElementById('clockEl').textContent =
    now.toLocaleTimeString([], {hour:'2-digit',minute:'2-digit',second:'2-digit'});
  document.getElementById('dateEl').textContent =
    now.toLocaleDateString([], {weekday:'short',day:'numeric',month:'short',year:'numeric'});
}
setInterval(updateClock, 1000); updateClock();

// ── Init Portal ───────────────────────────────────────
function initPortal() {
  const cls = document.getElementById('classSelect').value;
  if (!cls) { alert('Please select a class first!'); return; }
  totalPeriods = parseInt(document.getElementById('periodsCount').value);

  // Reset attendance store
  attendance = {};
  for (let p = 1; p <= totalPeriods; p++) {
    attendance[p] = {};
    STUDENTS.forEach(s => { attendance[p][s.id] = null; });
  }

  document.getElementById('portal').style.display = 'block';
  renderTabs();
  switchPeriod(1);
  document.getElementById('portal').scrollIntoView({behavior:'smooth'});
}

// ── Period Tabs ───────────────────────────────────────
function renderTabs() {
  const wrap = document.getElementById('periodTabs');
  wrap.innerHTML = '';
  for (let p = 1; p <= totalPeriods; p++) {
    const btn = document.createElement('button');
    btn.className = 'period-tab' + (p === activePeriod ? ' active' : '');
    btn.dataset.period = p;
    const isDone = isPeriodDone(p);
    if (isDone) btn.classList.add('done');
    btn.innerHTML = `<span class="dot"></span>P${p}`;
    btn.onclick = () => switchPeriod(p);
    wrap.appendChild(btn);
  }
}

function isPeriodDone(p) {
  if (!attendance[p]) return false;
  return STUDENTS.every(s => attendance[p][s.id] !== null);
}

// ── Switch Period ─────────────────────────────────────
function switchPeriod(p) {
  activePeriod = p;
  document.getElementById('activePeriodLabel').textContent = p;
  document.getElementById('cardPeriodBadge').textContent = `P${p}`;
  document.getElementById('cardSubject').textContent = SUBJECTS[(p - 1) % SUBJECTS.length];
  renderStudents();
  updateProgress();
  updateSummary();
  // Update tab highlights
  document.querySelectorAll('.period-tab').forEach(btn => {
    const bp = parseInt(btn.dataset.period);
    btn.classList.toggle('active', bp === activePeriod);
    btn.classList.toggle('done', isPeriodDone(bp));
  });
}

// ── Render Students ───────────────────────────────────
function renderStudents() {
  const tbody = document.getElementById('studentBody');
  tbody.innerHTML = '';
  STUDENTS.forEach(s => {
    const status = attendance[activePeriod][s.id];
    const tr = document.createElement('tr');
    tr.innerHTML = `
      <td class="student-id">${s.id}</td>
      <td class="student-name">${s.name}</td>
      <td>
        <div class="status-btns">
          <button class="status-btn btn-p ${status==='P'?'active':''}" onclick="mark('${s.id}','P',this)">P</button>
          <button class="status-btn btn-a ${status==='A'?'active':''}" onclick="mark('${s.id}','A',this)">A</button>
          <button class="status-btn btn-l ${status==='L'?'active':''}" onclick="mark('${s.id}','L',this)">L</button>
        </div>
      </td>
    `;
    tbody.appendChild(tr);
  });
}

// ── Mark Status ───────────────────────────────────────
function mark(id, status, btn) {
  attendance[activePeriod][id] = status;
  // Update sibling buttons
  const row = btn.closest('tr');
  row.querySelectorAll('.status-btn').forEach(b => {
    b.classList.remove('active');
    if ((b.classList.contains('btn-p') && status==='P') ||
        (b.classList.contains('btn-a') && status==='A') ||
        (b.classList.contains('btn-l') && status==='L')) {
      b.classList.add('active');
    }
  });
  updateProgress();
  updateSummary();
  document.querySelectorAll('.period-tab').forEach(t => {
    t.classList.toggle('done', isPeriodDone(parseInt(t.dataset.period)));
  });
}

function markAll(status) {
  STUDENTS.forEach(s => { attendance[activePeriod][s.id] = status; });
  renderStudents();
  updateProgress();
  updateSummary();
  document.querySelectorAll('.period-tab').forEach(t => {
    t.classList.toggle('done', isPeriodDone(parseInt(t.dataset.period)));
  });
}

// ── Progress ──────────────────────────────────────────
function updateProgress() {
  const total = STUDENTS.length;
  const marked = STUDENTS.filter(s => attendance[activePeriod][s.id] !== null).length;
  const pct = total ? Math.round(marked / total * 100) : 0;
  document.getElementById('progressLabel').textContent = `${marked} / ${total} marked`;
  document.getElementById('progressFill').style.width = pct + '%';
  document.getElementById('progressPct').textContent = pct + '%';
}

// ── Summary Stats ─────────────────────────────────────
function updateSummary() {
  // Across ALL periods, count for current active period only for simplicity
  const data = attendance[activePeriod];
  let present = 0, absent = 0, late = 0;
  STUDENTS.forEach(s => {
    if (data[s.id] === 'P') present++;
    else if (data[s.id] === 'A') absent++;
    else if (data[s.id] === 'L') late++;
  });
  document.getElementById('sumTotal').textContent = STUDENTS.length;
  document.getElementById('sumPresent').textContent = present;
  document.getElementById('sumAbsent').textContent = absent;
  document.getElementById('sumLate').textContent = late;
}

// ── Save / Reset ──────────────────────────────────────
function savePeriod() {
  const marked = STUDENTS.filter(s => attendance[activePeriod][s.id] !== null).length;
  if (marked < STUDENTS.length) {
    if (!confirm(`${STUDENTS.length - marked} student(s) not marked. Save anyway?`)) return;
  }
  document.querySelectorAll('.period-tab').forEach(t => {
    t.classList.toggle('done', isPeriodDone(parseInt(t.dataset.period)));
  });
  showToast('✓ Period ' + activePeriod + ' Saved!');
  // Auto-advance to next unmarked period
  if (activePeriod < totalPeriods) switchPeriod(activePeriod + 1);
}

function resetPeriod() {
  if (!confirm('Reset attendance for this period?')) return;
  STUDENTS.forEach(s => { attendance[activePeriod][s.id] = null; });
  renderStudents(); updateProgress(); updateSummary();
  document.querySelectorAll('.period-tab').forEach(t => {
    t.classList.toggle('done', isPeriodDone(parseInt(t.dataset.period)));
  });
}

// ── Toast ─────────────────────────────────────────────
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 3000);
}
</script>
</body>
</html>
