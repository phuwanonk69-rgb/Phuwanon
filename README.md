<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบการจัดการนักศึกษา</title>
    <!-- ใช้ Google Font (Prompt) -->
    <link href="https://fonts.googleapis.com/css2?family=Prompt:wght@300;400;500;600&display=swap" rel="stylesheet">
    <!-- โหลด Tailwind CSS ผ่าน CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Prompt', 'sans-serif'],
                    }
                }
            }
        }
    </script>
</head>
<body class="bg-slate-100 text-slate-800 flex h-screen overflow-hidden font-sans">

    <!-- เมนูด้านซ้าย (Sidebar) -->
    <aside class="w-64 bg-slate-800 text-white flex flex-col flex-shrink-0">
        <div class="p-5 text-xl font-semibold bg-slate-900 text-center">
            Admin Panel
        </div>
        <nav class="py-5 flex-1">
            <ul class="space-y-1">
                <li>
                    <a href="#" class="block py-3 px-5 bg-slate-700 text-white border-l-4 border-blue-500 transition">จัดการนักศึกษา</a>
                </li>
                <li>
                    <a href="#" class="block py-3 px-5 text-slate-400 hover:bg-slate-700 hover:text-white transition">จัดการรายวิชา</a>
                </li>
                <li>
                    <a href="#" class="block py-3 px-5 text-slate-400 hover:bg-slate-700 hover:text-white transition">ผลการเรียน</a>
                </li>
                <li>
                    <a href="#" class="block py-3 px-5 text-slate-400 hover:bg-slate-700 hover:text-white transition">ออกจากระบบ</a>
                </li>
            </ul>
        </nav>
    </aside>

    <!-- เนื้อหาหลัก (Main Content) -->
    <div class="flex-1 flex flex-col overflow-y-auto">
        <header class="bg-white px-8 py-4 shadow-sm flex justify-between items-center">
            <h1 class="text-2xl font-semibold text-slate-800">ระบบการจัดการนักศึกษา</h1>
            <div class="text-sm text-slate-500">ผู้ดูแลระบบ: Admin</div>
        </header>

        <main class="p-8">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
                <div class="bg-blue-50 border border-blue-100 rounded-lg p-4">
                    <div class="text-sm text-blue-700">นักศึกษาทั้งหมด</div>
                    <div id="totalStudents" class="text-2xl font-semibold text-blue-900">0</div>
                </div>
                <div class="bg-emerald-50 border border-emerald-100 rounded-lg p-4">
                    <div class="text-sm text-emerald-700">สาขาวิชาทั้งหมด</div>
                    <div id="majorCount" class="text-2xl font-semibold text-emerald-900">0</div>
                </div>
                <div class="bg-amber-50 border border-amber-100 rounded-lg p-4">
                    <div class="text-sm text-amber-700">ชั้นปีที่มี</div>
                    <div id="yearCount" class="text-2xl font-semibold text-amber-900">0</div>
                </div>
            </div>

            <div class="bg-white rounded-lg shadow-sm p-6 mb-6">
                <!-- ส่วนหัวการ์ด (ค้นหา & ปุ่มเพิ่ม) -->
                <div class="flex flex-col md:flex-row justify-between items-center mb-6 gap-4">
                    <h3 class="text-lg font-medium text-slate-800">รายชื่อนักศึกษาทั้งหมด</h3>
                    <div class="flex gap-3 items-center w-full md:w-auto">
                        <input type="text" id="searchInput" class="px-3 py-2 border border-slate-300 rounded-md text-sm w-full md:w-64 focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="ค้นหาชื่อ หรือรหัส..." onkeyup="searchStudent()">
                        <button onclick="openModal('add')" class="bg-blue-600 hover:bg-blue-700 text-white text-sm px-4 py-2 rounded-md transition whitespace-nowrap shadow-sm">+ เพิ่มนักศึกษาใหม่</button>
                    </div>
                </div>

                <!-- ตารางแสดงข้อมูล -->
                <div class="overflow-x-auto">
                    <table id="studentTable" class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-slate-50 text-slate-700 border-b border-slate-200">
                                <th class="py-3 px-4 font-medium">รหัสนักศึกษา</th>
                                <th class="py-3 px-4 font-medium">ชื่อ - นามสกุล</th>
                                <th class="py-3 px-4 font-medium">สาขาวิชา</th>
                                <th class="py-3 px-4 font-medium">ชั้นปี</th>
                                <th class="py-3 px-4 font-medium">จัดการ</th>
                            </tr>
                        </thead>
                        <tbody id="studentTableBody" class="divide-y divide-slate-100">
                            <tr class="hover:bg-slate-50/80 transition">
                                <td class="py-3 px-4">6601001</td>
                                <td class="py-3 px-4">นายสมชาย ใจดี</td>
                                <td class="py-3 px-4">วิทยาการคอมพิวเตอร์</td>
                                <td class="py-3 px-4">ปี 3</td>
                                <td class="py-3 px-4 flex gap-2">
                                    <button onclick="editRow(this)" class="bg-amber-500 hover:bg-amber-600 text-white text-xs px-3 py-1.5 rounded transition shadow-sm">แก้ไข</button>
                                    <button onclick="deleteRow(this)" class="bg-rose-500 hover:bg-rose-600 text-white text-xs px-3 py-1.5 rounded transition shadow-sm">ลบ</button>
                                </td>
                            </tr>
                            <tr class="hover:bg-slate-50/80 transition">
                                <td class="py-3 px-4">6601002</td>
                                <td class="py-3 px-4">นางสาวสมหญิง รักเรียน</td>
                                <td class="py-3 px-4">เทคโนโลยีสารสนเทศ</td>
                                <td class="py-3 px-4">ปี 2</td>
                                <td class="py-3 px-4 flex gap-2">
                                    <button onclick="editRow(this)" class="bg-amber-500 hover:bg-amber-600 text-white text-xs px-3 py-1.5 rounded transition shadow-sm">แก้ไข</button>
                                    <button onclick="deleteRow(this)" class="bg-rose-500 hover:bg-rose-600 text-white text-xs px-3 py-1.5 rounded transition shadow-sm">ลบ</button>
                                </td>
                            </tr>
                            <tr class="hover:bg-slate-50/80 transition">
                                <td class="py-3 px-4">6601003</td>
                                <td class="py-3 px-4">นายมานะ อดทน</td>
                                <td class="py-3 px-4">วิศวกรรมซอฟต์แวร์</td>
                                <td class="py-3 px-4">ปี 1</td>
                                <td class="py-3 px-4 flex gap-2">
                                    <button onclick="editRow(this)" class="bg-amber-500 hover:bg-amber-600 text-white text-xs px-3 py-1.5 rounded transition shadow-sm">แก้ไข</button>
                                    <button onclick="deleteRow(this)" class="bg-rose-500 hover:bg-rose-600 text-white text-xs px-3 py-1.5 rounded transition shadow-sm">ลบ</button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </main>
    </div>

    <!-- Modal สำหรับ เพิ่ม / แก้ไข ข้อมูล -->
    <div id="studentModal" class="fixed inset-0 bg-black/50 hidden justify-center items-center z-50">
        <div class="bg-white p-6 rounded-lg w-96 shadow-xl">
            <h3 id="modalTitle" class="text-lg font-semibold text-slate-800 mb-4">เพิ่มนักศึกษาใหม่</h3>
            
            <div class="space-y-4">
                <div>
                    <label class="block text-sm text-slate-600 mb-1">รหัสนักศึกษา</label>
                    <input type="text" id="stdId" class="w-full px-3 py-2 border border-slate-300 rounded-md text-sm focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="เช่น 6601004">
                </div>
                <div>
                    <label class="block text-sm text-slate-600 mb-1">ชื่อ - นามสกุล</label>
                    <input type="text" id="stdName" class="w-full px-3 py-2 border border-slate-300 rounded-md text-sm focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="เช่น นายสมปอง รักดี">
                </div>
                <div>
                    <label class="block text-sm text-slate-600 mb-1">สาขาวิชา</label>
                    <input type="text" id="stdMajor" class="w-full px-3 py-2 border border-slate-300 rounded-md text-sm focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="เช่น วิทยาการคอมพิวเตอร์">
                </div>
                <div>
                    <label class="block text-sm text-slate-600 mb-1">ชั้นปี</label>
                    <input type="text" id="stdYear" class="w-full px-3 py-2 border border-slate-300 rounded-md text-sm focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="เช่น ปี 1">
                </div>
            </div>

            <div class="flex justify-end gap-2 mt-6">
                <button onclick="closeModal()" class="bg-rose-500 hover:bg-rose-600 text-white text-sm px-4 py-2 rounded-md transition shadow-sm">ยกเลิก</button>
                <button onclick="saveStudent()" class="bg-blue-600 hover:bg-blue-700 text-white text-sm px-4 py-2 rounded-md transition shadow-sm">บันทึก</button>
            </div>
        </div>
    </div>

    <!-- ส่วน JavaScript ควบคุมการทำงาน -->
    <script>
        let currentMode = 'add';
        let selectedTr = null;
        const STORAGE_KEY = 'studentManagementData';

        function openModal(mode, tr = null) {
            currentMode = mode;
            const modal = document.getElementById('studentModal');
            const modalTitle = document.getElementById('modalTitle');
            
            if (mode === 'add') {
                modalTitle.innerText = "เพิ่มนักศึกษาใหม่";
                document.getElementById('stdId').value = '';
                document.getElementById('stdName').value = '';
                document.getElementById('stdMajor').value = '';
                document.getElementById('stdYear').value = '';
            } else if (mode === 'edit') {
                modalTitle.innerText = "แก้ไขข้อมูลนักศึกษา";
                selectedTr = tr;
                document.getElementById('stdId').value = tr.cells[0].innerText;
                document.getElementById('stdName').value = tr.cells[1].innerText;
                document.getElementById('stdMajor').value = tr.cells[2].innerText;
                document.getElementById('stdYear').value = tr.cells[3].innerText;
            }
            modal.classList.remove('hidden');
            modal.classList.add('flex');
        }

        function closeModal() {
            const modal = document.getElementById('studentModal');
            modal.classList.remove('flex');
            modal.classList.add('hidden');
        }

        function getStudentsFromTable() {
            const rows = Array.from(document.querySelectorAll('#studentTableBody tr'));
            return rows.map((row) => ({
                id: row.cells[0].innerText.trim(),
                name: row.cells[1].innerText.trim(),
                major: row.cells[2].innerText.trim(),
                year: row.cells[3].innerText.trim()
            }));
        }

        function saveStudentsToStorage() {
            localStorage.setItem(STORAGE_KEY, JSON.stringify(getStudentsFromTable()));
            updateSummary();
        }

        function updateSummary() {
            const students = getStudentsFromTable();
            const totalStudents = students.length;
            const majors = new Set(students.map((student) => student.major));
            const years = new Set(students.map((student) => student.year));

            document.getElementById('totalStudents').innerText = totalStudents;
            document.getElementById('majorCount').innerText = majors.size;
            document.getElementById('yearCount').innerText = years.size;
        }

        function addStudentRow(student, shouldSave = true) {
            const tbody = document.getElementById('studentTableBody');
            const newRow = tbody.insertRow();
            newRow.className = "hover:bg-slate-50/80 transition";
            newRow.innerHTML = `
                <td class="py-3 px-4">${student.id}</td>
                <td class="py-3 px-4">${student.name}</td>
                <td class="py-3 px-4">${student.major}</td>
                <td class="py-3 px-4">${student.year}</td>
                <td class="py-3 px-4 flex gap-2">
                    <button onclick="editRow(this)" class="bg-amber-500 hover:bg-amber-600 text-white text-xs px-3 py-1.5 rounded transition shadow-sm">แก้ไข</button>
                    <button onclick="deleteRow(this)" class="bg-rose-500 hover:bg-rose-600 text-white text-xs px-3 py-1.5 rounded transition shadow-sm">ลบ</button>
                </td>
            `;
            if (shouldSave) {
                saveStudentsToStorage();
            }
        }

        function loadStudentsFromStorage() {
            const storedData = localStorage.getItem(STORAGE_KEY);
            const tbody = document.getElementById('studentTableBody');

            if (storedData) {
                try {
                    const students = JSON.parse(storedData);
                    if (Array.isArray(students) && students.length > 0) {
                        tbody.innerHTML = '';
                        students.forEach((student) => addStudentRow(student, false));
                        updateSummary();
                        return;
                    }
                } catch (error) {
                    console.error('Failed to load students from storage', error);
                }
            }

            const existingStudents = getStudentsFromTable();
            if (existingStudents.length > 0) {
                saveStudentsToStorage();
            } else {
                updateSummary();
            }
        }

        function saveStudent() {
            const id = document.getElementById('stdId').value;
            const name = document.getElementById('stdName').value;
            const major = document.getElementById('stdMajor').value;
            const year = document.getElementById('stdYear').value;

            if (!id || !name || !major || !year) {
                alert("กรุณากรอกข้อมูลให้ครบทุกช่อง");
                return;
            }

            if (currentMode === 'add') {
                addStudentRow({ id, name, major, year });
            } else if (currentMode === 'edit' && selectedTr) {
                selectedTr.cells[0].innerText = id;
                selectedTr.cells[1].innerText = name;
                selectedTr.cells[2].innerText = major;
                selectedTr.cells[3].innerText = year;
                saveStudentsToStorage();
            }

            closeModal();
        }

        function editRow(button) {
            const tr = button.parentElement.parentElement;
            openModal('edit', tr);
        }

        function deleteRow(button) {
            if (confirm("คุณต้องการลบข้อมูลนักศึกษาคนนี้ใช่หรือไม่?")) {
                const tr = button.parentElement.parentElement;
                tr.remove();
                saveStudentsToStorage();
            }
        }

        function searchStudent() {
            let input = document.getElementById('searchInput');
            let filter = input.value.toLowerCase();
            let table = document.getElementById('studentTable');
            let tr = table.getElementsByTagName('tr');

            for (let i = 1; i < tr.length; i++) {
                let tdId = tr[i].getElementsByTagName('td')[0];
                let tdName = tr[i].getElementsByTagName('td')[1];
                if (tdId || tdName) {
                    let idText = tdId.textContent || tdId.innerText;
                    let nameText = tdName.textContent || tdName.innerText;
                    if (idText.toLowerCase().indexOf(filter) > -1 || nameText.toLowerCase().indexOf(filter) > -1) {
                        tr[i].style.display = "";
                    } else {
                        tr[i].style.display = "none";
                    }
                }
            }
        }

        document.getElementById('studentModal').addEventListener('click', function (event) {
            if (event.target === this) {
                closeModal();
            }
        });

        document.addEventListener('keydown', function (event) {
            if (event.key === 'Escape') {
                closeModal();
            }
        });

        loadStudentsFromStorage();
    </script>
</body>
</html>
