<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>我的待办清单</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #f0f7ff 0%, #f9f0ff 100%);
            color: #333;
            line-height: 1.6;
            padding: 20px;
            min-height: 100vh;
            overflow-y: auto !important; /* 确保页面始终可滚动 */
        }

        .container {
            max-width: 750px;
            margin: 0 auto;
            background: white;
            padding: 30px;
            border-radius: 16px;
            box-shadow: 0 8px 24px rgba(149, 157, 165, 0.1);
        }

        h1 {
            color: #165DFF;
            text-align: center;
            margin-bottom: 25px;
            font-weight: 600;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        h1::before {
            content: "📝";
            font-size: 28px;
        }

        .add-task {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        #task-title-input {
            flex: 1;
            min-width: 200px;
            padding: 12px 16px;
            border: 2px solid #e0e7ff;
            border-radius: 8px;
            font-size: 16px;
            transition: border-color 0.3s ease;
        }

        #task-title-input:focus {
            outline: none;
            border-color: #165DFF;
            box-shadow: 0 0 0 3px rgba(22, 93, 255, 0.1);
        }

        #add-btn {
            padding: 12px 24px;
            background-color: #165DFF;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 500;
            transition: background-color 0.3s ease, transform 0.2s ease;
            white-space: nowrap;
        }

        #add-btn:hover {
            background-color: #0040c9;
            transform: translateY(-2px);
        }

        .stats-container {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding: 10px 16px;
            background-color: #f5f8ff;
            border-radius: 8px;
            font-size: 14px;
            color: #444;
        }

        .stats {
            display: flex;
            gap: 20px;
        }

        .stats span {
            font-weight: 500;
            color: #165DFF;
        }

        #clear-completed {
            padding: 6px 12px;
            background-color: #722ED1;
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 14px;
            transition: background-color 0.3s ease;
        }

        #clear-completed:hover {
            background-color: #5a23a8;
        }

        #task-list {
            list-style: none;
            margin-top: 10px;
        }

        .task-item {
            margin-bottom: 15px;
            background-color: #fff;
            border-radius: 12px;
            border: 1px solid #e8f0fe;
            transition: box-shadow 0.3s ease;
            position: relative;
            overflow: visible !important; /* 关键：确保弹窗不被裁剪 */
        }

        .task-item:hover {
            box-shadow: 0 4px 12px rgba(22, 93, 255, 0.08);
        }

        /* 紧急度标记 */
        .task-priority {
            width: 8px;
            height: 100%;
            position: absolute;
            left: 0;
            top: 0;
        }
        .priority-high {
            background: linear-gradient(90deg, #ff4d4f 0%, #ff7875 100%);
            box-shadow: 0 0 8px rgba(255, 77, 79, 0.2);
        }
        .priority-medium {
            background: linear-gradient(90deg, #faad14 0%, #ffc53d 100%);
            box-shadow: 0 0 8px rgba(250, 173, 20, 0.2);
        }
        .priority-low {
            background: linear-gradient(90deg, #52c41a 0%, #73d13d 100%);
            box-shadow: 0 0 8px rgba(82, 196, 26, 0.2);
        }

        .task-header {
            display: flex;
            align-items: center;
            padding: 14px 16px 14px 24px;
            gap: 12px;
            position: relative;
            z-index: 1;
        }

        .task-checkbox {
            width: 20px;
            height: 20px;
            accent-color: #165DFF;
            cursor: pointer;
            flex-shrink: 0;
        }

        .task-title {
            flex: 1;
            font-size: 16px;
            outline: none;
            min-width: 0;
            white-space: pre-wrap;
            word-break: break-word;
        }

        .completed .task-title {
            text-decoration: line-through;
            color: #999;
        }

        /* 时间选择模块 - 完全置顶 */
        .task-time-picker {
            flex-shrink: 0;
            position: relative;
            width: 180px;
            z-index: 1000; /* 高于所有内容 */
        }

        .time-display {
            width: 100%;
            padding: 8px 12px;
            border: 1px solid #e0e7ff;
            border-radius: 8px;
            font-size: 14px;
            color: #555;
            background: #f9fbfd;
            cursor: pointer;
            text-align: center;
            user-select: none;
            transition: all 0.2s ease;
        }

        .time-display:hover {
            border-color: #165DFF;
            background: #f0f7ff;
        }

        /* 时间选择弹窗 - 全局置顶（关键修复） */
        .time-picker-modal {
            position: fixed !important; /* 固定定位，不受滚动影响 */
            z-index: 99999 !important; /* 超高层级，确保置顶 */
            width: 320px;
            background: white;
            border-radius: 12px;
            box-shadow: 0 12px 32px rgba(0,0,0,0.15);
            padding: 20px;
            display: none;
            border: 1px solid #e0e7ff;
            transform: translateZ(0); /* 硬件加速 */
            /* 防止滚动时弹窗抖动 */
            will-change: top, left;
            backface-visibility: hidden;
        }

        .time-picker-modal.visible {
            display: block;
            animation: fadeIn 0.2s ease-in-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* 日历样式 */
        .calendar-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .calendar-nav {
            display: flex;
            gap: 10px;
        }

        .calendar-nav-btn {
            width: 32px;
            height: 32px;
            border-radius: 50%;
            border: none;
            background: #f0f7ff;
            color: #165DFF;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.2s ease;
        }

        .calendar-nav-btn:hover {
            background: #165DFF;
            color: white;
        }

        .calendar-month {
            font-size: 16px;
            font-weight: 600;
            color: #165DFF;
        }

        .calendar-week {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            text-align: center;
            margin-bottom: 8px;
        }

        .calendar-week-day {
            font-size: 12px;
            color: #666;
            padding: 6px 0;
        }

        .calendar-days {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 4px;
        }

        .calendar-day {
            aspect-ratio: 1/1;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px;
            transition: all 0.2s ease;
            position: relative;
        }

        .calendar-day:hover {
            background: #f0f7ff;
        }

        .calendar-day.today {
            background: #165DFF;
            color: white;
            font-weight: 600;
        }

        .calendar-day.selected {
            background: #722ED1;
            color: white;
        }

        .calendar-day.weekend {
            color: #ff4d4f;
        }

        .calendar-day.holiday {
            background: #fff1f0;
            color: #ff4d4f;
            font-weight: 500;
        }

        .calendar-day.holiday::after {
            content: "休";
            position: absolute;
            font-size: 8px;
            bottom: 2px;
            right: 2px;
        }

        .calendar-day.disabled {
            color: #ccc;
            cursor: not-allowed;
            background: #fafafa;
        }

        /* 小时选择器 */
        .hour-selector {
            margin-top: 15px;
        }

        .hour-selector-title {
            font-size: 14px;
            color: #666;
            margin-bottom: 8px;
        }

        .hour-options {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            gap: 6px;
        }

        .hour-option {
            padding: 6px 0;
            text-align: center;
            border-radius: 6px;
            border: 1px solid #e0e7ff;
            cursor: pointer;
            font-size: 14px;
            transition: all 0.2s ease;
        }

        .hour-option:hover {
            border-color: #165DFF;
            background: #f0f7ff;
        }

        .hour-option.selected {
            background: #165DFF;
            color: white;
            border-color: #165DFF;
        }

        /* 确认按钮 */
        .time-confirm-btn {
            width: 100%;
            padding: 10px;
            background: #165DFF;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 500;
            cursor: pointer;
            margin-top: 15px;
            transition: all 0.2s ease;
        }

        .time-confirm-btn:hover {
            background: #0040c9;
        }

        .priority-select-container {
            flex-shrink: 0;
            position: relative;
            width: 80px;
            z-index: 2;
        }

        .priority-select {
            width: 100%;
            padding: 4px 8px;
            border: 1px solid #e0e7ff;
            border-radius: 6px;
            font-size: 12px;
            cursor: pointer;
            background: #f9fbfd;
            -webkit-appearance: menulist;
            appearance: menulist;
            transition: border-color 0.2s ease;
        }

        .priority-select:hover {
            border-color: #165DFF;
        }

        .task-actions {
            display: flex;
            gap: 8px;
            flex-shrink: 0;
            z-index: 2;
        }

        .toggle-detail-btn, .delete-btn {
            padding: 4px 8px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 12px;
            transition: background-color 0.2s ease;
        }

        .toggle-detail-btn {
            background-color: #e0e7ff;
            color: #165DFF;
        }

        .toggle-detail-btn:hover {
            background-color: #d1dfff;
        }

        .delete-btn {
            background-color: #ff4d4f;
            color: white;
        }

        .delete-btn:hover {
            background-color: #d9363e;
        }

        .task-details {
            padding: 0 24px 16px;
            display: none;
            position: relative;
            z-index: 1;
        }

        .task-details.active {
            display: block;
            animation: slideDown 0.2s ease-in-out;
        }

        @keyframes slideDown {
            from { opacity: 0; max-height: 0; }
            to { opacity: 1; max-height: 500px; }
        }

        .detail-textarea {
            width: 100%;
            min-height: 80px;
            padding: 10px;
            border: 1px solid #e0e7ff;
            border-radius: 8px;
            font-size: 14px;
            resize: vertical;
            outline: none;
            font-family: inherit;
            white-space: pre-wrap;
            word-break: break-word;
            line-height: 1.5;
            transition: border-color 0.2s ease;
        }

        .detail-textarea:focus {
            border-color: #165DFF;
            box-shadow: 0 0 0 3px rgba(22, 93, 255, 0.08);
        }

        .task-due {
            font-size: 12px;
            color: #888;
            margin-top: 8px;
            text-align: right;
        }
        .task-due.overdue {
            color: #ff4d4f;
            font-weight: 500;
        }

        .empty-state {
            text-align: center;
            padding: 40px 20px;
            color: #999;
            font-size: 16px;
        }

        /* 全局遮罩层（不阻止页面滚动） */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.1);
            z-index: 99998;
            display: none;
            pointer-events: none; /* 允许鼠标事件穿透到下层 */
        }

        .modal-overlay.show {
            display: block;
        }

        /* 移动端适配 */
        @media (max-width: 480px) {
            .add-task {
                flex-direction: column;
            }
            #add-btn {
                width: 100%;
            }
            .stats-container {
                flex-direction: column;
                gap: 10px;
                align-items: flex-start;
            }
            .stats {
                flex-direction: column;
                gap: 5px;
            }
            #clear-completed {
                align-self: stretch;
                text-align: center;
            }
            .task-header {
                flex-wrap: wrap;
                padding: 14px 16px 14px 24px;
            }
            .task-time-picker {
                width: 100%;
                margin-top: 8px;
            }
            .time-picker-modal {
                width: 90%;
                left: 5% !important;
                right: 5% !important;
            }
            .priority-select-container {
                width: 100%;
                margin-top: 8px;
            }
            .task-actions {
                margin-top: 8px;
            }
            .hour-options {
                grid-template-columns: repeat(4, 1fr);
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>我的待办清单</h1>

        <div class="add-task">
            <input type="text" id="task-title-input" placeholder="输入任务标题...">
            <button id="add-btn">➕ 添加任务</button>
        </div>

        <div class="stats-container">
            <div class="stats">
                <div>已完成: <span id="completed-count">0</span></div>
                <div>未完成: <span id="pending-count">0</span></div>
            </div>
            <button id="clear-completed">清空已完成</button>
        </div>

        <ul id="task-list"></ul>
    </div>

    <!-- 全局遮罩层 -->
    <div class="modal-overlay" id="modalOverlay"></div>

    <script>
        let tasks = [];
        let activeTimePickerIndex = -1;
        let dueTimeUpdateTimer = null;
        let isEditing = false;
        // 存储当前选中的日期和小时（全局）
        let calendarSelections = {};
        
        // 节假日数据（2025年法定节假日）
        const holidays = {
            "2025-01-01": "元旦",
            "2025-01-29": "春节",
            "2025-01-30": "春节",
            "2025-01-31": "春节",
            "2025-02-01": "春节",
            "2025-02-02": "春节",
            "2025-02-03": "春节",
            "2025-02-04": "春节",
            "2025-04-05": "清明节",
            "2025-05-01": "劳动节",
            "2025-05-02": "劳动节",
            "2025-05-03": "劳动节",
            "2025-05-04": "劳动节",
            "2025-05-05": "劳动节",
            "2025-06-02": "端午节",
            "2025-09-07": "中秋节",
            "2025-10-01": "国庆节",
            "2025-10-02": "国庆节",
            "2025-10-03": "国庆节",
            "2025-10-04": "国庆节",
            "2025-10-05": "国庆节",
            "2025-10-06": "国庆节",
            "2025-10-07": "国庆节"
        };

        const taskTitleInput = document.getElementById('task-title-input');
        const addBtn = document.getElementById('add-btn');
        const taskList = document.getElementById('task-list');
        const completedCount = document.getElementById('completed-count');
        const pendingCount = document.getElementById('pending-count');
        const clearCompletedBtn = document.getElementById('clear-completed');
        const modalOverlay = document.getElementById('modalOverlay');

        function init() {
            loadTasks();
            renderTasks();
            updateStats();
            bindEvents();
            startDueTimeUpdate();
        }

        function loadTasks() {
            try {
                const saved = localStorage.getItem('todoTasks');
                if (saved) {
                    tasks = JSON.parse(saved).map(task => ({
                        ...task,
                        title: task.title || task.content || '',
                        details: task.details || '',
                        hour: task.hour ?? '23',
                        priority: task.priority ?? 'medium'
                    }));
                }
            } catch (e) {
                console.error('加载任务失败', e);
                tasks = [];
            }
        }

        function saveTasks() {
            if (isEditing) return;
            localStorage.setItem('todoTasks', JSON.stringify(tasks));
        }

        function getTodayString() {
            const now = new Date();
            return `${now.getFullYear()}-${String(now.getMonth() + 1).padStart(2, '0')}-${String(now.getDate()).padStart(2, '0')}`;
        }

        function addTask() {
            const title = taskTitleInput.value.trim();
            if (!title) {
                alert('请输入任务标题');
                return;
            }

            tasks.push({
                title: title,
                details: '',
                completed: false,
                date: getTodayString(),
                hour: '23',
                priority: 'medium',
                isDetailsOpen: false
            });

            saveTasks();
            renderTasks();
            updateStats();
            taskTitleInput.value = '';
            taskTitleInput.focus();
        }

        function renderTasks() {
            if (isEditing) return;
            
            const isModalOpen = activeTimePickerIndex !== -1;
            const currentModalIndex = activeTimePickerIndex;

            taskList.innerHTML = '';
            
            if (tasks.length === 0) {
                taskList.innerHTML = '<li class="empty-state">暂无任务，添加一个开始吧！</li>';
                if (isModalOpen) openTimeModal(currentModalIndex);
                return;
            }

            tasks.forEach((task, index) => {
                const li = document.createElement('li');
                li.className = `task-item ${task.completed ? 'completed' : ''}`;
                
                const { dueText, isOverdue } = calcRemaining(task);
                
                li.innerHTML = `
                    <div class="task-priority priority-${task.priority}"></div>
                    <div class="task-header">
                        <input type="checkbox" class="task-checkbox" ${task.completed ? 'checked' : ''} data-index="${index}">
                        <div class="task-title" contenteditable="true" data-index="${index}">${task.title}</div>
                        <div class="task-time-picker" data-index="${index}">
                            <div class="time-display" id="timeDisplay-${index}">${task.date} ${task.hour}:00</div>
                            <div class="time-picker-modal" id="timeModal-${index}">
                                <div class="calendar-header">
                                    <div class="calendar-nav">
                                        <button class="calendar-nav-btn prev-month" data-index="${index}">◀</button>
                                        <button class="calendar-nav-btn next-month" data-index="${index}">▶</button>
                                    </div>
                                    <div class="calendar-month" id="calendarMonth-${index}">2025年1月</div>
                                </div>
                                <div class="calendar-week">
                                    <div class="calendar-week-day">日</div>
                                    <div class="calendar-week-day">一</div>
                                    <div class="calendar-week-day">二</div>
                                    <div class="calendar-week-day">三</div>
                                    <div class="calendar-week-day">四</div>
                                    <div class="calendar-week-day">五</div>
                                    <div class="calendar-week-day">六</div>
                                </div>
                                <div class="calendar-days" id="calendarDays-${index}"></div>
                                <div class="hour-selector">
                                    <div class="hour-selector-title">选择小时</div>
                                    <div class="hour-options" id="hourOptions-${index}"></div>
                                </div>
                                <button class="time-confirm-btn" data-index="${index}">确认选择</button>
                            </div>
                        </div>
                        <div class="priority-select-container">
                            <select class="priority-select" data-index="${index}">
                                <option value="high" ${task.priority === 'high' ? 'selected' : ''}>高紧急</option>
                                <option value="medium" ${task.priority === 'medium' ? 'selected' : ''}>中紧急</option>
                                <option value="low" ${task.priority === 'low' ? 'selected' : ''}>低紧急</option>
                            </select>
                        </div>
                        <div class="task-actions">
                            <button class="toggle-detail-btn" data-index="${index}">${task.isDetailsOpen ? '收起' : '详情'}</button>
                            <button class="delete-btn" data-index="${index}">删除</button>
                        </div>
                    </div>
                    <div class="task-details ${task.isDetailsOpen ? 'active' : ''}" data-index="${index}">
                        <textarea class="detail-textarea" data-index="${index}">${task.details}</textarea>
                        <div class="task-due ${isOverdue ? 'overdue' : ''}" id="dueText-${index}">${dueText}</div>
                    </div>
                `;
                
                taskList.appendChild(li);
                
                // 初始化日历
                initCalendar(index, task.date, task.hour);
            });

            if (isModalOpen) openTimeModal(currentModalIndex);
        }

        // 初始化日历（修复选择逻辑）
        function initCalendar(index, selectedDate, selectedHour) {
            const today = new Date();
            let currentDate = selectedDate ? new Date(selectedDate) : today;
            let currentYear = currentDate.getFullYear();
            let currentMonth = currentDate.getMonth();
            
            // 初始化当前选择状态
            if (!calendarSelections[index]) {
                calendarSelections[index] = {
                    date: selectedDate ? new Date(selectedDate) : today,
                    hour: selectedHour || '23'
                };
            }
            
            const selection = calendarSelections[index];
            
            // 更新月份显示
            document.getElementById(`calendarMonth-${index}`).textContent = `${currentYear}年${currentMonth + 1}月`;
            
            // 生成日历天数
            generateCalendarDays(index, currentYear, currentMonth, selection.date);
            
            // 生成小时选项
            generateHourOptions(index, selection.hour);
            
            // 绑定月份切换事件（修复闭包问题）
            const prevBtn = document.querySelector(`.prev-month[data-index="${index}"]`);
            const nextBtn = document.querySelector(`.next-month[data-index="${index}"]`);
            
            // 移除旧事件，防止重复绑定
            prevBtn.removeEventListener('click', prevBtn.clickHandler);
            nextBtn.removeEventListener('click', nextBtn.clickHandler);
            
            prevBtn.clickHandler = function() {
                currentMonth--;
                if (currentMonth < 0) {
                    currentMonth = 11;
                    currentYear--;
                }
                document.getElementById(`calendarMonth-${index}`).textContent = `${currentYear}年${currentMonth + 1}月`;
                generateCalendarDays(index, currentYear, currentMonth, selection.date);
            };
            
            nextBtn.clickHandler = function() {
                currentMonth++;
                if (currentMonth > 11) {
                    currentMonth = 0;
                    currentYear++;
                }
                document.getElementById(`calendarMonth-${index}`).textContent = `${currentYear}年${currentMonth + 1}月`;
                generateCalendarDays(index, currentYear, currentMonth, selection.date);
            };
            
            prevBtn.addEventListener('click', prevBtn.clickHandler);
            nextBtn.addEventListener('click', nextBtn.clickHandler);
            
            // 绑定日期选择事件（修复选择逻辑）
            const calendarDaysEl = document.getElementById(`calendarDays-${index}`);
            calendarDaysEl.removeEventListener('click', calendarDaysEl.clickHandler);
            
            calendarDaysEl.clickHandler = function(e) {
                const dayEl = e.target.closest('.calendar-day');
                if (!dayEl || dayEl.classList.contains('disabled')) return;
                
                // 移除之前的选中状态
                this.querySelectorAll('.calendar-day').forEach(el => {
                    el.classList.remove('selected');
                });
                
                // 添加新的选中状态
                dayEl.classList.add('selected');
                
                // 更新选中日期
                const day = parseInt(dayEl.dataset.day);
                selection.date = new Date(currentYear, currentMonth, day);
            };
            
            calendarDaysEl.addEventListener('click', calendarDaysEl.clickHandler);
            
            // 绑定小时选择事件（修复选择逻辑）
            const hourOptionsEl = document.getElementById(`hourOptions-${index}`);
            hourOptionsEl.removeEventListener('click', hourOptionsEl.clickHandler);
            
            hourOptionsEl.clickHandler = function(e) {
                const hourEl = e.target.closest('.hour-option');
                if (!hourEl) return;
                
                // 移除之前的选中状态
                this.querySelectorAll('.hour-option').forEach(el => {
                    el.classList.remove('selected');
                });
                
                // 添加新的选中状态
                hourEl.classList.add('selected');
                selection.hour = hourEl.dataset.hour;
            };
            
            hourOptionsEl.addEventListener('click', hourOptionsEl.clickHandler);
            
            // 绑定确认按钮事件（修复数据更新）
            const confirmBtn = document.querySelector(`.time-confirm-btn[data-index="${index}"]`);
            confirmBtn.removeEventListener('click', confirmBtn.clickHandler);
            
            confirmBtn.clickHandler = function() {
                // 格式化选中的日期
                const formattedDate = `${selection.date.getFullYear()}-${String(selection.date.getMonth() + 1).padStart(2, '0')}-${String(selection.date.getDate()).padStart(2, '0')}`;
                
                // 更新任务时间
                tasks[index].date = formattedDate;
                tasks[index].hour = selection.hour;
                
                // 立即保存并更新显示
                saveTasks();
                document.getElementById(`timeDisplay-${index}`).textContent = `${formattedDate} ${selection.hour}:00`;
                
                // 更新剩余时间显示
                updateDueTimeText();
                updateStats();
                
                // 关闭弹窗
                closeAllTimeModals();
            };
            
            confirmBtn.addEventListener('click', confirmBtn.clickHandler);
        }

        // 生成日历天数（优化日期判断）
        function generateCalendarDays(index, year, month, selectedDate) {
            const calendarDaysEl = document.getElementById(`calendarDays-${index}`);
            calendarDaysEl.innerHTML = '';
            
            const today = new Date();
            const todayDate = new Date(today.getFullYear(), today.getMonth(), today.getDate());
            const firstDay = new Date(year, month, 1);
            const lastDay = new Date(year, month + 1, 0);
            const firstDayOfWeek = firstDay.getDay(); // 0-6，0是周日
            
            // 添加月初的空白天
            for (let i = 0; i < firstDayOfWeek; i++) {
                const emptyDay = document.createElement('div');
                emptyDay.className = 'calendar-day disabled';
                calendarDaysEl.appendChild(emptyDay);
            }
            
            // 添加当月的天数
            for (let day = 1; day <= lastDay.getDate(); day++) {
                const dayEl = document.createElement('div');
                dayEl.className = 'calendar-day';
                dayEl.dataset.day = day;
                dayEl.textContent = day;
                
                const currentDateStr = `${year}-${String(month + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
                const dayDate = new Date(year, month, day);
                
                // 标记今天
                if (dayDate.getTime() === todayDate.getTime()) {
                    dayEl.classList.add('today');
                }
                
                // 标记选中的日期
                if (dayDate.getTime() === selectedDate.getTime()) {
                    dayEl.classList.add('selected');
                }
                
                // 标记周末
                const dayOfWeek = dayDate.getDay();
                if (dayOfWeek === 0 || dayOfWeek === 6) {
                    dayEl.classList.add('weekend');
                }
                
                // 标记节假日
                if (holidays[currentDateStr]) {
                    dayEl.classList.add('holiday');
                }
                
                // 禁用过去的日期（除了今天）
                if (dayDate < todayDate) {
                    dayEl.classList.add('disabled');
                }
                
                calendarDaysEl.appendChild(dayEl);
            }
        }

        // 生成小时选项（优化选中状态）
        function generateHourOptions(index, selectedHour) {
            const hourOptionsEl = document.getElementById(`hourOptions-${index}`);
            hourOptionsEl.innerHTML = '';
            
            for (let hour = 0; hour < 24; hour++) {
                const hourStr = String(hour).padStart(2, '0');
                const hourEl = document.createElement('div');
                hourEl.className = `hour-option ${hourStr === selectedHour ? 'selected' : ''}`;
                hourEl.dataset.hour = hourStr;
                hourEl.textContent = `${hourStr}:00`;
                hourOptionsEl.appendChild(hourEl);
            }
        }

        function updateDueTimeText() {
            if (tasks.length === 0 || isEditing) return;
            
            tasks.forEach((task, index) => {
                const dueTextEl = document.getElementById(`dueText-${index}`);
                if (!dueTextEl) return;
                
                const { dueText, isOverdue } = calcRemaining(task);
                dueTextEl.textContent = dueText;
                dueTextEl.className = `task-due ${isOverdue ? 'overdue' : ''}`;
            });
        }

        function startDueTimeUpdate() {
            if (dueTimeUpdateTimer) clearInterval(dueTimeUpdateTimer);
            dueTimeUpdateTimer = setInterval(updateDueTimeText, 60000);
        }

        function calcRemaining(task) {
            const now = new Date();
            const target = new Date(`${task.date} ${task.hour}:00:00`);
            const diffMs = target - now;
            const isOverdue = diffMs < 0;

            if (isOverdue) {
                const absDiffMs = Math.abs(diffMs);
                const days = Math.floor(absDiffMs / (1000 * 60 * 60 * 24));
                const hours = Math.floor((absDiffMs % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                
                let overdueText = '已逾期';
                if (days > 0) overdueText += ` ${days}天`;
                if (hours > 0) overdueText += ` ${hours}小时`;
                return { dueText: overdueText, isOverdue: true };
            }

            const days = Math.floor(diffMs / (1000 * 60 * 60 * 24));
            const hours = Math.floor((diffMs % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));

            let dueText = '';
            if (days === 0) {
                dueText = hours > 0 ? `剩 ${hours} 小时` : '剩不到1小时';
            } else {
                dueText = `剩 ${days}天${hours > 0 ? ` ${hours}小时` : ''}`;
            }

            return { dueText, isOverdue: false };
        }

        function updateStats() {
            const completed = tasks.filter(t => t.completed).length;
            const pending = tasks.length - completed;
            completedCount.textContent = completed;
            pendingCount.textContent = pending;

            clearCompletedBtn.disabled = completed === 0;
            clearCompletedBtn.style.opacity = completed === 0 ? 0.6 : 1;
        }

        function toggleTask(index) {
            if (isEditing) return;
            tasks[index].completed = !tasks[index].completed;
            saveTasks();
            renderTasks();
            updateStats();
        }

        function toggleDetails(index) {
            if (isEditing) return;
            tasks[index].isDetailsOpen = !tasks[index].isDetailsOpen;
            saveTasks();
            renderTasks();
        }

        function deleteTask(index) {
            if (isEditing) return;
            if (confirm('确定删除该任务？')) {
                tasks.splice(index, 1);
                // 清理选择状态
                delete calendarSelections[index];
                saveTasks();
                renderTasks();
                updateStats();
            }
        }

        function updateTaskProp(index, type, value) {
            if (isEditing && (type === 'title' || type === 'details')) return;
            
            switch(type) {
                case 'title':
                    tasks[index].title = value.trim();
                    break;
                case 'details':
                    tasks[index].details = value;
                    break;
                case 'date':
                    tasks[index].date = value;
                    break;
                case 'hour':
                    tasks[index].hour = value.padStart(2, '0');
                    break;
                case 'priority':
                    tasks[index].priority = value;
                    break;
            }
            saveTasks();
            if (!isEditing) {
                renderTasks();
            }
        }

        function clearCompletedTasks() {
            if (isEditing || !tasks.some(t => t.completed)) return;
            if (confirm('确定清空所有已完成任务？')) {
                tasks = tasks.filter(t => !t.completed);
                // 清理选择状态
                calendarSelections = {};
                saveTasks();
                renderTasks();
                updateStats();
            }
        }

        // 打开时间选择弹窗（优化定位，滚动时保持位置）
        function openTimeModal(index) {
            if (isEditing) return;
            closeAllTimeModals();
            
            const modal = document.getElementById(`timeModal-${index}`);
            const timeDisplay = document.getElementById(`timeDisplay-${index}`);
            
            if (modal && timeDisplay) {
                // 获取时间显示框的位置（相对于视口）
                const rect = timeDisplay.getBoundingClientRect();
                
                // 设置弹窗位置（固定定位，基于视口）
                modal.style.top = `${rect.bottom + 10}px`;
                modal.style.left = `${rect.left}px`;
                
                // 确保弹窗不超出视口右侧
                if (rect.left + 320 > window.innerWidth) {
                    modal.style.left = `${window.innerWidth - 330}px`;
                }
                
                // 确保弹窗不超出视口底部
                if (rect.bottom + 10 + modal.offsetHeight > window.innerHeight) {
                    modal.style.top = `${rect.top - modal.offsetHeight - 10}px`;
                }
                
                modal.classList.add('visible');
                modalOverlay.classList.add('show');
                activeTimePickerIndex = index;
                
                // 重新初始化日历选择状态
                if (calendarSelections[index]) {
                    const selection = calendarSelections[index];
                    const currentDate = selection.date;
                    generateCalendarDays(index, currentDate.getFullYear(), currentDate.getMonth(), currentDate);
                    generateHourOptions(index, selection.hour);
                }
            }
        }

        function closeAllTimeModals() {
            document.querySelectorAll('.time-picker-modal').forEach(modal => {
                modal.classList.remove('visible');
            });
            modalOverlay.classList.remove('show');
            activeTimePickerIndex = -1;
        }

        function bindEvents() {
            addBtn.addEventListener('click', addTask);
            taskTitleInput.addEventListener('keypress', e => e.key === 'Enter' && addTask());

            clearCompletedBtn.addEventListener('click', clearCompletedTasks);

            // 遮罩层点击关闭弹窗，但不阻止滚动
            modalOverlay.addEventListener('click', function(e) {
                e.stopPropagation();
                closeAllTimeModals();
            });

            // 任务列表点击事件
            taskList.addEventListener('click', (e) => {
                if (isEditing) return;
                
                // 阻止事件冒泡到遮罩层
                e.stopPropagation();
                
                const idx = parseInt(e.target.dataset.index);
                
                // 点击时间显示框打开日历
                if (e.target.classList.contains('time-display')) {
                    const index = parseInt(e.target.id.split('-')[1]);
                    openTimeModal(index);
                    return;
                }

                if (isNaN(idx)) return;

                if (e.target.classList.contains('task-checkbox')) {
                    toggleTask(idx);
                } else if (e.target.classList.contains('toggle-detail-btn')) {
                    toggleDetails(idx);
                } else if (e.target.classList.contains('delete-btn')) {
                    deleteTask(idx);
                }
            });

            // 编辑状态管理
            taskList.addEventListener('focus', (e) => {
                if (e.target.classList.contains('task-title') || e.target.classList.contains('detail-textarea')) {
                    isEditing = true;
                }
            }, { capture: true });

            taskList.addEventListener('blur', (e) => {
                if (e.target.classList.contains('task-title')) {
                    const idx = parseInt(e.target.dataset.index);
                    updateTaskProp(idx, 'title', e.target.textContent);
                    isEditing = false;
                } else if (e.target.classList.contains('detail-textarea')) {
                    const idx = parseInt(e.target.dataset.index);
                    updateTaskProp(idx, 'details', e.target.value);
                    isEditing = false;
                }
            }, { capture: true });

            // 优先级选择
            taskList.addEventListener('change', (e) => {
                if (isEditing) return;
                
                const idx = parseInt(e.target.dataset.index);
                if (isNaN(idx)) return;

                if (e.target.classList.contains('priority-select')) {
                    updateTaskProp(idx, 'priority', e.target.value);
                }
            });

            // 详情输入实时更新
            taskList.addEventListener('input', (e) => {
                if (e.target.classList.contains('detail-textarea')) {
                    const idx = parseInt(e.target.dataset.index);
                    tasks[idx].details = e.target.value;
                }
            });

            // 阻止日历内点击关闭弹窗
            document.addEventListener('click', (e) => {
                if (e.target.closest('.time-picker-modal')) {
                    e.stopImmediatePropagation();
                }
            }, { capture: true });
            
            // 窗口滚动时重新定位弹窗（保持位置固定）
            window.addEventListener('scroll', () => {
                if (activeTimePickerIndex !== -1) {
                    openTimeModal(activeTimePickerIndex);
                }
            });
            
            // 窗口大小变化时重新定位弹窗
            window.addEventListener('resize', () => {
                if (activeTimePickerIndex !== -1) {
                    openTimeModal(activeTimePickerIndex);
                }
            });
        }

        // 页面卸载时清理定时器
        window.addEventListener('beforeunload', () => {
            if (dueTimeUpdateTimer) clearInterval(dueTimeUpdateTimer);
        });

        // 初始化
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
