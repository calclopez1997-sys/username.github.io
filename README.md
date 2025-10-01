<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Connect Organization</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/sortablejs@latest/Sortable.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.31/jspdf.plugin.autotable.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <style>
        :root {
            --primary: #FFA559;
            --primary-dark: #FF7F50;
            --primary-intense: #FF6B35;
            --secondary: #4A6FA5;
            --secondary-light: #6B8EC7;
            --success: #4CAF50;
            --warning: #FFC107;
            --danger: #FF5252;
            --light: #FFFFFF;
            --dark: #333333;
            --gray: #F5F5F5;
            --gray-light: #F0F0F0;
            --shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.1);
            --radius: 12px;
            --radius-sm: 8px;
            --transition: all 0.3s ease;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
        }

        body {
            background-color: var(--gray);
            color: var(--dark);
            line-height: 1.6;
            transition: var(--transition);
        }

        body.dark-mode {
            --light: #2D2D2D;
            --dark: #F5F5F5;
            --gray: #1E1E1E;
            --gray-light: #252525;
            --shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
        }

        /* Authentication Screen */
        .auth-container {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, rgba(255, 165, 89, 0.1) 0%, rgba(74, 111, 165, 0.1) 100%);
            padding: 20px;
        }

        .auth-card {
            background-color: var(--light);
            border-radius: var(--radius);
            box-shadow: var(--shadow-lg);
            width: 100%;
            max-width: 450px;
            padding: 40px;
            animation: fadeIn 0.5s ease forwards;
        }

        .auth-header {
            text-align: center;
            margin-bottom: 30px;
        }

        .auth-logo {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin-bottom: 15px;
        }

        .auth-logo i {
            font-size: 32px;
            color: var(--primary);
        }

        .auth-logo span {
            font-size: 24px;
            font-weight: 700;
            color: var(--primary);
        }

        .auth-title {
            font-size: 28px;
            font-weight: 700;
            color: var(--dark);
            margin-bottom: 10px;
        }

        .auth-subtitle {
            font-size: 16px;
            color: #777;
        }

        .auth-tabs {
            display: flex;
            margin-bottom: 25px;
            border-bottom: 1px solid #eee;
        }

        .auth-tab {
            flex: 1;
            text-align: center;
            padding: 12px 0;
            font-weight: 600;
            color: #777;
            cursor: pointer;
            transition: var(--transition);
            border-bottom: 2px solid transparent;
        }

        .auth-tab.active {
            color: var(--primary);
            border-bottom: 2px solid var(--primary);
        }

        .auth-form {
            display: none;
        }

        .auth-form.active {
            display: block;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: var(--dark);
        }

        .form-control {
            width: 100%;
            padding: 12px 15px;
            border: 1px solid #ddd;
            border-radius: var(--radius-sm);
            background-color: var(--gray-light);
            transition: var(--transition);
            font-size: 16px;
        }

        .form-control:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(255, 165, 89, 0.2);
        }

        .btn {
            width: 100%;
            padding: 12px 20px;
            border: none;
            border-radius: var(--radius-sm);
            background-color: var(--primary);
            color: white;
            font-weight: 600;
            font-size: 16px;
            cursor: pointer;
            transition: var(--transition);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .btn:hover {
            background-color: var(--primary-dark);
            transform: translateY(-2px);
        }

        .btn-secondary {
            background-color: var(--secondary);
        }

        .btn-secondary:hover {
            background-color: var(--secondary-light);
        }

        .btn-outline {
            background-color: transparent;
            border: 1px solid var(--primary);
            color: var(--primary);
        }

        .btn-outline:hover {
            background-color: var(--primary);
            color: white;
        }

        .auth-divider {
            display: flex;
            align-items: center;
            margin: 25px 0;
            color: #777;
        }

        .auth-divider::before,
        .auth-divider::after {
            content: '';
            flex: 1;
            height: 1px;
            background-color: #eee;
        }

        .auth-divider span {
            padding: 0 15px;
            font-size: 14px;
        }

        .social-login {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .social-btn {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            padding: 10px;
            border-radius: var(--radius-sm);
            border: 1px solid #ddd;
            background-color: var(--light);
            color: var(--dark);
            font-weight: 500;
            cursor: pointer;
            transition: var(--transition);
        }

        .social-btn:hover {
            background-color: var(--gray-light);
        }

        .social-btn i {
            font-size: 18px;
        }

        .social-btn.google i {
            color: #DB4437;
        }

        .social-btn.apple i {
            color: #000;
        }

        /* Main App Container */
        .app-container {
            display: none;
            min-height: 100vh;
        }

        .app-container.active {
            display: flex;
        }

        /* Sidebar */
        .sidebar {
            width: 260px;
            background-color: var(--light);
            padding: 20px;
            box-shadow: var(--shadow);
            transition: var(--transition);
            overflow-y: auto;
            display: flex;
            flex-direction: column;
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 30px;
            font-size: 24px;
            font-weight: 700;
            color: var(--primary);
        }

        .logo i {
            font-size: 28px;
        }

        .user-profile {
            display: flex;
            align-items: center;
            padding: 15px;
            background-color: var(--gray-light);
            border-radius: var(--radius);
            margin-bottom: 25px;
            cursor: pointer;
            transition: var(--transition);
        }

        .user-profile:hover {
            background-color: var(--gray);
        }

        .user-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background-color: var(--primary);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: 600;
            margin-right: 12px;
            position: relative;
            overflow: hidden;
        }

        .user-avatar img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .user-avatar .avatar-upload {
            position: absolute;
            bottom: 0;
            right: 0;
            background-color: var(--primary);
            color: white;
            width: 16px;
            height: 16px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 8px;
            cursor: pointer;
        }

        .user-info {
            flex: 1;
        }

        .user-name {
            font-weight: 600;
            color: var(--dark);
        }

        .user-code {
            font-size: 12px;
            color: #777;
        }

        .user-menu {
            display: flex;
            flex-direction: column;
            gap: 5px;
        }

        .user-menu-item {
            padding: 8px 12px;
            border-radius: var(--radius-sm);
            color: var(--dark);
            font-size: 14px;
            cursor: pointer;
            transition: var(--transition);
        }

        .user-menu-item:hover {
            background-color: var(--gray-light);
        }

        .nav-menu {
            list-style: none;
            margin-top: 20px;
            flex: 1;
        }

        .nav-item {
            margin-bottom: 8px;
        }

        .nav-link {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 12px 16px;
            border-radius: var(--radius);
            color: var(--dark);
            text-decoration: none;
            transition: var(--transition);
            cursor: pointer;
        }

        .nav-link:hover, .nav-link.active {
            background-color: var(--primary);
            color: white;
        }

        .nav-link i {
            font-size: 18px;
            width: 20px;
            text-align: center;
        }

        .sidebar-footer {
            margin-top: auto;
            padding-top: 20px;
            border-top: 1px solid #eee;
        }

        .logout-btn {
            display: flex;
            align-items: center;
            gap: 10px;
            width: 100%;
            padding: 10px 15px;
            border: none;
            border-radius: var(--radius);
            background-color: transparent;
            color: var(--dark);
            font-weight: 500;
            cursor: pointer;
            transition: var(--transition);
        }

        .logout-btn:hover {
            background-color: var(--gray-light);
        }

        /* Main Content */
        .main-content {
            flex: 1;
            padding: 30px;
            overflow-y: auto;
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
        }

        .page-title {
            font-size: 28px;
            font-weight: 700;
            color: var(--dark);
        }

        .header-actions {
            display: flex;
            gap: 15px;
        }

        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: var(--radius);
            background-color: var(--primary);
            color: white;
            font-weight: 600;
            cursor: pointer;
            transition: var(--transition);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .btn:hover {
            background-color: var(--primary-dark);
            transform: translateY(-2px);
        }

        .btn-icon {
            padding: 10px;
            background-color: var(--light);
            color: var(--dark);
            border-radius: 50%;
        }

        /* Search Bar */
        .search-container {
            position: relative;
            width: 250px;
        }

        .search-input {
            width: 100%;
            padding: 10px 15px 10px 40px;
            border: 1px solid #ddd;
            border-radius: var(--radius);
            background-color: var(--light);
            transition: var(--transition);
        }

        .search-input:focus {
            outline: none;
            border-color: var(--primary);
        }

        .search-icon {
            position: absolute;
            left: 12px;
            top: 50%;
            transform: translateY(-50%);
            color: #777;
        }

        /* Dashboard */
        .dashboard-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .card {
            background-color: var(--light);
            border-radius: var(--radius);
            padding: 20px;
            box-shadow: var(--shadow);
            transition: var(--transition);
            position: relative;
            overflow: hidden;
        }

        .card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 5px;
            height: 100%;
            background-color: var(--primary);
        }

        .card:hover {
            transform: translateY(-5px);
            box-shadow: var(--shadow-lg);
        }

        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .card-title {
            font-size: 18px;
            font-weight: 600;
            color: var(--dark);
        }

        .card-icon {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
            color: white;
        }

        .card-value {
            font-size: 32px;
            font-weight: 700;
            margin-bottom: 5px;
        }

        .card-text {
            color: #777;
            font-size: 14px;
        }

        /* Tasks */
        .tasks-container {
            background-color: var(--light);
            border-radius: var(--radius);
            padding: 25px;
            box-shadow: var(--shadow);
            margin-bottom: 30px;
        }

        .tasks-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .priority-filter {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }

        .filter-btn {
            padding: 8px 16px;
            border-radius: 30px;
            background-color: var(--gray-light);
            color: var(--dark);
            font-weight: 500;
            cursor: pointer;
            transition: var(--transition);
            border: none;
        }

        .filter-btn.active {
            background-color: var(--primary);
            color: white;
        }

        .task-list {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .task-item {
            display: flex;
            align-items: center;
            padding: 15px;
            background-color: var(--gray-light);
            border-radius: var(--radius);
            transition: var(--transition);
            position: relative;
        }

        .task-item:hover {
            transform: translateX(5px);
        }

        .task-item.high-priority {
            background-color: rgba(255, 107, 53, 0.1);
            border-left: 4px solid var(--primary-intense);
        }

        .task-checkbox {
            width: 20px;
            height: 20px;
            border-radius: 4px;
            border: 2px solid #ddd;
            margin-right: 15px;
            cursor: pointer;
            position: relative;
            transition: var(--transition);
        }

        .task-checkbox.checked {
            background-color: var(--success);
            border-color: var(--success);
        }

        .task-checkbox.checked::after {
            content: '✓';
            position: absolute;
            top: -3px;
            left: 3px;
            color: white;
            font-weight: bold;
            animation: checkmark 0.3s ease forwards;
        }

        @keyframes checkmark {
            0% { transform: scale(0); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); }
        }

        .task-content {
            flex: 1;
        }

        .task-title {
            font-weight: 600;
            margin-bottom: 4px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .task-title .alert-icon {
            color: var(--primary-intense);
        }

        .task-description {
            font-size: 14px;
            color: #666;
            margin-bottom: 4px;
        }

        .task-dates {
            font-size: 13px;
            color: #777;
        }

        .task-priority {
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
        }

        .priority-high {
            background-color: rgba(255, 82, 82, 0.2);
            color: var(--danger);
        }

        .priority-medium {
            background-color: rgba(255, 193, 7, 0.2);
            color: var(--warning);
        }

        .priority-low {
            background-color: rgba(76, 175, 80, 0.2);
            color: var(--success);
        }

        .task-category {
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            background-color: rgba(74, 111, 165, 0.2);
            color: var(--secondary);
            margin-left: 8px;
        }

        .task-actions {
            display: flex;
            gap: 8px;
            margin-left: 10px;
        }

        .task-action-btn {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            border: none;
            background-color: var(--gray-light);
            color: var(--dark);
            cursor: pointer;
            transition: var(--transition);
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .task-action-btn:hover {
            background-color: var(--primary);
            color: white;
        }

        .task-action-btn.delete:hover {
            background-color: var(--danger);
        }

        /* Task Subsections */
        .task-subsections {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }

        .subsection-card {
            background-color: var(--gray-light);
            border-radius: var(--radius);
            padding: 15px;
            cursor: pointer;
            transition: var(--transition);
            text-align: center;
        }

        .subsection-card:hover {
            background-color: var(--primary);
            color: white;
        }

        .subsection-card.active {
            background-color: var(--primary);
            color: white;
        }

        .subsection-icon {
            font-size: 24px;
            margin-bottom: 10px;
        }

        .subsection-title {
            font-weight: 600;
        }

        /* Calendar */
        .calendar-container {
            background-color: var(--light);
            border-radius: var(--radius);
            padding: 25px;
            box-shadow: var(--shadow);
            margin-bottom: 30px;
        }

        .calendar-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .calendar-nav {
            display: flex;
            gap: 10px;
            align-items: center;
        }

        .calendar-btn {
            background-color: var(--gray-light);
            border: none;
            width: 36px;
            height: 36px;
            border-radius: 50%;
            cursor: pointer;
            transition: var(--transition);
        }

        .calendar-btn:hover {
            background-color: var(--primary);
            color: white;
        }

        .calendar-select {
            padding: 8px 12px;
            border-radius: var(--radius-sm);
            border: 1px solid #ddd;
            background-color: var(--gray-light);
            font-weight: 500;
        }

        .calendar-view-toggle {
            display: flex;
            gap: 5px;
        }

        .view-btn {
            padding: 8px 12px;
            border: none;
            background-color: var(--gray-light);
            border-radius: var(--radius-sm);
            cursor: pointer;
            transition: var(--transition);
        }

        .view-btn.active {
            background-color: var(--primary);
            color: white;
        }

        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 10px;
        }

        .calendar-day-header {
            text-align: center;
            font-weight: 600;
            padding: 10px 0;
            color: #777;
        }

        .calendar-day {
            aspect-ratio: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
            border-radius: var(--radius);
            background-color: var(--gray-light);
            cursor: pointer;
            transition: var(--transition);
            position: relative;
            padding-top: 8px;
            min-height: 80px;
        }

        .calendar-day:hover {
            background-color: var(--primary);
            color: white;
        }

        .calendar-day.today {
            background-color: var(--primary);
            color: white;
            font-weight: 700;
        }

        .calendar-day.has-task {
            border: 1px solid var(--primary);
        }

        .calendar-day .task-indicator {
            width: 6px;
            height: 6px;
            border-radius: 50%;
            margin-top: 2px;
        }

        .calendar-day .task-indicator.high {
            background-color: var(--danger);
        }

        .calendar-day .task-indicator.medium {
            background-color: var(--warning);
        }

        .calendar-day .task-indicator.low {
            background-color: var(--success);
        }

        .calendar-day.other-month {
            color: #aaa;
        }

        .calendar-week-view {
            display: grid;
            grid-template-columns: 80px repeat(7, 1fr);
            gap: 10px;
        }

        .week-time {
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            color: #777;
        }

        .week-day {
            border-radius: var(--radius);
            background-color: var(--gray-light);
            min-height: 60px;
            padding: 8px;
            position: relative;
        }

        .week-day-header {
            font-weight: 600;
            margin-bottom: 5px;
            text-align: center;
        }

        .week-day.today {
            background-color: rgba(255, 165, 89, 0.2);
        }

        .week-task {
            background-color: var(--primary);
            color: white;
            border-radius: 4px;
            padding: 4px;
            font-size: 12px;
            margin-bottom: 4px;
            cursor: pointer;
            transition: var(--transition);
        }

        .week-task:hover {
            transform: scale(1.02);
        }

        .calendar-day-view {
            max-width: 800px;
            margin: 0 auto;
        }

        .day-hour {
            display: grid;
            grid-template-columns: 80px 1fr;
            gap: 10px;
            margin-bottom: 10px;
        }

        .hour-label {
            text-align: right;
            font-size: 14px;
            color: #777;
            padding-right: 10px;
        }

        .hour-content {
            background-color: var(--gray-light);
            border-radius: var(--radius);
            min-height: 40px;
            padding: 8px;
            position: relative;
        }

        .day-task {
            background-color: var(--primary);
            color: white;
            border-radius: 4px;
            padding: 6px;
            font-size: 14px;
            margin-bottom: 8px;
            cursor: pointer;
            transition: var(--transition);
        }

        .day-task:hover {
            transform: scale(1.02);
        }

        /* Task Form */
        .task-form {
            background-color: var(--gray-light);
            border-radius: var(--radius);
            padding: 20px;
            margin-bottom: 20px;
            display: none;
        }

        .task-form.active {
            display: block;
            animation: fadeIn 0.3s ease forwards;
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
        }

        .form-control {
            width: 100%;
            padding: 10px 15px;
            border: 1px solid #ddd;
            border-radius: var(--radius-sm);
            background-color: var(--light);
            transition: var(--transition);
        }

        .form-control:focus {
            outline: none;
            border-color: var(--primary);
        }

        .form-row {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
        }

        /* Charts */
        .charts-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .chart-card {
            background-color: var(--light);
            border-radius: var(--radius);
            padding: 20px;
            box-shadow: var(--shadow);
        }

        /* Finance */
        .finance-container {
            background-color: var(--light);
            border-radius: var(--radius);
            padding: 25px;
            box-shadow: var(--shadow);
            margin-bottom: 30px;
        }

        .finance-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .finance-summary {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
            margin-bottom: 20px;
        }

        .summary-item {
            text-align: center;
            padding: 15px;
            background-color: var(--gray-light);
            border-radius: var(--radius);
        }

        .summary-value {
            font-size: 24px;
            font-weight: 700;
            margin-bottom: 5px;
        }

        .income {
            color: var(--success);
        }

        .expense {
            color: var(--danger);
        }

        /* Transaction Cards */
        .transaction-cards {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }

        .transaction-card {
            background-color: var(--gray-light);
            border-radius: var(--radius);
            padding: 15px;
            transition: var(--transition);
            cursor: pointer;
            border-left: 4px solid transparent;
            position: relative;
        }

        .transaction-card.income {
            border-left-color: var(--success);
        }

        .transaction-card.expense {
            border-left-color: var(--danger);
        }

        .transaction-card:hover {
            transform: translateY(-5px);
        }

        .transaction-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }

        .transaction-title {
            font-weight: 600;
        }

        .transaction-amount {
            font-weight: 700;
            font-size: 18px;
        }

        .transaction-amount.income {
            color: var(--success);
        }

        .transaction-amount.expense {
            color: var(--danger);
        }

        .transaction-details {
            display: flex;
            justify-content: space-between;
            font-size: 14px;
            color: #777;
        }

        .transaction-actions {
            position: absolute;
            top: 10px;
            right: 10px;
            display: flex;
            gap: 5px;
        }

        .transaction-action-btn {
            width: 24px;
            height: 24px;
            border-radius: 50%;
            border: none;
            background-color: var(--gray-light);
            color: var(--dark);
            cursor: pointer;
            transition: var(--transition);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 10px;
        }

        .transaction-action-btn:hover {
            background-color: var(--primary);
            color: white;
        }

        .transaction-action-btn.delete:hover {
            background-color: var(--danger);
        }

        /* Notes */
        .notes-container {
            background-color: var(--light);
            border-radius: var(--radius);
            padding: 25px;
            box-shadow: var(--shadow);
        }

        .notes-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 15px;
        }

        .note {
            background-color: var(--gray-light);
            padding: 15px;
            border-radius: var(--radius);
            min-height: 150px;
            position: relative;
            transition: var(--transition);
            cursor: pointer;
        }

        .note:hover {
            transform: translateY(-5px);
        }

        .note.pinned {
            border: 2px solid var(--primary);
        }

        .note-title {
            font-weight: 600;
            margin-bottom: 10px;
        }

        .note-content {
            font-size: 14px;
            color: #555;
        }

        .note-date {
            position: absolute;
            bottom: 10px;
            right: 10px;
            font-size: 12px;
            color: #999;
        }

        .note-pin {
            position: absolute;
            top: 10px;
            right: 10px;
            color: #999;
            cursor: pointer;
        }

        .note-pin.pinned {
            color: var(--primary);
        }

        .note-category {
            position: absolute;
            top: 10px;
            left: 10px;
            padding: 3px 8px;
            border-radius: 10px;
            font-size: 10px;
            font-weight: 600;
            background-color: rgba(74, 111, 165, 0.2);
            color: var(--secondary);
        }

        /* Projects */
        .projects-container {
            background-color: var(--light);
            border-radius: var(--radius);
            padding: 25px;
            box-shadow: var(--shadow);
            margin-bottom: 30px;
        }

        .projects-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
        }

        .project-card {
            background-color: var(--gray-light);
            border-radius: var(--radius);
            padding: 20px;
            transition: var(--transition);
            cursor: pointer;
        }

        .project-card:hover {
            transform: translateY(-5px);
        }

        .project-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .project-title {
            font-size: 20px;
            font-weight: 700;
            color: var(--dark);
        }

        .project-status {
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            background-color: rgba(255, 165, 89, 0.2);
            color: var(--primary);
        }

        .project-progress {
            margin-bottom: 15px;
        }

        .progress-bar {
            height: 8px;
            background-color: #ddd;
            border-radius: 4px;
            overflow: hidden;
            margin-bottom: 5px;
        }

        .progress-fill {
            height: 100%;
            background-color: var(--primary);
            border-radius: 4px;
            transition: width 0.5s ease;
        }

        .progress-text {
            display: flex;
            justify-content: space-between;
            font-size: 14px;
            color: #777;
        }

        .project-tasks {
            margin-bottom: 10px;
        }

        .project-task {
            display: flex;
            align-items: center;
            padding: 8px 0;
            border-bottom: 1px solid rgba(0, 0, 0, 0.05);
        }

        .project-task:last-child {
            border-bottom: none;
        }

        .project-task-checkbox {
            width: 16px;
            height: 16px;
            border-radius: 3px;
            border: 2px solid #ddd;
            margin-right: 10px;
            cursor: pointer;
            position: relative;
        }

        .project-task-checkbox.checked {
            background-color: var(--success);
            border-color: var(--success);
        }

        .project-task-checkbox.checked::after {
            content: '✓';
            position: absolute;
            top: -3px;
            left: 2px;
            color: white;
            font-weight: bold;
            font-size: 12px;
        }

        .project-task-title {
            font-size: 14px;
            color: var(--dark);
        }

        .project-task-priority {
            margin-left: auto;
            padding: 2px 8px;
            border-radius: 10px;
            font-size: 10px;
            font-weight: 600;
        }

        /* Reports */
        .reports-container {
            background-color: var(--light);
            border-radius: var(--radius);
            padding: 25px;
            box-shadow: var(--shadow);
            margin-bottom: 30px;
        }

        .reports-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
        }

        .report-card {
            background-color: var(--gray-light);
            border-radius: var(--radius);
            padding: 20px;
            transition: var(--transition);
            cursor: pointer;
        }

        .report-card:hover {
            transform: translateY(-5px);
        }

        .report-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .report-title {
            font-size: 18px;
            font-weight: 600;
            color: var(--dark);
        }

        .report-icon {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
            color: white;
            background-color: var(--primary);
        }

        .report-description {
            font-size: 14px;
            color: #777;
            margin-bottom: 15px;
        }

        .report-actions {
            display: flex;
            gap: 10px;
        }

        .report-btn {
            padding: 8px 12px;
            border: none;
            border-radius: var(--radius-sm);
            background-color: var(--primary);
            color: white;
            font-weight: 500;
            cursor: pointer;
            transition: var(--transition);
            font-size: 12px;
        }

        .report-btn:hover {
            background-color: var(--primary-dark);
        }

        .report-btn.secondary {
            background-color: var(--secondary);
        }

        .report-btn.secondary:hover {
            background-color: var(--secondary-light);
        }

        /* User Management */
        .users-container {
            background-color: var(--light);
            border-radius: var(--radius);
            padding: 25px;
            box-shadow: var(--shadow);
            margin-bottom: 30px;
        }

        .users-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .users-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 15px;
        }

        .user-card {
            background-color: var(--gray-light);
            border-radius: var(--radius);
            padding: 15px;
            transition: var(--transition);
        }

        .user-card:hover {
            transform: translateY(-5px);
        }

        .user-card-header {
            display: flex;
            align-items: center;
            margin-bottom: 15px;
        }

        .user-card-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background-color: var(--primary);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: 600;
            margin-right: 12px;
        }

        .user-card-info {
            flex: 1;
        }

        .user-card-name {
            font-weight: 600;
            color: var(--dark);
        }

        .user-card-email {
            font-size: 12px;
            color: #777;
        }

        .user-card-code {
            font-size: 12px;
            color: var(--primary);
            font-weight: 600;
            margin-bottom: 10px;
        }

        .user-card-permissions {
            display: flex;
            gap: 5px;
            margin-bottom: 10px;
        }

        .permission-badge {
            padding: 3px 8px;
            border-radius: 10px;
            font-size: 10px;
            font-weight: 600;
            background-color: rgba(74, 111, 165, 0.2);
            color: var(--secondary);
        }

        .user-card-actions {
            display: flex;
            gap: 5px;
        }

        .user-card-btn {
            padding: 5px 10px;
            border: none;
            border-radius: var(--radius-sm);
            background-color: var(--primary);
            color: white;
            font-weight: 500;
            cursor: pointer;
            transition: var(--transition);
            font-size: 10px;
        }

        .user-card-btn:hover {
            background-color: var(--primary-dark);
        }

        .user-card-btn.danger {
            background-color: var(--danger);
        }

        .user-card-btn.danger:hover {
            background-color: #E04040;
        }

        /* Gamification */
        .badges-container {
            display: flex;
            gap: 15px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .badge {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 15px;
            background-color: var(--gray-light);
            border-radius: var(--radius);
            transition: var(--transition);
        }

        .badge:hover {
            transform: translateY(-5px);
        }

        .badge-icon {
            font-size: 32px;
            color: var(--primary);
            margin-bottom: 8px;
        }

        .badge-title {
            font-weight: 600;
            font-size: 14px;
            text-align: center;
        }

        .badge-description {
            font-size: 12px;
            color: #777;
            text-align: center;
            max-width: 120px;
        }

        /* Settings */
        .settings-section {
            background-color: var(--light);
            border-radius: var(--radius);
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: var(--shadow);
        }

        .settings-title {
            font-size: 18px;
            font-weight: 600;
            margin-bottom: 15px;
            color: var(--dark);
        }

        .settings-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid #eee;
        }

        .settings-item:last-child {
            border-bottom: none;
        }

        .settings-label {
            font-weight: 500;
        }

        .settings-description {
            font-size: 14px;
            color: #777;
        }

        /* Toggle Switch */
        .switch {
            position: relative;
            display: inline-block;
            width: 50px;
            height: 24px;
        }

        .switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }

        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            transition: .4s;
            border-radius: 24px;
        }

        .slider:before {
            position: absolute;
            content: "";
            height: 16px;
            width: 16px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }

        input:checked + .slider {
            background-color: var(--primary);
        }

        input:checked + .slider:before {
            transform: translateX(26px);
        }

        /* Category Management */
        .category-list {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-bottom: 15px;
        }

        .category-item {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 8px 12px;
            background-color: var(--gray-light);
            border-radius: 20px;
        }

        .category-color {
            width: 12px;
            height: 12px;
            border-radius: 50%;
        }

        .category-name {
            font-weight: 500;
        }

        .category-delete {
            color: #777;
            cursor: pointer;
        }

        .category-delete:hover {
            color: var(--danger);
        }

        /* Export Options */
        .export-options {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }

        .export-btn {
            padding: 8px 16px;
            border: none;
            border-radius: var(--radius);
            background-color: var(--gray-light);
            color: var(--dark);
            font-weight: 500;
            cursor: pointer;
            transition: var(--transition);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .export-btn:hover {
            background-color: var(--primary);
            color: white;
        }

        /* Feedback Messages */
        .feedback-message {
            padding: 15px;
            border-radius: var(--radius);
            margin-bottom: 20px;
            text-align: center;
            font-weight: 500;
        }

        .feedback-success {
            background-color: rgba(76, 175, 80, 0.1);
            color: var(--success);
        }

        .feedback-warning {
            background-color: rgba(255, 193, 7, 0.1);
            color: var(--warning);
        }

        /* Notification */
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            background-color: var(--primary);
            color: white;
            border-radius: var(--radius);
            box-shadow: var(--shadow-lg);
            z-index: 1000;
            display: flex;
            align-items: center;
            gap: 10px;
            transform: translateX(120%);
            transition: transform 0.3s ease;
        }

        .notification.show {
            transform: translateX(0);
        }

        .notification-icon {
            font-size: 20px;
        }

        .notification-close {
            margin-left: 15px;
            cursor: pointer;
            font-size: 16px;
        }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        .modal.active {
            display: flex;
        }

        .modal-content {
            background-color: var(--light);
            border-radius: var(--radius);
            padding: 25px;
            width: 90%;
            max-width: 500px;
            max-height: 80vh;
            overflow-y: auto;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            animation: modalFadeIn 0.3s ease forwards;
        }

        @keyframes modalFadeIn {
            from { opacity: 0; transform: translateY(-20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .modal-title {
            font-size: 20px;
            font-weight: 600;
            color: var(--dark);
        }

        .modal-close {
            font-size: 24px;
            cursor: pointer;
            color: #777;
        }

        .modal-close:hover {
            color: var(--dark);
        }

        /* Responsive */
        @media (max-width: 768px) {
            .sidebar {
                width: 70px;
                padding: 15px 10px;
            }
            
            .logo span, .nav-link span, .user-info, .logout-btn span {
                display: none;
            }
            
            .dashboard-grid {
                grid-template-columns: 1fr;
            }
            
            .charts-container {
                grid-template-columns: 1fr;
            }
            
            .finance-form {
                grid-template-columns: 1fr;
            }
            
            .form-row {
                grid-template-columns: 1fr;
            }
            
            .search-container {
                width: 100%;
            }
            
            .task-subsections {
                grid-template-columns: 1fr;
            }
            
            .projects-grid {
                grid-template-columns: 1fr;
            }
            
            .reports-grid {
                grid-template-columns: 1fr;
            }
            
            .users-grid {
                grid-template-columns: 1fr;
            }
            
            .transaction-cards {
                grid-template-columns: 1fr;
            }
            
            .finance-summary {
                grid-template-columns: 1fr;
            }
        }

        /* Animations */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .fade-in {
            animation: fadeIn 0.5s ease forwards;
        }

        /* Drag and Drop */
        .sortable-ghost {
            opacity: 0.4;
        }
        
        .sortable-drag {
            opacity: 0.8;
        }

        /* Color Themes */
        .theme-blue {
            --primary: #4A6FA5;
            --primary-dark: #3A5A85;
            --primary-intense: #2A4A75;
        }

        .theme-green {
            --primary: #4CAF50;
            --primary-dark: #3D8B40;
            --primary-intense: #2E7D32;
        }

        .theme-purple {
            --primary: #9C27B0;
            --primary-dark: #7B1FA2;
            --primary-intense: #6A1B9A;
        }

        .theme-red {
            --primary: #F44336;
            --primary-dark: #D32F2F;
            --primary-intense: #C62828;
        }

        /* Edit Form */
        .edit-form {
            background-color: var(--gray-light);
            border-radius: var(--radius);
            padding: 20px;
            margin-bottom: 20px;
            display: none;
        }

        .edit-form.active {
            display: block;
            animation: fadeIn 0.3s ease forwards;
        }

        /* Loading Spinner */
        .spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            width: 36px;
            height: 36px;
            border-radius: 50%;
            border-left-color: var(--primary);
            animation: spin 1s ease infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Progress Indicators */
        .progress-indicator {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 15px;
        }

        .progress-circle {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: conic-gradient(var(--primary) 0%, var(--primary) var(--progress), #ddd var(--progress), #ddd 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
        }

        .progress-circle::before {
            content: '';
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background-color: var(--light);
        }

        .progress-text {
            position: absolute;
            font-weight: 600;
            font-size: 12px;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }

        ::-webkit-scrollbar-track {
            background: var(--gray-light);
        }

        ::-webkit-scrollbar-thumb {
            background: var(--primary);
            border-radius: 4px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: var(--primary-dark);
        }
    </style>
</head>
<body>
    <!-- Authentication Screen -->
    <div class="auth-container" id="authContainer">
        <div class="auth-card">
            <div class="auth-header">
                <div class="auth-logo">
                    <i class="fas fa-network-wired"></i>
                    <span>Connect Org</span>
                </div>
                <h1 class="auth-title">Bienvenido</h1>
                <p class="auth-subtitle">Inicia sesión para continuar</p>
            </div>
            
            <div class="auth-tabs">
                <div class="auth-tab active" data-tab="login">Iniciar Sesión</div>
                <div class="auth-tab" data-tab="register">Registrarse</div>
            </div>
            
            <!-- Login Form -->
            <form class="auth-form active" id="loginForm">
                <div class="form-group">
                    <label class="form-label">Correo Electrónico</label>
                    <input type="email" class="form-control" placeholder="correo@ejemplo.com" required>
                </div>
                
                <div class="form-group">
                    <label class="form-label">Contraseña</label>
                    <input type="password" class="form-control" placeholder="••••••••" required>
                </div>
                
                <div class="form-group">
                    <button type="submit" class="btn">Iniciar Sesión</button>
                </div>
            </form>
            
            <!-- Register Form -->
            <form class="auth-form" id="registerForm">
                <div class="form-group">
                    <label class="form-label">Nombre Completo</label>
                    <input type="text" class="form-control" placeholder="Juan Pérez" required>
                </div>
                
                <div class="form-group">
                    <label class="form-label">Correo Electrónico</label>
                    <input type="email" class="form-control" placeholder="correo@ejemplo.com" required>
                </div>
                
                <div class="form-group">
                    <label class="form-label">Contraseña</label>
                    <input type="password" class="form-control" placeholder="••••••••" required>
                </div>
                
                <div class="form-group">
                    <label class="form-label">Confirmar Contraseña</label>
                    <input type="password" class="form-control" placeholder="••••••••" required>
                </div>
                
                <div class="form-group">
                    <button type="submit" class="btn">Registrarse</button>
                </div>
            </form>
            
            <div class="auth-divider">
                <span>O continúa con</span>
            </div>
            
            <div class="social-login">
                <button class="social-btn google">
                    <i class="fab fa-google"></i>
                    Google
                </button>
                
                <button class="social-btn apple">
                    <i class="fab fa-apple"></i>
                    Apple
                </button>
            </div>
        </div>
    </div>

    <!-- Main App Container -->
    <div class="app-container" id="appContainer">
        <!-- Sidebar -->
        <aside class="sidebar">
            <div class="logo">
                <i class="fas fa-network-wired"></i>
                <span>Connect Org</span>
            </div>
            
            <div class="user-profile" id="userProfile">
                <div class="user-avatar" id="userAvatar">
                    <span id="avatarInitials">CL</span>
                    <div class="avatar-upload" id="avatarUpload">
                        <i class="fas fa-camera"></i>
                    </div>
                    <input type="file" id="avatarInput" accept="image/*" style="display: none;">
                </div>
                <div class="user-info">
                    <div class="user-name" id="userName">César López</div>
                    <div class="user-code" id="userCode">USR-CL-9834</div>
                </div>
                <i class="fas fa-chevron-down"></i>
            </div>
            
            <div class="user-menu" id="userMenu" style="display: none;">
                <div class="user-menu-item" id="profileMenuItem">Mi Perfil</div>
                <div class="user-menu-item" id="settingsMenuItem">Configuración</div>
            </div>
            
            <ul class="nav-menu">
                <li class="nav-item">
                    <a class="nav-link active" data-page="dashboard">
                        <i class="fas fa-home"></i>
                        <span>Dashboard</span>
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" data-page="tasks">
                        <i class="fas fa-tasks"></i>
                        <span>Gestión de Tareas</span>
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" data-page="calendar">
                        <i class="fas fa-calendar-alt"></i>
                        <span>Calendario</span>
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" data-page="charts">
                        <i class="fas fa-chart-pie"></i>
                        <span>Gráficas</span>
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" data-page="finance">
                        <i class="fas fa-wallet"></i>
                        <span>Gestión de Finanzas</span>
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" data-page="projects">
                        <i class="fas fa-project-diagram"></i>
                        <span>Proyectos</span>
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" data-page="reports">
                        <i class="fas fa-file-alt"></i>
                        <span>Reportes</span>
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" data-page="users">
                        <i class="fas fa-users"></i>
                        <span>Gestión de Usuarios</span>
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" data-page="notes">
                        <i class="fas fa-sticky-note"></i>
                        <span>Notas Rápidas</span>
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" data-page="settings">
                        <i class="fas fa-cog"></i>
                        <span>Configuración</span>
                    </a>
                </li>
            </ul>
            
            <div class="sidebar-footer">
                <button class="logout-btn" id="logoutBtn">
                    <i class="fas fa-sign-out-alt"></i>
                    <span>Cerrar Sesión</span>
                </button>
            </div>
        </aside>
        
        <!-- Main Content -->
        <main class="main-content">
            <!-- Dashboard Page -->
            <div id="dashboard" class="page-content">
                <div class="header">
                    <h1 class="page-title">Dashboard</h1>
                    <div class="header-actions">
                        <div class="search-container">
                            <i class="fas fa-search search-icon"></i>
                            <input type="text" class="search-input" placeholder="Buscar..." id="dashboardSearch">
                        </div>
                        <button class="btn btn-icon" id="darkModeToggle">
                            <i class="fas fa-moon"></i>
                        </button>
                    </div>
                </div>
                
                <div class="dashboard-grid">
                    <div class="card fade-in">
                        <div class="card-header">
                            <h3 class="card-title">Tareas Urgentes Hoy</h3>
                            <div class="card-icon" style="background-color: var(--primary);">
                                <i class="fas fa-exclamation-circle"></i>
                            </div>
                        </div>
                        <div class="card-value" id="urgentTasksCount">3</div>
                        <div class="card-text">Todas de alta prioridad</div>
                    </div>
                    
                    <div class="card fade-in" style="animation-delay: 0.1s;">
                        <div class="card-header">
                            <h3 class="card-title">Avance Semanal</h3>
                            <div class="card-icon" style="background-color: var(--success);">
                                <i class="fas fa-chart-line"></i>
                            </div>
                        </div>
                        <div class="card-value">78%</div>
                        <div class="card-text">+12% que la semana pasada</div>
                    </div>
                    
                    <div class="card fade-in" style="animation-delay: 0.2s;">
                        <div class="card-header">
                            <h3 class="card-title">Balance Financiero</h3>
                            <div class="card-icon" style="background-color: var(--secondary);">
                                <i class="fas fa-dollar-sign"></i>
                            </div>
                        </div>
                        <div class="card-value" id="dashboardBalance">$2,450</div>
                        <div class="card-text">Ingresos: $3,200 | Gastos: $750</div>
                    </div>
                </div>
                
                <div class="tasks-container">
                    <div class="tasks-header">
                        <h2 class="card-title">Tareas de Alta Prioridad</h2>
                        <div class="priority-filter">
                            <button class="filter-btn active" data-filter="all">Todas</button>
                            <button class="filter-btn" data-filter="high">Alta</button>
                            <button class="filter-btn" data-filter="medium">Media</button>
                            <button class="filter-btn" data-filter="low">Baja</button>
                        </div>
                    </div>
                    
                    <div class="task-list" id="highPriorityTasks">
                        <div class="task-item high-priority" data-task-id="1">
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">
                                    Preparar presentación para cliente
                                    <i class="fas fa-exclamation-circle alert-icon"></i>
                                </div>
                                <div class="task-description">Revisar todos los datos y preparar slides</div>
                                <div class="task-dates">Hoy</div>
                            </div>
                            <div class="task-priority priority-high">Alta</div>
                            <div class="task-category">Trabajo</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="1">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="1">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="task-item high-priority" data-task-id="2">
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">
                                    Revisión de presupuesto
                                    <i class="fas fa-exclamation-circle alert-icon"></i>
                                </div>
                                <div class="task-description">Analizar gastos del último trimestre</div>
                                <div class="task-dates">Hoy</div>
                            </div>
                            <div class="task-priority priority-high">Alta</div>
                            <div class="task-category">Trabajo</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="2">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="2">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="task-item high-priority" data-task-id="3">
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">
                                    Entregar informe final
                                    <i class="fas fa-exclamation-circle alert-icon"></i>
                                </div>
                                <div class="task-description">Subir informe a plataforma universitaria</div>
                                <div class="task-dates">Hoy</div>
                            </div>
                            <div class="task-priority priority-high">Alta</div>
                            <div class="task-category">Estudio</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="3">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="3">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">Notas Ancladas</h3>
                    </div>
                    <div class="notes-grid" style="grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));">
                        <div class="note pinned" data-note-id="1">
                            <div class="note-category">Trabajo</div>
                            <i class="fas fa-thumbtack note-pin pinned"></i>
                            <div class="note-title">Reunión importante</div>
                            <div class="note-content">Preparar documentación para la reunión con inversores el viernes.</div>
                            <div class="note-date">16/05/2023</div>
                        </div>
                    </div>
                </div>
                
                <div class="card" style="margin-top: 20px;">
                    <div class="card-header">
                        <h3 class="card-title">Logros</h3>
                    </div>
                    <div class="badges-container">
                        <div class="badge">
                            <i class="fas fa-trophy badge-icon"></i>
                            <div class="badge-title">Campeón Semanal</div>
                            <div class="badge-description">Completaste todas tus tareas esta semana</div>
                        </div>
                        <div class="badge">
                            <i class="fas fa-star badge-icon"></i>
                            <div class="badge-title">Organizador Experto</div>
                            <div class="badge-description">Creaste 10 categorías personalizadas</div>
                        </div>
                        <div class="badge">
                            <i class="fas fa-fire badge-icon"></i>
                            <div class="badge-title">Racha de 7 días</div>
                            <div class="badge-description">Completaste tareas diarias por una semana</div>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Tasks Page -->
            <div id="tasks" class="page-content" style="display: none;">
                <div class="header">
                    <h1 class="page-title">Gestión de Tareas</h1>
                    <div class="header-actions">
                        <button class="btn" id="newTaskBtn">
                            <i class="fas fa-plus"></i>
                            Nueva Tarea
                        </button>
                    </div>
                </div>
                
                <div class="task-form" id="taskForm">
                    <h3 class="card-title">Nueva Tarea</h3>
                    <div class="form-group">
                        <label class="form-label">Título</label>
                        <input type="text" class="form-control" id="taskTitle" placeholder="Título de la tarea">
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Descripción</label>
                        <textarea class="form-control" id="taskDescription" rows="3" placeholder="Breve descripción"></textarea>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Categoría</label>
                        <select class="form-control" id="taskCategory">
                            <option value="work">Trabajo</option>
                            <option value="personal">Personal</option>
                            <option value="health">Salud</option>
                            <option value="study">Estudio</option>
                            <option value="other">Otros</option>
                        </select>
                    </div>
                    
                    <div class="form-row">
                        <div class="form-group">
                            <label class="form-label">Fecha de Inicio</label>
                            <input type="date" class="form-control" id="taskStartDate">
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label">Fecha de Finalización</label>
                            <input type="date" class="form-control" id="taskEndDate">
                        </div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Prioridad</label>
                        <select class="form-control" id="taskPriority">
                            <option value="high">Alta</option>
                            <option value="medium">Media</option>
                            <option value="low">Baja</option>
                        </select>
                    </div>
                    
                    <div class="form-group">
                        <button class="btn" id="saveTaskBtn">Guardar Tarea</button>
                        <button class="btn" id="cancelTaskBtn" style="background-color: var(--gray-light); color: var(--dark);">Cancelar</button>
                    </div>
                </div>

                <!-- Edit Task Form -->
                <div class="edit-form" id="editTaskForm">
                    <h3 class="card-title">Editar Tarea</h3>
                    <div class="form-group">
                        <label class="form-label">Título</label>
                        <input type="text" class="form-control" id="editTaskTitle" placeholder="Título de la tarea">
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Descripción</label>
                        <textarea class="form-control" id="editTaskDescription" rows="3" placeholder="Breve descripción"></textarea>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Categoría</label>
                        <select class="form-control" id="editTaskCategory">
                            <option value="work">Trabajo</option>
                            <option value="personal">Personal</option>
                            <option value="health">Salud</option>
                            <option value="study">Estudio</option>
                            <option value="other">Otros</option>
                        </select>
                    </div>
                    
                    <div class="form-row">
                        <div class="form-group">
                            <label class="form-label">Fecha de Inicio</label>
                            <input type="date" class="form-control" id="editTaskStartDate">
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label">Fecha de Finalización</label>
                            <input type="date" class="form-control" id="editTaskEndDate">
                        </div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Prioridad</label>
                        <select class="form-control" id="editTaskPriority">
                            <option value="high">Alta</option>
                            <option value="medium">Media</option>
                            <option value="low">Baja</option>
                        </select>
                    </div>
                    
                    <div class="form-group">
                        <button class="btn" id="updateTaskBtn">Actualizar Tarea</button>
                        <button class="btn" id="cancelEditTaskBtn" style="background-color: var(--gray-light); color: var(--dark);">Cancelar</button>
                    </div>
                </div>
                
                <div class="task-subsections">
                    <div class="subsection-card active" data-subsection="general">
                        <div class="subsection-icon">
                            <i class="fas fa-tasks"></i>
                        </div>
                        <div class="subsection-title">Tareas Generales</div>
                    </div>
                    
                    <div class="subsection-card" data-subsection="daily">
                        <div class="subsection-icon">
                            <i class="fas fa-calendar-day"></i>
                        </div>
                        <div class="subsection-title">Tareas Diarias</div>
                    </div>
                    
                    <div class="subsection-card" data-subsection="category">
                        <div class="subsection-icon">
                            <i class="fas fa-folder"></i>
                        </div>
                        <div class="subsection-title">Tareas por Categoría</div>
                    </div>
                    
                    <div class="subsection-card" data-subsection="completed">
                        <div class="subsection-icon">
                            <i class="fas fa-check-circle"></i>
                        </div>
                        <div class="subsection-title">Tareas Finalizadas</div>
                    </div>
                    
                    <div class="subsection-card" data-subsection="pending">
                        <div class="subsection-icon">
                            <i class="fas fa-clock"></i>
                        </div>
                        <div class="subsection-title">Pendientes</div>
                    </div>
                </div>
                
                <!-- Tareas Generales -->
                <div id="generalTasks" class="tasks-container subsection-content">
                    <div class="tasks-header">
                        <h2 class="card-title">Todas las Tareas</h2>
                    </div>
                    
                    <div class="priority-filter">
                        <button class="filter-btn active" data-filter="all">Todo</button>
                        <button class="filter-btn" data-filter="high">Alta</button>
                        <button class="filter-btn" data-filter="medium">Media</button>
                        <button class="filter-btn" data-filter="low">Baja</button>
                    </div>
                    
                    <div class="task-list" id="taskList">
                        <div class="task-item" data-priority="high" data-task-id="4">
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">Reunión con equipo de diseño</div>
                                <div class="task-description">Discutir nuevos prototipos</div>
                                <div class="task-dates">Inicio: 17/05/2023 | Fin: 17/05/2023</div>
                            </div>
                            <div class="task-priority priority-high">Alta</div>
                            <div class="task-category">Trabajo</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="4">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="4">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="task-item" data-priority="medium" data-task-id="5">
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">Enviar reporte semanal</div>
                                <div class="task-description">Recopilar datos de todos los departamentos</div>
                                <div class="task-dates">Inicio: 17/05/2023 | Fin: 19/05/2023</div>
                            </div>
                            <div class="task-priority priority-medium">Media</div>
                            <div class="task-category">Trabajo</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="5">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="5">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="task-item" data-priority="low" data-task-id="6">
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">Comprar suministros de oficina</div>
                                <div class="task-description">Papel, bolígrafos, carpetas</div>
                                <div class="task-dates">Inicio: 18/05/2023 | Fin: 18/05/2023</div>
                            </div>
                            <div class="task-priority priority-low">Baja</div>
                            <div class="task-category">Trabajo</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="6">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="6">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="task-item" data-priority="medium" data-task-id="7">
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">Ir al gimnasio</div>
                                <div class="task-description">Rutina de cardio y pesas</div>
                                <div class="task-dates">Inicio: 17/05/2023 | Fin: 17/05/2023</div>
                            </div>
                            <div class="task-priority priority-medium">Media</div>
                            <div class="task-category">Salud</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="7">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="7">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="task-item" data-priority="high" data-task-id="8">
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">Estudiar para examen</div>
                                <div class="task-description">Repasar capítulos 5 y 6</div>
                                <div class="task-dates">Inicio: 18/05/2023 | Fin: 20/05/2023</div>
                            </div>
                            <div class="task-priority priority-high">Alta</div>
                            <div class="task-category">Estudio</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="8">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="8">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="task-item" data-priority="medium" data-task-id="9">
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">Cena con mi madre</div>
                                <div class="task-description">Celebrar su cumpleaños</div>
                                <div class="task-dates">Inicio: 29/11/2025 | Fin: 30/11/2025</div>
                            </div>
                            <div class="task-priority priority-medium">Media</div>
                            <div class="task-category">Personal</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="9">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="9">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- Tareas Diarias -->
                <div id="dailyTasks" class="tasks-container subsection-content" style="display: none;">
                    <div class="tasks-header">
                        <h2 class="card-title">Tareas para Hoy (<span id="todayDate"></span>)</h2>
                    </div>
                    
                    <div class="task-list" id="dailyTaskList">
                        <div class="task-item" data-priority="high" data-task-id="4">
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">Reunión con equipo de diseño</div>
                                <div class="task-description">Discutir nuevos prototipos</div>
                                <div class="task-dates">Hoy</div>
                            </div>
                            <div class="task-priority priority-high">Alta</div>
                            <div class="task-category">Trabajo</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="4">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="4">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="task-item" data-priority="medium" data-task-id="7">
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">Ir al gimnasio</div>
                                <div class="task-description">Rutina de cardio y pesas</div>
                                <div class="task-dates">Hoy</div>
                            </div>
                            <div class="task-priority priority-medium">Media</div>
                            <div class="task-category">Salud</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="7">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="7">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="task-item" data-priority="low" data-task-id="10">
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">Llamar a mamá</div>
                                <div class="task-description">Felicitación por su cumpleaños</div>
                                <div class="task-dates">Hoy</div>
                            </div>
                            <div class="task-priority priority-low">Baja</div>
                            <div class="task-category">Personal</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="10">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="10">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- Tareas por Categoría -->
                <div id="categoryTasks" class="tasks-container subsection-content" style="display: none;">
                    <div class="tasks-header">
                        <h2 class="card-title">Tareas por Categoría</h2>
                    </div>
                    
                    <div class="category-filter">
                        <button class="filter-btn active" data-category="work">Trabajo</button>
                        <button class="filter-btn" data-category="personal">Personal</button>
                        <button class="filter-btn" data-category="health">Salud</button>
                        <button class="filter-btn" data-category="study">Estudio</button>
                        <button class="filter-btn" data-category="other">Otros</button>
                    </div>
                    
                    <div class="task-list" id="categoryTaskList">
                        <!-- Tasks will be populated by JavaScript based on selected category -->
                    </div>
                </div>
                
                <!-- Tareas Finalizadas -->
                <div id="completedTasks" class="tasks-container subsection-content" style="display: none;">
                    <div class="tasks-header">
                        <h2 class="card-title">Tareas Finalizadas</h2>
                    </div>
                    
                    <div class="priority-filter">
                        <button class="filter-btn active" data-filter="all">Todas</button>
                        <button class="filter-btn" data-filter="high">Alta</button>
                        <button class="filter-btn" data-filter="medium">Media</button>
                        <button class="filter-btn" data-filter="low">Baja</button>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Ordenar por:</label>
                        <select class="form-control" id="sortCompletedTasks" style="width: 200px;">
                            <option value="date">Fecha de finalización</option>
                            <option value="priority">Prioridad</option>
                            <option value="category">Categoría</option>
                        </select>
                    </div>
                    
                    <div class="task-list" id="completedTaskList">
                        <div class="task-item" data-priority="high">
                            <div class="task-checkbox checked"></div>
                            <div class="task-content">
                                <div class="task-title">Preparar presentación para cliente</div>
                                <div class="task-description">Revisar todos los datos y preparar slides</div>
                                <div class="task-dates">Completada: 15/05/2023</div>
                            </div>
                            <div class="task-priority priority-high">Alta</div>
                            <div class="task-category">Trabajo</div>
                        </div>
                        
                        <div class="task-item" data-priority="medium">
                            <div class="task-checkbox checked"></div>
                            <div class="task-content">
                                <div class="task-title">Actualizar documentación</div>
                                <div class="task-description">Revisar y actualizar manuales de usuario</div>
                                <div class="task-dates">Completada: 14/05/2023</div>
                            </div>
                            <div class="task-priority priority-medium">Media</div>
                            <div class="task-category">Trabajo</div>
                        </div>
                        
                        <div class="task-item" data-priority="low">
                            <div class="task-checkbox checked"></div>
                            <div class="task-content">
                                <div class="task-title">Comprar groceries</div>
                                <div class="task-description">Lista de compras semanal</div>
                                <div class="task-dates">Completada: 13/05/2023</div>
                            </div>
                            <div class="task-priority priority-low">Baja</div>
                            <div class="task-category">Personal</div>
                        </div>
                        
                        <div class="task-item" data-priority="high">
                            <div class="task-checkbox checked"></div>
                            <div class="task-content">
                                <div class="task-title">Entregar proyecto final</div>
                                <div class="task-description">Subir proyecto a plataforma universitaria</div>
                                <div class="task-dates">Completada: 12/05/2023</div>
                            </div>
                            <div class="task-priority priority-high">Alta</div>
                            <div class="task-category">Estudio</div>
                        </div>
                    </div>
                </div>
                
                <!-- Tareas Pendientes -->
                <div id="pendingTasks" class="tasks-container subsection-content" style="display: none;">
                    <div class="tasks-header">
                        <h2 class="card-title">Tareas Pendientes</h2>
                    </div>
                    
                    <div class="priority-filter">
                        <button class="filter-btn active" data-filter="all">Todo</button>
                        <button class="filter-btn" data-filter="high">Alta</button>
                        <button class="filter-btn" data-filter="medium">Media</button>
                        <button class="filter-btn" data-filter="low">Baja</button>
                    </div>
                    
                    <div class="task-list" id="pendingTaskList">
                        <div class="task-item" data-priority="high" data-task-id="4">
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">Reunión con equipo de diseño</div>
                                <div class="task-description">Discutir nuevos prototipos</div>
                                <div class="task-dates">Inicio: 17/05/2023 | Fin: 17/05/2023</div>
                            </div>
                            <div class="task-priority priority-high">Alta</div>
                            <div class="task-category">Trabajo</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="4">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="4">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="task-item" data-priority="medium" data-task-id="5">
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">Enviar reporte semanal</div>
                                <div class="task-description">Recopilar datos de todos los departamentos</div>
                                <div class="task-dates">Inicio: 17/05/2023 | Fin: 19/05/2023</div>
                            </div>
                            <div class="task-priority priority-medium">Media</div>
                            <div class="task-category">Trabajo</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="5">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="5">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="task-item" data-priority="low" data-task-id="6">
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">Comprar suministros de oficina</div>
                                <div class="task-description">Papel, bolígrafos, carpetas</div>
                                <div class="task-dates">Inicio: 18/05/2023 | Fin: 18/05/2023</div>
                            </div>
                            <div class="task-priority priority-low">Baja</div>
                            <div class="task-category">Trabajo</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="6">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="6">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Calendar Page -->
            <div id="calendar" class="page-content" style="display: none;">
                <div class="header">
                    <h1 class="page-title">Calendario</h1>
                </div>
                
                <div class="calendar-container">
                    <div class="calendar-header">
                        <h2 class="card-title">Calendario Dinámico</h2>
                        <div class="calendar-nav">
                            <button class="calendar-btn" id="prevYear"><i class="fas fa-angle-double-left"></i></button>
                            <button class="calendar-btn" id="prevMonth"><i class="fas fa-chevron-left"></i></button>
                            
                            <select class="calendar-select" id="yearSelect">
                                <!-- Years will be populated by JavaScript -->
                            </select>
                            
                            <select class="calendar-select" id="monthSelect">
                                <option value="0">Enero</option>
                                <option value="1">Febrero</option>
                                <option value="2">Marzo</option>
                                <option value="3">Abril</option>
                                <option value="4">Mayo</option>
                                <option value="5">Junio</option>
                                <option value="6">Julio</option>
                                <option value="7">Agosto</option>
                                <option value="8">Septiembre</option>
                                <option value="9">Octubre</option>
                                <option value="10">Noviembre</option>
                                <option value="11">Diciembre</option>
                            </select>
                            
                            <button class="calendar-btn" id="nextMonth"><i class="fas fa-chevron-right"></i></button>
                            <button class="calendar-btn" id="nextYear"><i class="fas fa-angle-double-right"></i></button>
                            
                            <div class="calendar-view-toggle">
                                <button class="view-btn active" data-view="month">Mes</button>
                                <button class="view-btn" data-view="week">Semana</button>
                                <button class="view-btn" data-view="day">Día</button>
                            </div>
                        </div>
                    </div>
                    
                    <div id="calendarView">
                        <!-- Calendar will be populated by JavaScript -->
                    </div>
                </div>
            </div>
            
            <!-- Charts Page -->
            <div id="charts" class="page-content" style="display: none;">
                <div class="header">
                    <h1 class="page-title">Gráficas</h1>
                    <div class="header-actions">
                        <button class="btn" id="refreshChartsBtn">
                            <i class="fas fa-sync-alt"></i>
                            Actualizar
                        </button>
                    </div>
                </div>
                
                <div class="charts-container">
                    <div class="chart-card">
                        <h3 class="card-title">Tareas Completadas vs. Pendientes</h3>
                        <canvas id="tasksChart" width="400" height="300"></canvas>
                    </div>
                    
                    <div class="chart-card">
                        <h3 class="card-title">Distribución por Prioridades</h3>
                        <canvas id="priorityChart" width="400" height="300"></canvas>
                    </div>
                </div>
                
                <div class="charts-container">
                    <div class="chart-card">
                        <h3 class="card-title">Tareas Completadas Diarias</h3>
                        <canvas id="dailyTasksChart" width="400" height="300"></canvas>
                    </div>
                    
                    <div class="chart-card">
                        <h3 class="card-title">Tareas Completadas Mensuales</h3>
                        <canvas id="monthlyTasksChart" width="400" height="300"></canvas>
                    </div>
                </div>
                
                <div class="charts-container">
                    <div class="chart-card">
                        <h3 class="card-title">Rendimiento por Categoría</h3>
                        <canvas id="categoryPerformanceChart" width="400" height="300"></canvas>
                    </div>
                    
                    <div class="chart-card">
                        <h3 class="card-title">Productividad por Semana</h3>
                        <canvas id="productivityChart" width="400" height="300"></canvas>
                    </div>
                </div>
                
                <div class="feedback-message feedback-success">
                    <i class="fas fa-check-circle"></i> ¡Genial! Cumpliste el 80% de tus tareas esta semana.
                </div>
            </div>
            
            <!-- Finance Page -->
            <div id="finance" class="page-content" style="display: none;">
                <div class="header">
                    <h1 class="page-title">Gestión de Finanzas</h1>
                    <div class="header-actions">
                        <button class="btn" id="newTransactionBtn">
                            <i class="fas fa-plus"></i>
                            Nueva Transacción
                        </button>
                        <div class="export-options">
                            <button class="export-btn" id="exportFinanceExcelBtn">
                                <i class="fas fa-file-excel"></i>
                                Exportar a Excel
                            </button>
                        </div>
                    </div>
                </div>
                
                <div class="finance-container">
                    <div class="finance-summary">
                        <div class="summary-item">
                            <div class="summary-value income" id="totalIncome">$3,200</div>
                            <div>Ingresos</div>
                        </div>
                        
                        <div class="summary-item">
                            <div class="summary-value expense" id="totalExpense">$750</div>
                            <div>Gastos</div>
                        </div>
                        
                        <div class="summary-item">
                            <div class="summary-value" id="totalBalance" style="color: var(--success);">$2,450</div>
                            <div>Balance</div>
                        </div>
                    </div>
                    
                    <h3 class="card-title">Transacciones Recientes</h3>
                    
                    <div class="transaction-cards" id="transactionCards">
                        <div class="transaction-card income" data-transaction-id="1">
                            <div class="transaction-header">
                                <div class="transaction-title">Salario</div>
                                <div class="transaction-amount income">+$3,200</div>
                            </div>
                            <div class="transaction-details">
                                <div>Trabajo</div>
                                <div>01/05/2023</div>
                            </div>
                            <div class="transaction-actions">
                                <button class="transaction-action-btn edit" data-transaction-id="1">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="transaction-action-btn delete" data-transaction-id="1">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="transaction-card expense" data-transaction-id="2">
                            <div class="transaction-header">
                                <div class="transaction-title">Supermercado</div>
                                <div class="transaction-amount expense">-$150</div>
                            </div>
                            <div class="transaction-details">
                                <div>Alimentación</div>
                                <div>03/05/2023</div>
                            </div>
                            <div class="transaction-actions">
                                <button class="transaction-action-btn edit" data-transaction-id="2">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="transaction-action-btn delete" data-transaction-id="2">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="transaction-card expense" data-transaction-id="3">
                            <div class="transaction-header">
                                <div class="transaction-title">Transporte</div>
                                <div class="transaction-amount expense">-$80</div>
                            </div>
                            <div class="transaction-details">
                                <div>Transporte</div>
                                <div>05/05/2023</div>
                            </div>
                            <div class="transaction-actions">
                                <button class="transaction-action-btn edit" data-transaction-id="3">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="transaction-action-btn delete" data-transaction-id="3">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="transaction-card expense" data-transaction-id="4">
                            <div class="transaction-header">
                                <div class="transaction-title">Cine</div>
                                <div class="transaction-amount expense">-$40</div>
                            </div>
                            <div class="transaction-details">
                                <div>Entretenimiento</div>
                                <div>07/05/2023</div>
                            </div>
                            <div class="transaction-actions">
                                <button class="transaction-action-btn edit" data-transaction-id="4">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="transaction-action-btn delete" data-transaction-id="4">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                    
                    <div class="charts-container">
                        <div class="chart-card">
                            <h3 class="card-title">Ingresos vs. Gastos</h3>
                            <canvas id="incomeExpenseChart" width="400" height="200"></canvas>
                        </div>
                        
                        <div class="chart-card">
                            <h3 class="card-title">Flujo Mensual</h3>
                            <canvas id="cashFlowChart" width="400" height="200"></canvas>
                        </div>
                    </div>
                    
                    <div class="chart-card">
                        <h3 class="card-title">Distribución de Gastos</h3>
                        <canvas id="expenseDistributionChart" width="400" height="200"></canvas>
                    </div>
                    
                    <div class="feedback-message feedback-warning">
                        <i class="fas fa-exclamation-triangle"></i> Has superado tu presupuesto en transporte.
                    </div>
                    
                    <div class="feedback-message feedback-success">
                        <i class="fas fa-piggy-bank"></i> Tus ahorros aumentaron un 20% este mes.
                    </div>
                </div>
            </div>
            
            <!-- Projects Page -->
            <div id="projects" class="page-content" style="display: none;">
                <div class="header">
                    <h1 class="page-title">Proyectos</h1>
                    <div class="header-actions">
                        <button class="btn" id="newProjectBtn">
                            <i class="fas fa-plus"></i>
                            Nuevo Proyecto
                        </button>
                    </div>
                </div>
                
                <div class="projects-container">
                    <div class="projects-grid" id="projectsGrid">
                        <div class="project-card">
                            <div class="project-header">
                                <div class="project-title">Lanzamiento de Curso Online</div>
                                <div class="project-status">En Progreso</div>
                            </div>
                            
                            <div class="project-progress">
                                <div class="progress-bar">
                                    <div class="progress-fill" style="width: 65%;"></div>
                                </div>
                                <div class="progress-text">
                                    <span>65% Completado</span>
                                    <span>13 de 20 tareas</span>
                                </div>
                            </div>
                            
                            <div class="project-tasks">
                                <div class="project-task">
                                    <div class="project-task-checkbox checked"></div>
                                    <div class="project-task-title">Crear contenido del curso</div>
                                    <div class="project-task-priority priority-high">Alta</div>
                                </div>
                                
                                <div class="project-task">
                                    <div class="project-task-checkbox checked"></div>
                                    <div class="project-task-title">Grabar videos</div>
                                    <div class="project-task-priority priority-high">Alta</div>
                                </div>
                                
                                <div class="project-task">
                                    <div class="project-task-checkbox"></div>
                                    <div class="project-task-title">Diseñar página de ventas</div>
                                    <div class="project-task-priority priority-medium">Media</div>
                                </div>
                                
                                <div class="project-task">
                                    <div class="project-task-checkbox"></div>
                                    <div class="project-task-title">Configurar plataforma</div>
                                    <div class="project-task-priority priority-medium">Media</div>
                                </div>
                            </div>
                        </div>
                        
                        <div class="project-card">
                            <div class="project-header">
                                <div class="project-title">Rediseño Web Corporativo</div>
                                <div class="project-status">En Progreso</div>
                            </div>
                            
                            <div class="project-progress">
                                <div class="progress-bar">
                                    <div class="progress-fill" style="width: 40%;"></div>
                                </div>
                                <div class="progress-text">
                                    <span>40% Completado</span>
                                    <span>4 de 10 tareas</span>
                                </div>
                            </div>
                            
                            <div class="project-tasks">
                                <div class="project-task">
                                    <div class="project-task-checkbox checked"></div>
                                    <div class="project-task-title">Investigación de UX</div>
                                    <div class="project-task-priority priority-high">Alta</div>
                                </div>
                                
                                <div class="project-task">
                                    <div class="project-task-checkbox checked"></div>
                                    <div class="project-task-title">Diseño de prototipos</div>
                                    <div class="project-task-priority priority-high">Alta</div>
                                </div>
                                
                                <div class="project-task">
                                    <div class="project-task-checkbox"></div>
                                    <div class="project-task-title">Desarrollo frontend</div>
                                    <div class="project-task-priority priority-medium">Media</div>
                                </div>
                                
                                <div class="project-task">
                                    <div class="project-task-checkbox"></div>
                                    <div class="project-task-title">Pruebas y optimización</div>
                                    <div class="project-task-priority priority-low">Baja</div>
                                </div>
                            </div>
                        </div>
                        
                        <div class="project-card">
                            <div class="project-header">
                                <div class="project-title">Campaña de Marketing</div>
                                <div class="project-status">Planificación</div>
                            </div>
                            
                            <div class="project-progress">
                                <div class="progress-bar">
                                    <div class="progress-fill" style="width: 20%;"></div>
                                </div>
                                <div class="progress-text">
                                    <span>20% Completado</span>
                                    <span>2 de 10 tareas</span>
                                </div>
                            </div>
                            
                            <div class="project-tasks">
                                <div class="project-task">
                                    <div class="project-task-checkbox checked"></div>
                                    <div class="project-task-title">Definir objetivos</div>
                                    <div class="project-task-priority priority-high">Alta</div>
                                </div>
                                
                                <div class="project-task">
                                    <div class="project-task-checkbox checked"></div>
                                    <div class="project-task-title">Investigar mercado</div>
                                    <div class="project-task-priority priority-high">Alta</div>
                                </div>
                                
                                <div class="project-task">
                                    <div class="project-task-checkbox"></div>
                                    <div class="project-task-title">Crear contenido</div>
                                    <div class="project-task-priority priority-medium">Media</div>
                                </div>
                                
                                <div class="project-task">
                                    <div class="project-task-checkbox"></div>
                                    <div class="project-task-title">Lanzar campaña</div>
                                    <div class="project-task-priority priority-medium">Media</div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Reports Page -->
            <div id="reports" class="page-content" style="display: none;">
                <div class="header">
                    <h1 class="page-title">Reportes</h1>
                    <div class="header-actions">
                        <button class="btn" id="generateReportBtn">
                            <i class="fas fa-file-alt"></i>
                            Generar Reporte
                        </button>
                    </div>
                </div>
                
                <div class="reports-container">
                    <div class="reports-grid">
                        <div class="report-card">
                            <div class="report-header">
                                <div class="report-title">Reporte Semanal de Tareas</div>
                                <div class="report-icon">
                                    <i class="fas fa-tasks"></i>
                                </div>
                            </div>
                            <div class="report-description">
                                Resumen de tareas completadas y pendientes durante la semana actual.
                            </div>
                            <div class="report-actions">
                                <button class="report-btn" id="weeklyTasksPdfBtn">Ver PDF</button>
                                <button class="report-btn secondary" id="weeklyTasksExcelBtn">Descargar Excel</button>
                            </div>
                        </div>
                        
                        <div class="report-card">
                            <div class="report-header">
                                <div class="report-title">Reporte Mensual de Finanzas</div>
                                <div class="report-icon">
                                    <i class="fas fa-wallet"></i>
                                </div>
                            </div>
                            <div class="report-description">
                                Análisis detallado de ingresos, gastos y balance del mes actual.
                            </div>
                            <div class="report-actions">
                                <button class="report-btn" id="monthlyFinancePdfBtn">Ver PDF</button>
                                <button class="report-btn secondary" id="monthlyFinanceExcelBtn">Descargar Excel</button>
                            </div>
                        </div>
                        
                        <div class="report-card">
                            <div class="report-header">
                                <div class="report-title">Reporte de Productividad</div>
                                <div class="report-icon">
                                    <i class="fas fa-chart-line"></i>
                                </div>
                            </div>
                            <div class="report-description">
                                Análisis de rendimiento y productividad por categorías y proyectos.
                            </div>
                            <div class="report-actions">
                                <button class="report-btn" id="productivityPdfBtn">Ver PDF</button>
                                <button class="report-btn secondary" id="productivityExcelBtn">Descargar Excel</button>
                            </div>
                        </div>
                        
                        <div class="report-card">
                            <div class="report-header">
                                <div class="report-title">Reporte de Proyectos</div>
                                <div class="report-icon">
                                    <i class="fas fa-project-diagram"></i>
                                </div>
                            </div>
                            <div class="report-description">
                                Estado actual de todos los proyectos con avance y próximos hitos.
                            </div>
                            <div class="report-actions">
                                <button class="report-btn" id="projectsPdfBtn">Ver PDF</button>
                                <button class="report-btn secondary" id="projectsExcelBtn">Descargar Excel</button>
                            </div>
                        </div>
                    </div>
                    
                    <div class="card" style="margin-top: 20px;">
                        <div class="card-header">
                            <h3 class="card-title">Reportes Automáticos</h3>
                        </div>
                        <div class="settings-item">
                            <div>
                                <div class="settings-label">Reporte semanal de tareas</div>
                                <div class="settings-description">Recibir cada lunes un resumen de las tareas de la semana anterior</div>
                            </div>
                            <label class="switch">
                                <input type="checkbox" checked>
                                <span class="slider"></span>
                            </label>
                        </div>
                        
                        <div class="settings-item">
                            <div>
                                <div class="settings-label">Reporte mensual de finanzas</div>
                                <div class="settings-description">Recibir el primer día de cada mes un resumen financiero</div>
                            </div>
                            <label class="switch">
                                <input type="checkbox" checked>
                                <span class="slider"></span>
                            </label>
                        </div>
                        
                        <div class="settings-item">
                            <div>
                                <div class="settings-label">Reporte de productividad</div>
                                <div class="settings-description">Recibir un análisis mensual de tu productividad</div>
                            </div>
                            <label class="switch">
                                <input type="checkbox">
                                <span class="slider"></span>
                            </label>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Users Page -->
            <div id="users" class="page-content" style="display: none;">
                <div class="header">
                    <h1 class="page-title">Gestión de Usuarios</h1>
                    <div class="header-actions">
                        <button class="btn" id="inviteUserBtn">
                            <i class="fas fa-user-plus"></i>
                            Invitar Usuario
                        </button>
                    </div>
                </div>
                
                <div class="users-container">
                    <div class="users-header">
                        <h2 class="card-title">Usuarios del Sistema</h2>
                    </div>
                    
                    <div class="users-grid" id="usersGrid">
                        <div class="user-card">
                            <div class="user-card-header">
                                <div class="user-card-avatar">CL</div>
                                <div class="user-card-info">
                                    <div class="user-card-name">César López</div>
                                    <div class="user-card-email">cesar@ejemplo.com</div>
                                </div>
                            </div>
                            <div class="user-card-code">USR-CL-9834</div>
                            <div class="user-card-permissions">
                                <div class="permission-badge">Ver</div>
                                <div class="permission-badge">Editar</div>
                                <div class="permission-badge">Admin</div>
                            </div>
                            <div class="user-card-actions">
                                <button class="user-card-btn">Editar</button>
                                <button class="user-card-btn danger">Eliminar</button>
                            </div>
                        </div>
                        
                        <div class="user-card">
                            <div class="user-card-header">
                                <div class="user-card-avatar">MG</div>
                                <div class="user-card-info">
                                    <div class="user-card-name">María García</div>
                                    <div class="user-card-email">maria@ejemplo.com</div>
                                </div>
                            </div>
                            <div class="user-card-code">USR-MG-5621</div>
                            <div class="user-card-permissions">
                                <div class="permission-badge">Ver</div>
                                <div class="permission-badge">Editar</div>
                            </div>
                            <div class="user-card-actions">
                                <button class="user-card-btn">Editar</button>
                                <button class="user-card-btn danger">Eliminar</button>
                            </div>
                        </div>
                        
                        <div class="user-card">
                            <div class="user-card-header">
                                <div class="user-card-avatar">JR</div>
                                <div class="user-card-info">
                                    <div class="user-card-name">Juan Rodríguez</div>
                                    <div class="user-card-email">juan@ejemplo.com</div>
                                </div>
                            </div>
                            <div class="user-card-code">USR-JR-7845</div>
                            <div class="user-card-permissions">
                                <div class="permission-badge">Ver</div>
                            </div>
                            <div class="user-card-actions">
                                <button class="user-card-btn">Editar</button>
                                <button class="user-card-btn danger">Eliminar</button>
                            </div>
                        </div>
                    </div>
                    
                    <div class="card" style="margin-top: 20px;">
                        <div class="card-header">
                            <h3 class="card-title">Invitar Usuario</h3>
                        </div>
                        <div class="form-group">
                            <label class="form-label">Código de Usuario</label>
                            <input type="text" class="form-control" id="inviteUserCode" placeholder="Ingresa el código de usuario">
                        </div>
                        <div class="form-group">
                            <button class="btn" id="sendInviteBtn">Invitar Usuario</button>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Notes Page -->
            <div id="notes" class="page-content" style="display: none;">
                <div class="header">
                    <h1 class="page-title">Notas Rápidas</h1>
                    <div class="header-actions">
                        <button class="btn" id="newNoteBtn">
                            <i class="fas fa-plus"></i>
                            Nueva Nota
                        </button>
                    </div>
                </div>
                
                <div class="notes-container">
                    <div class="notes-grid" id="notesGrid">
                        <div class="note" data-note-id="2">
                            <div class="note-category">Trabajo</div>
                            <i class="fas fa-thumbtack note-pin"></i>
                            <div class="note-title">Ideas para el proyecto</div>
                            <div class="note-content">Implementar sistema de notificaciones push para recordatorios de tareas.</div>
                            <div class="note-date">17/05/2023</div>
                        </div>
                        
                        <div class="note pinned" data-note-id="1">
                            <div class="note-category">Trabajo</div>
                            <i class="fas fa-thumbtack note-pin pinned"></i>
                            <div class="note-title">Reunión importante</div>
                            <div class="note-content">Preparar documentación para la reunión con inversores el viernes.</div>
                            <div class="note-date">16/05/2023</div>
                        </div>
                        
                        <div class="note" data-note-id="3">
                            <div class="note-category">Personal</div>
                            <i class="fas fa-thumbtack note-pin"></i>
                            <div class="note-title">Compras</div>
                            <div class="note-content">Comprar: café, papel, bolígrafos, cargador para el portátil.</div>
                            <div class="note-date">15/05/2023</div>
                        </div>
                        
                        <div class="note" data-note-id="4">
                            <div class="note-category">Personal</div>
                            <i class="fas fa-thumbtack note-pin"></i>
                            <div class="note-title">Recordatorio</div>
                            <div class="note-content">Renovar suscripción a herramientas de diseño antes del 30 de mayo.</div>
                            <div class="note-date">14/05/2023</div>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Settings Page -->
            <div id="settings" class="page-content" style="display: none;">
                <div class="header">
                    <h1 class="page-title">Configuración</h1>
                </div>
                
                <div class="settings-section">
                    <h3 class="settings-title">Preferencias</h3>
                    <div class="settings-item">
                        <div>
                            <div class="settings-label">Modo Oscuro</div>
                            <div class="settings-description">Cambiar entre tema claro y oscuro</div>
                        </div>
                        <label class="switch">
                            <input type="checkbox" id="darkModeSwitch">
                            <span class="slider"></span>
                        </label>
                    </div>
                    
                    <div class="settings-item">
                        <div>
                            <div class="settings-label">Notificaciones</div>
                            <div class="settings-description">Recibir recordatorios de tareas</div>
                        </div>
                        <label class="switch">
                            <input type="checkbox" checked>
                            <span class="slider"></span>
                        </label>
                    </div>
                    
                    <div class="settings-item">
                        <div>
                            <div class="settings-label">Modo Colaborativo</div>
                            <div class="settings-description">Permitir que otros usuarios editen</div>
                        </div>
                        <label class="switch">
                            <input type="checkbox">
                            <span class="slider"></span>
                        </label>
                    </div>
                </div>
                
                <div class="settings-section">
                    <h3 class="settings-title">Tema Visual</h3>
                    <div class="settings-item">
                        <div>
                            <div class="settings-label">Color Principal</div>
                            <div class="settings-description">Selecciona el color principal de la interfaz</div>
                        </div>
                        <div class="color-options">
                            <button class="color-option" data-theme="default" style="background-color: var(--primary);"></button>
                            <button class="color-option" data-theme="blue" style="background-color: #4A6FA5;"></button>
                            <button class="color-option" data-theme="green" style="background-color: #4CAF50;"></button>
                            <button class="color-option" data-theme="purple" style="background-color: #9C27B0;"></button>
                            <button class="color-option" data-theme="red" style="background-color: #F44336;"></button>
                        </div>
                    </div>
                </div>
                
                <div class="settings-section">
                    <h3 class="settings-title">Gestión de Usuarios</h3>
                    <div class="settings-item">
                        <div>
                            <div class="settings-label">Invitar Usuario</div>
                            <div class="settings-description">Añadir nuevos miembros al equipo</div>
                        </div>
                        <button class="btn">
                            <i class="fas fa-user-plus"></i>
                            Invitar
                        </button>
                    </div>
                    
                    <div class="settings-item">
                        <div>
                            <div class="settings-label">Lista de Usuarios</div>
                            <div class="settings-description">Ver y gestionar usuarios existentes</div>
                        </div>
                        <button class="btn">
                            <i class="fas fa-users"></i>
                            Ver
                        </button>
                    </div>
                </div>
                
                <div class="settings-section">
                    <h3 class="settings-title">Personalización de Categorías</h3>
                    <div class="category-list">
                        <div class="category-item">
                            <div class="category-color" style="background-color: var(--secondary);"></div>
                            <div class="category-name">Trabajo</div>
                            <i class="fas fa-times category-delete"></i>
                        </div>
                        <div class="category-item">
                            <div class="category-color" style="background-color: var(--primary);"></div>
                            <div class="category-name">Personal</div>
                            <i class="fas fa-times category-delete"></i>
                        </div>
                        <div class="category-item">
                            <div class="category-color" style="background-color: var(--success);"></div>
                            <div class="category-name">Salud</div>
                            <i class="fas fa-times category-delete"></i>
                        </div>
                        <div class="category-item">
                            <div class="category-color" style="background-color: var(--warning);"></div>
                            <div class="category-name">Estudio</div>
                            <i class="fas fa-times category-delete"></i>
                        </div>
                    </div>
                    
                    <button class="btn" id="addCategoryBtn">
                        <i class="fas fa-plus"></i>
                        Nueva Categoría
                    </button>
                </div>
                
                <div class="settings-section">
                    <h3 class="settings-title">Integraciones</h3>
                    <div class="settings-item">
                        <div>
                            <div class="settings-label">Google Calendar</div>
                            <div class="settings-description">Sincronizar eventos con tu calendario de Google</div>
                        </div>
                        <label class="switch">
                            <input type="checkbox">
                            <span class="slider"></span>
                        </label>
                    </div>
                    
                    <div class="settings-item">
                        <div>
                            <div class="settings-label">Gmail</div>
                            <div class="settings-description">Recibir recordatorios y notificaciones por correo</div>
                        </div>
                        <label class="switch">
                            <input type="checkbox">
                            <span class="slider"></span>
                        </label>
                    </div>
                    
                    <div class="settings-item">
                        <div>
                            <div class="settings-label">Slack</div>
                            <div class="settings-description">Enviar notificaciones a tu espacio de trabajo de Slack</div>
                        </div>
                        <label class="switch">
                            <input type="checkbox">
                            <span class="slider"></span>
                        </label>
                    </div>
                </div>
                
                <div class="settings-section">
                    <h3 class="settings-title">Automatizaciones</h3>
                    <div class="settings-item">
                        <div>
                            <div class="settings-label">Recordatorios de tareas urgentes</div>
                            <div class="settings-description">Enviar notificación si una tarea de alta prioridad no se completa</div>
                        </div>
                        <label class="switch">
                            <input type="checkbox" checked>
                            <span class="slider"></span>
                        </label>
                    </div>
                    
                    <div class="settings-item">
                        <div>
                            <div class="settings-label">Reporte semanal automático</div>
                            <div class="settings-description">Generar y enviar reporte semanal cada lunes</div>
                        </div>
                        <label class="switch">
                            <input type="checkbox">
                            <span class="slider"></span>
                        </label>
                    </div>
                </div>
                
                <div class="settings-section">
                    <h3 class="settings-title">Exportar Datos</h3>
                    <div class="export-options">
                        <button class="export-btn" id="exportTasksExcelBtn">
                            <i class="fas fa-file-excel"></i>
                            Exportar Tareas a Excel
                        </button>
                        <button class="export-btn" id="exportTasksPdfBtn">
                            <i class="fas fa-file-pdf"></i>
                            Exportar Tareas a PDF
                        </button>
                        <button class="export-btn" id="exportFinanceExcelBtn">
                            <i class="fas fa-file-excel"></i>
                            Exportar Finanzas a Excel
                        </button>
                        <button class="export-btn" id="exportFinancePdfBtn">
                            <i class="fas fa-file-pdf"></i>
                            Exportar Finanzas a PDF
                        </button>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <!-- Notification -->
    <div class="notification" id="notification">
        <i class="fas fa-bell notification-icon"></i>
        <span id="notificationText">Nueva tarea urgente agregada</span>
        <i class="fas fa-times notification-close" id="notificationClose"></i>
    </div>

    <!-- Modal for Calendar Day Details -->
    <div class="modal" id="calendarDayModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 class="modal-title" id="modalDayTitle">Tareas del día</h3>
                <i class="fas fa-times modal-close" id="modalClose"></i>
            </div>
            <div class="task-list" id="modalDayTasks">
                <!-- Tasks will be populated by JavaScript -->
            </div>
        </div>
    </div>

    <!-- Modal for New Transaction -->
    <div class="modal" id="transactionModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 class="modal-title">Nueva Transacción</h3>
                <i class="fas fa-times modal-close" id="transactionModalClose"></i>
            </div>
            
            <div class="form-group">
                <label class="form-label">Tipo</label>
                <select class="form-control" id="transactionType">
                    <option value="income">Ingreso</option>
                    <option value="expense">Egreso</option>
                </select>
            </div>
            
            <div class="form-group">
                <label class="form-label">Descripción</label>
                <input type="text" class="form-control" id="transactionDescription" placeholder="Descripción">
            </div>
            
            <div class="form-group">
                <label class="form-label">Categoría</label>
                <select class="form-control" id="transactionCategory">
                    <option value="food">Alimentación</option>
                    <option value="transport">Transporte</option>
                    <option value="entertainment">Entretenimiento</option>
                    <option value="health">Salud</option>
                    <option value="education">Educación</option>
                    <option value="investment">Inversión</option>
                    <option value="other">Otros</option>
                </select>
            </div>
            
            <div class="form-group">
                <label class="form-label">Monto</label>
                <input type="number" class="form-control" id="transactionAmount" placeholder="0.00">
            </div>
            
            <div class="form-group">
                <label class="form-label">Fecha</label>
                <input type="date" class="form-control" id="transactionDate">
            </div>
            
            <div class="form-group">
                <button class="btn" id="saveTransactionBtn">Guardar Transacción</button>
                <button class="btn" id="cancelTransactionBtn" style="background-color: var(--gray-light); color: var(--dark);">Cancelar</button>
            </div>
        </div>
    </div>

    <!-- Modal for Edit Transaction -->
    <div class="modal" id="editTransactionModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 class="modal-title">Editar Transacción</h3>
                <i class="fas fa-times modal-close" id="editTransactionModalClose"></i>
            </div>
            
            <div class="form-group">
                <label class="form-label">Tipo</label>
                <select class="form-control" id="editTransactionType">
                    <option value="income">Ingreso</option>
                    <option value="expense">Egreso</option>
                </select>
            </div>
            
            <div class="form-group">
                <label class="form-label">Descripción</label>
                <input type="text" class="form-control" id="editTransactionDescription" placeholder="Descripción">
            </div>
            
            <div class="form-group">
                <label class="form-label">Categoría</label>
                <select class="form-control" id="editTransactionCategory">
                    <option value="food">Alimentación</option>
                    <option value="transport">Transporte</option>
                    <option value="entertainment">Entretenimiento</option>
                    <option value="health">Salud</option>
                    <option value="education">Educación</option>
                    <option value="investment">Inversión</option>
                    <option value="other">Otros</option>
                </select>
            </div>
            
            <div class="form-group">
                <label class="form-label">Monto</label>
                <input type="number" class="form-control" id="editTransactionAmount" placeholder="0.00">
            </div>
            
            <div class="form-group">
                <label class="form-label">Fecha</label>
                <input type="date" class="form-control" id="editTransactionDate">
            </div>
            
            <div class="form-group">
                <button class="btn" id="updateTransactionBtn">Actualizar Transacción</button>
                <button class="btn" id="cancelEditTransactionBtn" style="background-color: var(--gray-light); color: var(--dark);">Cancelar</button>
            </div>
        </div>
    </div>

    <!-- Modal for New Note -->
    <div class="modal" id="noteModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 class="modal-title">Nueva Nota</h3>
                <i class="fas fa-times modal-close" id="noteModalClose"></i>
            </div>
            
            <div class="form-group">
                <label class="form-label">Título</label>
                <input type="text" class="form-control" id="noteTitle" placeholder="Título de la nota">
            </div>
            
            <div class="form-group">
                <label class="form-label">Contenido</label>
                <textarea class="form-control" id="noteContent" rows="5" placeholder="Escribe aquí tu nota..."></textarea>
            </div>
            
            <div class="form-group">
                <label class="form-label">Categoría</label>
                <select class="form-control" id="noteCategory">
                    <option value="work">Trabajo</option>
                    <option value="personal">Personal</option>
                    <option value="health">Salud</option>
                    <option value="study">Estudio</option>
                    <option value="other">Otros</option>
                </select>
            </div>
            
            <div class="form-group">
                <label class="form-label">
                    <input type="checkbox" id="notePinned"> Anclar nota
                </label>
            </div>
            
            <div class="form-group">
                <button class="btn" id="saveNoteBtn">Guardar Nota</button>
                <button class="btn" id="cancelNoteBtn" style="background-color: var(--gray-light); color: var(--dark);">Cancelar</button>
            </div>
        </div>
    </div>

    <!-- Modal for Edit Note -->
    <div class="modal" id="editNoteModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 class="modal-title">Editar Nota</h3>
                <i class="fas fa-times modal-close" id="editNoteModalClose"></i>
            </div>
            
            <div class="form-group">
                <label class="form-label">Título</label>
                <input type="text" class="form-control" id="editNoteTitle" placeholder="Título de la nota">
            </div>
            
            <div class="form-group">
                <label class="form-label">Contenido</label>
                <textarea class="form-control" id="editNoteContent" rows="5" placeholder="Escribe aquí tu nota..."></textarea>
            </div>
            
            <div class="form-group">
                <label class="form-label">Categoría</label>
                <select class="form-control" id="editNoteCategory">
                    <option value="work">Trabajo</option>
                    <option value="personal">Personal</option>
                    <option value="health">Salud</option>
                    <option value="study">Estudio</option>
                    <option value="other">Otros</option>
                </select>
            </div>
            
            <div class="form-group">
                <label class="form-label">
                    <input type="checkbox" id="editNotePinned"> Anclar nota
                </label>
            </div>
            
            <div class="form-group">
                <button class="btn" id="updateNoteBtn">Actualizar Nota</button>
                <button class="btn" id="cancelEditNoteBtn" style="background-color: var(--gray-light); color: var(--dark);">Cancelar</button>
            </div>
        </div>
    </div>

    <!-- Modal for New Category -->
    <div class="modal" id="categoryModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 class="modal-title">Nueva Categoría</h3>
                <i class="fas fa-times modal-close" id="categoryModalClose"></i>
            </div>
            
            <div class="form-group">
                <label class="form-label">Nombre de Categoría</label>
                <input type="text" class="form-control" id="categoryName" placeholder="Nombre de la categoría">
            </div>
            
            <div class="form-group">
                <label class="form-label">Color</label>
                <div class="color-options">
                    <button class="color-option" data-color="#4A6FA5" style="background-color: #4A6FA5;"></button>
                    <button class="color-option" data-color="#4CAF50" style="background-color: #4CAF50;"></button>
                    <button class="color-option" data-color="#FFC107" style="background-color: #FFC107;"></button>
                    <button class="color-option" data-color="#9C27B0" style="background-color: #9C27B0;"></button>
                    <button class="color-option" data-color="#F44336" style="background-color: #F44336;"></button>
                    <button class="color-option" data-color="#FF9800" style="background-color: #FF9800;"></button>
                    <button class="color-option" data-color="#795548" style="background-color: #795548;"></button>
                    <button class="color-option" data-color="#607D8B" style="background-color: #607D8B;"></button>
                </div>
            </div>
            
            <div class="form-group">
                <button class="btn" id="saveCategoryBtn">Guardar Categoría</button>
                <button class="btn" id="cancelCategoryBtn" style="background-color: var(--gray-light); color: var(--dark);">Cancelar</button>
            </div>
        </div>
    </div>

    <script>
        // Global variables for optimization
        let chartsInitialized = false;
        let calendarInitialized = false;
        let financeChartsInitialized = false;
        let taskIdCounter = 10; // Starting from 10 since we already have tasks with IDs 1-9
        let transactionIdCounter = 5; // Starting from 5 since we already have transactions with IDs 1-4
        let noteIdCounter = 5; // Starting from 5 since we already have notes with IDs 1-4
        let currentUser = null;
        let currentEditingTaskId = null;
        let currentEditingTransactionId = null;
        let currentEditingNoteId = null;
        let selectedCategoryColor = '#4A6FA5';
        
        // Initialize app
        document.addEventListener('DOMContentLoaded', function() {
            // Initialize authentication
            initAuthentication();
            
            // Check if user is logged in
            if (currentUser) {
                showApp();
                
                // Initialize navigation
                initNavigation();
                
                // Initialize dark mode
                initDarkMode();
                
                // Initialize task management
                initTaskManagement();
                
                // Initialize calendar
                initCalendar();
                
                // Initialize notes
                initNotes();
                
                // Initialize search
                initSearch();
                
                // Initialize sortable elements
                initSortable();
                
                // Initialize notifications
                initNotifications();
                
                // Initialize modals
                initModals();
                
                // Initialize projects
                initProjects();
                
                // Initialize finance
                initFinance();
                
                // Initialize user management
                initUserManagement();
                
                // Initialize reports
                initReports();
                
                // Initialize settings
                initSettings();
                
                // Initialize profile
                initProfile();
            }
        });
        
        // Authentication functionality
        function initAuthentication() {
            const authTabs = document.querySelectorAll('.auth-tab');
            const loginForm = document.getElementById('loginForm');
            const registerForm = document.getElementById('registerForm');
            const socialBtns = document.querySelectorAll('.social-btn');
            
            // Check if user is already logged in (for demo purposes)
            const savedUser = localStorage.getItem('currentUser');
            if (savedUser) {
                currentUser = JSON.parse(savedUser);
            }
            
            // Auth tabs functionality
            authTabs.forEach(tab => {
                tab.addEventListener('click', function() {
                    // Remove active class from all tabs
                    authTabs.forEach(t => t.classList.remove('active'));
                    
                    // Add active class to clicked tab
                    this.classList.add('active');
                    
                    // Show corresponding form
                    const tabName = this.getAttribute('data-tab');
                    if (tabName === 'login') {
                        loginForm.classList.add('active');
                        registerForm.classList.remove('active');
                    } else {
                        loginForm.classList.remove('active');
                        registerForm.classList.add('active');
                    }
                });
            });
            
            // Login form functionality
            if (loginForm) {
                loginForm.addEventListener('submit', function(e) {
                    e.preventDefault();
                    
                    const email = this.querySelector('input[type="email"]').value;
                    const password = this.querySelector('input[type="password"]').value;
                    
                    // For demo purposes, we'll accept any email/password combination
                    if (email && password) {
                        // Create user object
                        const user = {
                            name: 'César López',
                            email: email,
                            code: 'USR-CL-9834',
                            avatar: 'CL'
                        };
                        
                        // Save user to localStorage
                        localStorage.setItem('currentUser', JSON.stringify(user));
                        currentUser = user;
                        
                        // Show app
                        showApp();
                        
                        // Initialize app components
                        initNavigation();
                        initDarkMode();
                        initTaskManagement();
                        initCalendar();
                        initNotes();
                        initSearch();
                        initSortable();
                        initNotifications();
                        initModals();
                        initProjects();
                        initFinance();
                        initUserManagement();
                        initReports();
                        initSettings();
                        initProfile();
                    }
                });
            }
            
            // Register form functionality
            if (registerForm) {
                registerForm.addEventListener('submit', function(e) {
                    e.preventDefault();
                    
                    const name = this.querySelector('input[type="text"]').value;
                    const email = this.querySelector('input[type="email"]').value;
                    const password = this.querySelector('input[type="password"]').value;
                    const confirmPassword = this.querySelectorAll('input[type="password"]')[1].value;
                    
                    // For demo purposes, we'll accept any form data
                    if (name && email && password && password === confirmPassword) {
                        // Generate user code
                        const initials = name.split(' ').map(n => n[0]).join('').toUpperCase();
                        const randomCode = Math.floor(Math.random() * 10000);
                        const userCode = `USR-${initials}-${randomCode}`;
                        
                        // Create user object
                        const user = {
                            name: name,
                            email: email,
                            code: userCode,
                            avatar: initials
                        };
                        
                        // Save user to localStorage
                        localStorage.setItem('currentUser', JSON.stringify(user));
                        currentUser = user;
                        
                        // Show app
                        showApp();
                        
                        // Initialize app components
                        initNavigation();
                        initDarkMode();
                        initTaskManagement();
                        initCalendar();
                        initNotes();
                        initSearch();
                        initSortable();
                        initNotifications();
                        initModals();
                        initProjects();
                        initFinance();
                        initUserManagement();
                        initReports();
                        initSettings();
                        initProfile();
                    }
                });
            }
            
            // Social login functionality
            socialBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    const provider = this.classList.contains('google') ? 'Google' : 'Apple';
                    
                    // For demo purposes, we'll just create a user with the provider name
                    const user = {
                        name: `Usuario de ${provider}`,
                        email: `usuario@${provider.toLowerCase()}.com`,
                        code: `USR-${provider.substring(0, 2).toUpperCase()}-${Math.floor(Math.random() * 10000)}`,
                        avatar: provider.substring(0, 2).toUpperCase()
                    };
                    
                    // Save user to localStorage
                    localStorage.setItem('currentUser', JSON.stringify(user));
                    currentUser = user;
                    
                    // Show app
                    showApp();
                    
                    // Initialize app components
                    initNavigation();
                    initDarkMode();
                    initTaskManagement();
                    initCalendar();
                    initNotes();
                    initSearch();
                    initSortable();
                    initNotifications();
                    initModals();
                    initProjects();
                    initFinance();
                    initUserManagement();
                    initReports();
                    initSettings();
                    initProfile();
                });
            });
        }
        
        // Show app and hide auth
        function showApp() {
            const authContainer = document.getElementById('authContainer');
            const appContainer = document.getElementById('appContainer');
            
            if (authContainer && appContainer) {
                authContainer.style.display = 'none';
                appContainer.classList.add('active');
                
                // Update user profile in sidebar
                if (currentUser) {
                    const userAvatar = document.getElementById('userAvatar');
                    const userName = document.getElementById('userName');
                    const userCode = document.getElementById('userCode');
                    const avatarInitials = document.getElementById('avatarInitials');
                    
                    if (userAvatar) userAvatar.textContent = currentUser.avatar;
                    if (userName) userName.textContent = currentUser.name;
                    if (userCode) userCode.textContent = currentUser.code;
                    if (avatarInitials) avatarInitials.textContent = currentUser.avatar;
                }
            }
        }
        
        // Logout functionality
        function logout() {
            localStorage.removeItem('currentUser');
            currentUser = null;
            
            const authContainer = document.getElementById('authContainer');
            const appContainer = document.getElementById('appContainer');
            
            if (authContainer && appContainer) {
                authContainer.style.display = 'flex';
                appContainer.classList.remove('active');
            }
        }
        
        // User profile dropdown
        function toggleUserMenu() {
            const userMenu = document.getElementById('userMenu');
            if (userMenu) {
                userMenu.style.display = userMenu.style.display === 'none' ? 'block' : 'none';
            }
        }
        
        // Navigation functionality
        function initNavigation() {
            const navLinks = document.querySelectorAll('.nav-link');
            const userProfile = document.getElementById('userProfile');
            const logoutBtn = document.getElementById('logoutBtn');
            
            navLinks.forEach(link => {
                link.addEventListener('click', function(e) {
                    e.preventDefault();
                    
                    // Remove active class from all links
                    navLinks.forEach(l => l.classList.remove('active'));
                    
                    // Add active class to clicked link
                    this.classList.add('active');
                    
                    // Get page ID
                    const pageId = this.getAttribute('data-page');
                    
                    // Hide all pages
                    document.querySelectorAll('.page-content').forEach(page => {
                        page.style.display = 'none';
                    });
                    
                    // Show selected page
                    const selectedPage = document.getElementById(pageId);
                    if (selectedPage) {
                        selectedPage.style.display = 'block';
                        
                        // Initialize page-specific functionality
                        switch(pageId) {
                            case 'charts':
                                if (!chartsInitialized) {
                                    initCharts();
                                    chartsInitialized = true;
                                }
                                break;
                            case 'calendar':
                                if (!calendarInitialized) {
                                    renderCalendar();
                                    calendarInitialized = true;
                                }
                                break;
                            case 'finance':
                                if (!financeChartsInitialized) {
                                    initFinanceCharts();
                                    financeChartsInitialized = true;
                                }
                                break;
                            case 'tasks':
                                // Set today's date for daily tasks
                                const today = new Date();
                                const formattedDate = today.toLocaleDateString('es-ES', { 
                                    day: 'numeric', 
                                    month: 'long', 
                                    year: 'numeric' 
                                });
                                const todayDateElement = document.getElementById('todayDate');
                                if (todayDateElement) {
                                    todayDateElement.textContent = formattedDate;
                                }
                                break;
                        }
                    }
                });
            });
            
            // User profile dropdown
            if (userProfile) {
                userProfile.addEventListener('click', toggleUserMenu);
            }
            
            // Logout button
            if (logoutBtn) {
                logoutBtn.addEventListener('click', logout);
            }
        }
        
        // Dark mode functionality
        function initDarkMode() {
            const darkModeToggle = document.getElementById('darkModeToggle');
            const darkModeSwitch = document.getElementById('darkModeSwitch');
            
            function toggleDarkMode() {
                document.body.classList.toggle('dark-mode');
                const isDarkMode = document.body.classList.contains('dark-mode');
                
                // Update icon
                const icon = darkModeToggle.querySelector('i');
                if (icon) {
                    icon.className = isDarkMode ? 'fas fa-sun' : 'fas fa-moon';
                }
                
                // Update switch
                if (darkModeSwitch) {
                    darkModeSwitch.checked = isDarkMode;
                }
                
                // Save preference to localStorage
                localStorage.setItem('darkMode', isDarkMode);
            }
            
            // Load saved preference
            const savedDarkMode = localStorage.getItem('darkMode') === 'true';
            if (savedDarkMode) {
                document.body.classList.add('dark-mode');
                if (darkModeToggle) {
                    darkModeToggle.querySelector('i').className = 'fas fa-sun';
                }
                if (darkModeSwitch) {
                    darkModeSwitch.checked = true;
                }
            }
            
            // Add event listeners
            if (darkModeToggle) {
                darkModeToggle.addEventListener('click', toggleDarkMode);
            }
            
            if (darkModeSwitch) {
                darkModeSwitch.addEventListener('change', toggleDarkMode);
            }
        }
        
        // Profile functionality
        function initProfile() {
            const profileMenuItem = document.getElementById('profileMenuItem');
            const settingsMenuItem = document.getElementById('settingsMenuItem');
            const avatarUpload = document.getElementById('avatarUpload');
            const avatarInput = document.getElementById('avatarInput');
            
            // Profile menu item
            if (profileMenuItem) {
                profileMenuItem.addEventListener('click', function() {
                    // Navigate to settings page
                    const settingsLink = document.querySelector('[data-page="settings"]');
                    if (settingsLink) {
                        settingsLink.click();
                    }
                    
                    // Close user menu
                    const userMenu = document.getElementById('userMenu');
                    if (userMenu) {
                        userMenu.style.display = 'none';
                    }
                });
            }
            
            // Settings menu item
            if (settingsMenuItem) {
                settingsMenuItem.addEventListener('click', function() {
                    // Navigate to settings page
                    const settingsLink = document.querySelector('[data-page="settings"]');
                    if (settingsLink) {
                        settingsLink.click();
                    }
                    
                    // Close user menu
                    const userMenu = document.getElementById('userMenu');
                    if (userMenu) {
                        userMenu.style.display = 'none';
                    }
                });
            }
            
            // Avatar upload
            if (avatarUpload && avatarInput) {
                avatarUpload.addEventListener('click', function() {
                    avatarInput.click();
                });
                
                avatarInput.addEventListener('change', function(e) {
                    const file = e.target.files[0];
                    if (file) {
                        const reader = new FileReader();
                        reader.onload = function(e) {
                            const userAvatar = document.getElementById('userAvatar');
                            if (userAvatar) {
                                userAvatar.innerHTML = `<img src="${e.target.result}" alt="Avatar">`;
                            }
                            
                            // Save avatar to localStorage
                            localStorage.setItem('userAvatar', e.target.result);
                        };
                        reader.readAsDataURL(file);
                    }
                });
            }
            
            // Load saved avatar
            const savedAvatar = localStorage.getItem('userAvatar');
            if (savedAvatar) {
                const userAvatar = document.getElementById('userAvatar');
                if (userAvatar) {
                    userAvatar.innerHTML = `<img src="${savedAvatar}" alt="Avatar">`;
                }
            }
        }
        
        // Task management functionality
        function initTaskManagement() {
            // Task subsections functionality
            const subsectionCards = document.querySelectorAll('.subsection-card');
            
            subsectionCards.forEach(card => {
                card.addEventListener('click', function() {
                    // Remove active class from all cards
                    subsectionCards.forEach(c => c.classList.remove('active'));
                    
                    // Add active class to clicked card
                    this.classList.add('active');
                    
                    // Hide all subsection contents
                    document.querySelectorAll('.subsection-content').forEach(content => {
                        content.style.display = 'none';
                    });
                    
                    // Show selected subsection
                    const subsection = this.getAttribute('data-subsection');
                    const subsectionContent = document.getElementById(subsection + 'Tasks');
                    if (subsectionContent) {
                        subsectionContent.style.display = 'block';
                    }
                    
                    // If category subsection is selected, populate with work tasks by default
                    if (subsection === 'category') {
                        populateCategoryTasks('work');
                    }
                });
            });
            
            // Task form functionality
            const newTaskBtn = document.getElementById('newTaskBtn');
            const taskForm = document.getElementById('taskForm');
            const saveTaskBtn = document.getElementById('saveTaskBtn');
            const cancelTaskBtn = document.getElementById('cancelTaskBtn');
            const taskList = document.getElementById('taskList');
            
            if (newTaskBtn && taskForm && saveTaskBtn && cancelTaskBtn) {
                newTaskBtn.addEventListener('click', function() {
                    taskForm.classList.add('active');
                });
                
                cancelTaskBtn.addEventListener('click', function() {
                    taskForm.classList.remove('active');
                    // Clear form
                    const titleInput = document.getElementById('taskTitle');
                    const descriptionInput = document.getElementById('taskDescription');
                    const startDateInput = document.getElementById('taskStartDate');
                    const endDateInput = document.getElementById('taskEndDate');
                    
                    if (titleInput) titleInput.value = '';
                    if (descriptionInput) descriptionInput.value = '';
                    if (startDateInput) startDateInput.value = '';
                    if (endDateInput) endDateInput.value = '';
                });
                
                saveTaskBtn.addEventListener('click', function() {
                    const titleInput = document.getElementById('taskTitle');
                    const descriptionInput = document.getElementById('taskDescription');
                    const categoryInput = document.getElementById('taskCategory');
                    const startDateInput = document.getElementById('taskStartDate');
                    const endDateInput = document.getElementById('taskEndDate');
                    const priorityInput = document.getElementById('taskPriority');
                    
                    const title = titleInput ? titleInput.value : '';
                    const description = descriptionInput ? descriptionInput.value : '';
                    const category = categoryInput ? categoryInput.value : '';
                    const startDate = startDateInput ? startDateInput.value : '';
                    const endDate = endDateInput ? endDateInput.value : '';
                    const priority = priorityInput ? priorityInput.value : '';
                    
                    if (title && startDate && endDate && taskList) {
                        const priorityClass = `priority-${priority}`;
                        const priorityText = priority === 'high' ? 'Alta' : priority === 'medium' ? 'Media' : 'Baja';
                        const categoryText = getCategoryName(category);
                        
                        // Generate unique ID for the task
                        const taskId = taskIdCounter++;
                        
                        const newTask = document.createElement('div');
                        newTask.className = 'task-item fade-in';
                        newTask.setAttribute('data-priority', priority);
                        newTask.setAttribute('data-task-id', taskId);
                        newTask.setAttribute('data-start-date', startDate);
                        newTask.setAttribute('data-end-date', endDate);
                        newTask.innerHTML = `
                            <div class="task-checkbox"></div>
                            <div class="task-content">
                                <div class="task-title">${title}</div>
                                <div class="task-description">${description}</div>
                                <div class="task-dates">Inicio: ${formatDate(startDate)} | Fin: ${formatDate(endDate)}</div>
                            </div>
                            <div class="task-priority ${priorityClass}">${priorityText}</div>
                            <div class="task-category">${categoryText}</div>
                            <div class="task-actions">
                                <button class="task-action-btn edit" data-task-id="${taskId}">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="task-action-btn delete" data-task-id="${taskId}">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        `;
                        
                        taskList.prepend(newTask);
                        
                        // Add checkbox functionality to new task
                        const newCheckbox = newTask.querySelector('.task-checkbox');
                        if (newCheckbox) {
                            newCheckbox.addEventListener('click', function() {
                                handleTaskCheckbox(this);
                            });
                        }
                        
                        // Add edit functionality to new task
                        const editBtn = newTask.querySelector('.task-action-btn.edit');
                        if (editBtn) {
                            editBtn.addEventListener('click', function() {
                                const taskId = this.getAttribute('data-task-id');
                                openEditTaskForm(taskId);
                            });
                        }
                        
                        // Add delete functionality to new task
                        const deleteBtn = newTask.querySelector('.task-action-btn.delete');
                        if (deleteBtn) {
                            deleteBtn.addEventListener('click', function() {
                                const taskId = this.getAttribute('data-task-id');
                                deleteTask(taskId);
                            });
                        }
                        
                        // Check if task is high priority and for today
                        const today = new Date().toISOString().split('T')[0];
                        if (priority === 'high' && startDate === today) {
                            // Add to dashboard
                            addToDashboard(newTask);
                            
                            // Show notification
                            showNotification('Nueva tarea urgente agregada');
                        }
                        
                        // Close form and reset
                        taskForm.classList.remove('active');
                        if (titleInput) titleInput.value = '';
                        if (descriptionInput) descriptionInput.value = '';
                        if (startDateInput) startDateInput.value = '';
                        if (endDateInput) endDateInput.value = '';
                        
                        // Update calendar if initialized
                        if (calendarInitialized) {
                            renderCalendar();
                        }
                    }
                });
            }
            
            // Edit task form functionality
            const editTaskForm = document.getElementById('editTaskForm');
            const updateTaskBtn = document.getElementById('updateTaskBtn');
            const cancelEditTaskBtn = document.getElementById('cancelEditTaskBtn');
            
            if (editTaskForm && updateTaskBtn && cancelEditTaskBtn) {
                cancelEditTaskBtn.addEventListener('click', function() {
                    editTaskForm.classList.remove('active');
                    currentEditingTaskId = null;
                });
                
                updateTaskBtn.addEventListener('click', function() {
                    const titleInput = document.getElementById('editTaskTitle');
                    const descriptionInput = document.getElementById('editTaskDescription');
                    const categoryInput = document.getElementById('editTaskCategory');
                    const startDateInput = document.getElementById('editTaskStartDate');
                    const endDateInput = document.getElementById('editTaskEndDate');
                    const priorityInput = document.getElementById('editTaskPriority');
                    
                    const title = titleInput ? titleInput.value : '';
                    const description = descriptionInput ? descriptionInput.value : '';
                    const category = categoryInput ? categoryInput.value : '';
                    const startDate = startDateInput ? startDateInput.value : '';
                    const endDate = endDateInput ? endDateInput.value : '';
                    const priority = priorityInput ? priorityInput.value : '';
                    
                    if (title && startDate && endDate && currentEditingTaskId) {
                        // Find the task element
                        const taskElement = document.querySelector(`[data-task-id="${currentEditingTaskId}"]`);
                        if (taskElement) {
                            // Update task properties
                            taskElement.setAttribute('data-priority', priority);
                            taskElement.setAttribute('data-start-date', startDate);
                            taskElement.setAttribute('data-end-date', endDate);
                            
                            // Update task content
                            const taskTitle = taskElement.querySelector('.task-title');
                            const taskDescription = taskElement.querySelector('.task-description');
                            const taskDates = taskElement.querySelector('.task-dates');
                            const taskPriority = taskElement.querySelector('.task-priority');
                            const taskCategory = taskElement.querySelector('.task-category');
                            
                            if (taskTitle) taskTitle.textContent = title;
                            if (taskDescription) taskDescription.textContent = description;
                            if (taskDates) taskDates.textContent = `Inicio: ${formatDate(startDate)} | Fin: ${formatDate(endDate)}`;
                            
                            // Update priority class and text
                            if (taskPriority) {
                                taskPriority.className = `task-priority priority-${priority}`;
                                taskPriority.textContent = priority === 'high' ? 'Alta' : priority === 'medium' ? 'Media' : 'Baja';
                            }
                            
                            // Update category text
                            if (taskCategory) {
                                taskCategory.textContent = getCategoryName(category);
                            }
                            
                            // Update high priority class
                            if (priority === 'high') {
                                taskElement.classList.add('high-priority');
                            } else {
                                taskElement.classList.remove('high-priority');
                            }
                            
                            // Update dashboard if needed
                            const today = new Date().toISOString().split('T')[0];
                            if (priority === 'high' && startDate === today) {
                                // Check if task is already in dashboard
                                const dashboardTask = document.querySelector(`#highPriorityTasks [data-task-id="${currentEditingTaskId}"]`);
                                if (!dashboardTask) {
                                    addToDashboard(taskElement);
                                }
                            } else {
                                // Remove from dashboard if it was there
                                const dashboardTask = document.querySelector(`#highPriorityTasks [data-task-id="${currentEditingTaskId}"]`);
                                if (dashboardTask) {
                                    dashboardTask.remove();
                                    updateUrgentTasksCount();
                                }
                            }
                        }
                        
                        // Close form and reset
                        editTaskForm.classList.remove('active');
                        currentEditingTaskId = null;
                        
                        // Update calendar if initialized
                        if (calendarInitialized) {
                            renderCalendar();
                        }
                        
                        // Show notification
                        showNotification('Tarea actualizada correctamente');
                    }
                });
            }
            
            // Initialize task checkboxes
            initTaskCheckboxes();
            
            // Initialize filter buttons
            initFilterButtons();
            
            // Initialize sort completed tasks
            initSortCompletedTasks();
            
            // Initialize task action buttons
            initTaskActionButtons();
        }
        
        // Open edit task form
        function openEditTaskForm(taskId) {
            const taskElement = document.querySelector(`[data-task-id="${taskId}"]`);
            if (!taskElement) return;
            
            const editTaskForm = document.getElementById('editTaskForm');
            const titleInput = document.getElementById('editTaskTitle');
            const descriptionInput = document.getElementById('editTaskDescription');
            const categoryInput = document.getElementById('editTaskCategory');
            const startDateInput = document.getElementById('editTaskStartDate');
            const endDateInput = document.getElementById('editTaskEndDate');
            const priorityInput = document.getElementById('editTaskPriority');
            
            if (!editTaskForm || !titleInput || !descriptionInput || !categoryInput || !startDateInput || !endDateInput || !priorityInput) return;
            
            // Get current task values
            const taskTitle = taskElement.querySelector('.task-title').textContent;
            const taskDescription = taskElement.querySelector('.task-description').textContent;
            const taskCategoryText = taskElement.querySelector('.task-category').textContent;
            const taskDatesText = taskElement.querySelector('.task-dates').textContent;
            const taskPriorityClass = taskElement.querySelector('.task-priority').className;
            
            // Extract dates from text
            const datesMatch = taskDatesText.match(/Inicio: (\d{2}\/\d{2}\/\d{4}) \| Fin: (\d{2}\/\d{2}\/\d{4})/);
            if (datesMatch) {
                const startDate = parseDate(datesMatch[1]);
                const endDate = parseDate(datesMatch[2]);
                
                if (startDateInput) startDateInput.value = startDate;
                if (endDateInput) endDateInput.value = endDate;
            }
            
            // Extract priority from class
            let priority = 'medium';
            if (taskPriorityClass.includes('priority-high')) priority = 'high';
            else if (taskPriorityClass.includes('priority-low')) priority = 'low';
            
            // Extract category from text
            let category = 'work';
            if (taskCategoryText === 'Personal') category = 'personal';
            else if (taskCategoryText === 'Salud') category = 'health';
            else if (taskCategoryText === 'Estudio') category = 'study';
            else if (taskCategoryText === 'Otros') category = 'other';
            
            // Set form values
            if (titleInput) titleInput.value = taskTitle;
            if (descriptionInput) descriptionInput.value = taskDescription;
            if (categoryInput) categoryInput.value = category;
            if (priorityInput) priorityInput.value = priority;
            
            // Set current editing task ID
            currentEditingTaskId = taskId;
            
            // Show form
            editTaskForm.classList.add('active');
        }
        
        // Delete task
        function deleteTask(taskId) {
            const taskElement = document.querySelector(`[data-task-id="${taskId}"]`);
            if (!taskElement) return;
            
            // Remove from dashboard if it's there
            const dashboardTask = document.querySelector(`#highPriorityTasks [data-task-id="${taskId}"]`);
            if (dashboardTask) {
                dashboardTask.remove();
                updateUrgentTasksCount();
            }
            
            // Remove from all task lists
            const allTaskElements = document.querySelectorAll(`[data-task-id="${taskId}"]`);
            allTaskElements.forEach(element => {
                element.style.opacity = '0';
                setTimeout(() => {
                    element.remove();
                }, 300);
            });
            
            // Update calendar if initialized
            if (calendarInitialized) {
                renderCalendar();
            }
            
            // Show notification
            showNotification('Tarea eliminada correctamente');
        }
        
        // Initialize task checkboxes
        function initTaskCheckboxes() {
            const taskCheckboxes = document.querySelectorAll('.task-checkbox');
            
            taskCheckboxes.forEach(checkbox => {
                checkbox.addEventListener('click', function() {
                    handleTaskCheckbox(this);
                });
            });
        }
        
        // Handle task checkbox click
        function handleTaskCheckbox(checkbox) {
            checkbox.classList.toggle('checked');
            
            // Move to completed tasks if checked
            if (checkbox.classList.contains('checked')) {
                const taskItem = checkbox.closest('.task-item');
                const completedTaskList = document.getElementById('completedTaskList');
                
                if (taskItem && completedTaskList) {
                    // Clone task for completed list
                    const completedTask = taskItem.cloneNode(true);
                    const taskDates = completedTask.querySelector('.task-dates');
                    if (taskDates) {
                        taskDates.textContent = `Completada: ${new Date().toLocaleDateString()}`;
                    }
                    completedTaskList.prepend(completedTask);
                    
                    // Add checkbox functionality to completed task
                    const completedCheckbox = completedTask.querySelector('.task-checkbox');
                    if (completedCheckbox) {
                        completedCheckbox.addEventListener('click', function() {
                            this.classList.toggle('checked');
                        });
                    }
                    
                    // Check if task is in dashboard and remove it
                    const taskId = taskItem.getAttribute('data-task-id');
                    if (taskId) {
                        const dashboardTask = document.querySelector(`#highPriorityTasks [data-task-id="${taskId}"]`);
                        if (dashboardTask) {
                            dashboardTask.style.opacity = '0';
                            setTimeout(() => {
                                dashboardTask.remove();
                                updateUrgentTasksCount();
                            }, 300);
                        }
                    }
                    
                    // Hide original task
                    setTimeout(() => {
                        taskItem.style.opacity = '0';
                        setTimeout(() => {
                            taskItem.style.display = 'none';
                        }, 300);
                    }, 500);
                    
                    // Update charts if initialized
                    if (chartsInitialized) {
                        updateCharts();
                    }
                }
            }
        }
        
        // Add task to dashboard
        function addToDashboard(taskElement) {
            const highPriorityTasks = document.getElementById('highPriorityTasks');
            if (highPriorityTasks) {
                // Clone the task for dashboard
                const dashboardTask = taskElement.cloneNode(true);
                
                // Add high priority class if not already
                if (!dashboardTask.classList.contains('high-priority')) {
                    dashboardTask.classList.add('high-priority');
                }
                
                // Add alert icon if not already
                const taskTitle = dashboardTask.querySelector('.task-title');
                if (taskTitle && !taskTitle.querySelector('.alert-icon')) {
                    const alertIcon = document.createElement('i');
                    alertIcon.className = 'fas fa-exclamation-circle alert-icon';
                    taskTitle.appendChild(alertIcon);
                }
                
                // Update dates to show "Hoy"
                const taskDates = dashboardTask.querySelector('.task-dates');
                if (taskDates) {
                    taskDates.textContent = 'Hoy';
                }
                
                // Add checkbox functionality
                const checkbox = dashboardTask.querySelector('.task-checkbox');
                if (checkbox) {
                    checkbox.addEventListener('click', function() {
                        handleTaskCheckbox(this);
                    });
                }
                
                // Add edit functionality
                const editBtn = dashboardTask.querySelector('.task-action-btn.edit');
                if (editBtn) {
                    editBtn.addEventListener('click', function() {
                        const taskId = this.getAttribute('data-task-id');
                        openEditTaskForm(taskId);
                    });
                }
                
                // Add delete functionality
                const deleteBtn = dashboardTask.querySelector('.task-action-btn.delete');
                if (deleteBtn) {
                    deleteBtn.addEventListener('click', function() {
                        const taskId = this.getAttribute('data-task-id');
                        deleteTask(taskId);
                    });
                }
                
                // Add to dashboard
                highPriorityTasks.prepend(dashboardTask);
                
                // Update count
                updateUrgentTasksCount();
            }
        }
        
        // Update urgent tasks count
        function updateUrgentTasksCount() {
            const urgentTasksCount = document.getElementById('urgentTasksCount');
            const highPriorityTasks = document.querySelectorAll('#highPriorityTasks .task-item').length;
            
            if (urgentTasksCount) {
                urgentTasksCount.textContent = highPriorityTasks;
            }
        }
        
        // Initialize filter buttons
        function initFilterButtons() {
            const filterBtns = document.querySelectorAll('.filter-btn');
            
            filterBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    const parent = this.closest('.priority-filter, .category-filter');
                    if (parent) {
                        // Remove active class from all filter buttons in the same container
                        parent.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
                        
                        // Add active class to clicked button
                        this.classList.add('active');
                        
                        // Get filter value
                        const filter = this.getAttribute('data-filter') || this.getAttribute('data-category');
                        
                        // Handle category filter
                        if (this.closest('#categoryTasks')) {
                            populateCategoryTasks(filter);
                            return;
                        }
                        
                        // Get the task list to filter
                        let taskList;
                        if (this.closest('#generalTasks')) {
                            taskList = document.getElementById('taskList');
                        } else if (this.closest('#completedTasks')) {
                            taskList = document.getElementById('completedTaskList');
                        } else if (this.closest('#pendingTasks')) {
                            taskList = document.getElementById('pendingTaskList');
                        } else if (this.closest('#highPriorityTasks')) {
                            taskList = document.getElementById('highPriorityTasks');
                        }
                        
                        if (taskList) {
                            // Filter tasks
                            const tasks = taskList.querySelectorAll('.task-item');
                            tasks.forEach(task => {
                                if (filter === 'all') {
                                    task.style.display = 'flex';
                                } else {
                                    const priority = task.getAttribute('data-priority');
                                    if (priority === filter) {
                                        task.style.display = 'flex';
                                    } else {
                                        task.style.display = 'none';
                                    }
                                }
                            });
                        }
                    }
                });
            });
        }
        
        // Initialize sort completed tasks
        function initSortCompletedTasks() {
            const sortCompletedTasks = document.getElementById('sortCompletedTasks');
            
            if (sortCompletedTasks) {
                sortCompletedTasks.addEventListener('change', function() {
                    const sortBy = this.value;
                    const completedTaskList = document.getElementById('completedTaskList');
                    
                    if (completedTaskList) {
                        const tasks = Array.from(completedTaskList.querySelectorAll('.task-item'));
                        
                        tasks.sort((a, b) => {
                            if (sortBy === 'date') {
                                const dateA = new Date(a.querySelector('.task-dates').textContent.replace('Completada: ', ''));
                                const dateB = new Date(b.querySelector('.task-dates').textContent.replace('Completada: ', ''));
                                return dateB - dateA; // Newest first
                            } else if (sortBy === 'priority') {
                                const priorityOrder = { 'high': 1, 'medium': 2, 'low': 3 };
                                const priorityA = priorityOrder[a.getAttribute('data-priority')];
                                const priorityB = priorityOrder[b.getAttribute('data-priority')];
                                return priorityA - priorityB;
                            } else if (sortBy === 'category') {
                                const categoryA = a.querySelector('.task-category').textContent;
                                const categoryB = b.querySelector('.task-category').textContent;
                                return categoryA.localeCompare(categoryB);
                            }
                        });
                        
                        // Reorder tasks in the DOM
                        tasks.forEach(task => {
                            completedTaskList.appendChild(task);
                        });
                    }
                });
            }
        }
        
        // Initialize task action buttons
        function initTaskActionButtons() {
            const editBtns = document.querySelectorAll('.task-action-btn.edit');
            const deleteBtns = document.querySelectorAll('.task-action-btn.delete');
            
            editBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    const taskId = this.getAttribute('data-task-id');
                    openEditTaskForm(taskId);
                });
            });
            
            deleteBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    const taskId = this.getAttribute('data-task-id');
                    deleteTask(taskId);
                });
            });
        }
        
        // Function to populate category tasks
        function populateCategoryTasks(category) {
            const categoryTaskList = document.getElementById('categoryTaskList');
            
            if (categoryTaskList) {
                categoryTaskList.innerHTML = '';
                
                // Get all tasks from general tasks
                const allTasks = document.querySelectorAll('#taskList .task-item');
                
                allTasks.forEach(task => {
                    const taskCategoryElement = task.querySelector('.task-category');
                    if (taskCategoryElement) {
                        const taskCategory = taskCategoryElement.textContent;
                        const categoryName = getCategoryName(category);
                        
                        if (taskCategory === categoryName) {
                            const clonedTask = task.cloneNode(true);
                            
                            // Add checkbox functionality
                            const checkbox = clonedTask.querySelector('.task-checkbox');
                            if (checkbox) {
                                checkbox.addEventListener('click', function() {
                                    handleTaskCheckbox(this);
                                });
                            }
                            
                            // Add edit functionality
                            const editBtn = clonedTask.querySelector('.task-action-btn.edit');
                            if (editBtn) {
                                editBtn.addEventListener('click', function() {
                                    const taskId = this.getAttribute('data-task-id');
                                    openEditTaskForm(taskId);
                                });
                            }
                            
                            // Add delete functionality
                            const deleteBtn = clonedTask.querySelector('.task-action-btn.delete');
                            if (deleteBtn) {
                                deleteBtn.addEventListener('click', function() {
                                    const taskId = this.getAttribute('data-task-id');
                                    deleteTask(taskId);
                                });
                            }
                            
                            categoryTaskList.appendChild(clonedTask);
                        }
                    }
                });
            }
        }
        
        // Calendar functionality
        function initCalendar() {
            const yearSelect = document.getElementById('yearSelect');
            const monthSelect = document.getElementById('monthSelect');
            const prevYearBtn = document.getElementById('prevYear');
            const nextYearBtn = document.getElementById('nextYear');
            const prevMonthBtn = document.getElementById('prevMonth');
            const nextMonthBtn = document.getElementById('nextMonth');
            const viewBtns = document.querySelectorAll('.view-btn');
            
            if (!yearSelect || !monthSelect || !prevYearBtn || !nextYearBtn || !prevMonthBtn || !nextMonthBtn) {
                return;
            }
            
            // Populate year select (1995-2040)
            for (let year = 1995; year <= 2040; year++) {
                const option = document.createElement('option');
                option.value = year;
                option.textContent = year;
                yearSelect.appendChild(option);
            }
            
            // Set current year and month
            const currentDate = new Date();
            yearSelect.value = currentDate.getFullYear();
            monthSelect.value = currentDate.getMonth();
            
            // Event listeners
            yearSelect.addEventListener('change', renderCalendar);
            monthSelect.addEventListener('change', renderCalendar);
            
            prevYearBtn.addEventListener('click', function() {
                yearSelect.value = parseInt(yearSelect.value) - 1;
                renderCalendar();
            });
            
            nextYearBtn.addEventListener('click', function() {
                yearSelect.value = parseInt(yearSelect.value) + 1;
                renderCalendar();
            });
            
            prevMonthBtn.addEventListener('click', function() {
                if (parseInt(monthSelect.value) === 0) {
                    monthSelect.value = 11;
                    yearSelect.value = parseInt(yearSelect.value) - 1;
                } else {
                    monthSelect.value = parseInt(monthSelect.value) - 1;
                }
                renderCalendar();
            });
            
            nextMonthBtn.addEventListener('click', function() {
                if (parseInt(monthSelect.value) === 11) {
                    monthSelect.value = 0;
                    yearSelect.value = parseInt(yearSelect.value) + 1;
                } else {
                    monthSelect.value = parseInt(monthSelect.value) + 1;
                }
                renderCalendar();
            });
            
            // View toggle
            viewBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    viewBtns.forEach(b => b.classList.remove('active'));
                    this.classList.add('active');
                    renderCalendar();
                });
            });
        }
        
        // Function to render calendar
        function renderCalendar() {
            const yearSelect = document.getElementById('yearSelect');
            const monthSelect = document.getElementById('monthSelect');
            const calendarView = document.getElementById('calendarView');
            const activeView = document.querySelector('.view-btn.active');
            
            if (!yearSelect || !monthSelect || !calendarView || !activeView) {
                return;
            }
            
            const year = parseInt(yearSelect.value);
            const month = parseInt(monthSelect.value);
            const view = activeView.getAttribute('data-view');
            
            // Clear calendar view
            calendarView.innerHTML = '';
            
            if (view === 'month') {
                renderMonthView(year, month, calendarView);
            } else if (view === 'week') {
                renderWeekView(year, month, calendarView);
            } else if (view === 'day') {
                renderDayView(year, month, calendarView);
            }
        }
        
        // Function to render month view
        function renderMonthView(year, month, calendarView) {
            const calendarGrid = document.createElement('div');
            calendarGrid.className = 'calendar-grid';
            calendarView.appendChild(calendarGrid);
            
            // Add day headers
            const dayHeaders = ['Dom', 'Lun', 'Mar', 'Mié', 'Jue', 'Vie', 'Sáb'];
            dayHeaders.forEach(day => {
                const dayHeader = document.createElement('div');
                dayHeader.className = 'calendar-day-header';
                dayHeader.textContent = day;
                calendarGrid.appendChild(dayHeader);
            });
            
            // Get first day of month and number of days
            const firstDay = new Date(year, month, 1).getDay();
            const daysInMonth = new Date(year, month + 1, 0).getDate();
            
            // Get days in previous month
            const daysInPrevMonth = new Date(year, month, 0).getDate();
            
            // Add empty cells for days before the first day of the month
            for (let i = firstDay - 1; i >= 0; i--) {
                const day = document.createElement('div');
                day.className = 'calendar-day other-month';
                day.textContent = daysInPrevMonth - i;
                calendarGrid.appendChild(day);
            }
            
            // Get all tasks to display on calendar
            const allTasks = document.querySelectorAll('#taskList .task-item');
            
            // Add days of the month
            const currentDate = new Date();
            for (let day = 1; day <= daysInMonth; day++) {
                const dayElement = document.createElement('div');
                dayElement.className = 'calendar-day';
                dayElement.textContent = day;
                
                // Format date for comparison
                const formattedDate = `${year}-${String(month + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
                
                // Check if today
                if (year === currentDate.getFullYear() && 
                    month === currentDate.getMonth() && 
                    day === currentDate.getDate()) {
                    dayElement.classList.add('today');
                }
                
                // Check if there are tasks for this day
                let hasTask = false;
                const taskPriorities = [];
                
                allTasks.forEach(task => {
                    const startDate = task.getAttribute('data-start-date');
                    const endDate = task.getAttribute('data-end-date');
                    const priority = task.getAttribute('data-priority');
                    
                    if (startDate && endDate) {
                        // Check if the task spans this day
                        const taskStart = new Date(startDate);
                        const taskEnd = new Date(endDate);
                        const currentDay = new Date(year, month, day);
                        
                        if (currentDay >= taskStart && currentDay <= taskEnd) {
                            hasTask = true;
                            if (!taskPriorities.includes(priority)) {
                                taskPriorities.push(priority);
                            }
                        }
                    }
                });
                
                if (hasTask) {
                    dayElement.classList.add('has-task');
                    
                    // Add task indicators
                    taskPriorities.forEach(priority => {
                        const indicator = document.createElement('div');
                        indicator.className = `task-indicator ${priority}`;
                        dayElement.appendChild(indicator);
                    });
                }
                
                // Add click event to show day details
                dayElement.addEventListener('click', function() {
                    showDayTasks(year, month, day);
                });
                
                calendarGrid.appendChild(dayElement);
            }
            
            // Add empty cells for days after the last day of the month
            const totalCells = calendarGrid.children.length - 7; // Subtract day headers
            const remainingCells = 42 - totalCells; // 6 rows * 7 days = 42 cells
            
            for (let i = 1; i <= remainingCells; i++) {
                const day = document.createElement('div');
                day.className = 'calendar-day other-month';
                day.textContent = i;
                calendarGrid.appendChild(day);
            }
        }
        
        // Function to render week view
        function renderWeekView(year, month, calendarView) {
            const calendarWeekView = document.createElement('div');
            calendarWeekView.className = 'calendar-week-view';
            calendarView.appendChild(calendarWeekView);
            
            // Add time column header
            const timeHeader = document.createElement('div');
            timeHeader.className = 'week-time';
            timeHeader.textContent = 'Hora';
            calendarWeekView.appendChild(timeHeader);
            
            // Add day headers
            const dayNames = ['Domingo', 'Lunes', 'Martes', 'Miércoles', 'Jueves', 'Viernes', 'Sábado'];
            dayNames.forEach(dayName => {
                const dayHeader = document.createElement('div');
                dayHeader.className = 'week-day-header';
                dayHeader.textContent = dayName;
                calendarWeekView.appendChild(dayHeader);
            });
            
            // Get all tasks to display on calendar
            const allTasks = document.querySelectorAll('#taskList .task-item');
            
            // Add time slots and day cells
            const today = new Date();
            for (let hour = 8; hour <= 20; hour++) {
                // Time label
                const timeLabel = document.createElement('div');
                timeLabel.className = 'week-time';
                timeLabel.textContent = `${hour}:00`;
                calendarWeekView.appendChild(timeLabel);
                
                // Day cells
                for (let day = 0; day < 7; day++) {
                    const dayCell = document.createElement('div');
                    dayCell.className = 'week-day';
                    
                    // Check if today
                    if (year === today.getFullYear() && 
                        month === today.getMonth() && 
                        day === today.getDay() && 
                        hour === today.getHours()) {
                        dayCell.classList.add('today');
                    }
                    
                    // Check if there are tasks for this day and hour
                    allTasks.forEach(task => {
                        const startDate = task.getAttribute('data-start-date');
                        const endDate = task.getAttribute('data-end-date');
                        const taskTitle = task.querySelector('.task-title').textContent;
                        
                        if (startDate && endDate) {
                            // Check if the task spans this day
                            const taskStart = new Date(startDate);
                            const taskEnd = new Date(endDate);
                            
                            // For simplicity, we'll just add tasks to random hours
                            if (Math.random() > 0.8 && hour >= 9 && hour <= 18) {
                                const task = document.createElement('div');
                                task.className = 'week-task';
                                task.textContent = taskTitle;
                                task.draggable = true;
                                dayCell.appendChild(task);
                            }
                        }
                    });
                    
                    calendarWeekView.appendChild(dayCell);
                }
            }
            
            // Make tasks draggable
            const draggableTasks = calendarWeekView.querySelectorAll('.week-task');
            draggableTasks.forEach(task => {
                task.addEventListener('dragstart', function(e) {
                    e.dataTransfer.setData('text/plain', 'task');
                });
            });
            
            // Make day cells drop targets
            const dropTargets = calendarWeekView.querySelectorAll('.week-day');
            dropTargets.forEach(target => {
                target.addEventListener('dragover', function(e) {
                    e.preventDefault();
                });
                
                target.addEventListener('drop', function(e) {
                    e.preventDefault();
                    const data = e.dataTransfer.getData('text/plain');
                    if (data === 'task') {
                        const newTask = document.createElement('div');
                        newTask.className = 'week-task';
                        newTask.textContent = 'Reunión';
                        newTask.draggable = true;
                        this.appendChild(newTask);
                        
                        // Add drag functionality to new task
                        newTask.addEventListener('dragstart', function(e) {
                            e.dataTransfer.setData('text/plain', 'task');
                        });
                    }
                });
            });
        }
        
        // Function to render day view
        function renderDayView(year, month, calendarView) {
            const calendarDayView = document.createElement('div');
            calendarDayView.className = 'calendar-day-view';
            calendarView.appendChild(calendarDayView);
            
            // Get all tasks to display on calendar
            const allTasks = document.querySelectorAll('#taskList .task-item');
            
            // Add time slots
            for (let hour = 8; hour <= 20; hour++) {
                const dayHour = document.createElement('div');
                dayHour.className = 'day-hour';
                
                const hourLabel = document.createElement('div');
                hourLabel.className = 'hour-label';
                hourLabel.textContent = `${hour}:00`;
                dayHour.appendChild(hourLabel);
                
                const hourContent = document.createElement('div');
                hourContent.className = 'hour-content';
                
                // Check if there are tasks for this hour
                allTasks.forEach(task => {
                    const startDate = task.getAttribute('data-start-date');
                    const endDate = task.getAttribute('data-end-date');
                    const taskTitle = task.querySelector('.task-title').textContent;
                    
                    if (startDate && endDate) {
                        // For simplicity, we'll just add tasks to random hours
                        if (Math.random() > 0.7 && hour >= 9 && hour <= 18) {
                            const task = document.createElement('div');
                            task.className = 'day-task';
                            task.textContent = taskTitle;
                            task.draggable = true;
                            hourContent.appendChild(task);
                        }
                    }
                });
                
                dayHour.appendChild(hourContent);
                calendarDayView.appendChild(dayHour);
            }
            
            // Make tasks draggable
            const draggableTasks = calendarDayView.querySelectorAll('.day-task');
            draggableTasks.forEach(task => {
                task.addEventListener('dragstart', function(e) {
                    e.dataTransfer.setData('text/plain', 'task');
                });
            });
            
            // Make hour cells drop targets
            const dropTargets = calendarDayView.querySelectorAll('.hour-content');
            dropTargets.forEach(target => {
                target.addEventListener('dragover', function(e) {
                    e.preventDefault();
                });
                
                target.addEventListener('drop', function(e) {
                    e.preventDefault();
                    const data = e.dataTransfer.getData('text/plain');
                    if (data === 'task') {
                        const newTask = document.createElement('div');
                        newTask.className = 'day-task';
                        newTask.textContent = 'Reunión';
                        newTask.draggable = true;
                        this.appendChild(newTask);
                        
                        // Add drag functionality to new task
                        newTask.addEventListener('dragstart', function(e) {
                            e.dataTransfer.setData('text/plain', 'task');
                        });
                    }
                });
            });
        }
        
        // Function to show tasks for a specific day
        function showDayTasks(year, month, day) {
            const modal = document.getElementById('calendarDayModal');
            const modalDayTitle = document.getElementById('modalDayTitle');
            const modalDayTasks = document.getElementById('modalDayTasks');
            
            if (modal && modalDayTitle && modalDayTasks) {
                // Format date for display
                const date = new Date(year, month, day);
                const formattedDate = date.toLocaleDateString('es-ES', { 
                    weekday: 'long', 
                    year: 'numeric', 
                    month: 'long', 
                    day: 'numeric' 
                });
                
                modalDayTitle.textContent = `Tareas del ${formattedDate}`;
                
                // Clear previous tasks
                modalDayTasks.innerHTML = '';
                
                // Format date for comparison
                const formattedDateCompare = `${year}-${String(month + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
                
                // Get all tasks
                const allTasks = document.querySelectorAll('#taskList .task-item');
                
                // Check each task to see if it falls on this day
                allTasks.forEach(task => {
                    const startDate = task.getAttribute('data-start-date');
                    const endDate = task.getAttribute('data-end-date');
                    
                    if (startDate && endDate) {
                        // Check if the task spans this day
                        const taskStart = new Date(startDate);
                        const taskEnd = new Date(endDate);
                        const currentDay = new Date(year, month, day);
                        
                        if (currentDay >= taskStart && currentDay <= taskEnd) {
                            // Clone task for modal
                            const modalTask = task.cloneNode(true);
                            
                            // Add checkbox functionality
                            const checkbox = modalTask.querySelector('.task-checkbox');
                            if (checkbox) {
                                checkbox.addEventListener('click', function() {
                                    handleTaskCheckbox(this);
                                });
                            }
                            
                            // Add edit functionality
                            const editBtn = modalTask.querySelector('.task-action-btn.edit');
                            if (editBtn) {
                                editBtn.addEventListener('click', function() {
                                    const taskId = this.getAttribute('data-task-id');
                                    openEditTaskForm(taskId);
                                    modal.classList.remove('active');
                                });
                            }
                            
                            // Add delete functionality
                            const deleteBtn = modalTask.querySelector('.task-action-btn.delete');
                            if (deleteBtn) {
                                deleteBtn.addEventListener('click', function() {
                                    const taskId = this.getAttribute('data-task-id');
                                    deleteTask(taskId);
                                    modal.classList.remove('active');
                                });
                            }
                            
                            modalDayTasks.appendChild(modalTask);
                        }
                    }
                });
                
                // Show modal
                modal.classList.add('active');
            }
        }
        
        // Initialize charts
        function initCharts() {
            // Tasks Chart
            const tasksCtx = document.getElementById('tasksChart');
            if (tasksCtx) {
                new Chart(tasksCtx, {
                    type: 'bar',
                    data: {
                        labels: ['Completadas', 'Pendientes'],
                        datasets: [{
                            label: 'Tareas',
                            data: [24, 12],
                            backgroundColor: [
                                'rgba(76, 175, 80, 0.7)',
                                'rgba(255, 82, 82, 0.7)'
                            ],
                            borderColor: [
                                'rgba(76, 175, 80, 1)',
                                'rgba(255, 82, 82, 1)'
                            ],
                            borderWidth: 1
                        }]
                    },
                    options: {
                        responsive: true,
                        scales: {
                            y: {
                                beginAtZero: true
                            }
                        }
                    }
                });
            }
            
            // Priority Chart
            const priorityCtx = document.getElementById('priorityChart');
            if (priorityCtx) {
                new Chart(priorityCtx, {
                    type: 'doughnut',
                    data: {
                        labels: ['Alta', 'Media', 'Baja'],
                        datasets: [{
                            data: [5, 8, 3],
                            backgroundColor: [
                                'rgba(255, 82, 82, 0.7)',
                                'rgba(255, 193, 7, 0.7)',
                                'rgba(76, 175, 80, 0.7)'
                            ],
                            borderColor: [
                                'rgba(255, 82, 82, 1)',
                                'rgba(255, 193, 7, 1)',
                                'rgba(76, 175, 80, 1)'
                            ],
                            borderWidth: 1
                        }]
                    },
                    options: {
                        responsive: true
                    }
                });
            }
            
            // Daily Tasks Chart
            const dailyTasksCtx = document.getElementById('dailyTasksChart');
            if (dailyTasksCtx) {
                new Chart(dailyTasksCtx, {
                    type: 'line',
                    data: {
                        labels: ['Lun', 'Mar', 'Mié', 'Jue', 'Vie', 'Sáb', 'Dom'],
                        datasets: [{
                            label: 'Tareas Completadas',
                            data: [5, 7, 3, 8, 6, 4, 2],
                            fill: true,
                            backgroundColor: 'rgba(255, 165, 89, 0.2)',
                            borderColor: 'rgba(255, 165, 89, 1)',
                            tension: 0.4
                        }]
                    },
                    options: {
                        responsive: true,
                        scales: {
                            y: {
                                beginAtZero: true
                            }
                        }
                    }
                });
            }
            
            // Monthly Tasks Chart
            const monthlyTasksCtx = document.getElementById('monthlyTasksChart');
            if (monthlyTasksCtx) {
                new Chart(monthlyTasksCtx, {
                    type: 'bar',
                    data: {
                        labels: ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun'],
                        datasets: [{
                            label: 'Tareas Completadas',
                            data: [45, 52, 38, 65, 59, 70],
                            backgroundColor: 'rgba(74, 111, 165, 0.7)',
                            borderColor: 'rgba(74, 111, 165, 1)',
                            borderWidth: 1
                        }]
                    },
                    options: {
                        responsive: true,
                        scales: {
                            y: {
                                beginAtZero: true
                            }
                        }
                    }
                });
            }
            
            // Category Performance Chart
            const categoryPerformanceCtx = document.getElementById('categoryPerformanceChart');
            if (categoryPerformanceCtx) {
                new Chart(categoryPerformanceCtx, {
                    type: 'radar',
                    data: {
                        labels: ['Trabajo', 'Personal', 'Salud', 'Estudio', 'Otros'],
                        datasets: [{
                            label: 'Tareas Completadas',
                            data: [18, 12, 8, 10, 5],
                            backgroundColor: 'rgba(255, 165, 89, 0.2)',
                            borderColor: 'rgba(255, 165, 89, 1)',
                            pointBackgroundColor: 'rgba(255, 165, 89, 1)',
                            pointBorderColor: '#fff',
                            pointHoverBackgroundColor: '#fff',
                            pointHoverBorderColor: 'rgba(255, 165, 89, 1)'
                        }]
                    },
                    options: {
                        responsive: true,
                        scales: {
                            r: {
                                beginAtZero: true,
                                max: 20
                            }
                        }
                    }
                });
            }
            
            // Productivity Chart
            const productivityCtx = document.getElementById('productivityChart');
            if (productivityCtx) {
                new Chart(productivityCtx, {
                    type: 'line',
                    data: {
                        labels: ['Semana 1', 'Semana 2', 'Semana 3', 'Semana 4'],
                        datasets: [{
                            label: 'Productividad',
                            data: [65, 70, 80, 85],
                            fill: true,
                            backgroundColor: 'rgba(255, 165, 89, 0.2)',
                            borderColor: 'rgba(255, 165, 89, 1)',
                            tension: 0.4
                        }]
                    },
                    options: {
                        responsive: true,
                        scales: {
                            y: {
                                beginAtZero: true,
                                max: 100
                            }
                        }
                    }
                });
            }
        }
        
        // Update charts with new data
        function updateCharts() {
            // This function would update the charts with new data when tasks are completed
            // For now, we'll just reinitialize the charts
            if (chartsInitialized) {
                // Destroy existing charts
                Chart.helpers.each(Chart.instances, function(instance) {
                    instance.destroy();
                });
                
                // Reinitialize charts
                initCharts();
            }
        }
        
        // Initialize finance charts
        function initFinanceCharts() {
            // Income vs Expense Chart
            const incomeExpenseCtx = document.getElementById('incomeExpenseChart');
            if (incomeExpenseCtx) {
                new Chart(incomeExpenseCtx, {
                    type: 'bar',
                    data: {
                        labels: ['Ene', 'Feb', 'Mar', 'Abr', 'May'],
                        datasets: [
                            {
                                label: 'Ingresos',
                                data: [2500, 2800, 3000, 3200, 3200],
                                backgroundColor: 'rgba(76, 175, 80, 0.7)',
                                borderColor: 'rgba(76, 175, 80, 1)',
                                borderWidth: 1
                            },
                            {
                                label: 'Gastos',
                                data: [1200, 900, 1100, 800, 750],
                                backgroundColor: 'rgba(255, 82, 82, 0.7)',
                                borderColor: 'rgba(255, 82, 82, 1)',
                                borderWidth: 1
                            }
                        ]
                    },
                    options: {
                        responsive: true,
                        scales: {
                            y: {
                                beginAtZero: true
                            }
                        }
                    }
                });
            }
            
            // Cash Flow Chart
            const cashFlowCtx = document.getElementById('cashFlowChart');
            if (cashFlowCtx) {
                new Chart(cashFlowCtx, {
                    type: 'line',
                    data: {
                        labels: ['Ene', 'Feb', 'Mar', 'Abr', 'May'],
                        datasets: [{
                            label: 'Balance',
                            data: [1500, 2200, 1800, 2500, 2450],
                            fill: true,
                            backgroundColor: 'rgba(255, 165, 89, 0.2)',
                            borderColor: 'rgba(255, 165, 89, 1)',
                            tension: 0.4
                        }]
                    },
                    options: {
                        responsive: true,
                        scales: {
                            y: {
                                beginAtZero: true
                            }
                        }
                    }
                });
            }
            
            // Expense Distribution Chart
            const expenseDistributionCtx = document.getElementById('expenseDistributionChart');
            if (expenseDistributionCtx) {
                new Chart(expenseDistributionCtx, {
                    type: 'pie',
                    data: {
                        labels: ['Transporte', 'Alimentación', 'Entretenimiento', 'Salud', 'Educación', 'Inversión', 'Otros'],
                        datasets: [{
                            data: [150, 300, 100, 50, 80, 40, 30],
                            backgroundColor: [
                                'rgba(255, 99, 132, 0.7)',
                                'rgba(54, 162, 235, 0.7)',
                                'rgba(255, 206, 86, 0.7)',
                                'rgba(75, 192, 192, 0.7)',
                                'rgba(153, 102, 255, 0.7)',
                                'rgba(255, 159, 64, 0.7)',
                                'rgba(199, 199, 199, 0.7)'
                            ],
                            borderColor: [
                                'rgba(255, 99, 132, 1)',
                                'rgba(54, 162, 235, 1)',
                                'rgba(255, 206, 86, 1)',
                                'rgba(75, 192, 192, 1)',
                                'rgba(153, 102, 255, 1)',
                                'rgba(255, 159, 64, 1)',
                                'rgba(199, 199, 199, 1)'
                            ],
                            borderWidth: 1
                        }]
                    },
                    options: {
                        responsive: true
                    }
                });
            }
        }
        
        // Initialize notes
        function initNotes() {
            const newNoteBtn = document.getElementById('newNoteBtn');
            const notesGrid = document.getElementById('notesGrid');
            
            if (newNoteBtn && notesGrid) {
                newNoteBtn.addEventListener('click', function() {
                    const noteModal = document.getElementById('noteModal');
                    if (noteModal) {
                        noteModal.classList.add('active');
                    }
                });
            }
            
            // Pin functionality for existing notes
            const notePins = document.querySelectorAll('.note-pin');
            notePins.forEach(pin => {
                pin.addEventListener('click', function() {
                    this.classList.toggle('pinned');
                    const note = this.closest('.note');
                    if (note) {
                        note.classList.toggle('pinned');
                        
                        // If pinned, add to dashboard
                        if (this.classList.contains('pinned')) {
                            const dashboardNotes = document.querySelector('#dashboard .notes-grid');
                            if (dashboardNotes) {
                                const clonedNote = note.cloneNode(true);
                                dashboardNotes.appendChild(clonedNote);
                                
                                // Add pin functionality to cloned note
                                const clonedPin = clonedNote.querySelector('.note-pin');
                                if (clonedPin) {
                                    clonedPin.addEventListener('click', function() {
                                        this.classList.toggle('pinned');
                                        clonedNote.classList.toggle('pinned');
                                        
                                        // If unpinned, remove from dashboard
                                        if (!this.classList.contains('pinned')) {
                                            clonedNote.remove();
                                        }
                                    });
                                }
                            }
                        }
                    }
                });
            });
            
            // Note click functionality for editing
            const notes = document.querySelectorAll('.note');
            notes.forEach(note => {
                note.addEventListener('click', function(e) {
                    // Don't open edit modal if clicking on pin
                    if (e.target.classList.contains('note-pin')) return;
                    
                    const noteId = this.getAttribute('data-note-id');
                    if (noteId) {
                        openEditNoteForm(noteId);
                    }
                });
            });
        }
        
        // Open edit note form
        function openEditNoteForm(noteId) {
            const noteElement = document.querySelector(`[data-note-id="${noteId}"]`);
            if (!noteElement) return;
            
            const editNoteModal = document.getElementById('editNoteModal');
            const titleInput = document.getElementById('editNoteTitle');
            const contentInput = document.getElementById('editNoteContent');
            const categoryInput = document.getElementById('editNoteCategory');
            const pinnedInput = document.getElementById('editNotePinned');
            
            if (!editNoteModal || !titleInput || !contentInput || !categoryInput || !pinnedInput) return;
            
            // Get current note values
            const noteTitle = noteElement.querySelector('.note-title').textContent;
            const noteContent = noteElement.querySelector('.note-content').textContent;
            const noteCategoryText = noteElement.querySelector('.note-category').textContent;
            const isPinned = noteElement.classList.contains('pinned');
            
            // Extract category from text
            let category = 'work';
            if (noteCategoryText === 'Personal') category = 'personal';
            else if (noteCategoryText === 'Salud') category = 'health';
            else if (noteCategoryText === 'Estudio') category = 'study';
            else if (noteCategoryText === 'Otros') category = 'other';
            
            // Set form values
            if (titleInput) titleInput.value = noteTitle;
            if (contentInput) contentInput.value = noteContent;
            if (categoryInput) categoryInput.value = category;
            if (pinnedInput) pinnedInput.checked = isPinned;
            
            // Set current editing note ID
            currentEditingNoteId = noteId;
            
            // Show modal
            editNoteModal.classList.add('active');
        }
        
        // Initialize notifications
        function initNotifications() {
            const notification = document.getElementById('notification');
            const notificationClose = document.getElementById('notificationClose');
            
            if (notificationClose) {
                notificationClose.addEventListener('click', function() {
                    notification.classList.remove('show');
                });
            }
        }
        
        // Show notification
        function showNotification(message) {
            const notification = document.getElementById('notification');
            const notificationText = document.getElementById('notificationText');
            
            if (notification && notificationText) {
                notificationText.textContent = message;
                notification.classList.add('show');
                
                // Auto-hide after 3 seconds
                setTimeout(() => {
                    notification.classList.remove('show');
                }, 3000);
            }
        }
        
        // Initialize modals
        function initModals() {
            // Calendar day modal
            const calendarDayModal = document.getElementById('calendarDayModal');
            const modalClose = document.getElementById('modalClose');
            
            if (calendarDayModal && modalClose) {
                modalClose.addEventListener('click', function() {
                    calendarDayModal.classList.remove('active');
                });
                
                // Close modal when clicking outside
                calendarDayModal.addEventListener('click', function(e) {
                    if (e.target === calendarDayModal) {
                        calendarDayModal.classList.remove('active');
                    }
                });
            }
            
            // Transaction modal
            const transactionModal = document.getElementById('transactionModal');
            const transactionModalClose = document.getElementById('transactionModalClose');
            const newTransactionBtn = document.getElementById('newTransactionBtn');
            const cancelTransactionBtn = document.getElementById('cancelTransactionBtn');
            
            if (transactionModal && transactionModalClose && newTransactionBtn && cancelTransactionBtn) {
                newTransactionBtn.addEventListener('click', function() {
                    transactionModal.classList.add('active');
                });
                
                transactionModalClose.addEventListener('click', function() {
                    transactionModal.classList.remove('active');
                });
                
                cancelTransactionBtn.addEventListener('click', function() {
                    transactionModal.classList.remove('active');
                });
                
                // Close modal when clicking outside
                transactionModal.addEventListener('click', function(e) {
                    if (e.target === transactionModal) {
                        transactionModal.classList.remove('active');
                    }
                });
            }
            
            // Edit transaction modal
            const editTransactionModal = document.getElementById('editTransactionModal');
            const editTransactionModalClose = document.getElementById('editTransactionModalClose');
            const cancelEditTransactionBtn = document.getElementById('cancelEditTransactionBtn');
            
            if (editTransactionModal && editTransactionModalClose && cancelEditTransactionBtn) {
                editTransactionModalClose.addEventListener('click', function() {
                    editTransactionModal.classList.remove('active');
                    currentEditingTransactionId = null;
                });
                
                cancelEditTransactionBtn.addEventListener('click', function() {
                    editTransactionModal.classList.remove('active');
                    currentEditingTransactionId = null;
                });
                
                // Close modal when clicking outside
                editTransactionModal.addEventListener('click', function(e) {
                    if (e.target === editTransactionModal) {
                        editTransactionModal.classList.remove('active');
                        currentEditingTransactionId = null;
                    }
                });
            }
            
            // Note modal
            const noteModal = document.getElementById('noteModal');
            const noteModalClose = document.getElementById('noteModalClose');
            const saveNoteBtn = document.getElementById('saveNoteBtn');
            const cancelNoteBtn = document.getElementById('cancelNoteBtn');
            
            if (noteModal && noteModalClose && saveNoteBtn && cancelNoteBtn) {
                noteModalClose.addEventListener('click', function() {
                    noteModal.classList.remove('active');
                });
                
                cancelNoteBtn.addEventListener('click', function() {
                    noteModal.classList.remove('active');
                });
                
                saveNoteBtn.addEventListener('click', function() {
                    const titleInput = document.getElementById('noteTitle');
                    const contentInput = document.getElementById('noteContent');
                    const categoryInput = document.getElementById('noteCategory');
                    const pinnedInput = document.getElementById('notePinned');
                    
                    const title = titleInput ? titleInput.value : '';
                    const content = contentInput ? contentInput.value : '';
                    const category = categoryInput ? categoryInput.value : '';
                    const isPinned = pinnedInput ? pinnedInput.checked : false;
                    
                    if (title && content) {
                        const notesGrid = document.getElementById('notesGrid');
                        if (notesGrid) {
                            // Generate unique ID for the note
                            const noteId = noteIdCounter++;
                            
                            const newNote = document.createElement('div');
                            newNote.className = `note fade-in ${isPinned ? 'pinned' : ''}`;
                            newNote.setAttribute('data-note-id', noteId);
                            newNote.innerHTML = `
                                <div class="note-category">${getCategoryName(category)}</div>
                                <i class="fas fa-thumbtack note-pin ${isPinned ? 'pinned' : ''}"></i>
                                <div class="note-title">${title}</div>
                                <div class="note-content">${content}</div>
                                <div class="note-date">${new Date().toLocaleDateString()}</div>
                            `;
                            
                            notesGrid.prepend(newNote);
                            
                            // Add pin functionality
                            const pinIcon = newNote.querySelector('.note-pin');
                            if (pinIcon) {
                                pinIcon.addEventListener('click', function() {
                                    this.classList.toggle('pinned');
                                    newNote.classList.toggle('pinned');
                                    
                                    // If pinned, add to dashboard
                                    if (this.classList.contains('pinned')) {
                                        const dashboardNotes = document.querySelector('#dashboard .notes-grid');
                                        if (dashboardNotes) {
                                            const clonedNote = newNote.cloneNode(true);
                                            dashboardNotes.appendChild(clonedNote);
                                            
                                            // Add pin functionality to cloned note
                                            const clonedPin = clonedNote.querySelector('.note-pin');
                                            if (clonedPin) {
                                                clonedPin.addEventListener('click', function() {
                                                    this.classList.toggle('pinned');
                                                    clonedNote.classList.toggle('pinned');
                                                    
                                                    // If unpinned, remove from dashboard
                                                    if (!this.classList.contains('pinned')) {
                                                        clonedNote.remove();
                                                    }
                                                });
                                            }
                                        }
                                    }
                                });
                            }
                            
                            // Add click functionality for editing
                            newNote.addEventListener('click', function(e) {
                                // Don't open edit modal if clicking on pin
                                if (e.target.classList.contains('note-pin')) return;
                                
                                openEditNoteForm(noteId);
                            });
                            
                            // If pinned, add to dashboard
                            if (isPinned) {
                                const dashboardNotes = document.querySelector('#dashboard .notes-grid');
                                if (dashboardNotes) {
                                    const clonedNote = newNote.cloneNode(true);
                                    dashboardNotes.appendChild(clonedNote);
                                    
                                    // Add pin functionality to cloned note
                                    const clonedPin = clonedNote.querySelector('.note-pin');
                                    if (clonedPin) {
                                        clonedPin.addEventListener('click', function() {
                                            this.classList.toggle('pinned');
                                            clonedNote.classList.toggle('pinned');
                                            
                                            // If unpinned, remove from dashboard
                                            if (!this.classList.contains('pinned')) {
                                                clonedNote.remove();
                                            }
                                        });
                                    }
                                }
                            }
                        }
                        
                        // Close modal and reset
                        noteModal.classList.remove('active');
                        if (titleInput) titleInput.value = '';
                        if (contentInput) contentInput.value = '';
                        
                        // Show notification
                        showNotification('Nota guardada correctamente');
                    }
                });
                
                // Close modal when clicking outside
                noteModal.addEventListener('click', function(e) {
                    if (e.target === noteModal) {
                        noteModal.classList.remove('active');
                    }
                });
            }
            
            // Edit note modal
            const editNoteModal = document.getElementById('editNoteModal');
            const editNoteModalClose = document.getElementById('editNoteModalClose');
            const updateNoteBtn = document.getElementById('updateNoteBtn');
            const cancelEditNoteBtn = document.getElementById('cancelEditNoteBtn');
            
            if (editNoteModal && editNoteModalClose && updateNoteBtn && cancelEditNoteBtn) {
                editNoteModalClose.addEventListener('click', function() {
                    editNoteModal.classList.remove('active');
                    currentEditingNoteId = null;
                });
                
                cancelEditNoteBtn.addEventListener('click', function() {
                    editNoteModal.classList.remove('active');
                    currentEditingNoteId = null;
                });
                
                updateNoteBtn.addEventListener('click', function() {
                    const titleInput = document.getElementById('editNoteTitle');
                    const contentInput = document.getElementById('editNoteContent');
                    const categoryInput = document.getElementById('editNoteCategory');
                    const pinnedInput = document.getElementById('editNotePinned');
                    
                    const title = titleInput ? titleInput.value : '';
                    const content = contentInput ? contentInput.value : '';
                    const category = categoryInput ? categoryInput.value : '';
                    const isPinned = pinnedInput ? pinnedInput.checked : false;
                    
                    if (title && content && currentEditingNoteId) {
                        // Find the note element
                        const noteElement = document.querySelector(`[data-note-id="${currentEditingNoteId}"]`);
                        if (noteElement) {
                            // Update note content
                            const noteTitle = noteElement.querySelector('.note-title');
                            const noteContent = noteElement.querySelector('.note-content');
                            const noteCategory = noteElement.querySelector('.note-category');
                            const notePin = noteElement.querySelector('.note-pin');
                            
                            if (noteTitle) noteTitle.textContent = title;
                            if (noteContent) noteContent.textContent = content;
                            if (noteCategory) noteCategory.textContent = getCategoryName(category);
                            
                            // Update pinned status
                            if (isPinned) {
                                noteElement.classList.add('pinned');
                                if (notePin) notePin.classList.add('pinned');
                                
                                // Add to dashboard if not already there
                                const dashboardNote = document.querySelector(`#dashboard .notes-grid [data-note-id="${currentEditingNoteId}"]`);
                                if (!dashboardNote) {
                                    const dashboardNotes = document.querySelector('#dashboard .notes-grid');
                                    if (dashboardNotes) {
                                        const clonedNote = noteElement.cloneNode(true);
                                        dashboardNotes.appendChild(clonedNote);
                                        
                                        // Add pin functionality to cloned note
                                        const clonedPin = clonedNote.querySelector('.note-pin');
                                        if (clonedPin) {
                                            clonedPin.addEventListener('click', function() {
                                                this.classList.toggle('pinned');
                                                clonedNote.classList.toggle('pinned');
                                                
                                                // If unpinned, remove from dashboard
                                                if (!this.classList.contains('pinned')) {
                                                    clonedNote.remove();
                                                }
                                            });
                                        }
                                    }
                                }
                            } else {
                                noteElement.classList.remove('pinned');
                                if (notePin) notePin.classList.remove('pinned');
                                
                                // Remove from dashboard if it was there
                                const dashboardNote = document.querySelector(`#dashboard .notes-grid [data-note-id="${currentEditingNoteId}"]`);
                                if (dashboardNote) {
                                    dashboardNote.remove();
                                }
                            }
                        }
                        
                        // Close modal and reset
                        editNoteModal.classList.remove('active');
                        currentEditingNoteId = null;
                        
                        // Show notification
                        showNotification('Nota actualizada correctamente');
                    }
                });
                
                // Close modal when clicking outside
                editNoteModal.addEventListener('click', function(e) {
                    if (e.target === editNoteModal) {
                        editNoteModal.classList.remove('active');
                        currentEditingNoteId = null;
                    }
                });
            }
            
            // Category modal
            const categoryModal = document.getElementById('categoryModal');
            const categoryModalClose = document.getElementById('categoryModalClose');
            const addCategoryBtn = document.getElementById('addCategoryBtn');
            const saveCategoryBtn = document.getElementById('saveCategoryBtn');
            const cancelCategoryBtn = document.getElementById('cancelCategoryBtn');
            
            if (categoryModal && categoryModalClose && addCategoryBtn && saveCategoryBtn && cancelCategoryBtn) {
                addCategoryBtn.addEventListener('click', function() {
                    categoryModal.classList.add('active');
                });
                
                categoryModalClose.addEventListener('click', function() {
                    categoryModal.classList.remove('active');
                });
                
                cancelCategoryBtn.addEventListener('click', function() {
                    categoryModal.classList.remove('active');
                });
                
                saveCategoryBtn.addEventListener('click', function() {
                    const nameInput = document.getElementById('categoryName');
                    const name = nameInput ? nameInput.value : '';
                    
                    if (name) {
                        // Add new category to the list
                        const categoryList = document.querySelector('.category-list');
                        if (categoryList) {
                            const newCategory = document.createElement('div');
                            newCategory.className = 'category-item';
                            newCategory.innerHTML = `
                                <div class="category-color" style="background-color: ${selectedCategoryColor};"></div>
                                <div class="category-name">${name}</div>
                                <i class="fas fa-times category-delete"></i>
                            `;
                            
                            categoryList.appendChild(newCategory);
                            
                            // Add delete functionality
                            const deleteBtn = newCategory.querySelector('.category-delete');
                            if (deleteBtn) {
                                deleteBtn.addEventListener('click', function() {
                                    newCategory.remove();
                                });
                            }
                        }
                        
                        // Close modal and reset
                        categoryModal.classList.remove('active');
                        if (nameInput) nameInput.value = '';
                        
                        // Show notification
                        showNotification('Categoría agregada correctamente');
                    }
                });
                
                // Color options
                const colorOptions = document.querySelectorAll('#categoryModal .color-option');
                colorOptions.forEach(option => {
                    option.addEventListener('click', function() {
                        // Remove active class from all options
                        colorOptions.forEach(opt => opt.classList.remove('active'));
                        
                        // Add active class to clicked option
                        this.classList.add('active');
                        
                        // Update selected color
                        selectedCategoryColor = this.getAttribute('data-color');
                    });
                });
                
                // Close modal when clicking outside
                categoryModal.addEventListener('click', function(e) {
                    if (e.target === categoryModal) {
                        categoryModal.classList.remove('active');
                    }
                });
            }
        }
        
        // Initialize projects
        function initProjects() {
            const newProjectBtn = document.getElementById('newProjectBtn');
            
            if (newProjectBtn) {
                newProjectBtn.addEventListener('click', function() {
                    showNotification('Función de crear proyecto próximamente');
                });
            }
            
            // Initialize project task checkboxes
            const projectTaskCheckboxes = document.querySelectorAll('.project-task-checkbox');
            projectTaskCheckboxes.forEach(checkbox => {
                checkbox.addEventListener('click', function() {
                    this.classList.toggle('checked');
                    
                    // Update project progress
                    const projectCard = this.closest('.project-card');
                    if (projectCard) {
                        updateProjectProgress(projectCard);
                    }
                });
            });
        }
        
        // Update project progress
        function updateProjectProgress(projectCard) {
            const taskCheckboxes = projectCard.querySelectorAll('.project-task-checkbox');
            const totalTasks = taskCheckboxes.length;
            const completedTasks = projectCard.querySelectorAll('.project-task-checkbox.checked').length;
            
            const progressPercent = Math.round((completedTasks / totalTasks) * 100);
            
            const progressFill = projectCard.querySelector('.progress-fill');
            const progressText = projectCard.querySelector('.progress-text span:first-child');
            
            if (progressFill) {
                progressFill.style.width = `${progressPercent}%`;
            }
            
            if (progressText) {
                progressText.textContent = `${progressPercent}% Completado`;
            }
        }
        
        // Initialize finance
        function initFinance() {
            const saveTransactionBtn = document.getElementById('saveTransactionBtn');
            const transactionCards = document.getElementById('transactionCards');
            
            if (saveTransactionBtn && transactionCards) {
                saveTransactionBtn.addEventListener('click', function() {
                    const transactionType = document.getElementById('transactionType');
                    const transactionDescription = document.getElementById('transactionDescription');
                    const transactionCategory = document.getElementById('transactionCategory');
                    const transactionAmount = document.getElementById('transactionAmount');
                    const transactionDate = document.getElementById('transactionDate');
                    
                    const type = transactionType ? transactionType.value : '';
                    const description = transactionDescription ? transactionDescription.value : '';
                    const category = transactionCategory ? transactionCategory.value : '';
                    const amount = transactionAmount ? parseFloat(transactionAmount.value) : 0;
                    const date = transactionDate ? transactionDate.value : new Date().toISOString().split('T')[0];
                    
                    if (description && amount > 0 && transactionCards) {
                        // Create new transaction card
                        const newCard = document.createElement('div');
                        newCard.className = `transaction-card ${type}`;
                        newCard.setAttribute('data-transaction-id', transactionIdCounter++);
                        
                        const formattedAmount = type === 'income' ? `+$${amount.toFixed(2)}` : `-$${amount.toFixed(2)}`;
                        const categoryName = getTransactionCategoryName(category);
                        
                        newCard.innerHTML = `
                            <div class="transaction-header">
                                <div class="transaction-title">${description}</div>
                                <div class="transaction-amount ${type}">${formattedAmount}</div>
                            </div>
                            <div class="transaction-details">
                                <div>${categoryName}</div>
                                <div>${formatDate(date)}</div>
                            </div>
                            <div class="transaction-actions">
                                <button class="transaction-action-btn edit" data-transaction-id="${newCard.getAttribute('data-transaction-id')}">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="transaction-action-btn delete" data-transaction-id="${newCard.getAttribute('data-transaction-id')}">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        `;
                        
                        // Add to transaction cards
                        transactionCards.prepend(newCard);
                        
                        // Add edit functionality
                        const editBtn = newCard.querySelector('.transaction-action-btn.edit');
                        if (editBtn) {
                            editBtn.addEventListener('click', function() {
                                const transactionId = this.getAttribute('data-transaction-id');
                                openEditTransactionForm(transactionId);
                            });
                        }
                        
                        // Add delete functionality
                        const deleteBtn = newCard.querySelector('.transaction-action-btn.delete');
                        if (deleteBtn) {
                            deleteBtn.addEventListener('click', function() {
                                const transactionId = this.getAttribute('data-transaction-id');
                                deleteTransaction(transactionId);
                            });
                        }
                        
                        // Close modal
                        const transactionModal = document.getElementById('transactionModal');
                        if (transactionModal) {
                            transactionModal.classList.remove('active');
                        }
                        
                        // Clear form
                        if (transactionDescription) transactionDescription.value = '';
                        if (transactionAmount) transactionAmount.value = '';
                        if (transactionDate) transactionDate.value = '';
                        
                        // Show notification
                        showNotification('Transacción agregada correctamente');
                        
                        // Update finance summary
                        updateFinanceSummary(type, amount);
                        
                        // Update charts if initialized
                        if (financeChartsInitialized) {
                            updateFinanceCharts();
                        }
                    }
                });
            }
            
            // Update transaction form functionality
            const updateTransactionBtn = document.getElementById('updateTransactionBtn');
            
            if (updateTransactionBtn) {
                updateTransactionBtn.addEventListener('click', function() {
                    const transactionType = document.getElementById('editTransactionType');
                    const transactionDescription = document.getElementById('editTransactionDescription');
                    const transactionCategory = document.getElementById('editTransactionCategory');
                    const transactionAmount = document.getElementById('editTransactionAmount');
                    const transactionDate = document.getElementById('editTransactionDate');
                    
                    const type = transactionType ? transactionType.value : '';
                    const description = transactionDescription ? transactionDescription.value : '';
                    const category = transactionCategory ? transactionCategory.value : '';
                    const amount = transactionAmount ? parseFloat(transactionAmount.value) : 0;
                    const date = transactionDate ? transactionDate.value : new Date().toISOString().split('T')[0];
                    
                    if (description && amount > 0 && currentEditingTransactionId) {
                        // Find the transaction element
                        const transactionElement = document.querySelector(`[data-transaction-id="${currentEditingTransactionId}"]`);
                        if (transactionElement) {
                            // Get old values for summary update
                            const oldType = transactionElement.classList.contains('income') ? 'income' : 'expense';
                            const oldAmountText = transactionElement.querySelector('.transaction-amount').textContent;
                            const oldAmount = parseFloat(oldAmountText.replace(/[^0-9.-]/g, ''));
                            
                            // Update transaction properties
                            transactionElement.className = `transaction-card ${type}`;
                            
                            // Update transaction content
                            const transactionTitle = transactionElement.querySelector('.transaction-title');
                            const transactionAmountElement = transactionElement.querySelector('.transaction-amount');
                            const transactionDetails = transactionElement.querySelectorAll('.transaction-details div');
                            
                            if (transactionTitle) transactionTitle.textContent = description;
                            if (transactionAmountElement) {
                                const formattedAmount = type === 'income' ? `+$${amount.toFixed(2)}` : `-$${amount.toFixed(2)}`;
                                transactionAmountElement.textContent = formattedAmount;
                                transactionAmountElement.className = `transaction-amount ${type}`;
                            }
                            
                            if (transactionDetails[0]) transactionDetails[0].textContent = getTransactionCategoryName(category);
                            if (transactionDetails[1]) transactionDetails[1].textContent = formatDate(date);
                            
                            // Update finance summary
                            // First, reverse the old transaction
                            const reversedType = oldType === 'income' ? 'expense' : 'income';
                            updateFinanceSummary(reversedType, oldAmount);
                            
                            // Then, apply the new transaction
                            updateFinanceSummary(type, amount);
                            
                            // Update charts if initialized
                            if (financeChartsInitialized) {
                                updateFinanceCharts();
                            }
                        }
                        
                        // Close modal and reset
                        const editTransactionModal = document.getElementById('editTransactionModal');
                        if (editTransactionModal) {
                            editTransactionModal.classList.remove('active');
                        }
                        currentEditingTransactionId = null;
                        
                        // Show notification
                        showNotification('Transacción actualizada correctamente');
                    }
                });
            }
            
            // Initialize transaction action buttons
            const transactionEditBtns = document.querySelectorAll('.transaction-action-btn.edit');
            const transactionDeleteBtns = document.querySelectorAll('.transaction-action-btn.delete');
            
            transactionEditBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    const transactionId = this.getAttribute('data-transaction-id');
                    openEditTransactionForm(transactionId);
                });
            });
            
            transactionDeleteBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    const transactionId = this.getAttribute('data-transaction-id');
                    deleteTransaction(transactionId);
                });
            });
            
            // Export finance to Excel
            const exportFinanceExcelBtn = document.getElementById('exportFinanceExcelBtn');
            if (exportFinanceExcelBtn) {
                exportFinanceExcelBtn.addEventListener('click', function() {
                    exportFinanceToExcel();
                });
            }
        }
        
        // Open edit transaction form
        function openEditTransactionForm(transactionId) {
            const transactionElement = document.querySelector(`[data-transaction-id="${transactionId}"]`);
            if (!transactionElement) return;
            
            const editTransactionModal = document.getElementById('editTransactionModal');
            const typeInput = document.getElementById('editTransactionType');
            const descriptionInput = document.getElementById('editTransactionDescription');
            const categoryInput = document.getElementById('editTransactionCategory');
            const amountInput = document.getElementById('editTransactionAmount');
            const dateInput = document.getElementById('editTransactionDate');
            
            if (!editTransactionModal || !typeInput || !descriptionInput || !categoryInput || !amountInput || !dateInput) return;
            
            // Get current transaction values
            const transactionTitle = transactionElement.querySelector('.transaction-title').textContent;
            const transactionAmountText = transactionElement.querySelector('.transaction-amount').textContent;
            const transactionCategoryText = transactionElement.querySelectorAll('.transaction-details div')[0].textContent;
            const transactionDateText = transactionElement.querySelectorAll('.transaction-details div')[1].textContent;
            
            // Extract type and amount from text
            const isIncome = transactionElement.classList.contains('income');
            const type = isIncome ? 'income' : 'expense';
            const amount = parseFloat(transactionAmountText.replace(/[^0-9.-]/g, ''));
            
            // Extract category from text
            let category = 'food';
            if (transactionCategoryText === 'Transporte') category = 'transport';
            else if (transactionCategoryText === 'Entretenimiento') category = 'entertainment';
            else if (transactionCategoryText === 'Salud') category = 'health';
            else if (transactionCategoryText === 'Educación') category = 'education';
            else if (transactionCategoryText === 'Inversión') category = 'investment';
            else if (transactionCategoryText === 'Otros') category = 'other';
            
            // Parse date
            const date = parseDate(transactionDateText);
            
            // Set form values
            if (typeInput) typeInput.value = type;
            if (descriptionInput) descriptionInput.value = transactionTitle;
            if (categoryInput) categoryInput.value = category;
            if (amountInput) amountInput.value = amount;
            if (dateInput) dateInput.value = date;
            
            // Set current editing transaction ID
            currentEditingTransactionId = transactionId;
            
            // Show modal
            editTransactionModal.classList.add('active');
        }
        
        // Delete transaction
        function deleteTransaction(transactionId) {
            const transactionElement = document.querySelector(`[data-transaction-id="${transactionId}"]`);
            if (!transactionElement) return;
            
            // Get transaction values for summary update
            const isIncome = transactionElement.classList.contains('income');
            const type = isIncome ? 'income' : 'expense';
            const amountText = transactionElement.querySelector('.transaction-amount').textContent;
            const amount = parseFloat(amountText.replace(/[^0-9.-]/g, ''));
            
            // Remove transaction
            transactionElement.style.opacity = '0';
            setTimeout(() => {
                transactionElement.remove();
            }, 300);
            
            // Update finance summary (reverse the transaction)
            const reversedType = type === 'income' ? 'expense' : 'income';
            updateFinanceSummary(reversedType, amount);
            
            // Update charts if initialized
            if (financeChartsInitialized) {
                updateFinanceCharts();
            }
            
            // Show notification
            showNotification('Transacción eliminada correctamente');
        }
        
        // Update finance summary
        function updateFinanceSummary(type, amount) {
            const incomeElement = document.getElementById('totalIncome');
            const expenseElement = document.getElementById('totalExpense');
            const balanceElement = document.getElementById('totalBalance');
            const dashboardBalanceElement = document.getElementById('dashboardBalance');
            
            if (incomeElement && expenseElement && balanceElement) {
                const currentIncome = parseFloat(incomeElement.textContent.replace('$', '').replace(',', ''));
                const currentExpense = parseFloat(expenseElement.textContent.replace('$', '').replace(',', ''));
                
                let newIncome = currentIncome;
                let newExpense = currentExpense;
                
                if (type === 'income') {
                    newIncome += amount;
                } else {
                    newExpense += amount;
                }
                
                const newBalance = newIncome - newExpense;
                
                incomeElement.textContent = `$${newIncome.toFixed(2).replace(/\B(?=(\d{3})+(?!\d))/g, ',')}`;
                expenseElement.textContent = `$${newExpense.toFixed(2).replace(/\B(?=(\d{3})+(?!\d))/g, ',')}`;
                balanceElement.textContent = `$${newBalance.toFixed(2).replace(/\B(?=(\d{3})+(?!\d))/g, ',')}`;
                
                // Update dashboard balance
                if (dashboardBalanceElement) {
                    dashboardBalanceElement.textContent = `$${newBalance.toFixed(2).replace(/\B(?=(\d{3})+(?!\d))/g, ',')}`;
                }
                
                // Update color based on balance
                if (newBalance < 0) {
                    balanceElement.style.color = 'var(--danger)';
                    if (dashboardBalanceElement) {
                        dashboardBalanceElement.style.color = 'var(--danger)';
                    }
                } else {
                    balanceElement.style.color = 'var(--success)';
                    if (dashboardBalanceElement) {
                        dashboardBalanceElement.style.color = 'var(--success)';
                    }
                }
            }
        }
        
        // Update finance charts
        function updateFinanceCharts() {
            // This function would update the finance charts with new data
            // For now, we'll just reinitialize the charts
            if (financeChartsInitialized) {
                // Destroy existing charts
                Chart.helpers.each(Chart.instances, function(instance) {
                    if (instance.canvas.id === 'incomeExpenseChart' || 
                        instance.canvas.id === 'cashFlowChart' || 
                        instance.canvas.id === 'expenseDistributionChart') {
                        instance.destroy();
                    }
                });
                
                // Reinitialize finance charts
                initFinanceCharts();
            }
        }
        
        // Export finance to Excel
        function exportFinanceToExcel() {
            const transactions = [];
            
            // Get all transaction data
            const transactionCards = document.querySelectorAll('.transaction-card');
            transactionCards.forEach(card => {
                const title = card.querySelector('.transaction-title').textContent;
                const amountText = card.querySelector('.transaction-amount').textContent;
                const category = card.querySelectorAll('.transaction-details div')[0].textContent;
                const date = card.querySelectorAll('.transaction-details div')[1].textContent;
                const isIncome = card.classList.contains('income');
                
                transactions.push({
                    Título: title,
                    Monto: amountText,
                    Categoría: category,
                    Fecha: date,
                    Tipo: isIncome ? 'Ingreso' : 'Egreso'
                });
            });
            
            // Create workbook
            const wb = XLSX.utils.book_new();
            const ws = XLSX.utils.json_to_sheet(transactions);
            
            // Add worksheet to workbook
            XLSX.utils.book_append_sheet(wb, ws, 'Transacciones');
            
            // Save workbook
            XLSX.writeFile(wb, 'transacciones.xlsx');
            
            // Show notification
            showNotification('Datos exportados a Excel correctamente');
        }
        
        // Initialize user management
        function initUserManagement() {
            const inviteUserBtn = document.getElementById('inviteUserBtn');
            const sendInviteBtn = document.getElementById('sendInviteBtn');
            const inviteUserCode = document.getElementById('inviteUserCode');
            
            if (inviteUserBtn) {
                inviteUserBtn.addEventListener('click', function() {
                    showNotification('Función de invitar usuario próximamente');
                });
            }
            
            if (sendInviteBtn && inviteUserCode) {
                sendInviteBtn.addEventListener('click', function() {
                    const code = inviteUserCode.value;
                    if (code) {
                        // For demo purposes, we'll just show a notification
                        showNotification(`Invitación enviada al usuario con código: ${code}`);
                        inviteUserCode.value = '';
                    } else {
                        showNotification('Por favor, ingresa un código de usuario');
                    }
                });
            }
            
            // User card buttons
            const userCardBtns = document.querySelectorAll('.user-card-btn');
            userCardBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    if (this.classList.contains('danger')) {
                        showNotification('Función de eliminar usuario próximamente');
                    } else {
                        showNotification('Función de editar usuario próximamente');
                    }
                });
            });
        }
        
        // Initialize reports
        function initReports() {
            const generateReportBtn = document.getElementById('generateReportBtn');
            
            if (generateReportBtn) {
                generateReportBtn.addEventListener('click', function() {
                    showNotification('Función de generar reporte próximamente');
                });
            }
            
            // Weekly tasks PDF
            const weeklyTasksPdfBtn = document.getElementById('weeklyTasksPdfBtn');
            if (weeklyTasksPdfBtn) {
                weeklyTasksPdfBtn.addEventListener('click', function() {
                    generateWeeklyTasksPDF();
                });
            }
            
            // Weekly tasks Excel
            const weeklyTasksExcelBtn = document.getElementById('weeklyTasksExcelBtn');
            if (weeklyTasksExcelBtn) {
                weeklyTasksExcelBtn.addEventListener('click', function() {
                    generateWeeklyTasksExcel();
                });
            }
            
            // Monthly finance PDF
            const monthlyFinancePdfBtn = document.getElementById('monthlyFinancePdfBtn');
            if (monthlyFinancePdfBtn) {
                monthlyFinancePdfBtn.addEventListener('click', function() {
                    generateMonthlyFinancePDF();
                });
            }
            
            // Monthly finance Excel
            const monthlyFinanceExcelBtn = document.getElementById('monthlyFinanceExcelBtn');
            if (monthlyFinanceExcelBtn) {
                monthlyFinanceExcelBtn.addEventListener('click', function() {
                    generateMonthlyFinanceExcel();
                });
            }
            
            // Productivity PDF
            const productivityPdfBtn = document.getElementById('productivityPdfBtn');
            if (productivityPdfBtn) {
                productivityPdfBtn.addEventListener('click', function() {
                    generateProductivityPDF();
                });
            }
            
            // Productivity Excel
            const productivityExcelBtn = document.getElementById('productivityExcelBtn');
            if (productivityExcelBtn) {
                productivityExcelBtn.addEventListener('click', function() {
                    generateProductivityExcel();
                });
            }
            
            // Projects PDF
            const projectsPdfBtn = document.getElementById('projectsPdfBtn');
            if (projectsPdfBtn) {
                projectsPdfBtn.addEventListener('click', function() {
                    generateProjectsPDF();
                });
            }
            
            // Projects Excel
            const projectsExcelBtn = document.getElementById('projectsExcelBtn');
            if (projectsExcelBtn) {
                projectsExcelBtn.addEventListener('click', function() {
                    generateProjectsExcel();
                });
            }
        }
        
        // Generate weekly tasks PDF
        function generateWeeklyTasksPDF() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();
            
            // Add title
            doc.setFontSize(18);
            doc.text('Reporte Semanal de Tareas', 14, 22);
            
            // Add date
            doc.setFontSize(12);
            const today = new Date();
            const formattedDate = today.toLocaleDateString('es-ES');
            doc.text(`Fecha: ${formattedDate}`, 14, 32);
            
            // Add tasks table
            const tasks = [];
            
            // Get all tasks
            const allTasks = document.querySelectorAll('#taskList .task-item');
            allTasks.forEach(task => {
                const title = task.querySelector('.task-title').textContent;
                const description = task.querySelector('.task-description').textContent;
                const priority = task.querySelector('.task-priority').textContent;
                const category = task.querySelector('.task-category').textContent;
                const dates = task.querySelector('.task-dates').textContent;
                
                tasks.push([
                    title,
                    description,
                    priority,
                    category,
                    dates
                ]);
            });
            
            // Add table to PDF
            doc.autoTable({
                head: [['Título', 'Descripción', 'Prioridad', 'Categoría', 'Fechas']],
                body: tasks,
                startY: 40
            });
            
            // Save PDF
            doc.save('reporte_semanal_tareas.pdf');
            
            // Show notification
            showNotification('Reporte PDF generado correctamente');
        }
        
        // Generate weekly tasks Excel
        function generateWeeklyTasksExcel() {
            const tasks = [];
            
            // Get all tasks
            const allTasks = document.querySelectorAll('#taskList .task-item');
            allTasks.forEach(task => {
                const title = task.querySelector('.task-title').textContent;
                const description = task.querySelector('.task-description').textContent;
                const priority = task.querySelector('.task-priority').textContent;
                const category = task.querySelector('.task-category').textContent;
                const dates = task.querySelector('.task-dates').textContent;
                
                tasks.push({
                    Título: title,
                    Descripción: description,
                    Prioridad: priority,
                    Categoría: category,
                    Fechas: dates
                });
            });
            
            // Create workbook
            const wb = XLSX.utils.book_new();
            const ws = XLSX.utils.json_to_sheet(tasks);
            
            // Add worksheet to workbook
            XLSX.utils.book_append_sheet(wb, ws, 'Tareas Semanales');
            
            // Save workbook
            XLSX.writeFile(wb, 'reporte_semanal_tareas.xlsx');
            
            // Show notification
            showNotification('Reporte Excel generado correctamente');
        }
        
        // Generate monthly finance PDF
        function generateMonthlyFinancePDF() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();
            
            // Add title
            doc.setFontSize(18);
            doc.text('Reporte Mensual de Finanzas', 14, 22);
            
            // Add date
            doc.setFontSize(12);
            const today = new Date();
            const formattedDate = today.toLocaleDateString('es-ES');
            doc.text(`Fecha: ${formattedDate}`, 14, 32);
            
            // Add summary
            const incomeElement = document.getElementById('totalIncome');
            const expenseElement = document.getElementById('totalExpense');
            const balanceElement = document.getElementById('totalBalance');
            
            if (incomeElement && expenseElement && balanceElement) {
                const income = incomeElement.textContent;
                const expense = expenseElement.textContent;
                const balance = balanceElement.textContent;
                
                doc.setFontSize(14);
                doc.text('Resumen Financiero', 14, 45);
                
                doc.setFontSize(12);
                doc.text(`Ingresos: ${income}`, 14, 55);
                doc.text(`Gastos: ${expense}`, 14, 62);
                doc.text(`Balance: ${balance}`, 14, 69);
            }
            
            // Add transactions table
            const transactions = [];
            
            // Get all transactions
            const allTransactions = document.querySelectorAll('.transaction-card');
            allTransactions.forEach(transaction => {
                const title = transaction.querySelector('.transaction-title').textContent;
                const amount = transaction.querySelector('.transaction-amount').textContent;
                const category = transaction.querySelectorAll('.transaction-details div')[0].textContent;
                const date = transaction.querySelectorAll('.transaction-details div')[1].textContent;
                
                transactions.push([
                    title,
                    amount,
                    category,
                    date
                ]);
            });
            
            // Add table to PDF
            doc.autoTable({
                head: [['Descripción', 'Monto', 'Categoría', 'Fecha']],
                body: transactions,
                startY: 80
            });
            
            // Save PDF
            doc.save('reporte_mensual_finanzas.pdf');
            
            // Show notification
            showNotification('Reporte PDF generado correctamente');
        }
        
        // Generate monthly finance Excel
        function generateMonthlyFinanceExcel() {
            const transactions = [];
            
            // Get all transactions
            const allTransactions = document.querySelectorAll('.transaction-card');
            allTransactions.forEach(transaction => {
                const title = transaction.querySelector('.transaction-title').textContent;
                const amount = transaction.querySelector('.transaction-amount').textContent;
                const category = transaction.querySelectorAll('.transaction-details div')[0].textContent;
                const date = transaction.querySelectorAll('.transaction-details div')[1].textContent;
                
                transactions.push({
                    Descripción: title,
                    Monto: amount,
                    Categoría: category,
                    Fecha: date
                });
            });
            
            // Create workbook
            const wb = XLSX.utils.book_new();
            const ws = XLSX.utils.json_to_sheet(transactions);
            
            // Add worksheet to workbook
            XLSX.utils.book_append_sheet(wb, ws, 'Transacciones Mensuales');
            
            // Save workbook
            XLSX.writeFile(wb, 'reporte_mensual_finanzas.xlsx');
            
            // Show notification
            showNotification('Reporte Excel generado correctamente');
        }
        
        // Generate productivity PDF
        function generateProductivityPDF() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();
            
            // Add title
            doc.setFontSize(18);
            doc.text('Reporte de Productividad', 14, 22);
            
            // Add date
            doc.setFontSize(12);
            const today = new Date();
            const formattedDate = today.toLocaleDateString('es-ES');
            doc.text(`Fecha: ${formattedDate}`, 14, 32);
            
            // Add productivity summary
            doc.setFontSize(14);
            doc.text('Resumen de Productividad', 14, 45);
            
            doc.setFontSize(12);
            doc.text('Tareas completadas esta semana: 24', 14, 55);
            doc.text('Tareas pendientes: 12', 14, 62);
            doc.text('Productividad: 67%', 14, 69);
            
            // Add tasks by category
            const categories = {
                'Trabajo': 18,
                'Personal': 12,
                'Salud': 8,
                'Estudio': 10,
                'Otros': 5
            };
            
            doc.setFontSize(14);
            doc.text('Tareas por Categoría', 14, 85);
            
            doc.setFontSize(12);
            let yPos = 95;
            for (const [category, count] of Object.entries(categories)) {
                doc.text(`${category}: ${count} tareas`, 14, yPos);
                yPos += 7;
            }
            
            // Save PDF
            doc.save('reporte_productividad.pdf');
            
            // Show notification
            showNotification('Reporte PDF generado correctamente');
        }
        
        // Generate productivity Excel
        function generateProductivityExcel() {
            const data = [];
            
            // Add summary data
            data.push({
                Métrica: 'Tareas completadas esta semana',
                Valor: 24
            });
            
            data.push({
                Métrica: 'Tareas pendientes',
                Valor: 12
            });
            
            data.push({
                Métrica: 'Productividad',
                Valor: '67%'
            });
            
            // Add category data
            const categories = {
                'Trabajo': 18,
                'Personal': 12,
                'Salud': 8,
                'Estudio': 10,
                'Otros': 5
            };
            
            for (const [category, count] of Object.entries(categories)) {
                data.push({
                    Métrica: `Tareas de ${category}`,
                    Valor: count
                });
            }
            
            // Create workbook
            const wb = XLSX.utils.book_new();
            const ws = XLSX.utils.json_to_sheet(data);
            
            // Add worksheet to workbook
            XLSX.utils.book_append_sheet(wb, ws, 'Productividad');
            
            // Save workbook
            XLSX.writeFile(wb, 'reporte_productividad.xlsx');
            
            // Show notification
            showNotification('Reporte Excel generado correctamente');
        }
        
        // Generate projects PDF
        function generateProjectsPDF() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();
            
            // Add title
            doc.setFontSize(18);
            doc.text('Reporte de Proyectos', 14, 22);
            
            // Add date
            doc.setFontSize(12);
            const today = new Date();
            const formattedDate = today.toLocaleDateString('es-ES');
            doc.text(`Fecha: ${formattedDate}`, 14, 32);
            
            // Add projects data
            const projects = [];
            
            // Get all projects
            const allProjects = document.querySelectorAll('.project-card');
            allProjects.forEach(project => {
                const title = project.querySelector('.project-title').textContent;
                const status = project.querySelector('.project-status').textContent;
                const progressText = project.querySelector('.progress-text span:first-child').textContent;
                
                projects.push([
                    title,
                    status,
                    progressText
                ]);
            });
            
            // Add table to PDF
            doc.autoTable({
                head: [['Proyecto', 'Estado', 'Progreso']],
                body: projects,
                startY: 40
            });
            
            // Save PDF
            doc.save('reporte_proyectos.pdf');
            
            // Show notification
            showNotification('Reporte PDF generado correctamente');
        }
        
        // Generate projects Excel
        function generateProjectsExcel() {
            const projects = [];
            
            // Get all projects
            const allProjects = document.querySelectorAll('.project-card');
            allProjects.forEach(project => {
                const title = project.querySelector('.project-title').textContent;
                const status = project.querySelector('.project-status').textContent;
                const progressText = project.querySelector('.progress-text span:first-child').textContent;
                
                projects.push({
                    Proyecto: title,
                    Estado: status,
                    Progreso: progressText
                });
            });
            
            // Create workbook
            const wb = XLSX.utils.book_new();
            const ws = XLSX.utils.json_to_sheet(projects);
            
            // Add worksheet to workbook
            XLSX.utils.book_append_sheet(wb, ws, 'Proyectos');
            
            // Save workbook
            XLSX.writeFile(wb, 'reporte_proyectos.xlsx');
            
            // Show notification
            showNotification('Reporte Excel generado correctamente');
        }
        
        // Initialize settings
        function initSettings() {
            // Theme options
            const colorOptions = document.querySelectorAll('.color-option');
            colorOptions.forEach(option => {
                option.addEventListener('click', function() {
                    const theme = this.getAttribute('data-theme');
                    applyTheme(theme);
                    
                    // Save theme preference
                    localStorage.setItem('theme', theme);
                });
            });
            
            // Load saved theme
            const savedTheme = localStorage.getItem('theme');
            if (savedTheme) {
                applyTheme(savedTheme);
            }
            
            // Export tasks to Excel
            const exportTasksExcelBtn = document.getElementById('exportTasksExcelBtn');
            if (exportTasksExcelBtn) {
                exportTasksExcelBtn.addEventListener('click', function() {
                    exportTasksToExcel();
                });
            }
            
            // Export tasks to PDF
            const exportTasksPdfBtn = document.getElementById('exportTasksPdfBtn');
            if (exportTasksPdfBtn) {
                exportTasksPdfBtn.addEventListener('click', function() {
                    exportTasksToPDF();
                });
            }
            
            // Export finance to PDF
            const exportFinancePdfBtn = document.getElementById('exportFinancePdfBtn');
            if (exportFinancePdfBtn) {
                exportFinancePdfBtn.addEventListener('click', function() {
                    generateMonthlyFinancePDF();
                });
            }
            
            // Category delete buttons
            const categoryDeleteBtns = document.querySelectorAll('.category-delete');
            categoryDeleteBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    const categoryItem = this.closest('.category-item');
                    if (categoryItem) {
                        categoryItem.remove();
                        showNotification('Categoría eliminada correctamente');
                    }
                });
            });
        }
        
        // Apply theme
        function applyTheme(theme) {
            document.body.className = '';
            
            if (theme === 'blue') {
                document.body.classList.add('theme-blue');
            } else if (theme === 'green') {
                document.body.classList.add('theme-green');
            } else if (theme === 'purple') {
                document.body.classList.add('theme-purple');
            } else if (theme === 'red') {
                document.body.classList.add('theme-red');
            }
        }
        
        // Export tasks to Excel
        function exportTasksToExcel() {
            const tasks = [];
            
            // Get all tasks
            const allTasks = document.querySelectorAll('#taskList .task-item');
            allTasks.forEach(task => {
                const title = task.querySelector('.task-title').textContent;
                const description = task.querySelector('.task-description').textContent;
                const priority = task.querySelector('.task-priority').textContent;
                const category = task.querySelector('.task-category').textContent;
                const dates = task.querySelector('.task-dates').textContent;
                
                tasks.push({
                    Título: title,
                    Descripción: description,
                    Prioridad: priority,
                    Categoría: category,
                    Fechas: dates
                });
            });
            
            // Create workbook
            const wb = XLSX.utils.book_new();
            const ws = XLSX.utils.json_to_sheet(tasks);
            
            // Add worksheet to workbook
            XLSX.utils.book_append_sheet(wb, ws, 'Tareas');
            
            // Save workbook
            XLSX.writeFile(wb, 'tareas.xlsx');
            
            // Show notification
            showNotification('Tareas exportadas a Excel correctamente');
        }
        
        // Export tasks to PDF
        function exportTasksToPDF() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();
            
            // Add title
            doc.setFontSize(18);
            doc.text('Reporte de Tareas', 14, 22);
            
            // Add date
            doc.setFontSize(12);
            const today = new Date();
            const formattedDate = today.toLocaleDateString('es-ES');
            doc.text(`Fecha: ${formattedDate}`, 14, 32);
            
            // Add tasks table
            const tasks = [];
            
            // Get all tasks
            const allTasks = document.querySelectorAll('#taskList .task-item');
            allTasks.forEach(task => {
                const title = task.querySelector('.task-title').textContent;
                const description = task.querySelector('.task-description').textContent;
                const priority = task.querySelector('.task-priority').textContent;
                const category = task.querySelector('.task-category').textContent;
                const dates = task.querySelector('.task-dates').textContent;
                
                tasks.push([
                    title,
                    description,
                    priority,
                    category,
                    dates
                ]);
            });
            
            // Add table to PDF
            doc.autoTable({
                head: [['Título', 'Descripción', 'Prioridad', 'Categoría', 'Fechas']],
                body: tasks,
                startY: 40
            });
            
            // Save PDF
            doc.save('tareas.pdf');
            
            // Show notification
            showNotification('Tareas exportadas a PDF correctamente');
        }
        
        // Initialize search functionality
        function initSearch() {
            const searchInputs = document.querySelectorAll('.search-input');
            
            searchInputs.forEach(input => {
                input.addEventListener('input', function() {
                    const searchTerm = this.value.toLowerCase();
                    
                    if (searchTerm.length > 2) {
                        // Search in tasks
                        const allTasks = document.querySelectorAll('.task-item');
                        allTasks.forEach(task => {
                            const titleElement = task.querySelector('.task-title');
                            const descriptionElement = task.querySelector('.task-description');
                            
                            if (titleElement && descriptionElement) {
                                const title = titleElement.textContent.toLowerCase();
                                const description = descriptionElement.textContent.toLowerCase();
                                
                                if (title.includes(searchTerm) || description.includes(searchTerm)) {
                                    task.style.display = 'flex';
                                    task.style.backgroundColor = 'rgba(255, 165, 89, 0.1)';
                                } else {
                                    task.style.display = 'none';
                                }
                            }
                        });
                        
                        // Search in notes
                        const allNotes = document.querySelectorAll('.note');
                        allNotes.forEach(note => {
                            const titleElement = note.querySelector('.note-title');
                            const contentElement = note.querySelector('.note-content');
                            
                            if (titleElement && contentElement) {
                                const title = titleElement.textContent.toLowerCase();
                                const content = contentElement.textContent.toLowerCase();
                                
                                if (title.includes(searchTerm) || content.includes(searchTerm)) {
                                    note.style.display = 'block';
                                    note.style.backgroundColor = 'rgba(255, 165, 89, 0.1)';
                                } else {
                                    note.style.display = 'none';
                                }
                            }
                        });
                        
                        // Search in transactions
                        const allTransactions = document.querySelectorAll('.transaction-card');
                        allTransactions.forEach(transaction => {
                            const titleElement = transaction.querySelector('.transaction-title');
                            
                            if (titleElement) {
                                const title = titleElement.textContent.toLowerCase();
                                
                                if (title.includes(searchTerm)) {
                                    transaction.style.display = 'block';
                                    transaction.style.backgroundColor = 'rgba(255, 165, 89, 0.1)';
                                } else {
                                    transaction.style.display = 'none';
                                }
                            }
                        });
                    } else {
                        // Reset display
                        const allTasks = document.querySelectorAll('.task-item');
                        allTasks.forEach(task => {
                            task.style.display = 'flex';
                            task.style.backgroundColor = '';
                        });
                        
                        const allNotes = document.querySelectorAll('.note');
                        allNotes.forEach(note => {
                            note.style.display = 'block';
                            note.style.backgroundColor = '';
                        });
                        
                        const allTransactions = document.querySelectorAll('.transaction-card');
                        allTransactions.forEach(transaction => {
                            transaction.style.display = 'block';
                            transaction.style.backgroundColor = '';
                        });
                    }
                });
            });
        }
        
        // Initialize sortable elements
        function initSortable() {
            // Initialize sortable for task lists
            const taskLists = document.querySelectorAll('.task-list');
            taskLists.forEach(list => {
                new Sortable(list, {
                    animation: 150,
                    ghostClass: 'sortable-ghost',
                    dragClass: 'sortable-drag'
                });
            });
            
            // Initialize sortable for notes grid
            const notesGrid = document.getElementById('notesGrid');
            if (notesGrid) {
                new Sortable(notesGrid, {
                    animation: 150,
                    ghostClass: 'sortable-ghost',
                    dragClass: 'sortable-drag'
                });
            }
        }
        
        // Helper function to get category name
        function getCategoryName(category) {
            const categories = {
                'work': 'Trabajo',
                'personal': 'Personal',
                'health': 'Salud',
                'study': 'Estudio',
                'other': 'Otros'
            };
            return categories[category] || category;
        }
        
        // Helper function to get transaction category name
        function getTransactionCategoryName(category) {
            const categories = {
                'food': 'Alimentación',
                'transport': 'Transporte',
                'entertainment': 'Entretenimiento',
                'health': 'Salud',
                'education': 'Educación',
                'investment': 'Inversión',
                'other': 'Otros'
            };
            return categories[category] || category;
        }
        
        // Helper function to format date
        function formatDate(dateString) {
            const date = new Date(dateString);
            return date.toLocaleDateString();
        }
        
        // Helper function to parse date
        function parseDate(dateString) {
            const parts = dateString.split('/');
            if (parts.length === 3) {
                return `${parts[2]}-${parts[1].padStart(2, '0')}-${parts[0].padStart(2, '0')}`;
            }
            return dateString;
        }
    </script>
</body>
</html>
