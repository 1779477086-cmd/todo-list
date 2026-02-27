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
        }

        .task-item:hover {
            box-shadow: 0 4px 12px rgba(22, 93, 255, 0.08);
        }

        /* 紧急度标记 */
        .task-priority {
            width: 8px;
            height: 100%;
            float: left;
        }
        .priority-high {
            background-color: #ff4d4f;
        }
        .priority-medium {
            background-color: #faad14;
        }
        .priority-low {
            background-color: #52c41a;
        }

        .task-header {
            display: flex;
            align-items: center;
            padding: 14px 16px;
            gap: 12px;
            cursor: pointer;
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
        }

        .completed .task-title {
            text-decoration: line-through;
            color: #999;
        }

        /* 放大的时间选择模块 */
        .task-time-picker {
            flex-shrink: 0;
            position: relative;
            width: 180px;
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
        }

        /* 时间选择弹窗 */
        .time-picker-modal {
            position: absolute;
            top: 100%;
            left: 0;
            z-index: 100;
            width: 220px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 4px 16px rgba(0,0,0,0.1);
            padding: 15px;
            display: none;
        }

        .task-time-picker:hover .time-picker-modal,
        .time-picker-modal.active {
            display: block;
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
            /* 滚轮样式 */
            -webkit-appearance: none;
            appearance: none;
        }

        .priority-select {
            flex-shrink: 0;
            width: 80px;
            padding: 4px 8px;
            border: 1px solid #e0e7ff;
            border-radius: 6px;
            font-size: 12px;
            cursor: pointer;
        }

        .task-actions {
            display: flex;
            gap: 8px;
            flex-shrink: 0;
        }

        .toggle-detail-btn, .delete-btn {
            padding: 4px 8px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 12px;
        }

        .toggle-detail-btn {
            background-color: #e0e7ff;
            color: #165DFF;
        }

        .delete-btn {
            background-color: #ff4d4f;
            color: white;
        }

        .delete-btn:hover {
            background-color: #d9363e;
        }

        /* 任务详情区域 */
        .task-details {
            padding: 0 16px 16px;
            display: none;
        }

        .task-details.active {
            display: block;
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
            }
            .task-time-picker {
                width: 100%;
                margin-top: 8px;
            }
            .time-picker-modal {
                width: 100%;
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

    <script>
        let tasks = [];

        const taskTitleInput = document.getElementById('task-title-input');
        const addBtn = document.getElementById('add-btn');
        const taskList = document.getElementById('task-list');
        const completedCount = document.getElementById('completed-count');
        const pendingCount = document.getElementById('pending-count');
        const clearCompletedBtn = document.getElementById('clear-completed');

        function init() {
            loadTasks();
            renderTasks();
            updateStats();
            bindEvents();
            setInterval(renderTasks, 1000); // 实时更新剩余时间
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
                        priority: task.priority ?? 'medium' // 默认中紧急度
                    }));
                }
            } catch (e) {
                console.error('加载任务失败', e);
                tasks = [];
            }
        }

        function saveTasks() {
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
                isDetailsOpen: false // 详情是否展开
            });

            saveTasks();
            renderTasks();
            updateStats();
            taskTitleInput.value = '';
            taskTitleInput.focus();
        }

        // 渲染所有任务
        function renderTasks() {
            taskList.innerHTML = '';
            
            if (tasks.length === 0) {
                taskList.innerHTML = '<li class="empty-state">暂无任务，添加一个开始吧！</li>';
                return;
            }

            tasks.forEach((task, index) => {
                const li = document.createElement('li');
                li.className = `task-item ${task.completed ? 'completed' : ''}`;
                
                // 计算剩余时间
                const { dueText, isOverdue } = calcRemaining(task);
                
                // 拼接任务HTML（含紧急度、标题、时间、详情、操作）
                li.innerHTML = `
                    <!-- 紧急度标记 -->
                    <div class="task-priority priority-${task.priority}"></div>
                    
                    <!-- 任务头部（标题/时间/操作） -->
                    <div class="task-header">
                        <input type="checkbox" class="task-checkbox" ${task.completed ? 'checked' : ''} data-index="${index}">
                        <div class="task-title" contenteditable="true" data-index="${index}">${task.title}</div>
                        
                        <!-- 放大的时间选择器（日期+小时合一） -->
                        <div class="task-time-picker" data-index="${index}">
                            <div class="time-display">${task.date} ${task.hour}:00</div>
                            <div class="time-picker-modal">
                                <input type="date" class="date-input" value="${task.date}" data-index="${index}">
                                <select class="hour-picker" data-index="${index}">
                                    ${Array.from({length:24}, (_, i) => `<option value="${String(i).padStart(2, '0')}" ${task.hour === String(i).padStart(2, '0') ? 'selected' : ''}>${i} 点</option>`).join('')}
                                </select>
                            </div>
                        </div>
                        
                        <!-- 紧急度选择 -->
                        <select class="priority-select" data-index="${index}">
                            <option value="high" ${task.priority === 'high' ? 'selected' : ''}>高紧急</option>
                            <option value="medium" ${task.priority === 'medium' ? 'selected' : ''}>中紧急</option>
                            <option value="low" ${task.priority === 'low' ? 'selected' : ''}>低紧急</option>
                        </select>
                        
                        <!-- 操作按钮 -->
                        <div class="task-actions">
                            <button class="toggle-detail-btn" data-index="${index}">${task.isDetailsOpen ? '收起' : '详情'}</button>
                            <button class="delete-btn" data-index="${index}">删除</button>
                        </div>
                    </div>
                    
                    <!-- 任务详情区域 -->
                    <div class="task-details ${task.isDetailsOpen ? 'active' : ''}" data-index="${index}">
                        <textarea class="detail-textarea" data-index="${index}">${task.details}</textarea>
                        <div class="task-due ${isOverdue ? 'overdue' : ''}">${dueText}</div>
                    </div>
                `;
                
                taskList.appendChild(li);
                
                // 保持时间选择弹窗激活状态（解决闪退）
                const timePickerModal = li.querySelector('.time-picker-modal');
                timePickerModal.addEventListener('click', (e) => e.stopPropagation());
            });
        }

        // 精确计算剩余时间（日期+小时）
        function calcRemaining(task) {
            const now = new Date();
            const target = new Date(`${task.date} ${task.hour}:00:00`);
            const diffMs = target - now;
            const isOverdue = diffMs < 0;

            // 逾期状态
            if (isOverdue) {
                const absDiffMs = Math.abs(diffMs);
                const days = Math.floor(absDiffMs / (1000 * 60 * 60 * 24));
                const hours = Math.floor((absDiffMs % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                
                let overdueText = '已逾期';
                if (days > 0) overdueText += ` ${days}天`;
                if (hours > 0) overdueText += ` ${hours}小时`;
                return { dueText: overdueText, isOverdue: true };
            }

            // 未逾期
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
            tasks[index].completed = !tasks[index].completed;
            saveTasks();
            renderTasks();
            updateStats();
        }

        // 切换详情展开/收起
        function toggleDetails(index) {
            tasks[index].isDetailsOpen = !tasks[index].isDetailsOpen;
            saveTasks();
            renderTasks();
        }

        // 删除任务
        function deleteTask(index) {
            if (confirm('确定删除该任务？')) {
                tasks.splice(index, 1);
                saveTasks();
                renderTasks();
                updateStats();
            }
        }

        // 更新任务属性（标题/详情/日期/小时/紧急度）
        function updateTaskProp(index, type, value) {
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
            renderTasks();
        }

        // 清空已完成任务
        function clearCompletedTasks() {
            if (!tasks.some(t => t.completed)) return;
            if (confirm('确定清空所有已完成任务？')) {
                tasks = tasks.filter(t => !t.completed);
                saveTasks();
                renderTasks();
                updateStats();
            }
        }

        // 绑定所有事件
        function bindEvents() {
            // 添加任务
            addBtn.addEventListener('click', addTask);
            taskTitleInput.addEventListener('keypress', e => e.key === 'Enter' && addTask());

            // 清空已完成
            clearCompletedBtn.addEventListener('click', clearCompletedTasks);

            // 任务列表事件委托
            taskList.addEventListener('click', (e) => {
                const idx = parseInt(e.target.dataset.index);
                if (isNaN(idx)) return;

                // 切换完成状态
                if (e.target.classList.contains('task-checkbox')) {
                    toggleTask(idx);
                }
                // 展开/收起详情
                else if (e.target.classList.contains('toggle-detail-btn')) {
                    toggleDetails(idx);
                }
                // 删除任务
                else if (e.target.classList.contains('delete-btn')) {
                    deleteTask(idx);
                }
            });

            // 编辑标题（失焦时保存）
            taskList.addEventListener('blur', (e) => {
                if (e.target.classList.contains('task-title')) {
                    const idx = parseInt(e.target.dataset.index);
                    updateTaskProp(idx, 'title', e.target.textContent);
                }
            }, true);

            // 编辑详情（实时保存）
            taskList.addEventListener('input', (e) => {
                if (e.target.classList.contains('detail-textarea')) {
                    const idx = parseInt(e.target.dataset.index);
                    updateTaskProp(idx, 'details', e.target.value);
                }
                // 更新日期
                else if (e.target.classList.contains('date-input')) {
                    const idx = parseInt(e.target.dataset.index);
                    updateTaskProp(idx, 'date', e.target.value);
                }
                // 更新小时（滚轮选择）
                else if (e.target.classList.contains('hour-picker')) {
                    const idx = parseInt(e.target.dataset.index);
                    updateTaskProp(idx, 'hour', e.target.value);
                }
                // 更新紧急度
                else if (e.target.classList.contains('priority-select')) {
                    const idx = parseInt(e.target.dataset.index);
                    updateTaskProp(idx, 'priority', e.target.value);
                }
            });

            // 阻止时间选择弹窗点击冒泡（解决闪退）
            taskList.addEventListener('click', (e) => {
                document.querySelectorAll('.time-picker-modal').forEach(modal => {
                    modal.classList.remove('active');
                });
            });
        }

        // 页面加载完成初始化
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
