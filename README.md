[Stock Price Calculator.html.html](https://github.com/user-attachments/files/25457506/Stock.Price.Calculator.html.html)
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>평단가 계산기 | 물타기·불타기</title>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;700;900&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg: #0d0f14;
      --surface: #161a23;
      --surface2: #1e2330;
      --border: #2a3048;
      --accent: #4f8eff;
      --accent2: #ff5f57;
      --accent3: #30d158;
      --text: #e8eaf2;
      --text-dim: #7c85a2;
      --text-muted: #4a5270;
      --mono: 'DM Mono', monospace;
      --sans: 'Noto Sans KR', sans-serif;
    }

    html, body {
      min-height: 100vh;
      background: var(--bg);
      color: var(--text);
      font-family: var(--sans);
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 24px 16px;
    }

    /* Background grid */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image:
        linear-gradient(rgba(79,142,255,0.04) 1px, transparent 1px),
        linear-gradient(90deg, rgba(79,142,255,0.04) 1px, transparent 1px);
      background-size: 40px 40px;
      pointer-events: none;
    }

    body::after {
      content: '';
      position: fixed;
      top: -200px; left: 50%;
      transform: translateX(-50%);
      width: 600px; height: 600px;
      background: radial-gradient(ellipse, rgba(79,142,255,0.12) 0%, transparent 70%);
      pointer-events: none;
    }

    .card {
      position: relative;
      width: 100%;
      max-width: 420px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 32px 28px;
      box-shadow: 0 0 0 1px rgba(255,255,255,0.03), 0 40px 80px rgba(0,0,0,0.5);
      animation: fadeUp 0.5s ease both;
    }

    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(20px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    .badge {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      background: rgba(79,142,255,0.12);
      border: 1px solid rgba(79,142,255,0.25);
      color: var(--accent);
      font-size: 11px;
      font-weight: 700;
      letter-spacing: 0.08em;
      padding: 4px 10px;
      border-radius: 100px;
      margin-bottom: 14px;
      text-transform: uppercase;
    }

    .badge::before {
      content: '';
      width: 6px; height: 6px;
      background: var(--accent);
      border-radius: 50%;
      box-shadow: 0 0 8px var(--accent);
      animation: pulse 2s ease-in-out infinite;
    }

    @keyframes pulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.3; }
    }

    h1 {
      font-size: 22px;
      font-weight: 900;
      line-height: 1.2;
      margin-bottom: 4px;
      letter-spacing: -0.03em;
    }

    .subtitle {
      font-size: 13px;
      color: var(--text-dim);
      margin-bottom: 28px;
    }

    .section-label {
      font-size: 10px;
      font-weight: 700;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: var(--text-muted);
      margin-bottom: 12px;
    }

    .input-group {
      display: flex;
      flex-direction: column;
      gap: 10px;
      margin-bottom: 10px;
    }

    .input-row {
      display: flex;
      align-items: center;
      justify-content: space-between;
      background: var(--surface2);
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 12px 16px;
      transition: border-color 0.2s, box-shadow 0.2s;
    }

    .input-row:focus-within {
      border-color: var(--accent);
      box-shadow: 0 0 0 3px rgba(79,142,255,0.1);
    }

    .input-row label {
      font-size: 13px;
      color: var(--text-dim);
      font-weight: 500;
      white-space: nowrap;
    }

    .input-row input {
      background: transparent;
      border: none;
      outline: none;
      color: var(--text);
      font-family: var(--sans);
      font-size: 15px;
      font-weight: 500;
      text-align: right;
      width: 55%;
    }

    .input-row input::placeholder { color: var(--text-muted); }

    /* 숫자 입력 화살표 제거 */
    .input-row input::-webkit-outer-spin-button,
    .input-row input::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }
    .input-row input[type=number] { -moz-appearance: textfield; }

    .divider {
      display: flex;
      align-items: center;
      gap: 12px;
      margin: 20px 0;
    }

    .divider::before, .divider::after {
      content: '';
      flex: 1;
      height: 1px;
      background: var(--border);
    }

    .divider-icon {
      font-size: 18px;
      opacity: 0.6;
    }

    .btn {
      width: 100%;
      padding: 15px;
      margin-top: 20px;
      background: var(--accent);
      color: #fff;
      border: none;
      border-radius: 12px;
      font-family: var(--sans);
      font-size: 15px;
      font-weight: 700;
      cursor: pointer;
      letter-spacing: -0.01em;
      transition: transform 0.15s, box-shadow 0.15s, background 0.15s;
      box-shadow: 0 4px 20px rgba(79,142,255,0.35);
    }

    .btn:hover {
      background: #6aa0ff;
      transform: translateY(-1px);
      box-shadow: 0 8px 28px rgba(79,142,255,0.45);
    }

    .btn:active {
      transform: translateY(0);
      box-shadow: 0 2px 10px rgba(79,142,255,0.3);
    }

    .result-box {
      margin-top: 24px;
      background: var(--surface2);
      border: 1px solid var(--border);
      border-radius: 14px;
      overflow: hidden;
      opacity: 1;
      transform: translateY(0);
      transition: opacity 0.3s ease, transform 0.3s ease;
    }

    .result-main {
      padding: 20px;
      text-align: center;
      border-bottom: 1px solid var(--border);
      background: linear-gradient(135deg, rgba(79,142,255,0.08), rgba(79,142,255,0.02));
    }

    .result-main-label {
      font-size: 11px;
      font-weight: 700;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      color: var(--text-muted);
      margin-bottom: 6px;
    }

    .result-main-value {
      font-family: var(--sans);
      font-size: 34px;
      font-weight: 500;
      color: var(--accent);
      letter-spacing: -0.03em;
      line-height: 1;
    }

    .result-main-unit {
      font-size: 14px;
      color: var(--text-dim);
      margin-top: 4px;
    }

    .result-sub {
      display: grid;
      grid-template-columns: 1fr 1fr;
    }

    .result-sub-item {
      padding: 16px;
      text-align: center;
    }

    .result-sub-item:first-child {
      border-right: 1px solid var(--border);
    }

    .result-sub-label {
      font-size: 11px;
      color: var(--text-muted);
      font-weight: 600;
      letter-spacing: 0.06em;
      text-transform: uppercase;
      margin-bottom: 5px;
    }

    .result-sub-value {
      font-family: var(--sans);
      font-size: 17px;
      font-weight: 500;
      color: var(--text);
    }

    .footer {
      text-align: center;
      margin-top: 24px;
      font-size: 12px;
      color: var(--text-muted);
    }
  </style>
</head>
<body>
  <div class="card">
    <div class="badge">Stock Calculator</div>
    <h1>평단가 계산기</h1>
    <p class="subtitle">물타기 · 불타기 후 평균 단가를 계산합니다</p>

    <div class="section-label">기존 보유</div>
    <div class="input-group">
      <div class="input-row">
        <label>기존 평단가</label>
        <input type="number" id="oldPrice" placeholder="0" min="0">
      </div>
      <div class="input-row">
        <label>보유 수량</label>
        <input type="number" id="oldQty" placeholder="0" min="0">
      </div>
    </div>

    <div class="divider"><span class="divider-icon">＋</span></div>

    <div class="section-label">추가 매수</div>
    <div class="input-group">
      <div class="input-row">
        <label>추가 매수 단가</label>
        <input type="number" id="newPrice" placeholder="0" min="0">
      </div>
      <div class="input-row">
        <label>추가 매수 수량</label>
        <input type="number" id="newQty" placeholder="0" min="0">
      </div>
    </div>



    <div class="result-box" id="resultBox">
      <div class="result-main">
        <div class="result-main-label">최종 평단가</div>
        <div class="result-main-value" id="resultAvg">0</div>
        <div class="result-main-unit">원</div>
      </div>
      <div class="result-sub">
        <div class="result-sub-item">
          <div class="result-sub-label">총 보유 수량</div>
          <div class="result-sub-value"><span id="resultQty">0</span> 주</div>
        </div>
        <div class="result-sub-item">
          <div class="result-sub-label">총 투자 금액</div>
          <div class="result-sub-value"><span id="resultTotal">0</span> 원</div>
        </div>
      </div>
    </div>

    <p class="footer">© 무료 투자 도구 · 투자 판단의 책임은 본인에게 있습니다</p>
  </div>

  <script>
    function calculate() {
      const oldPrice = Number(document.getElementById('oldPrice').value) || 0;
      const oldQty   = Number(document.getElementById('oldQty').value)   || 0;
      const newPrice = Number(document.getElementById('newPrice').value) || 0;
      const newQty   = Number(document.getElementById('newQty').value)   || 0;

      const totalOld = oldPrice * oldQty;
      const totalNew = newPrice * newQty;
      const totalAmt = totalOld + totalNew;
      const totalQty = oldQty + newQty;
      const avgPrice = totalQty > 0 ? totalAmt / totalQty : 0;

      document.getElementById('resultAvg').textContent   = Math.round(avgPrice).toLocaleString();
      document.getElementById('resultQty').textContent   = totalQty.toLocaleString();
      document.getElementById('resultTotal').textContent = totalAmt.toLocaleString();
    }

    // 숫자만 입력 허용 + 자동계산
    document.querySelectorAll('input').forEach(function(input) {
      input.addEventListener('keydown', function(e) {
        const allowed = ['Backspace','Delete','ArrowLeft','ArrowRight','ArrowUp','ArrowDown','Tab'];
        if (!allowed.includes(e.key) && !/^\d$/.test(e.key)) {
          e.preventDefault();
        }
      });
      input.addEventListener('input', calculate);
      input.addEventListener('paste', function(e) {
        e.preventDefault();
        const text = (e.clipboardData || window.clipboardData).getData('text');
        const onlyNums = text.replace(/[^\d]/g, '');
        if (onlyNums) document.execCommand('insertText', false, onlyNums);
        calculate();
      });
    });
  </script>
</body>
</html>
