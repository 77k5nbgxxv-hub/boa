<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>LUCYS LION DEN LLC | Business Dashboard</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

  <style>
    :root{
      --primary:#005eb8;
      --primary-dark:#003b7a;
      --primary-light:#edf5ff;
      --accent:#0a8f7d;
      --bg:#f3f7fb;
      --card:#ffffff;
      --text:#1b1b1b;
      --muted:#5c6470;
      --line:#dfe7f2;
      --danger:#d9534f;
      --warning:#f7b731;
      --shadow:0 18px 40px rgba(18,43,70,0.08);
    }
    *{box-sizing:border-box}
    html,body{margin:0;height:100%;font-family:Arial,Helvetica,sans-serif;background:var(--bg);color:var(--text)}
    a{text-decoration:none;color:inherit}
    button,input{font:inherit}
    .hidden{display:none !important}

    .login-shell{
      min-height:100vh;
      display:grid;
      grid-template-columns:1.05fr 0.95fr;
      background:linear-gradient(135deg,#eef6ff,#f8fbff);
    }

    .login-visual{
      position:relative;
      padding:48px;
      display:flex;
      align-items:center;
      justify-content:center;
      background:linear-gradient(135deg,#0a2d52,#0d4a88 48%,#0f6ac0);
      color:white;
      overflow:hidden;
    }
    .login-visual::before,.login-visual::after{
      content:"";position:absolute;border-radius:50%;background:rgba(255,255,255,0.08);
    }
    .login-visual::before{width:500px;height:500px;top:-180px;right:-120px}
    .login-visual::after{width:360px;height:360px;bottom:-120px;left:-80px}

    .visual-inner{position:relative;z-index:1;width:min(540px,100%)}
    .brand{display:flex;align-items:center;gap:12px;font-weight:700;font-size:1.4rem;margin-bottom:26px}
    .brand-mark{width:38px;height:38px;border-radius:50%;background:rgba(255,255,255,0.12);display:grid;place-items:center;border:1px solid rgba(255,255,255,0.2);font-weight:700}
    .visual-copy h1{font-size:clamp(2.2rem,4vw,4rem);line-height:1.08;letter-spacing:-0.05em;margin:0 0 18px}
    .visual-copy p{font-size:1.08rem;line-height:1.7;max-width:520px;color:rgba(255,255,255,0.82);margin-bottom:28px}
    .mini-card{
      background:rgba(255,255,255,0.08);border:1px solid rgba(255,255,255,0.18);
      border-radius:24px;padding:22px;backdrop-filter:blur(8px);
      box-shadow:0 18px 35px rgba(0,0,0,0.15);
    }
    .mini-card .row{display:flex;justify-content:space-between;color:rgba(255,255,255,0.82);margin-top:12px;font-size:0.92rem}
    .mini-card strong{font-size:2rem;display:block;margin-top:6px}

    .login-panel{background:white;display:flex;align-items:center;justify-content:center;padding:32px 24px}
    .login-box{width:min(440px,100%)}
    .login-box h2{font-size:2rem;margin:0 0 8px;letter-spacing:-0.04em}
    .subtext{color:var(--muted);margin:0 0 28px;font-size:0.98rem}
    .field{margin-bottom:18px}
    .field label{display:block;font-weight:700;margin-bottom:8px;font-size:0.9rem}
    .field input{width:100%;height:52px;border-radius:12px;border:1px solid var(--line);background:#fff;padding:0 14px;font-size:1rem;outline:none;transition:0.15s ease}
    .field input:focus{border-color:var(--primary);box-shadow:0 0 0 4px rgba(0,94,184,0.08)}
    .check-row{display:flex;justify-content:space-between;align-items:center;gap:12px;margin:8px 0 22px;font-size:0.9rem;color:var(--muted)}
    .remember{display:flex;align-items:center;gap:8px}
    .remember input{accent-color:var(--primary)}
    .link{color:var(--primary);font-weight:700;cursor:pointer}
    .primary-btn{width:100%;height:54px;border:none;border-radius:12px;background:linear-gradient(135deg,var(--primary),#0b74d3);color:white;font-weight:700;font-size:1rem;cursor:pointer;box-shadow:0 12px 24px rgba(0,94,184,0.2)}
    .secondary-btn{width:100%;height:52px;border:1px solid var(--line);border-radius:12px;background:white;color:var(--primary-dark);font-weight:700;cursor:pointer;margin-top:14px}
    .meta{margin-top:24px;text-align:center;color:var(--muted);font-size:0.9rem}
    .meta a{color:var(--primary);font-weight:700}
    .error{color:var(--danger);font-size:0.88rem;margin-top:10px;min-height:18px}

    .dashboard{min-height:100vh;padding:24px;background:#f4f7fb}
    .topbar{
      max-width:1400px;margin:0 auto 24px;background:white;border:1px solid var(--line);
      border-radius:18px;box-shadow:var(--shadow);display:flex;justify-content:space-between;align-items:center;padding:18px 22px;
    }
    .topbar-left{display:flex;align-items:center;gap:14px}
    .brand-small{width:34px;height:34px;border-radius:12px;background:linear-gradient(135deg,var(--primary),var(--primary-dark));color:white;display:grid;place-items:center;font-weight:700}
    .brand-name{font-weight:700;font-size:1.1rem}
    .topbar nav{display:flex;gap:26px;color:var(--muted);font-size:0.95rem;font-weight:600}
    .topbar nav a:hover{color:var(--primary)}
    .profile{display:flex;align-items:center;gap:12px}
    .avatar{width:36px;height:36px;background:linear-gradient(135deg,#dfeeff,#bfdfff);color:var(--primary-dark);border-radius:50%;display:grid;place-items:center;font-weight:700}

    .layout{max-width:1400px;margin:0 auto;display:grid;grid-template-columns:260px 1fr;gap:24px}
    .sidebar{background:white;border:1px solid var(--line);border-radius:20px;box-shadow:var(--shadow);padding:22px 18px;height:fit-content}
    .side-section{margin-bottom:18px}
    .side-section h4{margin:0 0 12px;color:var(--muted);text-transform:uppercase;font-size:0.76rem;letter-spacing:0.08em}
    .menu-list{list-style:none;padding:0;margin:0;display:grid;gap:8px}
    .menu-list li{padding:10px 12px;border-radius:10px;color:var(--text);font-weight:600;background:#f7faff;border:1px solid transparent}
    .menu-list li.active{background:var(--primary-light);border-color:#d0e6ff;color:var(--primary-dark)}
    .sidebar-card{background:linear-gradient(135deg,#eef8ff,#f8fbff);border:1px solid var(--line);border-radius:18px;padding:16px;margin-top:20px}
    .sidebar-card strong{display:block;font-size:1.7rem;margin:6px 0 10px}
    .main{display:grid;gap:24px}
    .header-row{display:flex;justify-content:space-between;align-items:center;gap:12px;margin-bottom:6px}
    .header-row h1{margin:0;font-size:clamp(2rem,2vw,2.6rem);letter-spacing:-0.04em}
    .header-row .actions{display:flex;gap:12px}
    .ghost-btn{background:white;border:1px solid var(--line);color:var(--primary-dark);font-weight:700;border-radius:12px;padding:10px 14px;cursor:pointer}
    .stats-grid{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:18px}
    .stat-card{background:white;border:1px solid var(--line);border-radius:18px;padding:18px 18px 16px;box-shadow:var(--shadow)}
    .stat-card .label{color:var(--muted);font-size:0.85rem;margin-bottom:8px}
    .stat-card .value{font-weight:700;font-size:1.8rem;letter-spacing:-0.04em;margin-bottom:8px}
    .trend{font-size:0.82rem;font-weight:700;color:var(--accent)}
    .trend.neg{color:var(--danger)}
    .content-grid{display:grid;grid-template-columns:1.2fr 0.8fr;gap:24px}
    .panel{background:white;border:1px solid var(--line);border-radius:20px;box-shadow:var(--shadow);padding:20px}
    .panel-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:18px}
    .panel-header h3{margin:0;font-size:1.25rem}
    .panel-header a{color:var(--primary);font-weight:700;font-size:0.9rem}
    .chart{height:190px;display:flex;align-items:end;gap:12px;padding-top:10px}
    .bar-wrap{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:end;height:100%}
    .bar{width:100%;max-width:34px;border-radius:8px 8px 0 0;background:linear-gradient(180deg,#77b8ff,var(--primary));min-height:20px}
    .month{font-size:0.74rem;color:var(--muted);margin-top:10px}
    .transactions{width:100%;border-collapse:collapse;font-size:0.95rem}
    .transactions th,.transactions td{text-align:left;padding:12px 0;border-bottom:1px solid #edf1f6}
    .transactions th{color:var(--muted);font-size:0.8rem;text-transform:uppercase;letter-spacing:0.06em}
    .merchant{display:flex;align-items:center;gap:10px;font-weight:700}
    .merchant-dot{width:10px;height:10px;border-radius:50%;background:var(--primary)}
    .credit{color:var(--accent);font-weight:700}
    .debit{color:var(--text);font-weight:700}
    .statement-list{display:grid;gap:14px;margin-top:6px}
    .statement{display:flex;justify-content:space-between;gap:12px;align-items:center;padding:14px 14px;border:1px solid var(--line);border-radius:12px;background:#fafcff}
    .statement strong{display:block;margin-bottom:3px;font-size:0.98rem}
    .statement small{color:var(--muted)}
    .download{background:var(--primary-light);border:1px solid #d5e7ff;color:var(--primary-dark);font-weight:700;padding:9px 12px;border-radius:10px;cursor:pointer}
    .insights{display:grid;gap:12px}
    .insight{background:#f9fbff;border:1px solid var(--line);border-radius:14px;padding:14px}
    .insight-row{display:flex;align-items:center;justify-content:space-between;gap:12px;margin-bottom:10px}
    .pill{background:#eafaf6;color:var(--accent);border-radius:999px;padding:6px 10px;font-size:0.74rem;font-weight:700}
    .muted{color:var(--muted);font-size:0.86rem}

    @media (max-width:1100px){
      .layout{grid-template-columns:1fr}
      .sidebar{order:2}
      .main{order:1}
      .stats-grid{grid-template-columns:repeat(2,minmax(0,1fr))}
      .content-grid{grid-template-columns:1fr}
    }
    @media (max-width:780px){
      .login-shell{grid-template-columns:1fr}
      .login-visual{min-height:320px;padding:30px 20px}
      .topbar{flex-wrap:wrap;gap:12px}
      .topbar nav{order:3;width:100%;overflow-x:auto;padding-bottom:4px}
      .stats-grid{grid-template-columns:1fr}
      .header-row{flex-direction:column;align-items:flex-start}
      .header-row .actions{width:100%}
      .header-row .actions button{flex:1}
    }
  </style>
</head>
<body>

  <script>
    const DATA = {
      businessName: "LUCYS LION DEN LLC",
      businessPeriod: "December 2025 – Present",
      accountNumber: "483116762998",
      primaryUser: "Christin Salvato",
      email: "lucyslionden@gmail.com",
      password: "demo123",
      accountType: "Advantage Business Account",
      checking: 127000.00,
      availableBalance: 127000.00,
      savings: 41400.00,
      investments: 31350.00,
      totalBalance: 199750.00,
      monthlyIncome: 482600.00,
      monthlySpend: 188400.00,
      savingsGoal: 22500.00,

      statements: [
        {
          month: "Dec 2025",
          openingBalance: 87200,
          deposits: 124000,
          debits: 48200,
          endingBalance: 127000,
          notes: "Holiday bookings, travel sessions, bridal styling, and on-location hair/makeup services.",
          transactions: [
            { vendor: "Studio North", type: "Credit", amount: 18200, note: "Brand campaign package" },
            { vendor: "Delta Air Lines", type: "Debit", amount: 1380, note: "Flight to Nashville shoot" },
            { vendor: "Marriott Suites", type: "Debit", amount: 2400, note: "Hotel for 3-day travel shoot" },
            { vendor: "MUA Beauty Co.", type: "Credit", amount: 15600, note: "Freelance makeup artistry" },
            { vendor: "The Glam Room", type: "Debit", amount: 950, note: "Hair styling products and tools" },
            { vendor: "Uber", type: "Debit", amount: 420, note: "Transportation to client locations" }
          ]
        },
        {
          month: "Jan 2026",
          openingBalance: 127000,
          deposits: 158000,
          debits: 51000,
          endingBalance: 234000,
          notes: "New year bookings jumped with editorial portraits, product coverage, and travel glam sessions.",
          transactions: [
            { vendor: "L. Rivers Creative", type: "Credit", amount: 22350, note: "Photographer retainer" },
            { vendor: "Hertz Rentals", type: "Debit", amount: 1750, note: "Vehicle rental for multiple shoots" },
            { vendor: "Airbnb Host", type: "Debit", amount: 2140, note: "Out-of-town shoot lodging" },
            { vendor: "Beauty Lab Pro", type: "Credit", amount: 19875, note: "Freelance hair styling" },
            { vendor: "Tidal Film Supply", type: "Debit", amount: 980, note: "Lighting and filters" },
            { vendor: "Sephora Pro", type: "Debit", amount: 640, note: "Product and makeup supplies" }
          ]
        },
        {
          month: "Feb 2026",
          openingBalance: 234000,
          deposits: 192000,
          debits: 64000,
          endingBalance: 362000,
          notes: "Valentine’s event photography and travel bridal services created a strong revenue lift.",
          transactions: [
            { vendor: "Southern Glow Bridal", type: "Credit", amount: 24600, note: "Wedding prep and glam package" },
            { vendor: "American Airlines", type: "Debit", amount: 1460, note: "Travel to Atlanta shoot" },
            { vendor: "Hotel Indigo", type: "Debit", amount: 2235, note: "One night stay" },
            { vendor: "Toi Studio", type: "Credit", amount: 19500, note: "Portrait session revenue" },
            { vendor: "Sally Beauty Supply", type: "Debit", amount: 1180, note: "Hair styling inventory" },
            { vendor: "Local Produce Co.", type: "Debit", amount: 340, note: "Client meal support" }
          ]
        },
        {
          month: "Mar 2026",
          openingBalance: 362000,
          deposits: 226000,
          debits: 71000,
          endingBalance: 517000,
          notes: "Spring campaign travel combined with makeup and hair client work created a steady demand spike.",
          transactions: [
            { vendor: "Northline Media", type: "Credit", amount: 28300, note: "Campaign photography" },
            { vendor: "Avis Car Rental", type: "Debit", amount: 1850, note: "Rental van for 5-day shoot" },
            { vendor: "The Rowan Hotel", type: "Debit", amount: 2530, note: "Travel lodging" },
            { vendor: "Velvet Glam", type: "Credit", amount: 21450, note: "Freelance makeup and hair artistry" },
            { vendor: "Adobe Creative Cloud", type: "Debit", amount: 690, note: "Editing software subscription" },
            { vendor: "Roadside Fuel", type: "Debit", amount: 510, note: "Fuel and mileage reimbursement" }
          ]
        },
        {
          month: "Apr 2026",
          openingBalance: 517000,
          deposits: 260000,
          debits: 86000,
          endingBalance: 691000,
          notes: "Traveling shoots for beauty brands and portrait sessions accelerated across multiple cities.",
          transactions: [
            { vendor: "Mosaic Beauty Group", type: "Credit", amount: 32150, note: "Beauty editorial contract" },
            { vendor: "Delta Air Lines", type: "Debit", amount: 1590, note: "Flight to Los Angeles" },
            { vendor: "Airbnb Summit", type: "Debit", amount: 2820, note: "Temporary production housing" },
            { vendor: "Glass & Glow", type: "Credit", amount: 23625, note: "On-location makeup services" },
            { vendor: "Curbside Rentals", type: "Debit", amount: 970, note: "Photo props and backdrop gear" },
            { vendor: "UPS Store", type: "Debit", amount: 620, note: "Shipping of equipment" }
          ]
        },
        {
          month: "May 2026",
          openingBalance: 691000,
          deposits: 295000,
          debits: 97000,
          endingBalance: 889000,
          notes: "Wedding season and traveling bridal styling work materially increased operating income.",
          transactions: [
            { vendor: "Rose & Ember Co.", type: "Credit", amount: 34130, note: "Wedding photography package" },
            { vendor: "Southwest Airlines", type: "Debit", amount: 1715, note: "Travel for destination wedding" },
            { vendor: "The Harbor Inn", type: "Debit", amount: 2940, note: "Hotel stay for multi-day event" },
            { vendor: "The Salon Collective", type: "Credit", amount: 28350, note: "Makeup and hairstyling bookings" },
            { vendor: "Aveda Pro", type: "Debit", amount: 950, note: "Hair care products" },
            { vendor: "Café & Catering Co.", type: "Debit", amount: 480, note: "Client meal support" }
          ]
        },
        {
          month: "Jun 2026",
          openingBalance: 889000,
          deposits: 332000,
          debits: 108500,
          endingBalance: 1112500,
          notes: "Summer shoot season pushed revenue higher with travel, beauty editorial, and destination sessions.",
          transactions: [
            { vendor: "Wild Bloom Studio", type: "Credit", amount: 37360, note: "Fashion editorials" },
            { vendor: "JetBlue", type: "Debit", amount: 1945, note: "Airfare for out-of-state gig" },
            { vendor: "The Bungalow Suites", type: "Debit", amount: 3180, note: "Three-night accommodation" },
            { vendor: "Aster & Co.", type: "Credit", amount: 28610, note: "Freelance hair styling services" },
            { vendor: "Morphe Pro", type: "Debit", amount: 1460, note: "Makeup inventory" },
            { vendor: "Roadline Transit", type: "Debit", amount: 630, note: "Local transportation and mileage" }
          ]
        },
        {
          month: "Jul 2026",
          openingBalance: 1112500,
          deposits: 368000,
          debits: 119000,
          endingBalance: 1400500,
          notes: "Peak season income rose with destination wedding, portrait sessions, and beauty travel work.",
          transactions: [
            { vendor: "Luma & Lace", type: "Credit", amount: 42150, note: "Wedding photography coverage" },
            { vendor: "United Airlines", type: "Debit", amount: 2210, note: "Flight to Miami" },
            { vendor: "Seabreeze Lodge", type: "Debit", amount: 3630, note: "Hotel stay for event production" },
            { vendor: "Velvet Muse", type: "Credit", amount: 31280, note: "Makeup artistry for lifestyle campaign" },
            { vendor: "Kitsune Atelier", type: "Debit", amount: 1280, note: "Hair accessories and styling tools" },
            { vendor: "Fuel Up Express", type: "Debit", amount: 760, note: "Fuel reimbursement" }
          ]
        },
        {
          month: "Aug 2026",
          openingBalance: 1400500,
          deposits: 405000,
          debits: 132500,
          endingBalance: 1728000,
          notes: "Photography contract income, salon work, and on-location beauty bookings all increased across the month.",
          transactions: [
            { vendor: "Northline Collection", type: "Credit", amount: 44850, note: "Portrait and campaign shoot" },
            { vendor: "Alaska Air", type: "Debit", amount: 2270, note: "Regional travel" },
            { vendor: "Haven & Co. Suites", type: "Debit", amount: 3950, note: "Travel lodging" },
            { vendor: "Glam Atelier", type: "Credit", amount: 33600, note: "Freelance hair and beauty services" },
            { vendor: "Beauty Cartel", type: "Debit", amount: 1810, note: "Makeup and styling inventory" },
            { vendor: "Shipment One", type: "Debit", amount: 700, note: "Equipment shipment" }
          ]
        },
        {
          month: "Sep 2026",
          openingBalance: 1728000,
          deposits: 442000,
          debits: 147800,
          endingBalance: 1812200,
          notes: "The business continued growing through luxury portrait work, product campaigns, and traveling glam setups.",
          transactions: [
            { vendor: "Harbor Lane Studio", type: "Credit", amount: 48230, note: "Editorial photography" },
            { vendor: "Delta Air Lines", type: "Debit", amount: 2430, note: "Shoot travel" },
            { vendor: "The Wayfarer Hotel", type: "Debit", amount: 4135, note: "Client location lodging" },
            { vendor: "Miro Beauty Group", type: "Credit", amount: 36720, note: "Makeup artistry services" },
            { vendor: "Satin & Stone", type: "Debit", amount: 1750, note: "Hair styling products" },
            { vendor: "Fuel Ledger", type: "Debit", amount: 840, note: "Mileage reimbursement" }
          ]
        },
        {
          month: "Oct 2026",
          openingBalance: 1812200,
          deposits: 480000,
          debits: 160300,
          endingBalance: 1971900,
          notes: "Current month remains the strongest, driven by large production bookings and luxury beauty styling demand.",
          transactions: [
            { vendor: "The Soleil House", type: "Credit", amount: 51050, note: "Brand campaign photography" },
            { vendor: "American Airlines", type: "Debit", amount: 2680, note: "Travel for multi-city shoot" },
            { vendor: "Lark Hotel Collective", type: "Debit", amount: 4465, note: "Travel and lodging" },
            { vendor: "Heritage Glam Co.", type: "Credit", amount: 39200, note: "Freelance hair and makeup" },
            { vendor: "The Beauty Supply House", type: "Debit", amount: 1825, note: "Supplies and tools" },
            { vendor: "Westside Crew", type: "Debit", amount: 920, note: "Assistant and transportation support" }
          ]
        }
      ]
    };

    function formatMoney(value) {
      return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(value);
    }

    function showDashboard() {
      document.getElementById('miniAvail').textContent = formatMoney(DATA.availableBalance).replace('$','');
      document.getElementById('miniChecking').textContent = formatMoney(DATA.checking).replace('$','');
      document.getElementById('miniSavings').textContent = formatMoney(DATA.savings).replace('$','');
      document.getElementById('miniInvestments').textContent = formatMoney(DATA.investments).replace('$','');

      document.getElementById('businessTitle').textContent = DATA.businessName;
      document.getElementById('accType').textContent = DATA.accountType;
      document.getElementById('accountNumber').textContent = DATA.accountNumber;
      document.getElementById('headerUser').textContent = DATA.primaryUser;
      document.getElementById('welcomeText').textContent = 'Good morning, ' + DATA.primaryUser.split(' ')[0] + '.';
      document.getElementById('sidebarCash').textContent = formatMoney(DATA.availableBalance);
      document.getElementById('totalBalance').textContent = formatMoney(DATA.totalBalance);
      document.getElementById('incomeValue').textContent = formatMoney(DATA.monthlyIncome);
      document.getElementById('spendValue').textContent = formatMoney(DATA.monthlySpend);
      document.getElementById('goalValue').textContent = formatMoney(DATA.savingsGoal);

      const barChart = document.getElementById('cashFlowChart');
      barChart.innerHTML = '';

      const depValues = DATA.statements.map(s => {
        const totalDeposits = s.transactions.filter(t => t.type === 'Credit').reduce((sum, item) => sum + item.amount, 0);
        return totalDeposits;
      });

      const maxVal = Math.max(...depValues, 100000);
      depValues.forEach((value, index) => {
        const wrap = document.createElement('div');
        wrap.className = 'bar-wrap';
        const bar = document.createElement('div');
        bar.className = 'bar';
        bar.style.height = ((value / maxVal) * 100) + '%';
        const month = document.createElement('div');
        month.className = 'month';
        month.textContent = DATA.statements[index].month.replace(' ', '\n').split('\n')[0];
        wrap.appendChild(bar);
        wrap.appendChild(month);
        barChart.appendChild(wrap);
      });

      const tbody = document.getElementById('transactionsBody');
      tbody.innerHTML = '';

      DATA.statements.slice(-4).forEach(statement => {
        statement.transactions.slice(0, 3).forEach(item => {
          const tr = document.createElement('tr');

          const merchantCell = document.createElement('td');
          const merchantWrap = document.createElement('div');
          merchantWrap.className = 'merchant';
          const dot = document.createElement('span');
          dot.className = 'merchant-dot';
          dot.style.background = item.type === 'Credit' ? '#0a8f7d' : '#005eb8';
          const label = document.createElement('span');
          label.textContent = item.vendor;
          merchantWrap.appendChild(dot);
          merchantWrap.appendChild(label);
          merchantCell.appendChild(merchantWrap);

          const dateCell = document.createElement('td');
          dateCell.textContent = statement.month;

          const typeCell = document.createElement('td');
          const typeSpan = document.createElement('span');
          typeSpan.className = item.type === 'Credit' ? 'credit' : 'debit';
          typeSpan.textContent = item.type;
          typeCell.appendChild(typeSpan);

          const amountCell = document.createElement('td');
          amountCell.class