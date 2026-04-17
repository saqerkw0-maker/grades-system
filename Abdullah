# grades-system
```html
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام إدارة الدرجات الاحترافي</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @media print {
            body { background-color: white !important; }
            .no-print { display: none !important; }
            .print-only { display: block !important; }
            .container { max-width: 100% !important; padding: 0 !important; margin: 0 !important; }
            .shadow-sm, .shadow-md, .shadow-lg, .shadow-xl { box-shadow: none !important; }
            .border { border-color: #ddd !important; }
            input { border: none !important; background: transparent !important; pointer-events: none; color: black !important; }
            .break-inside-avoid { break-inside: avoid; }
            /* تحسين طباعة القوائم */
            .badge-print { border: 1px solid #000; padding: 2px 6px; border-radius: 4px; }
        }
        
        body.print-preview-mode { background-color: white; }
        body.print-preview-mode .no-print { display: none !important; }
        body.print-preview-mode .print-only { display: block !important; }
        body.print-preview-mode .shadow-sm, body.print-preview-mode .shadow-md { box-shadow: none !important; }
        body.print-preview-mode input { border: none !important; background: transparent !important; pointer-events: none; }
        
        .print-only { display: none; }
        
        input[type="number"]::-webkit-inner-spin-button, 
        input[type="number"]::-webkit-outer-spin-button { -webkit-appearance: none; margin: 0; }
        .transition-width { transition: width 0.5s ease-in-out; }
        
        .modal-overlay {
            position: fixed; top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(15, 23, 42, 0.7); display: none; justify-content: center; align-items: center; z-index: 100;
            backdrop-filter: blur(4px);
        }
        .modal-overlay.active { display: flex; }
        
        /* تأثيرات إضافية */
        .hover-lift { transition: transform 0.2s ease, box-shadow 0.2s ease; }
        .hover-lift:hover { transform: translateY(-2px); box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1); }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 font-sans min-h-screen relative">

    <!-- الشريط العلوي -->
    <nav class="bg-indigo-700 text-white p-4 shadow-md no-print sticky top-0 z-40">
        <div class="container mx-auto flex justify-between items-center">
            <div class="flex items-center gap-3 text-xl font-bold">
                <i class="fa-solid fa-graduation-cap text-3xl"></i>
                <span>المنصة الذكية للدرجات</span>
            </div>
            <div class="flex gap-3">
                <button onclick="openModal('settingsModal')" class="flex items-center justify-center w-10 h-10 bg-indigo-600 hover:bg-indigo-500 rounded-full transition shadow" title="إعدادات الأعمدة">
                    <i class="fa-solid fa-gear"></i>
                </button>
                <button onclick="handlePrint()" class="flex items-center gap-2 bg-indigo-600 hover:bg-indigo-500 px-4 py-2 rounded-lg transition shadow">
                    <i class="fa-solid fa-print"></i>
                    <span class="hidden sm:inline">طباعة التقرير</span>
                </button>
            </div>
        </div>
    </nav>

    <!-- شريط تنبيه معاينة الطباعة -->
    <div id="printPreviewAlert" class="hidden bg-amber-100 border-b-4 border-amber-500 text-amber-800 p-4 sticky top-0 z-50 flex justify-between items-center shadow-md">
        <div>
            <i class="fa-solid fa-circle-info mr-2"></i>
            <strong>وضع الطباعة نشط:</strong> قم بالضغط على (Ctrl+P) لطباعة التقرير أو حفظه كـ PDF.
        </div>
        <div class="flex gap-2">
            <button onclick="window.print()" class="bg-indigo-600 text-white px-4 py-1.5 rounded-md hover:bg-indigo-700 shadow-sm"><i class="fa-solid fa-print ml-1"></i> طباعة</button>
            <button onclick="exitPrintPreview()" class="bg-white text-slate-700 border border-slate-300 px-4 py-1.5 rounded-md hover:bg-slate-50 shadow-sm">إغلاق المعاينة</button>
        </div>
    </div>

    <div class="container mx-auto p-4 flex flex-col lg:flex-row gap-6 mt-4" id="mainContent">
        
        <!-- القائمة الجانبية -->
        <aside class="w-full lg:w-72 shrink-0 no-print">
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-5 sticky top-24">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-lg font-bold flex items-center gap-2 text-slate-700">
                        <i class="fa-solid fa-school text-indigo-500"></i> الفصول الدراسية
                    </h3>
                    <button onclick="openModal('addClassModal')" class="text-indigo-600 hover:text-indigo-800 bg-indigo-50 hover:bg-indigo-100 w-8 h-8 flex items-center justify-center rounded-full transition" title="إضافة فصل جديد">
                        <i class="fa-solid fa-plus"></i>
                    </button>
                </div>
                
                <div class="flex flex-col gap-3 max-h-[60vh] overflow-y-auto pr-1" id="classesList">
                    <!-- سيتم إضافة الفصول هنا -->
                </div>
            </div>
        </aside>

        <!-- المحتوى الرئيسي -->
        <main class="flex-1 flex flex-col gap-6 w-full max-w-full">
            
            <!-- ترويسة الطباعة -->
            <div class="print-only text-center mb-10 border-b-2 border-slate-800 pb-6">
                <h1 class="text-3xl font-bold mb-3 text-slate-900">التقرير الختامي لأداء الطلاب</h1>
                <h2 class="text-xl text-slate-600 font-bold" id="printClassName">الفصل: </h2>
            </div>

            <!-- بطاقات الإحصائيات -->
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                <div class="bg-white rounded-2xl p-5 shadow-sm border border-slate-100 flex flex-col sm:flex-row items-center sm:items-start text-center sm:text-right gap-4 hover-lift">
                    <div class="bg-indigo-50 p-4 rounded-xl text-indigo-600"><i class="fa-solid fa-users text-2xl"></i></div>
                    <div>
                        <p class="text-sm text-slate-500 font-medium">الطلاب</p>
                        <p class="text-2xl font-black text-slate-800 mt-1" id="statTotalStudents">0</p>
                    </div>
                </div>
                <div class="bg-white rounded-2xl p-5 shadow-sm border border-slate-100 flex flex-col sm:flex-row items-center sm:items-start text-center sm:text-right gap-4 hover-lift">
                    <div class="bg-emerald-50 p-4 rounded-xl text-emerald-600"><i class="fa-solid fa-chart-pie text-2xl"></i></div>
                    <div>
                        <p class="text-sm text-slate-500 font-medium">المتوسط</p>
                        <p class="text-2xl font-black text-slate-800 mt-1"><span id="statAverage">0</span> <span class="text-xs text-slate-400 font-normal" id="statMaxTotal">/ 100</span></p>
                    </div>
                </div>
                <div class="bg-white rounded-2xl p-5 shadow-sm border border-slate-100 flex flex-col sm:flex-row items-center sm:items-start text-center sm:text-right gap-4 hover-lift">
                    <div class="bg-amber-50 p-4 rounded-xl text-amber-500"><i class="fa-solid fa-medal text-2xl"></i></div>
                    <div>
                        <p class="text-sm text-slate-500 font-medium">المتفوقين</p>
                        <p class="text-2xl font-black text-amber-600 mt-1" id="statTopStudents">0</p>
                    </div>
                </div>
                <div class="bg-white rounded-2xl p-5 shadow-sm border border-slate-100 flex flex-col sm:flex-row items-center sm:items-start text-center sm:text-right gap-4 hover-lift">
                    <div class="bg-rose-50 p-4 rounded-xl text-rose-500"><i class="fa-solid fa-triangle-exclamation text-2xl"></i></div>
                    <div>
                        <p class="text-sm text-slate-500 font-medium">المتعثرين</p>
                        <p class="text-2xl font-black text-rose-600 mt-1" id="statStrugglingStudents">0</p>
                    </div>
                </div>
            </div>

            <!-- جدول الدرجات -->
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden w-full">
                <div class="p-5 bg-slate-50 border-b border-slate-200 flex flex-col sm:flex-row justify-between items-center gap-4 no-print">
                    <h2 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                        <i class="fa-solid fa-table-list text-indigo-500"></i> <span id="tableTitle">سجل الدرجات</span>
                    </h2>
                    <button onclick="addStudent()" class="flex items-center gap-2 bg-emerald-600 hover:bg-emerald-700 text-white px-5 py-2.5 rounded-xl text-sm font-bold transition shadow-sm w-full sm:w-auto justify-center">
                        <i class="fa-solid fa-user-plus"></i> إدراج طالب جديد
                    </button>
                </div>
                <div class="overflow-x-auto w-full">
                    <table class="w-full text-right border-collapse min-w-[700px]">
                        <thead>
                            <tr class="bg-slate-100 text-slate-700 text-sm border-b border-slate-200">
                                <th class="p-4 w-12 text-center font-bold">#</th>
                                <th class="p-4 font-bold">اسم الطالب</th>
                                <th class="p-4 w-36 text-center font-bold" id="thCol1">العمود الأول</th>
                                <th class="p-4 w-36 text-center font-bold" id="thCol2">العمود الثاني</th>
                                <th class="p-4 w-36 text-center font-bold bg-slate-200/50">المجموع (<span id="thTotalMax">100</span>)</th>
                                <th class="p-4 w-20 text-center no-print">إجراء</th>
                            </tr>
                        </thead>
                        <tbody id="studentsTableBody">
                            <!-- صفوف الطلاب -->
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- قسم الرسوم البيانية والقوائم المتطورة -->
            <div class="grid grid-cols-1 xl:grid-cols-2 gap-6 break-inside-avoid print:mt-8">
                
                <!-- الإنفوجرافيك -->
                <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-6 w-full">
                    <h3 class="text-lg font-bold text-slate-800 mb-6 flex items-center gap-2">
                        <i class="fa-solid fa-chart-column text-indigo-500"></i> مؤشر الأداء وتوزيع التقديرات
                    </h3>
                    <div class="flex flex-col gap-5 mt-4" id="infographicContainer"></div>
                </div>

                <!-- قوائم الشرف والتعثر المنسقة -->
                <div class="grid grid-cols-1 gap-6 w-full">
                    <!-- المتفوقين -->
                    <div class="bg-gradient-to-br from-white to-amber-50/30 rounded-2xl shadow-sm border border-amber-200 p-6 relative overflow-hidden">
                        <div class="absolute -right-4 -top-4 text-amber-100 opacity-50"><i class="fa-solid fa-award text-9xl"></i></div>
                        <h3 class="text-amber-800 font-bold mb-4 flex items-center gap-2 border-b border-amber-200 pb-3 relative z-10">
                            <i class="fa-solid fa-crown text-xl"></i> لوحة الشرف الأكاديمي
                        </h3>
                        <div class="space-y-3 relative z-10" id="topStudentsList"></div>
                    </div>

                    <!-- المتعثرين -->
                    <div class="bg-gradient-to-br from-white to-rose-50/30 rounded-2xl shadow-sm border border-rose-200 p-6 relative overflow-hidden">
                        <div class="absolute -left-4 -top-4 text-rose-100 opacity-50"><i class="fa-solid fa-life-ring text-9xl"></i></div>
                        <h3 class="text-rose-800 font-bold mb-4 flex items-center gap-2 border-b border-rose-200 pb-3 relative z-10">
                            <i class="fa-solid fa-hand-holding-medical text-xl"></i> الخطة العلاجية للمتعثرين
                        </h3>
                        <div class="space-y-3 relative z-10" id="strugglingStudentsList"></div>
                    </div>
                </div>
            </div>
            
        </main>
    </div>

    <!-- Modals (النوافذ المنبثقة) -->
    
    <!-- Modal: إعدادات الأعمدة -->
    <div id="settingsModal" class="modal-overlay">
        <div class="bg-white p-6 rounded-2xl shadow-2xl w-[500px] max-w-[95%]">
            <h3 class="text-xl font-bold mb-5 flex items-center gap-2 text-indigo-800 border-b pb-3">
                <i class="fa-solid fa-sliders"></i> إعدادات الدرجات
            </h3>
            
            <div class="space-y-6">
                <!-- إعداد العمود الأول -->
                <div class="bg-slate-50 p-4 rounded-xl border border-slate-200">
                    <h4 class="font-bold text-slate-700 mb-3"><i class="fa-solid fa-1 text-slate-400"></i> العمود الأول</h4>
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block text-sm text-slate-600 mb-1">اسم العمود</label>
                            <input type="text" id="setCol1Name" class="w-full border border-slate-300 rounded-lg p-2 focus:border-indigo-500 outline-none">
                        </div>
                        <div>
                            <label class="block text-sm text-slate-600 mb-1">الدرجة العظمى</label>
                            <input type="number" id="setCol1Max" min="1" class="w-full border border-slate-300 rounded-lg p-2 focus:border-indigo-500 outline-none">
                        </div>
                    </div>
                </div>

                <!-- إعداد العمود الثاني -->
                <div class="bg-slate-50 p-4 rounded-xl border border-slate-200">
                    <h4 class="font-bold text-slate-700 mb-3"><i class="fa-solid fa-2 text-slate-400"></i> العمود الثاني</h4>
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block text-sm text-slate-600 mb-1">اسم العمود</label>
                            <input type="text" id="setCol2Name" class="w-full border border-slate-300 rounded-lg p-2 focus:border-indigo-500 outline-none">
                        </div>
                        <div>
                            <label class="block text-sm text-slate-600 mb-1">الدرجة العظمى</label>
                            <input type="number" id="setCol2Max" min="1" class="w-full border border-slate-300 rounded-lg p-2 focus:border-indigo-500 outline-none">
                        </div>
                    </div>
                </div>
            </div>

            <div class="mt-6 flex justify-end gap-3 border-t pt-4">
                <button onclick="closeModal('settingsModal')" class="px-5 py-2 bg-slate-200 text-slate-700 rounded-xl hover:bg-slate-300 font-medium transition">إلغاء</button>
                <button onclick="saveSettings()" class="px-5 py-2 bg-indigo-600 text-white rounded-xl hover:bg-indigo-700 font-bold transition shadow-md">حفظ الإعدادات</button>
            </div>
        </div>
    </div>

    <!-- Modal: إضافة فصل -->
    <div id="addClassModal" class="modal-overlay">
        <div class="bg-white p-6 rounded-2xl shadow-2xl w-96 max-w-[90%] transform transition-all">
            <h3 class="text-xl font-bold mb-4 text-slate-800">إضافة فصل دراسي جديد</h3>
            <input type="text" id="newClassNameInput" class="w-full border-2 border-slate-200 rounded-xl p-3 mb-5 focus:border-indigo-500 outline-none" placeholder="مثال: الصف الأول (أ)">
            <div class="flex justify-end gap-3">
                <button onclick="closeModal('addClassModal')" class="px-4 py-2 bg-slate-100 text-slate-600 rounded-lg hover:bg-slate-200 transition font-medium">إلغاء</button>
                <button onclick="confirmAddClass()" class="px-4 py-2 bg-indigo-600 text-white rounded-lg hover:bg-indigo-700 transition shadow font-bold">إضافة الفصل</button>
            </div>
        </div>
    </div>

    <!-- Modal: تعديل فصل -->
    <div id="editClassModal" class="modal-overlay">
        <div class="bg-white p-6 rounded-2xl shadow-2xl w-96 max-w-[90%]">
            <h3 class="text-xl font-bold mb-4 text-slate-800">تعديل اسم الفصل</h3>
            <input type="hidden" id="editClassOldName">
            <input type="text" id="editClassNameInput" class="w-full border-2 border-slate-200 rounded-xl p-3 mb-5 focus:border-indigo-500 outline-none">
            <div class="flex justify-end gap-3">
                <button onclick="closeModal('editClassModal')" class="px-4 py-2 bg-slate-100 text-slate-600 rounded-lg hover:bg-slate-200 transition font-medium">إلغاء</button>
                <button onclick="confirmEditClass()" class="px-4 py-2 bg-indigo-600 text-white rounded-lg hover:bg-indigo-700 transition shadow font-bold">حفظ التعديلات</button>
            </div>
        </div>
    </div>

    <!-- Modal: تأكيد الحذف -->
    <div id="confirmDeleteModal" class="modal-overlay">
        <div class="bg-white p-6 rounded-2xl shadow-2xl w-96 max-w-[90%] border-t-4 border-rose-500">
            <h3 class="text-xl font-bold mb-2 text-rose-600 flex items-center gap-2">
                <i class="fa-solid fa-triangle-exclamation"></i> تأكيد الحذف
            </h3>
            <p class="text-slate-600 mb-6 font-medium" id="deleteConfirmMessage">هل أنت متأكد؟</p>
            <input type="hidden" id="deleteTargetType">
            <input type="hidden" id="deleteTargetId">
            <div class="flex justify-end gap-3">
                <button onclick="closeModal('confirmDeleteModal')" class="px-4 py-2 bg-slate-100 text-slate-600 rounded-lg hover:bg-slate-200 transition font-medium">تراجع</button>
                <button onclick="executeDelete()" class="px-4 py-2 bg-rose-600 text-white rounded-lg hover:bg-rose-700 transition shadow font-bold">نعم، تأكيد الحذف</button>
            </div>
        </div>
    </div>

    <!-- البرمجة -->
    <script>
        // إعدادات الدرجات (يمكن تغييرها)
        let gradeSettings = {
            col1: { name: 'أعمال السنة', max: 50 },
            col2: { name: 'الاختبار', max: 50 }
        };

        // البيانات
        let classesData = ['الصف الأول', 'الصف الثاني'];
        let activeClass = 'الصف الأول';
        
        let studentsData = [
            { id: 1, name: 'أحمد محمود', classwork: 45, quiz: 48, className: 'الصف الأول' },
            { id: 2, name: 'سالم عبدالله', classwork: 50, quiz: 50, className: 'الصف الأول' },
            { id: 3, name: 'عمر زيد', classwork: 38, quiz: 40, className: 'الصف الأول' },
            { id: 4, name: 'يوسف طارق', classwork: 15, quiz: 20, className: 'الصف الأول' },
            { id: 5, name: 'فهد عبدالعزيز', classwork: 10, quiz: 10, className: 'الصف الثاني' },
            { id: 6, name: 'خالد وليد', classwork: 48, quiz: 49, className: 'الصف الثاني' },
        ];

        function init() {
            if (classesData.length === 0) {
                classesData = ['فصل جديد'];
                activeClass = 'فصل جديد';
            }
            updateHeaders();
            renderClassMenu();
            updateUI();
        }

        // --- إعدادات الأعمدة ---
        function updateHeaders() {
            document.getElementById('thCol1').innerText = `${gradeSettings.col1.name} (${gradeSettings.col1.max})`;
            document.getElementById('thCol2').innerText = `${gradeSettings.col2.name} (${gradeSettings.col2.max})`;
            const totalMax = gradeSettings.col1.max + gradeSettings.col2.max;
            document.getElementById('thTotalMax').innerText = totalMax;
            document.getElementById('statMaxTotal').innerText = `/ ${totalMax}`;
        }

        function openModal(id) {
            document.getElementById(id).classList.add('active');
            if(id === 'addClassModal') {
                document.getElementById('newClassNameInput').value = '';
                document.getElementById('newClassNameInput').focus();
            } else if (id === 'settingsModal') {
                document.getElementById('setCol1Name').value = gradeSettings.col1.name;
                document.getElementById('setCol1Max').value = gradeSettings.col1.max;
                document.getElementById('setCol2Name').value = gradeSettings.col2.name;
                document.getElementById('setCol2Max').value = gradeSettings.col2.max;
            }
        }

        function closeModal(id) {
            document.getElementById(id).classList.remove('active');
        }

        function saveSettings() {
            const c1n = document.getElementById('setCol1Name').value.trim() || 'العمود 1';
            const c1m = Number(document.getElementById('setCol1Max').value) || 50;
            const c2n = document.getElementById('setCol2Name').value.trim() || 'العمود 2';
            const c2m = Number(document.getElementById('setCol2Max').value) || 50;

            gradeSettings.col1 = { name: c1n, max: c1m };
            gradeSettings.col2 = { name: c2n, max: c2m };

            // تصحيح درجات الطلاب إذا كانت تتجاوز الحد الأقصى الجديد
            studentsData.forEach(s => {
                if(s.classwork > c1m) s.classwork = c1m;
                if(s.quiz > c2m) s.quiz = c2m;
            });

            closeModal('settingsModal');
            updateHeaders();
            updateUI();
        }

        // --- إدارة الفصول ---
        function confirmAddClass() {
            const newName = document.getElementById('newClassNameInput').value.trim();
            if (newName !== '') {
                if(classesData.includes(newName)) { alert('الاسم موجود مسبقاً!'); return; }
                classesData.push(newName); activeClass = newName;
                closeModal('addClassModal'); renderClassMenu(); updateUI();
            }
        }

        function openEditClassModal(oldName) {
            document.getElementById('editClassOldName').value = oldName;
            document.getElementById('editClassNameInput').value = oldName;
            openModal('editClassModal');
        }

        function confirmEditClass() {
            const oldName = document.getElementById('editClassOldName').value;
            const newName = document.getElementById('editClassNameInput').value.trim();
            if (newName !== '' && newName !== oldName) {
                if(classesData.includes(newName)) { alert('الاسم مستخدم!'); return; }
                classesData = classesData.map(c => c === oldName ? newName : c);
                studentsData.forEach(s => { if(s.className === oldName) s.className = newName; });
                if(activeClass === oldName) activeClass = newName;
                closeModal('editClassModal'); renderClassMenu(); updateUI();
            } else { closeModal('editClassModal'); }
        }

        function openDeleteConfirm(type, id, name) {
            document.getElementById('deleteTargetType').value = type;
            document.getElementById('deleteTargetId').value = id;
            let msg = type === 'class' ? `حذف فصل "${name}" وجميع درجات طلابه؟` : 'حذف الطالب ودرجاته نهائياً؟';
            document.getElementById('deleteConfirmMessage').innerText = msg;
            openModal('confirmDeleteModal');
        }

        function executeDelete() {
            const type = document.getElementById('deleteTargetType').value;
            const id = document.getElementById('deleteTargetId').value;
            if (type === 'class') {
                classesData = classesData.filter(c => c !== id);
                studentsData = studentsData.filter(s => s.className !== id);
                if (activeClass === id) activeClass = classesData.length > 0 ? classesData[0] : '';
                if (classesData.length === 0) { classesData = ['فصل جديد']; activeClass = 'فصل جديد'; }
                renderClassMenu();
            } else if (type === 'student') {
                studentsData = studentsData.filter(s => s.id !== Number(id));
            }
            closeModal('confirmDeleteModal'); updateUI();
        }

        function renderClassMenu() {
            const menu = document.getElementById('classesList');
            menu.innerHTML = '';
            classesData.forEach(cls => {
                const isActive = activeClass === cls;
                const itemDiv = document.createElement('div');
                itemDiv.className = `flex justify-between items-center px-4 py-3 rounded-xl border-2 cursor-pointer transition-all ${
                    isActive ? 'bg-indigo-50 border-indigo-200 shadow-sm' : 'bg-white border-transparent hover:border-slate-200'
                }`;
                
                const nameSpan = document.createElement('span');
                nameSpan.className = `flex-1 font-bold select-none ${isActive ? 'text-indigo-800' : 'text-slate-600'}`;
                nameSpan.innerText = cls;
                nameSpan.onclick = () => { activeClass = cls; renderClassMenu(); updateUI(); };

                const actionsDiv = document.createElement('div');
                actionsDiv.className = 'flex gap-1 opacity-60 hover:opacity-100 transition-opacity';

                const editBtn = document.createElement('button');
                editBtn.innerHTML = '<i class="fa-solid fa-pen"></i>';
                editBtn.className = 'w-8 h-8 rounded hover:bg-indigo-100 hover:text-indigo-600 text-slate-400 transition';
                editBtn.onclick = (e) => { e.stopPropagation(); openEditClassModal(cls); };

                const delBtn = document.createElement('button');
                delBtn.innerHTML = '<i class="fa-solid fa-trash"></i>';
                delBtn.className = 'w-8 h-8 rounded hover:bg-rose-100 hover:text-rose-600 text-slate-400 transition';
                delBtn.onclick = (e) => { e.stopPropagation(); openDeleteConfirm('class', cls, cls); };

                actionsDiv.appendChild(editBtn); actionsDiv.appendChild(delBtn);
                itemDiv.appendChild(nameSpan); itemDiv.appendChild(actionsDiv);
                menu.appendChild(itemDiv);
            });
        }

        // --- التقييم والمجموع ---
        function getGradeInfo(total) {
            const max = gradeSettings.col1.max + gradeSettings.col2.max;
            const percentage = (total / max) * 100;
            
            if(percentage >= 90) return { label: 'ممتاز', color: 'text-emerald-600', bg: 'bg-emerald-50', border: 'border-emerald-200', icon: 'fa-star' };
            if(percentage >= 80) return { label: 'جيد جداً', color: 'text-blue-600', bg: 'bg-blue-50', border: 'border-blue-200', icon: 'fa-thumbs-up' };
            if(percentage >= 65) return { label: 'جيد', color: 'text-amber-600', bg: 'bg-amber-50', border: 'border-amber-200', icon: 'fa-check' };
            if(percentage >= 50) return { label: 'مقبول', color: 'text-orange-500', bg: 'bg-orange-50', border: 'border-orange-200', icon: 'fa-minus' };
            return { label: 'ضعيف', color: 'text-rose-600', bg: 'bg-rose-50', border: 'border-rose-200', icon: 'fa-xmark' };
        }

        // --- واجهة المستخدم ---
        function updateUI() {
            document.getElementById('tableTitle').innerText = activeClass ? `سجل درجات: ${activeClass}` : 'سجل الدرجات';
            document.getElementById('printClassName').innerText = `الفصل: ${activeClass}`;

            const currentStudents = studentsData.filter(s => s.className === activeClass);
            currentStudents.forEach(s => { s.total = (Number(s.classwork) || 0) + (Number(s.quiz) || 0); });

            renderTable(currentStudents);
            updateStats(currentStudents);
            renderInfographic(currentStudents);
            renderLists(currentStudents);
        }

        function renderTable(students) {
            const tbody = document.getElementById('studentsTableBody');
            tbody.innerHTML = '';

            if (students.length === 0) {
                tbody.innerHTML = `<tr><td colspan="6" class="p-10 text-center text-slate-400 font-medium">لا توجد بيانات.. اضغط على إدراج طالب جديد.</td></tr>`;
                return;
            }

            const maxTotal = gradeSettings.col1.max + gradeSettings.col2.max;

            students.forEach((student, index) => {
                const tr = document.createElement('tr');
                tr.className = 'border-b border-slate-100 hover:bg-indigo-50/30 transition-colors';
                
                const gradeInfo = getGradeInfo(student.total);

                tr.innerHTML = `
                    <td class="p-4 text-slate-400 font-bold text-center">${index + 1}</td>
                    <td class="p-2">
                        <div class="flex items-center gap-2">
                            <input type="text" value="${student.name}" onchange="updateStudent(${student.id}, 'name', this.value)" 
                                class="w-full bg-transparent border-2 border-transparent hover:border-slate-200 focus:border-indigo-400 focus:bg-white rounded-lg px-3 py-2 outline-none transition font-bold text-slate-700" placeholder="اسم الطالب...">
                        </div>
                    </td>
                    <td class="p-2">
                        <input type="number" min="0" max="${gradeSettings.col1.max}" value="${student.classwork}" onchange="updateStudent(${student.id}, 'classwork', this.value)" 
                            class="w-full text-center bg-transparent border-2 border-transparent hover:border-slate-200 focus:border-indigo-400 focus:bg-white rounded-lg px-2 py-2 outline-none transition text-lg font-medium text-slate-600">
                    </td>
                    <td class="p-2">
                        <input type="number" min="0" max="${gradeSettings.col2.max}" value="${student.quiz}" onchange="updateStudent(${student.id}, 'quiz', this.value)" 
                            class="w-full text-center bg-transparent border-2 border-transparent hover:border-slate-200 focus:border-indigo-400 focus:bg-white rounded-lg px-2 py-2 outline-none transition text-lg font-medium text-slate-600">
                    </td>
                    <td class="p-4 text-center bg-slate-50/50">
                        <div class="flex flex-col items-center justify-center gap-1">
                            <span class="text-xl font-black ${gradeInfo.color}">${student.total}</span>
                            <span class="text-[10px] px-2 py-0.5 rounded-full border ${gradeInfo.bg} ${gradeInfo.border} ${gradeInfo.color} font-bold badge-print">
                                ${gradeInfo.label}
                            </span>
                        </div>
                    </td>
                    <td class="p-3 text-center no-print">
                        <button onclick="openDeleteConfirm('student', ${student.id}, '')" class="text-slate-300 hover:text-rose-500 hover:bg-rose-50 w-10 h-10 rounded-xl transition" title="حذف">
                            <i class="fa-solid fa-trash-can"></i>
                        </button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        function updateStats(students) {
            const maxTotal = gradeSettings.col1.max + gradeSettings.col2.max;
            document.getElementById('statTotalStudents').innerText = students.length;
            
            let tops = 0, strugglers = 0, sum = 0;
            students.forEach(s => {
                const perc = (s.total / maxTotal) * 100;
                if(perc >= 90) tops++;
                if(perc < 50) strugglers++;
                sum += s.total;
            });

            document.getElementById('statTopStudents').innerText = tops;
            document.getElementById('statStrugglingStudents').innerText = strugglers;
            document.getElementById('statAverage').innerText = students.length > 0 ? (sum / students.length).toFixed(1) : 0;
        }

        function renderInfographic(students) {
            const container = document.getElementById('infographicContainer');
            container.innerHTML = '';
            const maxTotal = gradeSettings.col1.max + gradeSettings.col2.max;
            
            const dist = [
                { label: 'ممتاز (90-100%)', count: 0, color: 'bg-emerald-500' },
                { label: 'جيد جداً (80-89%)', count: 0, color: 'bg-blue-400' },
                { label: 'جيد (65-79%)', count: 0, color: 'bg-amber-400' },
                { label: 'مقبول (50-64%)', count: 0, color: 'bg-orange-400' },
                { label: 'ضعيف (-50%)', count: 0, color: 'bg-rose-500' },
            ];

            students.forEach(s => {
                const perc = (s.total / maxTotal) * 100;
                if (perc >= 90) dist[0].count++; else if (perc >= 80) dist[1].count++;
                else if (perc >= 65) dist[2].count++; else if (perc >= 50) dist[3].count++; else dist[4].count++;
            });
            
            const maxCount = Math.max(...dist.map(d => d.count), 1);
            
            dist.forEach(item => {
                const percentage = (item.count / maxCount) * 100;
                container.innerHTML += `
                    <div class="flex items-center gap-4 group">
                        <div class="w-32 shrink-0 text-sm font-bold text-slate-600 group-hover:text-slate-800 transition">${item.label}</div>
                        <div class="flex-1 bg-slate-100 h-6 rounded-full overflow-hidden shadow-inner p-0.5">
                            <div class="h-full ${item.color} rounded-full transition-all duration-1000 ease-out flex items-center justify-end pr-2 text-white text-xs font-bold" style="width: ${percentage}%">
                                ${item.count > 0 ? item.count : ''}
                            </div>
                        </div>
                        <div class="w-8 text-center font-black text-slate-700 text-lg">${item.count}</div>
                    </div>`;
            });
        }

        function renderLists(students) {
            const topList = document.getElementById('topStudentsList');
            const strugglingList = document.getElementById('strugglingStudentsList');
            topList.innerHTML = ''; strugglingList.innerHTML = '';
            
            const maxTotal = gradeSettings.col1.max + gradeSettings.col2.max;

            const tops = students.filter(s => (s.total / maxTotal) * 100 >= 90).sort((a,b) => b.total - a.total);
            const strugglers = students.filter(s => (s.total / maxTotal) * 100 < 50).sort((a,b) => a.total - b.total);

            // رسائل توجيهية تلقائية
            const praiseMessages = ['أداء استثنائي!', 'فخر للمدرسة', 'استمر في التألق', 'نموذج يحتذى به', 'عمل رائع جداً'];
            const adviceMessages = ['يحتاج لمتابعة مستمرة', 'التواصل مع ولي الأمر', 'تكثيف أوراق العمل', 'تخصيص وقت للمراجعة'];

            if(tops.length === 0) topList.innerHTML = '<div class="text-center p-4 text-amber-600/70 text-sm font-medium">بانتظار تسجيل الدرجات لتحديد المتفوقين...</div>';
            tops.forEach((s, i) => { 
                const msg = praiseMessages[i % praiseMessages.length];
                topList.innerHTML += `
                    <div class="flex items-center justify-between bg-white p-3 rounded-xl shadow-sm border border-amber-100/50 hover-lift">
                        <div class="flex items-center gap-3">
                            <div class="w-8 h-8 rounded-full bg-amber-100 text-amber-600 flex items-center justify-center font-bold text-sm">${i+1}</div>
                            <div>
                                <div class="font-bold text-slate-800">${s.name || 'طالب جديد'}</div>
                                <div class="text-xs text-amber-600 font-medium">${msg}</div>
                            </div>
                        </div>
                        <div class="bg-amber-500 text-white px-3 py-1 rounded-lg font-black text-lg shadow-sm">${s.total}</div>
                    </div>`;
            });

            if(strugglers.length === 0) strugglingList.innerHTML = '<div class="text-center p-4 text-rose-600/70 text-sm font-medium">لا يوجد متعثرين في هذا الفصل، مستوى ممتاز!</div>';
            strugglers.forEach((s, i) => { 
                const msg = adviceMessages[i % adviceMessages.length];
                strugglingList.innerHTML += `
                    <div class="flex items-center justify-between bg-white p-3 rounded-xl shadow-sm border border-rose-100/50 hover-lift">
                        <div class="flex items-center gap-3">
                            <div class="w-8 h-8 rounded-full bg-rose-100 text-rose-600 flex items-center justify-center"><i class="fa-solid fa-user-minus text-xs"></i></div>
                            <div>
                                <div class="font-bold text-slate-800">${s.name || 'طالب جديد'}</div>
                                <div class="text-xs text-rose-500 font-medium"><i class="fa-solid fa-lightbulb text-amber-400 mr-1"></i> ${msg}</div>
                            </div>
                        </div>
                        <div class="bg-rose-50 text-rose-700 border border-rose-200 px-3 py-1 rounded-lg font-black text-lg">${s.total}</div>
                    </div>`;
            });
        }

        // --- التحديث ---
        function addStudent() {
            if(!activeClass) return;
            studentsData.push({ id: Date.now(), name: '', classwork: 0, quiz: 0, className: activeClass });
            updateUI();
            setTimeout(() => {
                const inputs = document.querySelectorAll('#studentsTableBody input[type="text"]');
                if(inputs.length > 0) inputs[inputs.length - 1].focus();
            }, 50);
        }

        function updateStudent(id, field, value) {
            const index = studentsData.findIndex(s => s.id === id);
            if (index > -1) {
                if(field === 'classwork' || field === 'quiz') {
                    let numValue = Number(value);
                    const maxAllowed = field === 'classwork' ? gradeSettings.col1.max : gradeSettings.col2.max;
                    if(numValue > maxAllowed) numValue = maxAllowed;
                    if(numValue < 0) numValue = 0;
                    studentsData[index][field] = numValue;
                } else {
                    studentsData[index][field] = value;
                }
                updateUI();
            }
        }

        // --- الطباعة ---
        function handlePrint() {
            try { window.print(); } catch (e) {}
            document.body.classList.add('print-preview-mode');
            document.getElementById('printPreviewAlert').classList.remove('hidden');
        }

        function exitPrintPreview() {
            document.body.classList.remove('print-preview-mode');
            document.getElementById('printPreviewAlert').classList.add('hidden');
        }

        window.onload = init;
    </script>
</body>
</html>

```
