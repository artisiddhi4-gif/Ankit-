# Ankit-
TuitionPro is the ultimate, easy-to-use digital management app designed specifically for tuition teachers, coaching classes, and home tutors to manage students, monthly fees, and daily attendance effortlessly without any paper hassle.
<!DOCTYPE html>
<html lang="hi" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TuitionPro - Student & Fee Manager</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: { 50: '#eff6ff', 500: '#3b82f6', 600: '#2563eb', 700: '#1d4ed8' }
                    }
                }
            }
        }
    </script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Poppins', sans-serif; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
    </style>
</head>
<body class="h-full bg-slate-100 dark:bg-slate-900 text-slate-800 dark:text-slate-100 flex flex-col justify-between selection:bg-blue-500 selection:text-white">

    <!-- Top Header -->
    <header class="bg-gradient-to-r from-blue-600 to-indigo-700 text-white shadow-md sticky top-0 z-40 px-4 py-3 flex justify-between items-center">
        <div class="flex items-center space-x-2">
            <div class="w-9 h-9 bg-white/20 rounded-xl flex items-center justify-center font-bold text-lg"><i class="fa-solid fa-graduation-cap"></i></div>
            <div>
                <h1 id="headerCoachingName" class="font-bold text-base leading-tight">TuitionPro</h1>
                <p class="text-xs text-blue-100">Fee & Attendance Manager</p>
            </div>
        </div>
        <div class="flex items-center space-x-2">
            <button onclick="openSettingsModal()" class="w-9 h-9 bg-white/10 hover:bg-white/20 rounded-xl flex items-center justify-center transition" title="Settings">
                <i class="fa-solid fa-gear"></i>
            </button>
            <button onclick="toggleDarkMode()" class="w-9 h-9 bg-white/10 hover:bg-white/20 rounded-xl flex items-center justify-center transition" title="Theme">
                <i id="themeIcon" class="fa-solid fa-moon"></i>
            </button>
        </div>
    </header>

    <!-- Main Container Area -->
    <main class="flex-1 overflow-y-auto pb-24 max-w-md w-full mx-auto px-4 pt-4">

        <!-- TAB 1: STUDENTS MANAGEMENT -->
        <div id="tabStudents" class="tab-content active space-y-4">
            
            <!-- Quick Stats & Actions Card -->
            <div class="bg-white dark:bg-slate-800 p-4 rounded-2xl shadow-sm border border-slate-200/80 dark:border-slate-700 grid grid-cols-2 gap-3">
                <div class="bg-blue-50 dark:bg-slate-700/50 p-3 rounded-xl border border-blue-100 dark:border-slate-700">
                    <span class="text-xs text-slate-500 dark:text-slate-400 font-medium block">Total Students</span>
                    <span id="statTotal" class="text-2xl font-bold text-blue-600 dark:text-blue-400">0</span>
                </div>
                <div class="bg-amber-50 dark:bg-slate-700/50 p-3 rounded-xl border border-amber-100 dark:border-slate-700">
                    <span class="text-xs text-slate-500 dark:text-slate-400 font-medium block">Pending Fees</span>
                    <span id="statPending" class="text-2xl font-bold text-amber-600 dark:text-amber-400">₹0</span>
                </div>
            </div>

            <!-- Action Bar -->
            <div class="flex gap-2">
                <button onclick="openAddStudentModal()" class="flex-1 bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 px-4 rounded-xl shadow-md shadow-blue-500/20 flex items-center justify-center gap-2 transition active:scale-95">
                    <i class="fa-solid fa-user-plus"></i> Add Student
                </button>
                <button onclick="openBroadcastModal()" class="bg-emerald-600 hover:bg-emerald-700 text-white font-semibold py-3 px-4 rounded-xl shadow-md shadow-emerald-500/20 flex items-center justify-center gap-2 transition active:scale-95" title="Send Bulk Reminders">
                    <i class="fa-brands fa-whatsapp text-lg"></i> Remind All
                </button>
            </div>

            <!-- Search & Filter Bar -->
            <div class="flex gap-2">
                <div class="relative flex-1">
                    <i class="fa-solid fa-search absolute left-3.5 top-3.5 text-slate-400 text-sm"></i>
                    <input type="text" id="searchInput" oninput="renderStudents()" placeholder="Search name or mobile..." class="w-full bg-white dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-xl pl-10 pr-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <select id="filterClass" onchange="renderStudents()" class="bg-white dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-xl px-3 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500 font-medium">
                    <option value="">All Classes</option>
                </select>
            </div>

            <!-- Students List -->
            <div id="studentsListContainer" class="space-y-3">
                <!-- Dynamically Inserted Students -->
            </div>
        </div>

        <!-- TAB 2: DAILY ATTENDANCE -->
        <div id="tabAttendance" class="tab-content space-y-4">
            <div class="bg-white dark:bg-slate-800 p-4 rounded-2xl shadow-sm border border-slate-200 dark:border-slate-700 flex justify-between items-center">
                <div>
                    <h2 class="font-bold text-base">Daily Attendance</h2>
                    <p id="currentAttendanceDate" class="text-xs text-slate-500 dark:text-slate-400">Mark today's presence</p>
                </div>
                <input type="date" id="attendanceDatePicker" onchange="loadAttendanceForDate()" class="bg-slate-100 dark:bg-slate-700 border-none rounded-xl px-3 py-2 text-sm font-medium focus:outline-none">
            </div>

            <div class="flex gap-2">
                <button onclick="markAllAttendance('Present')" class="flex-1 bg-emerald-50 dark:bg-emerald-950/40 text-emerald-600 dark:text-emerald-400 border border-emerald-200 dark:border-emerald-900 py-2.5 rounded-xl font-semibold text-sm transition">Mark All Present</button>
                <button onclick="sendAbsentAlerts()" class="flex-1 bg-rose-50 dark:bg-rose-950/40 text-rose-600 dark:text-rose-400 border border-rose-200 dark:border-rose-900 py-2.5 rounded-xl font-semibold text-sm transition"><i class="fa-brands fa-whatsapp"></i> Absent Alerts</button>
            </div>

            <div id="attendanceListContainer" class="space-y-2.5">
                <!-- Attendance Rows -->
            </div>
        </div>

        <!-- TAB 3: REPORTS & TOOLS -->
        <div id="tabReports" class="tab-content space-y-4">
            <div class="bg-white dark:bg-slate-800 p-5 rounded-2xl shadow-sm border border-slate-200 dark:border-slate-700 space-y-4">
                <h3 class="font-bold text-base border-b border-slate-100 dark:border-slate-700 pb-3 flex items-center gap-2">
                    <i class="fa-solid fa-chart-pie text-blue-500"></i> Fee Collection Summary
                </h3>
                <div class="space-y-2 text-sm">
                    <div class="flex justify-between py-1 border-b border-slate-100 dark:border-slate-700/50">
                        <span class="text-slate-500 dark:text-slate-400">Total Expected Collection</span>
                        <span id="repTotalExpected" class="font-bold">₹0</span>
                    </div>
                    <div class="flex justify-between py-1 border-b border-slate-100 dark:border-slate-700/50">
                        <span class="text-slate-500 dark:text-slate-400">Total Collected (Paid)</span>
                        <span id="repTotalCollected" class="font-bold text-emerald-600">₹0</span>
                    </div>
                    <div class="flex justify-between py-1">
                        <span class="text-slate-500 dark:text-slate-400">Total Pending Balance</span>
                        <span id="repTotalPending" class="font-bold text-rose-600">₹0</span>
                    </div>
                </div>
            </div>

            <!-- Management Tools -->
            <div class="bg-white dark:bg-slate-800 p-5 rounded-2xl shadow-sm border border-slate-200 dark:border-slate-700 space-y-3">
                <h3 class="font-bold text-base border-b border-slate-100 dark:border-slate-700 pb-3 flex items-center gap-2">
                    <i class="fa-solid fa-tools text-blue-500"></i> Data & Monthly Actions
                </h3>
                
                <button onclick="confirmNewMonthReset()" class="w-full bg-amber-500 hover:bg-amber-600 text-white font-semibold py-3 px-4 rounded-xl shadow-sm flex items-center justify-center gap-2 transition text-sm">
                    <i class="fa-solid fa-rotate-right"></i> New Month Reset (All to Pending)
                </button>

                <div class="grid grid-cols-2 gap-2 pt-2">
                    <button onclick="exportToCSV()" class="bg-slate-100 dark:bg-slate-700 hover:bg-slate-200 dark:hover:bg-slate-600 font-semibold py-2.5 px-3 rounded-xl flex items-center justify-center gap-2 text-xs transition">
                        <i class="fa-solid fa-file-excel text-emerald-600"></i> Export CSV
                    </button>
                    <button onclick="openBackupModal()" class="bg-slate-100 dark:bg-slate-700 hover:bg-slate-200 dark:hover:bg-slate-600 font-semibold py-2.5 px-3 rounded-xl flex items-center justify-center gap-2 text-xs transition">
                        <i class="fa-solid fa-database text-blue-600"></i> Backup & Restore
                    </button>
                </div>

                <button onclick="openPrivacyModal()" class="w-full text-center text-xs text-slate-400 hover:text-blue-500 pt-2 transition">
                    Privacy Policy & Play Store Info
                </button>
            </div>
        </div>

    </main>

    <!-- Bottom Navigation Bar -->
    <nav class="fixed bottom-0 left-0 right-0 max-w-md mx-auto bg-white dark:bg-slate-800 border-t border-slate-200 dark:border-slate-700 py-2 px-6 flex justify-around items-center z-40 shadow-lg">
        <button onclick="switchTab('students')" id="navStudents" class="flex flex-col items-center text-blue-600 dark:text-blue-400 transition">
            <i class="fa-solid fa-user-graduate text-lg"></i>
            <span class="text-[10px] font-semibold mt-1">Students</span>
        </button>
        <button onclick="switchTab('attendance')" id="navAttendance" class="flex flex-col items-center text-slate-400 hover:text-slate-600 dark:hover:text-slate-200 transition">
            <i class="fa-solid fa-clipboard-user text-lg"></i>
            <span class="text-[10px] font-semibold mt-1">Attendance</span>
        </button>
        <button onclick="switchTab('reports')" id="navReports" class="flex flex-col items-center text-slate-400 hover:text-slate-600 dark:hover:text-slate-200 transition">
            <i class="fa-solid fa-chart-line text-lg"></i>
            <span class="text-[10px] font-semibold mt-1">Reports</span>
        </button>
    </nav>

    <!-- MODALS -->

    <!-- Add/Edit Student Modal -->
    <div id="studentModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-end sm:items-center justify-center p-0 sm:p-4">
        <div class="bg-white dark:bg-slate-800 w-full max-w-md rounded-t-3xl sm:rounded-2xl p-6 shadow-2xl max-h-[90vh] overflow-y-auto">
            <div class="flex justify-between items-center mb-4">
                <h3 id="modalTitle" class="font-bold text-lg">Add New Student</h3>
                <button onclick="closeStudentModal()" class="w-8 h-8 rounded-full bg-slate-100 dark:bg-slate-700 flex items-center justify-center text-slate-500"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <form id="studentForm" onsubmit="saveStudent(event)" class="space-y-4">
                <input type="hidden" id="studentId">
                <div>
                    <label class="block text-xs font-semibold text-slate-500 dark:text-slate-400 uppercase mb-1">Student Full Name</label>
                    <input type="text" id="studentName" required placeholder="e.g. Rahul Sharma" class="w-full bg-slate-50 dark:bg-slate-700/50 border border-slate-200 dark:border-slate-700 rounded-xl px-4 py-3 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs font-semibold text-slate-500 dark:text-slate-400 uppercase mb-1">Class / Standard</label>
                        <input type="text" id="studentClass" required placeholder="e.g. 10th Math" class="w-full bg-slate-50 dark:bg-slate-700/50 border border-slate-200 dark:border-slate-700 rounded-xl px-4 py-3 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-500 dark:text-slate-400 uppercase mb-1">Monthly Fee (₹)</label>
                        <input type="number" id="studentFee" required placeholder="500" class="w-full bg-slate-50 dark:bg-slate-700/50 border border-slate-200 dark:border-slate-700 rounded-xl px-4 py-3 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-500 dark:text-slate-400 uppercase mb-1">Parent's WhatsApp Mobile No.</label>
                    <input type="tel" id="studentMobile" required placeholder="9876543210" pattern="[0-9]{10}" class="w-full bg-slate-50 dark:bg-slate-700/50 border border-slate-200 dark:border-slate-700 rounded-xl px-4 py-3 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <button type="submit" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3.5 rounded-xl shadow-lg shadow-blue-500/25 transition">Save Student</button>
            </form>
        </div>
    </div>

    <!-- Fee Receipt Modal -->
    <div id="receiptModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white dark:bg-slate-800 w-full max-w-sm rounded-2xl p-6 shadow-2xl text-center space-y-4">
            <div id="receiptCardContent" class="bg-slate-50 dark:bg-slate-700/40 p-4 rounded-xl border border-slate-200 dark:border-slate-700 text-left space-y-2">
                <div class="text-center pb-2 border-b border-dashed border-slate-300 dark:border-slate-600">
                    <h4 id="rcptCoachingName" class="font-bold text-base">TuitionPro</h4>
                    <p class="text-[10px] text-slate-500">OFFICIAL FEE RECEIPT</p>
                </div>
                <div class="text-xs space-y-1">
                    <p><strong>Receipt No:</strong> <span id="rcptNo">#REC-101</span></p>
                    <p><strong>Date:</strong> <span id="rcptDate">17/11/2026</span></p>
                    <p><strong>Student Name:</strong> <span id="rcptName">Rahul Sharma</span></p>
                    <p><strong>Class:</strong> <span id="rcptClass">10th</span></p>
                    <p class="pt-2 border-t border-slate-200 dark:border-slate-600 flex justify-between font-bold text-sm">
                        <span>Amount Paid:</span> <span id="rcptAmount" class="text-emerald-600">₹500</span>
                    </p>
                </div>
            </div>
            <div class="flex gap-2">
                <button onclick="shareReceiptWhatsApp()" class="flex-1 bg-emerald-600 hover:bg-emerald-700 text-white font-semibold py-2.5 rounded-xl text-sm flex items-center justify-center gap-2"><i class="fa-brands fa-whatsapp"></i> Send Receipt</button>
                <button onclick="closeReceiptModal()" class="bg-slate-200 dark:bg-slate-700 font-semibold px-4 py-2.5 rounded-xl text-sm">Close</button>
            </div>
        </div>
    </div>

    <!-- Settings Modal -->
    <div id="settingsModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-end sm:items-center justify-center p-0 sm:p-4">
        <div class="bg-white dark:bg-slate-800 w-full max-w-md rounded-t-3xl sm:rounded-2xl p-6 shadow-2xl space-y-4">
            <div class="flex justify-between items-center mb-2">
                <h3 class="font-bold text-lg">Coaching & UPI Settings</h3>
                <button onclick="closeSettingsModal()" class="w-8 h-8 rounded-full bg-slate-100 dark:bg-slate-700 flex items-center justify-center text-slate-500"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <div class="space-y-3">
                <div>
                    <label class="block text-xs font-semibold text-slate-500 dark:text-slate-400 uppercase mb-1">Coaching / Institute Name</label>
                    <input type="text" id="settingCoachingName" placeholder="e.g. Apex Mathematics Classes" class="w-full bg-slate-50 dark:bg-slate-700/50 border border-slate-200 dark:border-slate-700 rounded-xl px-4 py-3 text-sm focus:outline-none">
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-500 dark:text-slate-400 uppercase mb-1">Teacher Name</label>
                    <input type="text" id="settingTeacherName" placeholder="e.g. Ankit Sir" class="w-full bg-slate-50 dark:bg-slate-700/50 border border-slate-200 dark:border-slate-700 rounded-xl px-4 py-3 text-sm focus:outline-none">
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-500 dark:text-slate-400 uppercase mb-1">UPI ID (For Online Fee Payment)</label>
                    <input type="text" id="settingUpiId" placeholder="e.g. 9876543210@paytm" class="w-full bg-slate-50 dark:bg-slate-700/50 border border-slate-200 dark:border-slate-700 rounded-xl px-4 py-3 text-sm focus:outline-none">
                </div>
                <button onclick="saveSettings()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3.5 rounded-xl shadow-lg transition">Save Settings</button>
            </div>
        </div>
    </div>

    <!-- Backup & Restore Modal -->
    <div id="backupModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white dark:bg-slate-800 w-full max-w-sm rounded-2xl p-6 shadow-2xl space-y-4">
            <h3 class="font-bold text-base">Backup & Restore Data</h3>
            <p class="text-xs text-slate-500">Export your data to JSON or restore it on another phone.</p>
            <button onclick="exportJSONBackup()" class="w-full bg-blue-600 text-white font-semibold py-2.5 rounded-xl text-sm"><i class="fa-solid fa-download"></i> Download Backup File</button>
            <div>
                <label class="block text-xs font-semibold text-slate-500 mb-1">Restore from Backup</label>
                <input type="file" id="importFile" accept=".json" onchange="importJSONBackup(event)" class="w-full text-xs text-slate-500 file:mr-4 file:py-2 file:px-4 file:rounded-xl file:border-0 file:text-xs file:font-semibold file:bg-blue-50 file:text-blue-700 hover:file:bg-blue-100">
            </div>
            <button onclick="closeBackupModal()" class="w-full bg-slate-200 dark:bg-slate-700 font-semibold py-2.5 rounded-xl text-sm">Close</button>
        </div>
  
