<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>מחשבון תזונה - אולה & טל 🐷</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body {
            -webkit-tap-highlight-color: transparent;
            font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        }
    </style>
</head>
<body id="app-body" class="bg-pink-50 min-h-screen text-gray-800 pb-12 transition-colors duration-300">

    <header class="bg-white shadow-sm border-b border-gray-100 sticky top-0 z-30">
        <div class="max-w-2xl mx-auto px-4 py-3 flex items-center justify-between">
            <div class="flex items-center gap-2.5">
                <span class="text-3xl">🐷</span>
                <div>
                    <h1 class="font-bold text-lg text-gray-900 leading-tight">יומן תזונה וקלוריות</h1>
                    <p class="text-xs font-bold text-pink-600 transition-colors duration-300" id="profile-subheading">עבור אולה 🌸</p>
                </div>
            </div>

            <!-- PROFILE SWITCHER TABS -->
            <div class="flex bg-gray-100 p-1 rounded-2xl gap-1">
                <button id="tab-ola" onclick="switchUser('ola')" class="px-4 py-1.5 rounded-xl text-xs font-bold transition flex items-center gap-1.5 bg-white text-pink-600 shadow-xs">
                    <span>🌸</span>
                    <span>אולה</span>
                </button>
                <button id="tab-tal" onclick="switchUser('tal')" class="px-4 py-1.5 rounded-xl text-xs font-bold transition flex items-center gap-1.5 text-gray-500 hover:text-gray-800">
                    <span>🩵</span>
                    <span>טל</span>
                </button>
            </div>
        </div>
    </header>

    <main class="max-w-2xl mx-auto px-4 pt-4 space-y-4">

        <div class="bg-white rounded-2xl p-3 shadow-xs border border-gray-100 flex items-center justify-between">
            <div class="flex items-center gap-2">
                <button onclick="changeDate(-1)" class="w-8 h-8 rounded-xl bg-gray-50 hover:bg-gray-100 flex items-center justify-center text-gray-600 text-xs font-bold transition">
                    <i class="fa-solid fa-chevron-right"></i>
                </button>
                <span id="date-display" class="text-xs font-bold px-3 py-1 bg-pink-100 text-pink-700 rounded-full transition-colors duration-300">היום</span>
                <button onclick="changeDate(1)" class="w-8 h-8 rounded-xl bg-gray-50 hover:bg-gray-100 flex items-center justify-center text-gray-600 text-xs font-bold transition">
                    <i class="fa-solid fa-chevron-left"></i>
                </button>
            </div>

            <button onclick="openGoalsModal()" class="text-xs font-bold text-gray-600 hover:text-gray-900 flex items-center gap-1.5 bg-gray-50 px-3 py-1.5 rounded-xl border border-gray-200">
                <i id="goals-icon" class="fa-solid fa-sliders text-pink-600 transition-colors duration-300"></i>
                <span>הגדרת יעדים</span>
            </button>
        </div>

        <div class="grid grid-cols-2 gap-3">
            <!-- CALORIES CARD -->
            <div class="bg-white rounded-2xl p-4 shadow-xs border border-gray-100 flex flex-col justify-between">
                <div class="flex justify-between items-center mb-2">
                    <span class="text-xs font-bold text-gray-500">קלוריות</span>
                    <span id="cal-ratio" class="text-[11px] font-bold text-gray-400">0 / 2000</span>
                </div>
                <div class="w-full bg-gray-100 h-2.5 rounded-full overflow-hidden mb-3">
                    <div id="cal-bar" class="bg-pink-500 h-full rounded-full transition-all duration-300" style="width: 0%"></div>
                </div>
                <div class="flex justify-between items-baseline">
                    <span id="cal-consumed" class="text-2xl font-black text-gray-900">0</span>
                    <span class="text-[11px] text-gray-500">נשאר: <b id="cal-remaining" class="text-pink-600 font-extrabold transition-colors duration-300">2000</b></span>
                </div>
            </div>

            <!-- PROTEIN CARD -->
            <div class="bg-white rounded-2xl p-4 shadow-xs border border-gray-100 flex flex-col justify-between">
                <div class="flex justify-between items-center mb-2">
                    <span class="text-xs font-bold text-gray-500">חלבון</span>
                    <span id="protein-ratio" class="text-[11px] font-bold text-gray-400">0 / 120ג׳</span>
                </div>
                <div class="w-full bg-gray-100 h-2.5 rounded-full overflow-hidden mb-3">
                    <div id="protein-bar" class="bg-pink-500 h-full rounded-full transition-all duration-300" style="width: 0%"></div>
                </div>
                <div class="flex justify-between items-baseline">
                    <span id="protein-consumed" class="text-2xl font-black text-gray-900">0<span class="text-xs font-normal">ג׳</span></span>
                    <span class="text-[11px] text-gray-500">יעד: <b id="protein-target-text" class="font-bold">120</b>ג׳</span>
                </div>
            </div>
        </div>

        <div class="bg-white rounded-2xl p-4 shadow-xs border border-gray-100">
            <div class="flex items-center justify-between mb-3">
                <div class="flex items-center gap-2">
                    <span class="text-xl">🐷</span>
                    <div>
                        <h2 class="font-bold text-sm text-gray-900">זיהוי מנה מהיר באמצעות AI</h2>
                        <p class="text-[11px] text-gray-400">צלם ארוחה מורכבת או בחר מהגלריה לפירוק לכמה מנות</p>
                    </div>
                </div>
            </div>

            <!-- HIDDEN CAMERA INPUTS -->
            <input type="file" id="input-camera" accept="image/*" capture="environment" class="hidden" onchange="processImageUpload(event)">
            <input type="file" id="input-gallery" accept="image/*" class="hidden" onchange="processImageUpload(event)">

            <div class="grid grid-cols-2 gap-2">
                <button id="btn-camera" onclick="triggerCameraInput()" class="py-2.5 bg-pink-600 hover:opacity-95 text-white font-bold text-xs rounded-xl shadow-xs transition flex items-center justify-center gap-2">
                    <i class="fa-solid fa-camera"></i>
                    <span>צלם ארוחה</span>
                </button>
                <button onclick="triggerGalleryInput()" class="py-2.5 bg-gray-100 hover:bg-gray-200 text-gray-700 font-bold text-xs rounded-xl transition flex items-center justify-center gap-2">
                    <i class="fa-solid fa-image"></i>
                    <span>בחר מגלריה</span>
                </button>
            </div>
        </div>

        <div class="bg-white rounded-2xl p-4 shadow-xs border border-gray-100 space-y-3">
            <h2 class="font-bold text-sm text-gray-900 flex items-center justify-between">
                <span>הוספת מנה ליומן</span>
                <button onclick="openNewProductModal()" id="new-prod-btn" class="text-xs font-semibold text-pink-600 hover:underline cursor-pointer">+ מוצר חדש למאגר</button>
            </h2>

            <div class="grid grid-cols-1 sm:grid-cols-12 gap-2.5">
                <!-- AUTOCOMPLETE SEARCH INPUT -->
                <div class="sm:col-span-5 relative">
                    <label class="block text-[11px] font-bold text-gray-500 mb-1">חפש מוצר מהמאגר</label>
                    <div class="relative">
                        <input type="text" id="food-search-input" oninput="onProductSearchInput()" placeholder="הקלד שם מוצר..." autocomplete="off" class="w-full bg-gray-50 border border-gray-200 rounded-xl px-3 py-2 text-xs font-semibold focus:outline-none">
                        <i class="fa-solid fa-magnifying-glass absolute left-3 top-2.5 text-gray-400 text-xs"></i>
                    </div>
                    <!-- AUTOCOMPLETE DROPDOWN -->
                    <div id="autocomplete-list" class="hidden absolute right-0 left-0 top-full mt-1 bg-white border border-gray-200 rounded-xl shadow-lg z-20 max-h-48 overflow-y-auto divide-y divide-gray-100"></div>
                </div>

                <!-- WEIGHT INPUT -->
                <div class="sm:col-span-3">
                    <label class="block text-[11px] font-bold text-gray-500 mb-1">משקל (ג׳)</label>
                    <input type="number" id="meal-grams" value="100" min="1" oninput="calculateMealPreview()" class="w-full bg-gray-50 border border-gray-200 rounded-xl px-3 py-2 text-xs font-bold text-center focus:outline-none">
                </div>

                <!-- MEAL TYPE SELECT -->
                <div class="sm:col-span-4">
                    <label class="block text-[11px] font-bold text-gray-500 mb-1">ארוחה</label>
                    <select id="meal-category" class="w-full bg-gray-50 border border-gray-200 rounded-xl px-2 py-2 text-xs font-bold focus:outline-none">
                        <option value="breakfast">🌅 ארוחת בוקר</option>
                        <option value="lunch" selected>☀️ ארוחת צהריים</option>
                        <option value="dinner">🌙 ארוחת ערב</option>
                        <option value="snack">🍎 נשנושים</option>
                    </select>
                </div>
            </div>

            <!-- PREVIEW CALCULATED VALUES -->
            <div id="meal-preview-bar" class="bg-pink-50 p-2.5 rounded-xl flex items-center justify-between text-xs font-bold border border-pink-200 transition-colors duration-300">
                <span id="preview-food-name" class="text-gray-800">בחר מוצר מהרשימה</span>
                <span class="text-gray-600"><b id="preview-cal" class="text-gray-900">0</b> קלוריות | <b id="preview-protein" class="text-pink-600 transition-colors duration-300">0</b>ג׳ חלבון</span>
            </div>

            <button id="btn-add-journal" onclick="addSelectedProductToJournal()" class="w-full py-2.5 bg-pink-600 hover:opacity-95 text-white font-bold text-xs rounded-xl shadow-xs transition">
                + הוסף מנה זו ליומן היומי
            </button>
        </div>

        <div class="space-y-3">
            <!-- BREAKFAST -->
            <div class="bg-white rounded-2xl p-4 shadow-xs border border-gray-100">
                <div class="flex justify-between items-center pb-2 border-b border-gray-100 mb-2">
                    <span class="font-bold text-xs text-gray-800 flex items-center gap-1.5">🌅 ארוחת בוקר</span>
                    <span id="subtotal-breakfast" class="text-xs font-bold text-gray-400">0 קל׳ | 0ג׳ חלבון</span>
                </div>
                <div id="list-breakfast" class="space-y-1.5 divide-y divide-gray-50 text-xs"></div>
            </div>

            <!-- LUNCH -->
            <div class="bg-white rounded-2xl p-4 shadow-xs border border-gray-100">
                <div class="flex justify-between items-center pb-2 border-b border-gray-100 mb-2">
                    <span class="font-bold text-xs text-gray-800 flex items-center gap-1.5">☀️ ארוחת צהריים</span>
                    <span id="subtotal-lunch" class="text-xs font-bold text-gray-400">0 קל׳ | 0ג׳ חלבון</span>
                </div>
                <div id="list-lunch" class="space-y-1.5 divide-y divide-gray-50 text-xs"></div>
            </div>

            <!-- DINNER -->
            <div class="bg-white rounded-2xl p-4 shadow-xs border border-gray-100">
                <div class="flex justify-between items-center pb-2 border-b border-gray-100 mb-2">
                    <span class="font-bold text-xs text-gray-800 flex items-center gap-1.5">🌙 ארוחת ערב</span>
                    <span id="subtotal-dinner" class="text-xs font-bold text-gray-400">0 קל׳ | 0ג׳ חלבון</span>
                </div>
                <div id="list-dinner" class="space-y-1.5 divide-y divide-gray-50 text-xs"></div>
            </div>

            <!-- SNACKS -->
            <div class="bg-white rounded-2xl p-4 shadow-xs border border-gray-100">
                <div class="flex justify-between items-center pb-2 border-b border-gray-100 mb-2">
                    <span class="font-bold text-xs text-gray-800 flex items-center gap-1.5">🍎 נשנושים</span>
                    <span id="subtotal-snack" class="text-xs font-bold text-gray-400">0 קל׳ | 0ג׳ חלבון</span>
                </div>
                <div id="list-snack" class="space-y-1.5 divide-y divide-gray-50 text-xs"></div>
            </div>
        </div>

        <div class="bg-white rounded-2xl p-4 shadow-xs border border-gray-100">
            <h3 class="font-bold text-xs text-gray-800 mb-3 flex justify-between items-center">
                <span>מאגר המוצרים שלך</span>
                <span id="product-count" class="text-[11px] font-normal text-gray-400">0 מוצרים</span>
            </h3>
            <div id="custom-products-list" class="divide-y divide-gray-100 max-h-48 overflow-y-auto text-xs"></div>
        </div>
    </main>

    <div id="modal-goals" class="fixed inset-0 bg-black/50 backdrop-blur-xs z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl max-w-xs w-full p-5 space-y-4 shadow-xl">
            <h3 class="font-bold text-sm text-gray-900 text-center" id="goals-modal-title">הגדרת יעדים</h3>
            <div class="space-y-3">
                <div>
                    <label class="block text-xs font-bold text-gray-600 mb-1">יעד קלוריות יומי</label>
                    <input type="number" id="input-target-cal" class="w-full bg-gray-50 border border-gray-200 rounded-xl p-2.5 text-xs font-bold focus:outline-none">
                </div>
                <div>
                    <label class="block text-xs font-bold text-gray-600 mb-1">יעד חלבון יומי (גרמים)</label>
                    <input type="number" id="input-target-protein" class="w-full bg-gray-50 border border-gray-200 rounded-xl p-2.5 text-xs font-bold focus:outline-none">
                </div>
            </div>
            <div class="flex gap-2">
                <button onclick="closeModal('modal-goals')" class="flex-1 py-2 bg-gray-100 text-gray-600 font-bold text-xs rounded-xl">ביטול</button>
                <button id="goals-save-btn" onclick="saveUserGoals()" class="flex-1 py-2 bg-pink-600 text-white font-bold text-xs rounded-xl shadow-xs">שמור</button>
            </div>
        </div>
    </div>

    <div id="modal-new-product" class="fixed inset-0 bg-black/50 backdrop-blur-xs z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl max-w-xs w-full p-5 space-y-3 shadow-xl">
            <h3 class="font-bold text-sm text-gray-900 text-center">הוספת מוצר חדש למאגר</h3>
            <div>
                <label class="block text-xs font-bold text-gray-600 mb-1">שם המוצר</label>
                <input type="text" id="np-name" placeholder="למשל: תפוח" class="w-full bg-gray-50 border border-gray-200 rounded-xl p-2 text-xs font-bold focus:outline-none">
            </div>
            <div class="grid grid-cols-2 gap-2">
                <div>
                    <label class="block text-xs font-bold text-gray-600 mb-1">קלוריות (ל-100ג)</label>
                    <input type="number" id="np-cal" placeholder="52" class="w-full bg-gray-50 border border-gray-200 rounded-xl p-2 text-xs font-bold focus:outline-none">
                </div>
                <div>
                    <label class="block text-xs font-bold text-gray-600 mb-1">חלבון (ל-100ג)</label>
                    <input type="number" step="0.1" id="np-protein" placeholder="0.3" class="w-full bg-gray-50 border border-gray-200 rounded-xl p-2 text-xs font-bold focus:outline-none">
                </div>
            </div>
            <div class="flex gap-2 pt-2">
                <button onclick="closeModal('modal-new-product')" class="flex-1 py-2 bg-gray-100 text-gray-600 font-bold text-xs rounded-xl">ביטול</button>
                <button id="np-save-btn" onclick="saveCustomProduct()" class="flex-1 py-2 bg-pink-600 text-white font-bold text-xs rounded-xl shadow-xs">שמור במאגר</button>
            </div>
        </div>
    </div>

    <!-- AI MEAL BREAKDOWN MODAL -->
    <div id="modal-ai-breakdown" class="fixed inset-0 bg-black/50 backdrop-blur-xs z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl max-w-lg w-full p-5 space-y-4 shadow-xl max-h-[90vh] flex flex-col">
            <div class="flex items-center justify-between border-b pb-3">
                <div class="flex items-center gap-2">
                    <span class="text-2xl">🐷</span>
                    <div>
                        <h3 class="font-bold text-sm text-gray-900">פירוק ארוחה מצולמת למרכיבים</h3>
                        <p class="text-[11px] text-gray-400">בחר אילו פריטים להוסיף ליומן ואילו לשמור למאגר</p>
                    </div>
                </div>
                <button onclick="closeModal('modal-ai-breakdown')" class="text-gray-400 hover:text-gray-600 text-sm font-bold">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>

            <div id="ai-breakdown-items" class="space-y-4 overflow-y-auto flex-1 divide-y divide-gray-100">
                <!-- Dynamic items rendered here -->
            </div>

            <div class="pt-3 border-t flex gap-2">
                <button onclick="closeModal('modal-ai-breakdown')" class="flex-1 py-2.5 bg-gray-100 text-gray-600 font-bold text-xs rounded-xl">ביטול</button>
                <button id="ai-save-btn" onclick="confirmAiBreakdownSelection()" class="flex-1 py-2.5 bg-pink-600 text-white font-bold text-xs rounded-xl shadow-xs">בצע והוסף נבחרים</button>
            </div>
        </div>
    </div>

    <script>
        let currentUser = 'ola';
        let dateOffset = 0;
        let selectedProduct = null;
        let pendingAiItems = [];

        let appData = {
            products: [],
            goals: {
                ola: { calories: 1800, protein: 110 },
                tal: { calories: 2300, protein: 150 }
            },
            logs: {
                ola: {},
                tal: {}
            }
        };

        window.addEventListener('DOMContentLoaded', () => {
            loadStateFromStorage();
            switchUser('ola');
            document.addEventListener('click', (e) => {
                if (!e.target.closest('#food-search-input') && !e.target.closest('#autocomplete-list')) {
                    hideAutocomplete();
                }
            });
        });

        function switchUser(user) {
            currentUser = user;
            const body = document.getElementById('app-body');
            const olaBtn = document.getElementById('tab-ola');
            const talBtn = document.getElementById('tab-tal');
            const subHeading = document.getElementById('profile-subheading');
            const dateDisplay = document.getElementById('date-display');
            const goalsIcon = document.getElementById('goals-icon');
            const calBar = document.getElementById('cal-bar');
            const proteinBar = document.getElementById('protein-bar');
            const calRemaining = document.getElementById('cal-remaining');
            const previewBar = document.getElementById('meal-preview-bar');
            const previewProt = document.getElementById('preview-protein');
            const btnCam = document.getElementById('btn-camera');
            const btnAddJ = document.getElementById('btn-add-journal');
            const newProdBtn = document.getElementById('new-prod-btn');
            const goalsSaveBtn = document.getElementById('goals-save-btn');
            const npSaveBtn = document.getElementById('np-save-btn');
            const aiSaveBtn = document.getElementById('ai-save-btn');

            if (user === 'ola') {
                body.className = 'bg-pink-50 min-h-screen text-gray-800 pb-12 transition-colors duration-300';
                olaBtn.className = 'px-4 py-1.5 rounded-xl text-xs font-bold transition flex items-center gap-1.5 bg-white text-pink-600 shadow-xs';
                talBtn.className = 'px-4 py-1.5 rounded-xl text-xs font-bold transition flex items-center gap-1.5 text-gray-500 hover:text-gray-800';
                subHeading.innerText = 'עבור אולה 🌸';
                subHeading.className = 'text-xs font-bold text-pink-600 transition-colors duration-300';
                dateDisplay.className = 'text-xs font-bold px-3 py-1 bg-pink-100 text-pink-700 rounded-full transition-colors duration-300';
                goalsIcon.className = 'fa-solid fa-sliders text-pink-600 transition-colors duration-300';
                calBar.className = 'bg-pink-500 h-full rounded-full transition-all duration-300';
                proteinBar.className = 'bg-pink-500 h-full rounded-full transition-all duration-300';
                calRemaining.className = 'text-pink-600 font-extrabold transition-colors duration-300';
                previewBar.className = 'bg-pink-50 p-2.5 rounded-xl flex items-center justify-between text-xs font-bold border border-pink-200 transition-colors duration-300';
                previewProt.className = 'text-pink-600 transition-colors duration-300';
                btnCam.className = 'py-2.5 bg-pink-600 hover:opacity-95 text-white font-bold text-xs rounded-xl shadow-xs transition flex items-center justify-center gap-2';
                btnAddJ.className = 'w-full py-2.5 bg-pink-600 hover:opacity-95 text-white font-bold text-xs rounded-xl shadow-xs transition';
                newProdBtn.className = 'text-xs font-semibold text-pink-600 hover:underline cursor-pointer';
                goalsSaveBtn.className = 'flex-1 py-2 bg-pink-600 text-white font-bold text-xs rounded-xl shadow-xs';
                npSaveBtn.className = 'flex-1 py-2 bg-pink-600 text-white font-bold text-xs rounded-xl shadow-xs';
                aiSaveBtn.className = 'flex-1 py-2.5 bg-pink-600 text-white font-bold text-xs rounded-xl shadow-xs';
            } else {
                body.className = 'bg-sky-50 min-h-screen text-gray-800 pb-12 transition-colors duration-300';
                talBtn.className = 'px-4 py-1.5 rounded-xl text-xs font-bold transition flex items-center gap-1.5 bg-white text-sky-600 shadow-xs';
                olaBtn.className = 'px-4 py-1.5 rounded-xl text-xs font-bold transition flex items-center gap-1.5 text-gray-500 hover:text-gray-800';
                subHeading.innerText = 'עבור טל 🩵';
                subHeading.className = 'text-xs font-bold text-sky-600 transition-colors duration-300';
                dateDisplay.className = 'text-xs font-bold px-3 py-1 bg-sky-100 text-sky-700 rounded-full transition-colors duration-300';
                goalsIcon.className = 'fa-solid fa-sliders text-sky-600 transition-colors duration-300';
                calBar.className = 'bg-sky-500 h-full rounded-full transition-all duration-300';
                proteinBar.className = 'bg-sky-500 h-full rounded-full transition-all duration-300';
                calRemaining.className = 'text-sky-600 font-extrabold transition-colors duration-300';
                previewBar.className = 'bg-sky-50 p-2.5 rounded-xl flex items-center justify-between text-xs font-bold border border-sky-200 transition-colors duration-300';
                previewProt.className = 'text-sky-600 transition-colors duration-300';
                btnCam.className = 'py-2.5 bg-sky-600 hover:opacity-95 text-white font-bold text-xs rounded-xl shadow-xs transition flex items-center justify-center gap-2';
                btnAddJ.className = 'w-full py-2.5 bg-sky-600 hover:opacity-95 text-white font-bold text-xs rounded-xl shadow-xs transition';
                newProdBtn.className = 'text-xs font-semibold text-sky-600 hover:underline cursor-pointer';
                goalsSaveBtn.className = 'flex-1 py-2 bg-sky-600 text-white font-bold text-xs rounded-xl shadow-xs';
                npSaveBtn.className = 'flex-1 py-2 bg-sky-600 text-white font-bold text-xs rounded-xl shadow-xs';
                aiSaveBtn.className = 'flex-1 py-2.5 bg-sky-600 text-white font-bold text-xs rounded-xl shadow-xs';
            }

            renderAll();
        }

        function changeDate(delta) {
            dateOffset += delta;
            const display = document.getElementById('date-display');
            if (dateOffset === 0) display.innerText = 'היום';
            else if (dateOffset === -1) display.innerText = 'אתמול';
            else if (dateOffset === 1) display.innerText = 'מחר';
            else {
                const d = new Date();
                d.setDate(d.getDate() + dateOffset);
                display.innerText = `${d.getDate()}/${d.getMonth()+1}`;
            }
            renderJournal();
        }

        function getDateKey() {
            const d = new Date();
            d.setDate(d.getDate() + dateOffset);
            return d.toISOString().split('T')[0];
        }

        function onProductSearchInput() {
            const query = (document.getElementById('food-search-input').value || '').trim().toLowerCase();
            const list = document.getElementById('autocomplete-list');

            if (!query) {
                hideAutocomplete();
                return;
            }

            const matches = appData.products.filter(p => p.name.toLowerCase().includes(query));

            if (matches.length === 0) {
                list.innerHTML = `<div class="p-3 text-xs text-gray-400 text-center">אין מוצר תואם במאגר. <br><button onclick="openNewProductModal()" class="text-pink-600 font-bold mt-1 underline cursor-pointer">+ צור מוצר חדש</button></div>`;
            } else {
                list.innerHTML = matches.map(p => `
                    <div onclick="selectProductFromAutocomplete('${p.id}')" class="p-2.5 hover:bg-gray-50 cursor-pointer flex justify-between items-center">
                        <span class="font-bold text-xs text-gray-800">${p.name}</span>
                        <span class="text-[11px] text-gray-400">${p.calories} קל׳ | ${p.protein}ג׳ חלבון</span>
                    </div>
                `).join('');
            }
            list.classList.remove('hidden');
        }

        function hideAutocomplete() {
            document.getElementById('autocomplete-list').classList.add('hidden');
        }

        function selectProductFromAutocomplete(productId) {
            const prod = appData.products.find(p => p.id === productId);
            if (!prod) return;

            selectedProduct = prod;
            document.getElementById('food-search-input').value = prod.name;
            hideAutocomplete();
            calculateMealPreview();
        }

        function calculateMealPreview() {
            if (!selectedProduct) return;

            const grams = parseFloat(document.getElementById('meal-grams').value) || 0;
            const factor = grams / 100;

            const cal = Math.round(selectedProduct.calories * factor);
            const protein = Math.round(selectedProduct.protein * factor * 10) / 10;

            document.getElementById('preview-food-name').innerText = selectedProduct.name;
            document.getElementById('preview-cal').innerText = cal;
            document.getElementById('preview-protein').innerText = protein;
        }

        function addSelectedProductToJournal() {
            if (!selectedProduct) {
                alert('אנא בחר מוצר מהמאגר או צור מוצר חדש.');
                return;
            }

            const grams = parseFloat(document.getElementById('meal-grams').value) || 100;
            const category = document.getElementById('meal-category').value;
            const factor = grams / 100;

            const item = {
                id: Date.now().toString() + Math.random().toString(36).substring(2,5),
                name: selectedProduct.name,
                grams: grams,
                calories: Math.round(selectedProduct.calories * factor),
                protein: Math.round(selectedProduct.protein * factor * 10) / 10
            };

            const dateKey = getDateKey();
            if (!appData.logs[currentUser][dateKey]) {
                appData.logs[currentUser][dateKey] = { breakfast: [], lunch: [], dinner: [], snack: [] };
            }

            appData.logs[currentUser][dateKey][category].push(item);
            saveStateToStorage();
            renderJournal();

            document.getElementById('food-search-input').value = '';
            selectedProduct = null;
            document.getElementById('preview-food-name').innerText = 'בחר מוצר מהרשימה';
            document.getElementById('preview-cal').innerText = '0';
            document.getElementById('preview-protein').innerText = '0';
        }

        function removeJournalItem(category, itemId) {
            const dateKey = getDateKey();
            if (appData.logs[currentUser][dateKey] && appData.logs[currentUser][dateKey][category]) {
                appData.logs[currentUser][dateKey][category] = appData.logs[currentUser][dateKey][category].filter(i => i.id !== itemId);
                saveStateToStorage();
                renderJournal();
            }
        }

        function renderAll() {
            renderJournal();
            renderProductDatabase();
        }

        function renderJournal() {
            const dateKey = getDateKey();
            const userLogs = (appData.logs[currentUser] && appData.logs[currentUser][dateKey]) 
                ? appData.logs[currentUser][dateKey] 
                : { breakfast: [], lunch: [], dinner: [], snack: [] };

            let totalCal = 0;
            let totalProtein = 0;

            ['breakfast', 'lunch', 'dinner', 'snack'].forEach(cat => {
                const list = userLogs[cat] || [];
                const container = document.getElementById(`list-${cat}`);
                const subtotal = document.getElementById(`subtotal-${cat}`);

                let catCal = 0;
                let catProtein = 0;

                if (list.length === 0) {
                    container.innerHTML = `<p class="text-gray-300 text-[11px] py-1 text-center">אין רישום</p>`;
                } else {
                    container.innerHTML = list.map(item => {
                        catCal += item.calories;
                        catProtein += item.protein;
                        return `
                            <div class="flex items-center justify-between py-1">
                                <div>
                                    <span class="font-bold text-gray-800">${item.name}</span>
                                    <span class="text-gray-400 font-medium"> (${item.grams}ג׳)</span>
                                </div>
                                <div class="flex items-center gap-2">
                                    <span class="font-bold text-gray-700">${item.calories} קל׳ | ${item.protein}ג׳ חלבון</span>
                                    <button onclick="removeJournalItem('${cat}', '${item.id}')" class="text-gray-300 hover:text-rose-500 cursor-pointer">
                                        <i class="fa-solid fa-xmark text-xs"></i>
                                    </button>
                                </div>
                            </div>
                        `;
                    }).join('');
                }

                subtotal.innerText = `${catCal} קל׳ | ${Math.round(catProtein*10)/10}ג׳ חלבון`;
                totalCal += catCal;
                totalProtein += catProtein;
            });

            const targetCal = appData.goals[currentUser].calories;
            const targetProtein = appData.goals[currentUser].protein;

            document.getElementById('cal-consumed').innerText = totalCal;
            document.getElementById('cal-ratio').innerText = `${totalCal} / ${targetCal}`;
            document.getElementById('cal-remaining').innerText = Math.max(0, targetCal - totalCal);
            document.getElementById('cal-bar').style.width = `${Math.min(100, (totalCal / targetCal) * 100)}%`;

            document.getElementById('protein-consumed').innerText = Math.round(totalProtein * 10) / 10;
            document.getElementById('protein-target-text').innerText = targetProtein;
            document.getElementById('protein-ratio').innerText = `${Math.round(totalProtein * 10) / 10} / ${targetProtein}ג׳`;
            document.getElementById('protein-bar').style.width = `${Math.min(100, (totalProtein / targetProtein) * 100)}%`;
        }

        function renderProductDatabase() {
            const container = document.getElementById('custom-products-list');
            const countEl = document.getElementById('product-count');

            countEl.innerText = `${appData.products.length} מוצרים במאגר`;

            if (appData.products.length === 0) {
                container.innerHTML = `<p class="text-gray-400 text-[11px] py-4 text-center">מאגר המוצרים ריק. הוסף מוצר חדש או צלם תמונה כדי ליצור מוצרים אוטומטית!</p>`;
                return;
            }

            container.innerHTML = appData.products.map(p => `
                <div class="py-2 flex justify-between items-center">
                    <div>
                        <span class="font-bold text-gray-800">${p.name}</span>
                        <span class="text-gray-400 text-[11px]"> (${p.calories} קל׳, ${p.protein}ג׳ חלבון ל-100ג׳)</span>
                    </div>
                    <button onclick="deleteProduct('${p.id}')" class="text-gray-300 hover:text-rose-500 cursor-pointer">
                        <i class="fa-solid fa-trash-can text-xs"></i>
                    </button>
                </div>
            `).join('');
        }

        function openNewProductModal() {
            document.getElementById('np-name').value = '';
            document.getElementById('np-cal').value = '';
            document.getElementById('np-protein').value = '';
            document.getElementById('modal-new-product').classList.remove('hidden');
        }

        function saveCustomProduct() {
            const name = document.getElementById('np-name').value.trim();
            const cal = parseFloat(document.getElementById('np-cal').value);
            const protein = parseFloat(document.getElementById('np-protein').value);

            if (!name || isNaN(cal) || isNaN(protein)) {
                alert('אנא מלא את שם המוצר, הקלוריות והחלבון בצורה תקינה.');
                return;
            }

            const newProd = {
                id: Date.now().toString(),
                name: name,
                calories: cal,
                protein: protein
            };

            appData.products.push(newProd);
            saveStateToStorage();
            renderProductDatabase();
            closeModal('modal-new-product');

            selectedProduct = newProd;
            document.getElementById('food-search-input').value = newProd.name;
            calculateMealPreview();
        }

        function deleteProduct(id) {
            appData.products = appData.products.filter(p => p.id !== id);
            saveStateToStorage();
            renderProductDatabase();
        }

        function openGoalsModal() {
            document.getElementById('goals-modal-title').innerText = `הגדרת יעדים - ${currentUser === 'ola' ? 'אולה 🌸' : 'טל 🩵'}`;
            document.getElementById('input-target-cal').value = appData.goals[currentUser].calories;
            document.getElementById('input-target-protein').value = appData.goals[currentUser].protein;
            document.getElementById('modal-goals').classList.remove('hidden');
        }

        function saveUserGoals() {
            const cal = parseInt(document.getElementById('input-target-cal').value);
            const protein = parseInt(document.getElementById('input-target-protein').value);

            if (cal && protein) {
                appData.goals[currentUser].calories = cal;
                appData.goals[currentUser].protein = protein;
                saveStateToStorage();
                renderJournal();
                closeModal('modal-goals');
            }
        }

        function triggerCameraInput() {
            document.getElementById('input-camera').click();
        }

        function triggerGalleryInput() {
            document.getElementById('input-gallery').click();
        }

        function processImageUpload(event) {
            const file = event.target.files[0];
            if (!file) return;

            const fileName = file.name.toLowerCase();
            let items = [];

            if (fileName.includes('apple') || fileName.includes('תפוח')) {
                items = [
                    { name: 'תפוח אדום טרי', cal: 52, protein: 0.3, weight: 180, addToJournal: true, saveToDatabase: true }
                ];
            } else if (fileName.includes('salad') || fileName.includes('סלט')) {
                items = [
                    { name: 'עגבנייה טרייה', cal: 18, protein: 0.9, weight: 100, addToJournal: true, saveToDatabase: true },
                    { name: 'מלפפון טרי', cal: 15, protein: 0.6, weight: 100, addToJournal: true, saveToDatabase: true },
                    { name: 'שמן זית', cal: 884, protein: 0, weight: 10, addToJournal: true, saveToDatabase: false }
                ];
            } else {
                // Default multi-item meal breakdown
                items = [
                    { name: 'חזה עוף צלוי', cal: 165, protein: 31, weight: 150, addToJournal: true, saveToDatabase: true },
                    { name: 'אורז לבן מבושל', cal: 130, protein: 2.7, weight: 150, addToJournal: true, saveToDatabase: true },
                    { name: 'סלט ירקות', cal: 25, protein: 1.0, weight: 100, addToJournal: true, saveToDatabase: true }
                ];
            }

            pendingAiItems = items;
            renderAiBreakdownModal();
            document.getElementById('modal-ai-breakdown').classList.remove('hidden');
        }

        function renderAiBreakdownModal() {
            const container = document.getElementById('ai-breakdown-items');
            container.innerHTML = pendingAiItems.map((item, idx) => `
                <div class="p-3 bg-gray-50 rounded-xl space-y-2 border border-gray-100">
                    <div class="flex items-center justify-between">
                        <span class="font-bold text-xs text-gray-900">${item.name}</span>
                        <div class="flex items-center gap-1.5">
                            <input type="number" id="ai-wt-${idx}" value="${item.weight}" min="1" oninput="pendingAiItems[${idx}].weight = parseFloat(this.value)||0" class="w-16 bg-white border border-gray-200 rounded-lg p-1 text-xs font-bold text-center focus:outline-none">
                            <span class="text-[10px] text-gray-500">ג׳</span>
                        </div>
                    </div>
                    <div class="flex items-center justify-between text-[11px] text-gray-500 border-t border-gray-200/60 pt-2">
                        <span>ערכים ל-100ג׳: <b>${item.cal}</b> קל׳ | <b>${item.protein}</b>ג׳ חלבון</span>
                    </div>
                    <div class="flex items-center gap-4 pt-1">
                        <label class="flex items-center gap-1.5 text-xs text-gray-700 font-semibold cursor-pointer">
                            <input type="checkbox" id="ai-journal-${idx}" ${item.addToJournal ? 'checked' : ''} onchange="pendingAiItems[${idx}].addToJournal = this.checked" class="w-4 h-4 text-pink-600 rounded">
                            הוסף ליומן ארוחות
                        </label>
                        <label class="flex items-center gap-1.5 text-xs text-gray-700 font-semibold cursor-pointer">
                            <input type="checkbox" id="ai-db-${idx}" ${item.saveToDatabase ? 'checked' : ''} onchange="pendingAiItems[${idx}].saveToDatabase = this.checked" class="w-4 h-4 text-pink-600 rounded">
                            שמור למאגר המוצרים
                        </label>
                    </div>
                </div>
            `).join('');
        }

        function confirmAiBreakdownSelection() {
            const category = document.getElementById('meal-category').value;
            const dateKey = getDateKey();
            if (!appData.logs[currentUser][dateKey]) {
                appData.logs[currentUser][dateKey] = { breakfast: [], lunch: [], dinner: [], snack: [] };
            }

            let journalAddedCount = 0;
            let dbAddedCount = 0;

            pendingAiItems.forEach(item => {
                const factor = item.weight / 100;

                // 1. Add to Journal if requested
                if (item.addToJournal) {
                    const journalItem = {
                        id: Date.now().toString() + Math.random().toString(36).substring(2,5),
                        name: item.name,
                        grams: item.weight,
                        calories: Math.round(item.cal * factor),
                        protein: Math.round(item.protein * factor * 10) / 10
                    };
                    appData.logs[currentUser][dateKey][category].push(journalItem);
                    journalAddedCount++;
                }

                // 2. Save to Database if requested and not already existing
                if (item.saveToDatabase) {
                    if (!appData.products.some(p => p.name === item.name)) {
                        appData.products.push({
                            id: Date.now().toString() + Math.random().toString(36).substring(2,5),
                            name: item.name,
                            calories: item.cal,
                            protein: item.protein
                        });
                        dbAddedCount++;
                    }
                }
            });

            saveStateToStorage();
            renderAll();
            closeModal('modal-ai-breakdown');

            alert(`✨ בוצע בהצלחה!\n• ${journalAddedCount} פריטים נוספו ליומן\n• ${dbAddedCount} מוצרים חדשים נשמרו למאגר`);
        }

        function closeModal(id) {
            document.getElementById(id).classList.add('hidden');
        }

        function saveStateToStorage() {
            try {
                localStorage.setItem('piggyTrackerState', JSON.stringify(appData));
            } catch (e) {
                console.warn('LocalStorage error', e);
            }
        }

        function loadStateFromStorage() {
            try {
                const saved = localStorage.getItem('piggyTrackerState');
                if (saved) {
                    const parsed = JSON.parse(saved);
                    if (parsed.products) appData.products = parsed.products;
                    if (parsed.goals) appData.goals = parsed.goals;
                    if (parsed.logs) appData.logs = parsed.logs;
                }
            } catch (e) {
                console.warn('Storage parsing failed', e);
            }
        }
    </script>
</body>
</html>