# Task_income_bd
<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>TaskEarn - টাস্ক থেকে আয় করুন</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+Bengali:wght@300;400;500;600;700;800;900&family=Space+Grotesk:wght@400;500;600;700&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --bg-primary: #0a0a0f;
            --bg-secondary: #12121a;
            --bg-card: #1a1a25;
            --fg-primary: #f5f5f7;
            --fg-secondary: #a0a0b0;
            --accent: #f59e0b;
            --accent-glow: rgba(245, 158, 11, 0.3);
            --success: #10b981;
            --danger: #ef4444;
            --border: #2a2a3a;
            --admin-accent: #6366f1;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Noto Sans Bengali', sans-serif;
            background: var(--bg-primary);
            color: var(--fg-primary);
            min-height: 100vh;
            overflow-x: hidden;
        }
        .font-display { font-family: 'Space Grotesk', sans-serif; }
        
        .hidden { display: none !important; }
        .screen { display: none; min-height: 100vh; animation: fadeIn 0.3s ease-out; }
        .screen.active { display: block; }
        
        .app-container { max-width: 480px; margin: 0 auto; position: relative; padding-bottom: 80px; }
        
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes slideUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
        
        .gradient-bg { background: linear-gradient(135deg, var(--bg-primary) 0%, #0f0f18 50%, #0a0a12 100%); }
        .glass-card { background: rgba(26, 26, 37, 0.8); backdrop-filter: blur(20px); border: 1px solid var(--border); border-radius: 20px; }
        
        .btn-primary { background: linear-gradient(135deg, #f59e0b 0%, #d97706 100%); color: #000; font-weight: 600; padding: 14px 28px; border-radius: 12px; border: none; cursor: pointer; transition: all 0.3s ease; font-family: inherit; font-size: 16px; }
        .btn-primary:hover { transform: translateY(-2px); box-shadow: 0 10px 30px var(--accent-glow); }
        
        .btn-secondary { background: var(--bg-card); color: var(--fg-primary); border: 1px solid var(--border); padding: 14px 28px; border-radius: 12px; cursor: pointer; transition: all 0.3s ease; font-family: inherit; font-size: 16px; font-weight: 500; }
        .btn-secondary:hover { border-color: var(--accent); background: rgba(245, 158, 11, 0.1); }
        
        .input-field { width: 100%; background: var(--bg-secondary); border: 1px solid var(--border); border-radius: 12px; padding: 16px 20px; color: var(--fg-primary); font-size: 16px; font-family: inherit; transition: all 0.3s ease; }
        .input-field:focus { outline: none; border-color: var(--accent); box-shadow: 0 0 0 3px var(--accent-glow); }
        
        .balance-card { background: linear-gradient(135deg, #1a1a25 0%, #252530 100%); border-radius: 24px; padding: 24px; position: relative; overflow: hidden; }
        .balance-card::before { content: ''; position: absolute; top: -50%; right: -50%; width: 100%; height: 100%; background: radial-gradient(circle, var(--accent-glow) 0%, transparent 70%); animation: float 4s ease-in-out infinite; }
        @keyframes float { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }

        .bottom-nav { position: fixed; bottom: 0; left: 50%; transform: translateX(-50%); width: 100%; max-width: 480px; background: rgba(10, 10, 15, 0.95); backdrop-filter: blur(20px); border-top: 1px solid var(--border); padding: 12px 0; z-index: 1000; }
        .nav-item { display: flex; flex-direction: column; align-items: center; gap: 4px; padding: 8px 16px; cursor: pointer; transition: all 0.3s ease; color: var(--fg-secondary); }
        .nav-item.active { color: var(--accent); }
        
        /* Admin Styles */
        .admin-sidebar { width: 260px; height: 100vh; position: fixed; left: 0; top: 0; background: var(--bg-secondary); border-right: 1px solid var(--border); z-index: 100; transition: transform 0.3s; }
        .admin-main { margin-left: 260px; padding: 24px; min-height: 100vh; }
        .admin-nav-item { display: flex; align-items: center; gap: 12px; padding: 12px 20px; color: var(--fg-secondary); cursor: pointer; transition: all 0.2s; border-left: 3px solid transparent; }
        .admin-nav-item.active { background: rgba(99, 102, 241, 0.1); color: var(--fg-primary); border-left-color: var(--admin-accent); }
        
        .card-admin { background: var(--bg-card); border: 1px solid var(--border); border-radius: 16px; padding: 24px; overflow-x: auto; }
        table { width: 100%; border-collapse: collapse; min-width: 600px; }
        th, td { padding: 12px 16px; text-align: left; border-bottom: 1px solid var(--border); }
        th { color: var(--fg-secondary); font-weight: 500; font-size: 14px; }
        tr:hover { background: rgba(255,255,255,0.02); }
        
        .badge { padding: 4px 10px; border-radius: 20px; font-size: 11px; font-weight: 600; }
        .badge-pending { background: rgba(245, 158, 11, 0.2); color: #fbbf24; }
        .badge-approved { background: rgba(16, 185, 129, 0.2); color: #10b981; }
        .badge-rejected { background: rgba(239, 68, 68, 0.2); color: #ef4444; }
        
        .btn-sm { padding: 6px 12px; border-radius: 6px; font-size: 12px; font-weight: 600; cursor: pointer; border: none; transition: all 0.2s; }
        .btn-approve { background: var(--success); color: #fff; }
        .btn-reject { background: var(--danger); color: #fff; }
        .btn-edit { background: var(--admin-accent); color: #fff; }
        
        .modal { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); display: none; justify-content: center; align-items: center; z-index: 2000; }
        .modal.active { display: flex; }
        .modal-content { background: var(--bg-card); border: 1px solid var(--border); border-radius: 16px; padding: 24px; width: 90%; max-width: 500px; animation: slideUp 0.3s ease; }

        @media (max-width: 768px) {
            .admin-sidebar { transform: translateX(-100%); z-index: 101; }
            .admin-sidebar.open { transform: translateX(0); }
            .admin-main { margin-left: 0; }
        }
        
        .admin-toggle-btn {
            position: fixed; top: 16px; left: 16px; z-index: 99; background: var(--bg-secondary);
            border: 1px solid var(--border); padding: 10px; border-radius: 10px; cursor: pointer; display: none;
        }
        @media (max-width: 768px) { .admin-toggle-btn { display: block; } }
    </style>
</head>
<body>

    <!-- Toast Notification -->
    <div id="toast" class="fixed top-5 left-1/2 transform -translate-x-1/2 -translate-y-20 bg-gray-800 border border-gray-700 px-6 py-3 rounded-xl z-50 transition-transform duration-300" style="max-width: 90%;">
        <p id="toastMessage"></p>
    </div>

    <!-- ======================= -->
    <!-- PART 1: USER APP SCREENS -->
    <!-- ======================= -->

    <!-- Auth Screen -->
    <div class="screen active" id="authScreen">
        <div class="gradient-bg min-h-screen flex flex-col items-center justify-center p-6">
            <div class="text-center" style="animation: slideUp 0.5s ease-out">
                <div class="w-16 h-16 bg-gradient-to-br from-amber-500 to-orange-600 rounded-2xl flex items-center justify-center mx-auto mb-4 shadow-lg">
                    <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="#000" stroke-width="2.5"><circle cx="12" cy="12" r="10"/><path d="M12 6v6l4 2"/></svg>
                </div>
                <h1 class="font-display text-4xl font-bold mb-2">TaskEarn</h1>
                <p class="text-gray-400 mb-8">টাস্ক করুন, আয় করুন</p>
            </div>

            <!-- Login Form -->
            <div class="w-full max-w-sm" id="loginForm">
                <div class="glass-card p-6">
                    <h2 class="text-xl font-bold mb-6 text-center">লগইন করুন</h2>
                    <div class="space-y-4">
                        <div>
                            <label class="block text-sm text-gray-400 mb-2">ফোন নম্বর</label>
                            <input type="tel" id="loginPhone" class="input-field" placeholder="০১XXXXXXXXX">
                        </div>
                        <div>
                            <label class="block text-sm text-gray-400 mb-2">পাসওয়ার্ড</label>
                            <input type="password" id="loginPassword" class="input-field" placeholder="পাসওয়ার্ড দিন">
                        </div>
                        <button class="btn-primary w-full mt-6" onclick="handleLogin()">লগইন করুন</button>
                    </div>
                    <p class="text-center text-gray-400 mt-6">অ্যাকাউন্ট নেই? <span class="text-amber-500 cursor-pointer font-semibold" onclick="showSignup()">রেজিস্ট্রেশন করুন</span></p>
                    
                    <!-- Admin Link -->
                    <div class="mt-8 pt-4 border-t border-gray-700 text-center">
                        <span class="text-gray-600 text-xs cursor-pointer hover:text-gray-400" onclick="showScreen('adminLoginScreen')">Admin Panel</span>
                    </div>
                </div>
            </div>

            <!-- Signup Form -->
            <div class="w-full max-w-sm hidden" id="signupForm">
                <div class="glass-card p-6">
                    <h2 class="text-xl font-bold mb-6 text-center">রেজিস্ট্রেশন করুন</h2>
                    <div class="space-y-4">
                        <div>
                            <label class="block text-sm text-gray-400 mb-2">আপনার নাম</label>
                            <input type="text" id="signupName" class="input-field" placeholder="আপনার নাম লিখুন">
                        </div>
                        <div>
                            <label class="block text-sm text-gray-400 mb-2">ফোন নম্বর</label>
                            <input type="tel" id="signupPhone" class="input-field" placeholder="০১XXXXXXXXX">
                        </div>
                        <div>
                            <label class="block text-sm text-gray-400 mb-2">পাসওয়ার্ড</label>
                            <input type="password" id="signupPassword" class="input-field" placeholder="পাসওয়ার্ড দিন">
                        </div>
                        <div>
                            <label class="block text-sm text-gray-400 mb-2">রেফারেল কোড (ঐচ্ছিক)</label>
                            <input type="text" id="referralCode" class="input-field" placeholder="রেফারেল কোড থাকলে দিন">
                        </div>
                        <button class="btn-primary w-full mt-6" onclick="handleSignup()">রেজিস্ট্রেশন করুন</button>
                    </div>
                    <p class="text-center text-gray-400 mt-6">অ্যাকাউন্ট আছে? <span class="text-amber-500 cursor-pointer font-semibold" onclick="showLogin()">লগইন করুন</span></p>
                </div>
            </div>
        </div>
    </div>

    <!-- Dashboard Screen -->
    <div class="screen" id="dashboardScreen">
        <div class="gradient-bg p-6 pt-8 app-container">
            <div class="flex justify-between items-center mb-6">
                <div>
                    <p class="text-gray-400 text-sm">স্বাগতম</p>
                    <h2 class="text-xl font-bold" id="dashboardName">ব্যবহারকারী</h2>
                </div>
                <button onclick="logout()" class="p-3 rounded-full bg-gray-800 hover:bg-gray-700 transition">
                    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/><polyline points="16 17 21 12 16 7"/><line x1="21" y1="12" x2="9" y2="12"/></svg>
                </button>
            </div>

            <div class="balance-card mb-6">
                <div class="relative z-10">
                    <p class="text-gray-400 text-sm mb-1">মোট ব্যালেন্স</p>
                    <h1 class="font-display text-4xl font-bold text-amber-500" id="totalBalance">৳0.00</h1>
                    <div class="flex gap-4 mt-4">
                        <div>
                            <p class="text-gray-500 text-xs">মোট উইথড্র</p>
                            <p class="font-semibold text-blue-400" id="totalWithdrawn">৳0.00</p>
                        </div>
                    </div>
                </div>
            </div>

            <div class="grid grid-cols-3 gap-3 mb-6">
                <div class="bg-gray-800 rounded-xl p-3 text-center border border-gray-700">
                    <p class="text-xl font-bold text-amber-500" id="completedTasks">0</p>
                    <p class="text-xs text-gray-400">টাস্ক</p>
                </div>
                <div class="bg-gray-800 rounded-xl p-3 text-center border border-gray-700">
                    <p class="text-xl font-bold text-green-500" id="totalReferrals">0</p>
                    <p class="text-xs text-gray-400">রেফারেল</p>
                </div>
                <div class="bg-gray-800 rounded-xl p-3 text-center border border-gray-700">
                    <p class="text-xl font-bold text-blue-500" id="pendingWithdraw">0</p>
                    <p class="text-xs text-gray-400">পেন্ডিং</p>
                </div>
            </div>

            <div class="flex justify-between items-center mb-4">
                <h3 class="font-bold text-lg">উপলব্ধ টাস্ক</h3>
                <span class="text-amber-500 text-sm cursor-pointer" onclick="showScreen('tasksScreen')">সব দেখুন</span>
            </div>
            <div class="space-y-3" id="dashboardTasks"></div>
        </div>
    </div>

    <!-- Tasks Screen -->
    <div class="screen" id="tasksScreen">
        <div class="gradient-bg p-6 pt-8 app-container">
            <h2 class="text-2xl font-bold mb-6">টাস্ক সমূহ</h2>
            <div class="space-y-3" id="allTasks"></div>
        </div>
    </div>

    <!-- Withdraw Screen -->
    <div class="screen" id="withdrawScreen">
        <div class="gradient-bg p-6 pt-8 app-container">
            <h2 class="text-2xl font-bold mb-6">উইথড্র করুন</h2>
            <div class="glass-card p-4 mb-6">
                <div class="flex justify-between items-center">
                    <div>
                        <p class="text-gray-400 text-sm">উপলব্ধ ব্যালেন্স</p>
                        <p class="text-2xl font-bold text-amber-500" id="withdrawableBalance">৳0.00</p>
                    </div>
                    <div class="text-right">
                        <p class="text-gray-400 text-xs">ন্যূনতম</p>
                        <p class="font-semibold">৳50</p>
                    </div>
                </div>
            </div>

            <h3 class="font-semibold mb-3">পেমেন্ট মেথড</h3>
            <div class="grid grid-cols-2 gap-3 mb-6">
                <div class="withdraw-method glass-card p-4 cursor-pointer" onclick="selectMethod('bkash')" id="bkashMethod">
                    <p class="font-semibold text-pink-500">বিকাশ</p>
                </div>
                <div class="withdraw-method glass-card p-4 cursor-pointer" onclick="selectMethod('nagad')" id="nagadMethod">
                    <p class="font-semibold text-orange-500">নগদ</p>
                </div>
            </div>

            <div class="space-y-4">
                <input type="tel" id="accountNumber" class="input-field" placeholder="একাউন্ট নম্বর">
                <input type="number" id="withdrawAmount" class="input-field" placeholder="পরিমাণ (টাকা)">
                <button class="btn-primary w-full" onclick="handleWithdraw()">উইথড্র রিকুয়েস্ট</button>
            </div>
        </div>
    </div>

    <!-- History Screen -->
    <div class="screen" id="historyScreen">
        <div class="gradient-bg p-6 pt-8 app-container">
            <h2 class="text-2xl font-bold mb-6">উইথড্র ইতিহাস</h2>
            <div id="withdrawHistoryList"></div>
        </div>
    </div>

    <!-- Referral Screen -->
    <div class="screen" id="referralScreen">
        <div class="gradient-bg p-6 pt-8 app-container">
            <h2 class="text-2xl font-bold mb-6">রেফারেল প্রোগ্রাম</h2>
            
            <!-- NEW: Redeem Code Section -->
            <div class="glass-card p-6 mb-6" id="redeemSection">
                <h3 class="font-bold mb-2 text-lg">রেফারেল কোড ব্যবহার করুন</h3>
                <p class="text-xs text-gray-400 mb-3">বন্ধুর কোড ব্যবহার করে আপনি এবং আপনার বন্ধু উভয়ে ৫ টাকা করে পাবেন।</p>
                
                <div id="redeemInputArea" class="flex gap-2">
                    <input type="text" id="redeemCodeInput" class="input-field flex-1" placeholder="কোড লিখুন">
                    <button onclick="redeemReferralCode()" class="btn-primary py-2 px-4 text-sm">জমা দিন</button>
                </div>
                <div id="redeemStatusArea" class="hidden text-center">
                    <p class="text-green-500 font-semibold flex items-center justify-center gap-2">
                        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3"><path d="M20 6L9 17l-5-5"/></svg>
                        আপনি ইতিমধ্যে কোড ব্যবহার করেছেন
                    </p>
                </div>
            </div>

            <!-- Share Code Section -->
            <div class="glass-card p-6 mb-6 text-center border-2 border-dashed border-amber-500">
                <p class="text-gray-400 text-sm mb-2">আপনার রেফারেল কোড শেয়ার করুন</p>
                <p class="text-3xl font-bold font-display text-amber-500 mb-3" id="myReferralCode">XXXXXX</p>
                <button class="bg-amber-500 text-black px-4 py-2 rounded-lg font-semibold" onclick="copyReferralCode()">কপি করুন</button>
            </div>
            
            <div class="grid grid-cols-2 gap-3">
                <div class="bg-gray-800 rounded-xl p-4 text-center">
                    <p class="text-2xl font-bold text-amber-500" id="referralCount">0</p>
                    <p class="text-xs text-gray-400">মোট রেফারেল</p>
                </div>
                <div class="bg-gray-800 rounded-xl p-4 text-center">
                    <p class="text-2xl font-bold text-green-500" id="referralEarnings">৳0</p>
                    <p class="text-xs text-gray-400">রেফারেল আয়</p>
                </div>
            </div>
        </div>
    </div>

    <!-- User Bottom Nav -->
    <div class="bottom-nav hidden" id="bottomNav">
        <div class="flex justify-around">
            <div class="nav-item active" onclick="showScreen('dashboardScreen')" id="navDashboard">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/></svg>
                <span class="text-xs">হোম</span>
            </div>
            <div class="nav-item" onclick="showScreen('tasksScreen')" id="navTasks">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 11l3 3L22 4"/><path d="M21 12v7a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11"/></svg>
                <span class="text-xs">টাস্ক</span>
            </div>
            <div class="nav-item" onclick="showScreen('withdrawScreen')" id="navWithdraw">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="12" y1="1" x2="12" y2="23"/><path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"/></svg>
                <span class="text-xs">উইথড্র</span>
            </div>
            <div class="nav-item" onclick="showScreen('historyScreen')" id="navHistory">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
                <span class="text-xs">ইতিহাস</span>
            </div>
            <div class="nav-item" onclick="showScreen('referralScreen')" id="navReferral">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/></svg>
                <span class="text-xs">রেফার</span>
            </div>
        </div>
    </div>

    <!-- ======================= -->
    <!-- PART 2: ADMIN PANEL     -->
    <!-- ======================= -->

    <!-- Admin Login -->
    <div class="screen" id="adminLoginScreen">
        <div class="gradient-bg min-h-screen flex flex-col items-center justify-center p-6">
            <div class="glass-card p-8 w-full max-w-sm">
                <h2 class="text-2xl font-bold mb-6 text-center text-indigo-400">Admin Login</h2>
                <div class="space-y-4">
                    <input type="password" id="adminPasswordInput" class="input-field" placeholder="Enter Admin Password">
                    <button onclick="handleAdminLogin()" class="w-full py-3 bg-indigo-600 hover:bg-indigo-700 rounded-xl font-semibold transition">Login</button>
                    <p class="text-center text-gray-500 text-sm cursor-pointer" onclick="showScreen('authScreen'); showLogin();">Back to User App</p>
                </div>
            </div>
        </div>
    </div>

    <!-- Admin Dashboard -->
    <div class="screen" id="adminPanelScreen">
        <!-- Mobile Menu Toggle -->
        <div class="admin-toggle-btn" onclick="toggleAdminSidebar()">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <line x1="3" y1="6" x2="21" y2="6"></line>
                <line x1="3" y1="12" x2="21" y2="12"></line>
                <line x1="3" y1="18" x2="21" y2="18"></line>
            </svg>
        </div>

        <!-- Sidebar -->
        <div class="admin-sidebar" id="adminSidebar">
            <div class="p-5 border-b border-gray-800">
                <h1 class="font-display text-xl font-bold text-indigo-400">TaskEarn Admin</h1>
            </div>
            <nav class="mt-4">
                <div class="admin-nav-item active" onclick="showAdminSection('dashboard')">
                    <svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><rect x="3" y="3" width="7" height="7"/><rect x="14" y="3" width="7" height="7"/><rect x="14" y="14" width="7" height="7"/><rect x="3" y="14" width="7" height="7"/></svg>
                    <span>ড্যাশবোর্ড</span>
                </div>
                <div class="admin-nav-item" onclick="showAdminSection('users')">
                    <svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/></svg>
                    <span>ইউজার ম্যানেজমেন্ট</span>
                </div>
                <div class="admin-nav-item" onclick="showAdminSection('withdraws')">
                    <svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><line x1="12" y1="1" x2="12" y2="23"/><path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"/></svg>
                    <span>উইথড্র রিকুয়েস্ট</span>
                </div>
                <div class="admin-nav-item" onclick="showAdminSection('tasks')">
                    <svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path d="M9 11l3 3L22 4"/><path d="M21 12v7a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11"/></svg>
                    <span>টাস্ক ম্যানেজমেন্ট</span>
                </div>
                <div class="admin-nav-item mt-8 border-t border-gray-800 pt-4" onclick="adminLogout()">
                    <svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/><polyline points="16 17 21 12 16 7"/><line x1="21" y1="12" x2="9" y2="12"/></svg>
                    <span>লগআউট</span>
                </div>
            </nav>
        </div>

        <!-- Main Admin Content -->
        <div class="admin-main">
            <!-- Admin Dashboard Content -->
            <div id="adminSection-dashboard">
                <h2 class="text-2xl font-bold font-display mb-6">অ্যাডমিন ড্যাশবোর্ড</h2>
                <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-8">
                    <div class="bg-gray-800 rounded-xl p-5 border border-gray-700">
                        <p class="text-gray-400 text-sm">মোট ইউজার</p>
                        <p class="text-3xl font-bold mt-2" id="adminStatUsers">0</p>
                    </div>
                    <div class="bg-gray-800 rounded-xl p-5 border border-gray-700">
                        <p class="text-gray-400 text-sm">পেন্ডিং উইথড্র</p>
                        <p class="text-3xl font-bold mt-2 text-yellow-500" id="adminStatPending">0</p>
                    </div>
                    <div class="bg-gray-800 rounded-xl p-5 border border-gray-700">
                        <p class="text-gray-400 text-sm">আজকের নতুন</p>
                        <p class="text-3xl font-bold mt-2 text-green-500" id="adminStatToday">0</p>
                    </div>
                    <div class="bg-gray-800 rounded-xl p-5 border border-gray-700">
                        <p class="text-gray-400 text-sm">মোট টাস্ক</p>
                        <p class="text-3xl font-bold mt-2 text-indigo-500" id="adminStatTasks">0</p>
                    </div>
                </div>
            </div>

            <!-- Admin Users Content -->
            <div id="adminSection-users" class="hidden">
                <h2 class="text-2xl font-bold font-display mb-6">ইউজার ম্যানেজমেন্ট</h2>
                <div class="card-admin">
                    <table>
                        <thead>
                            <tr>
                                <th>নাম</th>
                                <th>ফোন</th>
                                <th>ব্যালেন্স</th>
                                <th>অ্যাকশন</th>
                            </tr>
                        </thead>
                        <tbody id="adminUsersTable"></tbody>
                    </table>
                </div>
            </div>

            <!-- Admin Withdraws Content -->
            <div id="adminSection-withdraws" class="hidden">
                <h2 class="text-2xl font-bold font-display mb-6">উইথড্র রিকুয়েস্ট</h2>
                <div class="card-admin">
                    <table>
                        <thead>
                            <tr>
                                <th>ইউজার</th>
                                <th>মেথড</th>
                                <th>একাউন্ট</th>
                                <th>পরিমাণ</th>
                                <th>স্ট্যাটাস</th>
                                <th>অ্যাকশন</th>
                            </tr>
                        </thead>
                        <tbody id="adminWithdrawsTable"></tbody>
                    </table>
                </div>
            </div>

            <!-- Admin Tasks Content -->
            <div id="adminSection-tasks" class="hidden">
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-2xl font-bold font-display">টাস্ক ম্যানেজমেন্ট</h2>
                    <button onclick="openTaskModal()" class="btn-sm btn-edit py-2 px-4 text-base">নতুন টাস্ক যোগ করুন</button>
                </div>
                <div class="card-admin">
                    <table>
                        <thead>
                            <tr>
                                <th>চ্যানেল</th>
                                <th>রিওয়ার্ড</th>
                                <th>অ্যাকশন</th>
                            </tr>
                        </thead>
                        <tbody id="adminTasksTable"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- Modals -->
    <div class="modal" id="userModal">
        <div class="modal-content">
            <h3 class="text-xl font-bold mb-4">ব্যালেন্স পরিবর্তন</h3>
            <input type="hidden" id="editUserId">
            <div class="mb-4">
                <label class="block text-sm text-gray-400 mb-2">নতুন ব্যালেন্স</label>
                <input type="number" id="editUserNewBalance" class="input-field">
            </div>
            <div class="flex gap-3">
                <button onclick="saveUserBalance()" class="btn-approve py-2 px-4 flex-1 rounded-lg">সেভ করুন</button>
                <button onclick="closeModal('userModal')" class="bg-gray-700 py-2 px-4 flex-1 rounded-lg">বাতিল</button>
            </div>
        </div>
    </div>

    <div class="modal" id="taskModal">
        <div class="modal-content">
            <h3 class="text-xl font-bold mb-4">নতুন টাস্ক যোগ করুন</h3>
            <div class="space-y-4">
                <input type="text" id="taskName" class="input-field" placeholder="চ্যানেলের নাম">
                <input type="text" id="taskUsername" class="input-field" placeholder="ইউজারনেম (@ ছাড়া)">
                <input type="number" id="taskReward" class="input-field" placeholder="রিওয়ার্ড (টাকা)">
                <input type="text" id="taskLink" class="input-field" placeholder="লিংক (https://t.me/...)">
            </div>
            <div class="flex gap-3 mt-6">
                <button onclick="saveTask()" class="btn-approve py-2 px-4 flex-1 rounded-lg">যোগ করুন</button>
                <button onclick="closeModal('taskModal')" class="bg-gray-700 py-2 px-4 flex-1 rounded-lg">বাতিল</button>
            </div>
        </div>
    </div>

    <script>
        // ================= CONFIG & STATE =================
        const ADMIN_PASSWORD = "Amitarafat4321"; 
        
        const STORAGE_KEYS = {
            users: 'taskearn_users',
            withdraws: 'taskearn_withdraws',
            tasks: 'taskearn_tasks',
            currentUser: 'taskearn_currentUser',
            adminLoggedIn: 'taskearn_admin_logged_in'
        };

        let currentUser = null;
        let selectedMethod = null;

        // ================= UTILITIES =================
        const $ = id => document.getElementById(id);
        const getData = key => JSON.parse(localStorage.getItem(key)) || [];
        const saveData = (key, data) => localStorage.setItem(key, JSON.stringify(data));

        function showToast(message) {
            $('toastMessage').innerText = message;
            $('toast').classList.add('translate-y-0');
            $('toast').classList.remove('-translate-y-20');
            setTimeout(() => {
                $('toast').classList.remove('translate-y-0');
                $('toast').classList.add('-translate-y-20');
            }, 3000);
        }

        function generateId(prefix) { return prefix + Date.now().toString(36).toUpperCase(); }

        // ================= CORE APP LOGIC =================
        
        function initTasks() {
            if (!localStorage.getItem(STORAGE_KEYS.tasks)) {
                const defaultTasks = [
                    { id: 1, name: 'Tech News BD', username: 'technewsbd', reward: 3, link: 'https://t.me/technewsbd' },
                    { id: 2, name: 'Earn Money Tips', username: 'earnmoneytips', reward: 4, link: 'https://t.me/earnmoneytips' }
                ];
                saveData(STORAGE_KEYS.tasks, defaultTasks);
            }
        }
        
        // --- Navigation ---
        function showScreen(screenId) {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            $(screenId).classList.add('active');
            
            const userScreens = ['dashboardScreen', 'tasksScreen', 'withdrawScreen', 'historyScreen', 'referralScreen'];
            if (userScreens.includes(screenId)) {
                $('bottomNav').classList.remove('hidden');
                updateUserUI();
            } else {
                $('bottomNav').classList.add('hidden');
            }
        }

        function showLogin() { $('loginForm').classList.remove('hidden'); $('signupForm').classList.add('hidden'); }
        function showSignup() { $('loginForm').classList.add('hidden'); $('signupForm').classList.remove('hidden'); }

        // --- Auth ---
        function handleSignup() {
            const name = $('signupName').value.trim();
            const phone = $('signupPhone').value.trim();
            const password = $('signupPassword').value;
            const refCode = $('referralCode').value.trim().toUpperCase();

            if (!name || !phone || !password) return showToast('সব তথ্য পূরণ করুন');
            if (phone.length !== 11) return showToast('সঠিক ১১ ডিজিটের ফোন নম্বর দিন');

            const users = getData(STORAGE_KEYS.users);
            if (users.find(u => u.phone === phone)) return showToast('এই নম্বর দিয়ে ইতিমধ্যে একটি একাউন্ট আছে!');

            const newUser = {
                id: generateId('U'),
                name, phone, password,
                balance: 0, // Start with 0, bonus added if code used
                referralCode: generateId('REF'),
                referredBy: null,
                referralEarnings: 0,
                completedTasks: [],
                registeredAt: new Date().toISOString()
            };

            // Process Referral Code during Signup
            if (refCode) {
                const referrer = users.find(u => u.referralCode === refCode);
                if (referrer) {
                    newUser.referredBy = referrer.id;
                    newUser.balance += 5; // New user gets 5 Tk
                    referrer.balance += 5; // Referrer gets 5 Tk
                    referrer.referralEarnings = (referrer.referralEarnings || 0) + 5;
                    showToast('রেফারেল কোড সফল! ৫ টাকা বোনাস পেয়েছেন');
                } else {
                    showToast('ভুল রেফারেল কোড! কোড ছাড়া একাউন্ট তৈরি হচ্ছে।');
                }
            }

            users.push(newUser);
            saveData(STORAGE_KEYS.users, users);
            
            currentUser = newUser;
            saveData(STORAGE_KEYS.currentUser, newUser);
            if(!refCode) showToast('রেজিস্ট্রেশন সফল!');
            showMainApp();
        }

        function handleLogin() {
            const phone = $('loginPhone').value.trim();
            const password = $('loginPassword').value;
            
            if (!phone || !password) return showToast('ফোন ও পাসওয়ার্ড দিন');
            
            const users = getData(STORAGE_KEYS.users);
            const user = users.find(u => u.phone === phone && u.password === password);
            
            if (!user) return showToast('ফোন নম্বর অথবা পাসওয়ার্ড ভুল!');
            
            currentUser = user;
            saveData(STORAGE_KEYS.currentUser, user);
            showToast('লগইন সফল!');
            showMainApp();
        }

        function logout() {
            currentUser = null;
            localStorage.removeItem(STORAGE_KEYS.currentUser);
            showScreen('authScreen');
            showLogin();
        }

        function showMainApp() {
            showScreen('dashboardScreen');
            loadTasksUser();
        }

        // --- User Dashboard & Tasks ---
        function updateUserUI() {
            if (!currentUser) return;
            const users = getData(STORAGE_KEYS.users);
            const freshUser = users.find(u => u.id === currentUser.id);
            if (freshUser) currentUser = freshUser;
            
            $('dashboardName').innerText = currentUser.name;
            $('totalBalance').innerText = '৳' + currentUser.balance.toFixed(2);
            $('myReferralCode').innerText = currentUser.referralCode;
            $('completedTasks').innerText = currentUser.completedTasks.length;
            
            const withdraws = getData(STORAGE_KEYS.withdraws);
            const myWithdraws = withdraws.filter(w => w.userId === currentUser.id);
            const totalW = myWithdraws.filter(w => w.status === 'approved').reduce((a, b) => a + b.amount, 0);
            const pendingW = myWithdraws.filter(w => w.status === 'pending').length;
            
            $('totalWithdrawn').innerText = '৳' + totalW.toFixed(2);
            $('pendingWithdraw').innerText = pendingW;
            $('withdrawableBalance').innerText = '৳' + currentUser.balance.toFixed(2);
            
            const refs = users.filter(u => u.referredBy === currentUser.id).length;
            $('totalReferrals').innerText = refs;
            $('referralCount').innerText = refs;
            $('referralEarnings').innerText = '৳' + (currentUser.referralEarnings || 0);

            // Handle Redeem Code UI Visibility
            if (currentUser.referredBy) {
                $('redeemInputArea').classList.add('hidden');
                $('redeemStatusArea').classList.remove('hidden');
            } else {
                $('redeemInputArea').classList.remove('hidden');
                $('redeemStatusArea').classList.add('hidden');
            }

            loadHistoryUser();
        }

        // NEW: Redeem Referral Code Function
        function redeemReferralCode() {
            const code = $('redeemCodeInput').value.trim().toUpperCase();
            if (!code) return showToast('অনুগ্রহ করে একটি কোড দিন');

            if (code === currentUser.referralCode) return showToast('আপনি নিজের কোড ব্যবহার করতে পারবেন না!');
            if (currentUser.referredBy) return showToast('আপনি ইতিমধ্যে একটি কোড ব্যবহার করেছেন!');

            const users = getData(STORAGE_KEYS.users);
            const referrer = users.find(u => u.referralCode === code);

            if (!referrer) return showToast('এই কোডটি সঠিক নয়!');

            // Process Referral
            const uIdx = users.findIndex(u => u.id === currentUser.id);
            const rIdx = users.findIndex(u => u.id === referrer.id);

            if (uIdx !== -1 && rIdx !== -1) {
                users[uIdx].balance += 5;
                users[uIdx].referredBy = referrer.id;
                
                users[rIdx].balance += 5;
                users[rIdx].referralEarnings = (users[rIdx].referralEarnings || 0) + 5;

                saveData(STORAGE_KEYS.users, users);
                currentUser = users[uIdx];
                saveData(STORAGE_KEYS.currentUser, currentUser);
                
                showToast('কোড সফল! আপনি ৫ টাকা পেয়েছেন');
                updateUserUI();
            }
        }

        function loadTasksUser() {
            const tasks = getData(STORAGE_KEYS.tasks);
            const dashContainer = $('dashboardTasks');
            const allContainer = $('allTasks');
            
            let html = '';
            tasks.forEach(t => {
                const isDone = currentUser && currentUser.completedTasks.includes(t.id);
                html += `
                    <div class="glass-card p-4 flex items-center gap-4 mb-3">
                        <div class="w-12 h-12 rounded-xl bg-blue-600 flex items-center justify-center flex-shrink-0">
                            <svg width="24" height="24" viewBox="0 0 24 24" fill="white"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm4.64 6.8c-.15 1.58-.8 5.42-1.13 7.19-.14.75-.42 1-.68 1.03-.58.05-1.02-.38-1.58-.75-.88-.58-1.38-.94-2.23-1.5-.99-.65-.35-1.01.22-1.59.15-.15 2.71-2.48 2.76-2.69a.2.2 0 00-.05-.18c-.06-.05-.14-.03-.21-.02-.09.02-1.49.95-4.22 2.79-.4.27-.76.41-1.08.4-.36-.01-1.04-.2-1.55-.37-.63-.2-1.12-.31-1.08-.66.02-.18.27-.36.74-.55 2.92-1.27 4.86-2.11 5.83-2.51 2.78-1.16 3.35-1.36 3.73-1.36.08 0 .27.02.39.12.1.08.13.19.14.27-.01.06.01.24 0 .38z"/></svg>
                        </div>
                        <div class="flex-1">
                            <p class="font-semibold">${t.name}</p>
                            <p class="text-xs text-gray-400">@${t.username}</p>
                        </div>
                        <div class="text-right">
                            <p class="text-amber-500 font-bold">+৳${t.reward}</p>
                            ${isDone ? 
                                '<span class="text-green-500 text-xs block mt-1">সম্পন্ন</span>' : 
                                `<button onclick="completeTask(${t.id}, '${t.link}')" class="btn-secondary text-xs py-1 px-3 mt-1">জয়েন করুন</button>`
                            }
                        </div>
                    </div>
                `;
            });
            
            dashContainer.innerHTML = html.substring(0, html.length * 0.4); 
            allContainer.innerHTML = html;
        }

        function completeTask(taskId, link) {
            if (!currentUser) return;
            window.open(link, '_blank');
            
            const tasks = getData(STORAGE_KEYS.tasks);
            const task = tasks.find(t => t.id === taskId);
            if (!task) return;

            const users = getData(STORAGE_KEYS.users);
            const uIndex = users.findIndex(u => u.id === currentUser.id);
            
            if (users[uIndex].completedTasks.includes(taskId)) return showToast('ইতোমধ্যে সম্পন্ন হয়েছে');

            users[uIndex].completedTasks.push(taskId);
            users[uIndex].balance += task.reward;
            
            saveData(STORAGE_KEYS.users, users);
            currentUser = users[uIndex];
            saveData(STORAGE_KEYS.currentUser, currentUser);
            
            showToast('টাস্ক সম্পন্ন! ৳' + task.reward + ' পেয়েছেন');
            loadTasksUser();
            updateUserUI();
        }

        // --- User Withdraw ---
        function selectMethod(method) {
            selectedMethod = method;
            $('bkashMethod').style.borderColor = method === 'bkash' ? '#f59e0b' : '#2a2a3a';
            $('nagadMethod').style.borderColor = method === 'nagad' ? '#f59e0b' : '#2a2a3a';
        }

        function handleWithdraw() {
            const acc = $('accountNumber').value.trim();
            const amt = parseFloat($('withdrawAmount').value);

            if (!selectedMethod) return showToast('মেথড সিলেক্ট করুন');
            if (!acc || acc.length !== 11) return showToast('সঠিক একাউন্ট নম্বর দিন');
            if (!amt || amt < 50) return showToast('ন্যূনতম ৳৫০ উইথড্র করুন');
            if (amt > currentUser.balance) return showToast('অপর্যাপ্ত ব্যালেন্স');

            const withdraw = {
                id: generateId('W'),
                userId: currentUser.id,
                userName: currentUser.name,
                method: selectedMethod === 'bkash' ? 'বিকাশ' : 'নগদ',
                acc: acc,
                amount: amt,
                status: 'pending',
                date: new Date().toISOString()
            };

            const users = getData(STORAGE_KEYS.users);
            const uIdx = users.findIndex(u => u.id === currentUser.id);
            users[uIdx].balance -= amt;
            saveData(STORAGE_KEYS.users, users);
            currentUser = users[uIdx];
            saveData(STORAGE_KEYS.currentUser, currentUser);

            const withdraws = getData(STORAGE_KEYS.withdraws);
            withdraws.push(withdraw);
            saveData(STORAGE_KEYS.withdraws, withdraws);

            showToast('উইথড্র রিকুয়েস্ট সফল!');
            $('accountNumber').value = '';
            $('withdrawAmount').value = '';
            updateUserUI();
        }

        function loadHistoryUser() {
            const container = $('withdrawHistoryList');
            const withdraws = getData(STORAGE_KEYS.withdraws);
            const myW = withdraws.filter(w => w.userId === currentUser.id).reverse();
            
            if (myW.length === 0) {
                container.innerHTML = '<p class="text-center text-gray-500 py-8">কোনো ইতিহাস নেই</p>';
                return;
            }

            container.innerHTML = myW.map(w => `
                <div class="glass-card p-4 mb-3">
                    <div class="flex justify-between">
                        <div>
                            <p class="font-semibold">${w.method} - ${w.acc}</p>
                            <p class="text-xs text-gray-400">${new Date(w.date).toLocaleDateString('bn-BD')}</p>
                        </div>
                        <div class="text-right">
                            <p class="font-bold text-lg">৳${w.amount}</p>
                            <span class="badge badge-${w.status}">${w.status}</span>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function copyReferralCode() {
            navigator.clipboard.writeText(currentUser.referralCode);
            showToast('কোড কপি হয়েছে');
        }

        // ================= ADMIN LOGIC =================
        
        function handleAdminLogin() {
            const pass = $('adminPasswordInput').value;
            if (pass === ADMIN_PASSWORD) {
                sessionStorage.setItem(STORAGE_KEYS.adminLoggedIn, 'true');
                showScreen('adminPanelScreen');
                loadAdminDashboard();
            } else {
                showToast('ভুল পাসওয়ার্ড!');
            }
        }

        function adminLogout() {
            sessionStorage.removeItem(STORAGE_KEYS.adminLoggedIn);
            showScreen('authScreen');
            showLogin();
        }

        function showAdminSection(sectionName) {
            document.querySelectorAll('[id^="adminSection-"]').forEach(el => el.classList.add('hidden'));
            $('adminSection-' + sectionName).classList.remove('hidden');
            
            document.querySelectorAll('.admin-nav-item').forEach(n => n.classList.remove('active'));
            event.currentTarget.classList.add('active');
            
            if (window.innerWidth <= 768) {
                $('adminSidebar').classList.remove('open');
            }

            if (sectionName === 'dashboard') loadAdminDashboard();
            if (sectionName === 'users') loadAdminUsers();
            if (sectionName === 'withdraws') loadAdminWithdraws();
            if (sectionName === 'tasks') loadAdminTasks();
        }

        function toggleAdminSidebar() {
            $('adminSidebar').classList.toggle('open');
        }

        function loadAdminDashboard() {
            const users = getData(STORAGE_KEYS.users);
            const withdraws = getData(STORAGE_KEYS.withdraws);
            const tasks = getData(STORAGE_KEYS.tasks);
            const today = new Date().toDateString();
            
            $('adminStatUsers').innerText = users.length;
            $('adminStatPending').innerText = withdraws.filter(w => w.status === 'pending').length;
            $('adminStatToday').innerText = users.filter(u => new Date(u.registeredAt).toDateString() === today).length;
            $('adminStatTasks').innerText = tasks.length;
        }

        function loadAdminUsers() {
            const users = getData(STORAGE_KEYS.users);
            $('adminUsersTable').innerHTML = users.map(u => `
                <tr>
                    <td>${u.name}</td>
                    <td>${u.phone}</td>
                    <td class="text-green-400 font-bold">৳${u.balance.toFixed(2)}</td>
                    <td><button onclick="editUser('${u.id}')" class="btn-sm btn-edit">এডিট</button></td>
                </tr>
            `).join('');
        }

        function loadAdminWithdraws() {
            const withdraws = getData(STORAGE_KEYS.withdraws);
            $('adminWithdrawsTable').innerHTML = withdraws.map(w => `
                <tr>
                    <td>${w.userName}<br><span class="text-xs text-gray-500">${w.userId}</span></td>
                    <td>${w.method}</td>
                    <td>${w.acc}</td>
                    <td class="font-bold">৳${w.amount}</td>
                    <td><span class="badge badge-${w.status}">${w.status}</span></td>
                    <td>
                        ${w.status === 'pending' ? 
                            `<button onclick="updateWithdraw('${w.id}', 'approved')" class="btn-sm btn-approve">অনুমোদন</button>
                             <button onclick="updateWithdraw('${w.id}', 'rejected')" class="btn-sm btn-reject ml-1">বাতিল</button>` : 
                            '<span class="text-gray-500 text-xs">-</span>'}
                    </td>
                </tr>
            `).join('');
        }

        function loadAdminTasks() {
            const tasks = getData(STORAGE_KEYS.tasks);
            $('adminTasksTable').innerHTML = tasks.map((t, i) => `
                <tr>
                    <td>${t.name} <span class="text-gray-500">(@${t.username})</span></td>
                    <td class="text-green-400">৳${t.reward}</td>
                    <td><button onclick="deleteTask(${i})" class="btn-sm btn-reject">ডিলিট</button></td>
                </tr>
            `).join('');
        }

        // --- Admin Actions ---
        function editUser(id) {
            $('editUserId').value = id;
            const users = getData(STORAGE_KEYS.users);
            const user = users.find(u => u.id === id);
            $('editUserNewBalance').value = user.balance;
            $('userModal').classList.add('active');
        }

        function saveUserBalance() {
            const id = $('editUserId').value;
            const newBal = parseFloat($('editUserNewBalance').value);
            const users = getData(STORAGE_KEYS.users);
            const idx = users.findIndex(u => u.id === id);
            if (idx !== -1) {
                users[idx].balance = newBal;
                saveData(STORAGE_KEYS.users, users);
                closeModal('userModal');
                loadAdminUsers();
                showToast('ব্যালেন্স আপডেট হয়েছে');
            }
        }

        function updateWithdraw(id, status) {
            const withdraws = getData(STORAGE_KEYS.withdraws);
            const idx = withdraws.findIndex(w => w.id === id);
            
            if (idx !== -1) {
                const w = withdraws[idx];
                
                if (status === 'rejected') {
                    const users = getData(STORAGE_KEYS.users);
                    const uIdx = users.findIndex(u => u.id === w.userId);
                    if (uIdx !== -1) {
                        users[uIdx].balance += w.amount;
                        saveData(STORAGE_KEYS.users, users);
                    }
                }
                
                withdraws[idx].status = status;
                saveData(STORAGE_KEYS.withdraws, withdraws);
                loadAdminWithdraws();
                loadAdminDashboard();
                showToast(`উইথড্র ${status === 'approved' ? 'অনুমোদিত' : 'বাতিল'} হয়েছে`);
            }
        }

        function openTaskModal() { $('taskModal').classList.add('active'); }
        function closeModal(id) { $(id).classList.remove('active'); }

        function saveTask() {
            const name = $('taskName').value;
            const username = $('taskUsername').value;
            const reward = parseFloat($('taskReward').value);
            const link = $('taskLink').value;

            if (!name || !username || !reward || !link) return showToast('সব তথ্য দিন');

            const tasks = getData(STORAGE_KEYS.tasks);
            tasks.push({ id: Date.now(), name, username, reward, link });
            saveData(STORAGE_KEYS.tasks, tasks);
            
            closeModal('taskModal');
            loadAdminTasks();
            showToast('টাস্ক যোগ হয়েছে');
        }

        function deleteTask(index) {
            if(confirm('আপনি কি নিশ্চিত?')) {
                const tasks = getData(STORAGE_KEYS.tasks);
                tasks.splice(index, 1);
                saveData(STORAGE_KEYS.tasks, tasks);
                loadAdminTasks();
            }
        }

        // ================= INITIALIZATION =================
        function init() {
            initTasks();
            
            if (sessionStorage.getItem(STORAGE_KEYS.adminLoggedIn) === 'true') {
                showScreen('adminPanelScreen');
                loadAdminDashboard();
                return;
            }

            const savedUser = localStorage.getItem(STORAGE_KEYS.currentUser);
            if (savedUser) {
                currentUser = JSON.parse(savedUser);
                const users = getData(STORAGE_KEYS.users);
                const freshUser = users.find(u => u.id === currentUser.id);
                if (freshUser) {
                    currentUser = freshUser;
                    showMainApp();
                } else {
                    showScreen('authScreen');
                }
            } else {
                showScreen('authScreen');
            }

            const urlParams = new URLSearchParams(window.location.search);
            const ref = urlParams.get('ref');
            if (ref) {
                $('referralCode').value = ref;
                showSignup();
            }
        }

        init();
    </script>
</body>
</html>
