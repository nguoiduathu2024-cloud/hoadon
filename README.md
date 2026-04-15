<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>T-Mart – Hệ thống bán hàng</title>

  <!-- Google Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Barlow:wght@400;500;600;700;900&family=Barlow+Condensed:wght@600;700;800&family=IBM+Plex+Mono:wght@400;500&display=swap" rel="stylesheet" />

  <!-- jsPDF CDN -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.8.2/jspdf.plugin.autotable.min.js"></script>

  <!-- ═══════════════════════════════════════════
       CSS – HỆ THỐNG T-MART
  ═══════════════════════════════════════════ -->
  <style>
    /* ─── CSS VARIABLES ─────────────────────── */
    :root {
      --bg:         #F4F5F7;
      --surface:    #FFFFFF;
      --border:     #E2E5EA;
      --accent:     #0057FF;
      --accent-dark:#003FBF;
      --accent-lite:#E8EFFE;
      --green:      #00B37E;
      --green-lite: #E0F7F1;
      --red:        #E53E3E;
      --red-lite:   #FFF0F0;
      --amber:      #F59E0B;
      --amber-lite: #FFFBEB;
      --text:       #0D1117;
      --text-2:     #4A5568;
      --text-3:     #9AA5B4;
      --radius:     10px;
      --shadow:     0 1px 3px rgba(0,0,0,.08), 0 4px 16px rgba(0,0,0,.06);
      --shadow-lg:  0 4px 6px rgba(0,0,0,.06), 0 10px 40px rgba(0,0,0,.12);
      --font:       'Barlow', sans-serif;
      --font-mono:  'IBM Plex Mono', monospace;
      --font-head:  'Barlow Condensed', sans-serif;
    }

    /* ─── RESET ─────────────────────────────── */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    html { scroll-behavior: smooth; }
    body {
      font-family: var(--font);
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
      font-size: 15px;
      line-height: 1.5;
    }

    /* ─── UTILITY ────────────────────────────── */
    .hidden  { display: none !important; }
    .mono    { font-family: var(--font-mono); }
    .badge   { display: inline-flex; align-items: center; gap: 4px; padding: 3px 10px; border-radius: 999px; font-size: 12px; font-weight: 600; }
    .badge-green { background: var(--green-lite); color: var(--green); }
    .badge-blue  { background: var(--accent-lite); color: var(--accent); }
    .badge-amber { background: var(--amber-lite); color: var(--amber); }

    /* ─── SCROLLBAR ──────────────────────────── */
    ::-webkit-scrollbar { width: 6px; height: 6px; }
    ::-webkit-scrollbar-track { background: transparent; }
    ::-webkit-scrollbar-thumb { background: #CBD5E0; border-radius: 99px; }

    /* ════════════════════════════════════════════
       TRANG CHỦ
    ════════════════════════════════════════════ */
    #page-home { min-height: 100vh; display: flex; flex-direction: column; }

    /* Header */
    .home-header {
      background: var(--accent);
      padding: 0 40px;
      display: flex; align-items: center; justify-content: space-between;
      height: 64px;
      position: sticky; top: 0; z-index: 100;
    }
    .home-logo {
      font-family: var(--font-head);
      font-size: 28px; font-weight: 800;
      color: #fff; letter-spacing: 1px;
    }
    .home-logo span { color: #FFD600; }
    .home-header-meta { font-size: 13px; color: rgba(255,255,255,.75); }
    .home-header-meta strong { color: #fff; }

    /* Hero strip */
    .home-hero {
      background: linear-gradient(135deg, #003FBF 0%, #0057FF 55%, #0080FF 100%);
      padding: 60px 40px 80px;
      position: relative; overflow: hidden;
    }
    .home-hero::before {
      content: '';
      position: absolute; right: -60px; top: -60px;
      width: 400px; height: 400px; border-radius: 50%;
      background: rgba(255,255,255,.05);
    }
    .home-hero::after {
      content: '';
      position: absolute; right: 100px; bottom: -100px;
      width: 250px; height: 250px; border-radius: 50%;
      background: rgba(255,255,255,.04);
    }
    .home-hero-inner { max-width: 900px; margin: 0 auto; position: relative; z-index: 1; }
    .home-hero h1 {
      font-family: var(--font-head);
      font-size: 52px; font-weight: 800;
      color: #fff; letter-spacing: .5px; line-height: 1.1;
    }
    .home-hero h1 span { color: #FFD600; }
    .home-hero p { color: rgba(255,255,255,.8); margin-top: 10px; font-size: 16px; }
    .home-hero-address {
      display: flex; align-items: center; gap: 8px;
      color: rgba(255,255,255,.7); font-size: 14px; margin-top: 14px;
    }

    /* Main content */
    .home-main {
      flex: 1;
      max-width: 960px;
      margin: -30px auto 60px;
      padding: 0 24px;
      width: 100%;
    }

    /* Cards grid */
    .home-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }

    .card {
      background: var(--surface);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      border: 1px solid var(--border);
      padding: 28px;
      transition: transform .18s, box-shadow .18s;
    }
    .card:hover { transform: translateY(-2px); box-shadow: var(--shadow-lg); }

    .card-label {
      font-size: 11px; font-weight: 700;
      text-transform: uppercase; letter-spacing: 1.2px;
      color: var(--text-3); margin-bottom: 8px;
    }

    /* Action card */
    .card-action {
      border: 2px dashed var(--border);
      background: var(--accent-lite);
      cursor: pointer;
      display: flex; flex-direction: column;
      align-items: center; justify-content: center;
      text-align: center; padding: 36px 28px;
      transition: all .2s;
    }
    .card-action:hover { border-color: var(--accent); background: #D6E4FF; transform: translateY(-2px); }
    .card-action-icon {
      width: 64px; height: 64px;
      background: var(--accent); border-radius: 16px;
      display: flex; align-items: center; justify-content: center;
      margin-bottom: 16px;
      box-shadow: 0 4px 14px rgba(0,87,255,.35);
    }
    .card-action-icon svg { color: white; }
    .card-action-title { font-family: var(--font-head); font-size: 22px; font-weight: 700; color: var(--accent-dark); }
    .card-action-sub { font-size: 13px; color: var(--accent); margin-top: 4px; }

    /* Revenue card */
    .revenue-amount {
      font-family: var(--font-head);
      font-size: 38px; font-weight: 800;
      color: var(--green); letter-spacing: -1px; margin-top: 4px;
    }
    .revenue-month { font-size: 13px; color: var(--text-3); margin-top: 2px; }
    .revenue-divider { height: 1px; background: var(--border); margin: 16px 0; }
    .revenue-stats { display: flex; gap: 20px; }
    .revenue-stat label { font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; color: var(--text-3); }
    .revenue-stat span { display: block; font-size: 17px; font-weight: 700; color: var(--text); margin-top: 2px; }

    /* Recent invoices */
    .recent-card { grid-column: 1 / -1; }
    .recent-table { width: 100%; border-collapse: collapse; margin-top: 12px; }
    .recent-table th {
      text-align: left; font-size: 11px; font-weight: 700;
      text-transform: uppercase; letter-spacing: 1px; color: var(--text-3);
      padding: 8px 12px; border-bottom: 1px solid var(--border);
    }
    .recent-table td {
      padding: 10px 12px; font-size: 14px;
      border-bottom: 1px solid var(--border); color: var(--text-2);
    }
    .recent-table tr:last-child td { border-bottom: none; }
    .recent-table td:first-child { font-family: var(--font-mono); font-size: 13px; color: var(--accent); }
    .recent-table td.amount { font-weight: 700; color: var(--green); }
    .recent-empty { text-align: center; padding: 30px; color: var(--text-3); font-size: 14px; }

    /* ════════════════════════════════════════════
       TRANG TẠO HÓA ĐƠN
    ════════════════════════════════════════════ */
    #page-invoice { min-height: 100vh; display: flex; flex-direction: column; }

    /* Top bar */
    .inv-topbar {
      background: var(--surface);
      border-bottom: 1px solid var(--border);
      padding: 0 32px; height: 60px;
      display: flex; align-items: center; justify-content: space-between;
      position: sticky; top: 0; z-index: 100;
      box-shadow: 0 1px 4px rgba(0,0,0,.06);
    }
    .inv-topbar-left { display: flex; align-items: center; gap: 16px; }
    .btn-back {
      display: flex; align-items: center; gap: 6px;
      padding: 6px 14px; background: transparent;
      border: 1px solid var(--border); border-radius: 6px;
      font-family: var(--font); font-size: 14px; font-weight: 600;
      color: var(--text-2); cursor: pointer; transition: all .15s;
    }
    .btn-back:hover { background: var(--bg); border-color: var(--text-3); }
    .inv-topbar-title { font-family: var(--font-head); font-size: 20px; font-weight: 700; color: var(--text); }
    .inv-topbar-actions { display: flex; gap: 10px; }

    /* Layout */
    .inv-layout {
      flex: 1;
      display: grid;
      grid-template-columns: 1fr 360px;
      gap: 20px;
      max-width: 1280px;
      margin: 0 auto;
      padding: 24px 24px 40px;
      width: 100%;
      align-items: start;
    }

    /* Section heading */
    .sec-title {
      font-family: var(--font-head);
      font-size: 13px; font-weight: 700;
      text-transform: uppercase; letter-spacing: 1.2px;
      color: var(--text-3); margin-bottom: 12px;
      display: flex; align-items: center; gap: 8px;
    }
    .sec-title::after { content: ''; flex: 1; height: 1px; background: var(--border); }

    /* Store info banner */
    .store-banner {
      background: linear-gradient(135deg, var(--accent) 0%, #003FBF 100%);
      border-radius: var(--radius); padding: 20px 24px;
      color: #fff; margin-bottom: 16px;
      display: flex; align-items: center; gap: 20px;
    }
    .store-logo-box {
      width: 52px; height: 52px;
      background: rgba(255,255,255,.15); border-radius: 12px;
      display: flex; align-items: center; justify-content: center;
      flex-shrink: 0;
      font-family: var(--font-head); font-size: 22px; font-weight: 800; color: #FFD600;
    }
    .store-name { font-family: var(--font-head); font-size: 22px; font-weight: 800; letter-spacing: .5px; }
    .store-meta { font-size: 12px; color: rgba(255,255,255,.75); margin-top: 3px; line-height: 1.5; }

    /* Info card */
    .info-card {
      background: var(--surface); border: 1px solid var(--border);
      border-radius: var(--radius); padding: 20px; margin-bottom: 16px;
    }
    .info-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; }
    .info-grid .span2 { grid-column: span 2; }

    .field-group { display: flex; flex-direction: column; gap: 5px; }
    .field-label { font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: .8px; color: var(--text-3); }
    .field-input {
      width: 100%; padding: 9px 12px;
      border: 1.5px solid var(--border); border-radius: 7px;
      font-family: var(--font); font-size: 14px; color: var(--text);
      background: var(--bg); transition: border-color .15s, box-shadow .15s; outline: none;
    }
    .field-input:focus { border-color: var(--accent); box-shadow: 0 0 0 3px rgba(0,87,255,.12); background: var(--surface); }
    .field-input::placeholder { color: var(--text-3); }

    /* Product search */
    .product-search-row {
      display: grid;
      grid-template-columns: 160px 1fr 100px auto;
      gap: 10px; align-items: end; margin-bottom: 14px;
    }
    .autocomplete-wrapper { position: relative; }
    .autocomplete-drop {
      position: absolute; top: calc(100% + 4px); left: 0; right: 0;
      background: var(--surface); border: 1.5px solid var(--accent);
      border-radius: 8px; box-shadow: var(--shadow-lg);
      z-index: 200; max-height: 240px; overflow-y: auto;
    }
    .autocomplete-item {
      padding: 9px 14px; cursor: pointer;
      display: flex; justify-content: space-between; align-items: center; gap: 8px;
      transition: background .12s;
    }
    .autocomplete-item:hover, .autocomplete-item.active { background: var(--accent-lite); }
    .autocomplete-item span { font-size: 13px; color: var(--text); }
    .autocomplete-item small { font-size: 12px; color: var(--text-3); font-family: var(--font-mono); white-space: nowrap; }
    .autocomplete-item-price { font-weight: 700; color: var(--accent); font-size: 13px; white-space: nowrap; }

    /* Table */
    .table-wrap {
      background: var(--surface); border: 1px solid var(--border);
      border-radius: var(--radius); overflow: hidden; margin-bottom: 16px;
    }
    .inv-table { width: 100%; border-collapse: collapse; }
    .inv-table thead tr { background: #F8F9FC; }
    .inv-table th {
      text-align: left; font-size: 11px; font-weight: 700;
      text-transform: uppercase; letter-spacing: 1px; color: var(--text-3);
      padding: 11px 14px; border-bottom: 1px solid var(--border); white-space: nowrap;
    }
    .inv-table td {
      padding: 10px 14px; border-bottom: 1px solid var(--border);
      font-size: 14px; color: var(--text-2); vertical-align: middle;
    }
    .inv-table tbody tr:last-child td { border-bottom: none; }
    .inv-table tbody tr:hover { background: #FAFBFF; }
    .inv-table td.code { font-family: var(--font-mono); font-size: 12px; color: var(--accent); }
    .inv-table td.price { font-family: var(--font-mono); text-align: right; }
    .inv-table td.total { font-weight: 700; color: var(--text); font-family: var(--font-mono); text-align: right; }
    .inv-table th:last-child { text-align: center; }
    .inv-table td:last-child { text-align: center; }

    .qty-input {
      width: 72px; padding: 5px 8px;
      border: 1.5px solid var(--border); border-radius: 6px;
      font-family: var(--font-mono); font-size: 14px; text-align: center;
      color: var(--text); transition: border-color .15s; outline: none;
    }
    .qty-input:focus { border-color: var(--accent); }

    .btn-del {
      width: 30px; height: 30px;
      display: inline-flex; align-items: center; justify-content: center;
      background: var(--red-lite); border: 1px solid #FCA5A5;
      border-radius: 6px; cursor: pointer; color: var(--red);
      transition: all .15s; font-size: 16px; line-height: 1;
    }
    .btn-del:hover { background: var(--red); color: #fff; border-color: var(--red); }

    .table-empty {
      text-align: center; padding: 40px;
      color: var(--text-3); font-size: 14px;
    }
    .table-empty svg { display: block; margin: 0 auto 10px; opacity: .3; }

    /* Payment sidebar */
    .payment-sidebar { display: flex; flex-direction: column; gap: 16px; }
    .pay-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 20px; }

    .pay-row {
      display: flex; justify-content: space-between; align-items: center;
      padding: 8px 0; border-bottom: 1px solid var(--border); gap: 12px;
    }
    .pay-row:last-of-type { border-bottom: none; }
    .pay-row label { font-size: 14px; color: var(--text-2); white-space: nowrap; }
    .pay-row .pay-value { font-family: var(--font-mono); font-size: 15px; font-weight: 600; color: var(--text); text-align: right; }
    .pay-row input.field-input { width: 130px; text-align: right; font-family: var(--font-mono); }

    .pay-total-row {
      display: flex; justify-content: space-between; align-items: center;
      padding: 14px 0 6px; margin-top: 4px; border-top: 2px solid var(--border);
    }
    .pay-total-label { font-size: 16px; font-weight: 700; color: var(--text); }
    .pay-total-value { font-family: var(--font-head); font-size: 28px; font-weight: 800; color: var(--accent); }
    .pay-change-value { font-family: var(--font-head); font-size: 22px; font-weight: 800; color: var(--green); }

    /* Payment method */
    .pay-methods { display: flex; gap: 8px; }
    .pay-method-btn {
      flex: 1; padding: 10px 6px;
      border: 2px solid var(--border); border-radius: 8px;
      background: var(--bg); cursor: pointer;
      text-align: center; font-family: var(--font);
      font-size: 12px; font-weight: 600; color: var(--text-2);
      transition: all .15s;
    }
    .pay-method-btn:hover { border-color: var(--accent); color: var(--accent); background: var(--accent-lite); }
    .pay-method-btn.active { border-color: var(--accent); background: var(--accent-lite); color: var(--accent); }
    .pay-method-btn .method-icon { font-size: 20px; display: block; margin-bottom: 4px; }

    /* Buttons */
    .btn {
      display: inline-flex; align-items: center; justify-content: center; gap: 8px;
      padding: 11px 22px; border-radius: 8px;
      font-family: var(--font); font-size: 14px; font-weight: 700;
      cursor: pointer; border: none; transition: all .18s; white-space: nowrap;
    }
    .btn-primary { background: var(--accent); color: #fff; box-shadow: 0 2px 8px rgba(0,87,255,.3); }
    .btn-primary:hover { background: var(--accent-dark); box-shadow: 0 4px 14px rgba(0,87,255,.4); }
    .btn-success { background: var(--green); color: #fff; box-shadow: 0 2px 8px rgba(0,179,126,.3); }
    .btn-success:hover { background: #008F63; }
    .btn-pdf { background: var(--amber); color: #fff; box-shadow: 0 2px 8px rgba(245,158,11,.3); }
    .btn-pdf:hover { background: #D97706; }
    .btn-outline { background: transparent; color: var(--text-2); border: 1.5px solid var(--border); }
    .btn-outline:hover { background: var(--bg); border-color: var(--text-3); }
    .btn-full { width: 100%; }
    .btn-lg { padding: 14px 28px; font-size: 16px; }

    .action-group { display: flex; flex-direction: column; gap: 10px; }
    .action-group-row { display: flex; gap: 10px; }
    .action-group-row .btn { flex: 1; }

    /* Invoice number badge */
    .inv-number-badge { display: flex; align-items: center; gap: 8px; }
    .inv-number-badge .num {
      font-family: var(--font-mono); font-size: 15px; font-weight: 600;
      color: var(--accent); background: var(--accent-lite); padding: 3px 12px; border-radius: 6px;
    }

    /* Toast */
    #toast {
      position: fixed; bottom: 28px; right: 28px;
      background: var(--text); color: #fff;
      padding: 13px 22px; border-radius: 10px;
      font-size: 14px; font-weight: 500;
      box-shadow: 0 8px 24px rgba(0,0,0,.25);
      z-index: 9999; opacity: 0; transform: translateY(12px);
      transition: all .3s; pointer-events: none;
    }
    #toast.show { opacity: 1; transform: translateY(0); }
    #toast.success { background: var(--green); }
    #toast.error { background: var(--red); }

    /* Responsive */
    @media (max-width: 960px) {
      .inv-layout { grid-template-columns: 1fr; }
      .home-grid { grid-template-columns: 1fr; }
      .recent-card { grid-column: 1; }
      .info-grid { grid-template-columns: 1fr 1fr; }
      .product-search-row { grid-template-columns: 1fr 1fr; }
      .home-hero h1 { font-size: 36px; }
    }
  </style>
</head>
<body>

<!-- ════════════════════════════════════════════
     TRANG CHỦ
════════════════════════════════════════════ -->
<div id="page-home">
  <header class="home-header">
    <div class="home-logo">T<span>-</span>Mart</div>
    <div class="home-header-meta" id="header-date"></div>
  </header>

  <div class="home-hero">
    <div class="home-hero-inner">
      <h1>Hệ thống bán hàng<br><span>T-Mart</span></h1>
      <p>Quản lý hóa đơn – Thanh toán – Xuất PDF nhanh chóng</p>
      <div class="home-hero-address">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0118 0z"/><circle cx="12" cy="10" r="3"/></svg>
        33C, Ngõ Chợ Khâm Thiên, Văn Miếu - Quốc Tử Giám, Hà Nội &nbsp;|&nbsp; Hotline: 1900-9999
      </div>
    </div>
  </div>

  <main class="home-main">
    <div class="home-grid">

      <!-- Tạo hóa đơn mới -->
      <div class="card card-action" onclick="goToInvoice()">
        <div class="card-action-icon">
          <svg width="30" height="30" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
            <path d="M14 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V8z"/>
            <polyline points="14 2 14 8 20 8"/>
            <line x1="12" y1="18" x2="12" y2="12"/>
            <line x1="9" y1="15" x2="15" y2="15"/>
          </svg>
        </div>
        <div class="card-action-title">Tạo hóa đơn mới</div>
        <div class="card-action-sub">Nhấn để bắt đầu</div>
      </div>

      <!-- Doanh thu tháng -->
      <div class="card">
        <div class="card-label">📊 Doanh thu tháng này</div>
        <div class="revenue-amount" id="home-revenue">0 ₫</div>
        <div class="revenue-month" id="home-revenue-month"></div>
        <div class="revenue-divider"></div>
        <div class="revenue-stats">
          <div class="revenue-stat">
            <label>Số hóa đơn</label>
            <span id="home-invoice-count">0</span>
          </div>
          <div class="revenue-stat">
            <label>Trung bình / HĐ</label>
            <span id="home-avg">0 ₫</span>
          </div>
        </div>
      </div>

      <!-- Hóa đơn gần nhất -->
      <div class="card recent-card">
        <div class="sec-title">Hóa đơn gần nhất</div>
        <div id="recent-table-wrap">
          <div class="recent-empty">
            <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M14 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V8z"/><polyline points="14 2 14 8 20 8"/></svg>
            Chưa có hóa đơn nào trong tháng này
          </div>
        </div>
      </div>

    </div>
  </main>
</div>


<!-- ════════════════════════════════════════════
     TRANG TẠO HÓA ĐƠN
════════════════════════════════════════════ -->
<div id="page-invoice" class="hidden">

  <!-- Top bar -->
  <div class="inv-topbar">
    <div class="inv-topbar-left">
      <button class="btn-back" onclick="goHome()">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><polyline points="15 18 9 12 15 6"/></svg>
        Trang chủ
      </button>
      <div class="inv-topbar-title">Tạo hóa đơn</div>
      <div class="inv-number-badge">
        <div class="num" id="inv-number-display">#HD0001</div>
        <span class="badge badge-blue" id="inv-status-badge">Đang nhập</span>
      </div>
    </div>
    <div class="inv-topbar-actions">
      <button class="btn btn-outline" onclick="resetInvoice()">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="1 4 1 10 7 10"/><path d="M3.51 15a9 9 0 102.13-9.36L1 10"/></svg>
        Tạo mới
      </button>
      <button class="btn btn-pdf" onclick="exportPDF()">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></svg>
        Xuất PDF
      </button>
    </div>
  </div>

  <!-- Layout -->
  <div class="inv-layout">

    <!-- LEFT -->
    <div class="inv-left">

      <!-- Store banner -->
      <div class="store-banner">
        <div class="store-logo-box">T</div>
        <div>
          <div class="store-name">T-Mart</div>
          <div class="store-meta">
            📍 33C, Ngõ Chợ Khâm Thiên, Văn Miếu - Quốc Tử Giám, Hà Nội<br>
            📞 Hotline: 1900-9999
          </div>
        </div>
      </div>

      <!-- Invoice info -->
      <div class="info-card">
        <div class="sec-title">Thông tin hóa đơn</div>
        <div class="info-grid">
          <div class="field-group span2">
            <div class="field-label">Nhân viên bán hàng</div>
            <input class="field-input" id="inv-staff" type="text" placeholder="Nhập tên nhân viên..." />
          </div>
          <div class="field-group">
            <div class="field-label">Ngày</div>
            <input class="field-input" id="inv-date" type="date" />
          </div>
          <div class="field-group">
            <div class="field-label">Giờ : Phút</div>
            <div style="display:flex;gap:6px">
              <input class="field-input" id="inv-hour" type="number" min="0" max="23" placeholder="HH" style="width:60px;text-align:center" />
              <input class="field-input" id="inv-min"  type="number" min="0" max="59" placeholder="MM" style="width:60px;text-align:center" />
              <input class="hidden" id="inv-sec" type="number" value="0" />
            </div>
          </div>
        </div>
      </div>

      <!-- Product search -->
      <div class="info-card">
        <div class="sec-title">Thêm sản phẩm</div>
        <div class="product-search-row">
          <div class="field-group autocomplete-wrapper">
            <div class="field-label">Mã sản phẩm</div>
            <input class="field-input" id="search-code" type="text" placeholder="SP001…"
              oninput="onCodeInput(this)" onkeydown="onCodeKey(event)" autocomplete="off" />
            <div class="autocomplete-drop hidden" id="code-drop"></div>
          </div>
          <div class="field-group autocomplete-wrapper">
            <div class="field-label">Tên sản phẩm</div>
            <input class="field-input" id="search-name" type="text" placeholder="Tìm tên hoặc danh mục…"
              oninput="onNameInput(this)" onkeydown="onNameKey(event)" autocomplete="off" />
            <div class="autocomplete-drop hidden" id="name-drop"></div>
          </div>
          <div class="field-group">
            <div class="field-label">Số lượng</div>
            <input class="field-input" id="search-qty" type="number" min="1" value="1"
              onkeydown="if(event.key==='Enter')addProduct()" />
          </div>
          <div class="field-group">
            <div class="field-label" style="opacity:0">_</div>
            <button class="btn btn-primary" onclick="addProduct()" style="height:40px;padding:0 16px">
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
              Thêm
            </button>
          </div>
        </div>
        <div id="search-preview" style="font-size:13px;color:var(--text-3);margin-top:-8px;padding-left:2px;min-height:18px"></div>
      </div>

      <!-- Invoice table -->
      <div class="table-wrap">
        <table class="inv-table">
          <thead>
            <tr>
              <th style="width:36px">#</th>
              <th>Mã SP</th>
              <th>Tên sản phẩm</th>
              <th style="text-align:right">Đơn giá</th>
              <th style="text-align:center">SL</th>
              <th style="text-align:right">Thành tiền</th>
              <th style="width:48px">Xóa</th>
            </tr>
          </thead>
          <tbody id="inv-tbody">
            <tr>
              <td colspan="7" class="table-empty">
                <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                  <path d="M6 2L3 6v14a2 2 0 002 2h14a2 2 0 002-2V6l-3-4z"/>
                  <line x1="3" y1="6" x2="21" y2="6"/>
                  <path d="M16 10a4 4 0 01-8 0"/>
                </svg>
                Chưa có sản phẩm nào. Hãy thêm sản phẩm vào hóa đơn.
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <div style="text-align:right;font-size:15px;color:var(--text-2);padding:0 4px 4px">
        Tổng sản phẩm: <strong id="subtotal-display" style="color:var(--text);font-family:var(--font-mono)">0 ₫</strong>
      </div>

    </div><!-- /inv-left -->

    <!-- RIGHT: payment -->
    <div class="payment-sidebar">

      <div class="pay-card">
        <div class="sec-title">Thanh toán</div>
        <div class="pay-row">
          <label>Tổng sản phẩm</label>
          <div class="pay-value" id="pay-subtotal">0 ₫</div>
        </div>
        <div class="pay-row">
          <label>Chiết khấu (₫)</label>
          <input class="field-input" id="pay-discount" type="number" min="0" value="0"
            style="width:130px;text-align:right;font-family:var(--font-mono)" oninput="recalcTotal()" />
        </div>
        <div class="pay-row">
          <label>Thuế VAT (%)</label>
          <input class="field-input" id="pay-vat" type="number" min="0" max="100" value="0"
            style="width:130px;text-align:right;font-family:var(--font-mono)" oninput="recalcTotal()" />
        </div>
        <div class="pay-total-row">
          <div class="pay-total-label">TỔNG CỘNG</div>
          <div class="pay-total-value" id="pay-total">0 ₫</div>
        </div>
        <div class="pay-row" style="margin-top:6px">
          <label>Khách đưa (₫)</label>
          <input class="field-input" id="pay-given" type="number" min="0" value="0"
            style="width:130px;text-align:right;font-family:var(--font-mono)" oninput="recalcChange()" />
        </div>
        <div class="pay-row" style="border-bottom:none">
          <label style="font-weight:700;color:var(--text)">Tiền thối lại</label>
          <div class="pay-change-value" id="pay-change">0 ₫</div>
        </div>
      </div>

      <div class="pay-card">
        <div class="sec-title">Hình thức thanh toán</div>
        <div class="pay-methods">
          <button class="pay-method-btn active" onclick="selectPayMethod('cash', this)">
            <span class="method-icon">💵</span>Tiền mặt
          </button>
          <button class="pay-method-btn" onclick="selectPayMethod('transfer', this)">
            <span class="method-icon">🏦</span>Chuyển khoản
          </button>
          <button class="pay-method-btn" onclick="selectPayMethod('ewallet', this)">
            <span class="method-icon">📱</span>Ví điện tử
          </button>
        </div>
      </div>

      <div class="action-group">
        <button class="btn btn-success btn-full btn-lg" onclick="checkout()">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><polyline points="20 6 9 17 4 12"/></svg>
          THANH TOÁN
        </button>
        <div class="action-group-row">
          <button class="btn btn-pdf btn-full" onclick="exportPDF()">
            <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></svg>
            Xuất PDF
          </button>
          <button class="btn btn-outline btn-full" onclick="resetInvoice()">
            <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="1 4 1 10 7 10"/><path d="M3.51 15a9 9 0 102.13-9.36L1 10"/></svg>
            Tạo mới
          </button>
        </div>
      </div>

    </div><!-- /payment-sidebar -->

  </div><!-- /inv-layout -->
</div><!-- /page-invoice -->

<!-- Toast notification -->
<div id="toast"></div>


<!-- ════════════════════════════════════════════
     JAVASCRIPT
════════════════════════════════════════════ -->
<script>
/* ══════════════════════════════════════════════
   DANH SÁCH 100 SẢN PHẨM
   Gồm 10 danh mục: Đồ chơi, Truyện, Snack,
   Bánh kẹo, Nước uống, Bút/VPP, Vở, Gia dụng,
   Thực phẩm, Phụ kiện công nghệ
══════════════════════════════════════════════ */
const PRODUCTS = [
  /* ── Đồ chơi (SP001–SP010) ─────────────── */
  { code:'SP001', name:'Gấu bông Pikachu',                price:120000, cat:'Đồ chơi'   },
  { code:'SP002', name:'Gấu bông Doraemon nhỏ',           price: 85000, cat:'Đồ chơi'   },
  { code:'SP003', name:'Xe ô tô điều khiển từ xa',        price:250000, cat:'Đồ chơi'   },
  { code:'SP004', name:'Búp bê Barbie mini',               price:135000, cat:'Đồ chơi'   },
  { code:'SP005', name:'Bộ xếp hình Lego 100 mảnh',       price:195000, cat:'Đồ chơi'   },
  { code:'SP006', name:'Súng bắn nước đồ chơi',           price: 65000, cat:'Đồ chơi'   },
  { code:'SP007', name:'Máy bay điều khiển từ xa',        price:380000, cat:'Đồ chơi'   },
  { code:'SP008', name:'Đất nặn màu 6 hộp',               price: 55000, cat:'Đồ chơi'   },
  { code:'SP009', name:'Bộ đồ chơi bác sĩ',               price: 98000, cat:'Đồ chơi'   },
  { code:'SP010', name:'Con quay Spinner phát sáng',       price: 35000, cat:'Đồ chơi'   },

  /* ── Truyện / Sách (SP011–SP020) ─────── */
  { code:'SP011', name:'Truyện Doraemon tập 1',           price: 22000, cat:'Truyện'    },
  { code:'SP012', name:'Truyện Doraemon tập 2',           price: 22000, cat:'Truyện'    },
  { code:'SP013', name:'Truyện One Piece tập 1',          price: 28000, cat:'Truyện'    },
  { code:'SP014', name:'Truyện Dragon Ball tập 1',        price: 25000, cat:'Truyện'    },
  { code:'SP015', name:'Sách tô màu cho bé',              price: 45000, cat:'Truyện'    },
  { code:'SP016', name:'Truyện Conan tập 1',              price: 26000, cat:'Truyện'    },
  { code:'SP017', name:'Sách học chữ cái cho bé',         price: 38000, cat:'Truyện'    },
  { code:'SP018', name:'Truyện Thám Tử Lừng Danh tập 1', price: 29000, cat:'Truyện'    },
  { code:'SP019', name:'Sách Thiếu Nhi Kể Chuyện cổ tích',price: 55000, cat:'Truyện'   },
  { code:'SP020', name:'Truyện Nobita và Người Khổng Lồ', price: 30000, cat:'Truyện'   },

  /* ── Snack (SP021–SP030) ──────────────── */
  { code:'SP021', name:'Snack khoai tây Poca',            price: 15000, cat:'Snack'     },
  { code:'SP022', name:'Snack bắp rang bơ Orville',       price: 12000, cat:'Snack'     },
  { code:'SP023', name:'Snack tôm cay Super',             price: 10000, cat:'Snack'     },
  { code:'SP024', name:'Snack phô mai bò cười',           price: 20000, cat:'Snack'     },
  { code:'SP025', name:'Snack mực nướng miếng',           price: 12000, cat:'Snack'     },
  { code:'SP026', name:'Snack bò khô siêu cay',           price: 15000, cat:'Snack'     },
  { code:'SP027', name:'Snack rong biển Tao Kae Noi',     price: 18000, cat:'Snack'     },
  { code:'SP028', name:'Snack khoai tây lắc lắc',         price: 15000, cat:'Snack'     },
  { code:'SP029', name:'Snack cá viên chiên',             price:  8000, cat:'Snack'     },
  { code:'SP030', name:'Snack ngô nướng phô mai',         price: 12000, cat:'Snack'     },

  /* ── Bánh kẹo (SP031–SP040) ──────────── */
  { code:'SP031', name:'Bánh quy kem Oreo gói',           price: 22000, cat:'Bánh kẹo'  },
  { code:'SP032', name:'Kẹo dẻo hình gấu Haribo',         price: 18000, cat:'Bánh kẹo'  },
  { code:'SP033', name:'Bánh chocopie Lotte hộp 12',      price: 55000, cat:'Bánh kẹo'  },
  { code:'SP034', name:'Kẹo mút Chupa Chups',             price:  5000, cat:'Bánh kẹo'  },
  { code:'SP035', name:'Kẹo cao su Orbit hộp',            price: 15000, cat:'Bánh kẹo'  },
  { code:'SP036', name:'Bánh kem xốp Malai',              price: 25000, cat:'Bánh kẹo'  },
  { code:'SP037', name:'Kẹo dừa Bến Tre gói 300g',        price: 30000, cat:'Bánh kẹo'  },
  { code:'SP038', name:'Kẹo chanh muối gói nhỏ',          price:  8000, cat:'Bánh kẹo'  },
  { code:'SP039', name:'Bánh mì que giòn phô mai',        price:  8000, cat:'Bánh kẹo'  },
  { code:'SP040', name:'Bánh gạo nướng vị nori',          price: 15000, cat:'Bánh kẹo'  },

  /* ── Nước uống (SP041–SP050) ──────────── */
  { code:'SP041', name:'Nước ngọt Coca-Cola 330ml',       price: 12000, cat:'Nước uống' },
  { code:'SP042', name:'Nước ngọt Pepsi 330ml',           price: 12000, cat:'Nước uống' },
  { code:'SP043', name:'Nước tăng lực Sting 330ml',       price: 10000, cat:'Nước uống' },
  { code:'SP044', name:'Trà xanh C2 330ml',               price:  8000, cat:'Nước uống' },
  { code:'SP045', name:'Nước suối Aquafina 500ml',        price:  7000, cat:'Nước uống' },
  { code:'SP046', name:'Sữa tươi Vinamilk 180ml',         price:  8000, cat:'Nước uống' },
  { code:'SP047', name:'Nước cam ép Tipco 330ml',         price: 18000, cat:'Nước uống' },
  { code:'SP048', name:'Trà sữa Milo hộp 200ml',         price: 12000, cat:'Nước uống' },
  { code:'SP049', name:'Nước dừa tươi đóng lon 330ml',   price: 15000, cat:'Nước uống' },
  { code:'SP050', name:'Nước hoa quả Twister 1 lít',     price: 28000, cat:'Nước uống' },

  /* ── Bút / Văn phòng phẩm (SP051–SP065) */
  { code:'SP051', name:'Bút bi Thiên Long 0.7mm xanh',   price:  4000, cat:'VPP'       },
  { code:'SP052', name:'Bút bi Flexoffice đỏ',           price:  3500, cat:'VPP'       },
  { code:'SP053', name:'Bút chì 2B Hồng Hà',            price:  3000, cat:'VPP'       },
  { code:'SP054', name:'Bút xóa Pentel ZL31',            price: 12000, cat:'VPP'       },
  { code:'SP055', name:'Bút highlight 4 màu Stabilo',    price: 22000, cat:'VPP'       },
  { code:'SP056', name:'Bút lông dạ bảng đen',           price:  8000, cat:'VPP'       },
  { code:'SP057', name:'Bút gel 0.5mm mực đen',          price:  6000, cat:'VPP'       },
  { code:'SP058', name:'Thước kẻ nhựa 30cm',             price:  8000, cat:'VPP'       },
  { code:'SP059', name:'Kéo văn phòng nhỏ 15cm',        price: 15000, cat:'VPP'       },
  { code:'SP060', name:'Băng keo trong 2.5cm',           price:  8000, cat:'VPP'       },
  { code:'SP061', name:'Hồ dán UHU 21g',                 price: 12000, cat:'VPP'       },
  { code:'SP062', name:'Ghim kẹp giấy 50 cái hộp',      price: 10000, cat:'VPP'       },
  { code:'SP063', name:'Giấy note màu 76x76mm 100 tờ',  price: 18000, cat:'VPP'       },
  { code:'SP064', name:'Cặp tài liệu A4 nhựa',          price: 25000, cat:'VPP'       },
  { code:'SP065', name:'Compa vẽ kỹ thuật inox',        price: 35000, cat:'VPP'       },

  /* ── Vở (SP066–SP070) ──────────────────── */
  { code:'SP066', name:'Vở kẻ ngang 96 trang Hồng Hà',  price: 12000, cat:'Vở'        },
  { code:'SP067', name:'Vở toán 5 ô ly 100 trang',      price: 15000, cat:'Vở'        },
  { code:'SP068', name:'Tập vẽ trắng A4 50 tờ',         price: 18000, cat:'Vở'        },
  { code:'SP069', name:'Sổ tay bìa cứng A5 100 trang',  price: 35000, cat:'Vở'        },
  { code:'SP070', name:'Sổ nhật ký bìa da A6',          price: 45000, cat:'Vở'        },

  /* ── Đồ gia dụng (SP071–SP083) ───────── */
  { code:'SP071', name:'Cốc uống nước thủy tinh 300ml', price: 25000, cat:'Gia dụng'  },
  { code:'SP072', name:'Bình nước giữ nhiệt 500ml',     price:120000, cat:'Gia dụng'  },
  { code:'SP073', name:'Túi zip đựng thực phẩm 30 cái',price: 15000, cat:'Gia dụng'  },
  { code:'SP074', name:'Giẻ lau bếp siêu thấm 5 cái',  price: 22000, cat:'Gia dụng'  },
  { code:'SP075', name:'Hộp đựng cơm nhựa PP 650ml',   price: 45000, cat:'Gia dụng'  },
  { code:'SP076', name:'Khăn giấy ướt Wet Ones 80 tờ', price: 25000, cat:'Gia dụng'  },
  { code:'SP077', name:'Móc treo tường inox dán kính', price: 18000, cat:'Gia dụng'  },
  { code:'SP078', name:'Bàn chải đánh răng trẻ em',     price: 15000, cat:'Gia dụng'  },
  { code:'SP079', name:'Nước rửa tay khô 250ml',        price: 28000, cat:'Gia dụng'  },
  { code:'SP080', name:'Xà bông Lifebuoy 100g',         price: 20000, cat:'Gia dụng'  },
  { code:'SP081', name:'Kem đánh răng Colgate 75g',     price: 25000, cat:'Gia dụng'  },
  { code:'SP082', name:'Túi đựng rác đen 50x65cm 10c', price: 18000, cat:'Gia dụng'  },
  { code:'SP083', name:'Pin AA Panasonic vỉ 2 viên',    price: 22000, cat:'Gia dụng'  },

  /* ── Thực phẩm (SP084–SP091) ──────────── */
  { code:'SP084', name:'Mì tôm Hảo Hảo vị tôm chua cay',price:  5000, cat:'Thực phẩm'},
  { code:'SP085', name:'Mì tôm Omachi bò hầm khoai tây', price:  8000, cat:'Thực phẩm'},
  { code:'SP086', name:'Mì ly Nissin Seafood 75g',        price: 18000, cat:'Thực phẩm'},
  { code:'SP087', name:'Cháo ăn liền vị thịt heo',       price: 12000, cat:'Thực phẩm'},
  { code:'SP088', name:'Ruốc thịt heo lọ 80g',           price: 45000, cat:'Thực phẩm'},
  { code:'SP089', name:'Cá hộp Three Ladies 155g',       price: 32000, cat:'Thực phẩm'},
  { code:'SP090', name:'Sốt cà chua Heinz gói nhỏ',      price: 25000, cat:'Thực phẩm'},
  { code:'SP091', name:'Đường cát trắng Biên Hòa 1kg',   price: 28000, cat:'Thực phẩm'},

  /* ── Phụ kiện công nghệ (SP092–SP100) ─── */
  { code:'SP092', name:'Cáp sạc USB-C 1m 3A',            price: 55000, cat:'Phụ kiện' },
  { code:'SP093', name:'Tai nghe nhét tai có dây 3.5mm',  price: 85000, cat:'Phụ kiện' },
  { code:'SP094', name:'Ốp lưng điện thoại silicon',     price: 45000, cat:'Phụ kiện' },
  { code:'SP095', name:'Cường lực màn hình 9H full keo',  price: 35000, cat:'Phụ kiện' },
  { code:'SP096', name:'Củ sạc nhanh 20W USB-C PD',      price:150000, cat:'Phụ kiện' },
  { code:'SP097', name:'Giá đỡ điện thoại để bàn',       price: 65000, cat:'Phụ kiện' },
  { code:'SP098', name:'USB Flash Drive 32GB Sandisk',    price:145000, cat:'Phụ kiện' },
  { code:'SP099', name:'Hub USB 3.0 4 cổng',             price: 95000, cat:'Phụ kiện' },
  { code:'SP100', name:'Máy tính Casio FX-570VN PLUS',   price:280000, cat:'Phụ kiện' },
// ══════════════════════════════════════════════
// 100 SẢN PHẨM THỰC PHẨM BỔ SUNG (SP101–SP200)
// ══════════════════════════════════════════════

// ── Mì / Cháo / Phở ăn liền (SP101–SP115) ────
{ code:'SP101', name:'Mì tôm Kokomi vị tôm',              price:  4500, cat:'Thực phẩm' },
{ code:'SP102', name:'Mì tôm 3 Miền vị bò gầu',           price:  5500, cat:'Thực phẩm' },
{ code:'SP103', name:'Mì Đệ Nhất hương vị gà',            price:  6000, cat:'Thực phẩm' },
{ code:'SP104', name:'Phở bò ăn liền Vifon gói',          price:  7000, cat:'Thực phẩm' },
{ code:'SP105', name:'Bún bò Huế ăn liền Vifon',          price:  7000, cat:'Thực phẩm' },
{ code:'SP106', name:'Cháo trắng ăn liền Vifon gói',      price:  5000, cat:'Thực phẩm' },
{ code:'SP107', name:'Cháo bào ngư ăn liền',              price: 12000, cat:'Thực phẩm' },
{ code:'SP108', name:'Miến gà ăn liền Minh Châu',         price:  6000, cat:'Thực phẩm' },
{ code:'SP109', name:'Hủ tiếu Nam Vang ăn liền',          price:  7500, cat:'Thực phẩm' },
{ code:'SP110', name:'Mì Quảng ăn liền vị cá lóc',        price:  8000, cat:'Thực phẩm' },
{ code:'SP111', name:'Mì ly Hảo Hảo tôm chua cay',        price: 12000, cat:'Thực phẩm' },
{ code:'SP112', name:'Phở ly Vifon bò viên',              price: 15000, cat:'Thực phẩm' },
{ code:'SP113', name:'Bún riêu cua ăn liền gói',          price:  7000, cat:'Thực phẩm' },
{ code:'SP114', name:'Súp cua ăn liền Vifon gói',         price:  5500, cat:'Thực phẩm' },
{ code:'SP115', name:'Mì Omachi xốt spaghetti',           price:  9000, cat:'Thực phẩm' },

// ── Đồ hộp / Đóng gói (SP116–SP128) ──────────
{ code:'SP116', name:'Cá thu hộp Hạ Long 155g',           price: 28000, cat:'Thực phẩm' },
{ code:'SP117', name:'Cá mòi hộp Vàng Dương 155g',        price: 22000, cat:'Thực phẩm' },
{ code:'SP118', name:'Thịt hộp Vissan 150g',              price: 35000, cat:'Thực phẩm' },
{ code:'SP119', name:'Pate gan hộp Hạ Long 100g',         price: 20000, cat:'Thực phẩm' },
{ code:'SP120', name:'Xúc xích Vissan tiệt trùng 1 cái',  price:  8000, cat:'Thực phẩm' },
{ code:'SP121', name:'Xúc xích CP vị bò 1 cái',           price:  9000, cat:'Thực phẩm' },
{ code:'SP122', name:'Chả lụa Vissan gói 500g',           price: 55000, cat:'Thực phẩm' },
{ code:'SP123', name:'Giò thủ Hà Nội 200g',               price: 35000, cat:'Thực phẩm' },
{ code:'SP124', name:'Bò kho hộp Ba Huân 180g',           price: 38000, cat:'Thực phẩm' },
{ code:'SP125', name:'Đậu phụ hộp Bách Việt 300g',        price: 18000, cat:'Thực phẩm' },
{ code:'SP126', name:'Ngô ngọt đóng hộp 425g',            price: 22000, cat:'Thực phẩm' },
{ code:'SP127', name:'Nấm rơm hộp 230g',                  price: 20000, cat:'Thực phẩm' },
{ code:'SP128', name:'Đậu Hà Lan đóng hộp 400g',          price: 25000, cat:'Thực phẩm' },

// ── Gia vị / Nước chấm (SP129–SP142) ─────────
{ code:'SP129', name:'Nước mắm Phú Quốc chai 500ml',      price: 38000, cat:'Thực phẩm' },
{ code:'SP130', name:'Nước tương Maggi chai 200ml',        price: 18000, cat:'Thực phẩm' },
{ code:'SP131', name:'Tương ớt Chinsu chai 250g',          price: 20000, cat:'Thực phẩm' },
{ code:'SP132', name:'Tương cà Heinz chai 300g',           price: 32000, cat:'Thực phẩm' },
{ code:'SP133', name:'Dầu hào Lee Kum Kee 100ml',         price: 22000, cat:'Thực phẩm' },
{ code:'SP134', name:'Nước dừa Koki dùng nấu ăn 400ml',  price: 18000, cat:'Thực phẩm' },
{ code:'SP135', name:'Bột nêm Knorr gói 400g',            price: 45000, cat:'Thực phẩm' },
{ code:'SP136', name:'Hạt nêm Maggi gói 200g',            price: 28000, cat:'Thực phẩm' },
{ code:'SP137', name:'Mì chính Vedan gói 200g',           price: 15000, cat:'Thực phẩm' },
{ code:'SP138', name:'Bột ngọt Ajinomoto gói 100g',       price: 12000, cat:'Thực phẩm' },
{ code:'SP139', name:'Tiêu xay Cholimex gói 50g',         price: 15000, cat:'Thực phẩm' },
{ code:'SP140', name:'Bột cà ri Shan gói 50g',            price: 12000, cat:'Thực phẩm' },
{ code:'SP141', name:'Tỏi băm đóng hũ 200g',              price: 22000, cat:'Thực phẩm' },
{ code:'SP142', name:'Sả băm đóng hũ 200g',               price: 18000, cat:'Thực phẩm' },

// ── Dầu ăn / Bơ / Sữa đặc (SP143–SP152) ─────
{ code:'SP143', name:'Dầu ăn Simply chai 1 lít',          price: 55000, cat:'Thực phẩm' },
{ code:'SP144', name:'Dầu ô liu Borges chai 250ml',        price: 85000, cat:'Thực phẩm' },
{ code:'SP145', name:'Bơ lạt Anchor hộp 200g',            price: 48000, cat:'Thực phẩm' },
{ code:'SP146', name:'Margarine Tường An hộp 200g',       price: 28000, cat:'Thực phẩm' },
{ code:'SP147', name:'Sữa đặc Ông Thọ lon 380g',          price: 22000, cat:'Thực phẩm' },
{ code:'SP148', name:'Sữa đặc Ngôi Sao Phương Nam 380g',  price: 20000, cat:'Thực phẩm' },
{ code:'SP149', name:'Sữa bột Cô Gái Hà Lan 900g',        price:195000, cat:'Thực phẩm' },
{ code:'SP150', name:'Sữa tươi TH True Milk 1 lít',       price: 32000, cat:'Thực phẩm' },
{ code:'SP151', name:'Sữa chua Vinamilk hộp 100g',        price:  7000, cat:'Thực phẩm' },
{ code:'SP152', name:'Phô mai con bò cười hộp 8 miếng',   price: 35000, cat:'Thực phẩm' },

// ── Gạo / Bột / Ngũ cốc (SP153–SP162) ───────
{ code:'SP153', name:'Gạo ST25 túi 2kg',                  price: 65000, cat:'Thực phẩm' },
{ code:'SP154', name:'Gạo tẻ thường túi 5kg',             price: 95000, cat:'Thực phẩm' },
{ code:'SP155', name:'Bột mì số 8 Bình An túi 1kg',       price: 22000, cat:'Thực phẩm' },
{ code:'SP156', name:'Bột chiên xù Mikko gói 150g',       price: 18000, cat:'Thực phẩm' },
{ code:'SP157', name:'Bột năng túi 400g',                 price: 15000, cat:'Thực phẩm' },
{ code:'SP158', name:'Yến mạch Quaker gói 800g',          price: 85000, cat:'Thực phẩm' },
{ code:'SP159', name:'Ngũ cốc granola hộp 500g',          price: 65000, cat:'Thực phẩm' },
{ code:'SP160', name:'Bánh mì sandwich loaf Kinh Đô',     price: 22000, cat:'Thực phẩm' },
{ code:'SP161', name:'Bánh mì gối trắng Harvest',         price: 18000, cat:'Thực phẩm' },
{ code:'SP162', name:'Nui ống Acecook 500g',              price: 28000, cat:'Thực phẩm' },

// ── Đường / Muối / Đồ khô (SP163–SP170) ─────
{ code:'SP163', name:'Đường phèn vàng túi 500g',          price: 25000, cat:'Thực phẩm' },
{ code:'SP164', name:'Muối hầm i-ốt túi 500g',            price:  8000, cat:'Thực phẩm' },
{ code:'SP165', name:'Giấm gạo Aji-moto chai 300ml',      price: 18000, cat:'Thực phẩm' },
{ code:'SP166', name:'Mật ong rừng Tây Nguyên 250ml',     price: 75000, cat:'Thực phẩm' },
{ code:'SP167', name:'Tương đen Kikkoman 150ml',          price: 28000, cat:'Thực phẩm' },
{ code:'SP168', name:'Bột canh Hải Châu gói 200g',        price: 12000, cat:'Thực phẩm' },
{ code:'SP169', name:'Nấm hương khô túi 50g',             price: 28000, cat:'Thực phẩm' },
{ code:'SP170', name:'Mộc nhĩ khô túi 100g',              price: 22000, cat:'Thực phẩm' },

// ── Trà / Cà phê / Nước (SP171–SP183) ────────
{ code:'SP171', name:'Trà xanh O Long túi lọc hộp 25 túi',price: 35000, cat:'Thực phẩm' },
{ code:'SP172', name:'Trà atiso túi lọc hộp 20 túi',      price: 28000, cat:'Thực phẩm' },
{ code:'SP173', name:'Trà gừng mật ong túi lọc 20 túi',   price: 32000, cat:'Thực phẩm' },
{ code:'SP174', name:'Cà phê G7 3in1 hộp 20 gói',         price: 65000, cat:'Thực phẩm' },
{ code:'SP175', name:'Cà phê Nescafé đen đá gói 25g',     price:  5000, cat:'Thực phẩm' },
{ code:'SP176', name:'Cà phê Highlands bột túi 200g',     price: 75000, cat:'Thực phẩm' },
{ code:'SP177', name:'Cacao Van Houten bột hộp 200g',     price: 65000, cat:'Thực phẩm' },
{ code:'SP178', name:'Sữa hạnh nhân Almond Breeze 1 lít', price: 55000, cat:'Thực phẩm' },
{ code:'SP179', name:'Nước ép táo Welchs chai 300ml',     price: 22000, cat:'Thực phẩm' },
{ code:'SP180', name:'Soda chanh Twister 500ml',           price: 18000, cat:'Thực phẩm' },
{ code:'SP181', name:'Nước khoáng Lavie 1.5 lít',         price: 10000, cat:'Thực phẩm' },
{ code:'SP182', name:'Trà lipton túi lọc hộp 25 túi',     price: 32000, cat:'Thực phẩm' },
{ code:'SP183', name:'Bột protein socola shake 500g',     price:195000, cat:'Thực phẩm' },

// ── Đồ ăn vặt / Đóng gói sẵn (SP184–SP195) ──
{ code:'SP184', name:'Khô bò miếng Vina gói 100g',        price: 55000, cat:'Thực phẩm' },
{ code:'SP185', name:'Khô gà lá chanh gói 80g',           price: 45000, cat:'Thực phẩm' },
{ code:'SP186', name:'Nem chua thanh Huế gói 5 cái',      price: 15000, cat:'Thực phẩm' },
{ code:'SP187', name:'Trứng cút luộc đóng gói 10 quả',   price: 18000, cat:'Thực phẩm' },
{ code:'SP188', name:'Xúc xích cocktail Hà Nội 200g',     price: 35000, cat:'Thực phẩm' },
{ code:'SP189', name:'Đậu phộng rang muối túi 200g',      price: 22000, cat:'Thực phẩm' },
{ code:'SP190', name:'Hạt điều rang muối túi 100g',       price: 45000, cat:'Thực phẩm' },
{ code:'SP191', name:'Hạt hướng dương rang muối 200g',    price: 28000, cat:'Thực phẩm' },
{ code:'SP192', name:'Mứt dâu tây Hải Hà hũ 200g',       price: 35000, cat:'Thực phẩm' },
{ code:'SP193', name:'Mứt việt quất Smuckers hũ 340g',    price: 65000, cat:'Thực phẩm' },
{ code:'SP194', name:'Bơ đậu phộng Jiffy hũ 340g',        price: 75000, cat:'Thực phẩm' },
{ code:'SP195', name:'Bánh tráng nướng tây ninh gói',     price: 15000, cat:'Thực phẩm' },

// ── Rau củ / Trái cây sấy (SP196–SP200) ──────
{ code:'SP196', name:'Chuối sấy dẻo túi 200g',            price: 35000, cat:'Thực phẩm' },
{ code:'SP197', name:'Xoài sấy dẻo túi 150g',             price: 38000, cat:'Thực phẩm' },
{ code:'SP198', name:'Mít sấy giòn túi 100g',             price: 32000, cat:'Thực phẩm' },
{ code:'SP199', name:'Khoai lang sấy dẻo túi 200g',       price: 28000, cat:'Thực phẩm' },
{ code:'SP200', name:'Thập cẩm trái cây sấy túi 250g',    price: 42000, cat:'Thực phẩm' },
// ══════════════════════════════════════════════
// 100 SẢN PHẨM TRUYỆN TRANH / SÁCH VỞ (SP201–SP300)
// ══════════════════════════════════════════════

// ── Truyện tranh Nhật (SP201–SP225) ──────────
{ code:'SP201', name:'Truyện Naruto tập 1',                price: 28000, cat:'Truyện tranh' },
{ code:'SP202', name:'Truyện Naruto tập 2',                price: 28000, cat:'Truyện tranh' },
{ code:'SP203', name:'Truyện Naruto tập 3',                price: 28000, cat:'Truyện tranh' },
{ code:'SP204', name:'Truyện Bleach tập 1',                price: 26000, cat:'Truyện tranh' },
{ code:'SP205', name:'Truyện Bleach tập 2',                price: 26000, cat:'Truyện tranh' },
{ code:'SP206', name:'Truyện Attack on Titan tập 1',       price: 32000, cat:'Truyện tranh' },
{ code:'SP207', name:'Truyện Attack on Titan tập 2',       price: 32000, cat:'Truyện tranh' },
{ code:'SP208', name:'Truyện Demon Slayer tập 1',          price: 30000, cat:'Truyện tranh' },
{ code:'SP209', name:'Truyện Demon Slayer tập 2',          price: 30000, cat:'Truyện tranh' },
{ code:'SP210', name:'Truyện My Hero Academia tập 1',      price: 30000, cat:'Truyện tranh' },
{ code:'SP211', name:'Truyện My Hero Academia tập 2',      price: 30000, cat:'Truyện tranh' },
{ code:'SP212', name:'Truyện Fairy Tail tập 1',            price: 28000, cat:'Truyện tranh' },
{ code:'SP213', name:'Truyện Fairy Tail tập 2',            price: 28000, cat:'Truyện tranh' },
{ code:'SP214', name:'Truyện Sword Art Online tập 1',      price: 35000, cat:'Truyện tranh' },
{ code:'SP215', name:'Truyện Tokyo Ghoul tập 1',           price: 32000, cat:'Truyện tranh' },
{ code:'SP216', name:'Truyện Tokyo Ghoul tập 2',           price: 32000, cat:'Truyện tranh' },
{ code:'SP217', name:'Truyện Death Note tập 1',            price: 30000, cat:'Truyện tranh' },
{ code:'SP218', name:'Truyện Death Note tập 2',            price: 30000, cat:'Truyện tranh' },
{ code:'SP219', name:'Truyện Fullmetal Alchemist tập 1',   price: 28000, cat:'Truyện tranh' },
{ code:'SP220', name:'Truyện Hunter x Hunter tập 1',       price: 28000, cat:'Truyện tranh' },
{ code:'SP221', name:'Truyện Black Clover tập 1',          price: 29000, cat:'Truyện tranh' },
{ code:'SP222', name:'Truyện Blue Exorcist tập 1',         price: 28000, cat:'Truyện tranh' },
{ code:'SP223', name:'Truyện Haikyuu tập 1',               price: 28000, cat:'Truyện tranh' },
{ code:'SP224', name:'Truyện Jujutsu Kaisen tập 1',        price: 32000, cat:'Truyện tranh' },
{ code:'SP225', name:'Truyện Jujutsu Kaisen tập 2',        price: 32000, cat:'Truyện tranh' },

// ── Truyện tranh thiếu nhi / Việt (SP226–SP240) ─
{ code:'SP226', name:'Truyện Thần Đồng Đất Việt tập 1',   price: 22000, cat:'Truyện tranh' },
{ code:'SP227', name:'Truyện Thần Đồng Đất Việt tập 2',   price: 22000, cat:'Truyện tranh' },
{ code:'SP228', name:'Truyện Thần Đồng Đất Việt tập 3',   price: 22000, cat:'Truyện tranh' },
{ code:'SP229', name:'Truyện Dũng Sĩ Hesman tập 1',       price: 20000, cat:'Truyện tranh' },
{ code:'SP230', name:'Truyện Bảy Viên Ngọc Rồng tập 1',   price: 25000, cat:'Truyện tranh' },
{ code:'SP231', name:'Truyện Bảy Viên Ngọc Rồng tập 2',   price: 25000, cat:'Truyện tranh' },
{ code:'SP232', name:'Truyện Robotech tập 1',              price: 20000, cat:'Truyện tranh' },
{ code:'SP233', name:'Truyện Pokémon tập 1',               price: 25000, cat:'Truyện tranh' },
{ code:'SP234', name:'Truyện Pokémon tập 2',               price: 25000, cat:'Truyện tranh' },
{ code:'SP235', name:'Truyện Doraemon Plus tập 1',         price: 24000, cat:'Truyện tranh' },
{ code:'SP236', name:'Truyện Doraemon Plus tập 2',         price: 24000, cat:'Truyện tranh' },
{ code:'SP237', name:'Truyện Shin – Cậu bé bút chì tập 1',price: 22000, cat:'Truyện tranh' },
{ code:'SP238', name:'Truyện Shin – Cậu bé bút chì tập 2',price: 22000, cat:'Truyện tranh' },
{ code:'SP239', name:'Truyện Candy Candy tập 1',           price: 24000, cat:'Truyện tranh' },
{ code:'SP240', name:'Truyện Minions – Những kẻ quậy phá',price: 28000, cat:'Truyện tranh' },

// ── Sách kỹ năng sống / Self-help (SP241–SP255) ─
{ code:'SP241', name:'Sách Đắc Nhân Tâm – Dale Carnegie',  price: 68000, cat:'Sách'        },
{ code:'SP242', name:'Sách Nghĩ Giàu Làm Giàu – Napoleon', price: 72000, cat:'Sách'        },
{ code:'SP243', name:'Sách Tuổi Trẻ Đáng Giá Bao Nhiêu',  price: 75000, cat:'Sách'        },
{ code:'SP244', name:'Sách Nhà Giả Kim – Paulo Coelho',    price: 65000, cat:'Sách'        },
{ code:'SP245', name:'Sách Atomic Habits – James Clear',   price: 85000, cat:'Sách'        },
{ code:'SP246', name:'Sách Tư Duy Nhanh và Chậm',         price: 95000, cat:'Sách'        },
{ code:'SP247', name:'Sách Người Giàu Có Nhất Thành Babylon',price:55000, cat:'Sách'       },
{ code:'SP248', name:'Sách Bí Mật – The Secret',          price: 68000, cat:'Sách'        },
{ code:'SP249', name:'Sách Khéo Ăn Nói Sẽ Có Được TG',   price: 62000, cat:'Sách'        },
{ code:'SP250', name:'Sách Muôn Kiếp Nhân Sinh tập 1',    price: 88000, cat:'Sách'        },
{ code:'SP251', name:'Sách Muôn Kiếp Nhân Sinh tập 2',    price: 88000, cat:'Sách'        },
{ code:'SP252', name:'Sách Không Bao Giờ Là Thất Bại',    price: 58000, cat:'Sách'        },
{ code:'SP253', name:'Sách Ikigai – Đi Tìm Lý Do Sống',  price: 72000, cat:'Sách'        },
{ code:'SP254', name:'Sách Dám Bị Ghét – Kishimi Ichiro', price: 78000, cat:'Sách'        },
{ code:'SP255', name:'Sách Sức Mạnh Của Thói Quen',       price: 75000, cat:'Sách'        },

// ── Sách văn học / Tiểu thuyết (SP256–SP268) ──
{ code:'SP256', name:'Sách Tôi Thấy Hoa Vàng Trên Cỏ Xanh',price:72000, cat:'Sách'       },
{ code:'SP257', name:'Sách Mắt Biếc – Nguyễn Nhật Ánh',  price: 68000, cat:'Sách'        },
{ code:'SP258', name:'Sách Cho Tôi Xin Một Vé Đi Tuổi Thơ',price:62000, cat:'Sách'       },
{ code:'SP259', name:'Sách Hoàng Tử Bé – Antoine de Saint',price:55000, cat:'Sách'        },
{ code:'SP260', name:'Sách Bố Già – Mario Puzo',          price: 88000, cat:'Sách'        },
{ code:'SP261', name:'Sách Harry Potter tập 1',            price:120000, cat:'Sách'        },
{ code:'SP262', name:'Sách Harry Potter tập 2',            price:120000, cat:'Sách'        },
{ code:'SP263', name:'Sách Sherlock Holmes tuyển tập',     price: 95000, cat:'Sách'        },
{ code:'SP264', name:'Sách Số Đỏ – Vũ Trọng Phụng',       price: 58000, cat:'Sách'        },
{ code:'SP265', name:'Sách Chí Phèo và các truyện ngắn',  price: 48000, cat:'Sách'        },
{ code:'SP266', name:'Sách Dế Mèn Phiêu Lưu Ký',          price: 52000, cat:'Sách'        },
{ code:'SP267', name:'Sách Tắt Đèn – Ngô Tất Tố',         price: 48000, cat:'Sách'        },
{ code:'SP268', name:'Sách Truyện Kiều – Nguyễn Du',       price: 45000, cat:'Sách'        },

// ── Sách học tiếng Anh (SP269–SP278) ─────────
{ code:'SP269', name:'Sách Grammar in Use Intermediate',   price:145000, cat:'Sách'        },
{ code:'SP270', name:'Sách Oxford Word Skills Basic',      price:125000, cat:'Sách'        },
{ code:'SP271', name:'Sách Tiếng Anh 3000 từ vựng',       price: 65000, cat:'Sách'        },
{ code:'SP272', name:'Sách IELTS Cambridge 17',            price:185000, cat:'Sách'        },
{ code:'SP273', name:'Sách TOEIC 1000 bài tập',           price: 95000, cat:'Sách'        },
{ code:'SP274', name:'Sách Hack Não 1500 từ IELTS',        price: 78000, cat:'Sách'        },
{ code:'SP275', name:'Sách Phát Âm Chuẩn Tiếng Anh',      price: 68000, cat:'Sách'        },
{ code:'SP276', name:'Sách Anh Văn Giao Tiếp Hàng Ngày',  price: 72000, cat:'Sách'        },
{ code:'SP277', name:'Sách English Collocations in Use',   price:135000, cat:'Sách'        },
{ code:'SP278', name:'Sách Longman Pocket Dictionary',     price:115000, cat:'Sách'        },

// ── Sách giáo khoa / Tham khảo (SP279–SP288) ──
{ code:'SP279', name:'Sách Toán 6 – Bộ Cánh Diều',        price: 28000, cat:'Sách GK'     },
{ code:'SP280', name:'Sách Toán 7 – Bộ Cánh Diều',        price: 28000, cat:'Sách GK'     },
{ code:'SP281', name:'Sách Ngữ Văn 6 – Kết Nối Tri Thức', price: 26000, cat:'Sách GK'     },
{ code:'SP282', name:'Sách Khoa Học Tự Nhiên 6',           price: 26000, cat:'Sách GK'     },
{ code:'SP283', name:'Sách Lịch Sử và Địa Lý 6',          price: 26000, cat:'Sách GK'     },
{ code:'SP284', name:'Sách Toán Nâng Cao lớp 5',          price: 35000, cat:'Sách GK'     },
{ code:'SP285', name:'Sách Bài Tập Toán Lớp 4',           price: 22000, cat:'Sách GK'     },
{ code:'SP286', name:'Sách Ôn Thi Vào Lớp 10 Toán',       price: 55000, cat:'Sách GK'     },
{ code:'SP287', name:'Sách Ôn Thi THPT Quốc Gia Vật Lý',  price: 65000, cat:'Sách GK'     },
{ code:'SP288', name:'Sách Ôn Thi THPT Quốc Gia Hóa Học', price: 65000, cat:'Sách GK'     },

// ── Vở học sinh / Tập viết (SP289–SP300) ──────
{ code:'SP289', name:'Vở ô ly 4 dòng kẻ bé tập viết 48tr', price:  8000, cat:'Vở'         },
{ code:'SP290', name:'Vở kẻ ngang 48 trang lớp 1',         price:  7000, cat:'Vở'         },
{ code:'SP291', name:'Vở kẻ ngang 80 trang Ánh Dương',     price: 10000, cat:'Vở'         },
{ code:'SP292', name:'Vở toán ly 5x5 80 trang',            price: 10000, cat:'Vở'         },
{ code:'SP293', name:'Vở nhạc 5 dòng kẻ 32 trang',        price:  8000, cat:'Vở'         },
{ code:'SP294', name:'Bộ 10 cuốn vở kẻ ngang 96 trang',   price: 95000, cat:'Vở'         },
{ code:'SP295', name:'Vở luyện chữ đẹp lớp 2',            price: 12000, cat:'Vở'         },
{ code:'SP296', name:'Sổ tay học từ vựng A6 200 trang',    price: 28000, cat:'Vở'         },
{ code:'SP297', name:'Tập bìa nhựa Oxford A4 200 trang',   price: 38000, cat:'Vở'         },
{ code:'SP298', name:'Sổ còng bìa cứng A5 – dotted',       price: 45000, cat:'Vở'         },
{ code:'SP299', name:'Sổ nhật ký 365 ngày bìa vải',       price: 55000, cat:'Vở'         },
{ code:'SP300', name:'Bộ flashcard từ vựng tiếng Anh 200 thẻ',price:48000, cat:'Vở'      },
// ══════════════════════════════════════════════
// 100 SẢN PHẨM ĐỒ CHƠI (SP301–SP400)
// ══════════════════════════════════════════════

// ── Gấu bông / Thú nhồi bông (SP301–SP315) ───
{ code:'SP301', name:'Gấu bông Teddy Bear 40cm nâu',       price:120000, cat:'Đồ chơi' },
{ code:'SP302', name:'Gấu bông Teddy Bear 60cm trắng',     price:195000, cat:'Đồ chơi' },
{ code:'SP303', name:'Thú bông Mèo Hello Kitty 25cm',      price: 85000, cat:'Đồ chơi' },
{ code:'SP304', name:'Thú bông Thỏ tai dài 30cm hồng',     price: 95000, cat:'Đồ chơi' },
{ code:'SP305', name:'Thú bông Heo Peppa Pig 20cm',        price: 75000, cat:'Đồ chơi' },
{ code:'SP306', name:'Thú bông Khủng long bạo chúa 35cm',  price:135000, cat:'Đồ chơi' },
{ code:'SP307', name:'Gấu bông Doraemon đứng 30cm',        price:110000, cat:'Đồ chơi' },
{ code:'SP308', name:'Thú bông Stitch 25cm xanh',          price: 98000, cat:'Đồ chơi' },
{ code:'SP309', name:'Thú bông Minion 20cm vàng',          price: 88000, cat:'Đồ chơi' },
{ code:'SP310', name:'Thú bông Gấu Pooh 25cm vàng',        price: 90000, cat:'Đồ chơi' },
{ code:'SP311', name:'Thú bông Cá Heo 40cm xám',           price:125000, cat:'Đồ chơi' },
{ code:'SP312', name:'Thú bông Unicorn cầu vồng 35cm',     price:130000, cat:'Đồ chơi' },
{ code:'SP313', name:'Gối ôm Mèo Nằm 50cm xám',           price:145000, cat:'Đồ chơi' },
{ code:'SP314', name:'Thú bông Gấu Trúc 30cm đen trắng',  price:115000, cat:'Đồ chơi' },
{ code:'SP315', name:'Thú bông Chó Shiba Inu 25cm',        price: 95000, cat:'Đồ chơi' },

// ── Xe đồ chơi (SP316–SP330) ──────────────────
{ code:'SP316', name:'Xe ô tô đồ chơi chạy pin cỡ lớn',   price:185000, cat:'Đồ chơi' },
{ code:'SP317', name:'Xe tải chở hàng đồ chơi bằng sắt',  price: 75000, cat:'Đồ chơi' },
{ code:'SP318', name:'Xe cứu thương có đèn và âm thanh',   price: 95000, cat:'Đồ chơi' },
{ code:'SP319', name:'Xe cứu hỏa đồ chơi cỡ lớn',         price:125000, cat:'Đồ chơi' },
{ code:'SP320', name:'Xe máy bay đồ chơi điều khiển',      price:220000, cat:'Đồ chơi' },
{ code:'SP321', name:'Xe địa hình RC 4WD mini',            price:285000, cat:'Đồ chơi' },
{ code:'SP322', name:'Tàu hỏa chạy ray đồ chơi bộ 28 mảnh',price:195000,cat:'Đồ chơi' },
{ code:'SP323', name:'Xe ben đổ đất đồ chơi cỡ vừa',      price: 68000, cat:'Đồ chơi' },
{ code:'SP324', name:'Xe máy đồ chơi chạy pin',           price: 55000, cat:'Đồ chơi' },
{ code:'SP325', name:'Bộ đường đua xe Hot Wheels 30 mảnh', price:245000, cat:'Đồ chơi' },
{ code:'SP326', name:'Xe công trình đào đất đồ chơi',      price: 88000, cat:'Đồ chơi' },
{ code:'SP327', name:'Xe tank bắn đạn nước đồ chơi',       price: 72000, cat:'Đồ chơi' },
{ code:'SP328', name:'Thuyền điều khiển từ xa chạy nước',  price:320000, cat:'Đồ chơi' },
{ code:'SP329', name:'Xe hơi trượt đà ma sát cỡ nhỏ',     price: 25000, cat:'Đồ chơi' },
{ code:'SP330', name:'Bộ 12 xe mô hình mini diecast',      price:155000, cat:'Đồ chơi' },

// ── Búp bê / Đồ chơi bé gái (SP331–SP342) ────
{ code:'SP331', name:'Búp bê Barbie dạo phố thời trang',   price:165000, cat:'Đồ chơi' },
{ code:'SP332', name:'Búp bê Barbie bộ đồ công chúa',      price:195000, cat:'Đồ chơi' },
{ code:'SP333', name:'Búp bê Baby Alive uống nước 30cm',   price:225000, cat:'Đồ chơi' },
{ code:'SP334', name:'Búp bê LOL Surprise hộp mù',         price:185000, cat:'Đồ chơi' },
{ code:'SP335', name:'Bộ nhà búp bê 3 tầng có nội thất',   price:420000, cat:'Đồ chơi' },
{ code:'SP336', name:'Bộ trang điểm đồ chơi bé gái 20 món',price:125000, cat:'Đồ chơi' },
{ code:'SP337', name:'Bộ đồ làm bánh đồ chơi kitchen set', price:145000, cat:'Đồ chơi' },
{ code:'SP338', name:'Bộ nấu ăn đồ chơi 28 món nhựa',     price:135000, cat:'Đồ chơi' },
{ code:'SP339', name:'Bộ phụ kiện tóc đồ chơi cho bé gái',price: 55000, cat:'Đồ chơi' },
{ code:'SP340', name:'Công chúa Disney Elsa 30cm có âm thanh',price:215000,cat:'Đồ chơi'},
{ code:'SP341', name:'Búp bê đất sét polymerclay DIY',     price: 85000, cat:'Đồ chơi' },
{ code:'SP342', name:'Bộ đồ chơi tiệm salon tóc búp bê',   price:175000, cat:'Đồ chơi' },

// ── Xếp hình / Ghép hình (SP343–SP354) ───────
{ code:'SP343', name:'Bộ Lego Duplo nhà cơ bản 60 mảnh',  price:245000, cat:'Đồ chơi' },
{ code:'SP344', name:'Bộ Lego City xe cảnh sát 150 mảnh', price:385000, cat:'Đồ chơi' },
{ code:'SP345', name:'Bộ xếp hình gỗ động vật 50 mảnh',   price:125000, cat:'Đồ chơi' },
{ code:'SP346', name:'Bộ ghép hình puzzle 500 mảnh phong cảnh',price:165000,cat:'Đồ chơi'},
{ code:'SP347', name:'Bộ ghép hình puzzle 1000 mảnh',     price:225000, cat:'Đồ chơi' },
{ code:'SP348', name:'Rubik 3x3 GAN 356 RS',               price:185000, cat:'Đồ chơi' },
{ code:'SP349', name:'Rubik 2x2 Qiyi Pocket',              price: 65000, cat:'Đồ chơi' },
{ code:'SP350', name:'Rubik 4x4 Qiyi Wuque',               price:145000, cat:'Đồ chơi' },
{ code:'SP351', name:'Bộ xếp hình từ tính Magna-Tiles 32 miếng',price:385000,cat:'Đồ chơi'},
{ code:'SP352', name:'Bộ khối gỗ xây nhà cho bé 100 miếng',price:195000, cat:'Đồ chơi' },
{ code:'SP353', name:'Tangram 7 miếng gỗ màu',             price: 45000, cat:'Đồ chơi' },
{ code:'SP354', name:'Bộ domino gỗ màu 28 viên',           price: 65000, cat:'Đồ chơi' },

// ── Đồ chơi vận động / Ngoài trời (SP355–SP365) ─
{ code:'SP355', name:'Cầu lông đồ chơi bộ 2 vợt + cầu',   price: 55000, cat:'Đồ chơi' },
{ code:'SP356', name:'Bộ đánh bóng bàn mini để bàn',       price: 85000, cat:'Đồ chơi' },
{ code:'SP357', name:'Bóng đá số 3 da PVC cho bé',         price: 75000, cat:'Đồ chơi' },
{ code:'SP358', name:'Vòng lắc hula hoop nhựa màu sắc',    price: 45000, cat:'Đồ chơi' },
{ code:'SP359', name:'Nhảy dây đồ chơi tay cầm gỗ',       price: 25000, cat:'Đồ chơi' },
{ code:'SP360', name:'Ném vòng cổ chai bộ 12 vòng',        price: 38000, cat:'Đồ chơi' },
{ code:'SP361', name:'Phi tiêu nam châm bảng tròn',        price:125000, cat:'Đồ chơi' },
{ code:'SP362', name:'Kính viễn vọng đồ chơi cho bé',      price: 95000, cat:'Đồ chơi' },
{ code:'SP363', name:'Bộ dụng cụ làm vườn đồ chơi cho bé', price: 65000, cat:'Đồ chơi' },
{ code:'SP364', name:'Xe chòi chân hình thú cho bé 1–3 tuổi',price:285000,cat:'Đồ chơi'},
{ code:'SP365', name:'Xe ba bánh đạp chân trẻ em',         price:425000, cat:'Đồ chơi' },

// ── Đồ chơi giáo dục / STEM (SP366–SP378) ────
{ code:'SP366', name:'Bộ thí nghiệm khoa học núi lửa',     price:125000, cat:'Đồ chơi' },
{ code:'SP367', name:'Bộ thí nghiệm điện học cho bé',      price:155000, cat:'Đồ chơi' },
{ code:'SP368', name:'Bộ kính hiển vi đồ chơi 100x–900x',  price:245000, cat:'Đồ chơi' },
{ code:'SP369', name:'Bộ robot lắp ráp STEM 80 mảnh',      price:285000, cat:'Đồ chơi' },
{ code:'SP370', name:'Bảng chữ cái nam châm 64 chữ màu',   price: 65000, cat:'Đồ chơi' },
{ code:'SP371', name:'Bảng tính số nam châm cho bé',        price: 55000, cat:'Đồ chơi' },
{ code:'SP372', name:'Bộ flashcard động vật song ngữ Anh-Việt',price:48000,cat:'Đồ chơi'},
{ code:'SP373', name:'Đồng hồ học giờ đồ chơi gỗ',        price: 52000, cat:'Đồ chơi' },
{ code:'SP374', name:'Bộ thẻ học toán phép tính 100 thẻ',  price: 42000, cat:'Đồ chơi' },
{ code:'SP375', name:'Bảng vẽ điện tử xóa được 8.5 inch',  price: 78000, cat:'Đồ chơi' },
{ code:'SP376', name:'Bộ tranh tô màu sơn nước 24 màu',    price: 55000, cat:'Đồ chơi' },
{ code:'SP377', name:'Bộ slime tự làm DIY 6 màu',          price: 68000, cat:'Đồ chơi' },
{ code:'SP378', name:'Bộ hạt cườm xâu vòng 500 hạt',       price: 45000, cat:'Đồ chơi' },

// ── Đồ chơi nhập vai / Hóa trang (SP379–SP388) ─
{ code:'SP379', name:'Bộ đồ chơi bác sĩ 12 món',           price: 88000, cat:'Đồ chơi' },
{ code:'SP380', name:'Bộ đồ chơi cảnh sát áo + mũ + còng', price: 75000, cat:'Đồ chơi' },
{ code:'SP381', name:'Bộ đồ chơi dụng cụ sửa chữa 10 món', price: 78000, cat:'Đồ chơi' },
{ code:'SP382', name:'Bộ đồ chơi nấu bếp BBQ 15 món',      price:115000, cat:'Đồ chơi' },
{ code:'SP383', name:'Mũ phi công đồ chơi kèm kính',        price: 48000, cat:'Đồ chơi' },
{ code:'SP384', name:'Kiếm đồ chơi phát sáng LED',          price: 65000, cat:'Đồ chơi' },
{ code:'SP385', name:'Bộ hóa trang siêu nhân Power Rangers', price: 95000, cat:'Đồ chơi' },
{ code:'SP386', name:'Bộ hóa trang công chúa đầm + vương miện',price:125000,cat:'Đồ chơi'},
{ code:'SP387', name:'Súng đồ chơi bắn bọt xốp Nerf mini',  price: 85000, cat:'Đồ chơi' },
{ code:'SP388', name:'Súng nước đồ chơi cỡ lớn 500ml',      price: 55000, cat:'Đồ chơi' },

// ── Đồ chơi sáng tạo / Nghệ thuật (SP389–SP396) ─
{ code:'SP389', name:'Bộ màu nước 36 màu có cọ',           price: 65000, cat:'Đồ chơi' },
{ code:'SP390', name:'Bộ màu sáp dầu 48 màu Faber-Castell',price: 85000, cat:'Đồ chơi' },
{ code:'SP391', name:'Bộ đồ chơi in dấu hình thú 20 con',  price: 55000, cat:'Đồ chơi' },
{ code:'SP392', name:'Bộ làm nến thơm tự chế DIY',         price: 95000, cat:'Đồ chơi' },
{ code:'SP393', name:'Bộ làm xà phòng handmade cho bé',    price: 88000, cat:'Đồ chơi' },
{ code:'SP394', name:'Khung tranh cát màu cho bé 6 màu',   price: 48000, cat:'Đồ chơi' },
{ code:'SP395', name:'Bộ trang trí sỏi đá màu 200g',       price: 35000, cat:'Đồ chơi' },
{ code:'SP396', name:'Máy in hình mini đồ chơi cho bé',    price:225000, cat:'Đồ chơi' },

// ── Đồ chơi âm nhạc (SP397–SP400) ────────────
{ code:'SP397', name:'Đàn piano đồ chơi 37 phím có nhạc',  price:285000, cat:'Đồ chơi' },
{ code:'SP398', name:'Trống đồ chơi bộ 5 món cho bé',      price:125000, cat:'Đồ chơi' },
{ code:'SP399', name:'Đàn guitar đồ chơi 6 dây gỗ nhỏ',   price:165000, cat:'Đồ chơi' },
{ code:'SP400', name:'Kèn harmonica đồ chơi 16 lỗ màu',    price: 55000, cat:'Đồ chơi' },
// ══════════════════════════════════════════════
// 100 SẢN PHẨM ĐỒ DÙNG GIA ĐÌNH (SP401–SP500)
// ══════════════════════════════════════════════

// ── Bếp / Nấu ăn (SP401–SP415) ───────────────
{ code:'SP401', name:'Nồi cơm điện Toshiba 1.8 lít',       price:450000, cat:'Đồ dùng' },
{ code:'SP402', name:'Nồi inox 3 đáy size 20cm',           price:185000, cat:'Đồ dùng' },
{ code:'SP403', name:'Chảo chống dính Sunhouse 28cm',       price:225000, cat:'Đồ dùng' },
{ code:'SP404', name:'Bộ dao nhà bếp 3 món inox',          price:145000, cat:'Đồ dùng' },
{ code:'SP405', name:'Thớt gỗ tràm dày 3cm size 30x20',    price: 85000, cat:'Đồ dùng' },
{ code:'SP406', name:'Thớt nhựa HDPE kháng khuẩn',         price: 65000, cat:'Đồ dùng' },
{ code:'SP407', name:'Bộ muỗng thìa inox 6 cái',           price: 45000, cat:'Đồ dùng' },
{ code:'SP408', name:'Bộ đũa inox 10 đôi hộp',            price: 55000, cat:'Đồ dùng' },
{ code:'SP409', name:'Vá múc canh inox có lỗ',             price: 28000, cat:'Đồ dùng' },
{ code:'SP410', name:'Xửng hấp inox 3 tầng 26cm',          price:265000, cat:'Đồ dùng' },
{ code:'SP411', name:'Rổ nhựa rửa rau quả vuông',          price: 35000, cat:'Đồ dùng' },
{ code:'SP412', name:'Hộp đựng muối tiêu inox bộ 2',       price: 48000, cat:'Đồ dùng' },
{ code:'SP413', name:'Vĩ nướng thịt gang đúc 28cm',        price:155000, cat:'Đồ dùng' },
{ code:'SP414', name:'Kẹp gắp thức ăn inox 30cm',          price: 32000, cat:'Đồ dùng' },
{ code:'SP415', name:'Bộ nồi chảo 5 món Elmich',           price:685000, cat:'Đồ dùng' },

// ── Chén bát / Ly tách (SP416–SP428) ─────────
{ code:'SP416', name:'Bộ chén cơm sứ trắng 6 cái',        price: 95000, cat:'Đồ dùng' },
{ code:'SP417', name:'Bộ đĩa sứ tròn 3 size 18 cái',      price:145000, cat:'Đồ dùng' },
{ code:'SP418', name:'Tô sứ trắng vẽ hoa size 16cm',       price: 35000, cat:'Đồ dùng' },
{ code:'SP419', name:'Ly thủy tinh uống nước 300ml bộ 6',  price: 75000, cat:'Đồ dùng' },
{ code:'SP420', name:'Ly thủy tinh cao cốc bia 500ml',     price: 28000, cat:'Đồ dùng' },
{ code:'SP421', name:'Tách cà phê sứ có đĩa lót',         price: 45000, cat:'Đồ dùng' },
{ code:'SP422', name:'Bình trà thủy tinh chịu nhiệt 600ml',price: 95000, cat:'Đồ dùng' },
{ code:'SP423', name:'Ấm đun nước thủy tinh 1.5 lít',      price:185000, cat:'Đồ dùng' },
{ code:'SP424', name:'Bình giữ nhiệt 350ml Lock&Lock',      price:165000, cat:'Đồ dùng' },
{ code:'SP425', name:'Bình nước Tritan 700ml nắp lật',     price: 85000, cat:'Đồ dùng' },
{ code:'SP426', name:'Hộp cơm giữ nhiệt inox 2 tầng',      price:145000, cat:'Đồ dùng' },
{ code:'SP427', name:'Bình đun siêu tốc điện 1.7 lít',     price:245000, cat:'Đồ dùng' },
{ code:'SP428', name:'Bộ bình trà gốm sứ 5 món',           price:185000, cat:'Đồ dùng' },

// ── Vệ sinh nhà cửa (SP429–SP441) ────────────
{ code:'SP429', name:'Chổi quét nhà lông mềm cán dài',     price: 45000, cat:'Đồ dùng' },
{ code:'SP430', name:'Cây lau nhà xoay 360 độ có xô',      price:185000, cat:'Đồ dùng' },
{ code:'SP431', name:'Cây lau nhà bông sợi cotton',        price: 55000, cat:'Đồ dùng' },
{ code:'SP432', name:'Chổi cọ toilet inox cán dài',        price: 38000, cat:'Đồ dùng' },
{ code:'SP433', name:'Miếng mút rửa chén 5 cái',           price: 18000, cat:'Đồ dùng' },
{ code:'SP434', name:'Giẻ lau đa năng microfiber bộ 5',    price: 35000, cat:'Đồ dùng' },
{ code:'SP435', name:'Nước rửa bát Sunlight 750ml',         price: 32000, cat:'Đồ dùng' },
{ code:'SP436', name:'Nước tẩy nhà vệ sinh Vim 900ml',      price: 28000, cat:'Đồ dùng' },
{ code:'SP437', name:'Nước lau sàn Sunlight hương chanh 1L',price: 38000, cat:'Đồ dùng' },
{ code:'SP438', name:'Nước xịt vệ sinh đa năng Gift 500ml', price: 32000, cat:'Đồ dùng' },
{ code:'SP439', name:'Bột tẩy vệ sinh bồn cầu 450g',       price: 22000, cat:'Đồ dùng' },
{ code:'SP440', name:'Túi rác cuộn đen 58x78cm 10 cái',    price: 15000, cat:'Đồ dùng' },
{ code:'SP441', name:'Thùng rác đạp chân 10 lít có nắp',   price: 85000, cat:'Đồ dùng' },

// ── Lưu trữ / Sắp xếp (SP442–SP454) ─────────
{ code:'SP442', name:'Hộp nhựa đựng thực phẩm Lock&Lock 1L',price: 55000, cat:'Đồ dùng' },
{ code:'SP443', name:'Bộ 3 hộp đựng thực phẩm PP có nắp', price: 65000, cat:'Đồ dùng' },
{ code:'SP444', name:'Hũ thủy tinh nắp tre 500ml',         price: 48000, cat:'Đồ dùng' },
{ code:'SP445', name:'Hộp đựng đồ khô bộ 5 size có nắp',  price: 95000, cat:'Đồ dùng' },
{ code:'SP446', name:'Túi zip đựng thực phẩm đông lạnh',   price: 18000, cat:'Đồ dùng' },
{ code:'SP447', name:'Màng bọc thực phẩm PE 30cm x 30m',   price: 22000, cat:'Đồ dùng' },
{ code:'SP448', name:'Giỏ nhựa đựng đồ có tay cầm',        price: 35000, cat:'Đồ dùng' },
{ code:'SP449', name:'Kệ để giày nhựa 5 tầng',             price:145000, cat:'Đồ dùng' },
{ code:'SP450', name:'Móc dán tường inox chịu lực 5kg',    price: 25000, cat:'Đồ dùng' },
{ code:'SP451', name:'Hộp đựng giấy ăn trên bàn inox',     price: 45000, cat:'Đồ dùng' },
{ code:'SP452', name:'Kẹp phơi quần áo nhựa 20 cái',       price: 22000, cat:'Đồ dùng' },
{ code:'SP453', name:'Móc treo quần áo nhựa 10 cái',       price: 28000, cat:'Đồ dùng' },
{ code:'SP454', name:'Túi lưới giặt đồ bảo vệ quần áo',    price: 22000, cat:'Đồ dùng' },

// ── Phòng tắm / Vệ sinh cá nhân (SP455–SP467) ─
{ code:'SP455', name:'Khăn tắm bông mềm 70x140cm',         price: 85000, cat:'Đồ dùng' },
{ code:'SP456', name:'Bộ 3 khăn mặt bông sọc màu',        price: 55000, cat:'Đồ dùng' },
{ code:'SP457', name:'Bàn chải đánh răng điện Oral-B',      price:245000, cat:'Đồ dùng' },
{ code:'SP458', name:'Gương soi treo tường hình tròn 30cm', price: 65000, cat:'Đồ dùng' },
{ code:'SP459', name:'Kệ để đồ phòng tắm 3 tầng inox',     price:125000, cat:'Đồ dùng' },
{ code:'SP460', name:'Bộ dụng cụ cắt móng tay 7 món',      price: 48000, cat:'Đồ dùng' },
{ code:'SP461', name:'Máy sấy tóc mini du lịch 1000W',     price:185000, cat:'Đồ dùng' },
{ code:'SP462', name:'Lược chải tóc gỡ rối tự nhiên',      price: 25000, cat:'Đồ dùng' },
{ code:'SP463', name:'Bình đựng dầu gội xả bơm 350ml',     price: 28000, cat:'Đồ dùng' },
{ code:'SP464', name:'Thảm chân nhà tắm chống trơn',       price: 55000, cat:'Đồ dùng' },
{ code:'SP465', name:'Giỏ đựng đồ giặt có nắp 30 lít',     price: 85000, cat:'Đồ dùng' },
{ code:'SP466', name:'Bông tắm xơ mướp tự nhiên',          price: 18000, cat:'Đồ dùng' },
{ code:'SP467', name:'Ghế ngồi tắm nhựa cao cấp chống trơn',price: 65000, cat:'Đồ dùng' },

// ── Phòng ngủ / Phòng khách (SP468–SP478) ────
{ code:'SP468', name:'Gối ôm ngủ dài 1m2 ruột bông',       price:125000, cat:'Đồ dùng' },
{ code:'SP469', name:'Vỏ gối nằm cotton 50x70cm',          price: 35000, cat:'Đồ dùng' },
{ code:'SP470', name:'Chăn hè sợi tre kháng khuẩn 1m6',    price:285000, cat:'Đồ dùng' },
{ code:'SP471', name:'Ga trải giường cotton 1m8',           price:185000, cat:'Đồ dùng' },
{ code:'SP472', name:'Đèn ngủ cắm USB LED 3 chế độ sáng',  price: 55000, cat:'Đồ dùng' },
{ code:'SP473', name:'Đồng hồ để bàn báo thức kim',        price: 75000, cat:'Đồ dùng' },
{ code:'SP474', name:'Gương treo tường khung gỗ 40x60cm',  price:145000, cat:'Đồ dùng' },
{ code:'SP475', name:'Bình cắm hoa thủy tinh cao 25cm',    price: 55000, cat:'Đồ dùng' },
{ code:'SP476', name:'Rèm cửa sổ vải gấm 1.4x1.8m',       price:225000, cat:'Đồ dùng' },
{ code:'SP477', name:'Thảm trải sàn phòng khách 1.2x1.8m', price:285000, cat:'Đồ dùng' },
{ code:'SP478', name:'Hộp đựng khăn giấy ăn để bàn',       price: 38000, cat:'Đồ dùng' },

// ── Điện – Pin – Tiện ích (SP479–SP490) ──────
{ code:'SP479', name:'Pin AA Duracell vỉ 4 viên',           price: 38000, cat:'Đồ dùng' },
{ code:'SP480', name:'Pin AAA Energizer vỉ 4 viên',         price: 35000, cat:'Đồ dùng' },
{ code:'SP481', name:'Ổ cắm điện 5 lỗ có dây 1.5m',        price: 75000, cat:'Đồ dùng' },
{ code:'SP482', name:'Băng keo điện PVC màu đen',           price: 12000, cat:'Đồ dùng' },
{ code:'SP483', name:'Bóng đèn LED 9W đuôi E27',           price: 28000, cat:'Đồ dùng' },
{ code:'SP484', name:'Bóng đèn LED bulb 5W ánh vàng',      price: 18000, cat:'Đồ dùng' },
{ code:'SP485', name:'Đèn pin LED cầm tay mini',            price: 45000, cat:'Đồ dùng' },
{ code:'SP486', name:'Nến thơm hương lavender 100g',        price: 55000, cat:'Đồ dùng' },
{ code:'SP487', name:'Nhang trầm hương thẻ 50 que',        price: 28000, cat:'Đồ dùng' },
{ code:'SP488', name:'Bộ vít tua vít 6 món cán nhựa',      price: 48000, cat:'Đồ dùng' },
{ code:'SP489', name:'Búa đóng đinh cán gỗ 300g',          price: 55000, cat:'Đồ dùng' },
{ code:'SP490', name:'Keo dán đa năng UHU Power 35ml',      price: 28000, cat:'Đồ dùng' },

// ── Giặt ủi / Chăm sóc vải (SP491–SP500) ────
{ code:'SP491', name:'Bột giặt Omo hương ban mai 3kg',      price:145000, cat:'Đồ dùng' },
{ code:'SP492', name:'Nước giặt Comfort tinh dầu 2.6 lít', price:125000, cat:'Đồ dùng' },
{ code:'SP493', name:'Nước xả vải Downy cảm hứng 1.5 lít', price: 95000, cat:'Đồ dùng' },
{ code:'SP494', name:'Nước tẩy quần áo trắng Javel 1 lít', price: 22000, cat:'Đồ dùng' },
{ code:'SP495', name:'Máy sấy quần áo mini trong nhà',      price:265000, cat:'Đồ dùng' },
{ code:'SP496', name:'Bàn ủi hơi nước Philips 1600W',       price:385000, cat:'Đồ dùng' },
{ code:'SP497', name:'Giá phơi đồ inox gấp gọn 8 thanh',   price:145000, cat:'Đồ dùng' },
{ code:'SP498', name:'Miếng dán chống nhàu cổ áo 30 miếng',price: 18000, cat:'Đồ dùng' },
{ code:'SP499', name:'Cuộn lăn xơ vải lấy lông 60 tờ',     price: 22000, cat:'Đồ dùng' },
{ code:'SP500', name:'Túi hút chân không đựng chăn màn 60x80cm',price:48000,cat:'Đồ dùng'},
// ══════════════════════════════════════════════
// 100 SẢN PHẨM ĐIỆN TỬ / CÔNG NGHỆ (SP501–SP600)
// ══════════════════════════════════════════════

// ── Điện thoại / Máy tính bảng (SP501–SP512) ─
{ code:'SP501', name:'Điện thoại Samsung Galaxy A15 128GB', price:4290000, cat:'Điện tử' },
{ code:'SP502', name:'Điện thoại Samsung Galaxy A35 256GB', price:7490000, cat:'Điện tử' },
{ code:'SP503', name:'Điện thoại Xiaomi Redmi 13C 128GB',   price:3190000, cat:'Điện tử' },
{ code:'SP504', name:'Điện thoại Xiaomi Redmi Note 13 128GB',price:5490000,cat:'Điện tử' },
{ code:'SP505', name:'Điện thoại OPPO A38 128GB',           price:3990000, cat:'Điện tử' },
{ code:'SP506', name:'Điện thoại OPPO Reno11 F 256GB',      price:8490000, cat:'Điện tử' },
{ code:'SP507', name:'Điện thoại Vivo Y18s 128GB',          price:3290000, cat:'Điện tử' },
{ code:'SP508', name:'Điện thoại realme C67 128GB',         price:3990000, cat:'Điện tử' },
{ code:'SP509', name:'Máy tính bảng Samsung Tab A9 64GB',   price:5990000, cat:'Điện tử' },
{ code:'SP510', name:'Máy tính bảng Xiaomi Redmi Pad SE',   price:4990000, cat:'Điện tử' },
{ code:'SP511', name:'Máy tính bảng Lenovo Tab M10 Plus',   price:5490000, cat:'Điện tử' },
{ code:'SP512', name:'Điện thoại bàn phím Masstel dành cho người cao tuổi',price:690000,cat:'Điện tử'},

// ── Laptop / Máy tính (SP513–SP522) ──────────
{ code:'SP513', name:'Laptop Acer Aspire 3 i3 8GB 512GB',   price:10990000,cat:'Điện tử' },
{ code:'SP514', name:'Laptop Asus VivoBook 15 i5 8GB',      price:13490000,cat:'Điện tử' },
{ code:'SP515', name:'Laptop HP 240 G9 i3 8GB 256GB',       price: 9990000,cat:'Điện tử' },
{ code:'SP516', name:'Laptop Lenovo IdeaPad 1 AMD 8GB',     price: 8990000,cat:'Điện tử' },
{ code:'SP517', name:'Laptop Dell Inspiron 15 i5 16GB',     price:15990000,cat:'Điện tử' },
{ code:'SP518', name:'Máy tính để bàn mini PC Intel N100',  price: 4990000,cat:'Điện tử' },
{ code:'SP519', name:'Màn hình máy tính LG 24 inch FHD IPS',price: 3290000,cat:'Điện tử' },
{ code:'SP520', name:'Bàn phím cơ Keychron K2 Red Switch',  price: 1890000,cat:'Điện tử' },
{ code:'SP521', name:'Chuột gaming Logitech G102 có dây',   price:  490000,cat:'Điện tử' },
{ code:'SP522', name:'Bàn phím + chuột không dây Logitech MK295',price:690000,cat:'Điện tử'},

// ── Tai nghe (SP523–SP532) ────────────────────
{ code:'SP523', name:'Tai nghe True Wireless Samsung Galaxy Buds FE',price:1490000,cat:'Điện tử'},
{ code:'SP524', name:'Tai nghe True Wireless Xiaomi Redmi Buds 4',price:590000,cat:'Điện tử'},
{ code:'SP525', name:'Tai nghe True Wireless JBL Tune 215TWS',price:890000,cat:'Điện tử' },
{ code:'SP526', name:'Tai nghe chụp tai Sony WH-CH520 Bluetooth',price:1290000,cat:'Điện tử'},
{ code:'SP527', name:'Tai nghe chụp tai JBL Tune 520BT',    price: 990000, cat:'Điện tử' },
{ code:'SP528', name:'Tai nghe gaming headset Hyper X Cloud Stinger',price:1190000,cat:'Điện tử'},
{ code:'SP529', name:'Tai nghe nhét tai có dây Xiaomi Mi Basic',price: 95000,cat:'Điện tử'},
{ code:'SP530', name:'Tai nghe nhét tai Sony MDR-EX15LP',   price:  185000,cat:'Điện tử' },
{ code:'SP531', name:'Tai nghe chống ồn Anker Soundcore Q20i',price: 890000,cat:'Điện tử'},
{ code:'SP532', name:'Tai nghe bluetooth Sony WI-C200',     price:  690000,cat:'Điện tử' },

// ── Loa bluetooth (SP533–SP540) ───────────────
{ code:'SP533', name:'Loa bluetooth JBL Go 3 mini',         price:  690000,cat:'Điện tử' },
{ code:'SP534', name:'Loa bluetooth JBL Clip 4',            price:  990000,cat:'Điện tử' },
{ code:'SP535', name:'Loa bluetooth Marshall Emberton II',  price: 2990000,cat:'Điện tử' },
{ code:'SP536', name:'Loa bluetooth Sony SRS-XB13',         price:  890000,cat:'Điện tử' },
{ code:'SP537', name:'Loa bluetooth Xiaomi Mi Outdoor mini',price:  390000,cat:'Điện tử' },
{ code:'SP538', name:'Loa vi tính Edifier R1280T 2.0',      price: 1690000,cat:'Điện tử' },
{ code:'SP539', name:'Loa soundbar Xiaomi TV 2.0 100W',     price: 1290000,cat:'Điện tử' },
{ code:'SP540', name:'Loa karaoke bluetooth Acnos KB39',    price: 1890000,cat:'Điện tử' },

// ── Sạc / Cáp / Pin dự phòng (SP541–SP553) ───
{ code:'SP541', name:'Pin dự phòng Anker PowerCore 10000mAh',price: 490000,cat:'Điện tử' },
{ code:'SP542', name:'Pin dự phòng Xiaomi 20000mAh 22.5W',  price:  690000,cat:'Điện tử' },
{ code:'SP543', name:'Củ sạc nhanh Anker 65W GaN 2 cổng',   price:  590000,cat:'Điện tử' },
{ code:'SP544', name:'Củ sạc nhanh Baseus 30W USB-C PD',    price:  285000,cat:'Điện tử' },
{ code:'SP545', name:'Củ sạc nhanh 3 cổng Ugreen 65W',      price:  490000,cat:'Điện tử' },
{ code:'SP546', name:'Cáp sạc USB-C to USB-C Anker 60W 1m', price:  185000,cat:'Điện tử' },
{ code:'SP547', name:'Cáp sạc Lightning Ugreen MFi 1m',     price:  195000,cat:'Điện tử' },
{ code:'SP548', name:'Cáp sạc Micro USB Baseus 2.4A 1m',    price:   65000,cat:'Điện tử' },
{ code:'SP549', name:'Đế sạc không dây Belkin 15W',         price:  490000,cat:'Điện tử' },
{ code:'SP550', name:'Đế sạc không dây 3 in 1 Magsafe',     price:  890000,cat:'Điện tử' },
{ code:'SP551', name:'Hub USB-C 7 in 1 Ugreen 4K HDMI',     price:  690000,cat:'Điện tử' },
{ code:'SP552', name:'Bộ chuyển đổi HDMI to VGA Ugreen',    price:  145000,cat:'Điện tử' },
{ code:'SP553', name:'Ổ cắm USB 4 cổng sạc nhanh 65W',      price:  325000,cat:'Điện tử' },

// ── Phụ kiện điện thoại (SP554–SP565) ────────
{ code:'SP554', name:'Ốp lưng chống sốc iPhone 15 Pro Max', price:  145000,cat:'Điện tử' },
{ code:'SP555', name:'Ốp lưng Samsung Galaxy A55 silicon',  price:   65000,cat:'Điện tử' },
{ code:'SP556', name:'Ốp lưng Xiaomi Redmi Note 13 cứng',   price:   55000,cat:'Điện tử' },
{ code:'SP557', name:'Cường lực iPhone 15 Pro Max 9H',       price:   85000,cat:'Điện tử' },
{ code:'SP558', name:'Cường lực Samsung Galaxy A35 full màn',price:   65000,cat:'Điện tử' },
{ code:'SP559', name:'Giá đỡ điện thoại kẹp xe hơi',        price:   95000,cat:'Điện tử' },
{ code:'SP560', name:'Giá đỡ điện thoại vươn cổ ngỗng',    price:  125000,cat:'Điện tử' },
{ code:'SP561', name:'Bao da điện thoại đa năng size L',     price:   85000,cat:'Điện tử' },
{ code:'SP562', name:'Vòng nhẫn điện thoại PopSocket kim loại',price:  45000,cat:'Điện tử'},
{ code:'SP563', name:'Dây đeo điện thoại chéo vai 160cm',   price:   38000,cat:'Điện tử' },
{ code:'SP564', name:'Đầu đọc thẻ nhớ USB-C All in One',    price:   95000,cat:'Điện tử' },
{ code:'SP565', name:'Thẻ nhớ MicroSD SanDisk 128GB A2',    price:  245000,cat:'Điện tử' },

// ── Thiết bị mạng / Lưu trữ (SP566–SP574) ────
{ code:'SP566', name:'Bộ phát Wifi TP-Link Archer C6 AC1200',price:  590000,cat:'Điện tử' },
{ code:'SP567', name:'Bộ phát Wifi Xiaomi Router AX3000',   price:  890000,cat:'Điện tử' },
{ code:'SP568', name:'Thiết bị khuếch đại Wifi TP-Link RE330',price: 390000,cat:'Điện tử' },
{ code:'SP569', name:'USB Flash Drive SanDisk 64GB USB 3.1', price:  145000,cat:'Điện tử' },
{ code:'SP570', name:'USB Flash Drive Kingston 128GB',       price:  225000,cat:'Điện tử' },
{ code:'SP571', name:'Ổ cứng di động Seagate 1TB USB 3.0',  price:  990000,cat:'Điện tử' },
{ code:'SP572', name:'Ổ cứng di động WD 2TB My Passport',   price: 1690000,cat:'Điện tử' },
{ code:'SP573', name:'SSD Box đựng ổ cứng 2.5 inch USB-C',  price:  185000,cat:'Điện tử' },
{ code:'SP574', name:'Dây cáp mạng LAN Cat6 10m bấm sẵn',  price:   95000,cat:'Điện tử' },

// ── Máy ảnh / Camera (SP575–SP582) ───────────
{ code:'SP575', name:'Camera an ninh Xiaomi C400 4MP WiFi', price:  490000,cat:'Điện tử' },
{ code:'SP576', name:'Camera an ninh Ezviz C6N 1080P PTZ',  price:  590000,cat:'Điện tử' },
{ code:'SP577', name:'Camera hành trình ô tô 70mai A400',   price:  890000,cat:'Điện tử' },
{ code:'SP578', name:'Máy ảnh chụp lấy ngay Fujifilm Instax mini 12',price:1890000,cat:'Điện tử'},
{ code:'SP579', name:'Phim chụp Instax Mini 20 tờ',         price:  185000,cat:'Điện tử' },
{ code:'SP580', name:'Tripod đứng xách tay 1m3 nhôm',       price:  285000,cat:'Điện tử' },
{ code:'SP581', name:'Gimbals chống rung điện thoại 3 trục',price:  890000,cat:'Điện tử' },
{ code:'SP582', name:'Đèn LED ring light 26cm kèm tripod',  price:  345000,cat:'Điện tử' },

// ── Đồng hồ thông minh / Wearable (SP583–SP590) ─
{ code:'SP583', name:'Đồng hồ thông minh Samsung Galaxy Watch 6',price:4990000,cat:'Điện tử'},
{ code:'SP584', name:'Đồng hồ thông minh Xiaomi Watch S1 Active',price:2990000,cat:'Điện tử'},
{ code:'SP585', name:'Đồng hồ thông minh Garmin Forerunner 55', price:3990000,cat:'Điện tử'},
{ code:'SP586', name:'Vòng đeo tay thể thao Xiaomi Band 8', price:  690000,cat:'Điện tử' },
{ code:'SP587', name:'Vòng đeo tay Huawei Band 8',          price:  890000,cat:'Điện tử' },
{ code:'SP588', name:'Đồng hồ thông minh trẻ em 4G Wonlex', price:  890000,cat:'Điện tử' },
{ code:'SP589', name:'Đồng hồ thể thao GPS Garmin Instinct 2',price:6490000,cat:'Điện tử'},
{ code:'SP590', name:'Nhẫn thông minh theo dõi sức khỏe',   price: 1290000,cat:'Điện tử' },

// ── Máy in / Văn phòng điện tử (SP591–SP598) ──
{ code:'SP591', name:'Máy in phun màu Canon PIXMA E3370',   price: 1990000,cat:'Điện tử' },
{ code:'SP592', name:'Máy in laser HP LaserJet 107A',       price: 2990000,cat:'Điện tử' },
{ code:'SP593', name:'Mực in Canon 810 Black chính hãng',   price:  185000,cat:'Điện tử' },
{ code:'SP594', name:'Máy scan tài liệu cầm tay A4',        price: 1290000,cat:'Điện tử' },
{ code:'SP595', name:'Máy chiếu mini Xiaomi Mi Projector 2',price: 5990000,cat:'Điện tử' },
{ code:'SP596', name:'Bảng viết điện tử thông minh Wacom',  price: 1490000,cat:'Điện tử' },
{ code:'SP597', name:'Máy đọc sách Kindle Paperwhite 16GB', price: 3290000,cat:'Điện tử' },
{ code:'SP598', name:'Máy tính bỏ túi Casio FX-570EX',      price:  325000,cat:'Điện tử' },

// ── Phụ kiện máy tính (SP599–SP600) ──────────
{ code:'SP599', name:'Bàn di chuột gaming XXL 80x30cm',     price:  185000,cat:'Điện tử' },
{ code:'SP600', name:'Webcam Logitech C270 HD 720P',         price:  590000,cat:'Điện tử' },
// ══════════════════════════════════════════════
// 100 SẢN PHẨM ĐIỆN THOẠI iPHONE / SAMSUNG /
// HONOR / REALME (SP601–SP700)
// ══════════════════════════════════════════════

// ── iPhone (SP601–SP630) ──────────────────────
{ code:'SP601', name:'iPhone 15 Pro Max 256GB Titan Đen',    price:34990000, cat:'iPhone' },
{ code:'SP602', name:'iPhone 15 Pro Max 512GB Titan Trắng',  price:39990000, cat:'iPhone' },
{ code:'SP603', name:'iPhone 15 Pro 128GB Titan Tự Nhiên',   price:27990000, cat:'iPhone' },
{ code:'SP604', name:'iPhone 15 Pro 256GB Titan Xanh',       price:30990000, cat:'iPhone' },
{ code:'SP605', name:'iPhone 15 Plus 128GB Vàng',            price:23990000, cat:'iPhone' },
{ code:'SP606', name:'iPhone 15 Plus 256GB Hồng',            price:26990000, cat:'iPhone' },
{ code:'SP607', name:'iPhone 15 128GB Đen',                  price:19990000, cat:'iPhone' },
{ code:'SP608', name:'iPhone 15 256GB Xanh Dương',           price:22990000, cat:'iPhone' },
{ code:'SP609', name:'iPhone 15 512GB Vàng',                 price:28990000, cat:'iPhone' },
{ code:'SP610', name:'iPhone 14 Pro Max 128GB Tím Đậm',      price:28990000, cat:'iPhone' },
{ code:'SP611', name:'iPhone 14 Pro Max 256GB Vàng',         price:31990000, cat:'iPhone' },
{ code:'SP612', name:'iPhone 14 Pro 128GB Đen',              price:22990000, cat:'iPhone' },
{ code:'SP613', name:'iPhone 14 Pro 256GB Tím',              price:25990000, cat:'iPhone' },
{ code:'SP614', name:'iPhone 14 Plus 128GB Đỏ',             price:19490000, cat:'iPhone' },
{ code:'SP615', name:'iPhone 14 128GB Xanh Dương',           price:16990000, cat:'iPhone' },
{ code:'SP616', name:'iPhone 14 256GB Trắng',                price:19490000, cat:'iPhone' },
{ code:'SP617', name:'iPhone 13 Pro Max 256GB Xanh Sierra',  price:22990000, cat:'iPhone' },
{ code:'SP618', name:'iPhone 13 Pro 128GB Bạc',              price:18990000, cat:'iPhone' },
{ code:'SP619', name:'iPhone 13 128GB Hồng',                 price:14990000, cat:'iPhone' },
{ code:'SP620', name:'iPhone 13 256GB Đen Đêm',             price:17490000, cat:'iPhone' },
{ code:'SP621', name:'iPhone 13 Mini 128GB Xanh Lá',         price:12990000, cat:'iPhone' },
{ code:'SP622', name:'iPhone 12 64GB Tím',                   price:10990000, cat:'iPhone' },
{ code:'SP623', name:'iPhone 12 128GB Đỏ',                   price:12490000, cat:'iPhone' },
{ code:'SP624', name:'iPhone 12 Pro Max 256GB Xanh',         price:16990000, cat:'iPhone' },
{ code:'SP625', name:'iPhone 11 64GB Vàng',                  price: 8490000, cat:'iPhone' },
{ code:'SP626', name:'iPhone 11 128GB Tím',                  price: 9490000, cat:'iPhone' },
{ code:'SP627', name:'iPhone SE 2022 64GB Trắng',            price: 8990000, cat:'iPhone' },
{ code:'SP628', name:'iPhone SE 2022 128GB Đen',             price: 9990000, cat:'iPhone' },
{ code:'SP629', name:'iPhone XS Max 64GB Vàng – Cũ đẹp 97%',price: 7490000, cat:'iPhone' },
{ code:'SP630', name:'iPhone XR 128GB Đỏ – Cũ đẹp 97%',     price: 5990000, cat:'iPhone' },

// ── Samsung Galaxy S Series (SP631–SP645) ─────
{ code:'SP631', name:'Samsung Galaxy S24 Ultra 256GB Titan',  price:33990000, cat:'Samsung' },
{ code:'SP632', name:'Samsung Galaxy S24 Ultra 512GB Đen',    price:37990000, cat:'Samsung' },
{ code:'SP633', name:'Samsung Galaxy S24+ 256GB Tím',         price:23990000, cat:'Samsung' },
{ code:'SP634', name:'Samsung Galaxy S24 128GB Xám',          price:18990000, cat:'Samsung' },
{ code:'SP635', name:'Samsung Galaxy S24 256GB Vàng Hổ Phách',price:21490000, cat:'Samsung' },
{ code:'SP636', name:'Samsung Galaxy S23 FE 128GB Mint',      price:10990000, cat:'Samsung' },
{ code:'SP637', name:'Samsung Galaxy S23 Ultra 256GB Xanh',   price:25990000, cat:'Samsung' },
{ code:'SP638', name:'Samsung Galaxy S23+ 256GB Kem',         price:18990000, cat:'Samsung' },
{ code:'SP639', name:'Samsung Galaxy S23 128GB Hồng',         price:13990000, cat:'Samsung' },
{ code:'SP640', name:'Samsung Galaxy S22 Ultra 256GB Đỏ',     price:18490000, cat:'Samsung' },
{ code:'SP641', name:'Samsung Galaxy S22 128GB Trắng',        price:10990000, cat:'Samsung' },
{ code:'SP642', name:'Samsung Galaxy Z Fold 5 256GB Xanh',    price:39990000, cat:'Samsung' },
{ code:'SP643', name:'Samsung Galaxy Z Flip 5 256GB Vàng',    price:22990000, cat:'Samsung' },
{ code:'SP644', name:'Samsung Galaxy Z Fold 4 256GB Đen',     price:29990000, cat:'Samsung' },
{ code:'SP645', name:'Samsung Galaxy Z Flip 4 128GB Tím',     price:15990000, cat:'Samsung' },

// ── Samsung Galaxy A Series (SP646–SP658) ─────
{ code:'SP646', name:'Samsung Galaxy A55 5G 128GB Xanh Băng', price: 8990000, cat:'Samsung' },
{ code:'SP647', name:'Samsung Galaxy A55 5G 256GB Vàng',       price:10490000, cat:'Samsung' },
{ code:'SP648', name:'Samsung Galaxy A35 5G 128GB Xanh Dương', price: 6990000, cat:'Samsung' },
{ code:'SP649', name:'Samsung Galaxy A35 5G 256GB Tím',        price: 8490000, cat:'Samsung' },
{ code:'SP650', name:'Samsung Galaxy A25 5G 128GB Đen',        price: 5490000, cat:'Samsung' },
{ code:'SP651', name:'Samsung Galaxy A15 5G 128GB Xanh',       price: 4290000, cat:'Samsung' },
{ code:'SP652', name:'Samsung Galaxy A15 4G 128GB Hồng',       price: 3790000, cat:'Samsung' },
{ code:'SP653', name:'Samsung Galaxy A05s 64GB Bạc',           price: 2990000, cat:'Samsung' },
{ code:'SP654', name:'Samsung Galaxy A54 5G 128GB Trắng',      price: 7990000, cat:'Samsung' },
{ code:'SP655', name:'Samsung Galaxy A54 5G 256GB Xanh Lá',    price: 9490000, cat:'Samsung' },
{ code:'SP656', name:'Samsung Galaxy A34 5G 128GB Bạc',        price: 5990000, cat:'Samsung' },
{ code:'SP657', name:'Samsung Galaxy M55 5G 128GB Xám Xanh',   price: 6490000, cat:'Samsung' },
{ code:'SP658', name:'Samsung Galaxy M34 5G 128GB Xanh Băng',  price: 4990000, cat:'Samsung' },

// ── Honor (SP659–SP680) ───────────────────────
{ code:'SP659', name:'Honor Magic6 Pro 512GB Đen Huyền',       price:22990000, cat:'Honor'   },
{ code:'SP660', name:'Honor Magic6 Pro 256GB Xanh Dương',      price:19990000, cat:'Honor'   },
{ code:'SP661', name:'Honor Magic6 Lite 256GB Xanh Dương',     price: 7990000, cat:'Honor'   },
{ code:'SP662', name:'Honor Magic6 Lite 128GB Đen',            price: 6990000, cat:'Honor'   },
{ code:'SP663', name:'Honor Magic5 Pro 512GB Vàng',            price:16990000, cat:'Honor'   },
{ code:'SP664', name:'Honor Magic5 Lite 256GB Xanh Bạc Hà',   price: 6490000, cat:'Honor'   },
{ code:'SP665', name:'Honor Magic V2 512GB Đen Titan',         price:29990000, cat:'Honor'   },
{ code:'SP666', name:'Honor 200 Pro 512GB Tím',                price:14990000, cat:'Honor'   },
{ code:'SP667', name:'Honor 200 256GB Xanh Dương',             price: 9990000, cat:'Honor'   },
{ code:'SP668', name:'Honor 200 Lite 256GB Hồng',              price: 5990000, cat:'Honor'   },
{ code:'SP669', name:'Honor 90 512GB Đen',                     price: 9490000, cat:'Honor'   },
{ code:'SP670', name:'Honor 90 256GB Xanh Peacock',            price: 8490000, cat:'Honor'   },
{ code:'SP671', name:'Honor 90 Lite 256GB Xanh Bạc Hà',       price: 4990000, cat:'Honor'   },
{ code:'SP672', name:'Honor X9b 256GB Bạc Titan',              price: 6990000, cat:'Honor'   },
{ code:'SP673', name:'Honor X9b 128GB Đen',                    price: 5990000, cat:'Honor'   },
{ code:'SP674', name:'Honor X8b 256GB Vàng Hồng',              price: 4990000, cat:'Honor'   },
{ code:'SP675', name:'Honor X8b 128GB Đen',                    price: 4290000, cat:'Honor'   },
{ code:'SP676', name:'Honor X7b 128GB Xanh Dương',             price: 3490000, cat:'Honor'   },
{ code:'SP677', name:'Honor X6b 128GB Bạc',                    price: 2990000, cat:'Honor'   },
{ code:'SP678', name:'Honor Play 50 Plus 256GB Đen',           price: 3990000, cat:'Honor'   },
{ code:'SP679', name:'Honor Pad X9 128GB Xám',                 price: 5490000, cat:'Honor'   },
{ code:'SP680', name:'Honor Pad 9 256GB Xanh Không Gian',      price: 7990000, cat:'Honor'   },

// ── Realme (SP681–SP700) ──────────────────────
{ code:'SP681', name:'Realme GT 6 512GB Xanh Bạc Hà',         price:16990000, cat:'Realme'  },
{ code:'SP682', name:'Realme GT 6 256GB Đen',                  price:14990000, cat:'Realme'  },
{ code:'SP683', name:'Realme GT Neo 6 256GB Xanh Dương',       price:10990000, cat:'Realme'  },
{ code:'SP684', name:'Realme GT Neo 6 512GB Đen',              price:12990000, cat:'Realme'  },
{ code:'SP685', name:'Realme 12 Pro+ 5G 256GB Xanh',           price: 8990000, cat:'Realme'  },
{ code:'SP686', name:'Realme 12 Pro 5G 256GB Hải Quân',        price: 7490000, cat:'Realme'  },
{ code:'SP687', name:'Realme 12 5G 128GB Xanh Bạc Hà',        price: 5490000, cat:'Realme'  },
{ code:'SP688', name:'Realme 12 5G 256GB Xanh Dương',          price: 6490000, cat:'Realme'  },
{ code:'SP689', name:'Realme 12x 5G 128GB Xanh Dương',         price: 4490000, cat:'Realme'  },
{ code:'SP690', name:'Realme 11 Pro+ 5G 256GB Vàng Oasis',     price: 7990000, cat:'Realme'  },
{ code:'SP691', name:'Realme 11 Pro 5G 256GB Tím Bình Minh',   price: 6490000, cat:'Realme'  },
{ code:'SP692', name:'Realme 11 5G 128GB Xanh Dương',          price: 4990000, cat:'Realme'  },
{ code:'SP693', name:'Realme C67 5G 128GB Xanh',               price: 3990000, cat:'Realme'  },
{ code:'SP694', name:'Realme C67 4G 128GB Đen',                price: 3490000, cat:'Realme'  },
{ code:'SP695', name:'Realme C55 6GB/128GB Xanh Lá',           price: 3290000, cat:'Realme'  },
{ code:'SP696', name:'Realme C53 6GB/128GB Vàng',              price: 2990000, cat:'Realme'  },
{ code:'SP697', name:'Realme Narzo 60 Pro 5G 256GB Tím',       price: 6990000, cat:'Realme'  },
{ code:'SP698', name:'Realme Narzo 60x 5G 128GB Xanh Dương',   price: 3990000, cat:'Realme'  },
{ code:'SP699', name:'Realme Pad 2 WiFi 6GB/128GB Xám',        price: 5490000, cat:'Realme'  },
{ code:'SP700', name:'Realme Pad Mini 4G 64GB Xanh Dương',     price: 3490000, cat:'Realme'  },
// ══════════════════════════════════════════════
// 200 SẢN PHẨM LAPTOP: MacBook / Lenovo / HP /
// Dell / Asus / Acer / MSI / LG (SP701–SP900)
// ══════════════════════════════════════════════

// ── Apple MacBook (SP701–SP730) ───────────────
{ code:'SP701', name:'MacBook Pro 16 M3 Max 48GB 1TB Đen',   price:99990000, cat:'MacBook' },
{ code:'SP702', name:'MacBook Pro 16 M3 Max 36GB 512GB Bạc', price:89990000, cat:'MacBook' },
{ code:'SP703', name:'MacBook Pro 16 M3 Pro 18GB 512GB Đen', price:69990000, cat:'MacBook' },
{ code:'SP704', name:'MacBook Pro 16 M3 Pro 18GB 512GB Bạc', price:69990000, cat:'MacBook' },
{ code:'SP705', name:'MacBook Pro 14 M3 Max 48GB 1TB Đen',   price:94990000, cat:'MacBook' },
{ code:'SP706', name:'MacBook Pro 14 M3 Pro 18GB 512GB Bạc', price:64990000, cat:'MacBook' },
{ code:'SP707', name:'MacBook Pro 14 M3 Pro 18GB 1TB Đen',   price:69990000, cat:'MacBook' },
{ code:'SP708', name:'MacBook Pro 14 M3 8GB 512GB Bạc',      price:49990000, cat:'MacBook' },
{ code:'SP709', name:'MacBook Pro 14 M3 8GB 1TB Đen',        price:54990000, cat:'MacBook' },
{ code:'SP710', name:'MacBook Pro 13 M2 8GB 256GB Bạc',      price:32990000, cat:'MacBook' },
{ code:'SP711', name:'MacBook Pro 13 M2 8GB 512GB Xám',      price:36990000, cat:'MacBook' },
{ code:'SP712', name:'MacBook Pro 13 M2 16GB 512GB Bạc',     price:39990000, cat:'MacBook' },
{ code:'SP713', name:'MacBook Air 15 M3 8GB 256GB Đêm',      price:39990000, cat:'MacBook' },
{ code:'SP714', name:'MacBook Air 15 M3 8GB 512GB Bạc',      price:44990000, cat:'MacBook' },
{ code:'SP715', name:'MacBook Air 15 M3 16GB 512GB Xanh',    price:49990000, cat:'MacBook' },
{ code:'SP716', name:'MacBook Air 15 M3 24GB 1TB Đêm',       price:59990000, cat:'MacBook' },
{ code:'SP717', name:'MacBook Air 13 M3 8GB 256GB Xám',      price:28990000, cat:'MacBook' },
{ code:'SP718', name:'MacBook Air 13 M3 8GB 512GB Đêm',      price:32990000, cat:'MacBook' },
{ code:'SP719', name:'MacBook Air 13 M3 16GB 256GB Bạc',     price:34990000, cat:'MacBook' },
{ code:'SP720', name:'MacBook Air 13 M3 16GB 512GB Xanh',    price:38990000, cat:'MacBook' },
{ code:'SP721', name:'MacBook Air 13 M2 8GB 256GB Đêm',      price:24990000, cat:'MacBook' },
{ code:'SP722', name:'MacBook Air 13 M2 8GB 512GB Bạc',      price:28990000, cat:'MacBook' },
{ code:'SP723', name:'MacBook Air 13 M2 16GB 512GB Xám',     price:32990000, cat:'MacBook' },
{ code:'SP724', name:'MacBook Air 13 M2 24GB 1TB Đêm',       price:42990000, cat:'MacBook' },
{ code:'SP725', name:'MacBook Air 13 M1 8GB 256GB Vàng',     price:19990000, cat:'MacBook' },
{ code:'SP726', name:'MacBook Air 13 M1 8GB 512GB Xám',      price:22990000, cat:'MacBook' },
{ code:'SP727', name:'MacBook Air 13 M1 16GB 256GB Bạc',     price:23990000, cat:'MacBook' },
{ code:'SP728', name:'MacBook Pro 13 M1 8GB 256GB Bạc',      price:21990000, cat:'MacBook' },
{ code:'SP729', name:'MacBook Pro 13 M1 8GB 512GB Xám',      price:24990000, cat:'MacBook' },
{ code:'SP730', name:'MacBook Pro 13 M1 Pro 16GB 512GB Đen', price:29990000, cat:'MacBook' },

// ── Lenovo ThinkPad (SP731–SP745) ─────────────
{ code:'SP731', name:'Lenovo ThinkPad X1 Carbon G12 i7 32GB 1TB',  price:42990000, cat:'Lenovo' },
{ code:'SP732', name:'Lenovo ThinkPad X1 Carbon G11 i7 16GB 512GB',price:36990000, cat:'Lenovo' },
{ code:'SP733', name:'Lenovo ThinkPad X1 Yoga G8 i7 16GB 512GB',   price:38990000, cat:'Lenovo' },
{ code:'SP734', name:'Lenovo ThinkPad T14s G4 AMD R7 16GB 512GB',  price:24990000, cat:'Lenovo' },
{ code:'SP735', name:'Lenovo ThinkPad T14 G4 i5 16GB 512GB',       price:22990000, cat:'Lenovo' },
{ code:'SP736', name:'Lenovo ThinkPad E14 G5 i5 8GB 512GB',        price:16990000, cat:'Lenovo' },
{ code:'SP737', name:'Lenovo ThinkPad E14 G5 i7 16GB 512GB',       price:20990000, cat:'Lenovo' },
{ code:'SP738', name:'Lenovo ThinkPad L14 G4 i5 16GB 256GB',       price:18990000, cat:'Lenovo' },
{ code:'SP739', name:'Lenovo ThinkPad P16s G2 i7 32GB 1TB',        price:48990000, cat:'Lenovo' },
{ code:'SP740', name:'Lenovo ThinkPad X13 G4 i7 16GB 512GB',       price:26990000, cat:'Lenovo' },
{ code:'SP741', name:'Lenovo ThinkPad T16 G2 i7 32GB 512GB',       price:28990000, cat:'Lenovo' },
{ code:'SP742', name:'Lenovo ThinkPad Z16 G2 AMD R9 32GB 1TB',     price:34990000, cat:'Lenovo' },
{ code:'SP743', name:'Lenovo ThinkPad X1 Extreme G5 i9 32GB 1TB',  price:62990000, cat:'Lenovo' },
{ code:'SP744', name:'Lenovo ThinkPad E15 G4 i5 8GB 256GB',        price:15990000, cat:'Lenovo' },
{ code:'SP745', name:'Lenovo ThinkPad E16 G1 i7 16GB 512GB',       price:19990000, cat:'Lenovo' },

// ── Lenovo IdeaPad / Legion (SP746–SP762) ─────
{ code:'SP746', name:'Lenovo IdeaPad Slim 5 i5 16GB 512GB',        price:14990000, cat:'Lenovo' },
{ code:'SP747', name:'Lenovo IdeaPad Slim 5 i7 16GB 1TB',          price:18990000, cat:'Lenovo' },
{ code:'SP748', name:'Lenovo IdeaPad Slim 3 i5 8GB 512GB',         price:11990000, cat:'Lenovo' },
{ code:'SP749', name:'Lenovo IdeaPad Slim 3 i3 8GB 256GB',         price: 8990000, cat:'Lenovo' },
{ code:'SP750', name:'Lenovo IdeaPad Flex 5 i5 16GB 512GB 2in1',   price:16990000, cat:'Lenovo' },
{ code:'SP751', name:'Lenovo IdeaPad Gaming 3 i5 16GB 512GB RTX3050',price:18990000,cat:'Lenovo'},
{ code:'SP752', name:'Lenovo IdeaPad Gaming 3 i7 16GB 512GB RTX3050',price:21990000,cat:'Lenovo'},
{ code:'SP753', name:'Lenovo Legion 5 i7 16GB 512GB RTX4060',      price:28990000, cat:'Lenovo' },
{ code:'SP754', name:'Lenovo Legion 5 Pro i7 32GB 1TB RTX4070',    price:38990000, cat:'Lenovo' },
{ code:'SP755', name:'Lenovo Legion 5i Gen9 i9 32GB 1TB RTX4070Ti',price:48990000, cat:'Lenovo' },
{ code:'SP756', name:'Lenovo Legion 7 i9 32GB 2TB RTX4080',        price:62990000, cat:'Lenovo' },
{ code:'SP757', name:'Lenovo Legion Pro 7 i9 32GB 2TB RTX4090',    price:79990000, cat:'Lenovo' },
{ code:'SP758', name:'Lenovo LOQ 15 i5 16GB 512GB RTX3050',        price:16990000, cat:'Lenovo' },
{ code:'SP759', name:'Lenovo LOQ 15 i7 16GB 1TB RTX4060',          price:22990000, cat:'Lenovo' },
{ code:'SP760', name:'Lenovo Yoga 7 2in1 AMD R7 16GB 512GB',       price:19990000, cat:'Lenovo' },
{ code:'SP761', name:'Lenovo Yoga 9i i7 Evo 16GB 1TB OLED',        price:38990000, cat:'Lenovo' },
{ code:'SP762', name:'Lenovo Yoga Slim 6i i7 16GB 512GB',          price:22990000, cat:'Lenovo' },

// ── HP (SP763–SP785) ──────────────────────────
{ code:'SP763', name:'HP Spectre x360 14 i7 Evo 16GB 1TB OLED',   price:42990000, cat:'HP' },
{ code:'SP764', name:'HP Spectre x360 16 i7 32GB 1TB OLED',       price:52990000, cat:'HP' },
{ code:'SP765', name:'HP Envy x360 15 AMD R7 16GB 512GB',          price:22990000, cat:'HP' },
{ code:'SP766', name:'HP Envy 16 i7 32GB 1TB RTX4060',             price:34990000, cat:'HP' },
{ code:'SP767', name:'HP Envy 13 i5 8GB 512GB',                    price:18990000, cat:'HP' },
{ code:'SP768', name:'HP EliteBook 840 G10 i7 16GB 512GB',         price:28990000, cat:'HP' },
{ code:'SP769', name:'HP EliteBook 860 G10 i7 32GB 1TB',           price:34990000, cat:'HP' },
{ code:'SP770', name:'HP EliteBook 1040 G10 i7 16GB 512GB',        price:38990000, cat:'HP' },
{ code:'SP771', name:'HP ProBook 440 G10 i5 8GB 256GB',            price:16990000, cat:'HP' },
{ code:'SP772', name:'HP ProBook 450 G10 i7 16GB 512GB',           price:21990000, cat:'HP' },
{ code:'SP773', name:'HP ZBook Firefly 14 G10 i7 32GB 1TB',        price:44990000, cat:'HP' },
{ code:'SP774', name:'HP Victus 15 i5 8GB 512GB RTX3050',          price:17990000, cat:'HP' },
{ code:'SP775', name:'HP Victus 15 i7 16GB 512GB RTX4060',         price:24990000, cat:'HP' },
{ code:'SP776', name:'HP Victus 16 i7 16GB 1TB RTX4060',           price:26990000, cat:'HP' },
{ code:'SP777', name:'HP OMEN 16 i7 16GB 512GB RTX4070',           price:34990000, cat:'HP' },
{ code:'SP778', name:'HP OMEN 16 i9 32GB 1TB RTX4080',             price:54990000, cat:'HP' },
{ code:'SP779', name:'HP OMEN Transcend 16 i9 32GB 2TB RTX4090',   price:74990000, cat:'HP' },
{ code:'SP780', name:'HP Pavilion 15 i5 8GB 512GB',                price:13990000, cat:'HP' },
{ code:'SP781', name:'HP Pavilion Plus 14 i7 16GB 512GB OLED',     price:21990000, cat:'HP' },
{ code:'SP782', name:'HP 240 G10 i3 8GB 256GB',                    price: 8990000, cat:'HP' },
{ code:'SP783', name:'HP 240 G10 i5 8GB 512GB',                    price:11990000, cat:'HP' },
{ code:'SP784', name:'HP 245 G10 AMD R5 8GB 256GB',                price: 9990000, cat:'HP' },
{ code:'SP785', name:'HP 255 G10 AMD R7 16GB 512GB',               price:13990000, cat:'HP' },

// ── Dell (SP786–SP810) ────────────────────────
{ code:'SP786', name:'Dell XPS 15 i7 16GB 512GB OLED RTX4060',    price:44990000, cat:'Dell' },
{ code:'SP787', name:'Dell XPS 15 i9 32GB 1TB OLED RTX4070',      price:59990000, cat:'Dell' },
{ code:'SP788', name:'Dell XPS 13 i7 Evo 16GB 512GB',             price:34990000, cat:'Dell' },
{ code:'SP789', name:'Dell XPS 13 Plus i7 32GB 1TB OLED',         price:44990000, cat:'Dell' },
{ code:'SP790', name:'Dell XPS 14 i7 32GB 1TB OLED RTX4050',      price:52990000, cat:'Dell' },
{ code:'SP791', name:'Dell XPS 16 i9 64GB 2TB OLED RTX4080',      price:84990000, cat:'Dell' },
{ code:'SP792', name:'Dell Inspiron 15 3520 i5 8GB 512GB',        price:12990000, cat:'Dell' },
{ code:'SP793', name:'Dell Inspiron 15 3520 i7 16GB 512GB',       price:16990000, cat:'Dell' },
{ code:'SP794', name:'Dell Inspiron 16 5630 i5 8GB 512GB',        price:15990000, cat:'Dell' },
{ code:'SP795', name:'Dell Inspiron 16 5630 i7 16GB 1TB',         price:19990000, cat:'Dell' },
{ code:'SP796', name:'Dell Inspiron 14 5440 i5 16GB 512GB',       price:14990000, cat:'Dell' },
{ code:'SP797', name:'Dell Inspiron 14 Plus 7440 i7 16GB 1TB',    price:22990000, cat:'Dell' },
{ code:'SP798', name:'Dell Vostro 3520 i5 8GB 256GB',             price:11990000, cat:'Dell' },
{ code:'SP799', name:'Dell Vostro 5630 i7 16GB 512GB',            price:19990000, cat:'Dell' },
{ code:'SP800', name:'Dell Latitude 5540 i7 16GB 512GB',          price:26990000, cat:'Dell' },
{ code:'SP801', name:'Dell Latitude 7440 i7 32GB 1TB',            price:36990000, cat:'Dell' },
{ code:'SP802', name:'Dell Precision 3580 i7 16GB 512GB RTX500',  price:38990000, cat:'Dell' },
{ code:'SP803', name:'Dell Precision 5680 i9 32GB 1TB RTX3500',   price:68990000, cat:'Dell' },
{ code:'SP804', name:'Dell Alienware m16 i9 32GB 1TB RTX4080',    price:69990000, cat:'Dell' },
{ code:'SP805', name:'Dell Alienware x16 i9 64GB 2TB RTX4090',    price:99990000, cat:'Dell' },
{ code:'SP806', name:'Dell Alienware m18 i9 32GB 1TB RTX4080',    price:79990000, cat:'Dell' },
{ code:'SP807', name:'Dell G15 5530 i5 8GB 512GB RTX3050',        price:18990000, cat:'Dell' },
{ code:'SP808', name:'Dell G15 5530 i7 16GB 512GB RTX4060',       price:24990000, cat:'Dell' },
{ code:'SP809', name:'Dell G16 7630 i7 16GB 1TB RTX4070',         price:34990000, cat:'Dell' },
{ code:'SP810', name:'Dell G16 7630 i9 32GB 2TB RTX4080',         price:54990000, cat:'Dell' },

// ── Asus (SP811–SP835) ────────────────────────
{ code:'SP811', name:'Asus ZenBook 14 OLED i7 16GB 512GB',        price:22990000, cat:'Asus' },
{ code:'SP812', name:'Asus ZenBook 14X OLED i9 32GB 1TB',         price:32990000, cat:'Asus' },
{ code:'SP813', name:'Asus ZenBook Pro 16X OLED i9 32GB 2TB',     price:54990000, cat:'Asus' },
{ code:'SP814', name:'Asus VivoBook 15 i5 8GB 512GB',             price:12990000, cat:'Asus' },
{ code:'SP815', name:'Asus VivoBook 15 i7 16GB 512GB',            price:15990000, cat:'Asus' },
{ code:'SP816', name:'Asus VivoBook 16X OLED i7 16GB 1TB',        price:19990000, cat:'Asus' },
{ code:'SP817', name:'Asus VivoBook S 15 OLED i7 16GB 512GB',     price:21990000, cat:'Asus' },
{ code:'SP818', name:'Asus ExpertBook B1 i5 8GB 256GB',           price:13990000, cat:'Asus' },
{ code:'SP819', name:'Asus ExpertBook B9 i7 16GB 1TB',            price:36990000, cat:'Asus' },
{ code:'SP820', name:'Asus ProArt Studiobook 16 i9 64GB 4TB',     price:84990000, cat:'Asus' },
{ code:'SP821', name:'Asus ROG Zephyrus G14 R9 32GB 1TB RTX4060', price:34990000, cat:'Asus' },
{ code:'SP822', name:'Asus ROG Zephyrus G16 i9 32GB 1TB RTX4090', price:74990000, cat:'Asus' },
{ code:'SP823', name:'Asus ROG Strix G15 R9 16GB 512GB RTX4060',  price:28990000, cat:'Asus' },
{ code:'SP824', name:'Asus ROG Strix G17 i7 16GB 1TB RTX4070',    price:38990000, cat:'Asus' },
{ code:'SP825', name:'Asus ROG Strix Scar 16 i9 32GB 2TB RTX4090',price:79990000, cat:'Asus' },
{ code:'SP826', name:'Asus ROG Flow X13 R9 16GB 512GB RTX4060',   price:32990000, cat:'Asus' },
{ code:'SP827', name:'Asus ROG Flow Z13 i9 16GB 512GB RTX4050',   price:38990000, cat:'Asus' },
{ code:'SP828', name:'Asus TUF Gaming A15 R7 16GB 512GB RTX4060', price:22990000, cat:'Asus' },
{ code:'SP829', name:'Asus TUF Gaming A17 R9 16GB 512GB RTX4060', price:24990000, cat:'Asus' },
{ code:'SP830', name:'Asus TUF Gaming F15 i7 16GB 512GB RTX4060', price:23990000, cat:'Asus' },
{ code:'SP831', name:'Asus TUF Gaming F17 i7 32GB 1TB RTX4070',   price:34990000, cat:'Asus' },
{ code:'SP832', name:'Asus Laptop 15 i3 8GB 256GB',               price: 8990000, cat:'Asus' },
{ code:'SP833', name:'Asus Laptop 15 i5 8GB 512GB',               price:11990000, cat:'Asus' },
{ code:'SP834', name:'Asus Chromebook Plus CX34 i5 8GB 256GB',    price:12990000, cat:'Asus' },
{ code:'SP835', name:'Asus ProArt P16 AMD R9 32GB 1TB RTX4070',   price:54990000, cat:'Asus' },

// ── Acer (SP836–SP858) ────────────────────────
{ code:'SP836', name:'Acer Swift 14 AI i7 Evo 16GB 1TB OLED',     price:26990000, cat:'Acer' },
{ code:'SP837', name:'Acer Swift 3 i5 8GB 512GB',                  price:13990000, cat:'Acer' },
{ code:'SP838', name:'Acer Swift Go 14 OLED i7 16GB 512GB',        price:19990000, cat:'Acer' },
{ code:'SP839', name:'Acer Swift Go 16 OLED i5 16GB 512GB',        price:16990000, cat:'Acer' },
{ code:'SP840', name:'Acer Swift X 14 i7 16GB 512GB RTX4050',      price:22990000, cat:'Acer' },
{ code:'SP841', name:'Acer Aspire 3 i3 8GB 256GB',                 price: 8490000, cat:'Acer' },
{ code:'SP842', name:'Acer Aspire 3 i5 8GB 512GB',                 price:10990000, cat:'Acer' },
{ code:'SP843', name:'Acer Aspire 5 i5 16GB 512GB',                price:13990000, cat:'Acer' },
{ code:'SP844', name:'Acer Aspire 5 i7 16GB 512GB',                price:16990000, cat:'Acer' },
{ code:'SP845', name:'Acer Aspire 7 i5 8GB 512GB RTX3050',         price:16990000, cat:'Acer' },
{ code:'SP846', name:'Acer Aspire 7 i7 16GB 512GB RTX3050Ti',      price:20990000, cat:'Acer' },
{ code:'SP847', name:'Acer Nitro V 15 i5 8GB 512GB RTX4050',       price:18990000, cat:'Acer' },
{ code:'SP848', name:'Acer Nitro V 15 i7 16GB 512GB RTX4060',      price:22990000, cat:'Acer' },
{ code:'SP849', name:'Acer Nitro 5 i7 16GB 512GB RTX4060',         price:24990000, cat:'Acer' },
{ code:'SP850', name:'Acer Nitro 5 i9 32GB 1TB RTX4070',           price:34990000, cat:'Acer' },
{ code:'SP851', name:'Acer Predator Helios 16 i9 32GB 1TB RTX4080',price:59990000, cat:'Acer' },
{ code:'SP852', name:'Acer Predator Helios 18 i9 32GB 2TB RTX4090',price:79990000, cat:'Acer' },
{ code:'SP853', name:'Acer Predator Triton 16 i7 32GB 1TB RTX4070',price:44990000, cat:'Acer' },
{ code:'SP854', name:'Acer TravelMate P4 i5 8GB 256GB',            price:14990000, cat:'Acer' },
{ code:'SP855', name:'Acer TravelMate P6 i7 16GB 512GB',           price:22990000, cat:'Acer' },
{ code:'SP856', name:'Acer Chromebook 514 Intel N100 8GB 128GB',   price: 7990000, cat:'Acer' },
{ code:'SP857', name:'Acer ConceptD 7 i9 64GB 2TB RTX3080',        price:74990000, cat:'Acer' },
{ code:'SP858', name:'Acer Enduro N3 i5 8GB 256GB Rugged',         price:18990000, cat:'Acer' },

// ── MSI (SP859–SP875) ─────────────────────────
{ code:'SP859', name:'MSI Modern 14 i5 16GB 512GB',                price:14990000, cat:'MSI' },
{ code:'SP860', name:'MSI Modern 15 i7 16GB 512GB',                price:17990000, cat:'MSI' },
{ code:'SP861', name:'MSI Prestige 13 Evo i7 32GB 1TB',            price:28990000, cat:'MSI' },
{ code:'SP862', name:'MSI Prestige 14 i7 16GB 512GB RTX4050',      price:24990000, cat:'MSI' },
{ code:'SP863', name:'MSI Stealth 14 i7 16GB 512GB RTX4060',       price:32990000, cat:'MSI' },
{ code:'SP864', name:'MSI Stealth 16 i9 32GB 2TB RTX4090',         price:74990000, cat:'MSI' },
{ code:'SP865', name:'MSI Katana 15 i5 8GB 512GB RTX4050',         price:19990000, cat:'MSI' },
{ code:'SP866', name:'MSI Katana 17 i7 16GB 512GB RTX4060',        price:24990000, cat:'MSI' },
{ code:'SP867', name:'MSI Cyborg 15 i5 8GB 512GB RTX4050',         price:18990000, cat:'MSI' },
{ code:'SP868', name:'MSI Cyborg 15 i7 16GB 512GB RTX4060',        price:22990000, cat:'MSI' },
{ code:'SP869', name:'MSI Vector GP66 i9 32GB 1TB RTX4080',        price:59990000, cat:'MSI' },
{ code:'SP870', name:'MSI Raider GE78 i9 64GB 4TB RTX4090',        price:99990000, cat:'MSI' },
{ code:'SP871', name:'MSI Titan GT77 i9 64GB 4TB RTX3080Ti',       price:89990000, cat:'MSI' },
{ code:'SP872', name:'MSI Creator Z16P i9 32GB 2TB RTX3080Ti',     price:74990000, cat:'MSI' },
{ code:'SP873', name:'MSI Summit E14 i7 16GB 512GB',               price:24990000, cat:'MSI' },
{ code:'SP874', name:'MSI Thin GF63 i5 8GB 512GB RTX4050',         price:16990000, cat:'MSI' },
{ code:'SP875', name:'MSI Crosshair 16 i9 32GB 2TB RTX4080',       price:64990000, cat:'MSI' },

// ── LG Gram (SP876–SP888) ─────────────────────
{ code:'SP876', name:'LG Gram 14 i5 Evo 16GB 512GB',               price:24990000, cat:'LG Gram' },
{ code:'SP877', name:'LG Gram 14 i7 Evo 16GB 1TB',                 price:29990000, cat:'LG Gram' },
{ code:'SP878', name:'LG Gram 15 i5 Evo 16GB 512GB',               price:26990000, cat:'LG Gram' },
{ code:'SP879', name:'LG Gram 15 i7 Evo 32GB 1TB',                 price:34990000, cat:'LG Gram' },
{ code:'SP880', name:'LG Gram 16 i7 Evo 16GB 512GB',               price:32990000, cat:'LG Gram' },
{ code:'SP881', name:'LG Gram 16 i7 Evo 32GB 1TB',                 price:38990000, cat:'LG Gram' },
{ code:'SP882', name:'LG Gram 17 i7 Evo 16GB 512GB',               price:35990000, cat:'LG Gram' },
{ code:'SP883', name:'LG Gram 17 i7 Evo 32GB 2TB',                 price:44990000, cat:'LG Gram' },
{ code:'SP884', name:'LG Gram Style 14 OLED i7 16GB 512GB',        price:36990000, cat:'LG Gram' },
{ code:'SP885', name:'LG Gram Style 16 OLED i7 32GB 1TB',          price:44990000, cat:'LG Gram' },
{ code:'SP886', name:'LG Gram Pro 16 OLED i7 32GB 1TB',            price:48990000, cat:'LG Gram' },
{ code:'SP887', name:'LG Gram Pro 16 OLED i9 32GB 2TB',            price:58990000, cat:'LG Gram' },
{ code:'SP888', name:'LG Gram 2in1 16 i7 Evo 16GB 512GB',          price:38990000, cat:'LG Gram' },

// ── Microsoft Surface (SP889–SP898) ───────────
{ code:'SP889', name:'Surface Pro 10 i5 16GB 256GB',               price:28990000, cat:'Surface' },
{ code:'SP890', name:'Surface Pro 10 i7 32GB 512GB',               price:42990000, cat:'Surface' },
{ code:'SP891', name:'Surface Laptop 6 i5 16GB 256GB',             price:26990000, cat:'Surface' },
{ code:'SP892', name:'Surface Laptop 6 i7 32GB 512GB',             price:38990000, cat:'Surface' },
{ code:'SP893', name:'Surface Laptop Go 3 i5 8GB 256GB',           price:18990000, cat:'Surface' },
{ code:'SP894', name:'Surface Book 3 i7 32GB 1TB GTX1660Ti',       price:54990000, cat:'Surface' },
{ code:'SP895', name:'Surface Laptop Studio 2 i7 16GB RTX4050',    price:48990000, cat:'Surface' },
{ code:'SP896', name:'Surface Laptop Studio 2 i7 32GB RTX4060',    price:62990000, cat:'Surface' },
{ code:'SP897', name:'Surface Pro 9 i5 8GB 256GB',                 price:22990000, cat:'Surface' },
{ code:'SP898', name:'Surface Pro 9 i7 16GB 512GB',                price:34990000, cat:'Surface' },

// ── Phụ kiện Laptop (SP899–SP900) ─────────────
{ code:'SP899', name:'Túi chống sốc laptop 15.6 inch da cao cấp',  price:  285000, cat:'Phụ kiện Laptop' },
{ code:'SP900', name:'Balo laptop 15.6 inch chống nước Samsonite',  price:  890000, cat:'Phụ kiện Laptop' },
// ══════════════════════════════════════════════
// 100 SẢN PHẨM XE MÁY / XE MÁY ĐIỆN / XE ĐIỆN
// Honda / Yamaha / Suzuki / SYM / VinFast /
// Xmen / Yadea / Dat Bike (SP901–SP1000)
// ══════════════════════════════════════════════

// ── Honda Xe Số / Xe Côn (SP901–SP915) ───────
{ code:'SP901', name:'Honda Wave Alpha 110 phanh đĩa Đỏ Đen',      price: 18490000, cat:'Honda' },
{ code:'SP902', name:'Honda Wave Alpha 110 phanh đĩa Xanh Bạc',    price: 18490000, cat:'Honda' },
{ code:'SP903', name:'Honda Wave RSX 110 Fi phanh đĩa Đen Đỏ',     price: 20990000, cat:'Honda' },
{ code:'SP904', name:'Honda Wave RSX 110 Fi phanh đĩa Trắng Đen',  price: 20990000, cat:'Honda' },
{ code:'SP905', name:'Honda Future 125 Fi phanh đĩa Đen Xanh',     price: 30490000, cat:'Honda' },
{ code:'SP906', name:'Honda Future 125 Fi phanh đĩa Đỏ Đen',       price: 30490000, cat:'Honda' },
{ code:'SP907', name:'Honda Super Dream 110 Fi Đen Đỏ',             price: 19990000, cat:'Honda' },
{ code:'SP908', name:'Honda CB150R ExMotion Đen Nhám',              price: 68900000, cat:'Honda' },
{ code:'SP909', name:'Honda CB300R Đen Bóng',                       price: 99000000, cat:'Honda' },
{ code:'SP910', name:'Honda CB500F Đỏ Đen 2024',                    price:148000000, cat:'Honda' },
{ code:'SP911', name:'Honda CB650R Neo Sports Cafe Đen',            price:208000000, cat:'Honda' },
{ code:'SP912', name:'Honda Winner X 150 Special Đen Vàng',        price: 46990000, cat:'Honda' },
{ code:'SP913', name:'Honda Winner X 150 Standard Trắng Đen',      price: 43990000, cat:'Honda' },
{ code:'SP914', name:'Honda Sonic 150R Đỏ Đen 2024',               price: 52900000, cat:'Honda' },
{ code:'SP915', name:'Honda MSX 125 SF Xanh Trắng Đen',            price: 52900000, cat:'Honda' },

// ── Honda Tay Ga (SP916–SP928) ────────────────
{ code:'SP916', name:'Honda Vision 110 phanh đĩa Hồng Trắng',      price: 30990000, cat:'Honda' },
{ code:'SP917', name:'Honda Vision 110 phanh đĩa Đen Mờ',          price: 30990000, cat:'Honda' },
{ code:'SP918', name:'Honda Vision 110 phanh đĩa Xanh Bạc',        price: 30990000, cat:'Honda' },
{ code:'SP919', name:'Honda Air Blade 125 ABS Đen Đỏ 2024',        price: 47990000, cat:'Honda' },
{ code:'SP920', name:'Honda Air Blade 125 ABS Trắng Bạc 2024',     price: 47990000, cat:'Honda' },
{ code:'SP921', name:'Honda Air Blade 125 Standard Xanh Đen 2024', price: 43990000, cat:'Honda' },
{ code:'SP922', name:'Honda SH 125i ABS Smart Key Trắng Đen',      price: 70990000, cat:'Honda' },
{ code:'SP923', name:'Honda SH 150i ABS Smart Key Bạc Đen',        price: 83490000, cat:'Honda' },
{ code:'SP924', name:'Honda SH 160i ABS Smart Key Đen Xanh',       price: 95490000, cat:'Honda' },
{ code:'SP925', name:'Honda SH Mode 125 ABS Smart Key Xanh Vàng',  price: 57490000, cat:'Honda' },
{ code:'SP926', name:'Honda PCX 125 ABS Smart Key Đen Đỏ 2024',    price: 74490000, cat:'Honda' },
{ code:'SP927', name:'Honda PCX 160 ABS Smart Key Trắng Bạc 2024', price: 85490000, cat:'Honda' },
{ code:'SP928', name:'Honda Lead 125 ABS Smart Key Hồng Trắng',    price: 54990000, cat:'Honda' },

// ── Yamaha (SP929–SP943) ──────────────────────
{ code:'SP929', name:'Yamaha Exciter 155 VVA ABS Đen Nhám Vàng',   price: 52990000, cat:'Yamaha' },
{ code:'SP930', name:'Yamaha Exciter 155 VVA ABS Đỏ Đen 2024',     price: 52990000, cat:'Yamaha' },
{ code:'SP931', name:'Yamaha Exciter 155 VVA Standard Xanh Đen',   price: 48490000, cat:'Yamaha' },
{ code:'SP932', name:'Yamaha Jupiter MX King 150 Đen Xanh',        price: 48990000, cat:'Yamaha' },
{ code:'SP933', name:'Yamaha FreeGo S ABS Smart Key Trắng Xanh',   price: 38490000, cat:'Yamaha' },
{ code:'SP934', name:'Yamaha NMax 155 ABS Smart Key Xanh Đen 2024',price: 72990000, cat:'Yamaha' },
{ code:'SP935', name:'Yamaha NMax 155 Connected ABS Trắng Bạc',    price: 79990000, cat:'Yamaha' },
{ code:'SP936', name:'Yamaha Janus 125 Premium Hồng Trắng',        price: 31490000, cat:'Yamaha' },
{ code:'SP937', name:'Yamaha Grande 125 Hybrid Smart Key Tím',     price: 48490000, cat:'Yamaha' },
{ code:'SP938', name:'Yamaha Grande 125 Hybrid Smart Key Trắng',   price: 48490000, cat:'Yamaha' },
{ code:'SP939', name:'Yamaha Latte 125 Smart Key Kem Trắng',       price: 42990000, cat:'Yamaha' },
{ code:'SP940', name:'Yamaha Latte 125 Smart Key Xanh Bạc Hà',     price: 42990000, cat:'Yamaha' },
{ code:'SP941', name:'Yamaha MT-15 V2 ABS Xanh Đen 2024',          price: 82000000, cat:'Yamaha' },
{ code:'SP942', name:'Yamaha R15 V4 ABS Xanh Racing 2024',         price: 89000000, cat:'Yamaha' },
{ code:'SP943', name:'Yamaha Tricity 155 ABS Xám Bạc',             price:102000000, cat:'Yamaha' },

// ── Suzuki / SYM (SP944–SP952) ────────────────
{ code:'SP944', name:'Suzuki Raider R150 Fi Đen Đỏ 2024',          price: 46990000, cat:'Suzuki' },
{ code:'SP945', name:'Suzuki Raider R150 Fi Xanh Đen 2024',        price: 46990000, cat:'Suzuki' },
{ code:'SP946', name:'Suzuki GSX-R150 Xanh Trắng 2024',            price: 68900000, cat:'Suzuki' },
{ code:'SP947', name:'Suzuki GSX-S150 Đen Cam 2024',               price: 65900000, cat:'Suzuki' },
{ code:'SP948', name:'SYM Star SR 170 ABS Đen Đỏ 2024',            price: 43990000, cat:'SYM'    },
{ code:'SP949', name:'SYM Attila Elizabeth Fi Tím Hồng',            price: 26990000, cat:'SYM'    },
{ code:'SP950', name:'SYM Galaxy 125 Fi ABS Trắng Xanh',           price: 38990000, cat:'SYM'    },
{ code:'SP951', name:'SYM Bonus 110 Fi Đen Đỏ',                    price: 18490000, cat:'SYM'    },
{ code:'SP952', name:'SYM Shark 125 Fi ABS Xanh Đen',              price: 35490000, cat:'SYM'    },

// ── VinFast Xe Máy Điện (SP953–SP966) ────────
{ code:'SP953', name:'VinFast Evo200 Lipo Đen Xanh 2024',          price: 21990000, cat:'VinFast' },
{ code:'SP954', name:'VinFast Evo200 Lipo Trắng Hồng 2024',        price: 21990000, cat:'VinFast' },
{ code:'SP955', name:'VinFast Evo 200S Lipo Đen Đỏ 2024',          price: 24990000, cat:'VinFast' },
{ code:'SP956', name:'VinFast Theon S Lipo Trắng Bạc 2024',        price: 26490000, cat:'VinFast' },
{ code:'SP957', name:'VinFast Feliz S Lipo Xanh Mint 2024',        price: 19490000, cat:'VinFast' },
{ code:'SP958', name:'VinFast Feliz S Lipo Hồng Pastel 2024',      price: 19490000, cat:'VinFast' },
{ code:'SP959', name:'VinFast Impes Lipo Đen Vàng 2024',           price: 29990000, cat:'VinFast' },
{ code:'SP960', name:'VinFast Impes S Lipo Trắng Ngọc 2024',       price: 33990000, cat:'VinFast' },
{ code:'SP961', name:'VinFast Klara S2 Lipo Đen Đỏ Sport',         price: 22490000, cat:'VinFast' },
{ code:'SP962', name:'VinFast Klara A2 Lipo Xanh Dương 2024',      price: 20990000, cat:'VinFast' },
{ code:'SP963', name:'VinFast Ludo Lipo Vàng Chanh 2024',          price: 17490000, cat:'VinFast' },
{ code:'SP964', name:'VinFast Ludo Lipo Hồng Đào 2024',            price: 17490000, cat:'VinFast' },
{ code:'SP965', name:'VinFast Vento Lipo Đen Xanh Sport 2024',     price: 26990000, cat:'VinFast' },
{ code:'SP966', name:'VinFast Vento S Lipo Trắng Bạc 2024',        price: 30990000, cat:'VinFast' },

// ── VinFast Ô Tô Điện (SP967–SP974) ──────────
{ code:'SP967', name:'VinFast VF3 Xanh Dương – Tiêu chuẩn',        price:235000000, cat:'VinFast' },
{ code:'SP968', name:'VinFast VF3 Trắng Ngọc – Plus',              price:255000000, cat:'VinFast' },
{ code:'SP969', name:'VinFast VF5 Plus Xanh Mint 2024',            price:458000000, cat:'VinFast' },
{ code:'SP970', name:'VinFast VF6 Base Đen 2024',                   price:675000000, cat:'VinFast' },
{ code:'SP971', name:'VinFast VF6 Plus Trắng Ngọc 2024',           price:765000000, cat:'VinFast' },
{ code:'SP972', name:'VinFast VF7 Plus Xanh Dương 2024',           price:850000000, cat:'VinFast' },
{ code:'SP973', name:'VinFast VF8 Plus Đỏ 2024',                   price:999000000, cat:'VinFast' },
{ code:'SP974', name:'VinFast VF9 Plus Đen Titan 2024',            price:1390000000,cat:'VinFast' },

// ── Xmen Xe Máy Điện (SP975–SP983) ───────────
{ code:'SP975', name:'Xmen One S3 Đen Xanh 2024',                  price: 17490000, cat:'Xmen'   },
{ code:'SP976', name:'Xmen One S3 Trắng Hồng 2024',                price: 17490000, cat:'Xmen'   },
{ code:'SP977', name:'Xmen Max S2 Đen Đỏ Sport 2024',              price: 20990000, cat:'Xmen'   },
{ code:'SP978', name:'Xmen Max S2 Xanh Dương 2024',                price: 20990000, cat:'Xmen'   },
{ code:'SP979', name:'Xmen Ero S3 Hồng Pastel 2024',               price: 15990000, cat:'Xmen'   },
{ code:'SP980', name:'Xmen Ero S3 Xanh Bạc Hà 2024',               price: 15990000, cat:'Xmen'   },
{ code:'SP981', name:'Xmen Neo Đen Vàng Sport 2024',               price: 22990000, cat:'Xmen'   },
{ code:'SP982', name:'Xmen Nova S2 Trắng Bạc 2024',                price: 19490000, cat:'Xmen'   },
{ code:'SP983', name:'Xmen Iris S2 Tím Ánh Kim 2024',              price: 18490000, cat:'Xmen'   },

// ── Yadea Xe Máy Điện (SP984–SP991) ──────────
{ code:'SP984', name:'Yadea G5S Đen Đỏ Sport 2024',                price: 22990000, cat:'Yadea'  },
{ code:'SP985', name:'Yadea G5S Trắng Bạc 2024',                   price: 22990000, cat:'Yadea'  },
{ code:'SP986', name:'Yadea C-TS Pro Xanh Dương 2024',             price: 19490000, cat:'Yadea'  },
{ code:'SP987', name:'Yadea T9 Pro Đen Vàng 2024',                 price: 28990000, cat:'Yadea'  },
{ code:'SP988', name:'Yadea E8S Pro Trắng Hồng 2024',              price: 16990000, cat:'Yadea'  },
{ code:'SP989', name:'Yadea Limo Xanh Bạc Hà 2024',               price: 14990000, cat:'Yadea'  },
{ code:'SP990', name:'Yadea EM10 Pro Đen Đỏ 2024',                 price: 24990000, cat:'Yadea'  },
{ code:'SP991', name:'Yadea Trojan S Đen Nhám 2024',               price: 32990000, cat:'Yadea'  },

// ── Dat Bike / Selex / Pega (SP992–SP1000) ───
{ code:'SP992', name:'Dat Bike Weaver++ Đen Nhám 2024',            price: 65000000, cat:'Dat Bike'},
{ code:'SP993', name:'Dat Bike Weaver++ Trắng Bạc 2024',           price: 65000000, cat:'Dat Bike'},
{ code:'SP994', name:'Dat Bike Slade Đen Xanh 2024',               price: 85000000, cat:'Dat Bike'},
{ code:'SP995', name:'Selex Camel Đen Đỏ 2024',                    price: 28990000, cat:'Selex'  },
{ code:'SP996', name:'Selex Camel Xanh Dương 2024',                price: 28990000, cat:'Selex'  },
{ code:'SP997', name:'Selex Volt Pro Đen Cam Sport 2024',          price: 35990000, cat:'Selex'  },
{ code:'SP998', name:'Pega NewStyle Plus Đen Đỏ 2024',             price: 16490000, cat:'Pega'   },
{ code:'SP999', name:'Pega Aura S Hồng Trắng 2024',                price: 14990000, cat:'Pega'   },
{ code:'SP1000',name:'Pega CEO Sport Đen Xanh 2024',               price: 18990000, cat:'Pega'   },
// ══════════════════════════════════════════════
// 200 SẢN PHẨM QUẦN ÁO HÀNG HIỆU / GIÀY DÉP
// Nike / Adidas / Puma / Gucci / Louis Vuitton /
// Zara / H&M / Uniqlo / New Balance / Converse
// (SP1001–SP1200)
// ══════════════════════════════════════════════

// ── Nike Giày (SP1001–SP1020) ─────────────────
{ code:'SP1001', name:'Nike Air Force 1 Low Triple White',          price: 2490000, cat:'Nike'   },
{ code:'SP1002', name:'Nike Air Force 1 Low Black Swoosh',          price: 2490000, cat:'Nike'   },
{ code:'SP1003', name:'Nike Air Force 1 High Wheat Suede',          price: 2790000, cat:'Nike'   },
{ code:'SP1004', name:'Nike Air Max 90 White Grey',                 price: 3290000, cat:'Nike'   },
{ code:'SP1005', name:'Nike Air Max 270 React Black Red',           price: 3490000, cat:'Nike'   },
{ code:'SP1006', name:'Nike Air Max 97 Silver Bullet',              price: 3990000, cat:'Nike'   },
{ code:'SP1007', name:'Nike Air Max Plus TN Black Gold',            price: 3690000, cat:'Nike'   },
{ code:'SP1008', name:'Nike Air Jordan 1 Retro High OG Chicago',    price: 5490000, cat:'Nike'   },
{ code:'SP1009', name:'Nike Air Jordan 1 Mid SE Rebel Pink',        price: 3290000, cat:'Nike'   },
{ code:'SP1010', name:'Nike Air Jordan 4 Retro Fire Red',           price: 6490000, cat:'Nike'   },
{ code:'SP1011', name:'Nike Air Jordan 11 Retro Jubilee',           price: 7490000, cat:'Nike'   },
{ code:'SP1012', name:'Nike Dunk Low Panda White Black',            price: 2890000, cat:'Nike'   },
{ code:'SP1013', name:'Nike Dunk High University Blue',             price: 3190000, cat:'Nike'   },
{ code:'SP1014', name:'Nike Blazer Mid 77 Vintage White',           price: 2590000, cat:'Nike'   },
{ code:'SP1015', name:'Nike React Infinity Run FK4 Xanh Đen',       price: 3790000, cat:'Nike'   },
{ code:'SP1016', name:'Nike ZoomX Vaporfly Next% 3 Trắng',          price: 7990000, cat:'Nike'   },
{ code:'SP1017', name:'Nike Pegasus 40 Wolf Grey',                  price: 3290000, cat:'Nike'   },
{ code:'SP1018', name:'Nike Free Run 5.0 Black White',              price: 2990000, cat:'Nike'   },
{ code:'SP1019', name:'Nike SB Dunk Low Pro Paisley',               price: 3490000, cat:'Nike'   },
{ code:'SP1020', name:'Nike ACG Mountain Fly 2 Low Dark Grey',      price: 4290000, cat:'Nike'   },

// ── Nike Quần Áo (SP1021–SP1033) ─────────────
{ code:'SP1021', name:'Áo thun Nike Dri-FIT Training Tee Đen',      price:  890000, cat:'Nike'   },
{ code:'SP1022', name:'Áo thun Nike Sportswear Club Trắng',         price:  790000, cat:'Nike'   },
{ code:'SP1023', name:'Áo hoodie Nike Tech Fleece Full-Zip Đen',    price: 2490000, cat:'Nike'   },
{ code:'SP1024', name:'Áo hoodie Nike Club Fleece Xanh Navy',       price: 1490000, cat:'Nike'   },
{ code:'SP1025', name:'Áo khoác Nike Windrunner Jacket Xanh',       price: 2290000, cat:'Nike'   },
{ code:'SP1026', name:'Quần short Nike Pro Dri-FIT Đen',            price:  690000, cat:'Nike'   },
{ code:'SP1027', name:'Quần jogger Nike Tech Fleece Xám',           price: 1890000, cat:'Nike'   },
{ code:'SP1028', name:'Quần jogger Nike Sportswear Club Đen',       price: 1290000, cat:'Nike'   },
{ code:'SP1029', name:'Áo polo Nike Dri-FIT Victory Trắng',         price:  990000, cat:'Nike'   },
{ code:'SP1030', name:'Bộ thể thao Nike Dri-FIT Academy Đen Vàng',  price: 1490000, cat:'Nike'   },
{ code:'SP1031', name:'Áo khoác Nike Storm-FIT ADV Đen',            price: 3490000, cat:'Nike'   },
{ code:'SP1032', name:'Legging Nike Pro 365 Tights Đen',            price:  890000, cat:'Nike'   },
{ code:'SP1033', name:'Mũ Nike Heritage86 Futura Wash Đen',         price:  490000, cat:'Nike'   },

// ── Adidas Giày (SP1034–SP1050) ───────────────
{ code:'SP1034', name:'Adidas Stan Smith White Green Stripes',      price: 2290000, cat:'Adidas' },
{ code:'SP1035', name:'Adidas Superstar All White OG',              price: 2190000, cat:'Adidas' },
{ code:'SP1036', name:'Adidas Superstar Black Gold Stripes',        price: 2390000, cat:'Adidas' },
{ code:'SP1037', name:'Adidas Samba OG White Black Gum',            price: 2790000, cat:'Adidas' },
{ code:'SP1038', name:'Adidas Samba OG Core Black',                 price: 2790000, cat:'Adidas' },
{ code:'SP1039', name:'Adidas Gazelle Bold Platform Xanh',          price: 2490000, cat:'Adidas' },
{ code:'SP1040', name:'Adidas Campus 00s White Green',              price: 2190000, cat:'Adidas' },
{ code:'SP1041', name:'Adidas NMD R1 Triple Black Boost',           price: 3290000, cat:'Adidas' },
{ code:'SP1042', name:'Adidas NMD V3 Core Black White',             price: 2990000, cat:'Adidas' },
{ code:'SP1043', name:'Adidas Ultraboost 23 Cloud White',           price: 4490000, cat:'Adidas' },
{ code:'SP1044', name:'Adidas Ultraboost Light Core Black',         price: 4290000, cat:'Adidas' },
{ code:'SP1045', name:'Adidas Yeezy Boost 350 V2 Zebra',           price: 6990000, cat:'Adidas' },
{ code:'SP1046', name:'Adidas Yeezy Boost 350 V2 Bone',            price: 6490000, cat:'Adidas' },
{ code:'SP1047', name:'Adidas Yeezy Slide Pure',                    price: 2990000, cat:'Adidas' },
{ code:'SP1048', name:'Adidas Forum Low Cloud White',               price: 2190000, cat:'Adidas' },
{ code:'SP1049', name:'Adidas Handball Spezial Gum White',         price: 2490000, cat:'Adidas' },
{ code:'SP1050', name:'Adidas Ozweego Triple Black',                price: 2590000, cat:'Adidas' },

// ── Adidas Quần Áo (SP1051–SP1062) ───────────
{ code:'SP1051', name:'Áo thun Adidas Essentials 3-Stripes Đen',    price:  790000, cat:'Adidas' },
{ code:'SP1052', name:'Áo thun Adidas Trefoil Tee Trắng',           price:  690000, cat:'Adidas' },
{ code:'SP1053', name:'Áo hoodie Adidas Originals Trefoil Xám',     price: 1490000, cat:'Adidas' },
{ code:'SP1054', name:'Áo khoác Adidas Tiro 23 Track Jacket Đen',   price: 1290000, cat:'Adidas' },
{ code:'SP1055', name:'Bộ tracksuit Adidas 3-Stripes Xanh Navy',    price: 1890000, cat:'Adidas' },
{ code:'SP1056', name:'Quần jogger Adidas Essentials 3-Stripes Đen',price:  890000, cat:'Adidas' },
{ code:'SP1057', name:'Quần short Adidas Tiro 23 Training',         price:  590000, cat:'Adidas' },
{ code:'SP1058', name:'Áo polo Adidas Performance Primegreen',      price:  890000, cat:'Adidas' },
{ code:'SP1059', name:'Áo khoác Adidas Originals SST Đen Trắng',    price: 2290000, cat:'Adidas' },
{ code:'SP1060', name:'Mũ Adidas Originals Trefoil Baseball Cap',   price:  490000, cat:'Adidas' },
{ code:'SP1061', name:'Ba lô Adidas Classic 3-Stripes 26L Đen',     price: 1290000, cat:'Adidas' },
{ code:'SP1062', name:'Túi Adidas Linear Tote Bag Xanh Navy',       price:  690000, cat:'Adidas' },

// ── Puma (SP1063–SP1073) ──────────────────────
{ code:'SP1063', name:'Puma Suede Classic Triple Black',             price: 1990000, cat:'Puma'   },
{ code:'SP1064', name:'Puma Suede Classic White Team Gold',          price: 1990000, cat:'Puma'   },
{ code:'SP1065', name:'Puma RS-X Efekt Triple White',               price: 2490000, cat:'Puma'   },
{ code:'SP1066', name:'Puma Clyde Vintage Court Green',             price: 2190000, cat:'Puma'   },
{ code:'SP1067', name:'Puma Speedcat OG Black White',               price: 2290000, cat:'Puma'   },
{ code:'SP1068', name:'Puma Palermo Leather White Clover',          price: 2190000, cat:'Puma'   },
{ code:'SP1069', name:'Áo thun Puma ESS+ Embroidery Tee Đen',       price:  690000, cat:'Puma'   },
{ code:'SP1070', name:'Áo hoodie Puma Essentials Big Logo Xám',     price: 1290000, cat:'Puma'   },
{ code:'SP1071', name:'Quần jogger Puma Essentials Logo Đen',       price:  890000, cat:'Puma'   },
{ code:'SP1072', name:'Bộ tracksuit Puma Power Full-Zip Đỏ Đen',    price: 1690000, cat:'Puma'   },
{ code:'SP1073', name:'Mũ Puma Essentials Cap Đen',                 price:  390000, cat:'Puma'   },

// ── New Balance (SP1074–SP1084) ───────────────
{ code:'SP1074', name:'New Balance 574 Grey Navy',                  price: 2290000, cat:'New Balance' },
{ code:'SP1075', name:'New Balance 990v6 Made in USA Grey',         price: 5990000, cat:'New Balance' },
{ code:'SP1076', name:'New Balance 1906R Protection Pack White',    price: 3490000, cat:'New Balance' },
{ code:'SP1077', name:'New Balance 9060 Slate Blue',               price: 3290000, cat:'New Balance' },
{ code:'SP1078', name:'New Balance 530 White Silver Navy',         price: 2490000, cat:'New Balance' },
{ code:'SP1079', name:'New Balance 2002R Protection Pack Lunar',   price: 3690000, cat:'New Balance' },
{ code:'SP1080', name:'New Balance 327 White Black Gum',           price: 2190000, cat:'New Balance' },
{ code:'SP1081', name:'New Balance 550 White Green',               price: 2490000, cat:'New Balance' },
{ code:'SP1082', name:'New Balance Numeric 480 Black White',       price: 2390000, cat:'New Balance' },
{ code:'SP1083', name:'Áo thun New Balance Essentials Stacked',    price:  790000, cat:'New Balance' },
{ code:'SP1084', name:'Quần jogger New Balance Essentials Đen',    price:  990000, cat:'New Balance' },

// ── Converse (SP1085–SP1092) ──────────────────
{ code:'SP1085', name:'Converse Chuck Taylor All Star Low White',   price: 1490000, cat:'Converse' },
{ code:'SP1086', name:'Converse Chuck Taylor All Star Low Black',   price: 1490000, cat:'Converse' },
{ code:'SP1087', name:'Converse Chuck 70 High Vintage Canvas',     price: 1890000, cat:'Converse' },
{ code:'SP1088', name:'Converse Run Star Hike Low Black White',    price: 2190000, cat:'Converse' },
{ code:'SP1089', name:'Converse One Star Ox Suede Navy',           price: 1690000, cat:'Converse' },
{ code:'SP1090', name:'Converse Pro Leather Low White',            price: 1790000, cat:'Converse' },
{ code:'SP1091', name:'Converse Chuck Taylor Lugged Platform White',price: 1990000, cat:'Converse' },
{ code:'SP1092', name:'Converse Run Star Legacy Cx Low Black',     price: 2090000, cat:'Converse' },

// ── Vans (SP1093–SP1100) ──────────────────────
{ code:'SP1093', name:'Vans Old Skool Classic Black White',         price: 1790000, cat:'Vans'    },
{ code:'SP1094', name:'Vans Old Skool Stackform White Gum',         price: 1990000, cat:'Vans'    },
{ code:'SP1095', name:'Vans Sk8-Hi Black White',                    price: 1990000, cat:'Vans'    },
{ code:'SP1096', name:'Vans Era Classic Navy White',                price: 1590000, cat:'Vans'    },
{ code:'SP1097', name:'Vans Authentic Triple Black',                price: 1490000, cat:'Vans'    },
{ code:'SP1098', name:'Vans Slip-On Checkerboard',                  price: 1590000, cat:'Vans'    },
{ code:'SP1099', name:'Vans Knu Skool True White Gum',             price: 2190000, cat:'Vans'    },
{ code:'SP1100', name:'Vans UltraRange EXO Hi MTE-1 Black',        price: 2490000, cat:'Vans'    },

// ── Uniqlo (SP1101–SP1115) ────────────────────
{ code:'SP1101', name:'Áo thun Uniqlo UT Graphic Tee Trắng',        price:  299000, cat:'Uniqlo'  },
{ code:'SP1102', name:'Áo thun Uniqlo Airism Crew Neck Đen',        price:  299000, cat:'Uniqlo'  },
{ code:'SP1103', name:'Áo thun Uniqlo Supima Cotton Xanh Navy',     price:  399000, cat:'Uniqlo'  },
{ code:'SP1104', name:'Áo sơ mi Uniqlo Oxford Button-Down Trắng',   price:  699000, cat:'Uniqlo'  },
{ code:'SP1105', name:'Áo sơ mi Uniqlo Extra Fine Cotton Xanh',     price:  799000, cat:'Uniqlo'  },
{ code:'SP1106', name:'Áo khoác Uniqlo Ultra Light Down Jacket Đen',price: 1490000, cat:'Uniqlo'  },
{ code:'SP1107', name:'Áo hoodie Uniqlo Sweat Full-Zip Xám',        price:  699000, cat:'Uniqlo'  },
{ code:'SP1108', name:'Quần kaki Uniqlo Slim-Fit Chino Beige',      price:  699000, cat:'Uniqlo'  },
{ code:'SP1109', name:'Quần jeans Uniqlo Slim-Fit Tapered Xanh',    price:  799000, cat:'Uniqlo'  },
{ code:'SP1110', name:'Quần jeans Uniqlo Ultra Stretch Skinny Đen', price:  799000, cat:'Uniqlo'  },
{ code:'SP1111', name:'Áo len Uniqlo Extra Fine Merino Crew Xám',   price:  799000, cat:'Uniqlo'  },
{ code:'SP1112', name:'Áo polo Uniqlo Dry-EX Short Sleeve Trắng',   price:  499000, cat:'Uniqlo'  },
{ code:'SP1113', name:'Áo phao Uniqlo Hybrid Down Parka Đen',       price: 1890000, cat:'Uniqlo'  },
{ code:'SP1114', name:'Quần short Uniqlo Airism Active Đen',        price:  399000, cat:'Uniqlo'  },
{ code:'SP1115', name:'Áo blazer Uniqlo Wool Blend Navy',           price: 1490000, cat:'Uniqlo'  },

// ── Zara (SP1116–SP1128) ──────────────────────
{ code:'SP1116', name:'Áo sơ mi Zara Basic Poplin Trắng',           price:  690000, cat:'Zara'    },
{ code:'SP1117', name:'Áo sơ mi Zara Linen Blend Xanh Dương',       price:  790000, cat:'Zara'    },
{ code:'SP1118', name:'Áo thun Zara Essential Cotton Đen',          price:  490000, cat:'Zara'    },
{ code:'SP1119', name:'Áo khoác Zara Faux Leather Biker Đen',       price: 1990000, cat:'Zara'    },
{ code:'SP1120', name:'Quần tây Zara Straight Cut Slim Beige',      price: 1190000, cat:'Zara'    },
{ code:'SP1121', name:'Quần jeans Zara Z1975 Straight Xanh',        price: 1290000, cat:'Zara'    },
{ code:'SP1122', name:'Đầm Zara Satin Effect Midi Đen',             price: 1490000, cat:'Zara'    },
{ code:'SP1123', name:'Đầm Zara Floral Print Mini Hoa',             price: 1290000, cat:'Zara'    },
{ code:'SP1124', name:'Áo blazer Zara Textured Unstructured Kem',   price: 1890000, cat:'Zara'    },
{ code:'SP1125', name:'Chân váy Zara Pleated Midi Skirt Be',        price:  990000, cat:'Zara'    },
{ code:'SP1126', name:'Túi Zara Tote City Bag Da Đen',              price: 1590000, cat:'Zara'    },
{ code:'SP1127', name:'Giày cao gót Zara Heeled Mule Đen',          price: 1290000, cat:'Zara'    },
{ code:'SP1128', name:'Sandal Zara Flat Platform Leather Be',       price:  990000, cat:'Zara'    },

// ── H&M (SP1129–SP1140) ───────────────────────
{ code:'SP1129', name:'Áo thun H&M Divided Relaxed Fit Trắng',      price:  299000, cat:'H&M'     },
{ code:'SP1130', name:'Áo sơ mi H&M Slim Fit Oxford Xanh',          price:  499000, cat:'H&M'     },
{ code:'SP1131', name:'Áo hoodie H&M Oversized Printed Đen',        price:  599000, cat:'H&M'     },
{ code:'SP1132', name:'Quần jeans H&M Slim Jeans Xanh Đậm',         price:  699000, cat:'H&M'     },
{ code:'SP1133', name:'Quần kaki H&M Chino Regular Fit Beige',      price:  599000, cat:'H&M'     },
{ code:'SP1134', name:'Đầm H&M Wrap Dress Floral Print',            price:  799000, cat:'H&M'     },
{ code:'SP1135', name:'Áo khoác H&M Biker Jacket Đen',              price: 1290000, cat:'H&M'     },
{ code:'SP1136', name:'Chân váy H&M A-Line Midi Skirt Be',          price:  599000, cat:'H&M'     },
{ code:'SP1137', name:'Áo blazer H&M Single-Breasted Camel',        price:  999000, cat:'H&M'     },
{ code:'SP1138', name:'Áo len H&M Rib-Knit Sweater Xám',            price:  499000, cat:'H&M'     },
{ code:'SP1139', name:'Túi H&M Shopper Canvas Tote Đen',            price:  399000, cat:'H&M'     },
{ code:'SP1140', name:'Mũ H&M Cotton Baseball Cap Đen',             price:  299000, cat:'H&M'     },

// ── Gucci (SP1141–SP1153) ─────────────────────
{ code:'SP1141', name:'Giày Gucci Ace GG Supreme Low Trắng',        price:18990000, cat:'Gucci'   },
{ code:'SP1142', name:'Giày Gucci Rhyton Leather Trắng',            price:21990000, cat:'Gucci'   },
{ code:'SP1143', name:'Sneaker Gucci Screener GG Beige',            price:19990000, cat:'Gucci'   },
{ code:'SP1144', name:'Sandal Gucci Horsebit Slide Da Đen',         price:14990000, cat:'Gucci'   },
{ code:'SP1145', name:'Dép Gucci Slide Logo Rubber Trắng',          price: 9990000, cat:'Gucci'   },
{ code:'SP1146', name:'Túi Gucci Ophidia GG Small Tote Bag',        price:39990000, cat:'Gucci'   },
{ code:'SP1147', name:'Túi Gucci Marmont Mini Quilted Hồng',        price:34990000, cat:'Gucci'   },
{ code:'SP1148', name:'Túi Gucci Bamboo 1947 Small Top Handle',     price:52990000, cat:'Gucci'   },
{ code:'SP1149', name:'Ví Gucci GG Marmont Zip Around',             price:12990000, cat:'Gucci'   },
{ code:'SP1150', name:'Thắt lưng Gucci GG Buckle Belt Đen',         price: 8990000, cat:'Gucci'   },
{ code:'SP1151', name:'Áo thun Gucci Cotton Jersey Logo Đen',       price:11990000, cat:'Gucci'   },
{ code:'SP1152', name:'Áo polo Gucci Piqué Striped Trắng Xanh',     price:14990000, cat:'Gucci'   },
{ code:'SP1153', name:'Nước hoa Gucci Bloom EDP 100ml',             price:4990000,  cat:'Gucci'   },

// ── Louis Vuitton (SP1154–SP1163) ─────────────
{ code:'SP1154', name:'Túi LV Neverfull MM Monogram Beige',         price:62990000, cat:'LV'      },
{ code:'SP1155', name:'Túi LV Speedy 30 Monogram Canvas',           price:48990000, cat:'LV'      },
{ code:'SP1156', name:'Túi LV Pochette Métis Reverse Monogram',     price:72990000, cat:'LV'      },
{ code:'SP1157', name:'Ví LV Zippy Wallet Monogram Canvas',         price:18990000, cat:'LV'      },
{ code:'SP1158', name:'Ví LV Slender Wallet Taiga Da Đen',          price:14990000, cat:'LV'      },
{ code:'SP1159', name:'Thắt lưng LV Initiales 40mm Monogram',       price:12990000, cat:'LV'      },
{ code:'SP1160', name:'Giày LV Archlight Sneaker Trắng Đỏ',         price:29990000, cat:'LV'      },
{ code:'SP1161', name:'Giày LV Trainer Monogram Black',             price:26990000, cat:'LV'      },
{ code:'SP1162', name:'Sandal LV Lock It Mule Monogram',            price:19990000, cat:'LV'      },
{ code:'SP1163', name:'Nước hoa LV Coeur Battant EDP 100ml',        price:7990000,  cat:'LV'      },

// ── Balenciaga / Off-White / Stone Island (SP1164–SP1175) ─
{ code:'SP1164', name:'Giày Balenciaga Triple S Black White',        price:22990000, cat:'Balenciaga'},
{ code:'SP1165', name:'Giày Balenciaga Speed Runner Knit Black',    price:18990000, cat:'Balenciaga'},
{ code:'SP1166', name:'Giày Balenciaga Track LED Neon',             price:26990000, cat:'Balenciaga'},
{ code:'SP1167', name:'Túi Balenciaga Le Cagole Shoulder Bag Đen',  price:54990000, cat:'Balenciaga'},
{ code:'SP1168', name:'Áo hoodie Balenciaga Political Logo Đen',    price:24990000, cat:'Balenciaga'},
{ code:'SP1169', name:'Giày Off-White Out Of Office White Blue',    price:12990000, cat:'Off-White' },
{ code:'SP1170', name:'Giày Off-White Vulc Low Arrow White',        price:10990000, cat:'Off-White' },
{ code:'SP1171', name:'Áo thun Off-White Caravaggio Tee Trắng',     price: 8990000, cat:'Off-White' },
{ code:'SP1172', name:'Áo khoác Stone Island Nylon Metal Xanh Lá',  price:16990000, cat:'Stone Island'},
{ code:'SP1173', name:'Áo khoác Stone Island Ghost Piece Đen',      price:18990000, cat:'Stone Island'},
{ code:'SP1174', name:'Áo thun Stone Island Embossed Logo Trắng',   price: 4990000, cat:'Stone Island'},
{ code:'SP1175', name:'Quần Stone Island Ghost Piece Cargo Xám',    price:12990000, cat:'Stone Island'},

// ── Lacoste / Ralph Lauren / Tommy (SP1176–SP1187) ─
{ code:'SP1176', name:'Áo polo Lacoste L.12.12 Classic Trắng',      price: 2490000, cat:'Lacoste' },
{ code:'SP1177', name:'Áo polo Lacoste L.12.12 Classic Navy',       price: 2490000, cat:'Lacoste' },
{ code:'SP1178', name:'Áo polo Lacoste Slim Fit Xanh Dương',        price: 2290000, cat:'Lacoste' },
{ code:'SP1179', name:'Giày Lacoste Carnaby Evo Leather Trắng',     price: 2990000, cat:'Lacoste' },
{ code:'SP1180', name:'Áo polo Ralph Lauren Classic Fit Trắng',     price: 2190000, cat:'Ralph Lauren'},
{ code:'SP1181', name:'Áo polo Ralph Lauren Slim Fit Navy',         price: 2190000, cat:'Ralph Lauren'},
{ code:'SP1182', name:'Áo khoác Ralph Lauren Windbreaker Xanh',     price: 3990000, cat:'Ralph Lauren'},
{ code:'SP1183', name:'Quần kaki Ralph Lauren Classic Fit Khaki',   price: 1890000, cat:'Ralph Lauren'},
{ code:'SP1184', name:'Áo polo Tommy Hilfiger Slim Fit Đỏ Trắng',   price: 1890000, cat:'Tommy'   },
{ code:'SP1185', name:'Áo sơ mi Tommy Hilfiger Poplin Trắng',       price: 1690000, cat:'Tommy'   },
{ code:'SP1186', name:'Quần jeans Tommy Hilfiger Scanton Slim Xanh',price: 1990000, cat:'Tommy'   },
{ code:'SP1187', name:'Túi Tommy Hilfiger Tote Monogram Navy',      price: 1490000, cat:'Tommy'   },

// ── Dép / Sandal Cao Cấp (SP1188–SP1198) ─────
{ code:'SP1188', name:'Dép Birkenstock Arizona Leather Đen',         price: 2990000, cat:'Dép hiệu'},
{ code:'SP1189', name:'Dép Birkenstock Boston Suede Mocha',          price: 3290000, cat:'Dép hiệu'},
{ code:'SP1190', name:'Sandal Teva Original Universal Xanh',         price: 1490000, cat:'Dép hiệu'},
{ code:'SP1191', name:'Dép Crocs Classic Clog Electric Blue',        price: 1290000, cat:'Dép hiệu'},
{ code:'SP1192', name:'Dép Crocs Literide 360 Clog Xám',             price: 1490000, cat:'Dép hiệu'},
{ code:'SP1193', name:'Sandal Reef Cushion Vista Nâu',               price: 1690000, cat:'Dép hiệu'},
{ code:'SP1194', name:'Dép cao gót Nine West Peep Toe Đen',          price: 1890000, cat:'Dép hiệu'},
{ code:'SP1195', name:'Giày Steve Madden Leather Ankle Boot Đen',    price: 3490000, cat:'Dép hiệu'},
{ code:'SP1196', name:'Sandal Dr. Martens Blaire Leather Đen',       price: 3990000, cat:'Dép hiệu'},
{ code:'SP1197', name:'Boot Dr. Martens 1460 Pascal Đen',            price: 5490000, cat:'Dép hiệu'},
{ code:'SP1198', name:'Giày UGG Classic Short II Boot Nâu',          price: 4990000, cat:'Dép hiệu'},

// ── Phụ kiện thời trang (SP1199–SP1200) ──────
{ code:'SP1199', name:'Kính mắt Ray-Ban Aviator Classic Gold G-15',  price: 3490000, cat:'Phụ kiện thời trang'},
{ code:'SP1200', name:'Kính mắt Ray-Ban Wayfarer Classic Black',     price: 3190000, cat:'Phụ kiện thời trang'},

];

/* Lookup map: mã SP -> object */
const PRODUCT_MAP = {};
PRODUCTS.forEach(p => { PRODUCT_MAP[p.code] = p; });


/* ══════════════════════════════════════════════
   STATE
══════════════════════════════════════════════ */
let invoiceItems  = [];    /* [{code,name,price,qty}] */
let invoiceTotal  = 0;     /* tổng cộng đã tính */
let payMethod     = 'cash';
let invoiceNumber = 1;
let invoicePaid   = false;


/* ══════════════════════════════════════════════
   KHỞI TẠO KHI DOM READY
══════════════════════════════════════════════ */
document.addEventListener('DOMContentLoaded', () => {
  updateHeaderDate();
  setInterval(updateHeaderDate, 1000); /* đồng hồ realtime */
  loadHomeStats();

  /* Điền ngày giờ hiện tại vào form */
  const now = new Date();
  document.getElementById('inv-date').value = now.toISOString().slice(0, 10);
  document.getElementById('inv-hour').value = now.getHours();
  document.getElementById('inv-min').value  = now.getMinutes();
  document.getElementById('inv-sec').value  = now.getSeconds();

  /* Lấy số hóa đơn tiếp theo từ localStorage */
  const savedCounter = localStorage.getItem('tmart_inv_counter');
  if (savedCounter) invoiceNumber = parseInt(savedCounter) + 1;
  updateInvNumberDisplay();
});

/* Cập nhật đồng hồ header */
function updateHeaderDate() {
  const el = document.getElementById('header-date');
  if (!el) return;
  const now = new Date();
  el.innerHTML =
    `<strong>${now.toLocaleDateString('vi-VN',{weekday:'long'})}</strong>, ` +
    `${now.toLocaleDateString('vi-VN')} | ${now.toLocaleTimeString('vi-VN')}`;
}

/* Hiện số hóa đơn */
function updateInvNumberDisplay() {
  document.getElementById('inv-number-display').textContent =
    '#HD' + String(invoiceNumber).padStart(4, '0');
}


/* ══════════════════════════════════════════════
   ĐIỀU HƯỚNG TRANG
══════════════════════════════════════════════ */
function goToInvoice() {
  document.getElementById('page-home').classList.add('hidden');
  document.getElementById('page-invoice').classList.remove('hidden');
  window.scrollTo(0, 0);
}

function goHome() {
  document.getElementById('page-invoice').classList.add('hidden');
  document.getElementById('page-home').classList.remove('hidden');
  loadHomeStats();
  window.scrollTo(0, 0);
}


/* ══════════════════════════════════════════════
   AUTOCOMPLETE SẢN PHẨM
══════════════════════════════════════════════ */
let acActiveCode  = -1; /* chỉ số dòng đang active trong dropdown mã */
let acActiveName  = -1; /* chỉ số dòng đang active trong dropdown tên */
let selectedProduct = null; /* sản phẩm hiện được chọn để thêm */

/* Khi gõ vào ô Mã SP */
function onCodeInput(el) {
  const q = el.value.trim().toUpperCase();
  const drop = document.getElementById('code-drop');

  if (!q) { closeDrop(drop); clearPreview(); selectedProduct = null; return; }

  /* Khớp chính xác → tự điền luôn */
  if (PRODUCT_MAP[q]) {
    selectedProduct = PRODUCT_MAP[q];
    document.getElementById('search-name').value = selectedProduct.name;
    showPreview(selectedProduct);
    closeDrop(drop);
    return;
  }

  /* Tìm gần đúng */
  const qLow = q.toLowerCase();
  const matches = PRODUCTS.filter(p =>
    p.code.toLowerCase().includes(qLow) || p.name.toLowerCase().includes(qLow)
  ).slice(0, 8);
  renderDrop(drop, matches);
  acActiveCode = -1;
}

/* Khi gõ vào ô Tên SP */
function onNameInput(el) {
  const q = el.value.trim().toLowerCase();
  const drop = document.getElementById('name-drop');
  clearPreview();
  selectedProduct = null;
  document.getElementById('search-code').value = '';

  if (!q) { closeDrop(drop); return; }

  const matches = PRODUCTS.filter(p =>
    p.name.toLowerCase().includes(q) ||
    p.code.toLowerCase().includes(q)  ||
    p.cat.toLowerCase().includes(q)
  ).slice(0, 8);
  renderDrop(drop, matches);
  acActiveName = -1;
}

/* Vẽ dropdown */
function renderDrop(drop, items) {
  if (!items.length) { closeDrop(drop); return; }
  drop.innerHTML = items.map(p => `
    <div class="autocomplete-item" data-code="${p.code}" onclick="selectFromDrop('${p.code}')">
      <div style="min-width:0">
        <span>${p.name}</span><br>
        <small>${p.code} · ${p.cat}</small>
      </div>
      <div class="autocomplete-item-price">${fmtMoney(p.price)}</div>
    </div>`).join('');
  drop.classList.remove('hidden');
}

/* Chọn sản phẩm từ dropdown */
function selectFromDrop(code) {
  const p = PRODUCT_MAP[code];
  if (!p) return;
  selectedProduct = p;
  document.getElementById('search-code').value = p.code;
  document.getElementById('search-name').value = p.name;
  showPreview(p);
  closeDrop(document.getElementById('code-drop'));
  closeDrop(document.getElementById('name-drop'));
  document.getElementById('search-qty').focus();
  document.getElementById('search-qty').select();
}

function closeDrop(el) { el.classList.add('hidden'); el.innerHTML = ''; }

function showPreview(p) {
  document.getElementById('search-preview').innerHTML =
    `<span class="badge badge-blue">${p.cat}</span>&nbsp; Đơn giá: ` +
    `<strong style="color:var(--accent);font-family:var(--font-mono)">${fmtMoney(p.price)}</strong>`;
}
function clearPreview() { document.getElementById('search-preview').innerHTML = ''; }

/* Điều hướng bàn phím trong dropdown */
function onCodeKey(e) { handleDropKey(e, 'code-drop', v => acActiveCode = v, () => acActiveCode); }
function onNameKey(e) { handleDropKey(e, 'name-drop', v => acActiveName = v, () => acActiveName); }

function handleDropKey(e, dropId, setIdx, getIdx) {
  const drop = document.getElementById(dropId);
  if (drop.classList.contains('hidden')) {
    if (e.key === 'Enter') addProduct();
    return;
  }
  const items = drop.querySelectorAll('.autocomplete-item');
  if (!items.length) return;
  let idx = getIdx();

  if (e.key === 'ArrowDown') {
    e.preventDefault();
    idx = Math.min(idx + 1, items.length - 1);
  } else if (e.key === 'ArrowUp') {
    e.preventDefault();
    idx = Math.max(idx - 1, 0);
  } else if (e.key === 'Enter') {
    e.preventDefault();
    if (idx >= 0) items[idx].click(); else addProduct();
    return;
  } else if (e.key === 'Escape') {
    closeDrop(drop); return;
  }
  setIdx(idx);
  items.forEach((el, i) => el.classList.toggle('active', i === idx));
}

/* Đóng dropdown khi click ra ngoài */
document.addEventListener('click', e => {
  if (!e.target.closest('.autocomplete-wrapper')) {
    document.querySelectorAll('.autocomplete-drop').forEach(d => closeDrop(d));
  }
});


/* ══════════════════════════════════════════════
   THÊM SẢN PHẨM VÀO HÓA ĐƠN
══════════════════════════════════════════════ */
function addProduct() {
  /* Nếu chưa chọn qua dropdown, thử tìm theo mã nhập */
  if (!selectedProduct) {
    const code = document.getElementById('search-code').value.trim().toUpperCase();
    if (code && PRODUCT_MAP[code]) selectedProduct = PRODUCT_MAP[code];
  }

  if (!selectedProduct) {
    showToast('Vui lòng chọn sản phẩm hợp lệ!', 'error');
    return;
  }

  const qty = parseInt(document.getElementById('search-qty').value) || 1;
  if (qty <= 0) { showToast('Số lượng phải lớn hơn 0!', 'error'); return; }

  /* Kiểm tra trùng: cộng dồn số lượng */
  const existing = invoiceItems.find(i => i.code === selectedProduct.code);
  if (existing) {
    existing.qty += qty;
    showToast(`Đã cộng +${qty} vào: ${selectedProduct.name}`, 'success');
  } else {
    invoiceItems.push({
      code: selectedProduct.code,
      name: selectedProduct.name,
      price: selectedProduct.price,
      qty,
    });
    showToast(`Đã thêm: ${selectedProduct.name}`, 'success');
  }

  renderTable();
  recalcTotal();

  /* Reset ô nhập */
  document.getElementById('search-code').value = '';
  document.getElementById('search-name').value = '';
  document.getElementById('search-qty').value  = 1;
  clearPreview();
  selectedProduct = null;
  document.getElementById('search-code').focus();
}

/* Xóa một dòng sản phẩm */
function removeItem(idx) {
  invoiceItems.splice(idx, 1);
  renderTable();
  recalcTotal();
}

/* Thay đổi số lượng trực tiếp trong bảng */
function changeQty(idx, val) {
  const qty = parseInt(val);
  if (!isNaN(qty) && qty > 0) {
    invoiceItems[idx].qty = qty;
    /* Cập nhật ô tổng của dòng đó */
    const row = document.querySelector(`tr[data-idx="${idx}"]`);
    if (row) row.querySelector('.row-total').textContent = fmtMoney(invoiceItems[idx].price * qty);
    recalcTotal();
  }
}


/* ══════════════════════════════════════════════
   RENDER BẢNG HÓA ĐƠN
══════════════════════════════════════════════ */
function renderTable() {
  const tbody = document.getElementById('inv-tbody');

  if (!invoiceItems.length) {
    tbody.innerHTML = `
      <tr><td colspan="7" class="table-empty">
        <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
          <path d="M6 2L3 6v14a2 2 0 002 2h14a2 2 0 002-2V6l-3-4z"/>
          <line x1="3" y1="6" x2="21" y2="6"/>
          <path d="M16 10a4 4 0 01-8 0"/>
        </svg>
        Chưa có sản phẩm nào. Hãy thêm sản phẩm vào hóa đơn.
      </td></tr>`;
    return;
  }

  tbody.innerHTML = invoiceItems.map((item, i) => `
    <tr data-idx="${i}">
      <td style="color:var(--text-3);font-size:13px;font-weight:600">${i + 1}</td>
      <td class="code">${item.code}</td>
      <td style="font-weight:500;color:var(--text)">${item.name}</td>
      <td class="price">${fmtMoney(item.price)}</td>
      <td style="text-align:center">
        <input class="qty-input" type="number" min="1" value="${item.qty}"
          onchange="changeQty(${i}, this.value)"
          oninput="changeQty(${i}, this.value)" />
      </td>
      <td class="total row-total">${fmtMoney(item.price * item.qty)}</td>
      <td><button class="btn-del" title="Xóa dòng" onclick="removeItem(${i})">×</button></td>
    </tr>`).join('');
}


/* ══════════════════════════════════════════════
   TÍNH TIỀN
══════════════════════════════════════════════ */
function recalcTotal() {
  /* Tổng sản phẩm */
  const subtotal = invoiceItems.reduce((sum, item) => sum + item.price * item.qty, 0);

  /* Chiết khấu */
  const discount = Math.max(0, parseFloat(document.getElementById('pay-discount').value) || 0);

  /* Thuế VAT */
  const vatPct = Math.max(0, parseFloat(document.getElementById('pay-vat').value) || 0);
  const vatAmt = Math.round((subtotal - discount) * vatPct / 100);

  /* Tổng cộng */
  const total = Math.max(0, subtotal - discount + vatAmt);
  invoiceTotal = total;

  /* Cập nhật UI */
  document.getElementById('subtotal-display').textContent = fmtMoney(subtotal);
  document.getElementById('pay-subtotal').textContent     = fmtMoney(subtotal);
  document.getElementById('pay-total').textContent        = fmtMoney(total);

  recalcChange();
}

function recalcChange() {
  const given  = parseFloat(document.getElementById('pay-given').value) || 0;
  const change = given - invoiceTotal;
  const el = document.getElementById('pay-change');
  el.textContent = fmtMoney(Math.max(0, change));
  el.style.color = change >= 0 ? 'var(--green)' : 'var(--red)';
}

/* Chọn hình thức thanh toán */
function selectPayMethod(method, btn) {
  payMethod = method;
  document.querySelectorAll('.pay-method-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
}


/* ══════════════════════════════════════════════
   THANH TOÁN
══════════════════════════════════════════════ */
function checkout() {
  if (!invoiceItems.length) {
    showToast('Hóa đơn chưa có sản phẩm!', 'error'); return;
  }
  if (invoiceTotal <= 0) {
    showToast('Tổng cộng phải lớn hơn 0!', 'error'); return;
  }
  const given  = parseFloat(document.getElementById('pay-given').value) || 0;
  if (payMethod === 'cash' && given > 0 && given < invoiceTotal) {
    showToast('Số tiền khách đưa chưa đủ!', 'error'); return;
  }

  /* Lưu hóa đơn vào localStorage */
  saveInvoice();

  invoicePaid = true;
  document.getElementById('inv-status-badge').textContent = '✓ Đã thanh toán';
  document.getElementById('inv-status-badge').className   = 'badge badge-green';
  showToast('✓ Thanh toán thành công! Hóa đơn đã được lưu.', 'success');
}

/* Lưu hóa đơn vào localStorage */
function saveInvoice() {
  const now   = new Date();
  const staff = document.getElementById('inv-staff').value.trim() || 'Nhân viên';
  const month = `${now.getFullYear()}-${String(now.getMonth()+1).padStart(2,'0')}`;
  const methodLabel = { cash:'Tiền mặt', transfer:'Chuyển khoản', ewallet:'Ví điện tử' };

  /* Ghi vào danh sách hóa đơn */
  const allInv = JSON.parse(localStorage.getItem('tmart_invoices') || '[]');
  allInv.unshift({
    number : invoiceNumber,
    date   : now.toLocaleDateString('vi-VN'),
    time   : now.toLocaleTimeString('vi-VN'),
    staff,
    total  : invoiceTotal,
    method : payMethod,
    methodLabel: methodLabel[payMethod],
    month,
    items  : JSON.parse(JSON.stringify(invoiceItems)),
  });
  localStorage.setItem('tmart_invoices', JSON.stringify(allInv));

  /* Cộng doanh thu tháng */
  const revKey = `tmart_revenue_${month}`;
  const curRev = parseFloat(localStorage.getItem(revKey) || '0');
  localStorage.setItem(revKey, curRev + invoiceTotal);

  /* Lưu counter */
  localStorage.setItem('tmart_inv_counter', invoiceNumber);
  invoiceNumber++;
}


/* ══════════════════════════════════════════════
   TẠO MỚI HÓA ĐƠN
══════════════════════════════════════════════ */
function resetInvoice() {
  if (invoiceItems.length && !invoicePaid) {
    if (!confirm('Hóa đơn chưa thanh toán. Bạn có chắc muốn tạo hóa đơn mới không?')) return;
  }

  invoiceItems = [];
  invoicePaid  = false;

  renderTable();
  recalcTotal();

  /* Reset form */
  document.getElementById('inv-staff').value    = '';
  document.getElementById('pay-discount').value = 0;
  document.getElementById('pay-vat').value      = 0;
  document.getElementById('pay-given').value    = 0;
  document.getElementById('pay-change').textContent = '0 ₫';
  document.getElementById('pay-change').style.color = 'var(--green)';

  /* Cập nhật ngày giờ */
  const now = new Date();
  document.getElementById('inv-date').value = now.toISOString().slice(0, 10);
  document.getElementById('inv-hour').value = now.getHours();
  document.getElementById('inv-min').value  = now.getMinutes();
  document.getElementById('inv-sec').value  = now.getSeconds();

  /* Badge */
  document.getElementById('inv-status-badge').textContent = 'Đang nhập';
  document.getElementById('inv-status-badge').className   = 'badge badge-blue';

  updateInvNumberDisplay();
  showToast('Đã tạo hóa đơn mới', 'success');
}


/* ══════════════════════════════════════════════
   TRANG CHỦ – THỐNG KÊ
══════════════════════════════════════════════ */
function loadHomeStats() {
  const now   = new Date();
  const month = `${now.getFullYear()}-${String(now.getMonth()+1).padStart(2,'0')}`;
  const revKey = `tmart_revenue_${month}`;

  /* Doanh thu */
  const revenue  = parseFloat(localStorage.getItem(revKey) || '0');
  const allInv   = JSON.parse(localStorage.getItem('tmart_invoices') || '[]');
  const thisMonth = allInv.filter(inv => inv.month === month);

  document.getElementById('home-revenue').textContent       = fmtMoney(revenue);
  document.getElementById('home-invoice-count').textContent = thisMonth.length;
  document.getElementById('home-avg').textContent           =
    thisMonth.length ? fmtMoney(Math.round(revenue / thisMonth.length)) : '0 ₫';
  document.getElementById('home-revenue-month').textContent =
    `Tháng ${now.getMonth()+1} / ${now.getFullYear()}`;

  /* Bảng hóa đơn gần nhất */
  const wrap = document.getElementById('recent-table-wrap');
  if (!thisMonth.length) {
    wrap.innerHTML = '<div class="recent-empty"><svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M14 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V8z"/><polyline points="14 2 14 8 20 8"/></svg>Chưa có hóa đơn nào trong tháng này</div>';
    return;
  }

  const rows = thisMonth.slice(0, 8).map(inv => `
    <tr>
      <td>#HD${String(inv.number).padStart(4,'0')}</td>
      <td>${inv.date} ${inv.time}</td>
      <td>${inv.staff}</td>
      <td><span class="badge badge-blue">${inv.methodLabel || inv.method}</span></td>
      <td class="amount">${fmtMoney(inv.total)}</td>
    </tr>`).join('');

  wrap.innerHTML = `
    <table class="recent-table">
      <thead><tr>
        <th>Số HĐ</th><th>Thời gian</th><th>Nhân viên</th><th>Hình thức</th><th>Tổng tiền</th>
      </tr></thead>
      <tbody>${rows}</tbody>
    </table>`;
}


/* ══════════════════════════════════════════════
   XUẤT PDF VỚI jsPDF + AUTOTABLE
══════════════════════════════════════════════ */
function exportPDF() {
  if (!invoiceItems.length) {
    showToast('Hóa đơn chưa có sản phẩm!', 'error'); return;
  }

  const { jsPDF } = window.jspdf;
  const doc = new jsPDF({ unit:'mm', format:'a5', orientation:'portrait' });
  const W = doc.internal.pageSize.getWidth();
  let y = 12;

  /* Hàm chuyển ký tự tiếng Việt -> ASCII để PDF hiển thị đúng */
  const safe = s => (s || '').toString()
    .replace(/[àáâãäåắặằẩẫấ]/gi,'a').replace(/[èéêëẻẽẹếệềểễ]/gi,'e')
    .replace(/[ìíîïịỉĩ]/gi,'i').replace(/[òóôõöọổỏộổỗốợờởỡớ]/gi,'o')
    .replace(/[ùúûüụủũưứựừửữ]/gi,'u').replace(/[ýÿ]/gi,'y')
    .replace(/[đĐ]/g,'d').replace(/[ăẮẶẰẨẪẤ]/gi,'a')
    .replace(/Đ/g,'D').replace(/đ/g,'d');

  /* ── Header xanh ─────────────────────── */
  doc.setFillColor(0, 87, 255);
  doc.rect(0, 0, W, 28, 'F');

  doc.setTextColor(255, 214, 0);
  doc.setFontSize(24); doc.setFont('helvetica', 'bold');
  doc.text('T-MART', W/2, 11, { align:'center' });

  doc.setTextColor(255, 255, 255);
  doc.setFontSize(7); doc.setFont('helvetica', 'normal');
  doc.text('33C Ngo Cho Kham Thien, Van Mieu - Quoc Tu Giam, Ha Noi', W/2, 17.5, { align:'center' });
  doc.text('Hotline: 1900-9999', W/2, 22.5, { align:'center' });

  y = 34;

  /* ── Tiêu đề hóa đơn ──────────────────── */
  doc.setTextColor(20, 20, 20);
  doc.setFontSize(13); doc.setFont('helvetica', 'bold');
  doc.text('HOA DON BAN HANG', W/2, y, { align:'center' });
  y += 6;
  doc.setFontSize(8.5); doc.setFont('helvetica', 'normal'); doc.setTextColor(120, 120, 120);
  doc.text(`So: #HD${String(invoiceNumber - 1).padStart(4, '0')}`, W/2, y, { align:'center' });
  y += 7;

  /* Đường kẻ */
  doc.setDrawColor(220, 220, 220); doc.setLineWidth(0.3);
  doc.line(10, y, W-10, y); y += 5;

  /* ── Thông tin hóa đơn ─────────────────── */
  const staff   = document.getElementById('inv-staff').value || '-';
  const dateVal = document.getElementById('inv-date').value;
  const hour    = String(document.getElementById('inv-hour').value || 0).padStart(2,'0');
  const min     = String(document.getElementById('inv-min').value  || 0).padStart(2,'0');
  const sec     = String(document.getElementById('inv-sec').value  || 0).padStart(2,'0');
  const dtStr   = `${dateVal}  ${hour}:${min}:${sec}`;
  const methodLbl = { cash:'Tien mat', transfer:'Chuyen khoan', ewallet:'Vi dien tu' };

  [[`Nhan vien:`, safe(staff)],
   [`Ngay gio:`,  dtStr],
   [`Thanh toan:`,safe(methodLbl[payMethod])],
  ].forEach(([label, val]) => {
    doc.setFont('helvetica','bold'); doc.setFontSize(8.5); doc.setTextColor(90,90,90);
    doc.text(label, 12, y);
    doc.setFont('helvetica','normal'); doc.setTextColor(20,20,20);
    doc.text(val, 58, y);
    y += 5.5;
  });

  y += 2;
  doc.setDrawColor(220,220,220); doc.line(10, y, W-10, y); y += 5;

  /* ── Bảng sản phẩm ────────────────────── */
  const tableRows = invoiceItems.map(item => [
    item.code,
    safe(item.name),
    fmtMoney(item.price),
    item.qty.toString(),
    fmtMoney(item.price * item.qty),
  ]);

  doc.autoTable({
    startY: y,
    head: [['Ma SP', 'Ten san pham', 'Don gia', 'SL', 'Thanh tien']],
    body: tableRows,
    styles: {
      font: 'helvetica', fontSize: 8,
      cellPadding: 2.8, textColor: [30,30,30],
    },
    headStyles: {
      fillColor: [0,87,255], textColor: [255,255,255],
      fontStyle: 'bold', fontSize: 8.5,
    },
    columnStyles: {
      0: { cellWidth: 18 },
      1: { cellWidth: 'auto' },
      2: { cellWidth: 24, halign: 'right' },
      3: { cellWidth: 10, halign: 'center' },
      4: { cellWidth: 26, halign: 'right', fontStyle: 'bold' },
    },
    margin: { left: 10, right: 10 },
    theme: 'striped',
    alternateRowStyles: { fillColor: [248,250,252] },
  });

  y = doc.lastAutoTable.finalY + 6;
  doc.setDrawColor(200,200,200); doc.line(10, y, W-10, y); y += 6;

  /* ── Phần tổng kết ───────────────────── */
  const subtotal = invoiceItems.reduce((s,i) => s + i.price * i.qty, 0);
  const discount = Math.max(0, parseFloat(document.getElementById('pay-discount').value) || 0);
  const vatPct   = Math.max(0, parseFloat(document.getElementById('pay-vat').value) || 0);
  const vatAmt   = Math.round((subtotal - discount) * vatPct / 100);
  const total    = Math.max(0, subtotal - discount + vatAmt);
  const given    = parseFloat(document.getElementById('pay-given').value) || 0;
  const change   = Math.max(0, given - total);

  doc.setFontSize(8.5);
  [
    ['Tong san pham:',      fmtMoney(subtotal)],
    [`Chiet khau:`,         discount > 0 ? `- ${fmtMoney(discount)}` : fmtMoney(0)],
    [`Thue VAT (${vatPct}%):`, fmtMoney(vatAmt)],
  ].forEach(([label, val]) => {
    doc.setFont('helvetica','normal'); doc.setTextColor(90,90,90);
    doc.text(label, 12, y);
    doc.setTextColor(20,20,20);
    doc.text(val, W-12, y, { align:'right' });
    y += 5.5;
  });

  /* Dòng tổng cộng nền xanh */
  doc.setFillColor(0, 87, 255);
  doc.roundedRect(10, y - 1, W-20, 9.5, 2, 2, 'F');
  doc.setFont('helvetica','bold'); doc.setFontSize(10);
  doc.setTextColor(255,255,255);
  doc.text('TONG CONG:', 14, y + 6);
  doc.text(fmtMoney(total), W-14, y + 6, { align:'right' });
  y += 14;

  doc.setFontSize(8.5); doc.setFont('helvetica','normal'); doc.setTextColor(70,70,70);
  doc.text('Khach dua:', 12, y);
  doc.text(fmtMoney(given), W-12, y, { align:'right' });
  y += 5.5;

  doc.setFont('helvetica','bold'); doc.setTextColor(0, 160, 100);
  doc.text('Tien thoi lai:', 12, y);
  doc.text(fmtMoney(change), W-12, y, { align:'right' });
  y += 9;

  /* Đường kẻ đứt */
  doc.setLineDash([1.5, 1.5]);
  doc.setDrawColor(180,180,180); doc.setLineWidth(0.3);
  doc.line(10, y, W-10, y);
  doc.setLineDash([]);
  y += 8;

  /* Lời cảm ơn */
  doc.setFontSize(10); doc.setFont('helvetica','bolditalic');
  doc.setTextColor(0, 87, 255);
  doc.text('Cam on quy khach da mua hang!', W/2, y, { align:'center' });
  y += 5.5;
  doc.setFont('helvetica','normal'); doc.setFontSize(7.5); doc.setTextColor(160,160,160);
  doc.text('T-Mart  |  1900-9999  |  33C Ngo Cho Kham Thien, Ha Noi', W/2, y, { align:'center' });

  /* Lưu file */
  const filename = `HoaDon_HD${String(invoiceNumber - 1).padStart(4,'0')}_TMart.pdf`;
  doc.save(filename);
  showToast('✓ Đã xuất PDF: ' + filename, 'success');
}


/* ══════════════════════════════════════════════
   HELPER FUNCTIONS
══════════════════════════════════════════════ */
/* Format tiền Việt Nam */
function fmtMoney(n) {
  return new Intl.NumberFormat('vi-VN', { style:'currency', currency:'VND' }).format(n || 0);
}

/* Toast thông báo */
let toastTimer = null;
function showToast(msg, type = '') {
  const el = document.getElementById('toast');
  clearTimeout(toastTimer);
  el.textContent = msg;
  el.className   = type ? `show ${type}` : 'show';
  toastTimer = setTimeout(() => { el.className = ''; }, 3200);
}
</script>

</body>
</html>
