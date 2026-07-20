<!DOCTYPE html>
<html lang="zh-CN">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>智能行程定制系统</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: 'Microsoft YaHei', Arial, sans-serif;
      background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
      color: #333;
      min-height: 100vh;
      position: relative;
    }

    body::before {
      content: '';
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-image: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%239C92AC' fill-opacity='0.05'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
      pointer-events: none;
      z-index: -1;
    }

    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 20px;
    }

    header {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
      padding: 24px 32px;
      border-radius: 12px;
      margin-bottom: 24px;
      box-shadow: 0 8px 32px rgba(102, 126, 234, 0.3);
    }

    header .logo {
      font-size: 28px;
      font-weight: bold;
      text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    }

    header nav {
      margin-top: 12px;
      display: flex;
      align-items: center;
    }

    header nav a {
      color: white;
      text-decoration: none;
      margin-right: 24px;
      padding: 8px 16px;
      border-radius: 20px;
      transition: all 0.3s ease;
      font-weight: 500;
    }

    header nav a:hover,
    header nav a.active {
      background: rgba(255, 255, 255, 0.2);
      transform: translateY(-2px);
    }

    .section {
      background: rgba(255, 255, 255, 0.95);
      backdrop-filter: blur(10px);
      border-radius: 16px;
      padding: 32px;
      margin-bottom: 24px;
      box-shadow: 0 10px 40px rgba(0, 0, 0, 0.08);
      border: 1px solid rgba(255, 255, 255, 0.5);
    }

    .section h2 {
      color: #2c3e50;
      margin-bottom: 24px;
      font-size: 24px;
      border-bottom: 3px solid #667eea;
      padding-bottom: 12px;
      position: relative;
    }

    .section h2::after {
      content: '';
      position: absolute;
      bottom: -3px;
      left: 0;
      width: 60px;
      height: 3px;
      background: linear-gradient(90deg, #667eea, #764ba2);
      border-radius: 2px;
    }

    .search-box {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
    }

    .search-box input {
      flex: 1;
      padding: 12px;
      border: 2px solid #ddd;
      border-radius: 6px;
      font-size: 14px;
    }

    .search-box button {
      padding: 12px 24px;
      background: #667eea;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-size: 14px;
    }

    .search-box button:hover {
      background: #5a6fd6;
    }

    .card-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
      gap: 24px;
    }

    .card {
      border-radius: 16px;
      overflow: hidden;
      transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
      cursor: pointer;
      background: white;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
    }

    .card:hover {
      transform: translateY(-8px) scale(1.02);
      box-shadow: 0 20px 40px rgba(102, 126, 234, 0.2);
    }

    .card img {
      width: 100%;
      height: 180px;
      object-fit: cover;
      transition: transform 0.4s ease;
    }

    .card:hover img {
      transform: scale(1.1);
    }

    .card-body {
      padding: 20px;
      position: relative;
    }

    .card-title {
      font-size: 18px;
      font-weight: bold;
      margin-bottom: 10px;
      color: #2c3e50;
      transition: color 0.3s ease;
    }

    .card:hover .card-title {
      color: #667eea;
    }

    .card-meta {
      font-size: 13px;
      color: #7f8c8d;
      margin-bottom: 6px;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .card-rating {
      color: #f39c12;
      font-weight: bold;
      font-size: 14px;
      display: flex;
      align-items: center;
      gap: 4px;
    }

    .card-rating::before {
      content: '★';
    }

    .form-group {
      margin-bottom: 16px;
    }

    .form-group label {
      display: block;
      margin-bottom: 6px;
      font-weight: bold;
      color: #555;
    }

    .form-group input,
    .form-group select,
    .form-group textarea {
      width: 100%;
      padding: 10px;
      border: 2px solid #ddd;
      border-radius: 6px;
      font-size: 14px;
    }

    .form-group input:focus,
    .form-group select:focus,
    .form-group textarea:focus {
      outline: none;
      border-color: #667eea;
    }

    .btn {
      padding: 10px 20px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-size: 14px;
      transition: background 0.3s;
    }

    .btn-primary {
      background: #667eea;
      color: white;
    }

    .btn-primary:hover {
      background: #5a6fd6;
    }

    .btn-success {
      background: #27ae60;
      color: white;
    }

    .btn-success:hover {
      background: #219a52;
    }

    .btn-danger {
      background: #e74c3c;
      color: white;
    }

    .btn-danger:hover {
      background: #c0392b;
    }

    .btn-secondary {
      background: #95a5a6;
      color: white;
    }

    .timeline {
      position: relative;
      padding-left: 40px;
    }

    .timeline::before {
      content: '';
      position: absolute;
      left: 15px;
      top: 0;
      bottom: 0;
      width: 2px;
      background: #667eea;
    }

    .timeline-item {
      position: relative;
      margin-bottom: 30px;
    }

    .timeline-item::before {
      content: '';
      position: absolute;
      left: -34px;
      top: 8px;
      width: 16px;
      height: 16px;
      border-radius: 50%;
      background: #667eea;
      border: 4px solid white;
      box-shadow: 0 0 0 2px #667eea;
    }

    .timeline-date {
      font-size: 18px;
      font-weight: bold;
      color: #667eea;
      margin-bottom: 10px;
    }

    .timeline-card {
      background: #f8f9fa;
      border-radius: 8px;
      padding: 16px;
    }

    .timeline-card h4 {
      margin-bottom: 8px;
    }

    .timeline-card .time-slot {
      display: flex;
      justify-content: space-between;
      margin-bottom: 8px;
      padding: 8px;
      background: white;
      border-radius: 4px;
    }

    .modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.5);
      justify-content: center;
      align-items: center;
      z-index: 1000;
    }

    .modal.show {
      display: flex;
    }

    .modal-content {
      background: white;
      padding: 24px;
      border-radius: 8px;
      width: 90%;
      max-width: 500px;
      position: relative;
    }

    .modal-close {
      position: absolute;
      top: 10px;
      right: 10px;
      font-size: 24px;
      cursor: pointer;
    }

    .login-panel {
      max-width: 400px;
      margin: 0 auto;
    }

    .resource-table {
      width: 100%;
      border-collapse: collapse;
    }

    .resource-table th,
    .resource-table td {
      padding: 12px;
      text-align: left;
      border-bottom: 1px solid #ddd;
    }

    .resource-table th {
      background: #f5f5f5;
    }

    .budget-info {
      background: #e8f5e9;
      padding: 12px;
      border-radius: 6px;
      margin-bottom: 16px;
    }

    .budget-info .budget-used {
      color: #27ae60;
      font-weight: bold;
    }

    .budget-info .budget-limit {
      color: #333;
    }

    .swap-btn {
      font-size: 12px;
      padding: 4px 8px;
      margin-left: 8px;
    }

    .nav-tabs {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
    }

    .nav-tabs button {
      padding: 8px 16px;
      border: none;
      border-radius: 4px;
      background: #f0f0f0;
      cursor: pointer;
    }

    .nav-tabs button.active {
      background: #667eea;
      color: white;
    }

    .hot-recommendations {
      margin-bottom: 20px;
    }

    .hot-tags {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }

    .hot-tag {
      padding: 6px 12px;
      background: #ffeaa7;
      color: #d63031;
      border-radius: 20px;
      font-size: 13px;
      cursor: pointer;
    }

    .hot-tag:hover {
      background: #fdcb6e;
    }

    .spot-detail-header {
      display: flex;
      gap: 32px;
      margin-bottom: 32px;
    }

    .spot-detail-image {
      width: 400px;
      height: 280px;
      border-radius: 16px;
      object-fit: cover;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
    }

    .spot-detail-info {
      flex: 1;
    }

    .spot-detail-info h1 {
      font-size: 32px;
      color: #2c3e50;
      margin-bottom: 16px;
    }

    .spot-detail-tags {
      display: flex;
      gap: 8px;
      margin-bottom: 16px;
    }

    .spot-detail-tag {
      padding: 6px 14px;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
      border-radius: 20px;
      font-size: 13px;
    }

    .spot-detail-meta {
      display: flex;
      gap: 24px;
      margin-bottom: 20px;
      font-size: 14px;
      color: #7f8c8d;
    }

    .spot-detail-meta span {
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .spot-detail-desc {
      font-size: 15px;
      line-height: 1.8;
      color: #555;
      margin-bottom: 24px;
    }

    .spot-detail-map {
      height: 400px;
      border-radius: 16px;
      margin-bottom: 24px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
    }

    .route-panel {
      background: #f8f9fa;
      border-radius: 12px;
      padding: 20px;
      margin-bottom: 24px;
    }

    .route-panel h3 {
      margin-bottom: 16px;
      color: #2c3e50;
    }

    .route-select {
      display: flex;
      gap: 12px;
      margin-bottom: 16px;
    }

    .route-select select {
      padding: 10px 16px;
      border: 2px solid #ddd;
      border-radius: 8px;
      font-size: 14px;
    }

    .route-result {
      background: white;
      border-radius: 8px;
      padding: 16px;
    }

    .route-summary {
      display: flex;
      justify-content: space-between;
      padding: 12px;
      background: #e8f5e9;
      border-radius: 6px;
      margin-bottom: 12px;
    }

    .route-summary-item {
      text-align: center;
    }

    .route-summary-item .label {
      font-size: 12px;
      color: #7f8c8d;
      margin-bottom: 4px;
    }

    .route-summary-item .value {
      font-size: 18px;
      font-weight: bold;
      color: #27ae60;
    }

    .route-steps {
      max-height: 200px;
      overflow-y: auto;
    }

    .route-step {
      display: flex;
      gap: 12px;
      padding: 10px 0;
      border-bottom: 1px solid #eee;
    }

    .route-step:last-child {
      border-bottom: none;
    }

    .route-step-icon {
      width: 32px;
      height: 32px;
      border-radius: 50%;
      background: #667eea;
      color: white;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 14px;
      flex-shrink: 0;
    }

    .route-step-info {
      flex: 1;
    }

    .route-step-info .name {
      font-weight: bold;
      color: #2c3e50;
    }

    .route-step-info .desc {
      font-size: 12px;
      color: #7f8c8d;
      margin-top: 4px;
    }

    .back-btn {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 10px 20px;
      background: #95a5a6;
      color: white;
      border-radius: 8px;
      text-decoration: none;
      transition: all 0.3s ease;
    }

    .back-btn:hover {
      background: #7f8c8d;
      transform: translateY(-2px);
    }

    .spot-detail-actions {
      display: flex;
      gap: 12px;
      margin-bottom: 24px;
    }
  </style>
  <script src="https://webapi.amap.com/maps?v=2.0&key=您的高德地图API密钥"></script>
</head>

<body>
  <div class="container">
    <header>
      <div class="logo">智能行程定制系统</div>
      <nav>
        <a href="#home" class="active" onclick="showSection('home')">首页</a>
        <a href="#customize" onclick="showSection('customize')">智能定制</a>
        <a href="#itinerary" onclick="showSection('itinerary')">行程展示</a>
        <a href="#admin" onclick="showSection('admin')">资源管理</a>
        <span id="user-info" style="float: right; font-size: 14px;">
          <button onclick="showLoginModal()" id="login-btn">登录/注册</button>
          <span id="welcome-text" style="display: none;">欢迎, <span id="current-user"></span> <button onclick="logout()"
              style="background: none; border: none; color: white; text-decoration: underline;">退出</button></span>
        </span>
      </nav>
    </header>

    <div id="home" class="section">
      <h2>热门推荐</h2>
      <div class="hot-recommendations">
        <div class="search-box">
          <input type="text" id="search-input" placeholder="搜索目的地、景点...">
          <button onclick="search()">搜索</button>
        </div>
        <div class="hot-tags">
          <span class="hot-tag" onclick="searchByTag('历史古迹')">历史古迹</span>
          <span class="hot-tag" onclick="searchByTag('自然风光')">自然风光</span>
          <span class="hot-tag" onclick="searchByTag('美食')">美食</span>
          <span class="hot-tag" onclick="searchByTag('购物')">购物</span>
          <span class="hot-tag" onclick="searchByTag('北京')">北京</span>
          <span class="hot-tag" onclick="searchByTag('上海')">上海</span>
        </div>
      </div>
      <div id="recommend-list" class="card-grid"></div>
    </div>

    <div id="customize" class="section" style="display: none;">
      <h2>智能行程定制</h2>
      <form id="customize-form">
        <div class="form-group">
          <label>出发地</label>
          <input type="text" id="origin" value="北京" required>
        </div>
        <div class="form-group">
          <label>目的地</label>
          <input type="text" id="destination" value="北京" required>
        </div>
        <div class="form-group">
          <label>游玩天数</label>
          <select id="days" required>
            <option value="1">1天</option>
            <option value="2">2天</option>
            <option value="3" selected>3天</option>
            <option value="4">4天</option>
            <option value="5">5天</option>
            <option value="6">6天</option>
            <option value="7">7天</option>
          </select>
        </div>
        <div class="form-group">
          <label>预算（元）</label>
          <input type="number" id="budget" value="1500" required>
        </div>
        <div class="form-group">
          <label>出发日期</label>
          <input type="date" id="departure-date" required>
        </div>
        <div class="form-group">
          <label>偏好标签</label>
          <select id="preference" required>
            <option value="历史古迹">历史古迹</option>
            <option value="自然风光">自然风光</option>
            <option value="美食">美食</option>
            <option value="购物">购物</option>
            <option value="亲子">亲子</option>
          </select>
        </div>
        <button type="submit" class="btn btn-primary">生成行程</button>
      </form>
    </div>

    <div id="itinerary" class="section" style="display: none;">
      <h2>我的行程</h2>
      <div id="budget-display" class="budget-info" style="display: none;">
        已使用预算：<span class="budget-used" id="budget-used">0</span> / <span class="budget-limit"
          id="budget-limit">0</span> 元
      </div>
      <div id="itinerary-status" style="display: none; margin-bottom: 16px; padding: 12px; border-radius: 6px;"></div>
      <div id="itinerary-content"></div>
      <div id="no-itinerary" style="text-align: center; padding: 40px; color: #999;">
        <p>暂无行程，请先使用智能定制生成行程</p>
        <button class="btn btn-primary" onclick="showSection('customize')">去定制</button>
      </div>
    </div>

    <div id="admin" class="section" style="display: none;">
      <div class="nav-tabs">
        <button class="active" onclick="showAdminTab('spots')">景点管理</button>
        <button onclick="showAdminTab('hotels')">酒店管理</button>
        <button onclick="showAdminTab('transport')">交通管理</button>
      </div>
      <div id="admin-content"></div>
    </div>

    <div id="spot-detail" class="section" style="display: none;">
      <a href="#" class="back-btn" onclick="showSection('home')">
        ← 返回首页
      </a>
      <div id="spot-detail-content"></div>
    </div>
  </div>

  <div id="login-modal" class="modal">
    <div class="modal-content">
      <span class="modal-close" onclick="closeLoginModal()">&times;</span>
      <h2 id="modal-title">用户登录</h2>
      <div class="login-panel">
        <form id="login-form">
          <div class="form-group">
            <label>用户名</label>
            <input type="text" id="username" required>
          </div>
          <div class="form-group">
            <label>密码</label>
            <input type="password" id="password" required>
          </div>
          <button type="submit" class="btn btn-primary" style="width: 100%;">登录</button>
          <p style="text-align: center; margin-top: 10px;">
            <a href="#" onclick="switchToRegister()" style="color: #667eea;">还没有账号？立即注册</a>
          </p>
        </form>
        <form id="register-form" style="display: none;">
          <div class="form-group">
            <label>用户名</label>
            <input type="text" id="reg-username" required>
          </div>
          <div class="form-group">
            <label>密码</label>
            <input type="password" id="reg-password" required>
          </div>
          <div class="form-group">
            <label>确认密码</label>
            <input type="password" id="reg-password2" required>
          </div>
          <button type="submit" class="btn btn-success" style="width: 100%;">注册</button>
          <p style="text-align: center; margin-top: 10px;">
            <a href="#" onclick="switchToLogin()" style="color: #667eea;">已有账号？立即登录</a>
          </p>
        </form>
      </div>
    </div>
  </div>

  <div id="swap-modal" class="modal">
    <div class="modal-content">
      <span class="modal-close" onclick="closeSwapModal()">&times;</span>
      <h2>替换景点</h2>
      <p id="swap-current">当前景点：</p>
      <div class="form-group">
        <label>选择替换景点</label>
        <select id="swap-select"></select>
      </div>
      <button class="btn btn-primary" onclick="confirmSwap()">确认替换</button>
    </div>
  </div>

  <div id="add-resource-modal" class="modal">
    <div class="modal-content">
      <span class="modal-close" onclick="closeAddResourceModal()">&times;</span>
      <h2 id="add-resource-title">新增景点</h2>
      <form id="add-resource-form">
        <div class="form-group">
          <label>名称</label>
          <input type="text" id="res-name" required>
        </div>
        <div class="form-group">
          <label>类型</label>
          <select id="res-type">
            <option value="历史古迹">历史古迹</option>
            <option value="自然风光">自然风光</option>
            <option value="美食">美食</option>
            <option value="购物">购物</option>
            <option value="亲子">亲子</option>
          </select>
        </div>
        <div class="form-group">
          <label>地址</label>
          <input type="text" id="res-address">
        </div>
        <div class="form-group">
          <label>评分</label>
          <input type="number" id="res-rating" step="0.1" min="0" max="5">
        </div>
        <div class="form-group">
          <label>价格（元）</label>
          <input type="number" id="res-cost">
        </div>
        <div class="form-group">
          <label>坐标（经度,纬度）</label>
          <input type="text" id="res-coords" placeholder="116.4074,39.9042">
        </div>
        <div class="form-group">
          <label>简介</label>
          <textarea id="res-desc" rows="3"></textarea>
        </div>
        <button type="submit" class="btn btn-success">保存</button>
      </form>
    </div>
  </div>

  <script>
    const scenicSpots = [
      { id: 1, name: '故宫博物院', type: '历史古迹', address: '北京市东城区景山前街4号', rating: 4.9, cost: 60, coords: [116.3974, 39.9163], description: '世界上现存规模最大、保存最为完整的木质结构古建筑群' },
      { id: 2, name: '八达岭长城', type: '历史古迹', address: '北京市延庆区军都山关沟古道北口', rating: 4.8, cost: 40, coords: [115.9841, 40.3576], description: '万里长城的重要组成部分，是明长城的一个隘口' },
      { id: 3, name: '颐和园', type: '历史古迹', address: '北京市海淀区新建宫门路19号', rating: 4.8, cost: 30, coords: [116.2755, 39.9999], description: '中国现存规模最大、保存最完整的皇家园林' },
      { id: 4, name: '天坛公园', type: '历史古迹', address: '北京市东城区天坛东里甲1号', rating: 4.7, cost: 15, coords: [116.4147, 39.8822], description: '明清两代皇帝祭天祈谷的场所' },
      { id: 5, name: '圆明园', type: '历史古迹', address: '北京市海淀区清华西路28号', rating: 4.6, cost: 25, coords: [116.2789, 40.0062], description: '清代大型皇家园林，被誉为"万园之园"' },
      { id: 6, name: '鸟巢', type: '自然风光', address: '北京市朝阳区国家体育场南路1号', rating: 4.5, cost: 50, coords: [116.3966, 39.9927], description: '2008年北京奥运会主体育场' },
      { id: 7, name: '水立方', type: '自然风光', address: '北京市朝阳区天辰东路11号', rating: 4.4, cost: 30, coords: [116.3988, 39.9918], description: '2008年北京奥运会游泳比赛场馆' },
      { id: 8, name: '香山公园', type: '自然风光', address: '北京市海淀区买卖街40号', rating: 4.5, cost: 10, coords: [116.1971, 39.9998], description: '著名的皇家园林，秋季红叶闻名' },
      { id: 9, name: '什刹海', type: '自然风光', address: '北京市西城区地安门西大街', rating: 4.6, cost: 0, coords: [116.3822, 39.9392], description: '北京城内面积最大、风貌保存最完整的一片历史街区' },
      { id: 10, name: '南锣鼓巷', type: '购物', address: '北京市东城区南锣鼓巷', rating: 4.3, cost: 0, coords: [116.4007, 39.9372], description: '北京最古老的街区之一，充满老北京风情' },
      { id: 11, name: '王府井', type: '购物', address: '北京市东城区王府井大街', rating: 4.4, cost: 0, coords: [116.4174, 39.9142], description: '北京最著名的商业街' },
      { id: 12, name: '前门大街', type: '购物', address: '北京市东城区前门大街', rating: 4.2, cost: 0, coords: [116.4039, 39.9017], description: '北京著名的传统商业街' },
      { id: 13, name: '三里屯', type: '购物', address: '北京市朝阳区三里屯路', rating: 4.5, cost: 0, coords: [116.4478, 39.9356], description: '北京时尚潮流聚集地' },
      { id: 14, name: '北海公园', type: '自然风光', address: '北京市西城区文津街1号', rating: 4.7, cost: 10, coords: [116.3912, 39.9258], description: '中国现存最古老、最完整、最具综合性的皇家园林' },
      { id: 15, name: '明十三陵', type: '历史古迹', address: '北京市昌平区十三陵镇', rating: 4.5, cost: 130, coords: [116.2234, 40.2483], description: '明朝十三位皇帝的陵墓群' },
      { id: 16, name: '北京动物园', type: '亲子', address: '北京市西城区西直门外大街137号', rating: 4.6, cost: 15, coords: [116.3482, 39.9456], description: '中国开放最早、饲养动物种类最多的动物园' },
      { id: 17, name: '欢乐谷', type: '亲子', address: '北京市朝阳区东四环小武基北路', rating: 4.4, cost: 299, coords: [116.5218, 39.8822], description: '大型主题公园' },
      { id: 18, name: '国家博物馆', type: '历史古迹', address: '北京市东城区东长安街16号', rating: 4.8, cost: 0, coords: [116.4119, 39.9083], description: '中国最大的综合性博物馆' },
      { id: 19, name: '奥林匹克森林公园', type: '自然风光', address: '北京市朝阳区科荟路33号', rating: 4.6, cost: 0, coords: [116.3981, 40.0215], description: '亚洲最大的城市绿化景观' },
      { id: 20, name: '后海', type: '美食', address: '北京市西城区后海北沿', rating: 4.5, cost: 0, coords: [116.3845, 39.9347], description: '北京著名的酒吧街和美食街' }
    ];

    const hotels = [
      { id: 1, name: '北京饭店', address: '北京市东城区东长安街33号', rating: 4.7, cost: 800, coords: [116.4115, 39.9092] },
      { id: 2, name: '王府井大饭店', address: '北京市东城区王府井大街57号', rating: 4.5, cost: 600, coords: [116.4156, 39.9123] },
      { id: 3, name: '如家快捷酒店', address: '北京市朝阳区建国路88号', rating: 4.0, cost: 200, coords: [116.4725, 39.9018] },
      { id: 4, name: '7天连锁酒店', address: '北京市海淀区中关村大街27号', rating: 3.9, cost: 180, coords: [116.3199, 39.9898] },
      { id: 5, name: '全季酒店', address: '北京市西城区西直门外大街1号', rating: 4.4, cost: 400, coords: [116.3478, 39.9485] }
    ];

    const transportOptions = [
      { id: 1, type: '地铁', costPerKm: 3, speed: 30 },
      { id: 2, type: '公交', costPerKm: 2, speed: 15 },
      { id: 3, type: '出租车', costPerKm: 2.3, speed: 25 },
      { id: 4, type: '步行', costPerKm: 0, speed: 5 }
    ];

    let currentUser = null;
    let users = JSON.parse(localStorage.getItem('users') || '[]');
    let currentItinerary = null;
    let swapDayIndex = null;
    let swapSlotIndex = null;
    let editingResourceId = null;

    function showSection(sectionId) {
      document.querySelectorAll('.section').forEach(el => el.style.display = 'none');
      document.getElementById(sectionId).style.display = 'block';
      document.querySelectorAll('header nav a').forEach(el => el.classList.remove('active'));
      document.querySelector(`header nav a[href="#${sectionId}"]`).classList.add('active');

      if (sectionId === 'home') renderRecommendations();
      if (sectionId === 'itinerary') renderItinerary();
      if (sectionId === 'admin') showAdminTab('spots');
    }

    function renderRecommendations(filter = '') {
      const list = document.getElementById('recommend-list');
      const filtered = scenicSpots.filter(s =>
        s.name.includes(filter) ||
        s.type.includes(filter) ||
        s.address.includes(filter)
      );

      list.innerHTML = filtered.map(spot => `
                <div class="card" onclick="showSpotDetail(${spot.id})">
                    <img src="https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=${encodeURIComponent(spot.name + ' 风景照片')}&image_size=landscape_4_3" alt="${spot.name}" onerror="this.src='data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%27280%27 height=%27160%27 viewBox=%270 0 280 160%27%3E%3Crect fill=%27%23e0e0e0%27 width=%27280%27 height=%27160%27/%3E%3Ctext fill=%27%23999%27 font-family=%27Arial%27 font-size=%2714%27 x=%2750%25%27 y=%2750%25%27 text-anchor=%27middle%27 dominant-baseline=%27middle%27%3E${encodeURIComponent(spot.name)}%3C/text%3E%3C/svg%3E'">
                    <div class="card-body">
                        <div class="card-title">${spot.name}</div>
                        <div class="card-meta">${spot.type} | ${spot.address}</div>
                        <div class="card-meta">门票：¥${spot.cost}</div>
                        <div class="card-rating">★ ${spot.rating}</div>
                    </div>
                </div>
            `).join('');
    }

    function search() {
      const keyword = document.getElementById('search-input').value;
      renderRecommendations(keyword);
    }

    function searchByTag(tag) {
      document.getElementById('search-input').value = tag;
      renderRecommendations(tag);
    }

    let currentMap = null;
    let currentRoute = null;

    function showSpotDetail(id) {
      const spot = scenicSpots.find(s => s.id === id);
      if (!spot) return;

      document.querySelectorAll('.section').forEach(el => el.style.display = 'none');
      document.querySelectorAll('header nav a').forEach(el => el.classList.remove('active'));
      document.getElementById('spot-detail').style.display = 'block';

      const content = document.getElementById('spot-detail-content');
      content.innerHTML = `
        <div class="spot-detail-header">
          <img src="https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=${encodeURIComponent(spot.name + ' 风景照片')}&image_size=landscape_4_3" 
               alt="${spot.name}" class="spot-detail-image"
               onerror="this.src='data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%27400%27 height=%27280%27 viewBox=%270 0 400 280%27%3E%3Crect fill=%27%23e0e0e0%27 width=%27400%27 height=%27280%27/%3E%3Ctext fill=%27%23999%27 font-family=%27Arial%27 font-size=%2720%27 x=%2750%25%27 y=%2750%25%27 text-anchor=%27middle%27 dominant-baseline=%27middle%27%3E${encodeURIComponent(spot.name)}%3C/text%3E%3C/svg%3E'">
          <div class="spot-detail-info">
            <h1>${spot.name}</h1>
            <div class="spot-detail-tags">
              <span class="spot-detail-tag">${spot.type}</span>
              <span class="spot-detail-tag">★ ${spot.rating}</span>
            </div>
            <div class="spot-detail-meta">
              <span>📍 ${spot.address}</span>
              <span>💰 ¥${spot.cost}</span>
            </div>
            <p class="spot-detail-desc">${spot.description}</p>
            <div class="spot-detail-actions">
              <button class="btn btn-primary" onclick="addToItinerary(${spot.id})">添加到行程</button>
            </div>
          </div>
        </div>
        
        <h2>📍 位置地图</h2>
        <div id="spot-map" class="spot-detail-map"></div>
        
        <div class="route-panel">
          <h3>🗺️ 路线规划</h3>
          <div class="route-select">
            <select id="route-start">
              <option value="">选择起点</option>
              ${scenicSpots.filter(s => s.id !== spot.id).map(s =>
        `<option value="${s.id}">${s.name}</option>`
      ).join('')}
            </select>
            <select id="route-mode">
              <option value="driving">驾车</option>
              <option value="transit">公共交通</option>
              <option value="walking">步行</option>
            </select>
            <button class="btn btn-primary" onclick="searchRoute(${spot.id})">查询路线</button>
          </div>
          <div id="route-result" class="route-result" style="display: none;"></div>
        </div>
      `;

      initMap(spot);
    }

    function initMap(spot) {
      if (!window.AMap) {
        document.getElementById('spot-map').innerHTML = '<div style="text-align: center; padding: 100px; color: #999;">高德地图加载失败，请检查网络或API密钥配置</div>';
        return;
      }

      if (currentMap) {
        currentMap.destroy();
      }

      currentMap = new AMap.Map('spot-map', {
        zoom: 14,
        center: spot.coords,
        mapStyle: 'amap://styles/whitesmoke'
      });

      const marker = new AMap.Marker({
        position: spot.coords,
        title: spot.name,
        icon: new AMap.Icon({
          size: new AMap.Size(40, 40),
          image: 'https://webapi.amap.com/theme/v1.3/markers/n/mark_b.png',
          imageSize: new AMap.Size(40, 40)
        })
      });

      marker.setMap(currentMap);

      const infoWindow = new AMap.InfoWindow({
        title: spot.name,
        content: `
          <div style="padding: 10px;">
            <p style="margin-bottom: 5px;">类型：${spot.type}</p>
            <p style="margin-bottom: 5px;">地址：${spot.address}</p>
            <p>评分：★ ${spot.rating}</p>
          </div>
        `,
        offset: new AMap.Pixel(0, -10)
      });

      marker.on('click', function () {
        infoWindow.open(currentMap, spot.coords);
      });

      infoWindow.open(currentMap, spot.coords);
    }

    function searchRoute(destId) {
      const destSpot = scenicSpots.find(s => s.id === destId);
      const startId = document.getElementById('route-start').value;
      const mode = document.getElementById('route-mode').value;

      if (!startId) {
        alert('请选择起点');
        return;
      }

      const startSpot = scenicSpots.find(s => s.id === parseInt(startId));
      const resultDiv = document.getElementById('route-result');
      resultDiv.style.display = 'block';
      resultDiv.innerHTML = '<div style="text-align: center; padding: 20px;">正在规划路线...</div>';

      if (!window.AMap) {
        resultDiv.innerHTML = '<div style="text-align: center; padding: 20px; color: #999;">高德地图API未加载</div>';
        return;
      }

      AMap.plugin(['AMap.Driving', 'AMap.Transfer', 'AMap.Walking'], function () {
        let planner;

        if (mode === 'driving') {
          planner = new AMap.Driving({
            map: currentMap,
            panel: 'route-result'
          });
        } else if (mode === 'transit') {
          planner = new AMap.Transfer({
            map: currentMap,
            panel: 'route-result',
            city: '北京'
          });
        } else {
          planner = new AMap.Walking({
            map: currentMap,
            panel: 'route-result'
          });
        }

        planner.search(startSpot.coords, destSpot.coords, function (status, result) {
          if (status === 'complete') {
            if (mode === 'driving') {
              const route = result.routes[0];
              if (route) {
                resultDiv.innerHTML = `
                  <div class="route-summary">
                    <div class="route-summary-item">
                      <div class="label">距离</div>
                      <div class="value">${(route.distance / 1000).toFixed(1)}km</div>
                    </div>
                    <div class="route-summary-item">
                      <div class="label">时间</div>
                      <div class="value">${Math.round(route.time / 60)}分钟</div>
                    </div>
                    <div class="route-summary-item">
                      <div class="label">费用</div>
                      <div class="value">¥${Math.round(route.distance / 1000 * 2.3)}</div>
                    </div>
                  </div>
                  <div class="route-steps">
                    ${route.steps.map((step, idx) => `
                      <div class="route-step">
                        <div class="route-step-icon">${idx + 1}</div>
                        <div class="route-step-info">
                          <div class="name">${step.action} ${step.road || ''}</div>
                          <div class="desc">${(step.distance / 1000).toFixed(1)}km，约${Math.round(step.time / 60)}分钟</div>
                        </div>
                      </div>
                    `).join('')}
                  </div>
                `;
              }
            } else if (mode === 'transit') {
              const plan = result.plans[0];
              if (plan) {
                resultDiv.innerHTML = `
                  <div class="route-summary">
                    <div class="route-summary-item">
                      <div class="label">距离</div>
                      <div class="value">${(plan.distance / 1000).toFixed(1)}km</div>
                    </div>
                    <div class="route-summary-item">
                      <div class="label">时间</div>
                      <div class="value">${Math.round(plan.time / 60)}分钟</div>
                    </div>
                    <div class="route-summary-item">
                      <div class="label">票价</div>
                      <div class="value">¥${plan.cost}</div>
                    </div>
                  </div>
                  <div class="route-steps">
                    ${plan.segments.map((seg, idx) => `
                      <div class="route-step">
                        <div class="route-step-icon">${idx + 1}</div>
                        <div class="route-step-info">
                          <div class="name">${seg.type === 'bus' ? '🚌 ' + seg.line.name : seg.type === 'subway' ? '🚇 ' + seg.line.name : '🚶 步行'}</div>
                          <div class="desc">${seg.type === 'subway' ? seg.line.name + '线' : ''} ${seg.steps.length > 0 ? '经过' + seg.steps.length + '站' : ''}</div>
                        </div>
                      </div>
                    `).join('')}
                  </div>
                `;
              }
            } else {
              const route = result.routes[0];
              if (route) {
                resultDiv.innerHTML = `
                  <div class="route-summary">
                    <div class="route-summary-item">
                      <div class="label">距离</div>
                      <div class="value">${(route.distance / 1000).toFixed(1)}km</div>
                    </div>
                    <div class="route-summary-item">
                      <div class="label">时间</div>
                      <div class="value">${Math.round(route.time / 60)}分钟</div>
                    </div>
                  </div>
                  <div class="route-steps">
                    ${route.steps.map((step, idx) => `
                      <div class="route-step">
                        <div class="route-step-icon">${idx + 1}</div>
                        <div class="route-step-info">
                          <div class="name">${step.action}</div>
                          <div class="desc">${(step.distance / 1000).toFixed(1)}km</div>
                        </div>
                      </div>
                    `).join('')}
                  </div>
                `;
              }
            }
          } else {
            resultDiv.innerHTML = '<div style="text-align: center; padding: 20px; color: #e74c3c;">路线规划失败</div>';
          }
        });
      });
    }

    function addToItinerary(spotId) {
      alert('已将景点添加到行程！');
    }

    document.getElementById('customize-form').addEventListener('submit', async (e) => {
      e.preventDefault();

      const params = {
        origin: document.getElementById('origin').value,
        destination: document.getElementById('destination').value,
        days: parseInt(document.getElementById('days').value),
        budget: parseInt(document.getElementById('budget').value),
        departureDate: document.getElementById('departure-date').value,
        preference: document.getElementById('preference').value
      };

      if (!currentUser) {
        alert('请先登录！');
        showLoginModal();
        return;
      }

      const itinerary = await generateItinerary(params);
      currentItinerary = itinerary;
      localStorage.setItem('currentItinerary', JSON.stringify(itinerary));
      showSection('itinerary');
    });

    async function generateItinerary(params) {
      return new Promise(resolve => {
        setTimeout(() => {
          let filteredSpots = scenicSpots.filter(s => {
            if (params.preference === '历史古迹') {
              return s.type === '历史古迹';
            } else if (params.preference === '自然风光') {
              return s.type === '自然风光';
            } else if (params.preference === '美食') {
              return s.type === '美食';
            } else if (params.preference === '购物') {
              return s.type === '购物';
            } else if (params.preference === '亲子') {
              return s.type === '亲子';
            }
            return true;
          });

          filteredSpots = filteredSpots.sort((a, b) => b.rating - a.rating);

          const budgetLimit = params.budget * 1.1;
          const dailySpots = [];
          let totalCost = 0;

          for (let day = 1; day <= params.days; day++) {
            dailySpots.push([]);
          }

          let dayIndex = 0;
          const deferredSpots = [];

          for (let i = 0; i < filteredSpots.length; i++) {
            const spot = filteredSpots[i];

            if (totalCost + spot.cost > budgetLimit) {
              deferredSpots.push(spot);
              continue;
            }

            if (dailySpots[dayIndex].length >= 3) {
              dayIndex++;
              if (dayIndex >= params.days) {
                deferredSpots.push(spot);
                continue;
              }
            }

            const prevSpot = dailySpots[dayIndex][dailySpots[dayIndex].length - 1];
            if (prevSpot) {
              const distance = calculateDistance(prevSpot.coords, spot.coords);
              if (distance > 20) {
                deferredSpots.push(spot);
                continue;
              }
            }

            dailySpots[dayIndex].push(spot);
            totalCost += spot.cost;
          }

          for (const spot of deferredSpots) {
            if (totalCost + spot.cost > budgetLimit) break;

            let placed = false;
            for (let d = 0; d < params.days; d++) {
              if (dailySpots[d].length >= 3) continue;

              const prevSpot = dailySpots[d][dailySpots[d].length - 1];
              if (prevSpot) {
                const distance = calculateDistance(prevSpot.coords, spot.coords);
                if (distance > 20) continue;
              }

              dailySpots[d].push(spot);
              totalCost += spot.cost;
              placed = true;
              break;
            }

            if (!placed) {
              for (let d = 0; d < params.days; d++) {
                if (dailySpots[d].length >= 3) continue;

                dailySpots[d].push(spot);
                totalCost += spot.cost;
                placed = true;
                break;
              }
            }
          }

          const itinerary = {
            id: Date.now(),
            origin: params.origin,
            destination: params.destination,
            days: params.days,
            budget: params.budget,
            departureDate: params.departureDate,
            preference: params.preference,
            totalCost: Math.round(totalCost),
            status: new Date(params.departureDate) < new Date(new Date().toDateString()) ? 'archived' : 'active',
            days: dailySpots.map((spots, idx) => ({
              day: idx + 1,
              spots: spots.map((spot, sIdx) => ({
                ...spot,
                startTime: ['09:00', '11:30', '14:30'][sIdx],
                endTime: ['11:00', '13:30', '17:30'][sIdx],
                transport: sIdx > 0 ? calculateTransport(dailySpots[idx][sIdx - 1], spot) : null
              }))
            }))
          };

          resolve(itinerary);
        }, 500);
      });
    }

    function calculateDistance(coords1, coords2) {
      const [lon1, lat1] = coords1;
      const [lon2, lat2] = coords2;
      const R = 6371;
      const dLat = (lat2 - lat1) * Math.PI / 180;
      const dLon = (lon2 - lon1) * Math.PI / 180;
      const a = Math.sin(dLat / 2) * Math.sin(dLat / 2) +
        Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) *
        Math.sin(dLon / 2) * Math.sin(dLon / 2);
      const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
      return R * c;
    }

    function calculateTransport(spot1, spot2) {
      const distance = calculateDistance(spot1.coords, spot2.coords);
      const transport = distance > 5 ? transportOptions[0] : transportOptions[3];
      return {
        type: transport.type,
        distance: Math.round(distance * 10) / 10,
        time: Math.round(distance / transport.speed * 60),
        cost: Math.round(distance * transport.costPerKm)
      };
    }

    function renderItinerary() {
      const content = document.getElementById('itinerary-content');
      const noItinerary = document.getElementById('no-itinerary');
      const budgetDisplay = document.getElementById('budget-display');
      const statusDisplay = document.getElementById('itinerary-status');

      if (!currentItinerary) {
        content.innerHTML = '';
        noItinerary.style.display = 'block';
        budgetDisplay.style.display = 'none';
        statusDisplay.style.display = 'none';
        return;
      }

      noItinerary.style.display = 'none';
      budgetDisplay.style.display = 'block';
      statusDisplay.style.display = 'block';
      document.getElementById('budget-used').textContent = currentItinerary.totalCost;
      document.getElementById('budget-limit').textContent = currentItinerary.budget;

      const isArchived = currentItinerary.status === 'archived' ||
        (currentItinerary.departureDate &&
          new Date(currentItinerary.departureDate) < new Date(new Date().toDateString()));

      if (isArchived) {
        statusDisplay.textContent = '⚠️ 此行程已归档（出发日期已过），不可编辑';
        statusDisplay.style.background = '#fff3cd';
        statusDisplay.style.color = '#856404';
      } else {
        statusDisplay.textContent = '✓ 行程状态：正常';
        statusDisplay.style.background = '#d4edda';
        statusDisplay.style.color = '#155724';
      }

      content.innerHTML = `
                <div class="timeline">
                    ${currentItinerary.days.map((day, dayIdx) => `
                        <div class="timeline-item">
                            <div class="timeline-date">第 ${day.day} 天</div>
                            <div class="timeline-card">
                                ${day.spots.map((spot, sIdx) => `
                                    <div class="time-slot">
                                        <div>
                                            <strong>${spot.startTime} - ${spot.endTime}</strong>
                                            <h4 style="cursor: pointer; color: #667eea; text-decoration: underline; margin-bottom: 8px;" onclick="showSpotDetail(${spot.id})">${spot.name}</h4>
                                            <p>${spot.type} | ${spot.address} | ¥${spot.cost}</p>
                                            ${spot.transport ? `
                                                <p style="color: #667eea; font-size: 12px;">
                                                    🚇 ${spot.transport.type}：${spot.transport.distance}km，约${spot.transport.time}分钟，¥${spot.transport.cost}
                                                </p>
                                            ` : ''}
                                        </div>
                                        ${isArchived ? '' : `<button class="btn btn-secondary swap-btn" onclick="showSwapModal(${dayIdx}, ${sIdx})">替换</button>`}
                                    </div>
                                `).join('')}
                            </div>
                        </div>
                    `).join('')}
                </div>
            `;
    }

    function showSwapModal(dayIdx, slotIdx) {
      swapDayIndex = dayIdx;
      swapSlotIndex = slotIdx;
      const currentSpot = currentItinerary.days[dayIdx].spots[slotIdx];

      document.getElementById('swap-current').textContent = `当前景点：${currentSpot.name}`;

      const select = document.getElementById('swap-select');
      const availableSpots = scenicSpots.filter(s =>
        s.id !== currentSpot.id &&
        s.type === currentSpot.type
      );

      select.innerHTML = availableSpots.map(s =>
        `<option value="${s.id}">${s.name} (¥${s.cost}, ★${s.rating})</option>`
      ).join('');

      document.getElementById('swap-modal').classList.add('show');
    }

    function closeSwapModal() {
      document.getElementById('swap-modal').classList.remove('show');
    }

    function confirmSwap() {
      const newSpotId = parseInt(document.getElementById('swap-select').value);
      const newSpot = scenicSpots.find(s => s.id === newSpotId);

      if (!newSpot) return;

      const day = currentItinerary.days[swapDayIndex];
      const prevSpot = swapSlotIndex > 0 ? day.spots[swapSlotIndex - 1] : null;
      const nextSpot = swapSlotIndex < day.spots.length - 1 ? day.spots[swapSlotIndex + 1] : null;

      if (prevSpot) {
        const distance = calculateDistance(prevSpot.coords, newSpot.coords);
        if (distance > 20) {
          alert('替换景点与前一个景点距离超过20公里，不符合规则！');
          return;
        }
      }

      if (nextSpot) {
        const distance = calculateDistance(newSpot.coords, nextSpot.coords);
        if (distance > 20) {
          alert('替换景点与后一个景点距离超过20公里，不符合规则！');
          return;
        }
      }

      const oldCost = day.spots[swapSlotIndex].cost;
      day.spots[swapSlotIndex] = {
        ...newSpot,
        startTime: day.spots[swapSlotIndex].startTime,
        endTime: day.spots[swapSlotIndex].endTime,
        transport: prevSpot ? calculateTransport(prevSpot, newSpot) : null
      };

      if (nextSpot) {
        day.spots[swapSlotIndex + 1].transport = calculateTransport(newSpot, nextSpot);
      }

      currentItinerary.totalCost = currentItinerary.totalCost - oldCost + newSpot.cost;
      localStorage.setItem('currentItinerary', JSON.stringify(currentItinerary));

      closeSwapModal();
      renderItinerary();
    }

    function showLoginModal() {
      document.getElementById('login-modal').classList.add('show');
      switchToLogin();
    }

    function closeLoginModal() {
      document.getElementById('login-modal').classList.remove('show');
    }

    function switchToLogin() {
      document.getElementById('modal-title').textContent = '用户登录';
      document.getElementById('login-form').style.display = 'block';
      document.getElementById('register-form').style.display = 'none';
    }

    function switchToRegister() {
      document.getElementById('modal-title').textContent = '用户注册';
      document.getElementById('login-form').style.display = 'none';
      document.getElementById('register-form').style.display = 'block';
    }

    document.getElementById('login-form').addEventListener('submit', (e) => {
      e.preventDefault();
      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;

      const user = users.find(u => u.username === username && u.password === password);
      if (user) {
        currentUser = user;
        localStorage.setItem('currentUser', JSON.stringify(user));
        updateUserInfo();
        closeLoginModal();
        alert('登录成功！');
      } else {
        alert('用户名或密码错误！');
      }
    });

    document.getElementById('register-form').addEventListener('submit', (e) => {
      e.preventDefault();
      const username = document.getElementById('reg-username').value;
      const password = document.getElementById('reg-password').value;
      const password2 = document.getElementById('reg-password2').value;

      if (password !== password2) {
        alert('两次密码不一致！');
        return;
      }

      if (users.find(u => u.username === username)) {
        alert('用户名已存在！');
        return;
      }

      const newUser = { id: Date.now(), username, password };
      users.push(newUser);
      localStorage.setItem('users', JSON.stringify(users));
      currentUser = newUser;
      localStorage.setItem('currentUser', JSON.stringify(newUser));
      updateUserInfo();
      closeLoginModal();
      alert('注册成功！');
    });

    function logout() {
      currentUser = null;
      localStorage.removeItem('currentUser');
      updateUserInfo();
    }

    function updateUserInfo() {
      const loginBtn = document.getElementById('login-btn');
      const welcomeText = document.getElementById('welcome-text');
      const currentUserSpan = document.getElementById('current-user');

      if (currentUser) {
        loginBtn.style.display = 'none';
        welcomeText.style.display = 'inline';
        currentUserSpan.textContent = currentUser.username;
      } else {
        loginBtn.style.display = 'inline';
        welcomeText.style.display = 'none';
      }
    }

    let currentAdminTab = 'spots';

    function showAdminTab(tab) {
      currentAdminTab = tab;
      const content = document.getElementById('admin-content');

      if (tab === 'spots') {
        renderResourceTable(scenicSpots, '景点');
      } else if (tab === 'hotels') {
        renderResourceTable(hotels, '酒店');
      } else if (tab === 'transport') {
        renderResourceTable(transportOptions, '交通方式');
      }

      document.querySelectorAll('.nav-tabs button').forEach(el => el.classList.remove('active'));
      document.querySelector(`.nav-tabs button[onclick="showAdminTab('${tab}')"]`).classList.add('active');
    }

    function renderResourceTable(data, typeName) {
      const content = document.getElementById('admin-content');
      content.innerHTML = `
                <button class="btn btn-success" onclick="showAddResourceModal()">新增${typeName}</button>
                <table class="resource-table">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>名称</th>
                            ${typeName === '景点' ? '<th>类型</th><th>地址</th><th>评分</th><th>价格</th>' : ''}
                            ${typeName === '酒店' ? '<th>地址</th><th>评分</th><th>价格</th>' : ''}
                            ${typeName === '交通方式' ? '<th>类型</th><th>单价</th><th>速度</th>' : ''}
                            <th>操作</th>
                        </tr>
                    </thead>
                    <tbody>
                        ${data.map(item => `
                            <tr>
                                <td>${item.id}</td>
                                <td>${item.name}</td>
                                ${typeName === '景点' ? `
                                    <td>${item.type}</td>
                                    <td>${item.address}</td>
                                    <td>${item.rating}</td>
                                    <td>¥${item.cost}</td>
                                ` : ''}
                                ${typeName === '酒店' ? `
                                    <td>${item.address}</td>
                                    <td>${item.rating}</td>
                                    <td>¥${item.cost}</td>
                                ` : ''}
                                ${typeName === '交通方式' ? `
                                    <td>${item.type}</td>
                                    <td>¥${item.costPerKm}/km</td>
                                    <td>${item.speed}km/h</td>
                                ` : ''}
                                <td>
                                    <button class="btn btn-primary" style="padding: 4px 8px; font-size: 12px;" onclick="editResource(${item.id})">编辑</button>
                                    <button class="btn btn-danger" style="padding: 4px 8px; font-size: 12px;" onclick="deleteResource(${item.id})">删除</button>
                                </td>
                            </tr>
                        `).join('')}
                    </tbody>
                </table>
            `;
    }

    function showAddResourceModal() {
      document.getElementById('add-resource-title').textContent = `新增${currentAdminTab === 'spots' ? '景点' : currentAdminTab === 'hotels' ? '酒店' : '交通方式'}`;
      document.getElementById('add-resource-modal').classList.add('show');
    }

    function closeAddResourceModal() {
      document.getElementById('add-resource-modal').classList.remove('show');
      document.getElementById('add-resource-form').reset();
    }

    document.getElementById('add-resource-form').addEventListener('submit', (e) => {
      e.preventDefault();

      const name = document.getElementById('res-name').value;
      const type = document.getElementById('res-type').value;
      const address = document.getElementById('res-address').value;
      const rating = parseFloat(document.getElementById('res-rating').value);
      const cost = parseFloat(document.getElementById('res-cost').value);
      const coords = document.getElementById('res-coords').value.split(',').map(Number);
      const desc = document.getElementById('res-desc').value;

      if (editingResourceId !== null) {
        if (currentAdminTab === 'spots') {
          const index = scenicSpots.findIndex(s => s.id === editingResourceId);
          if (index > -1) {
            scenicSpots[index] = {
              ...scenicSpots[index],
              name,
              type,
              address,
              rating,
              cost,
              coords,
              description: desc
            };
          }
        } else if (currentAdminTab === 'hotels') {
          const index = hotels.findIndex(h => h.id === editingResourceId);
          if (index > -1) {
            hotels[index] = {
              ...hotels[index],
              name,
              address,
              rating,
              cost,
              coords
            };
          }
        }

        closeAddResourceModal();
        showAdminTab(currentAdminTab);
        alert('编辑成功！');
        editingResourceId = null;
      } else {
        if (currentAdminTab === 'spots') {
          scenicSpots.push({
            id: Date.now(),
            name,
            type,
            address,
            rating,
            cost,
            coords,
            description: desc
          });
        } else if (currentAdminTab === 'hotels') {
          hotels.push({
            id: Date.now(),
            name,
            address,
            rating,
            cost,
            coords
          });
        }

        closeAddResourceModal();
        showAdminTab(currentAdminTab);
        alert('添加成功！');
      }
    });

    function editResource(id) {
      editingResourceId = id;
      let resource;

      if (currentAdminTab === 'spots') {
        resource = scenicSpots.find(s => s.id === id);
      } else if (currentAdminTab === 'hotels') {
        resource = hotels.find(h => h.id === id);
      } else if (currentAdminTab === 'transport') {
        resource = transportOptions.find(t => t.id === id);
      }

      if (!resource) return;

      document.getElementById('add-resource-title').textContent = `编辑${currentAdminTab === 'spots' ? '景点' : currentAdminTab === 'hotels' ? '酒店' : '交通方式'}`;
      document.getElementById('res-name').value = resource.name;
      document.getElementById('res-type').value = resource.type || '历史古迹';
      document.getElementById('res-address').value = resource.address || '';
      document.getElementById('res-rating').value = resource.rating || '';
      document.getElementById('res-cost').value = resource.cost || resource.costPerKm || '';
      document.getElementById('res-coords').value = resource.coords ? resource.coords.join(',') : '';
      document.getElementById('res-desc').value = resource.description || '';

      document.getElementById('add-resource-modal').classList.add('show');
    }

    function deleteResource(id) {
      if (confirm('确定删除该资源吗？')) {
        if (currentAdminTab === 'spots') {
          const index = scenicSpots.findIndex(s => s.id === id);
          if (index > -1) scenicSpots.splice(index, 1);
        } else if (currentAdminTab === 'hotels') {
          const index = hotels.findIndex(h => h.id === id);
          if (index > -1) hotels.splice(index, 1);
        }

        showAdminTab(currentAdminTab);
        alert('删除成功！');
      }
    }

    window.addEventListener('load', () => {
      const savedUser = localStorage.getItem('currentUser');
      if (savedUser) {
        currentUser = JSON.parse(savedUser);
        updateUserInfo();
      }

      const savedItinerary = localStorage.getItem('currentItinerary');
      if (savedItinerary) {
        currentItinerary = JSON.parse(savedItinerary);
      }

      renderRecommendations();
    });
  </script>
</body>

</html>
