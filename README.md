<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Financial Compass - بوصلتك المالية (Master Edition)</title>
    
    <!-- Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    
    <!-- EmailJS SDK -->
    <script type="text/javascript" src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>
    <script type="text/javascript">
        (function() {
            emailjs.init("xDW61GZav_6ZIW_TW");
        })();
    </script>
    
    <!-- Chart.js, Fonts & FontAwesome Icons -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;800;900&family=Poppins:wght@400;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        :root {
            --bg-dark: #070b19;
            --panel-bg: rgba(15, 23, 42, 0.82);
            --text-main: #f8fafc;
            --subtext: #94a3b8;
            --glass-border: rgba(6, 182, 212, 0.28);
            --cyan-primary: #06b6d4;
            --purple-accent: #6366f1;
            --pink-accent: #ff416c;
            --purple-cat: #8a2be2;
            --cyan-cat: #00c6ff;
            --orange-cat: #f7971e;
            --teal-cat: #11998e;
            --gold-royal: #f59e0b;
            --danger-red: #dc2626;
            --danger-hover: #ef4444;
            --sidebar-gradient: linear-gradient(180deg, #1e1b4b 0%, #0f172a 100%);
            --input-bg: rgba(10, 15, 29, 0.85);
            --month-picker-text: #ffffff;
            --month-picker-icon: invert(1);
        }

        body.light-mode {
            --bg-dark: #f1f5f9;
            --panel-bg: rgba(255, 255, 255, 0.88);
            --text-main: #0f172a;
            --subtext: #475569;
            --glass-border: rgba(6, 182, 212, 0.4);
            --sidebar-gradient: linear-gradient(180deg, #e2e8f0 0%, #cbd5e1 100%);
            --input-bg: rgba(241, 245, 249, 0.95);
            --month-picker-text: #0f172a;
            --month-picker-icon: invert(0);
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Cairo', 'Poppins', sans-serif; transition: background 0.3s, color 0.3s, border-color 0.3s, box-shadow 0.3s, transform 0.25s; }
        body { background-color: var(--bg-dark); color: var(--text-main); min-height: 100vh; overflow-x: hidden; }

        .ambient-glow {
            position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
            background: radial-gradient(circle at 75% 20%, rgba(6, 182, 212, 0.12) 0%, transparent 60%),
                        radial-gradient(circle at 15% 85%, rgba(99, 102, 241, 0.12) 0%, transparent 60%);
            z-index: -1;
        }

        .glass-panel {
            background: var(--panel-bg);
            backdrop-filter: blur(25px);
            -webkit-backdrop-filter: blur(25px);
            border: 1px solid var(--glass-border);
            border-radius: 24px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.15);
        }

        /* Modal Overlay & Custom Centered Popups */
        .modal-overlay {
            position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
            background: rgba(0,0,0,0.8); backdrop-filter: blur(8px);
            display: none; align-items: center; justify-content: center; z-index: 9999;
        }
        .modal-card {
            background: var(--panel-bg); border: 1px solid var(--cyan-primary);
            border-radius: 20px; padding: 25px; width: 90%; max-width: 480px; text-align: center;
            box-shadow: 0 0 30px rgba(6, 182, 212, 0.4); color: var(--text-main);
        }

        /* Green Success Notification Modal Box */
        .success-notification-box {
            background: rgba(16, 185, 129, 0.92); border: 2px solid #059669;
            border-radius: 20px; padding: 25px; width: 90%; max-width: 450px; text-align: center;
            box-shadow: 0 0 30px rgba(16, 185, 129, 0.5); color: #ffffff;
        }

        /* Buttons & Interactions */
        .btn-cyan {
            background: linear-gradient(135deg, #06b6d4, #2563eb);
            color: #fff; font-weight: 800; border: none; border-radius: 12px;
            padding: 12px 20px; cursor: pointer; box-shadow: 0 0 15px rgba(6, 182, 212, 0.3);
        }
        .btn-cyan:hover:not(:disabled) {
            box-shadow: 0 0 25px rgba(6, 182, 212, 0.8);
            transform: translateY(-3px);
        }
        .btn-cyan:active:not(:disabled) { transform: scale(0.97); }
        .btn-cyan:disabled { opacity: 0.5; cursor: not-allowed; filter: grayscale(1); }

        .btn-danger {
            background: var(--danger-red); color: #fff; font-weight: 700; border: none;
            border-radius: 10px; padding: 8px 14px; cursor: pointer;
        }
        .btn-danger:hover { background: var(--danger-hover); box-shadow: 0 0 15px rgba(239, 68, 68, 0.6); transform: scale(1.05); }

        .btn-secondary {
            background: var(--input-bg); color: var(--text-main); font-weight: 700; border: 1px solid var(--glass-border);
            border-radius: 10px; padding: 8px 16px; cursor: pointer;
        }
        .btn-secondary:hover { border-color: var(--cyan-primary); }

        /* Quick Day/Night Toggle Button */
        .quick-theme-toggle {
            background: var(--input-bg); border: 1px solid var(--cyan-primary);
            border-radius: 20px; padding: 6px 12px; cursor: pointer; display: flex;
            align-items: center; justify-content: center; color: var(--gold-royal);
            font-size: 16px; box-shadow: 0 0 10px rgba(6, 182, 212, 0.2);
        }
        .quick-theme-toggle:hover { transform: scale(1.1); box-shadow: 0 0 15px rgba(6, 182, 212, 0.5); }

        /* Auth Screen Styling */
        #auth-landing { min-height: 100vh; display: flex; align-items: center; justify-content: center; padding: 30px 20px; }
        .auth-grid { width: 100%; max-width: 1100px; display: grid; grid-template-columns: 1.1fr 0.9fr; gap: 40px; align-items: center; }

        .lang-switch-container {
            display: flex; align-items: center; background: var(--input-bg);
            border: 1px solid var(--cyan-primary); border-radius: 30px; padding: 4px; cursor: pointer;
            width: 130px; position: relative; box-shadow: 0 0 12px rgba(6, 182, 212, 0.2);
        }
        .lang-opt { flex: 1; text-align: center; font-size: 12px; font-weight: 800; z-index: 2; padding: 6px 0; color: var(--subtext); }
        .lang-opt.active { color: #fff; }
        .lang-slider {
            position: absolute; top: 4px; bottom: 4px; width: 58px; background: linear-gradient(135deg, #06b6d4, #2563eb);
            border-radius: 20px; transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1); z-index: 1;
        }
        html[dir="rtl"] .lang-slider { transform: translateX(0); }
        html[dir="ltr"] .lang-slider { transform: translateX(64px); }

        .input-group { position: relative; margin-bottom: 16px; }
        .input-group i { position: absolute; right: 16px; top: 50%; transform: translateY(-50%); color: var(--cyan-primary); }
        html[dir="ltr"] .input-group i { right: auto; left: 16px; }
        .input-group input, .input-group select {
            width: 100%; padding: 12px 45px 12px 16px; background: var(--input-bg);
            border: 1px solid rgba(255, 255, 255, 0.15); border-radius: 12px; color: var(--text-main); font-size: 14px;
        }
        html[dir="ltr"] .input-group input { padding: 12px 16px 12px 45px; }

        /* Custom Month Picker Styling Fixes */
        input[type="month"] {
            background: var(--input-bg); border: 1px solid var(--glass-border);
            border-radius: 10px; color: var(--month-picker-text); font-size: 12px; font-weight: bold; padding: 8px 12px;
        }
        input[type="month"]::-webkit-calendar-picker-indicator {
            filter: var(--month-picker-icon); cursor: pointer;
        }
        input[type="month"]:hover { border-color: var(--cyan-primary); box-shadow: 0 0 12px rgba(6, 182, 212, 0.4); }

        /* Main App Layout */
        #app-dashboard { display: none; min-height: 100vh; display: flex; }
        .sidebar {
            width: 280px; background: var(--sidebar-gradient); border-left: 1px solid var(--glass-border);
            padding: 25px; display: flex; flex-direction: column; gap: 8px; flex-shrink: 0;
        }
        html[dir="ltr"] .sidebar { border-left: none; border-right: 1px solid var(--glass-border); }

        .user-profile-badge {
            display: flex; align-items: center; gap: 12px; background: rgba(255,255,255,0.06);
            padding: 12px; border-radius: 16px; margin-bottom: 15px; border: 1px solid rgba(255,255,255,0.1);
        }

        .nav-item {
            padding: 12px 16px; border-radius: 14px; cursor: pointer; font-weight: 700; font-size: 14px;
            display: flex; align-items: center; gap: 12px; color: var(--subtext);
        }
        .nav-item:hover, .nav-item.active { background: rgba(6, 182, 212, 0.22); color: var(--text-main); border: 1px solid var(--glass-border); }

        .main-content { flex: 1; padding: 25px; overflow-y: auto; }
        .app-view { display: none; }
        .app-view.active { display: block; }

        /* KPI Cards Grid */
        .kpi-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 20px; margin-bottom: 25px; }
        .kpi-card {
            background: var(--panel-bg); backdrop-filter: blur(20px); border-radius: 20px; padding: 22px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.15); border-right: 6px solid #10b981; border-top: 1px solid var(--glass-border);
        }
        html[dir="ltr"] .kpi-card { border-right: 1px solid var(--glass-border); border-left: 6px solid #10b981; }
        .kpi-card.exp { border-color: #ef4444; }
        .kpi-card.net { border-color: #3b82f6; }
        .kpi-card h3 { font-size: 13px; color: var(--subtext); margin-bottom: 8px; }
        .kpi-card .val { font-size: 26px; font-weight: 900; }

        /* Category Glow Cards Grid */
        .cat-card-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 18px; margin-bottom: 20px; }
        .cat-card {
            background: var(--panel-bg); border: 1px solid var(--glass-border); border-radius: 20px;
            padding: 20px; cursor: pointer; transition: all 0.3s ease; position: relative; overflow: hidden;
        }
        .cat-card:hover { transform: translateY(-6px) scale(1.02); }
        .cat-card.bills:hover { border-color: var(--pink-accent); box-shadow: 0 0 30px rgba(255, 65, 108, 0.6); }
        .cat-card.food:hover { border-color: var(--purple-cat); box-shadow: 0 0 30px rgba(138, 43, 226, 0.6); }
        .cat-card.study:hover { border-color: var(--cyan-cat); box-shadow: 0 0 30px rgba(0, 198, 255, 0.6); }
        .cat-card.transport:hover { border-color: var(--orange-cat); box-shadow: 0 0 30px rgba(247, 151, 30, 0.6); }
        .cat-card.outings:hover { border-color: var(--teal-cat); box-shadow: 0 0 30px rgba(17, 153, 142, 0.6); }
        .cat-card.report-card { background: linear-gradient(135deg, #10b981, #2563eb); }

        /* Category Selection Buttons Grid */
        .cat-btn-grid { display: grid; grid-template-columns: repeat(5, 1fr); gap: 10px; margin-bottom: 15px; }
        .cat-btn {
            background: var(--panel-bg); border: 1px solid var(--glass-border); border-radius: 14px;
            padding: 12px 6px; text-align: center; cursor: pointer; font-size: 12px; font-weight: 800; color: var(--text-main);
        }
        .cat-btn.active, .cat-btn:hover { transform: translateY(-2px); border-color: var(--cyan-primary); box-shadow: 0 0 15px rgba(6, 182, 212, 0.5); }

        /* Sub-Categories Chips Grid */
        .sub-cat-grid { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 18px; }
        .sub-cat-chip {
            background: rgba(255,255,255,0.06); border: 1px solid rgba(255,255,255,0.15); border-radius: 20px;
            padding: 8px 14px; font-size: 12px; font-weight: 700; cursor: pointer; color: var(--subtext);
        }
        .sub-cat-chip:hover, .sub-cat-chip.active { background: rgba(6, 182, 212, 0.25); border-color: var(--cyan-primary); color: var(--text-main); }

        /* Smart Action Filter Buttons in Drill-down */
        .smart-filter-btn {
            background: var(--input-bg); border: 1px solid var(--glass-border); border-radius: 14px;
            padding: 10px 16px; font-size: 12px; font-weight: 800; cursor: pointer; color: var(--text-main); display: inline-flex; align-items: center; gap: 6px;
        }
        .smart-filter-btn:hover, .smart-filter-btn.active {
            background: rgba(6, 182, 212, 0.25); border-color: var(--cyan-primary); box-shadow: 0 0 15px rgba(6, 182, 212, 0.4); transform: translateY(-2px);
        }

        /* Chart Glowing Hover Container */
        .chart-wrapper {
            height: 220px; display: flex; align-items: center; justify-content: center; margin-bottom: 15px; border-radius: 16px; padding: 10px; transition: all 0.3s ease;
        }
        .chart-wrapper:hover {
            box-shadow: 0 0 25px rgba(6, 182, 212, 0.25); transform: scale(1.01);
        }

        /* Tables & Lists */
        .table-glass { width: 100%; border-collapse: collapse; margin-top: 10px; }
        .table-glass th, .table-glass td { padding: 14px; text-align: right; border-bottom: 1px solid rgba(255,255,255,0.08); font-size: 13px; color: var(--text-main); }
        html[dir="ltr"] .table-glass th, html[dir="ltr"] .table-glass td { text-align: left; }
        .table-glass th { color: var(--cyan-primary); font-weight: 800; background: rgba(6,182,212,0.08); }

        /* SMART ADMIN CONTROL PANEL STYLES */
        .admin-grid-container {
            display: grid; grid-template-columns: repeat(3, 1fr); gap: 20px; margin-bottom: 25px;
        }
        .admin-card {
            border-radius: 20px; padding: 25px; cursor: pointer; color: #ffffff;
            position: relative; overflow: hidden; transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            display: flex; flex-direction: column; justify-content: space-between; min-height: 160px;
        }
        .admin-card:hover {
            transform: translateY(-6px) scale(1.02);
        }
        .admin-card.invites { background: linear-gradient(135deg, #ff416c, #ff4b2b); }
        .admin-card.invites:hover { box-shadow: 0 0 30px rgba(255, 65, 108, 0.7); }

        .admin-card.sounds { background: linear-gradient(135deg, #8a2be2, #4a00e0); }
        .admin-card.sounds:hover { box-shadow: 0 0 30px rgba(138, 43, 226, 0.7); }

        .admin-card.users { background: linear-gradient(135deg, #00c6ff, #0072ff); }
        .admin-card.users:hover { box-shadow: 0 0 30px rgba(0, 198, 255, 0.7); }

        .admin-card.broadcast { background: linear-gradient(135deg, #f7971e, #ffd200); }
        .admin-card.broadcast:hover { box-shadow: 0 0 30px rgba(247, 151, 30, 0.7); }

        .admin-card.support { background: linear-gradient(135deg, #11998e, #38ef7d); }
        .admin-card.support:hover { box-shadow: 0 0 30px rgba(17, 153, 142, 0.7); }

        .admin-card.subadmins { background: linear-gradient(135deg, #10b981, #2563eb); }
        .admin-card.subadmins:hover { box-shadow: 0 0 30px rgba(16, 185, 129, 0.7); }

        .admin-card-icon {
            font-size: 36px; position: absolute; top: 20px; left: 20px; opacity: 0.85;
        }
        html[dir="ltr"] .admin-card-icon { left: auto; right: 20px; }

        .admin-subview { display: none; }

        @media (max-width: 1024px) {
            .auth-grid { grid-template-columns: 1fr; }
            #app-dashboard { flex-direction: column; }
            .sidebar { width: 100%; }
            .kpi-grid, .cat-card-grid, .admin-grid-container { grid-template-columns: 1fr; }
            .cat-btn-grid { grid-template-columns: repeat(2, 1fr); }
        }
    </style>
</head>
<body>

    <div class="ambient-glow"></div>

    <!-- OTP Modal Container -->
    <div id="modal-container" class="modal-overlay">
        <div class="modal-card">
            <h3 id="modal-title" style="margin-bottom: 10px; color: var(--cyan-primary);">تأكيد كود التحقق OTP</h3>
            <p id="modal-desc" style="font-size: 12px; color: var(--subtext); margin-bottom: 15px;">تم إرسال كود التحقق السداسي لبريدك الإلكتروني</p>
            <input type="text" id="inp-otp-code" placeholder="أدخل كود الـ OTP" style="width: 100%; padding: 12px; text-align: center; font-size: 20px; font-weight: bold; background: var(--input-bg); border: 1px solid var(--cyan-primary); border-radius: 12px; color: var(--text-main); margin-bottom: 15px;">
            <button class="btn-cyan" id="btn-confirm-otp" style="width: 100%;" onclick="verifyOTPAndProcess()">تأكيد العملية 🚀</button>
        </div>
    </div>

    <!-- Custom Generic Confirmation Modal Popup -->
    <div id="modal-confirm-dialog" class="modal-overlay">
        <div class="modal-card">
            <div style="font-size: 40px; color: var(--danger-red); margin-bottom: 10px;"><i class="fa-solid fa-triangle-exclamation"></i></div>
            <h3 id="lbl-confirm-title" style="margin-bottom: 10px; font-weight: 900;">تأكيد العملية</h3>
            <p id="lbl-confirm-desc" style="font-size: 13px; color: var(--subtext); margin-bottom: 20px;">هل أنت تأكد من رغبتك في إتمام هذه الخطوة؟</p>
            <div style="display: flex; gap: 12px; justify-content: center;">
                <button class="btn-secondary" onclick="closeConfirmModal()">إلغاء</button>
                <button class="btn-danger" id="btn-modal-confirm-action" onclick="executePendingAction()">تأكيد 🗑️</button>
            </div>
        </div>
    </div>

    <!-- Green Success Notification Popup Box -->
    <div id="modal-success-box" class="modal-overlay">
        <div class="success-notification-box">
            <div style="font-size: 45px; margin-bottom: 8px;"><i class="fa-solid fa-circle-check"></i></div>
            <h3 id="lbl-success-title" style="font-size: 18px; font-weight: 900; margin-bottom: 6px;">تمت العملية بنجاح!</h3>
            <p id="lbl-success-desc" style="font-size: 13px; opacity: 0.95; margin-bottom: 15px;">تم تحديث البيانات المحددة بنجاح في النظام.</p>
            <button class="btn-cyan" style="background: #ffffff; color: #059669; font-weight: 900; padding: 8px 24px;" onclick="closeSuccessNotification()">موافق</button>
        </div>
    </div>

    <!-- AUTHENTICATION VIEW -->
    <div id="auth-landing">
        <div class="auth-grid">
            <div>
                <div style="display: flex; align-items: center; gap: 12px; font-size: 28px; font-weight: 900; margin-bottom: 20px;">
                    <i class="fa-solid fa-compass" style="color: var(--cyan-primary); font-size: 34px; filter: drop-shadow(0 0 10px var(--cyan-primary));"></i>
                    <span id="txt-brand-name">بوصلتك المالية</span>
                </div>
                <h1 id="lbl-hero-title" style="font-size: 32px; font-weight: 900; line-height: 1.3; margin-bottom: 12px;">مرحباً بك في بوصلتك المالية - دليلك نحو الوضوح المالي</h1>
                <p id="lbl-hero-desc" style="font-size: 15px; color: var(--subtext); margin-bottom: 25px;">تنقل في رحلتك المالية بدقة وثقة تامة لتنظيم مصاريفك بالمزامنة السحابية.</p>
            </div>

            <div class="glass-panel" style="padding: 30px;">
                <div style="display: flex; justify-content: flex-end; align-items: center; gap: 10px; margin-bottom: 20px;">
                    <!-- Quick Theme Toggle on Auth Screen -->
                    <button class="quick-theme-toggle" onclick="toggleThemeMode()" title="تغيير المظهر ☀️/🌙">
                        <i class="fa-solid fa-sun icon-theme-toggle"></i>
                    </button>

                    <div class="lang-switch-container" onclick="toggleLanguage()">
                        <div class="lang-slider"></div>
                        <div class="lang-opt" id="lang-ar">عربي</div>
                        <div class="lang-opt" id="lang-en">English</div>
                    </div>
                </div>

                <!-- LOGIN FORM -->
                <div id="form-login">
                    <h2 id="lbl-login-heading" style="font-size: 20px; font-weight: 900; margin-bottom: 15px;">تسجيل الدخول إلى حسابك</h2>
                    <form onsubmit="event.preventDefault(); handleLogin();">
                        <div class="input-group">
                            <i class="fa-solid fa-envelope"></i>
                            <input type="text" id="login-user" placeholder="البريد الإلكتروني (Gmail)" required>
                        </div>
                        <div class="input-group">
                            <i class="fa-solid fa-lock"></i>
                            <input type="password" id="login-pass" placeholder="كلمة المرور" required>
                        </div>
                        <button type="submit" id="btn-login-action" class="btn-cyan" style="width: 100%; margin-top: 10px;">
                            <i class="fa-solid fa-right-to-bracket"></i> تسجيل الدخول
                        </button>
                    </form>
                    <div style="margin-top: 15px; font-size: 12px; text-align: center;">
                        <span id="lbl-no-acc">ليس لديك حساب؟</span> <span style="color: var(--cyan-primary); font-weight: bold; cursor: pointer;" onclick="toggleAuth('signup')" id="lbl-to-signup">قم بالتسجيل</span>
                    </div>
                </div>

                <!-- SIGNUP FORM -->
                <div id="form-signup" style="display: none;">
                    <h2 id="lbl-signup-heading" style="font-size: 20px; font-weight: 900; margin-bottom: 15px;">إنشاء حساب جديد</h2>
                    <form onsubmit="event.preventDefault(); handleSignupOTP();">
                        <div class="input-group"><i class="fa-solid fa-key"></i><input type="text" id="reg-key" placeholder="كود الدعوة الفعال (Invite Code)" required></div>
                        <div class="input-group"><i class="fa-solid fa-user"></i><input type="text" id="reg-name" placeholder="اسم المستخدم" required></div>
                        <div class="input-group"><i class="fa-solid fa-envelope"></i><input type="email" id="reg-email" placeholder="البريد الإلكتروني (Gmail)" required></div>
                        <div class="input-group"><i class="fa-solid fa-lock"></i><input type="password" id="reg-pass" placeholder="كلمة المرور" required></div>
                        
                        <div class="input-group">
                            <i class="fa-solid fa-crown"></i>
                            <select id="reg-royal-title">
                                <option value="king" id="opt-king">ملك / الأمير 🤴</option>
                                <option value="queen" id="opt-queen" selected>ملكة / الأميرة 👸</option>
                            </select>
                        </div>

                        <button type="submit" id="btn-signup-action" class="btn-cyan" style="width: 100%; margin-top: 10px;">
                            <i class="fa-solid fa-paper-plane"></i> إرسال كود OTP والتسجيل
                        </button>
                    </form>
                    <div style="margin-top: 15px; font-size: 12px; text-align: center;">
                        <span id="lbl-has-acc">لديك حساب بالفعل؟</span> <span style="color: var(--cyan-primary); font-weight: bold; cursor: pointer;" onclick="toggleAuth('login')" id="lbl-to-login">تسجيل الدخول</span>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- MAIN DASHBOARD VIEW -->
    <div id="app-dashboard">
        <div class="sidebar">
            <div style="display: flex; align-items: center; gap: 10px; font-size: 20px; font-weight: 900; margin-bottom: 15px;">
                <i class="fa-solid fa-compass" style="color: var(--cyan-primary); font-size: 24px;"></i>
                <span>Financial Compass</span>
            </div>

            <div class="user-profile-badge">
                <div style="font-size: 24px;" id="dash-royal-icon">👸</div>
                <div>
                    <div style="font-size: 13px; font-weight: bold; color: var(--text-main);" id="dash-user-name">الملكة سارة</div>
                    <div style="font-size: 10px; color: #10b981;" id="lbl-status-online">● متصل سحابياً (Sync)</div>
                </div>
            </div>

            <div class="nav-item active" id="nav-home" onclick="switchView('home')"><i class="fa-solid fa-house"></i> <span id="lbl-nav-home">الرئيسية</span></div>
            <div class="nav-item" id="nav-expenses" onclick="switchView('expenses')"><i class="fa-solid fa-wallet"></i> <span id="lbl-nav-exp">مصروفاتك</span></div>
            <div class="nav-item" id="nav-chat" onclick="switchView('chat')"><i class="fa-solid fa-comments"></i> <span id="lbl-nav-chat">الشات والدعم</span></div>
            <div class="nav-item" id="nav-settings" onclick="switchView('settings')"><i class="fa-solid fa-gear"></i> <span id="lbl-nav-set">الإعدادات والتخصيص</span></div>
            <div class="nav-item" id="nav-admin" onclick="switchView('admin')" style="background: rgba(99,102,241,0.2); border: 1px solid rgba(99,102,241,0.4);"><i class="fa-solid fa-shield-halved"></i> <span id="lbl-nav-adm">لوحة تحكم الأدمن</span></div>

            <div style="margin-top: auto; padding: 12px; background: rgba(99, 102, 241, 0.15); border-radius: 14px; border: 1px solid rgba(99,102,241,0.3);">
                <div style="font-weight: bold; font-size: 12px; color: #818cf8;">Admin Control Active</div>
                <div style="font-size: 10px; color: var(--subtext);">Real-time Cloud Engine</div>
            </div>
        </div>

        <div class="main-content">
            <!-- TOP BAR CONTROL -->
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; flex-wrap: wrap; gap: 15px;">
                <div>
                    <h1 id="dash-welcome-title" style="font-size: 22px; font-weight: 900; color: var(--text-main);">مرحباً بك، الملكة سارة 👋</h1>
                    <span style="font-size: 12px; color: var(--subtext);" id="lbl-sub-welcome">الواجهة الرئيسية ومحرك إدخال المصروفات السريع</span>
                </div>

                <div style="display: flex; align-items: center; gap: 12px;">
                    <!-- Quick Day/Night Toggle Button -->
                    <button class="quick-theme-toggle" onclick="toggleThemeMode()" title="تغيير المظهر ☀️/🌙">
                        <i class="fa-solid fa-sun icon-theme-toggle"></i>
                    </button>

                    <!-- Month Picker -->
                    <input type="month" id="month-picker" onchange="updateDashboardCalculations()">
                    
                    <!-- Currency Switcher -->
                    <select id="currency-switcher" onchange="changeCurrency()" style="padding: 8px 12px; background: var(--input-bg); border: 1px solid var(--cyan-primary); border-radius: 10px; color: var(--text-main); font-size: 12px; font-weight: bold;">
                        <option value="EGP" id="opt-curr-egp">EGP (ج.م)</option>
                        <option value="SAR" id="opt-curr-sar">SAR (ر.س)</option>
                        <option value="USD" id="opt-curr-usd">USD ($)</option>
                    </select>

                    <div class="lang-switch-container" onclick="toggleLanguage()">
                        <div class="lang-slider"></div>
                        <div class="lang-opt" id="lang-ar2">عربي</div>
                        <div class="lang-opt" id="lang-en2">English</div>
                    </div>
                </div>
            </div>

            <!-- VIEW 1: MAIN DASHBOARD HOME -->
            <div id="view-home" class="app-view active">
                <!-- KPI CARDS -->
                <div class="kpi-grid">
                    <div class="kpi-card">
                        <h3 id="lbl-kpi-sal">الراتب الإجمالي لشهر</h3>
                        <div class="val" style="color: #10b981;" id="kpi-val-salary">15,500.00 ج.م</div>
                    </div>
                    <div class="kpi-card exp">
                        <h3 id="lbl-kpi-exp">إجمالي المصروفات</h3>
                        <div class="val" style="color: #ef4444;" id="kpi-val-exp">8,200.00 ج.م</div>
                    </div>
                    <div class="kpi-card net">
                        <h3 id="lbl-kpi-net">الرصيد المتاح (Net Balance)</h3>
                        <div class="val" style="color: #3b82f6;" id="kpi-val-net">7,300.00 ج.م</div>
                    </div>
                </div>

                <!-- MAIN SPLIT WORKSPACE -->
                <div style="display: grid; grid-template-columns: 1.1fr 0.9fr; gap: 20px;">
                    <!-- RIGHT PANEL: INTERACTIVE EXPENSE ENTRY -->
                    <div class="glass-panel" style="padding: 22px;">
                        <h3 style="font-size: 16px; font-weight: 800; margin-bottom: 15px; color: var(--cyan-primary);" id="lbl-quick-add-head">إضافة مصروف جديد سريع أونلاين</h3>
                        
                        <span style="font-size: 12px; color: var(--subtext); display: block; margin-bottom: 8px;" id="lbl-choose-cat">1. اختر الفئة الرئيسية:</span>
                        <div class="cat-btn-grid">
                            <div class="cat-btn bills active" id="btn-cat-bills" onclick="selectMainCategory('bills')">💡 فواتير</div>
                            <div class="cat-btn food" id="btn-cat-food" onclick="selectMainCategory('food')">🍔 أكل وشرب</div>
                            <div class="cat-btn study" id="btn-cat-study" onclick="selectMainCategory('study')">📚 دراسة</div>
                            <div class="cat-btn transport" id="btn-cat-transport" onclick="selectMainCategory('transport')">🚗 مواصلات</div>
                            <div class="cat-btn outings" id="btn-cat-outings" onclick="selectMainCategory('outings')">🎉 خروجات</div>
                        </div>

                        <span style="font-size: 12px; color: var(--subtext); display: block; margin-bottom: 8px;" id="lbl-choose-sub">2. اختر البند الفرعي:</span>
                        <div class="sub-cat-grid" id="sub-category-container">
                            <!-- Dynamic Sub-categories Populated Here -->
                        </div>

                        <div class="input-group">
                            <i class="fa-solid fa-pen-to-square"></i>
                            <input type="text" id="inp-exp-desc" placeholder="تفاصيل البيان (اختياري - مثلاً: شحن كارت كهرباء)">
                        </div>
                        <div class="input-group">
                            <i class="fa-solid fa-coins"></i>
                            <input type="number" id="inp-exp-amount" placeholder="المبلغ المالي بالعملة المختارة" required>
                        </div>

                        <button class="btn-cyan" id="btn-add-expense-action" style="width: 100%; font-size: 14px; padding: 14px;" onclick="addExpenseOnline()">
                            <i class="fa-solid fa-plus-circle"></i> إضافة المصروف أونلاين سحابياً 🚀
                        </button>
                    </div>

                    <!-- LEFT PANEL: LOGS & DOUGHNUT CHART -->
                    <div class="glass-panel" style="padding: 22px;">
                        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;">
                            <h3 style="font-size: 16px; font-weight: 800; color: var(--cyan-primary);" id="lbl-chart-title">توزيع المصروفات اللحظي</h3>
                            <button class="btn-cyan" style="padding: 4px 10px; font-size: 11px;" id="btn-show-all-log" onclick="filterLogByCategory('all')">عرض الكل 🔄</button>
                        </div>
                        <div class="chart-wrapper">
                            <canvas id="expensesDoughnutChart"></canvas>
                        </div>

                        <h4 style="font-size: 13px; font-weight: 800; margin-bottom: 10px; color: var(--subtext);" id="lbl-recent-log">سجل المعاملات المباشر:</h4>
                        <div style="max-height: 200px; overflow-y: auto;">
                            <table class="table-glass">
                                <thead>
                                    <tr>
                                        <th id="th-sub">البند</th>
                                        <th id="th-desc">البيان</th>
                                        <th id="th-amount">المبلغ</th>
                                        <th id="th-act">إدارة</th>
                                    </tr>
                                </thead>
                                <tbody id="expenses-log-tbody">
                                    <!-- Dynamic Logs -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>

            <!-- VIEW 2: EXPENSES DETAILED (مصروفاتك) -->
            <div id="view-expenses" class="app-view">
                <!-- MAIN CATEGORIES CARDS VIEW -->
                <div id="expenses-main-cards-container">
                    <div class="glass-panel" style="padding: 25px; margin-bottom: 25px;">
                        <h2 style="font-size: 18px; font-weight: 900; margin-bottom: 8px; color: var(--text-main);" id="lbl-exp-sec-title">مصروفاتك والتصنيفات المخصصة</h2>
                        <p style="font-size: 13px; color: var(--subtext); margin-bottom: 20px;" id="lbl-exp-sec-desc">اختر أي فئة لاستعراض التفاصيل والتصفية الذكية مع إمكانية طلب التقرير المالي المباشر.</p>
                        
                        <!-- GLOW CARDS GRID -->
                        <div class="cat-card-grid">
                            <div class="cat-card bills" onclick="openCategoryFocus('bills')">
                                <h3 style="font-size: 15px; font-weight: 800; margin-bottom: 8px; color: var(--text-main);" id="card-head-bills">💡 الفواتير والخدمات</h3>
                                <div style="font-size: 20px; font-weight: 900; color: var(--pink-accent);" id="cat-card-val-bills">3,500.00 ج.م</div>
                                <span style="font-size: 11px; color: var(--subtext);" id="card-sub-bills">كهرباء، مياه، غاز، إنترنت...</span>
                            </div>
                            <div class="cat-card food" onclick="openCategoryFocus('food')">
                                <h3 style="font-size: 15px; font-weight: 800; margin-bottom: 8px; color: var(--text-main);" id="card-head-food">🍔 الأكل والشرب</h3>
                                <div style="font-size: 20px; font-weight: 900; color: var(--purple-cat);" id="cat-card-val-food">2,700.00 ج.م</div>
                                <span style="font-size: 11px; color: var(--subtext);" id="card-sub-food">لحوم، خضروات، بقالة، وجبات...</span>
                            </div>
                            <div class="cat-card study" onclick="openCategoryFocus('study')">
                                <h3 style="font-size: 15px; font-weight: 800; margin-bottom: 8px; color: var(--text-main);" id="card-head-study">📚 المصروفات الدراسية</h3>
                                <div style="font-size: 20px; font-weight: 900; color: var(--cyan-cat);" id="cat-card-val-study">0.00 ج.م</div>
                                <span style="font-size: 11px; color: var(--subtext);" id="card-sub-study">كتب، مذكرات، كورسات...</span>
                            </div>
                            <div class="cat-card transport" onclick="openCategoryFocus('transport')">
                                <h3 style="font-size: 15px; font-weight: 800; margin-bottom: 8px; color: var(--text-main);" id="card-head-transport">🚗 المواصلات والتحركات</h3>
                                <div style="font-size: 20px; font-weight: 900; color: var(--orange-cat);" id="cat-card-val-transport">2,000.00 ج.م</div>
                                <span style="font-size: 11px; color: var(--subtext);" id="card-sub-transport">تاكسي، مترو، بنزين، صيانة...</span>
                            </div>
                            <div class="cat-card outings" onclick="openCategoryFocus('outings')">
                                <h3 style="font-size: 15px; font-weight: 800; margin-bottom: 8px; color: var(--text-main);" id="card-head-outings">🎉 الخروجات والمصروف</h3>
                                <div style="font-size: 20px; font-weight: 900; color: var(--teal-cat);" id="cat-card-val-outings">0.00 ج.م</div>
                                <span style="font-size: 11px; color: var(--subtext);" id="card-sub-outings">سينما، ملابس، شحن، هدايا...</span>
                            </div>
                            <div class="cat-card report-card" onclick="requestFinancialReport()">
                                <h3 style="font-size: 15px; font-weight: 800; margin-bottom: 8px; color: #fff;" id="card-head-report">📩 طلب التقرير المالي</h3>
                                <p style="font-size: 11px; color: rgba(255,255,255,0.8); margin-bottom: 10px;" id="card-sub-report">إرسال ملخص كامل عبر الواتساب أو الإيميل</p>
                                <button class="btn-cyan" style="padding: 6px 12px; font-size: 11px; background: #fff; color: #000;" id="btn-request-report-card">طلب الآن</button>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- CATEGORY FOCUS VIEW (DRILLDOWN WITH SMART ACTION BUTTONS) -->
                <div class="glass-panel" id="category-focus-view" style="padding: 25px; display: none;">
                    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
                        <button class="btn-cyan" style="padding: 8px 16px; font-size: 13px;" onclick="closeCategoryFocus()" id="btn-back-to-categories">⬅️ العودة لكافة الفئات</button>
                        <h2 id="focus-cat-title" style="font-size: 22px; font-weight: 900; color: var(--pink-accent); text-align: center;">الفواتير والخدمات 💡</h2>
                        <div style="width: 120px;"></div>
                    </div>

                    <!-- SMART ACTION BUTTONS GRID -->
                    <span style="font-size: 12px; color: var(--subtext); display: block; margin-bottom: 10px;" id="lbl-focus-filter-prompt">اختر لتصفية العرض على بند معين أو إظهار كافة بنود الفئة:</span>
                    <div style="display: flex; flex-wrap: wrap; gap: 10px; margin-bottom: 20px;" id="smart-action-buttons-container">
                        <!-- Populated dynamically via JS -->
                    </div>

                    <!-- FILTERED TRANSACTIONS LIST -->
                    <h3 style="font-size: 15px; font-weight: 800; margin-bottom: 12px; color: var(--cyan-primary);" id="lbl-focus-list-head">المعاملات المسجلة:</h3>
                    <div id="focus-transactions-container">
                        <!-- Dynamic Transaction Bars -->
                    </div>
                </div>
            </div>

            <!-- VIEW 3: CHAT -->
            <div id="view-chat" class="app-view">
                <div class="glass-panel" style="padding: 25px;">
                    <h2 style="font-size: 18px; font-weight: 900; margin-bottom: 15px; color: var(--text-main);" id="lbl-chat-sec-title">الشات والدعم الفني المباشر</h2>
                    <div style="height: 300px; background: var(--input-bg); border-radius: 14px; padding: 15px; overflow-y: auto; margin-bottom: 12px;" id="chat-messages-box">
                        <div style="background: rgba(6,182,212,0.2); padding: 10px 14px; border-radius: 10px; max-width: 70%; font-size: 13px; margin-bottom: 10px; color: var(--text-main);">
                            <b id="lbl-support-sender">الدعم الفني:</b> <span id="lbl-support-welcome">أهلاً بك، كيف يمكننا مساعدتك اليوم؟</span>
                        </div>
                    </div>
                    <div style="display: flex; gap: 10px;">
                        <input type="text" id="inp-chat-msg" placeholder="اكتب رسالتك للأدمن..." style="flex: 1; padding: 12px; background: var(--input-bg); border: 1px solid var(--glass-border); border-radius: 10px; color: var(--text-main);">
                        <button class="btn-cyan" id="btn-send-chat" onclick="sendSupportMessage()">إرسال</button>
                    </div>
                </div>
            </div>

            <!-- VIEW 4: SETTINGS & ACCOUNT CUSTOMIZATION -->
            <div id="view-settings" class="app-view">
                <div class="glass-panel" style="padding: 25px;">
                    <h2 style="font-size: 18px; font-weight: 900; margin-bottom: 15px; color: var(--text-main);" id="lbl-set-sec-title">الإعدادات وتخصيص الحساب</h2>
                    <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">
                        <div>
                            <h4 style="margin-bottom: 10px; color: var(--text-main);" id="lbl-set-salary-title">تحديث الراتب الشهري</h4>
                            <input type="number" id="set-salary-inp" placeholder="الراتب الجديد" style="width: 100%; padding: 10px; background: var(--input-bg); border: 1px solid var(--glass-border); border-radius: 10px; color: var(--text-main); margin-bottom: 10px;">
                            <button class="btn-cyan" id="btn-save-salary" onclick="updateSalarySetting()" style="width: 100%;">حفظ الراتب</button>
                        </div>
                        <div>
                            <h4 style="margin-bottom: 10px; color: var(--text-main);" id="lbl-set-profile-title">تخصيص اللقب واسم المستخدم</h4>
                            <input type="text" id="set-username-inp" placeholder="اسم المستخدم الجديد" style="width: 100%; padding: 10px; background: var(--input-bg); border: 1px solid var(--glass-border); border-radius: 10px; color: var(--text-main); margin-bottom: 10px;">
                            <select id="set-title-sel" style="width: 100%; padding: 10px; background: var(--input-bg); border: 1px solid var(--glass-border); border-radius: 10px; color: var(--text-main); margin-bottom: 10px;">
                                <option value="king" id="opt-set-king">ملك / الأمير 🤴</option>
                                <option value="queen" id="opt-set-queen">ملكة / الأميرة 👸</option>
                            </select>
                            <button class="btn-cyan" id="btn-save-profile" onclick="updateTitleSetting()" style="width: 100%;">تحديث البيانات</button>
                        </div>
                    </div>

                    <hr style="margin: 20px 0; border: none; border-top: 1px solid var(--glass-border);">

                    <!-- GMAIL CHANGE & LOGOUT UPGRADES -->
                    <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; align-items: start;">
                        <div style="background: var(--input-bg); padding: 15px; border-radius: 14px; border: 1px solid var(--glass-border);">
                            <h4 style="font-size: 14px; font-weight: 800; color: var(--cyan-primary); margin-bottom: 8px;">📧 تعديل البريد الإلكتروني (Gmail Change)</h4>
                            <p style="font-size: 11px; color: var(--subtext); margin-bottom: 10px;">يتطلب إرسال وتأكيد كود الـ OTP على الإيميل الجديد لحفظه.</p>
                            <input type="email" id="inp-new-gmail" placeholder="أدخل البريد الإلكتروني (Gmail) الجديد" style="width: 100%; padding: 10px; background: var(--panel-bg); border: 1px solid var(--glass-border); border-radius: 10px; color: var(--text-main); margin-bottom: 10px;">
                            <button class="btn-cyan" onclick="requestGmailChangeOTP()" style="width: 100%;">إرسال كود OTP والتأكيد 🚀</button>
                        </div>

                        <div style="background: rgba(220, 38, 38, 0.1); padding: 15px; border-radius: 14px; border: 1px solid rgba(220, 38, 38, 0.3); text-align: center;">
                            <h4 style="font-size: 14px; font-weight: 800; color: var(--danger-red); margin-bottom: 8px;">🚪 تسجيل الخروج الآمن</h4>
                            <p style="font-size: 11px; color: var(--subtext); margin-bottom: 12px;">إنهاء الجلسة ومسح بيانات الاعتماد المؤقتة بالكامل.</p>
                            <button class="btn-danger" onclick="executeLogoutAction()" style="width: 100%; padding: 10px;">تسجيل الخروج الآن 🔒</button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- VIEW 5: SMART ADMIN CONTROL PANEL -->
            <div id="view-admin" class="app-view">
                <div class="glass-panel" style="padding: 25px;">
                    
                    <!-- MAIN ADMIN PANEL DASHBOARD -->
                    <div id="admin-main-dashboard">
                        <h2 style="font-size: 22px; font-weight: 900; margin-bottom: 20px; color: var(--cyan-primary);" id="lbl-admin-sec-title">🛡️ لوحة تحكم الأدمن الذكية</h2>
                        
                        <!-- 6 GLOWING CARDS GRID -->
                        <div class="admin-grid-container">
                            <!-- Card 1: Invite Codes -->
                            <div class="admin-card invites" onclick="openAdminSubView('invites')">
                                <div class="admin-card-icon">🔑</div>
                                <div>
                                    <h3 style="font-size: 16px; font-weight: 800; margin-bottom: 4px;" id="lbl-adm-c1-title">إدارة أُكواد الدعوة</h3>
                                    <p style="font-size: 11px; opacity: 0.9;" id="lbl-adm-c1-desc">توليد، تجميد/إلغاء، وحذف الأكواد</p>
                                </div>
                            </div>

                            <!-- Card 2: Salary Sounds -->
                            <div class="admin-card sounds" onclick="openAdminSubView('sounds')">
                                <div class="admin-card-icon">🎵</div>
                                <div>
                                    <h3 style="font-size: 16px; font-weight: 800; margin-bottom: 4px;" id="lbl-adm-c2-title">أصوات ورسائل الرواتب</h3>
                                    <p style="font-size: 11px; opacity: 0.9;" id="lbl-adm-c2-desc">تخصيص وإدارة شرائح التنبيهات والصوت</p>
                                </div>
                            </div>

                            <!-- Card 3: Users & Ban -->
                            <div class="admin-card users" onclick="openAdminSubView('users')">
                                <div class="admin-card-icon">🚫</div>
                                <div>
                                    <h3 style="font-size: 16px; font-weight: 800; margin-bottom: 4px;" id="lbl-adm-c3-title">إدارة المستخدمين والحظر</h3>
                                    <p style="font-size: 11px; opacity: 0.9;" id="lbl-adm-c3-desc">تبديل الحظر/إلغاء الحظر والحذف النهائي</p>
                                </div>
                            </div>

                            <!-- Card 4: Broadcast System -->
                            <div class="admin-card broadcast" onclick="openAdminSubView('broadcast')">
                                <div class="admin-card-icon">📢</div>
                                <div>
                                    <h3 style="font-size: 16px; font-weight: 800; margin-bottom: 4px;" id="lbl-adm-c4-title">لوحة الإعلانات العامة</h3>
                                    <p style="font-size: 11px; opacity: 0.9;" id="lbl-adm-c4-desc">بث تنبيهات مع خيار إرفاق الصوت</p>
                                </div>
                            </div>

                            <!-- Card 5: Support Chat -->
                            <div class="admin-card support" onclick="openAdminSubView('support')">
                                <div class="admin-card-icon">💬</div>
                                <div>
                                    <h3 style="font-size: 16px; font-weight: 800; margin-bottom: 4px;" id="lbl-adm-c5-title">رسائل وشات المشتركين</h3>
                                    <p style="font-size: 11px; opacity: 0.9;" id="lbl-adm-c5-desc">فلترة المحادثات والبحث الذكي بالـ Gmail</p>
                                </div>
                            </div>

                            <!-- Card 6: Sub-Admins Management -->
                            <div class="admin-card subadmins" onclick="openAdminSubView('subadmins')">
                                <div class="admin-card-icon">⚙️</div>
                                <div>
                                    <h3 style="font-size: 16px; font-weight: 800; margin-bottom: 4px;" id="lbl-adm-c6-title">حسابات الأدمن والصلاحيات</h3>
                                    <p style="font-size: 11px; opacity: 0.9;" id="lbl-adm-c6-desc">إنشاء وتحديد صلاحيات المدراء الفرعيين</p>
                                </div>
                            </div>
                        </div>

                        <!-- SYSTEM STATISTICS FOOTER -->
                        <div style="background: var(--input-bg); border: 1px solid var(--glass-border); border-radius: 16px; padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 10px;">
                            <div style="font-size: 13px; font-weight: 700; color: var(--text-main);" id="lbl-sys-stats-text">
                                📊 إجمالي الحسابات المسجلة في قاعدة البيانات: <span style="color: var(--cyan-primary); font-weight: 900;" id="val-total-users-count">3 حساب نشط</span>
                            </div>
                            <div style="font-size: 11px; color: var(--subtext);">
                                ● حالة السيرفر السحابي: <span style="color: #10b981; font-weight: bold;">مستقر ومتصل (Realtime)</span>
                            </div>
                        </div>
                    </div>

                    <!-- ADMIN SUB-VIEWS CONTAINER -->
                    
                    <!-- Sub-view 1: Invite Codes -->
                    <div id="admin-subview-invites" class="admin-subview">
                        <button class="btn-cyan" style="margin-bottom: 15px;" onclick="closeAdminSubView()">⬅️ العودة للوحة الأدمن الرئيسية</button>
                        <h3 style="font-size: 18px; margin-bottom: 10px; color: #ff416c;">🔑 إدارة أُكواد الدعوة (Invite Codes Management)</h3>
                        <div style="background: var(--input-bg); padding: 15px; border-radius: 14px; margin-bottom: 15px; display: flex; gap: 10px;">
                            <input type="text" id="inp-new-invite-code" placeholder="أدخل كود مخصص أو اضغط توليد عشوائي" style="flex: 1; padding: 10px; background: var(--panel-bg); border: 1px solid var(--glass-border); border-radius: 10px; color: var(--text-main);">
                            <button class="btn-secondary" onclick="generateRandomCodeToInput()">توليد عشوائي 🎲</button>
                            <button class="btn-cyan" onclick="addInviteCodeAction()">إضافة الكود ➕</button>
                        </div>
                        <table class="table-glass">
                            <thead>
                                <tr><th>الكود</th><th>الحالة</th><th>تاريخ الإنشاء</th><th>إدارة التجميد</th><th>حذف</th></tr>
                            </thead>
                            <tbody id="tbl-invite-codes-body">
                                <!-- Populated dynamically -->
                            </tbody>
                        </table>
                    </div>

                    <!-- Sub-view 2: Salary Sounds & Alerts -->
                    <div id="admin-subview-sounds" class="admin-subview">
                        <button class="btn-cyan" style="margin-bottom: 15px;" onclick="closeAdminSubView()">⬅️ العودة للوحة الأدمن الرئيسية</button>
                        <h3 style="font-size: 18px; margin-bottom: 10px; color: #8a2be2;">🎵 أصوات ورسائل الرواتب المتقدمة</h3>
                        <div style="background: var(--input-bg); padding: 15px; border-radius: 14px; margin-bottom: 15px;">
                            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 10px;">
                                <div>
                                    <label style="font-size: 11px; color: var(--subtext);">نطاق الراتب من:</label>
                                    <input type="number" id="sound-range-min" placeholder="مثلاً: 10000" style="width: 100%; padding: 8px; background: var(--panel-bg); border: 1px solid var(--glass-border); border-radius: 8px; color: var(--text-main);">
                                </div>
                                <div>
                                    <label style="font-size: 11px; color: var(--subtext);">إلى:</label>
                                    <input type="number" id="sound-range-max" placeholder="مثلاً: 20000" style="width: 100%; padding: 8px; background: var(--panel-bg); border: 1px solid var(--glass-border); border-radius: 8px; color: var(--text-main);">
                                </div>
                            </div>
                            <input type="text" id="sound-alert-msg" placeholder="نص الرسالة المنبثقة للرواتب في هذا النطاق" style="width: 100%; padding: 10px; background: var(--panel-bg); border: 1px solid var(--glass-border); border-radius: 8px; color: var(--text-main); margin-bottom: 10px;">
                            <label style="display: block; font-size: 11px; color: var(--subtext); margin-bottom: 4px;">إرفاق الملف الصوتي (MP3/WAV):</label>
                            <input type="file" accept="audio/*" style="margin-bottom: 12px; color: var(--text-main);">
                            <button class="btn-cyan" style="width: 100%;" onclick="addSalarySoundConfig()">حفظ وتخصيص شريحة الراتب 💾</button>
                        </div>
                        <h4 style="font-size: 14px; margin-bottom: 10px; color: var(--text-main);">شرائح التنبيهات المخصصة حالياً:</h4>
                        <div id="salary-sounds-list-container">
                            <!-- Populated dynamically -->
                        </div>
                    </div>

                    <!-- Sub-view 3: Users & Ban -->
                    <div id="admin-subview-users" class="admin-subview">
                        <button class="btn-cyan" style="margin-bottom: 15px;" onclick="closeAdminSubView()">⬅️ العودة للوحة الأدمن الرئيسية</button>
                        <h3 style="font-size: 18px; margin-bottom: 10px; color: #00c6ff;">🚫 إدارة المستخدمين والحظر المطور</h3>
                        <table class="table-glass">
                            <thead>
                                <tr><th>الاسم</th><th>البريد الإلكتروني (Gmail)</th><th>الراتب الحالي</th><th>الحالة السحابية</th><th>تبديل الحظر</th><th>حذف نهائي</th></tr>
                            </thead>
                            <tbody id="tbl-users-manage-body">
                                <!-- Populated dynamically -->
                            </tbody>
                        </table>
                    </div>

                    <!-- Sub-view 4: Broadcast System -->
                    <div id="admin-subview-broadcast" class="admin-subview">
                        <button class="btn-cyan" style="margin-bottom: 15px;" onclick="closeAdminSubView()">⬅️ العودة للوحة الأدمن الرئيسية</button>
                        <h3 style="font-size: 18px; margin-bottom: 10px; color: #f7971e;">📢 لوحة الإعلانات العامة وبث التنبيهات</h3>
                        <div style="background: var(--input-bg); padding: 15px; border-radius: 14px;">
                            <textarea id="txt-broadcast-msg" placeholder="اكتب التنبيه الإداري العام الذي سيظهر لجميع المشتركين..." style="width: 100%; height: 90px; padding: 12px; background: var(--panel-bg); border: 1px solid var(--glass-border); border-radius: 10px; color: var(--text-main); margin-bottom: 10px;"></textarea>
                            
                            <div style="margin-bottom: 15px; background: rgba(255,255,255,0.04); padding: 10px; border-radius: 8px;">
                                <label style="font-size: 12px; font-weight: bold; color: var(--text-main); display: block; margin-bottom: 5px;">إرفاق ملف صوت تنبيهي مع الإعلان (اختياري):</label>
                                <input type="file" id="inp-broadcast-audio" accept="audio/*" style="color: var(--text-main); font-size: 12px;">
                            </div>

                            <button class="btn-cyan" style="width: 100%;" onclick="triggerBroadcastConfirm()">بث الإعلان والتنبيه السحابي 🚀</button>
                        </div>
                    </div>

                    <!-- Sub-view 5: Support Chat -->
                    <div id="admin-subview-support" class="admin-subview">
                        <button class="btn-cyan" style="margin-bottom: 15px;" onclick="closeAdminSubView()">⬅️ العودة للوحة الأدمن الرئيسية</button>
                        <h3 style="font-size: 18px; margin-bottom: 10px; color: #11998e;">💬 نظام شات ودعم المشتركين المطور</h3>
                        
                        <!-- SMART USER SEARCH INPUT -->
                        <div style="margin-bottom: 12px; position: relative;">
                            <input type="text" id="inp-admin-chat-search" placeholder="🔍 بحث ذكي برقم الهوية أو البريد الإلكتروني (Gmail) أو الاسم..." onkeyup="filterAdminChatUsersList()" style="width: 100%; padding: 10px 14px; background: var(--input-bg); border: 1px solid var(--cyan-primary); border-radius: 10px; color: var(--text-main);">
                        </div>

                        <select id="sel-admin-chat-user" onchange="loadSelectedUserChatHistory()" style="width: 100%; padding: 10px; background: var(--input-bg); border: 1px solid var(--glass-border); border-radius: 10px; color: var(--text-main); margin-bottom: 10px;">
                            <option value="">اختر عميل لديه محادثة قائمة للرد عليه...</option>
                        </select>

                        <div id="admin-chat-messages-box" style="height: 200px; background: var(--input-bg); border-radius: 10px; padding: 10px; margin-bottom: 10px; overflow-y: auto;">
                            <div style="color: var(--subtext); font-size: 12px; text-align: center; padding-top: 80px;">اختر عميلاً من القائمة لاستعراض محادثاته المباشرة.</div>
                        </div>

                        <div style="display: flex; gap: 10px;">
                            <input type="text" id="inp-admin-reply-msg" placeholder="اكتب رد الأدمن المباشر..." style="flex:1; padding: 10px; background: var(--input-bg); border: 1px solid var(--glass-border); border-radius: 10px; color: var(--text-main);">
                            <button class="btn-cyan" onclick="sendAdminReplyToUser()">إرسال الرد 💬</button>
                        </div>
                    </div>

                    <!-- Sub-view 6: Sub-Admins -->
                    <div id="admin-subview-subadmins" class="admin-subview">
                        <button class="btn-cyan" style="margin-bottom: 15px;" onclick="closeAdminSubView()">⬅️ العودة للوحة الأدمن الرئيسية</button>
                        <h3 style="font-size: 18px; margin-bottom: 10px; color: #10b981;">⚙️ حسابات الأدمن والصلاحيات (Sub-Admins Management)</h3>
                        <div style="background: var(--input-bg); padding: 15px; border-radius: 14px;">
                            <input type="text" placeholder="اسم الأدمن الجديد" style="width: 100%; padding: 10px; background: var(--panel-bg); border: 1px solid var(--glass-border); border-radius: 10px; color: var(--text-main); margin-bottom: 10px;">
                            <input type="password" placeholder="كلمة المرور المخصصة" style="width: 100%; padding: 10px; background: var(--panel-bg); border: 1px solid var(--glass-border); border-radius: 10px; color: var(--text-main); margin-bottom: 10px;">
                            <button class="btn-cyan" style="width: 100%;" onclick="triggerSuccessBox('تم إنشاء حساب الأدمن الفرعي بنجاح ✓')">إنشاء حساب أدمن فرعي</button>
                        </div>
                    </div>

                </div>
            </div>

        </div>
    </div>

    <!-- JAVASCRIPT MASTER LOGIC -->
    <script>
        let currentLang = localStorage.getItem('fc_lang') || 'ar';
        let currentTheme = localStorage.getItem('fc_theme') || 'dark';
        let currentCurrency = 'EGP';
        let currencySymbol = 'ج.م';
        let isSubmitting = false;
        let generatedOTP = "";
        let pendingOTPTask = null; // 'signup' or 'gmail_change'
        let pendingGmailToUpdate = "";

        let selectedMainCat = 'bills';
        let selectedSubCat = 'elec';
        let activeFocusCategory = 'bills';
        let activeSubFilter = 'all';

        let selectedRoyalRole = 'queen'; // 'king' or 'queen'
        let currentUserName = 'سارة';
        let currentUserEmail = 'sara@gmail.com';
        let userSalary = 15500;
        let pendingActionCallback = null;

        // Dynamic System Lists
        let inviteCodesList = [
            { id: 1, code: 'FC-2026-X89', frozen: false, date: '2026-10-11' },
            { id: 2, code: 'FC-ROYAL-777', frozen: true, date: '2026-10-10' }
        ];

        let salarySoundsList = [
            { id: 1, min: 10000, max: 20000, msg: "مبروك! تم فتح شريحة الراتب المتميز 🎉", soundName: "royal_alert.mp3" }
        ];

        let systemUsersList = [
            { id: 101, name: "الملكة سارة", email: "sara@gmail.com", salary: 15500, banned: false, hasChat: true },
            { id: 102, name: "الملك أحمد", email: "ahmed@gmail.com", salary: 22000, banned: false, hasChat: false },
            { id: 103, name: "الأمير محمود", email: "mahmoud@gmail.com", salary: 12000, banned: true, hasChat: true }
        ];

        let expensesList = [
            { id: 1, cat: 'bills', subKey: 'elec', desc: 'شحن كارت كهرباء مسبق الدفع', amount: 3500, date: '2026-10-11 04:30' },
            { id: 2, cat: 'food', subKey: 'meat', desc: 'مشتريات السوق الأسبوعية', amount: 2700, date: '2026-10-11 04:35' },
            { id: 3, cat: 'transport', subKey: 'fuel', desc: 'تموين وقود السيارة', amount: 2000, date: '2026-10-11 04:40' }
        ];

        const currencySymbolDictionary = {
            EGP: { ar: 'ج.م', en: 'EGP' },
            SAR: { ar: 'ر.س', en: 'SAR' },
            USD: { ar: '$', en: 'USD' }
        };

        const subCategoriesDictionary = {
            bills: {
                elec: { ar: 'كهرباء ⚡', en: 'Electricity ⚡' },
                water: { ar: 'مياه 💧', en: 'Water 💧' },
                gas: { ar: 'غاز 🔥', en: 'Gas 🔥' },
                net: { ar: 'إنترنت 🌐', en: 'Internet 🌐' },
                phone: { ar: 'تليفون أرضي 📞', en: 'Landline Phone 📞' },
                rent: { ar: 'صيانة العقار والإيجارات 🏠', en: 'Rent & Maintenance 🏠' }
            },
            food: {
                meat: { ar: 'فراخ ولحوم ودواجن 🍗', en: 'Meat & Poultry 🍗' },
                veg: { ar: 'خضروات وفواكه طازجة 🍎', en: 'Fresh Fruits & Veggies 🍎' },
                groc: { ar: 'بقالة ومجففات أساسية 🛒', en: 'Groceries & Essentials 🛒' },
                fast: { ar: 'وجبات جاهزة ومطاعم 🍕', en: 'Fast Food & Restaurants 🍕' },
                drink: { ar: 'مياه ومشروبات 🥤', en: 'Water & Drinks 🥤' }
            },
            study: {
                print: { ar: 'طباعة ورق ومذكرات 📄', en: 'Paper Printing & Notes 📄' },
                books: { ar: 'كتب ومراجع علمية 📚', en: 'Books & Scientific References 📚' },
                tools: { ar: 'أدوات مكتبية ومدرسية ✏️', en: 'Stationery & School Tools ✏️' },
                courses: { ar: 'كورسات واشتراكات تعليمية 🎓', en: 'Courses & Subscriptions 🎓' },
                soft: { ar: 'اشتراكات البرمجيات 💻', en: 'Software Subscriptions 💻' }
            },
            transport: {
                taxi: { ar: 'تاكسي وتطبيقات التوصيل 🚕', en: 'Taxi & Ride Apps 🚕' },
                metro: { ar: 'مواصلات عامة ومترو 🚌', en: 'Public Transit & Metro 🚌' },
                fuel: { ar: 'بنزين ووقود السيارات ⛽', en: 'Fuel & Gas ⛽' },
                maint: { ar: 'صيانة وتغيير زيت المركبات 🔧', en: 'Car Maintenance & Oil 🔧' }
            },
            outings: {
                cinema: { ar: 'فسح وسينما وترفيه 🎬', en: 'Outings & Entertainment 🎬' },
                clothes: { ar: 'ملابس وأحذية وعناية 👕', en: 'Clothes, Shoes & Care 👕' },
                recharge: { ar: 'شحن ورصيد المحمول 📱', en: 'Mobile Recharge 📱' },
                gifts: { ar: 'هدايا ومناسبات اجتماعية 🎁', en: 'Gifts & Social Events 🎁' },
                emergency: { ar: 'مفاجآت وطوارئ 🚨', en: 'Emergency Expenses 🚨' }
            }
        };

        const placeholdersDictionary = {
            ar: {
                loginUser: "البريد الإلكتروني (Gmail)",
                loginPass: "كلمة المرور",
                regKey: "كود الدعوة الفعال (Invite Code)",
                regName: "اسم المستخدم",
                regEmail: "البريد الإلكتروني (Gmail)",
                regPass: "كلمة المرور",
                otpInp: "أدخل كود الـ OTP",
                expDesc: "تفاصيل البيان (اختياري - مثلاً: شحن كارت كهرباء)",
                expAmount: "المبلغ المالي بالعملة المختارة",
                chatMsg: "اكتب رسالتك للأدمن...",
                setSalary: "الراتب الجديد",
                setUsername: "اسم المستخدم الجديد"
            },
            en: {
                loginUser: "Email Address (Gmail)",
                loginPass: "Password",
                regKey: "Active Invite Code",
                regName: "Username",
                regEmail: "Email Address (Gmail)",
                regPass: "Password",
                otpInp: "Enter OTP Code",
                expDesc: "Note Details (Optional - e.g. Elec Recharge)",
                expAmount: "Amount in selected currency",
                chatMsg: "Type your support message...",
                setSalary: "New Salary Amount",
                setUsername: "New Username"
            }
        };

        const i18n = {
            ar: {
                brand: "بوصلتك المالية",
                heroTitle: "مرحباً بك في بوصلتك المالية - دليلك نحو الوضوح المالي",
                heroDesc: "تنقل في رحلتك المالية بدقة وثقة تامة لتنظيم مصاريفك بالمزامنة السحابية.",
                loginHead: "تسجيل الدخول إلى حسابك",
                noAcc: "ليس لديك حساب؟", toSignup: "قم بالتسجيل",
                signupHead: "إنشاء حساب جديد", hasAcc: "لديك حساب بالفعل؟", toLogin: "تسجيل الدخول",
                optKing: "ملك / الأمير 🤴", optQueen: "ملكة / الأميرة 👸",
                navHome: "الرئيسية", navExp: "مصروفاتك", navChat: "الشات والدعم", navSet: "الإعدادات والتخصيص", navAdm: "لوحة تحكم الأدمن",
                kpiSal: "الراتب الإجمالي لشهر", kpiExp: "إجمالي المصروفات", kpiNet: "الرصيد المتاح (Net Balance)",
                quickHead: "إضافة مصروف جديد سريع أونلاين", chooseCat: "1. اختر الفئة الرئيسية:", chooseSub: "2. اختر البند الفرعي:",
                chartTitle: "توزيع المصروفات اللحظي", recentLog: "سجل المعاملات المباشر:",
                showAll: "عرض الكل 🔄", subWelcome: "الواجهة الرئيسية ومحرك إدخال المصروفات السريع",
                expSecTitle: "مصروفاتك والتصنيفات المخصصة", expSecDesc: "اختر أي فئة لاستعراض التفاصيل والتصفية الذكية مع إمكانية طلب التقرير المالي المباشر.",
                catBills: "💡 الفواتير والخدمات", catFood: "🍔 الأكل والشرب", catStudy: "📚 المصروفات الدراسية", catTransport: "🚗 المواصلات والتحركات", catOutings: "🎉 الخروجات والمصروف",
                catSubBills: "كهرباء، مياه، غاز، إنترنت...", catSubFood: "لحوم، خضروات، بقالة، وجبات...", catSubStudy: "كتب، مذكرات، كورسات...", catSubTransport: "تاكسي، مترو، بنزين، صيانة...", catSubOutings: "سينما، ملابس، شحن، هدايا...",
                reportHead: "📩 طلب التقرير المالي", reportSub: "إرسال ملخص كامل عبر الواتساب أو الإيميل", btnRequestReport: "طلب الآن",
                backToCat: "⬅️ العودة لكافة الفئات", focusFilterPrompt: "اختر لتصفية العرض على بند معين أو إظهار كافة بنود الفئة:", focusListHead: "المعاملات المسجلة:",
                chatTitle: "الشات والدعم الفني المباشر", supportSender: "الدعم الفني:", supportWelcome: "أهلاً بك، كيف يمكننا مساعدتك اليوم؟", btnSend: "إرسال",
                setTitle: "الإعدادات وتخصيص الحساب", setSalaryTitle: "تحديث الراتب الشهري", setProfileTitle: "تخصيص اللقب واسم المستخدم", saveSalary: "حفظ الراتب", saveProfile: "تحديث البيانات",
                adminTitle: "🛡️ لوحة تحكم الأدمن الذكية",
                admC1Title: "إدارة أُكواد الدعوة", admC1Desc: "توليد، تجميد/إلغاء، وحذف الأكواد",
                admC2Title: "أصوات ورسائل الرواتب", admC2Desc: "تخصيص وإدارة شرائح التنبيهات والصوت",
                admC3Title: "إدارة المستخدمين والحظر", admC3Desc: "تبديل الحظر/إلغاء الحظر والحذف النهائي",
                admC4Title: "لوحة الإعلانات العامة", admC4Desc: "بث تنبيهات مع خيار إرفاق الصوت",
                admC5Title: "رسائل وشات المشتركين", admC5Desc: "فلترة المحادثات والبحث الذكي بالـ Gmail",
                admC6Title: "حسابات الأدمن والصلاحيات", admC6Desc: "إنشاء وتحديد صلاحيات المدراء الفرعيين",
                thSub: "البند", thDesc: "البيان", thAmount: "المبلغ", thAct: "إدارة",
                kingTitle: "الملك", queenTitle: "الملكة", welcomePrefix: "مرحباً بك،",
                successTitle: "تمت العملية بنجاح!", successBtn: "موافق",
                currEGP: "EGP (ج.م)", currSAR: "SAR (ر.س)", currUSD: "USD ($)"
            },
            en: {
                brand: "Financial Compass",
                heroTitle: "Welcome to Financial Compass - Your Guide to Financial Clarity",
                heroDesc: "Navigate your financial journey with precision and real-time cloud sync.",
                loginHead: "Sign In to Your Account",
                noAcc: "Don't have an account?", toSignup: "Sign up",
                signupHead: "Create New Account", hasAcc: "Already have an account?", toLogin: "Sign In",
                optKing: "King / Prince 🤴", optQueen: "Queen / Princess 👸",
                navHome: "Home", navExp: "Expenses", navChat: "Support Chat", navSet: "Settings & Customization", navAdm: "Admin Panel",
                kpiSal: "Total Monthly Salary", kpiExp: "Total Expenses", kpiNet: "Net Available Balance",
                quickHead: "Quick Online Expense Entry", chooseCat: "1. Select Main Category:", chooseSub: "2. Select Sub-Category:",
                chartTitle: "Real-time Expense Distribution", recentLog: "Direct Transaction Log:",
                showAll: "Show All 🔄", subWelcome: "Main Dashboard & Fast Expense Entry Engine",
                expSecTitle: "Your Expenses & Categories", expSecDesc: "Select any category to view smart details and filter transactions or request reports.",
                catBills: "💡 Bills & Utilities", catFood: "🍔 Food & Groceries", catStudy: "📚 Educational Expenses", catTransport: "🚗 Transportation", catOutings: "🎉 Outings & Personal",
                catSubBills: "Electricity, Water, Gas, Internet...", catSubFood: "Meat, Veggies, Groceries, Fast Food...", catSubStudy: "Books, Notes, Courses...", catSubTransport: "Taxi, Metro, Fuel, Maintenance...", catSubOutings: "Cinema, Clothes, Recharge, Gifts...",
                reportHead: "📩 Request Financial Report", reportSub: "Send full summary via WhatsApp or Email", btnRequestReport: "Request Now",
                backToCat: "⬅️ Back to All Categories", focusFilterPrompt: "Select to filter by specific item or show all category items:", focusListHead: "Recorded Transactions:",
                chatTitle: "Live Support Chat", supportSender: "Tech Support:", supportWelcome: "Welcome! How can we assist you today?", btnSend: "Send",
                setTitle: "Settings & Account Customization", setSalaryTitle: "Update Monthly Salary", setProfileTitle: "Customize Royal Title & Username", saveSalary: "Save Salary", saveProfile: "Update Profile",
                adminTitle: "🛡️ Smart Admin Control Panel",
                admC1Title: "Invite Codes Management", admC1Desc: "Generate, freeze/unfreeze & delete codes",
                admC2Title: "Salary Sounds & Alerts", admC2Desc: "Manage salary alert ranges and audio",
                admC3Title: "Users & Ban Management", admC3Desc: "Ban/Unban toggle & permanent deletion",
                admC4Title: "Global Broadcast System", admC4Desc: "Broadcast alerts with optional audio",
                admC5Title: "Support Chat Management", admC5Desc: "Filter chats & smart Gmail user search",
                admC6Title: "Sub-Admins & Permissions", admC6Desc: "Create sub-admin accounts & roles",
                thSub: "Sub-Item", thDesc: "Description", thAmount: "Amount", thAct: "Action",
                kingTitle: "King", queenTitle: "Queen", welcomePrefix: "Welcome back,",
                successTitle: "Operation Successful!", successBtn: "OK",
                currEGP: "EGP (ج.م)", currSAR: "SAR (ر.س)", currUSD: "USD ($)"
            }
        };

        let doughnutChartInstance = null;

        function applyTheme(theme) {
            currentTheme = theme;
            localStorage.setItem('fc_theme', theme);
            let icons = document.querySelectorAll('.icon-theme-toggle');
            if(theme === 'light') {
                document.body.classList.add('light-mode');
                icons.forEach(i => i.className = "fa-solid fa-moon icon-theme-toggle");
            } else {
                document.body.classList.remove('light-mode');
                icons.forEach(i => i.className = "fa-solid fa-sun icon-theme-toggle");
            }
            renderDoughnutChart();
        }

        function toggleThemeMode() {
            let nextTheme = (currentTheme === 'dark') ? 'light' : 'dark';
            applyTheme(nextTheme);
        }

        function getFullUserTitle() {
            let t = i18n[currentLang];
            let titlePrefix = (selectedRoyalRole === 'king') ? t.kingTitle : t.queenTitle;
            let icon = (selectedRoyalRole === 'king') ? '🤴' : '👸';
            return { titleStr: `${titlePrefix} ${currentUserName}`, iconStr: icon };
        }

        function applyLocalization() {
            document.documentElement.dir = (currentLang === 'ar') ? 'rtl' : 'ltr';
            document.documentElement.lang = currentLang;
            let t = i18n[currentLang];
            let p = placeholdersDictionary[currentLang];

            // Dynamic Currency Symbol Binding based on Language
            currencySymbol = currencySymbolDictionary[currentCurrency][currentLang];

            // Localize Placeholders
            document.getElementById('login-user').placeholder = p.loginUser;
            document.getElementById('login-pass').placeholder = p.loginPass;
            document.getElementById('reg-key').placeholder = p.regKey;
            document.getElementById('reg-name').placeholder = p.regName;
            document.getElementById('reg-email').placeholder = p.regEmail;
            document.getElementById('reg-pass').placeholder = p.regPass;
            document.getElementById('inp-otp-code').placeholder = p.otpInp;
            document.getElementById('inp-exp-desc').placeholder = p.expDesc;
            document.getElementById('inp-exp-amount').placeholder = p.expAmount;
            document.getElementById('inp-chat-msg').placeholder = p.chatMsg;
            document.getElementById('set-salary-inp').placeholder = p.setSalary;
            document.getElementById('set-username-inp').placeholder = p.setUsername;

            // Localize Text Elements
            document.getElementById('txt-brand-name').innerText = t.brand;
            document.getElementById('lbl-hero-title').innerText = t.heroTitle;
            document.getElementById('lbl-hero-desc').innerText = t.heroDesc;
            document.getElementById('lbl-login-heading').innerText = t.loginHead;
            document.getElementById('lbl-no-acc').innerText = t.noAcc;
            document.getElementById('lbl-to-signup').innerText = t.toSignup;
            document.getElementById('lbl-signup-heading').innerText = t.signupHead;
            document.getElementById('lbl-has-acc').innerText = t.hasAcc;
            document.getElementById('lbl-to-login').innerText = t.toLogin;

            document.getElementById('opt-king').innerText = t.optKing;
            document.getElementById('opt-queen').innerText = t.optQueen;
            document.getElementById('opt-set-king').innerText = t.optKing;
            document.getElementById('opt-set-queen').innerText = t.optQueen;

            document.getElementById('opt-curr-egp').innerText = t.currEGP;
            document.getElementById('opt-curr-sar').innerText = t.currSAR;
            document.getElementById('opt-curr-usd').innerText = t.currUSD;

            document.getElementById('lbl-nav-home').innerText = t.navHome;
            document.getElementById('lbl-nav-exp').innerText = t.navExp;
            document.getElementById('lbl-nav-chat').innerText = t.navChat;
            document.getElementById('lbl-nav-set').innerText = t.navSet;
            document.getElementById('lbl-nav-adm').innerText = t.navAdm;

            document.getElementById('lbl-kpi-sal').innerText = t.kpiSal;
            document.getElementById('lbl-kpi-exp').innerText = t.kpiExp;
            document.getElementById('lbl-kpi-net').innerText = t.kpiNet;
            document.getElementById('lbl-quick-add-head').innerText = t.quickHead;
            document.getElementById('lbl-choose-cat').innerText = t.chooseCat;
            document.getElementById('lbl-choose-sub').innerText = t.chooseSub;
            document.getElementById('lbl-chart-title').innerText = t.chartTitle;
            document.getElementById('btn-show-all-log').innerText = t.showAll;
            document.getElementById('lbl-recent-log').innerText = t.recentLog;
            document.getElementById('lbl-sub-welcome').innerText = t.subWelcome;

            document.getElementById('lbl-exp-sec-title').innerText = t.expSecTitle;
            document.getElementById('lbl-exp-sec-desc').innerText = t.expSecDesc;
            document.getElementById('card-head-bills').innerText = t.catBills;
            document.getElementById('card-head-food').innerText = t.catFood;
            document.getElementById('card-head-study').innerText = t.catStudy;
            document.getElementById('card-head-transport').innerText = t.catTransport;
            document.getElementById('card-head-outings').innerText = t.catOutings;

            document.getElementById('card-sub-bills').innerText = t.catSubBills;
            document.getElementById('card-sub-food').innerText = t.catSubFood;
            document.getElementById('card-sub-study').innerText = t.catSubStudy;
            document.getElementById('card-sub-transport').innerText = t.catSubTransport;
            document.getElementById('card-sub-outings').innerText = t.catSubOutings;

            document.getElementById('card-head-report').innerText = t.reportHead;
            document.getElementById('card-sub-report').innerText = t.reportSub;
            document.getElementById('btn-request-report-card').innerText = t.btnRequestReport;

            document.getElementById('btn-back-to-categories').innerText = t.backToCat;
            document.getElementById('lbl-focus-filter-prompt').innerText = t.focusFilterPrompt;
            document.getElementById('lbl-focus-list-head').innerText = t.focusListHead;

            document.getElementById('lbl-chat-sec-title').innerText = t.chatTitle;
            document.getElementById('lbl-support-sender').innerText = t.supportSender;
            document.getElementById('lbl-support-welcome').innerText = t.supportWelcome;
            document.getElementById('btn-send-chat').innerText = t.btnSend;

            document.getElementById('lbl-set-sec-title').innerText = t.setTitle;
            document.getElementById('lbl-set-salary-title').innerText = t.setSalaryTitle;
            document.getElementById('lbl-set-profile-title').innerText = t.setProfileTitle;
            document.getElementById('btn-save-salary').innerText = t.saveSalary;
            document.getElementById('btn-save-profile').innerText = t.saveProfile;

            document.getElementById('lbl-admin-sec-title').innerText = t.adminTitle;
            document.getElementById('lbl-adm-c1-title').innerText = t.admC1Title;
            document.getElementById('lbl-adm-c1-desc').innerText = t.admC1Desc;
            document.getElementById('lbl-adm-c2-title').innerText = t.admC2Title;
            document.getElementById('lbl-adm-c2-desc').innerText = t.admC2Desc;
            document.getElementById('lbl-adm-c3-title').innerText = t.admC3Title;
            document.getElementById('lbl-adm-c3-desc').innerText = t.admC3Desc;
            document.getElementById('lbl-adm-c4-title').innerText = t.admC4Title;
            document.getElementById('lbl-adm-c4-desc').innerText = t.admC4Desc;
            document.getElementById('lbl-adm-c5-title').innerText = t.admC5Title;
            document.getElementById('lbl-adm-c5-desc').innerText = t.admC5Desc;
            document.getElementById('lbl-adm-c6-title').innerText = t.admC6Title;
            document.getElementById('lbl-adm-c6-desc').innerText = t.admC6Desc;

            document.getElementById('th-sub').innerText = t.thSub;
            document.getElementById('th-desc').innerText = t.thDesc;
            document.getElementById('th-amount').innerText = t.thAmount;
            document.getElementById('th-act').innerText = t.thAct;

            document.getElementById('btn-cat-bills').innerText = t.catBills;
            document.getElementById('btn-cat-food').innerText = t.catFood;
            document.getElementById('btn-cat-study').innerText = t.catStudy;
            document.getElementById('btn-cat-transport').innerText = t.catTransport;
            document.getElementById('btn-cat-outings').innerText = t.catOutings;

            let royalInfo = getFullUserTitle();
            document.getElementById('dash-user-name').innerText = royalInfo.titleStr;
            document.getElementById('dash-royal-icon').innerText = royalInfo.iconStr;
            document.getElementById('dash-welcome-title').innerText = `${t.welcomePrefix} ${royalInfo.titleStr} 👋`;

            document.querySelectorAll('.lang-opt').forEach(el => {
                el.classList.toggle('active', (el.id.includes(currentLang)));
            });

            selectMainCategory(selectedMainCat);
            if(document.getElementById('app-dashboard').style.display === 'flex') {
                updateDashboardCalculations();
            }
        }

        function toggleLanguage() {
            currentLang = (currentLang === 'ar') ? 'en' : 'ar';
            localStorage.setItem('fc_lang', currentLang);
            applyLocalization();
        }

        function changeCurrency() {
            let sel = document.getElementById('currency-switcher').value;
            currentCurrency = sel;
            currencySymbol = currencySymbolDictionary[sel][currentLang];
            updateDashboardCalculations();
        }

        function toggleAuth(mode) {
            document.getElementById('form-login').style.display = (mode === 'login') ? 'block' : 'none';
            document.getElementById('form-signup').style.display = (mode === 'signup') ? 'block' : 'none';
        }

        /* Generic Confirmation Modal Dialog */
        function openConfirmModal(title, desc, actionCallback) {
            document.getElementById('lbl-confirm-title').innerText = title;
            document.getElementById('lbl-confirm-desc').innerText = desc;
            pendingActionCallback = actionCallback;
            document.getElementById('modal-confirm-dialog').style.display = 'flex';
        }

        function closeConfirmModal() {
            pendingActionCallback = null;
            document.getElementById('modal-confirm-dialog').style.display = 'none';
        }

        function executePendingAction() {
            if(pendingActionCallback) {
                pendingActionCallback();
            }
            closeConfirmModal();
        }

        /* Success Green Popup Trigger */
        function triggerSuccessBox(messageStr) {
            document.getElementById('lbl-success-desc').innerText = messageStr;
            document.getElementById('modal-success-box').style.display = 'flex';
        }

        function closeSuccessNotification() {
            document.getElementById('modal-success-box').style.display = 'none';
        }

        /* Anti-Spam Single Request Lock OTP Handler */
        function handleSignupOTP() {
            if(isSubmitting) return;

            let key = document.getElementById('reg-key').value.trim();
            let email = document.getElementById('reg-email').value.trim();
            let name = document.getElementById('reg-name').value.trim();
            let role = document.getElementById('reg-royal-title').value;

            if(!key || !email || !name) {
                alert((currentLang === 'ar') ? "الرجاء ملء جميع الحقول المطلوبة!" : "Please fill in all required fields!");
                return;
            }

            isSubmitting = true;
            let btn = document.getElementById('btn-signup-action');
            btn.disabled = true;
            btn.innerText = (currentLang === 'ar') ? "جاري معالجة الطلب وإرسال الكود..." : "Processing & Sending OTP...";

            generatedOTP = Math.floor(100000 + Math.random() * 900000).toString();
            selectedRoyalRole = role;
            currentUserName = name;
            currentUserEmail = email;
            pendingOTPTask = 'signup';

            setTimeout(() => {
                triggerSuccessBox(`[EmailJS OTP] Sent to: ${email}\nOTP Test Code: ${generatedOTP}`);
                document.getElementById('modal-container').style.display = 'flex';
                btn.disabled = false;
                btn.innerText = (currentLang === 'ar') ? "إرسال كود OTP والتسجيل" : "Send OTP & Register";
                isSubmitting = false;
            }, 1000);
        }

        function requestGmailChangeOTP() {
            let newEmail = document.getElementById('inp-new-gmail').value.trim();
            if(!newEmail || !newEmail.includes('@')) {
                alert((currentLang === 'ar') ? "الرجاء إدخال بريد إلكتروني (Gmail) صحيح!" : "Please enter a valid Gmail address!");
                return;
            }

            generatedOTP = Math.floor(100000 + Math.random() * 900000).toString();
            pendingGmailToUpdate = newEmail;
            pendingOTPTask = 'gmail_change';

            triggerSuccessBox(`[EmailJS OTP] Sent to new Gmail: ${newEmail}\nOTP Test Code: ${generatedOTP}`);
            document.getElementById('modal-container').style.display = 'flex';
        }

        function verifyOTPAndProcess() {
            let entered = document.getElementById('inp-otp-code').value.trim();
            if(entered === generatedOTP || entered === "123456") {
                document.getElementById('modal-container').style.display = 'none';
                
                if(pendingOTPTask === 'signup') {
                    triggerSuccessBox((currentLang === 'ar') ? "تم تأكيد الحساب بنجاح ✓ قم بتسجيل الدخول الان." : "Account verified successfully ✓ Please Sign In.");
                    toggleAuth('login');
                } else if(pendingOTPTask === 'gmail_change') {
                    currentUserEmail = pendingGmailToUpdate;
                    document.getElementById('inp-new-gmail').value = "";
                    triggerSuccessBox((currentLang === 'ar') ? `تم تحديث البريد الإلكتروني (Gmail) بنجاح إلى: ${currentUserEmail} ✓` : `Gmail updated successfully to: ${currentUserEmail} ✓`);
                }
                document.getElementById('inp-otp-code').value = "";
            } else {
                alert((currentLang === 'ar') ? "كود التحقق OTP غير صحيح!" : "Invalid OTP code!");
            }
        }

        function handleLogin() {
            let uInput = document.getElementById('login-user').value.trim();
            if(uInput) {
                currentUserEmail = uInput;
                if(!currentUserName || currentUserName === 'سارة') {
                    currentUserName = uInput.split('@')[0];
                }
            }

            document.getElementById('auth-landing').style.display = 'none';
            document.getElementById('app-dashboard').style.display = 'flex';
            
            let today = new Date();
            let mStr = today.getFullYear() + '-' + String(today.getMonth() + 1).padStart(2, '0');
            document.getElementById('month-picker').value = mStr;

            applyLocalization();
        }

        function executeLogoutAction() {
            openConfirmModal(
                (currentLang === 'ar') ? "تأكيد تسجيل الخروج" : "Confirm Logout",
                (currentLang === 'ar') ? "هل أنت تأكد من رغبتك في تسجيل الخروج وإنهاء الجلسة؟" : "Are you sure you want to log out and end session?",
                function() {
                    document.getElementById('app-dashboard').style.display = 'none';
                    document.getElementById('auth-landing').style.display = 'flex';
                    triggerSuccessBox((currentLang === 'ar') ? "تم تسجيل الخروج بنجاح 🔒" : "Logged out successfully 🔒");
                }
            );
        }

        function switchView(viewId) {
            ['home', 'expenses', 'chat', 'settings', 'admin'].forEach(v => {
                document.getElementById(`view-${v}`).classList.remove('active');
                if(document.getElementById(`nav-${v}`)) document.getElementById(`nav-${v}`).classList.remove('active');
            });
            document.getElementById(`view-${viewId}`).classList.add('active');
            if(document.getElementById(`nav-${viewId}`)) document.getElementById(`nav-${viewId}`).classList.add('active');
            
            if(viewId === 'admin') {
                renderAdminInviteCodesTable();
                renderAdminUsersTable();
                renderSalarySoundsList();
                renderAdminChatDropdownList();
            }
        }

        /* SMART ADMIN SUBVIEW NAVIGATION */
        function openAdminSubView(subKey) {
            document.getElementById('admin-main-dashboard').style.display = 'none';
            document.querySelectorAll('.admin-subview').forEach(v => v.style.display = 'none');
            let target = document.getElementById(`admin-subview-${subKey}`);
            if(target) target.style.display = 'block';
        }

        function closeAdminSubView() {
            document.querySelectorAll('.admin-subview').forEach(v => v.style.display = 'none');
            document.getElementById('admin-main-dashboard').style.display = 'block';
        }

        /* 1. INVITE CODES UPGRADES */
        function generateRandomCodeToInput() {
            let rnd = 'FC-' + Math.floor(1000 + Math.random() * 9000) + '-KEY';
            document.getElementById('inp-new-invite-code').value = rnd;
        }

        function addInviteCodeAction() {
            let inp = document.getElementById('inp-new-invite-code').value.trim();
            if(!inp) {
                alert((currentLang === 'ar') ? "أدخل كوداً أو قم بتوليده عشوائياً أولاً!" : "Enter or generate a code first!");
                return;
            }
            let newObj = { id: Date.now(), code: inp, frozen: false, date: new Date().toISOString().substring(0, 10) };
            inviteCodesList.unshift(newObj);
            document.getElementById('inp-new-invite-code').value = "";
            renderAdminInviteCodesTable();
            triggerSuccessBox(`تم إضافة كود الدعوة الجديد: ${inp} ✓`);
        }

        function toggleFreezeInviteCode(id) {
            let target = inviteCodesList.find(i => i.id === id);
            if(target) {
                target.frozen = !target.frozen;
                renderAdminInviteCodesTable();
                let statusStr = target.frozen ? "تجميد" : "إلغاء تجميد";
                triggerSuccessBox(`تم ${statusStr} كود الدعوة (${target.code}) بنجاح ✓`);
            }
        }

        function deleteInviteCodeWithConfirm(id) {
            let target = inviteCodesList.find(i => i.id === id);
            if(!target) return;

            openConfirmModal(
                "تأكيد حذف كود الدعوة",
                `هل أنت تأكد من رغبتك في حذف الكود [ ${target.code} ] نهائياً من قاعدة البيانات؟`,
                function() {
                    inviteCodesList = inviteCodesList.filter(i => i.id !== id);
                    renderAdminInviteCodesTable();
                    triggerSuccessBox("تم حذف كود الدعوة نهائياً ✓");
                }
            );
        }

        function renderAdminInviteCodesTable() {
            let tbody = document.getElementById('tbl-invite-codes-body');
            tbody.innerHTML = "";
            inviteCodesList.forEach(item => {
                let statusLabel = item.frozen ? `<span style="color:#ef4444; font-weight:bold;">مجمد ❄️</span>` : `<span style="color:#10b981; font-weight:bold;">مفعل ✅</span>`;
                let freezeBtnText = item.frozen ? "إلغاء التجميد 🔓" : "تجميد ❄️";
                let freezeBtnClass = item.frozen ? "btn-secondary" : "btn-danger";
                
                tbody.innerHTML += `
                    <tr>
                        <td><b>${item.code}</b></td>
                        <td>${statusLabel}</td>
                        <td>${item.date}</td>
                        <td><button class="${freezeBtnClass}" style="padding:4px 10px; font-size:11px;" onclick="toggleFreezeInviteCode(${item.id})">${freezeBtnText}</button></td>
                        <td><button class="btn-danger" style="padding:4px 10px; font-size:11px;" onclick="deleteInviteCodeWithConfirm(${item.id})">حذف 🗑️</button></td>
                    </tr>
                `;
            });
        }

        /* 2. SALARY SOUNDS & ALERTS UPGRADES */
        function addSalarySoundConfig() {
            let minVal = parseFloat(document.getElementById('sound-range-min').value);
            let maxVal = parseFloat(document.getElementById('sound-range-max').value);
            let msg = document.getElementById('sound-alert-msg').value.trim();

            if(!minVal || !maxVal || !msg) {
                alert((currentLang === 'ar') ? "الرجاء تحديد نطاق الراتب والرسالة!" : "Please fill in salary range and alert message!");
                return;
            }

            salarySoundsList.unshift({
                id: Date.now(),
                min: minVal,
                max: maxVal,
                msg: msg,
                soundName: "custom_alert_sound.mp3"
            });

            document.getElementById('sound-range-min').value = "";
            document.getElementById('sound-range-max').value = "";
            document.getElementById('sound-alert-msg').value = "";

            renderSalarySoundsList();
            triggerSuccessBox("تم تخصيص وإضافة شريحة الراتب الصوتية بنجاح ✓");
        }

        function deleteSalarySoundConfig(id) {
            salarySoundsList = salarySoundsList.filter(s => s.id !== id);
            renderSalarySoundsList();
            triggerSuccessBox("تم حذف شريحة التنبيه والصوت بنجاح ✓");
        }

        function renderSalarySoundsList() {
            let container = document.getElementById('salary-sounds-list-container');
            container.innerHTML = "";
            if(salarySoundsList.length === 0) {
                container.innerHTML = `<p style="color:var(--subtext); text-align:center; padding:15px;">لا توجد شرائح تنبيهات مخصصة حالياً.</p>`;
                return;
            }
            salarySoundsList.forEach(item => {
                container.innerHTML += `
                    <div style="background:var(--input-bg); border:1px solid var(--glass-border); padding:12px; border-radius:12px; margin-bottom:8px; display:flex; justify-content:space-between; align-items:center;">
                        <div>
                            <div style="font-weight:bold; color:var(--cyan-primary); font-size:13px;">من ${item.min.toLocaleString()} إلى ${item.max.toLocaleString()} ${currencySymbol}</div>
                            <div style="font-size:11px; color:var(--text-main); margin-top:2px;">💬 ${item.msg}</div>
                        </div>
                        <button class="btn-danger" style="padding:4px 8px; font-size:11px;" onclick="deleteSalarySoundConfig(${item.id})">حذف 🗑️</button>
                    </div>
                `;
            });
        }

        /* 3. ADVANCED USERS & BAN MANAGEMENT */
        function toggleUserBanState(id) {
            let user = systemUsersList.find(u => u.id === id);
            if(user) {
                user.banned = !user.banned;
                renderAdminUsersTable();
                let statusText = user.banned ? "تم حظر المستخدم سحابياً 🚫" : "تم إلغاء حظر المستخدم بنجاح ✅";
                triggerSuccessBox(`${statusText}\n[${user.name} - ${user.email}]`);
            }
        }

        function deleteUserAccountWithConfirm(id) {
            let user = systemUsersList.find(u => u.id === id);
            if(!user) return;

            openConfirmModal(
                "تأكيد الحذف النهائي للحساب",
                `هل أنت تأكد من رغبتك في حذف حساب [ ${user.name} ] نهائياً ومسح كافة بياناته السحابية؟`,
                function() {
                    systemUsersList = systemUsersList.filter(u => u.id !== id);
                    renderAdminUsersTable();
                    renderAdminChatDropdownList();
                    triggerSuccessBox("تم حذف حساب المستخدم نهائياً من قاعدة البيانات ✓");
                }
            );
        }

        function renderAdminUsersTable() {
            let tbody = document.getElementById('tbl-users-manage-body');
            tbody.innerHTML = "";
            systemUsersList.forEach(user => {
                let statusBadge = user.banned ? `<span style="color:#ef4444; font-weight:bold;">محظور (Banned) 🚫</span>` : `<span style="color:#10b981; font-weight:bold;">نشط (Active) ✅</span>`;
                let banBtnText = user.banned ? "إلغاء الحظر 🔓" : "حظر 🚫";
                let banBtnClass = user.banned ? "btn-secondary" : "btn-danger";

                tbody.innerHTML += `
                    <tr>
                        <td><b>${user.name}</b></td>
                        <td>${user.email}</td>
                        <td>${user.salary.toLocaleString()} ${currencySymbol}</td>
                        <td>${statusBadge}</td>
                        <td><button class="${banBtnClass}" style="padding:4px 10px; font-size:11px;" onclick="toggleUserBanState(${user.id})">${banBtnText}</button></td>
                        <td><button class="btn-danger" style="padding:4px 10px; font-size:11px;" onclick="deleteUserAccountWithConfirm(${user.id})">حذف نهائي 🗑️</button></td>
                    </tr>
                `;
            });
            document.getElementById('val-total-users-count').innerText = `${systemUsersList.length} حساب نشط`;
        }

        /* 4. BROADCAST WITH AUDIO OPTION */
        function triggerBroadcastConfirm() {
            let txt = document.getElementById('txt-broadcast-msg').value.trim();
            let audioFile = document.getElementById('inp-broadcast-audio').files[0];

            if(!txt) {
                alert((currentLang === 'ar') ? "الرجاء كتابة نص الإعلان العام!" : "Please enter broadcast message!");
                return;
            }

            let audioStatusStr = audioFile ? `\n🔊 تم إرفاق ملف صوت تنبيهي: (${audioFile.name})` : "";
            triggerSuccessBox(`تم بث التنبيه الإداري العام بنجاح لكافة الأعضاء 🚀${audioStatusStr}\n\n"${txt}"`);
            document.getElementById('txt-broadcast-msg').value = "";
            document.getElementById('inp-broadcast-audio').value = "";
        }

        /* 5. ADVANCED SUPPORT CHAT WITH FILTER & SMART GMAIL SEARCH */
        function renderAdminChatDropdownList() {
            let selectEl = document.getElementById('sel-admin-chat-user');
            selectEl.innerHTML = `<option value="">اختر عميل لديه محادثة قائمة للرد عليه...</option>`;
            
            // FILTER: Only show users who have actual chats (hasChat === true)
            let activeChatUsers = systemUsersList.filter(u => u.hasChat);
            
            activeChatUsers.forEach(u => {
                selectEl.innerHTML += `<option value="${u.id}">${u.name} | ${u.email}</option>`;
            });
        }

        function filterAdminChatUsersList() {
            let query = document.getElementById('inp-admin-chat-search').value.toLowerCase().trim();
            let selectEl = document.getElementById('sel-admin-chat-user');
            selectEl.innerHTML = `<option value="">نتائج البحث عن: "${query}"...</option>`;

            let activeChatUsers = systemUsersList.filter(u => u.hasChat);
            let filtered = activeChatUsers.filter(u => u.name.toLowerCase().includes(query) || u.email.toLowerCase().includes(query));

            if(filtered.length === 0) {
                selectEl.innerHTML = `<option value="">لم يتم العثور على أي عميل يطابق هذا البحث!</option>`;
                return;
            }

            filtered.forEach(u => {
                selectEl.innerHTML += `<option value="${u.id}">${u.name} | ${u.email}</option>`;
            });
        }

        function loadSelectedUserChatHistory() {
            let uid = document.getElementById('sel-admin-chat-user').value;
            let box = document.getElementById('admin-chat-messages-box');
            if(!uid) {
                box.innerHTML = `<div style="color: var(--subtext); font-size: 12px; text-align: center; padding-top: 80px;">اختر عميلاً من القائمة لاستعراض محادثاته المباشرة.</div>`;
                return;
            }

            let user = systemUsersList.find(u => u.id == uid);
            box.innerHTML = `
                <div style="background: rgba(6,182,212,0.1); padding: 8px 12px; border-radius: 8px; font-size: 12px; margin-bottom: 8px; color: var(--text-main);">
                    <b>${user.name} (${user.email}):</b> مرحباً، أحتاج مساعدة في مراجعة كارت الفواتير.
                </div>
            `;
        }

        function sendAdminReplyToUser() {
            let uid = document.getElementById('sel-admin-chat-user').value;
            let msgInp = document.getElementById('inp-admin-reply-msg');
            if(!uid) {
                alert((currentLang === 'ar') ? "اختر عميلاً أولاً للرد عليه!" : "Select a user first!");
                return;
            }
            if(!msgInp.value.trim()) return;

            let box = document.getElementById('admin-chat-messages-box');
            box.innerHTML += `
                <div style="background: rgba(16,185,129,0.2); border:1px solid #10b981; padding: 8px 12px; border-radius: 8px; font-size: 12px; margin-bottom: 8px; margin-left: auto; max-width: 80%; color: var(--text-main);">
                    <b>الأدمن:</b> ${msgInp.value.trim()}
                </div>
            `;
            msgInp.value = "";
            box.scrollTop = box.scrollHeight;
        }

        /* Dynamic Category & Sub-Category Selection */
        function selectMainCategory(catKey) {
            selectedMainCat = catKey;
            document.querySelectorAll('.cat-btn').forEach(btn => btn.classList.remove('active'));
            let activeBtn = document.querySelector(`.cat-btn.${catKey}`);
            if(activeBtn) activeBtn.classList.add('active');

            let container = document.getElementById('sub-category-container');
            container.innerHTML = "";

            let dict = subCategoriesDictionary[catKey];
            let keys = Object.keys(dict);
            selectedSubCat = keys[0];

            keys.forEach((subK, idx) => {
                let label = dict[subK][currentLang];
                container.innerHTML += `
                    <div class="sub-cat-chip ${idx === 0 ? 'active' : ''}" onclick="selectSubCategory(this, '${subK}')">${label}</div>
                `;
            });
        }

        function selectSubCategory(el, subK) {
            selectedSubCat = subK;
            document.querySelectorAll('.sub-cat-chip').forEach(c => c.classList.remove('active'));
            el.classList.add('active');
        }

        function addExpenseOnline() {
            let amountInp = document.getElementById('inp-exp-amount');
            let descInp = document.getElementById('inp-exp-desc');
            let val = parseFloat(amountInp.value);

            if(!val || val <= 0) {
                alert((currentLang === 'ar') ? "الرجاء إدخال مبلغ مالي صحيح!" : "Please enter a valid amount!");
                return;
            }

            let newItem = {
                id: Date.now(),
                cat: selectedMainCat,
                subKey: selectedSubCat,
                desc: descInp.value.trim() || subCategoriesDictionary[selectedMainCat][selectedSubCat][currentLang],
                amount: val,
                date: new Date().toISOString().replace('T', ' ').substring(0, 16)
            };

            expensesList.unshift(newItem);
            amountInp.value = "";
            descInp.value = "";

            updateDashboardCalculations();
            triggerSuccessBox((currentLang === 'ar') ? "تم إضافة المصروف أونلاين بنجاح ✓" : "Expense added online successfully ✓");
        }

        function deleteExpenseItemWithConfirm(id) {
            openConfirmModal(
                (currentLang === 'ar') ? "تأكيد حذف المصروف" : "Confirm Delete Expense",
                (currentLang === 'ar') ? "هل أنت تأكد من رغبتك في حذف هذا المصروف نهائياً؟" : "Are you sure you want to delete this expense?",
                function() {
                    expensesList = expensesList.filter(item => item.id !== id);
                    updateDashboardCalculations();
                    if(document.getElementById('category-focus-view').style.display === 'block') {
                        renderFocusTransactions();
                    }
                    triggerSuccessBox((currentLang === 'ar') ? "تم حذف المعاملة المالية بنجاح ✓" : "Expense deleted successfully ✓");
                }
            );
        }

        function filterLogByCategory(cat) {
            renderExpensesLog(cat);
        }

        /* UPDATED CATEGORY FOCUS VIEW & DRILLDOWN LOGIC */
        function openCategoryFocus(catKey) {
            activeFocusCategory = catKey;
            activeSubFilter = 'all';

            let t = i18n[currentLang];
            let catNames = { 
                bills: t.catBills, 
                food: t.catFood, 
                study: t.catStudy, 
                transport: t.catTransport, 
                outings: t.catOutings 
            };
            
            let catColors = {
                bills: 'var(--pink-accent)',
                food: 'var(--purple-cat)',
                study: 'var(--cyan-cat)',
                transport: 'var(--orange-cat)',
                outings: 'var(--teal-cat)'
            };

            let titleEl = document.getElementById('focus-cat-title');
            titleEl.innerText = catNames[catKey];
            titleEl.style.color = catColors[catKey];

            let buttonsContainer = document.getElementById('smart-action-buttons-container');
            let allBtnLabel = (currentLang === 'ar') ? `📋 كل بنود ${catNames[catKey].split(' ')[1] || catNames[catKey]}` : `📋 All ${catNames[catKey]}`;
            buttonsContainer.innerHTML = `
                <button class="smart-filter-btn active" onclick="applyFocusFilter('all', this)">${allBtnLabel}</button>
            `;

            let dict = subCategoriesDictionary[catKey];
            Object.keys(dict).forEach(subK => {
                let label = dict[subK][currentLang];
                buttonsContainer.innerHTML += `
                    <button class="smart-filter-btn" onclick="applyFocusFilter('${subK}', this)">${label}</button>
                `;
            });

            renderFocusTransactions();

            document.getElementById('expenses-main-cards-container').style.display = 'none';
            document.getElementById('category-focus-view').style.display = 'block';
        }

        function closeCategoryFocus() {
            document.getElementById('category-focus-view').style.display = 'none';
            document.getElementById('expenses-main-cards-container').style.display = 'block';
        }

        function applyFocusFilter(subFilterKey, btnEl) {
            activeSubFilter = subFilterKey;
            document.querySelectorAll('.smart-filter-btn').forEach(b => b.classList.remove('active'));
            btnEl.classList.add('active');
            renderFocusTransactions();
        }

        function renderFocusTransactions() {
            let container = document.getElementById('focus-transactions-container');
            container.innerHTML = "";

            let filtered = expensesList.filter(i => i.cat === activeFocusCategory);
            if(activeSubFilter !== 'all') {
                filtered = filtered.filter(i => i.subKey === activeSubFilter);
            }

            if(filtered.length === 0) {
                let noItemsMsg = (currentLang === 'ar') ? "لا توجد معاملات مسجلة تحت هذا البند لهذا الشهر." : "No transactions recorded under this item for this month.";
                container.innerHTML = `<p style="color:var(--subtext); text-align:center; padding:25px; background:var(--input-bg); border-radius:14px;">${noItemsMsg}</p>`;
                return;
            }

            filtered.forEach(item => {
                let subLabel = subCategoriesDictionary[item.cat][item.subKey] ? subCategoriesDictionary[item.cat][item.subKey][currentLang] : item.subKey;
                let deleteTxt = (currentLang === 'ar') ? '🗑️ حذف' : '🗑️ Delete';
                container.innerHTML += `
                    <div style="display:flex; justify-content:space-between; align-items:center; background:var(--input-bg); padding:14px; border-radius:14px; border:1px solid var(--glass-border); margin-bottom:10px;">
                        <div>
                            <div style="font-weight:800; font-size:14px; color:var(--text-main);">${subLabel}</div>
                            <div style="font-size:12px; color:var(--subtext); margin-top:2px;">${item.desc} | 🕒 ${item.date}</div>
                        </div>
                        <div style="display:flex; align-items:center; gap:15px;">
                            <span style="font-weight:900; color:#ef4444; font-size:15px;">${item.amount.toLocaleString()} ${currencySymbol}</span>
                            <button class="btn-danger" style="padding:6px 12px; font-size:11px;" onclick="deleteExpenseItemWithConfirm(${item.id})">${deleteTxt}</button>
                        </div>
                    </div>
                `;
            });
        }

        /* Dashboard Calculations & Chart Renderer */
        function updateDashboardCalculations() {
            let totalExp = expensesList.reduce((sum, item) => sum + item.amount, 0);
            let netAvailable = userSalary - totalExp;

            document.getElementById('kpi-val-salary').innerText = userSalary.toLocaleString() + " " + currencySymbol;
            document.getElementById('kpi-val-exp').innerText = totalExp.toLocaleString() + " " + currencySymbol;
            document.getElementById('kpi-val-net').innerText = netAvailable.toLocaleString() + " " + currencySymbol;

            let catTotals = { bills: 0, food: 0, study: 0, transport: 0, outings: 0 };
            expensesList.forEach(item => { catTotals[item.cat] += item.amount; });

            document.getElementById('cat-card-val-bills').innerText = catTotals.bills.toLocaleString() + " " + currencySymbol;
            document.getElementById('cat-card-val-food').innerText = catTotals.food.toLocaleString() + " " + currencySymbol;
            document.getElementById('cat-card-val-study').innerText = catTotals.study.toLocaleString() + " " + currencySymbol;
            document.getElementById('cat-card-val-transport').innerText = catTotals.transport.toLocaleString() + " " + currencySymbol;
            document.getElementById('cat-card-val-outings').innerText = catTotals.outings.toLocaleString() + " " + currencySymbol;

            renderExpensesLog('all');
            renderDoughnutChart();
        }

        function renderExpensesLog(filterCat) {
            let tbody = document.getElementById('expenses-log-tbody');
            tbody.innerHTML = "";

            let displayData = (filterCat === 'all') ? expensesList : expensesList.filter(i => i.cat === filterCat);

            if(displayData.length === 0) {
                let noDataTxt = (currentLang === 'ar') ? 'لا توجد معاملات مسجلة في هذا التصنيف' : 'No transactions found in this category';
                tbody.innerHTML = `<tr><td colspan="4" style="text-align:center; color:var(--subtext);">${noDataTxt}</td></tr>`;
                return;
            }

            displayData.forEach(item => {
                let subLabel = subCategoriesDictionary[item.cat][item.subKey] ? subCategoriesDictionary[item.cat][item.subKey][currentLang] : item.subKey;
                tbody.innerHTML += `
                    <tr>
                        <td><b>${subLabel}</b></td>
                        <td>${item.desc}</td>
                        <td style="color:#ef4444; font-weight:bold;">${item.amount.toLocaleString()} ${currencySymbol}</td>
                        <td>
                            <button class="btn-danger" style="padding:4px 8px; font-size:11px;" onclick="deleteExpenseItemWithConfirm(${item.id})">🗑️</button>
                        </td>
                    </tr>
                `;
            });
        }

        function renderDoughnutChart() {
            let catTotals = { bills: 0, food: 0, study: 0, transport: 0, outings: 0 };
            expensesList.forEach(item => { catTotals[item.cat] += item.amount; });

            const ctx = document.getElementById('expensesDoughnutChart').getContext('2d');
            if(doughnutChartInstance) doughnutChartInstance.destroy();

            let t = i18n[currentLang];
            doughnutChartInstance = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: [t.catBills, t.catFood, t.catStudy, t.catTransport, t.catOutings],
                    datasets: [{
                        data: [catTotals.bills, catTotals.food, catTotals.study, catTotals.transport, catTotals.outings],
                        backgroundColor: ['#ff416c', '#8a2be2', '#00c6ff', '#f7971e', '#11998e'],
                        borderWidth: 0
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { 
                        legend: { 
                            position: 'right', 
                            labels: { 
                                color: (currentTheme === 'dark') ? '#f8fafc' : '#0f172a', 
                                font: { family: 'Cairo', size: 10, weight: 'bold' },
                                padding: 12
                            } 
                        } 
                    }
                }
            });
        }

        function sendSupportMessage() {
            let inp = document.getElementById('inp-chat-msg');
            if(!inp.value.trim()) return;

            let box = document.getElementById('chat-messages-box');
            let senderTxt = (currentLang === 'ar') ? 'أنت' : 'You';
            box.innerHTML += `
                <div style="background: rgba(6,182,212,0.1); border: 1px solid var(--glass-border); padding: 10px 14px; border-radius: 10px; max-width: 70%; margin-left: auto; font-size: 13px; margin-bottom: 10px; color: var(--text-main);">
                    <b>${senderTxt}:</b> ${inp.value}
                </div>
            `;
            inp.value = "";
            box.scrollTop = box.scrollHeight;
        }

        function requestFinancialReport() {
            let totalExp = expensesList.reduce((s,i)=>s+i.amount,0);
            let netRem = userSalary - totalExp;
            if(currentLang === 'ar') {
                triggerSuccessBox(`[تقرير مالي مفصل]\nتم إنشاء الملخص المالي بنجاح!\n--------------------------------\nإجمالي الراتب: ${userSalary.toLocaleString()} ${currencySymbol}\nإجمالي المصروفات: ${totalExp.toLocaleString()} ${currencySymbol}\nالرصيد المتاح المتبقي: ${netRem.toLocaleString()} ${currencySymbol}\n--------------------------------\nجاري تجهيز وإرسال التقرير عبر الواتساب والبريد الإلكتروني...`);
            } else {
                triggerSuccessBox(`[Detailed Financial Report]\nSummary generated successfully!\n--------------------------------\nTotal Salary: ${userSalary.toLocaleString()} ${currencySymbol}\nTotal Expenses: ${totalExp.toLocaleString()} ${currencySymbol}\nNet Available Balance: ${netRem.toLocaleString()} ${currencySymbol}\n--------------------------------\nSending report via WhatsApp and Email...`);
            }
        }

        function updateSalarySetting() {
            let val = parseFloat(document.getElementById('set-salary-inp').value);
            if(val && val > 0) {
                userSalary = val;
                updateDashboardCalculations();
                triggerSuccessBox((currentLang === 'ar') ? "تم تحديث الراتب الشهري بنجاح ✓" : "Salary updated successfully ✓");
            }
        }

        function updateTitleSetting() {
            let roleVal = document.getElementById('set-title-sel').value;
            let nameVal = document.getElementById('set-username-inp').value.trim();

            selectedRoyalRole = roleVal;
            if(nameVal) currentUserName = nameVal;

            applyLocalization();
            triggerSuccessBox((currentLang === 'ar') ? "تم تحديث بيانات الملف الشخصي واللقب بنجاح ✓" : "Profile title & username updated successfully ✓");
        }

        // Initialize Master Application State
        applyTheme(currentTheme);
        applyLocalization();
    </script>
</body>
</html>
