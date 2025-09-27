<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>認証機能付きビジネス経営管理システム（統合・修正版）</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: linear-gradient(135deg,#1e3c72 0%, #2a5298 100%); min-height: 100vh; padding: 20px; }
    .container { max-width: 1400px; margin: 0 auto; background: rgba(255,255,255,0.95); backdrop-filter: blur(10px); border-radius: 20px; box-shadow: 0 20px 40px rgba(0,0,0,0.15); overflow: hidden; }

    /* ログイン画面 */
    .login-container { max-width: 450px; margin: 50px auto; background: rgba(255,255,255,0.95); backdrop-filter: blur(10px); border-radius: 20px; box-shadow: 0 20px 40px rgba(0,0,0,0.15); overflow: hidden; }
    .login-header { background: linear-gradient(135deg,#1a2a6c,#b21f1f,#fdbb2d); color: #fff; padding: 40px; text-align: center; }
    .login-header h1 { font-size: 2.5rem; margin-bottom: 10px; text-shadow: 0 2px 4px rgba(0,0,0,0.3); }
    .login-header p { font-size: 1.1rem; opacity: 0.9; }
    .login-form { padding: 40px; }
    .form-group { margin-bottom: 25px; }
    .form-group label { display: block; margin-bottom: 8px; font-weight: 600; color: #1a2a6c; }
    .form-group input { width: 100%; padding: 15px; border: 2px solid #2d4056; border-radius: 10px; font-size: 16px; transition: all 0.3s ease; }
    .form-group input:focus { outline: none; border-color: #1a2a6c; box-shadow: 0 0 0 3px rgba(230, 230, 230, 0.1); }
    .login-btn { width: 100%; padding: 15px; background: linear-gradient(135deg,#1a2a6c 0%,#2a5298 100%); color: #fff; border: none; border-radius: 10px; font-size: 18px; font-weight: 600; cursor: pointer; transition: 0.3s; box-shadow: 0 5px 15px rgba(26,42,108,0.3); }
    .login-btn:hover { transform: translateY(-2px); box-shadow: 0 8px 25px rgba(26,42,108,0.4); }
    .error-message { background: linear-gradient(135deg,#ff6b6b,#ee5a24); color: #fff; padding: 15px; border-radius: 10px; margin-bottom: 20px; text-align: center; font-weight: 600; }
    .register-link { text-align: center; margin-top: 20px; padding-top: 20px; border-top: 1px solid #e0e6ed; }
    .register-link a { color: #1a2a6c; text-decoration: none; font-weight: 600; }
    .register-link a:hover { text-decoration: underline; }

    /* メイン */
    .header { background: linear-gradient(135deg,#1a2a6c,#b21f1f,#fdbb2d); color: #fff; padding: 30px; text-align: center; position: relative; }
    .header h1 { font-size: 2.8rem; margin-bottom: 10px; text-shadow: 0 2px 4px rgba(0,0,0,0.3); font-weight: 700; }
    .header p { font-size: 1.1rem; opacity: 0.9; margin-bottom: 15px; }
    .user-info { position: absolute; top: 20px; right: 20px; background: rgba(255,255,255,0.2); padding: 10px 20px; border-radius: 20px; backdrop-filter: blur(5px); }
    .logout-btn { background: rgba(255,255,255,0.2); color: #fff; border: 1px solid rgba(255,255,255,0.3); padding: 8px 16px; border-radius: 15px; cursor: pointer; margin-left: 10px; transition: 0.3s; }
    .logout-btn:hover { background: rgba(255,255,255,0.3); }
    .year-selector { margin: 10px 0; }
    .year-selector select { padding: 10px 20px; font-size: 16px; border: none; border-radius: 25px; background: rgba(255,255,255,0.2); color: #fff; backdrop-filter: blur(5px); }
    .year-selector select option { background: #1a2a6c; color: #fff; }
    .content { padding: 30px; }
    .kpi-dashboard { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px,1fr)); gap: 20px; margin-bottom: 30px; }
    .kpi-card { background: linear-gradient(135deg,#667eea 0%,#764ba2 100%); color: #fff; padding: 25px; border-radius: 15px; text-align: center; box-shadow: 0 10px 20px rgba(0,0,0,0.1); transition: transform 0.3s ease; position: relative; overflow: hidden; }
    .kpi-card::before { content: ''; position: absolute; top: -50%; left: -50%; width: 200%; height: 200%; background: linear-gradient(45deg,transparent,rgba(255,255,255,0.1),transparent); transform: rotate(45deg); transition: 0.5s; opacity: 0; }
    .kpi-card:hover { transform: translateY(-5px); }
    .kpi-card:hover::before { opacity: 1; left: 100%; }
    .kpi-card.revenue { background: linear-gradient(135deg,#4facfe 0%,#00f2fe 100%); }
    .kpi-card.expense { background: linear-gradient(135deg,#fa709a 0%,#fee140 100%); }
    .kpi-card.profit { background: linear-gradient(135deg,#a8edea 0%,#fed6e3 100%); color: #2c3e50; }
    .kpi-card.margin { background: linear-gradient(135deg,#d299c2 0%,#fef9d7 100%); color: #2c3e50; }
    .kpi-card.cash { background: linear-gradient(135deg,#89f7fe 0%,#66a6ff 100%); }
    .kpi-card h3 { font-size: 1.1rem; margin-bottom: 10px; opacity: 0.9; font-weight: 600; }
    .kpi-card .amount { font-size: 1.8rem; font-weight: bold; margin-bottom: 5px; }
    .kpi-card .percentage { font-size: 0.9rem; opacity: 0.8; }

    .table-container { background: #fff; border-radius: 15px; overflow-x: auto; box-shadow: 0 10px 30px rgba(0,0,0,0.1); margin-bottom: 20px; }
    table { width: 100%; border-collapse: collapse; min-width: 1200px; }
    th { background: linear-gradient(135deg,#1a2a6c 0%,#2a5298 100%); color: #fff; padding: 15px 8px; text-align: center; font-weight: 600; position: sticky; top: 0; z-index: 10; font-size: 13px; white-space: nowrap; }
    td { padding: 10px 8px; text-align: center; border-bottom: 1px solid #eee; transition: background-color 0.2s ease; font-size: 13px; }
    tr:hover td { background-color: #f8f9ff; }
    .month-cell { font-weight: 600; color: #1a2a6c; background-color: #f8f9fa; font-size: 14px; }
    .input-cell { padding: 3px; }
    .input-cell input { width: 100%; padding: 6px; border: 2px solid #e0e6ed; border-radius: 6px; text-align: right; font-size: 12px; transition: border-color 0.3s ease; }
    .input-cell input:focus { outline: none; border-color: #1a2a6c; box-shadow: 0 0 0 2px rgba(26,42,108,0.1); }
    .calculated-cell { background-color: #f8f9fa; font-weight: 600; }
    .total-row { background: linear-gradient(135deg,#ffecd2 0%,#fcb69f 100%); font-weight: bold; }
    .total-row td { border-top: 3px solid #1a2a6c; padding: 12px 8px; font-size: 14px; }
    .section-header { background: linear-gradient(135deg,#e3f2fd 0%,#bbdefb 100%); color: #1a2a6c; font-weight: bold; text-align: left; padding-left: 15px; }
    .positive { color: #27ae60; font-weight: 600; }
    .negative { color: #e74c3c; font-weight: 600; }
    .zero { color: #7f8c8d; }
    .buttons { margin-top: 20px; text-align: center; gap: 15px; display: flex; justify-content: center; flex-wrap: wrap; }
    .btn { padding: 12px 25px; border: none; border-radius: 25px; font-size: 16px; font-weight: 600; cursor: pointer; transition: 0.3s; text-decoration: none; display: inline-block; }
    .btn-primary { background: linear-gradient(135deg,#1a2a6c 0%,#2a5298 100%); color: #fff; box-shadow: 0 5px 15px rgba(26,42,108,0.3); }
    .btn-secondary { background: linear-gradient(135deg,#ffecd2 0%,#fcb69f 100%); color: #1a2a6c; box-shadow: 0 5px 15px rgba(252,182,159,0.3); }
    .btn-export { background: linear-gradient(135deg,#4CAF50 0%,#45a049 100%); color: #fff; box-shadow: 0 5px 15px rgba(76,175,80,0.3); }
    .btn:hover { transform: translateY(-2px); }
    .hidden { display: none !important; }

    @media (max-width: 768px) { .container, .login-container { margin: 10px; border-radius: 15px; } .header, .login-header { padding: 20px; } .header h1, .login-header h1 { font-size: 2rem; } .content, .login-form { padding: 20px; } .kpi-dashboard { grid-template-columns: repeat(2, 1fr); } .kpi-card .amount { font-size: 1.4rem; } .user-info { position: static; margin-top: 20px; text-align: center; } }
  </style>
</head>
<body>
  <!-- ログイン画面 -->
  <div id="loginScreen" class="login-container">
    <div class="login-header">
      <h1>🔐 ビジネス管理</h1>
      <p>Business Management System</p>
    </div>
    <div class="login-form">
      <div id="errorMessage" class="error-message hidden"></div>
      <div class="form-group">
        <label for="loginId">ユーザーID</label>
        <input type="text" id="loginId" placeholder="ユーザーIDを入力" value="&amp;F001" />
      </div>
      <div class="form-group">
        <label for="loginPassword">パスワード</label>
        <input type="password" id="loginPassword" placeholder="パスワードを入力" value="0123" />
      </div>
      <button class="login-btn" onclick="login()">ログイン</button>
      <div class="register-link">
        <a href="#" onclick="showRegister()">新規アカウント登録</a>
      </div>
    </div>
  </div>

  <!-- 新規登録画面 -->
  <div id="registerScreen" class="login-container hidden">
    <div class="login-header">
      <h1>📝 アカウント登録</h1>
      <p>New Account Registration</p>
    </div>
    <div class="login-form">
      <div id="registerErrorMessage" class="error-message hidden"></div>
      <div class="form-group">
        <label for="registerId">ユーザーID</label>
        <input type="text" id="registerId" placeholder="ユーザーIDを入力（4-20文字）" />
      </div>
      <div class="form-group">
        <label for="registerPassword">パスワード</label>
        <input type="password" id="registerPassword" placeholder="パスワードを入力（6文字以上）" />
      </div>
      <div class="form-group">
        <label for="registerPasswordConfirm">パスワード確認</label>
        <input type="password" id="registerPasswordConfirm" placeholder="パスワードを再入力" />
      </div>
      <div class="form-group">
        <label for="companyName">会社名</label>
        <input type="text" id="companyName" placeholder="会社名を入力" />
      </div>
      <button class="login-btn" onclick="register()">アカウント登録</button>
      <div class="register-link">
        <a href="#" onclick="showLogin()">ログインに戻る</a>
      </div>
    </div>
  </div>

  <!-- メインアプリケーション -->
  <div id="mainApp" class="container hidden">
    <div class="header">
      <div class="user-info">
        <span id="currentUser"></span>
        <span id="currentCompany"></span>
        <button class="logout-btn" onclick="logout()">ログアウト</button>
      </div>
      <h1>📈 ビジネス経営管理</h1>
      <p>Business Management Dashboard</p>
      <div class="year-selector">
        <select id="yearSelect" onchange="updateYear()">
          <option value="2024">2024年度</option>
          <option value="2025" selected>2025年度</option>
          <option value="2026">2026年度</option>
        </select>
      </div>
    </div>

    <div class="content">
      <div class="kpi-dashboard">
        <div class="kpi-card revenue"><h3>年間売上高</h3><div class="amount" id="totalRevenue">¥0</div></div>
        <div class="kpi-card expense"><h3>年間総費用</h3><div class="amount" id="totalCosts">¥0</div></div>
        <div class="kpi-card profit"><h3>年間営業利益</h3><div class="amount" id="totalProfit">¥0</div></div>
        <div class="kpi-card margin"><h3>営業利益率</h3><div class="amount" id="profitMargin">0%</div></div>
        <div class="kpi-card cash"><h3>累計キャッシュフロー</h3><div class="amount" id="cashFlow">¥0</div></div>
      </div>

      <div class="table-container">
        <table>
          <thead>
            <tr>
              <th rowspan="2">月</th>
              <th colspan="4">売上・収入</th>
              <th colspan="8">費用・支出</th>
              <th colspan="3">利益分析</th>
            </tr>
            <tr>
              <th>商品売上</th>
              <th>サービス売上</th>
              <th>その他収入</th>
              <th>売上合計</th>
              <th>仕入原価</th>
              <th>人件費</th>
              <th>地代家賃</th>
              <th>広告宣伝費</th>
              <th>通信費</th>
              <th>光熱費</th>
              <th>交通費</th>
              <th>その他経費</th>
              <th>売上総利益</th>
              <th>営業利益</th>
              <th>累計利益</th>
            </tr>
          </thead>
          <tbody id="businessData"></tbody>
        </table>
      </div>

      <div class="buttons">
        <button class="btn btn-primary" onclick="calculateAll()">📊 計算更新</button>
        <button class="btn btn-secondary" onclick="clearAll()">🗑️ 全クリア</button>
        <button class="btn btn-secondary" onclick="saveData()">💾 データ保存</button>
        <button class="btn btn-secondary" onclick="loadData()">📂 データ読込</button>
        <button class="btn btn-export" onclick="exportToCSV()">📤 CSV出力</button>
      </div>
    </div>
  </div>

  <script>
    // ====== 定数・ユーティリティ ======
    const months = ['1月','2月','3月','4月','5月','6月','7月','8月','9月','10月','11月','12月'];
    const STORAGE_USERS_KEY = 'bms_users_v2';
    const STORAGE_SESSION_KEY = 'bms_current_user_v2';
    const STORAGE_DATA_PREFIX = 'bms_data_v2';

    let currentYear = 2025;
    let currentUser = null; // ユーザーID文字列
    let users = { '&F001': { password: '0123', company: 'デフォルト株式会社', data: {} } }; // 既定ユーザー

    const $ = (id) => document.getElementById(id);
    const toNum = (v) => { const n = Number(v); return Number.isFinite(n) ? n : 0; };
    const formatCurrency = (amount) => '¥' + (Number(amount)||0).toLocaleString('ja-JP');
    const getBalanceClass = (amount) => amount > 0 ? 'positive' : (amount < 0 ? 'negative' : 'zero');

    // ====== ストレージ ======
    function loadUsersFromStorage(){
      try {
        const raw = localStorage.getItem(STORAGE_USERS_KEY);
        if (raw) users = JSON.parse(raw);
        else localStorage.setItem(STORAGE_USERS_KEY, JSON.stringify(users));
      } catch(e){ console.warn('ユーザー読み込み失敗', e); }
    }
    function persistUsers(){
      try { localStorage.setItem(STORAGE_USERS_KEY, JSON.stringify(users)); } catch(e) { console.warn('ユーザー保存失敗', e); }
    }
    function setCurrentUser(id){ sessionStorage.setItem(STORAGE_SESSION_KEY, id); }
    function getCurrentUser(){ return sessionStorage.getItem(STORAGE_SESSION_KEY); }
    function clearCurrentUser(){ sessionStorage.removeItem(STORAGE_SESSION_KEY); }
    function dataKey(userId, year){ return `${STORAGE_DATA_PREFIX}_${userId}_${year}`; }

    function saveUserData(){
      if (!currentUser) return;
      const data = {};
      document.querySelectorAll('input[type="number"]').forEach(inp => { if (inp.value !== '') data[inp.id] = inp.value; });
      // usersにも保持
      if (!users[currentUser].data) users[currentUser].data = {};
      users[currentUser].data[currentYear] = data;
      persistUsers();
      localStorage.setItem(dataKey(currentUser, currentYear), JSON.stringify(data));
    }
    function loadUserData(){
      if (!currentUser) return;
      const raw = localStorage.getItem(dataKey(currentUser, currentYear));
      const data = raw ? JSON.parse(raw) : (users[currentUser].data?.[currentYear] || {});
      Object.keys(data).forEach(id => { const el = $(id); if (el) el.value = data[id]; });
    }

    // ====== 画面切替 ======
    function showLogin(){ $('loginScreen').classList.remove('hidden'); $('registerScreen').classList.add('hidden'); $('mainApp').classList.add('hidden'); }
    function showRegister(){ $('loginScreen').classList.add('hidden'); $('registerScreen').classList.remove('hidden'); $('mainApp').classList.add('hidden'); }
    function showMain(){
      $('loginScreen').classList.add('hidden'); $('registerScreen').classList.add('hidden'); $('mainApp').classList.remove('hidden');
      $('currentUser').textContent = `👤 ${currentUser}`;
      $('currentCompany').textContent = ` | 🏢 ${users[currentUser]?.company || ''}`;
      generateTable();
      loadUserData();
      calculateAll();
    }

    // ====== 認証 ======
    function login(){
      const id = $('loginId').value.trim();
      const pw = $('loginPassword').value;
      if (!id || !pw) return showError('errorMessage','ユーザーIDとパスワードを入力してください');
      if (!users[id] || users[id].password !== pw) return showError('errorMessage','ユーザーIDまたはパスワードが正しくありません');
      currentUser = id; setCurrentUser(id); hideError('errorMessage'); showMain();
    }
    function register(){
      const id = $('registerId').value.trim();
      const pw = $('registerPassword').value;
      const pw2 = $('registerPasswordConfirm').value;
      const company = $('companyName').value.trim();
      if (!id || !pw || !pw2 || !company) return showError('registerErrorMessage','すべての項目を入力してください');
      if (id.length < 4 || id.length > 20) return showError('registerErrorMessage','ユーザーIDは4-20文字で入力してください');
      if (pw.length < 6) return showError('registerErrorMessage','パスワードは6文字以上で入力してください');
      if (pw !== pw2) return showError('registerErrorMessage','パスワードが一致しません');
      if (users[id]) return showError('registerErrorMessage','このユーザーIDは既に使用されています');
      users[id] = { password: pw, company, data: {} };
      persistUsers();
      hideError('registerErrorMessage');
      alert('アカウントが正常に作成されました。ログイン画面に戻ります。');
      $('registerId').value = $('registerPassword').value = $('registerPasswordConfirm').value = $('companyName').value = '';
      showLogin();
    }
    function logout(){
      if (!confirm('ログアウトしますか？')) return;
      saveUserData();
      clearCurrentUser();
      currentUser = null;
      $('loginId').value = '&F001';
      $('loginPassword').value = '0123';
      showLogin();
    }

    function showError(id, msg){ const el = $(id); el.textContent = msg; el.classList.remove('hidden'); }
    function hideError(id){ $(id).classList.add('hidden'); }

    // ====== 年度・テーブル ======
    function updateYear(){ saveUserData(); currentYear = Number($('yearSelect').value) || currentYear; generateTable(); loadUserData(); calculateAll(); }

    function generateTable(){
      const tbody = $('businessData');
      tbody.innerHTML = '';
      months.forEach((m, idx) => {
        const tr = document.createElement('tr');
        const tdMonth = document.createElement('td'); tdMonth.className = 'month-cell'; tdMonth.textContent = m; tr.appendChild(tdMonth);
        // 売上
        ['productSales','serviceSales','otherIncome'].forEach(key => {
          const td = document.createElement('td'); td.className = 'input-cell';
          const inp = document.createElement('input'); inp.type='number'; inp.min='0'; inp.id=`${key}_${idx}`; inp.placeholder='0'; inp.addEventListener('input', calculateAll);
          td.appendChild(inp); tr.appendChild(td);
        });
        const tdRev = document.createElement('td'); tdRev.id=`revenueTotal_${idx}`; tdRev.className='calculated-cell'; tdRev.textContent='¥0'; tr.appendChild(tdRev);
        // 費用
        ['cogs','personnel','rent','advertising','communication','utilities','transport','otherExpenses'].forEach(key => {
          const td = document.createElement('td'); td.className='input-cell';
          const inp = document.createElement('input'); inp.type='number'; inp.min='0'; inp.id=`${key}_${idx}`; inp.placeholder='0'; inp.addEventListener('input', calculateAll);
          td.appendChild(inp); tr.appendChild(td);
        });
        // 分析
        const tdGross = document.createElement('td'); tdGross.id=`grossProfit_${idx}`; tdGross.className='calculated-cell'; tdGross.textContent='¥0'; tr.appendChild(tdGross);
        const tdOp = document.createElement('td'); tdOp.id=`operatingProfit_${idx}`; tdOp.className='calculated-cell'; tdOp.textContent='¥0'; tr.appendChild(tdOp);
        const tdCum = document.createElement('td'); tdCum.id=`cumulative_${idx}`; tdCum.className='calculated-cell'; tdCum.textContent='¥0'; tr.appendChild(tdCum);
        tbody.appendChild(tr);
      });
      // 合計行
      const totalRow = document.createElement('tr'); totalRow.className='total-row';
      totalRow.innerHTML = `
        <td>年間合計</td>
        <td id="totalProductSales">¥0</td>
        <td id="totalServiceSales">¥0</td>
        <td id="totalOtherIncome">¥0</td>
        <td id="yearRevenueTotal">¥0</td>
        <td id="totalCogs">¥0</td>
        <td id="totalPersonnel">¥0</td>
        <td id="totalRent">¥0</td>
        <td id="totalAdvertising">¥0</td>
        <td id="totalCommunication">¥0</td>
        <td id="totalUtilities">¥0</td>
        <td id="totalTransport">¥0</td>
        <td id="totalOtherExpenses">¥0</td>
        <td id="yearGrossProfit">¥0</td>
        <td id="yearOperatingProfit">¥0</td>
        <td id="finalProfit">¥0</td>`;
      tbody.appendChild(totalRow);
    }

    // ====== 計算 ======
    function calculateAll(){
      let yearRevenue=0, yearCogs=0, yearOpex=0, cumulative=0;
      const totals = { productSales:0, serviceSales:0, otherIncome:0, cogs:0, personnel:0, rent:0, advertising:0, communication:0, utilities:0, transport:0, otherExpenses:0 };
      months.forEach((_, idx) => {
        const ps = toNum($(`productSales_${idx}`).value);
        const ss = toNum($(`serviceSales_${idx}`).value);
        const oi = toNum($(`otherIncome_${idx}`).value);
        const rev = ps + ss + oi;
        $(`revenueTotal_${idx}`).textContent = formatCurrency(rev);

        const cogs = toNum($(`cogs_${idx}`).value);
        const per = toNum($(`personnel_${idx}`).value);
        const rnt = toNum($(`rent_${idx}`).value);
        const adv = toNum($(`advertising_${idx}`).value);
        const com = toNum($(`communication_${idx}`).value);
        const utl = toNum($(`utilities_${idx}`).value);
        const trn = toNum($(`transport_${idx}`).value);
        const oth = toNum($(`otherExpenses_${idx}`).value);
        const opex = per + rnt + adv + com + utl + trn + oth;

        const gross = rev - cogs;
        const op = gross - opex;
        cumulative += op;

        $(`grossProfit_${idx}`).textContent = formatCurrency(gross);
        $(`grossProfit_${idx}`).className = 'calculated-cell ' + getBalanceClass(gross);
        $(`operatingProfit_${idx}`).textContent = formatCurrency(op);
        $(`operatingProfit_${idx}`).className = 'calculated-cell ' + getBalanceClass(op);
        $(`cumulative_${idx}`).textContent = formatCurrency(cumulative);
        $(`cumulative_${idx}`).className = 'calculated-cell ' + getBalanceClass(cumulative);

        yearRevenue += rev; yearCogs += cogs; yearOpex += opex;
        totals.productSales += ps; totals.serviceSales += ss; totals.otherIncome += oi; totals.cogs += cogs; totals.personnel += per; totals.rent += rnt; totals.advertising += adv; totals.communication += com; totals.utilities += utl; totals.transport += trn; totals.otherExpenses += oth;
      });

      const yearGross = yearRevenue - yearCogs;
      const yearOp = yearGross - yearOpex;
      const margin = yearRevenue ? (yearOp / yearRevenue * 100) : 0;

      // 合計行
      Object.entries(totals).forEach(([k,v]) => { const el = document.getElementById('total' + k.charAt(0).toUpperCase() + k.slice(1)); if (el) el.textContent = formatCurrency(v); });
      $('yearRevenueTotal').textContent = formatCurrency(yearRevenue);
      $('yearGrossProfit').textContent = formatCurrency(yearGross); $('yearGrossProfit').className = 'calculated-cell ' + getBalanceClass(yearGross);
      $('yearOperatingProfit').textContent = formatCurrency(yearOp); $('yearOperatingProfit').className = 'calculated-cell ' + getBalanceClass(yearOp);
      $('finalProfit').textContent = formatCurrency(cumulative); $('finalProfit').className = 'calculated-cell ' + getBalanceClass(cumulative);

      // KPI
      $('totalRevenue').textContent = formatCurrency(yearRevenue);
      $('totalCosts').textContent = formatCurrency(yearCogs + yearOpex);
      $('totalProfit').textContent = formatCurrency(yearOp);
      $('profitMargin').textContent = margin.toFixed(1) + '%';
      $('cashFlow').textContent = formatCurrency(cumulative);

      updateKPIColors(yearOp, margin, cumulative);
    }

    function updateKPIColors(profit, margin, cash){
      const profitEl = document.querySelector('.kpi-card.profit .amount');
      const marginEl = document.querySelector('.kpi-card.margin .amount');
      const cashEl = document.querySelector('.kpi-card.cash .amount');
      if (profitEl) profitEl.style.color = profit >= 0 ? '#27ae60' : '#e74c3c';
      if (marginEl) marginEl.style.color = margin >= 0 ? '#27ae60' : '#e74c3c';
      if (cashEl) cashEl.style.color = cash >= 0 ? '#27ae60' : '#e74c3c';
    }

    // ====== 操作 ======
    function clearAll(){ if (!confirm('すべての経営データをクリアしますか？')) return; document.querySelectorAll('input[type="number"]').forEach(i=>i.value=''); calculateAll(); }
    function saveData(){ saveUserData(); alert(`${currentYear}年度の経営データを保存しました`); }
    function loadData(){ generateTable(); loadUserData(); calculateAll(); alert(`${currentYear}年度の経営データを読み込みました`); }

    function exportToCSV(){
      const companyName = users[currentUser]?.company || '';
      let csv = `${companyName} - ${currentYear}年度 経営管理データ\n\n`;
      csv += '月,商品売上,サービス売上,その他収入,売上合計,仕入原価,人件費,地代家賃,広告宣伝費,通信費,光熱費,交通費,その他経費,売上総利益,営業利益,累計利益\n';
      months.forEach((m, idx) => {
        const row = [
          m,
          $(`productSales_${idx}`).value || '0',
          $(`serviceSales_${idx}`).value || '0',
          $(`otherIncome_${idx}`).value || '0',
          $(`revenueTotal_${idx}`).textContent.replace('¥','').replace(/,/g,''),
          $(`cogs_${idx}`).value || '0',
          $(`personnel_${idx}`).value || '0',
          $(`rent_${idx}`).value || '0',
          $(`advertising_${idx}`).value || '0',
          $(`communication_${idx}`).value || '0',
          $(`utilities_${idx}`).value || '0',
          $(`transport_${idx}`).value || '0',
          $(`otherExpenses_${idx}`).value || '0',
          $(`grossProfit_${idx}`).textContent.replace('¥','').replace(/,/g,''),
          $(`operatingProfit_${idx}`).textContent.replace('¥','').replace(/,/g,''),
          $(`cumulative_${idx}`).textContent.replace('¥','').replace(/,/g,'')
        ];
        csv += row.join(',') + '\n';
      });
      const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
      const a = document.createElement('a');
      a.href = URL.createObjectURL(blob);
      a.download = `${companyName}_経営管理データ_${currentYear}年度.csv`;
      document.body.appendChild(a); a.click(); document.body.removeChild(a);
      URL.revokeObjectURL(a.href);
    }

    // Enterキーでログイン/登録
    document.addEventListener('keydown', (e)=>{
      if (e.key !== 'Enter') return;
      if (!$('loginScreen').classList.contains('hidden')) return login();
      if (!$('registerScreen').classList.contains('hidden')) return register();
    });

    // ====== 初期化 ======
    function init(){
      loadUsersFromStorage();
      const savedUser = getCurrentUser();
      currentUser = savedUser || null;
      currentYear = Number($('yearSelect').value) || 2025;
      if (currentUser && users[currentUser]) { showMain(); } else { showLogin(); }
    }
    init();
  </script>
</body>
</html>
