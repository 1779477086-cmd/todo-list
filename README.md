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
            overflow: hidden;
            position: relative;
            /* 关键：取消overflow:hidden，避免弹窗被裁剪 */
            overflow: visible !important;
        }

        .task-item:hover {
            box-shadow: 0 4px 12px rgba(22, 93, 255, 0.08);
        }

        /* 紧急度标记 - 优化视觉设计 */
        .task-priority {
            width: 8px;
            height: 100%;
            position: absolute;
            left: 0;
            top: 0;
        }
        /* 高紧急：红色渐变 */
        .priority-high {
            background: linear-gradient(90deg, #ff4d4f 0%, #ff7875 100%);
            box-shadow: 0 0 8px rgba(255, 77, 79, 0.2);
        }
        /* 中紧急：橙色渐变 */
        .priority-medium {
            background: linear-gradient(90deg, #faad14 0%, #ffc53d 100%);
            box-shadow: 0 0 8px rgba(250, 173, 20, 0.2);
        }
        /* 低紧急：绿色渐变 */
        .priority-low {
            background: linear-gradient(90deg, #52c41a 0%, #73d13d 100%);
            box-shadow: 0 0 8px rgba(82, 196, 26, 0.2);
        }

        .task-header {
            display: flex;
            align-items: center;
            padding: 14px 16px 14px 24px; /* 左边留空间给紧急度标记 */
            gap: 12px;
            position: relative; /* 作为弹窗的定位父级 */
            z-index: 1; /* 基础层级 */
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
            /* 防止输入时光标错位 */
            white-space: pre-wrap;
            word-break: break-word;
        }

        .completed .task-title {
            text-decoration: line-through;
            color: #999;
        }

        /* 时间选择模块 - 层级优化 */
        .task-time-picker {
            flex-shrink: 0;
            position: relative;
            width: 180px;
            z-index: 2; /* 高于任务头部 */
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
            transition: border-color 0.2s ease;
        }

        .time-display:hover {
            border-color: #165DFF;
        }

        /* 时间选择弹窗 - 置顶显示 */
        .time-picker-modal {
            position: absolute;
            top: calc(100% + 5px);
            left: 0;
            z-index: 9999 !important; /* 最高层级，确保不被遮挡 */
            width: 220px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 8px 24px rgba(0,0,0,0.15);
            padding: 15px;
            display: none;
            border: 1px solid #e0e7ff;
            /* 硬件加速防闪烁 */
            transform: translateZ(0);
        }

        .time-picker-modal.visible {
            display: block;
            animation: fadeIn 0.15s ease-in-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-5px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .date-input {
            width: 100%;
            padding: 8px;
            border: 1px solid #e0e7ff;
            border-radius: 6px;
            font-size: 14px;
            margin-bottom: 10px;
        }

        .hour-picker {
            width: 100%;
            padding: 8px;
            border: 1px solid #e0e7ff;
            border-radius: 6px;
            font-size: 14px;
            -webkit-appearance: menulist;
            appearance: menulist;
        }

        /* 紧急度选择框 - 样式优化 */
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

        /* 任务详情区域 - 优化层级 */
        .task-details {
            padding: 0 24px 16px; /* 左边留空间 */
            display: none;
            position: relative;
            z-index: 1; /* 低于时间弹窗 */
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
            /* 关键：防止输入闪退 */
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

        /* 全局遮罩层 - 稳定版 */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.01);
            z-index: 9998;
            display: none;
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
                width: 100%;
                z-index: 9999 !important;
            }
            .priority-select-container {
                width: 100%;
                margin-top: 8px;
            }
            .task-actions {
                margin-top: 8px;
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
        let isEditing = false; // 标记是否正在编辑，防止输入时重渲染

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

        // 加载任务（兼容旧数据）
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
            // 防抖保存，避免频繁操作
            if (isEditing) return;
            localStorage.setItem('todoTasks', JSON.stringify(tasks));
        }

        // 获取今日日期字符串
        function getTodayString() {
            const now = new Date();
            return `${now.getFullYear()}-${String(now.getMonth() + 1).padStart(2, '0')}-${String(now.getDate()).padStart(2, '0')}`;
        }

        // 添加新任务
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

        // 渲染所有任务（无闪烁版本）
        function renderTasks() {
            // 正在编辑时不渲染，防止输入闪退
            if (isEditing) return;
            
            // 保存当前弹窗状态
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
                                <input type="date" class="date-input" value="${task.date}" data-index="${index}">
                                <select class="hour-picker" data-index="${index}">
                                    ${Array.from({length:24}, (_, i) => `<option value="${String(i).padStart(2, '0')}" ${task.hour === String(i).padStart(2, '0') ? 'selected' : ''}>${i} 点</option>`).join('')}
                                </select>
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
            });

            // 恢复弹窗状态
            if (isModalOpen) openTimeModal(currentModalIndex);
        }

        // 仅更新剩余时间文本（不重渲染整个列表）
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

        // 启动剩余时间更新定时器
        function startDueTimeUpdate() {
            if (dueTimeUpdateTimer) clearInterval(dueTimeUpdateTimer);
            dueTimeUpdateTimer = setInterval(updateDueTimeText, 60000);
        }

        // 精确计算剩余时间
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

        // 更新统计数据
        function updateStats() {
            const completed = tasks.filter(t => t.completed).length;
            const pending = tasks.length - completed;
            completedCount.textContent = completed;
            pendingCount.textContent = pending;

            clearCompletedBtn.disabled = completed === 0;
            clearCompletedBtn.style.opacity = completed === 0 ? 0.6 : 1;
        }

        // 切换任务完成状态
        function toggleTask(index) {
            if (isEditing) return;
            tasks[index].completed = !tasks[index].completed;
            saveTasks();
            renderTasks();
            updateStats();
        }

        // 切换详情展开/收起
        function toggleDetails(index) {
            if (isEditing) return;
            tasks[index].isDetailsOpen = !tasks[index].isDetailsOpen;
            saveTasks();
            renderTasks();
        }

        // 删除任务
        function deleteTask(index) {
            if (isEditing) return;
            if (confirm('确定删除该任务？')) {
                tasks.splice(index, 1);
                saveTasks();
                renderTasks();
                updateStats();
            }
        }

        // 更新任务属性
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
            // 非编辑状态才渲染，防止输入闪退
            if (!isEditing) {
                renderTasks();
            }
        }

        // 清空已完成任务
        function clearCompletedTasks() {
            if (isEditing || !tasks.some(t => t.completed)) return;
            if (confirm('确定清空所有已完成任务？')) {
                tasks = tasks.filter(t => !t.completed);
                saveTasks();
                renderTasks();
                updateStats();
            }
        }

        // 打开时间选择弹窗
        function openTimeModal(index) {
            if (isEditing) return;
            closeAllTimeModals();
            
            const modal = document.getElementById(`timeModal-${index}`);
            if (modal) {
                modal.classList.add('visible');
                modalOverlay.classList.add('show');
                activeTimePickerIndex = index;
            }
        }

        // 关闭所有时间选择弹窗
        function closeAllTimeModals() {
            document.querySelectorAll('.time-picker-modal').forEach(modal => {
                modal.classList.remove('visible');
            });
            modalOverlay.classList.remove('show');
            activeTimePickerIndex = -1;
        }

        // 绑定所有事件
        function bindEvents() {
            // 添加任务
            addBtn.addEventListener('click', addTask);
            taskTitleInput.addEventListener('keypress', e => e.key === 'Enter' && addTask());

            // 清空已完成
            clearCompletedBtn.addEventListener('click', clearCompletedTasks);

            // 点击遮罩关闭弹窗
            modalOverlay.addEventListener('click', closeAllTimeModals);

            // 任务列表事件委托
            taskList.addEventListener('click', (e) => {
                if (isEditing) return;
                
                const idx = parseInt(e.target.dataset.index);
                
                // 时间显示框点击
                if (e.target.classList.contains('time-display')) {
                    const index = parseInt(e.target.id.split('-')[1]);
                    openTimeModal(index);
                    e.stopPropagation();
                    return;
                }

                if (isNaN(idx)) return;
                e.stopPropagation();

                // 切换完成状态
                if (e.target.classList.contains('task-checkbox')) {
                    toggleTask(idx);
                } else if (e.target.classList.contains('toggle-detail-btn')) {
                    toggleDetails(idx);
                } else if (e.target.classList.contains('delete-btn')) {
                    deleteTask(idx);
                }
            });

            // 标记编辑状态 - 开始编辑
            taskList.addEventListener('focus', (e) => {
                if (e.target.classList.contains('task-title') || e.target.classList.contains('detail-textarea')) {
                    isEditing = true;
                }
            }, { capture: true });

            // 标记编辑状态 - 结束编辑
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

            // 编辑详情/时间/紧急度
            taskList.addEventListener('change', (e) => {
                if (isEditing) return;
                
                const idx = parseInt(e.target.dataset.index);
                if (isNaN(idx)) return;

                if (e.target.classList.contains('date-input')) {
                    updateTaskProp(idx, 'date', e.target.value);
                    closeAllTimeModals();
                } else if (e.target.classList.contains('hour-picker')) {
                    updateTaskProp(idx, 'hour', e.target.value);
                    closeAllTimeModals();
                } else if (e.target.classList.contains('priority-select')) {
                    updateTaskProp(idx, 'priority', e.target.value);
                }
            });

            // 实时保存详情（不触发重渲染）
            taskList.addEventListener('input', (e) => {
                if (e.target.classList.contains('detail-textarea')) {
                    const idx = parseInt(e.target.dataset.index);
                    // 只更新数据，不渲染，防止输入闪退
                    tasks[idx].details = e.target.value;
                }
            });

            // 阻止弹窗内部事件冒泡
            document.addEventListener('click', (e) => {
                if (e.target.closest('.time-picker-modal') || e.target.closest('.priority-select-container')) {
                    e.stopImmediatePropagation();
                }
            }, { capture: true });
        }

        // 页面卸载时清除定时器
        window.addEventListener('beforeunload', () => {
            if (dueTimeUpdateTimer) clearInterval(dueTimeUpdateTimer);
        });

        // 初始化
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
