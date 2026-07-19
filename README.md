# Testimony-tutor
A Tutorial management system 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    <title>Testimony Tutors - Complete Management System</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js">
    </script>
    <script src="https://unpkg.com/@zxing/library@0.19.1/umd/index.min.js">
    </script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        :root {
            --red: #D62828;
            --blue: #2563EB;
            --gold: #F5A623;
            --dark: #1a1a2e;
            --surface: #f8f9fc;
            --card: #ffffff;
            --text: #1e293b;
            --muted: #64748b;
            --border: #e2e8f0;
            --success: #059669;
            --warning: #D97706;
            --danger: #DC2626;
            --alert-bg: #FEF3C7;
            --alert-border: #F59E0B;
        }
        body {
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            background: var(--surface);
            color: var(--text);
            min-height: 100vh;
            padding-bottom: env(safe-area-inset-bottom);
        }
        @keyframes scanAnim {
            0% {
                top: 0;
            }
            50% {
                top: calc(100% - 3px);
            }
            100% {
                top: 0;
            }
        }
        @keyframes pulse {
            0%,
            100% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.04);
            }
        }
        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: translateY(8px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        .login-container {
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
            padding: 20px;
        }
        .login-box {
            background: white;
            border-radius: 24px;
            box-shadow: 0 25px 80px rgba(0, 0, 0, 0.4);
            width: 100%;
            max-width: 460px;
            padding: 48px 40px;
        }
        .login-logo .brand {
            font-family: 'Arial Black', sans-serif;
            font-size: 38px;
            font-weight: 900;
            color: var(--red);
            text-transform: uppercase;
            letter-spacing: 3px;
            line-height: 1;
        }
        .login-logo .brand span {
            display: block;
            font-size: 18px;
            color: var(--blue);
            letter-spacing: 10px;
            margin-top: 2px;
        }
        .brand-bar {
            width: 80px;
            height: 4px;
            background: linear-gradient(90deg, var(--red), var(--blue));
            margin: 14px auto 0;
            border-radius: 4px;
        }
        .login-tabs {
            display: flex;
            border-bottom: 2px solid var(--border);
            margin-bottom: 28px;
        }
        .login-tab {
            flex: 1;
            text-align: center;
            padding: 12px;
            cursor: pointer;
            color: var(--muted);
            font-weight: 600;
            font-size: 15px;
            transition: all 0.25s;
        }
        .login-tab.active {
            color: var(--red);
            border-bottom: 3px solid var(--red);
            margin-bottom: -2px;
        }
        .login-form {
            display: none;
        }
        .login-form.active {
            display: block;
        }
        .form-group {
            margin-bottom: 20px;
        }
        .form-group label {
            display: block;
            margin-bottom: 6px;
            color: var(--text);
            font-weight: 600;
            font-size: 14px;
        }
        .form-group input,
        .form-group select,
        .form-group textarea {
            width: 100%;
            padding: 13px 16px;
            border: 1.5px solid var(--border);
            border-radius: 12px;
            font-size: 15px;
            transition: border-color 0.2s;
            background: var(--surface);
            color: var(--text);
            font-family: inherit;
        }
        .form-group input:focus,
        .form-group select:focus,
        .form-group textarea:focus {
            outline: none;
            border-color: var(--red);
            background: white;
        }
        .login-btn {
            width: 100%;
            padding: 15px;
            background: linear-gradient(135deg, var(--red), var(--blue));
            color: white;
            border: none;
            border-radius: 12px;
            font-size: 16px;
            font-weight: 700;
            cursor: pointer;
            transition: all 0.3s;
            letter-spacing: 0.5px;
        }
        .login-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(214, 40, 40, 0.35);
        }
        .dashboard {
            display: none;
            min-height: 100vh;
        }
        .dashboard.active {
            display: block;
        }
        .sidebar {
            position: fixed;
            left: 0;
            top: 0;
            bottom: 0;
            width: 280px;
            background: linear-gradient(180deg, #1a1a2e 0%, #16213e 100%);
            color: white;
            overflow-y: auto;
            z-index: 100;
            transform: translateX(-100%);
            transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            padding-bottom: 20px;
        }
        .sidebar.open {
            transform: translateX(0);
        }
        .sidebar-header {
            padding: 28px 20px 20px;
            text-align: center;
            border-bottom: 1px solid rgba(255, 255, 255, 0.08);
        }
        .sidebar-logo .brand {
            font-family: 'Arial Black', sans-serif;
            font-size: 26px;
            font-weight: 900;
            color: var(--red);
            text-transform: uppercase;
            letter-spacing: 2px;
        }
        .sidebar-logo .brand span {
            display: block;
            font-size: 13px;
            color: var(--gold);
            letter-spacing: 7px;
        }
        .sidebar-header p {
            font-size: 12px;
            color: rgba(255, 255, 255, 0.5);
            margin-top: 8px;
        }
        .sidebar-menu {
            padding: 16px 0;
        }
        .menu-section {
            padding: 12px 20px 6px;
            font-size: 10px;
            text-transform: uppercase;
            letter-spacing: 2px;
            color: rgba(255, 255, 255, 0.3);
            font-weight: 700;
        }
        .menu-item {
            padding: 13px 22px;
            display: flex;
            align-items: center;
            color: rgba(255, 255, 255, 0.7);
            cursor: pointer;
            transition: all 0.25s;
            border-left: 3px solid transparent;
            font-size: 14px;
            font-weight: 500;
        }
        .menu-item:hover,
        .menu-item.active {
            background: rgba(255, 255, 255, 0.07);
            color: white;
            border-left-color: var(--red);
        }
        .menu-item .icon {
            margin-right: 14px;
            font-size: 18px;
            width: 22px;
            text-align: center;
        }
        .menu-item .badge {
            margin-left: auto;
            background: var(--red);
            color: white;
            font-size: 11px;
            font-weight: 700;
            padding: 2px 8px;
            border-radius: 20px;
        }
        .main-content {
            margin-left: 0;
            padding: 16px;
            min-height: 100vh;
        }
        .top-bar {
            background: white;
            padding: 14px 18px;
            border-radius: 16px;
            margin-bottom: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
            position: sticky;
            top: 0;
            z-index: 50;
        }
        .top-bar-left {
            display: flex;
            align-items: center;
            gap: 14px;
        }
        .hamburger {
            font-size: 28px;
            cursor: pointer;
            background: none;
            border: none;
            color: var(--text);
            padding: 4px 8px;
            border-radius: 8px;
            transition: background 0.2s;
            line-height: 1;
        }
        .hamburger:hover {
            background: var(--surface);
        }
        .hamburger:active {
            background: var(--border);
        }
        .welcome-text h3 {
            color: var(--text);
            font-size: 17px;
            font-weight: 700;
        }
        .welcome-text p {
            color: var(--muted);
            font-size: 12px;
            margin-top: 2px;
        }
        .top-right {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .top-alert-badge {
            background: var(--alert-bg);
            border: 1px solid var(--alert-border);
            color: var(--warning);
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            cursor: pointer;
            display: none;
            white-space: nowrap;
        }
        .top-alert-badge.visible {
            display: flex;
            align-items: center;
            gap: 4px;
        }
        .user-avatar {
            width: 38px;
            height: 38px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--red), var(--blue));
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 14px;
            font-weight: 700;
            flex-shrink: 0;
        }
        .logout-btn {
            padding: 6px 14px;
            background: #fee2e2;
            color: var(--red);
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            font-size: 12px;
            transition: all 0.2s;
        }
        .logout-btn:hover {
            background: var(--red);
            color: white;
        }
        .sidebar-overlay {
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.4);
            z-index: 90;
            display: none;
            backdrop-filter: blur(2px);
        }
        .sidebar-overlay.active {
            display: block;
        }
        .content-section {
            display: none;
            animation: fadeIn 0.25s ease;
        }
        .content-section.active {
            display: block;
        }
        .section-card {
            background: white;
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
        }
        .section-header {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            flex-wrap: wrap;
            gap: 12px;
            margin-bottom: 18px;
        }
        .section-header h2 {
            color: var(--text);
            font-size: 19px;
            font-weight: 700;
        }
        .section-header .actions {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
        }
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
            gap: 14px;
            margin-bottom: 20px;
        }
        .stat-card {
            background: white;
            padding: 18px;
            border-radius: 14px;
            box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
            cursor: pointer;
            transition: all 0.3s;
            border: 1.5px solid var(--border);
        }
        .stat-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 24px rgba(214, 40, 40, 0.12);
            border-color: var(--red);
        }
        .stat-card .label {
            color: var(--muted);
            font-size: 11px;
            font-weight: 600;
            margin-bottom: 6px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }
        .stat-card .number {
            font-size: 28px;
            font-weight: 800;
            color: var(--red);
        }
        .stat-card .sub {
            font-size: 11px;
            color: var(--muted);
            margin-top: 3px;
        }
        .alert-panel {
            background: var(--alert-bg);
            border: 1.5px solid var(--alert-border);
            border-radius: 12px;
            padding: 14px 18px;
            margin-bottom: 16px;
        }
        .alert-panel h4 {
            color: var(--warning);
            font-size: 14px;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .alert-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 14px;
            background: white;
            border-radius: 8px;
            margin-bottom: 6px;
            border-left: 4px solid var(--warning);
            flex-wrap: wrap;
            gap: 8px;
        }
        .alert-item.critical {
            border-left-color: var(--danger);
        }
        .alert-item .ai {
            font-weight: 600;
            font-size: 13px;
        }
        .alert-item .ad {
            font-size: 11px;
            color: var(--muted);
        }
        .alert-item .ab {
            padding: 4px 12px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 11px;
            font-weight: 600;
        }
        .btn-warn {
            background: var(--warning);
            color: white;
        }
        .btn-danger {
            background: var(--danger);
            color: white;
        }
        .table-responsive {
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
            margin: 0 -4px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 13px;
            min-width: 600px;
        }
        th {
            padding: 10px 12px;
            text-align: left;
            background: var(--surface);
            font-weight: 700;
            color: var(--red);
            font-size: 11px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            white-space: nowrap;
        }
        td {
            padding: 10px 12px;
            border-bottom: 1px solid var(--border);
            font-size: 13px;
            vertical-align: middle;
        }
        tr:hover td {
            background: #fafbff;
        }
        .badge {
            padding: 3px 10px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 700;
            display: inline-block;
            white-space: nowrap;
        }
        .badge-green {
            background: #d1fae5;
            color: #059669;
        }
        .badge-red {
            background: #fee2e2;
            color: var(--danger);
        }
        .badge-yellow {
            background: #fef3c7;
            color: var(--warning);
        }
        .badge-blue {
            background: #dbeafe;
            color: var(--blue);
        }
        .badge-gray {
            background: #f1f5f9;
            color: var(--muted);
        }
        .action-btns {
            display: flex;
            gap: 4px;
            flex-wrap: wrap;
        }
        .action-btn {
            padding: 4px 10px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 11px;
            font-weight: 600;
            transition: all 0.2s;
            white-space: nowrap;
        }
        .edit-btn {
            background: #dbeafe;
            color: var(--blue);
        }
        .view-btn {
            background: #fce7f3;
            color: #be185d;
        }
        .delete-btn {
            background: #fee2e2;
            color: var(--danger);
        }
        .id-btn {
            background: #fef3c7;
            color: var(--warning);
        }
        .pay-btn {
            background: #d1fae5;
            color: var(--success);
        }
        .attend-btn {
            background: #ede9fe;
            color: #7c3aed;
        }
        .btn {
            padding: 9px 18px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-weight: 600;
            font-size: 13px;
            transition: all 0.25s;
            white-space: nowrap;
        }
        .btn-primary {
            background: linear-gradient(135deg, var(--red), var(--blue));
            color: white;
        }
        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 18px rgba(214, 40, 40, 0.3);
        }
        .btn-success {
            background: var(--success);
            color: white;
        }
        .btn-success:hover {
            background: #047857;
        }
        .btn-outline {
            background: transparent;
            border: 2px solid var(--red);
            color: var(--red);
        }
        .btn-outline:hover {
            background: var(--red);
            color: white;
        }
        .btn-warning {
            background: var(--warning);
            color: white;
        }
        .search-bar {
            display: flex;
            gap: 8px;
            margin-bottom: 16px;
            flex-wrap: wrap;
        }
        .search-bar input,
        .search-bar select {
            padding: 9px 14px;
            border: 1.5px solid var(--border);
            border-radius: 10px;
            font-size: 13px;
            transition: border-color 0.2s;
            flex: 1;
            min-width: 140px;
            background: white;
        }
        .search-bar input:focus,
        .search-bar select:focus {
            outline: none;
            border-color: var(--red);
        }
        .form-row {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 14px;
        }
        .form-full {
            grid-column: 1/-1;
        }
        .photo-upload-wrap {
            display: flex;
            align-items: center;
            gap: 20px;
            flex-wrap: wrap;
        }
        .photo-preview {
            width: 90px;
            height: 90px;
            border-radius: 50%;
            background: var(--surface);
            border: 3px solid var(--gold);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 36px;
            font-weight: 800;
            color: var(--muted);
            overflow: hidden;
            flex-shrink: 0;
        }
        .photo-preview img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        .photo-upload-btn {
            padding: 8px 16px;
            background: var(--blue);
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 13px;
            font-weight: 600;
        }
        .photo-upload-btn:hover {
            background: #1d4ed8;
        }
        .photo-upload-input {
            display: none;
        }
        .modal {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.55);
            z-index: 200;
            align-items: center;
            justify-content: center;
            padding: 16px;
        }
        .modal.active {
            display: flex;
        }
        .modal-content {
            background: white;
            border-radius: 20px;
            padding: 24px;
            width: 100%;
            max-width: 520px;
            max-height: 92vh;
            overflow-y: auto;
            animation: fadeIn 0.25s ease;
        }
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 18px;
        }
        .modal-header h3 {
            color: var(--text);
            font-size: 18px;
            font-weight: 700;
        }
        .close-modal {
            font-size: 26px;
            cursor: pointer;
            color: var(--muted);
            line-height: 1;
            padding: 0 4px;
        }
        .close-modal:hover {
            color: var(--danger);
        }
        .grid-3 {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 16px;
        }
        .branch-card {
            background: var(--surface);
            padding: 20px;
            border-radius: 14px;
            border: 1.5px solid var(--border);
        }
        .branch-card h3 {
            color: var(--red);
            font-size: 17px;
            margin-bottom: 10px;
        }
        .branch-card p {
            color: var(--muted);
            font-size: 13px;
            line-height: 1.6;
            margin-bottom: 6px;
        }
        .branch-stats {
            display: flex;
            justify-content: space-between;
            padding-top: 12px;
            border-top: 1px solid var(--border);
            margin-top: 10px;
            font-size: 13px;
        }
        .id-card-wrap {
            display: flex;
            justify-content: center;
            margin: 16px 0;
        }
        .id-card {
            width: 320px;
            background: white;
            border-radius: 16px;
            box-shadow: 0 12px 40px rgba(0, 0, 0, 0.18);
            overflow: hidden;
            border: 2px solid #e5e7eb;
            position: relative;
        }
        .id-card-top {
            height: 10px;
            background: linear-gradient(90deg, var(--red) 0%, var(--blue) 100%);
        }
        .id-card-body {
            padding: 16px;
        }
        .id-card-header {
            text-align: center;
            margin-bottom: 10px;
            padding-bottom: 8px;
            border-bottom: 2px dashed var(--red);
        }
        .id-card-header h2 {
            color: var(--red);
            font-size: 18px;
            font-weight: 900;
            letter-spacing: 1px;
        }
        .id-card-header .slogan {
            font-size: 13px;
            color: var(--blue);
            font-weight: 700;
            font-style: italic;
            margin-top: 2px;
            letter-spacing: 1px;
        }
        .id-card-header .exam-type-badge {
            display: inline-block;
            margin-top: 4px;
            padding: 2px 14px;
            background: var(--gold);
            color: white;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 700;
            letter-spacing: 1px;
        }
        .id-card-photo {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            margin: 0 auto 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 32px;
            font-weight: 800;
            border: 3px solid var(--gold);
            overflow: hidden;
            background: linear-gradient(135deg, var(--red), var(--blue));
            color: white;
        }
        .id-card-photo img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        .id-card-detail {
            display: flex;
            margin-bottom: 4px;
            font-size: 12px;
        }
        .id-card-lbl {
            color: var(--blue);
            font-weight: 700;
            width: 80px;
            flex-shrink: 0;
        }
        .id-card-val {
            color: var(--text);
        }
        .id-card-qr-section {
            background: linear-gradient(135deg, #1a1a2e, #16213e);
            padding: 12px 14px 10px;
            margin-top: 12px;
            border-radius: 0 0 14px 14px;
            text-align: center;
        }
        .id-card-qr-section canvas,
        .id-card-qr-section img {
            display: inline-block;
            background: white;
            padding: 6px;
            border-radius: 8px;
            max-width: 100%;
            height: auto;
        }
        .qr-id-text {
            color: rgba(255, 255, 255, 0.7);
            font-size: 9px;
            text-align: center;
            margin-top: 4px;
            letter-spacing: 2px;
            font-family: 'Courier New', monospace;
        }
        .attendance-scanner {
            background: linear-gradient(135deg, #1a1a2e, #16213e);
            border-radius: 16px;
            padding: 24px;
            text-align: center;
            margin-bottom: 20px;
            color: white;
        }
        .scanner-icon {
            font-size: 44px;
            margin-bottom: 8px;
            animation: pulse 2s infinite;
        }
        .attendance-scanner h3 {
            font-size: 18px;
            margin-bottom: 4px;
        }
        .attendance-scanner p {
            color: rgba(255, 255, 255, 0.6);
            font-size: 13px;
            margin-bottom: 16px;
        }
        .scanner-input-row {
            display: flex;
            gap: 8px;
            justify-content: center;
            flex-wrap: wrap;
        }
        .scanner-input {
            padding: 11px 16px;
            border: 2px solid rgba(255, 255, 255, 0.2);
            border-radius: 10px;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            font-size: 14px;
            font-family: 'Courier New', monospace;
            letter-spacing: 2px;
            width: 220px;
            text-align: center;
            outline: none;
            transition: border-color 0.2s;
        }
        .scanner-input::placeholder {
            color: rgba(255, 255, 255, 0.35);
        }
        .scanner-input:focus {
            border-color: var(--gold);
        }
        .scan-btn {
            padding: 11px 22px;
            background: linear-gradient(135deg, var(--red), var(--gold));
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 14px;
            font-weight: 700;
            cursor: pointer;
            transition: all 0.25s;
        }
        .scan-btn:hover {
            transform: scale(1.04);
        }
        .scan-result {
            margin-top: 14px;
            padding: 14px 20px;
            border-radius: 10px;
            font-weight: 700;
            font-size: 14px;
            display: none;
        }
        .scan-result.success {
            background: rgba(5, 150, 105, 0.2);
            border: 1.5px solid #059669;
            color: #6ee7b7;
        }
        .scan-result.error {
            background: rgba(220, 38, 38, 0.2);
            border: 1.5px solid #DC2626;
            color: #fca5a5;
        }
        .attendance-tabs {
            display: flex;
            gap: 8px;
            margin-bottom: 14px;
            flex-wrap: wrap;
        }
        .att-tab {
            padding: 8px 16px;
            border: 2px solid var(--border);
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            font-size: 13px;
            transition: all 0.2s;
        }
        .att-tab.active {
            background: var(--red);
            color: white;
            border-color: var(--red);
        }
        .payment-plan-cards {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
            margin-bottom: 18px;
        }
        .plan-card {
            padding: 16px;
            border-radius: 12px;
            border: 2px solid var(--border);
            text-align: center;
            transition: all 0.2s;
            cursor: pointer;
        }
        .plan-card.full {
            border-color: var(--success);
            background: #f0fdf4;
        }
        .plan-card.half {
            border-color: var(--blue);
            background: #eff6ff;
        }
        .plan-card h4 {
            font-size: 14px;
            font-weight: 700;
            margin-bottom: 4px;
        }
        .plan-card .plan-price {
            font-size: 24px;
            font-weight: 800;
            margin-bottom: 2px;
        }
        .plan-card .plan-desc {
            font-size: 12px;
            color: var(--muted);
        }
        .plan-card.full h4 {
            color: var(--success);
        }
        .plan-card.full .plan-price {
            color: var(--success);
        }
        .plan-card.half h4 {
            color: var(--blue);
        }
        .plan-card.half .plan-price {
            color: var(--blue);
        }
        .expiry-row {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 10px 14px;
            border-radius: 8px;
            margin-bottom: 6px;
            flex-wrap: wrap;
        }
        .expiry-row.expired {
            background: #fee2e2;
            border: 1px solid #fca5a5;
        }
        .expiry-row.expiring {
            background: #fef3c7;
            border: 1px solid var(--alert-border);
        }
        .expiry-row.good {
            background: #f0fdf4;
            border: 1px solid #86efac;
        }
        .expiry-days {
            font-size: 20px;
            font-weight: 800;
            min-width: 40px;
            text-align: center;
        }
        .expiry-info {
            flex: 1;
        }
        .expiry-info .name {
            font-weight: 700;
            font-size: 13px;
        }
        .expiry-info .detail {
            font-size: 11px;
            color: var(--muted);
        }
        .cbt-option {
            padding: 12px 16px;
            margin: 8px 0;
            background: var(--surface);
            border: 2px solid var(--border);
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .cbt-option:hover {
            background: #fff;
            border-color: var(--blue);
        }
        .cbt-option.selected {
            background: #eff6ff;
            border-color: var(--blue);
        }
        .cbt-option.correct {
            background: #f0fdf4;
            border-color: var(--success);
        }
        .cbt-option.wrong {
            background: #fee2e2;
            border-color: var(--danger);
        }
        .cbt-timer {
            font-size: 18px;
            font-weight: 800;
            color: var(--red);
            text-align: right;
            margin-bottom: 10px;
        }
        .cbt-progress {
            font-size: 14px;
            color: var(--muted);
            font-weight: 600;
            margin-bottom: 14px;
            text-align: center;
        }
        .subjects-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 12px;
        }
        .subject-tag {
            background: var(--surface);
            padding: 14px;
            border-radius: 12px;
            border: 1.5px solid var(--border);
            text-align: center;
            cursor: pointer;
            transition: all 0.25s;
        }
        .subject-tag:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(214, 40, 40, 0.1);
            border-color: var(--red);
        }
        .subject-tag h4 {
            color: var(--red);
            font-size: 14px;
            margin-bottom: 4px;
        }
        .subject-tag p {
            color: var(--muted);
            font-size: 11px;
        }
        .tag-pill {
            display: inline-block;
            padding: 2px 8px;
            border-radius: 20px;
            font-size: 10px;
            font-weight: 700;
            margin: 2px;
            background: #dbeafe;
            color: var(--blue);
        }
        .empty-state {
            text-align: center;
            padding: 30px 16px;
            color: var(--muted);
        }
        .empty-state .icon {
            font-size: 40px;
            margin-bottom: 10px;
        }
        .note {
            background: #eff6ff;
            border-left: 4px solid var(--blue);
            padding: 10px 14px;
            border-radius: 0 8px 8px 0;
            font-size: 12px;
            color: var(--blue);
            margin-bottom: 16px;
        }
        .divider {
            border: none;
            border-top: 1px solid var(--border);
            margin: 16px 0;
        }
        .quick-picker-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 6px;
            max-height: 180px;
            overflow-y: auto;
            padding: 4px 0;
        }
        .quick-picker-grid button {
            padding: 6px 12px;
            border: none;
            border-radius: 8px;
            font-size: 12px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
            text-align: left;
            line-height: 1.3;
        }
        .quick-picker-grid button:active {
            transform: scale(0.95);
        }
        @media print {
            body * {
                visibility: hidden;
            }
            #printArea,
            #printArea * {
                visibility: visible;
            }
            #printArea {
                position: fixed;
                top: 0;
                left: 0;
                width: 100%;
            }
        }
        @media (min-width: 768px) {
            .sidebar {
                transform: translateX(0) !important;
            }
            .sidebar-overlay {
                display: none !important;
            }
            .main-content {
                margin-left: 280px;
                padding: 24px;
            }
            .hamburger {
                display: none;
            }
        }
        @media (max-width: 600px) {
            .login-box {
                padding: 32px 20px;
            }
            .login-logo .brand {
                font-size: 30px;
            }
            .login-logo .brand span {
                font-size: 14px;
                letter-spacing: 6px;
            }
            .form-row {
                grid-template-columns: 1fr;
            }
            .stats-grid {
                grid-template-columns: 1fr 1fr;
            }
            .payment-plan-cards {
                grid-template-columns: 1fr;
            }
            .section-card {
                padding: 14px;
            }
            .top-bar {
                padding: 10px 14px;
            }
            .welcome-text h3 {
                font-size: 15px;
            }
            .top-alert-badge {
                font-size: 10px;
                padding: 3px 8px;
            }
            .user-avatar {
                width: 32px;
                height: 32px;
                font-size: 12px;
            }
            .logout-btn {
                padding: 4px 10px;
                font-size: 11px;
            }
            .photo-upload-wrap {
                flex-direction: column;
                align-items: center;
            }
            .id-card {
                width: 280px;
            }
            .modal-content {
                padding: 16px;
            }
        }
        @media (max-width: 400px) {
            .stats-grid {
                grid-template-columns: 1fr 1fr;
                gap: 10px;
            }
            .stat-card {
                padding: 12px;
            }
            .stat-card .number {
                font-size: 22px;
            }
            .section-header h2 {
                font-size: 16px;
            }
            .btn {
                padding: 7px 14px;
                font-size: 12px;
            }
        }
        .sidebar::-webkit-scrollbar {
            width: 4px;
        }
        .sidebar::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.05);
        }
        .sidebar::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.2);
            border-radius: 4px;
        }
    </style>
</head>
<body>

    <!-- ========== LOGIN PAGE ========== -->
    <div id="loginContainer" class="login-container">
        <div class="login-box">
            <div class="login-logo">
                <div class="brand">TESTIMONY<span>TUTORS</span></div>
                <div class="brand-bar"></div>
            </div>
            <div class="login-tabs">
                <div class="login-tab active" onclick="switchTab('student')">🎓 Student</div>
                <div class="login-tab" onclick="switchTab('admin')">🔐 Admin</div>
            </div>
            <div id="studentLogin" class="login-form active">
                <form onsubmit="handleStudentLogin(event)">
                    <div class="form-group">
                        <label>Registration Number</label>
                        <input type="text" id="studentRegNo" placeholder="e.g. TT27001" required>
                    </div>
                    <div class="form-group">
                        <label>Password</label>
                        <input type="password" id="studentPassword" placeholder="Enter your password" required>
                    </div>
                    <button type="submit" class="login-btn">Login to Student Portal</button>
                </form>
            </div>
            <div id="adminLogin" class="login-form">
                <form onsubmit="handleAdminLogin(event)">
                    <div class="form-group">
                        <label>Admin ID</label>
                        <input type="text" id="adminId" placeholder="Enter admin ID" required>
                    </div>
                    <div class="form-group">
                        <label>Password</label>
                        <input type="password" id="adminPassword" placeholder="Enter password" required>
                    </div>
                    <button type="submit" class="login-btn">Login to Admin Dashboard</button>
                </form>
            </div>
        </div>
    </div>

    <!-- ========== ADMIN DASHBOARD ========== -->
    <div id="adminDashboard" class="dashboard">
        <div class="sidebar-overlay" id="adminSidebarOverlay" onclick="toggleSidebar('admin')"></div>
        <div class="sidebar" id="adminSidebar">
            <div class="sidebar-header">
                <div class="sidebar-logo">
                    <div class="brand">TESTIMONY<span>TUTORS</span></div>
                </div>
                <p>Admin Dashboard</p>
            </div>
            <div class="sidebar-menu">
                <div class="menu-section">Overview</div>
                <div class="menu-item active" onclick="showAdmin('dashboard')"><span class="icon">📊</span> Dashboard</div>
                <div class="menu-section">Students</div>
                <div class="menu-item" onclick="showAdmin('students')"><span class="icon">👥</span> Manage Students</div>
                <div class="menu-item" onclick="showAdmin('addStudent')"><span class="icon">➕</span> Add New Student</div>
                <div class="menu-section">Monitoring</div>
                <div class="menu-item" onclick="showAdmin('attendance')"><span class="icon">📋</span> Attendance <span class="badge" id="attendanceBadge">0</span></div>
                <div class="menu-item" onclick="showAdmin('payments')"><span class="icon">💰</span> Payments <span class="badge" id="paymentAlertBadge" style="background:var(--warning)">0</span></div>
                <div class="menu-section">Academics</div>
                <div class="menu-item" onclick="showAdmin('subjects')"><span class="icon">📚</span> Subjects</div>
                <div class="menu-item" onclick="showAdmin('cbt')"><span class="icon">💻</span> CBT Questions</div>
                <div class="menu-item" onclick="showAdmin('tests')"><span class="icon">📝</span> Tests & Exams</div>
                <div class="menu-section">Settings</div>
                <div class="menu-item" onclick="showAdmin('branches')"><span class="icon">🏢</span> Branches</div>
                <div class="menu-item" onclick="showAdmin('paymentSettings')"><span class="icon">💰</span> Payment Plans</div>
            </div>
        </div>

        <div class="main-content">
            <div class="top-bar">
                <div class="top-bar-left">
                    <button class="hamburger" onclick="toggleSidebar('admin')" aria-label="Toggle menu">☰</button>
                    <div class="welcome-text">
                        <h3>Welcome, Admin</h3>
                        <p id="adminDateLine">Loading date...</p>
                    </div>
                </div>
                <div class="top-right">
                    <div class="top-alert-badge" id="topAlertBadge" onclick="showAdmin('payments')">
                        ⚠️ <span id="topAlertCount">0</span>
                    </div>
                    <div class="user-avatar">AD</div>
                    <button class="logout-btn" onclick="logout()">Logout</button>
                </div>
            </div>

            <div id="adminDashboardView" class="content-section active">
                <div class="stats-grid" id="adminStatsGrid"></div>
                <div class="section-card" id="paymentAlertsCard" style="display:none">
                    <div class="alert-panel" id="paymentAlertsPanel"></div>
                </div>
                <div class="section-card">
                    <div class="section-header">
                        <h2>Recent Students</h2>
                        <button class="btn btn-primary" onclick="showAdmin('addStudent')">+ Add Student</button>
                    </div>
                    <div class="table-responsive">
                        <table>
                            <thead><tr><th>Reg No.</th><th>Name</th><th>Dept</th><th>Branch</th><th>Payment</th><th>Status</th><th>Actions</th></tr></thead>
                            <tbody id="recentStudentsTable"></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <div id="adminStudentsView" class="content-section">
                <div class="section-card">
                    <div class="section-header">
                        <h2>Manage Students</h2>
                        <button class="btn btn-primary" onclick="showAdmin('addStudent')">+ Add Student</button>
                    </div>
                    <div class="search-bar">
                        <input type="text" id="searchStudent" placeholder="Search by name, reg no, branch..." onkeyup="searchStudents()">
                        <select onchange="filterStudentsByBranch(this.value)">
                            <option value="">All Branches</option>
                            <option value="Iyana Agbala">Iyana Agbala</option>
                            <option value="Omi Bridge">Omi Bridge</option>
                            <option value="Ajameta">Ajameta</option>
                        </select>
                    </div>
                    <div class="table-responsive">
                        <table>
                            <thead><tr><th>Reg No.</th><th>Name</th><th>Dept</th><th>Branch</th><th>Payment</th><th>Expiry</th><th>Status</th><th>Actions</th></tr></thead>
                            <tbody id="studentsTableBody"></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <div id="adminAddStudentView" class="content-section">
                <div class="section-card">
                    <div class="section-header"><h2>Register New Student</h2></div>
                    <div class="note">📌 Subjects auto-assigned by department. Login username = reg no, password = surname. Upload passport photo.</div>
                    <form id="addStudentForm" onsubmit="addStudent(event)">
                        <div class="form-group">
                            <label>Passport Photo</label>
                            <div class="photo-upload-wrap">
                                <div class="photo-preview" id="photoPreview">📷</div>
                                <div>
                                    <button type="button" class="photo-upload-btn" onclick="document.getElementById('photoInput').click()">Upload Photo</button>
                                    <input type="file" id="photoInput" class="photo-upload-input" accept="image/*" onchange="previewPhoto(event)">
                                    <p style="font-size:11px;color:var(--muted);margin-top:4px">Max 2MB, JPG or PNG</p>
                                </div>
                            </div>
                        </div>
                        <div class="form-row">
                            <div class="form-group"><label>First Name *</label><input type="text" id="firstName" required></div>
                            <div class="form-group"><label>Last Name *</label><input type="text" id="lastName" required></div>
                        </div>
                        <div class="form-row">
                            <div class="form-group"><label>Date of Birth *</label><input type="date" id="dob" required></div>
                            <div class="form-group"><label>Gender *</label>
                                <select id="gender" required><option value="">Select</option><option>Male</option><option>Female</option></select>
                            </div>
                        </div>
                        <div class="form-row">
                            <div class="form-group"><label>Email *</label><input type="email" id="email" required></div>
                            <div class="form-group"><label>Phone *</label><input type="tel" id="phone" required></div>
                        </div>
                        <div class="form-group"><label>Address *</label><textarea id="address" rows="2" required></textarea></div>
                        <div class="form-row">
                            <div class="form-group"><label>Branch *</label>
                                <select id="branch" required><option value="">Select Branch</option>
                                    <option>Iyana Agbala</option><option>Omi Bridge</option><option>Ajameta</option>
                                </select>
                            </div>
                            <div class="form-group"><label>Department *</label>
                                <select id="department" required><option value="">Select Dept</option>
                                    <option>HUMANITIES</option><option>BUSINESS</option><option>SCIENCES</option>
                                </select>
                            </div>
                        </div>
                        <div class="form-group">
                            <label>Exam Type *</label>
                            <select id="examType" required>
                                <option value="">Select Exam Type</option>
                                <option value="NECO">NECO</option>
                                <option value="WAEC">WAEC</option>
                                <option value="JAMB">JAMB</option>
                            </select>
                        </div>
                        <div class="form-row">
                            <div class="form-group"><label>Registration Date *</label><input type="date" id="regDate" required></div>
                            <div class="form-group"><label>Payment Plan *</label>
                                <select id="paymentPlan" required onchange="updatePaymentFields()">
                                    <option value="">Select Plan</option>
                                    <option value="full">Full Payment — ₦15,000 / 30 days</option>
                                    <option value="half">Half Payment — ₦8,000 / 14 days</option>
                                </select>
                            </div>
                        </div>
                        <div class="form-row">
                            <div class="form-group"><label>Amount Paid (₦)</label><input type="number" id="amountPaid" value="0" readonly></div>
                            <div class="form-group"><label>Payment Date *</label><input type="date" id="paymentDate" required></div>
                        </div>
                        <div id="paymentSummary" style="margin-bottom:14px;"></div>
                        <div class="form-row">
                            <div class="form-group"><label>Guardian Name *</label><input type="text" id="guardianName" required></div>
                            <div class="form-group"><label>Guardian Phone *</label><input type="tel" id="guardianPhone" required></div>
                        </div>
                        <div style="background:#f0fdf4;border-radius:10px;padding:12px 16px;margin-bottom:14px;border:1px solid #86efac;">
                            <p style="font-size:13px;color:#065f46;font-weight:600;">🔐 Auto Login Credentials</p>
                            <p style="font-size:12px;color:var(--muted);">Username: <strong id="autoUsername">[will be reg no]</strong> &nbsp;|&nbsp; Password: <strong id="autoPassword">[surname]</strong></p>
                            <p style="font-size:11px;color:var(--muted);margin-top:4px;">Students can change their password after login.</p>
                        </div>
                        <button type="submit" class="btn btn-primary" style="width:100%;padding:14px;font-size:15px">Register Student</button>
                    </form>
                </div>
            </div>

            <div id="adminAttendanceView" class="content-section">
                <div class="attendance-scanner">
                    <div class="scanner-icon">📸</div>
                    <h3>QR Code Attendance Scanner</h3>
                    <p>Point camera at student ID card QR code, or type reg no.</p>
                    <div id="scannerViewport" style="display:none;position:relative;max-width:440px;margin:0 auto 14px;border-radius:14px;overflow:hidden;border:3px solid var(--gold);">
                        <video id="scannerVideo" style="width:100%;display:block;border-radius:12px;" autoplay muted playsinline></video>
                        <div style="position:absolute;inset:0;pointer-events:none;">
                            <div style="position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);width:70%;height:50px;border:3px solid var(--gold);border-radius:6px;box-shadow:0 0 0 9999px rgba(0,0,0,0.4);">
                                <div id="scanLine" style="position:absolute;top:0;left:0;right:0;height:3px;background:linear-gradient(90deg,transparent,var(--gold),transparent);animation:scanAnim 1.5s linear infinite;"></div>
                            </div>
                            <p style="position:absolute;bottom:10px;left:0;right:0;text-align:center;color:white;font-size:12px;font-weight:600;text-shadow:0 1px 4px rgba(0,0,0,0.8);">Align QR code within gold frame</p>
                        </div>
                    </div>
                    <div style="display:flex;gap:8px;justify-content:center;flex-wrap:wrap;margin-bottom:12px;">
                        <button id="startScanBtn" class="scan-btn" onclick="startCameraScanner()">📷 Start Camera</button>
                        <button id="stopScanBtn" class="scan-btn" onclick="stopCameraScanner()" style="display:none;background:linear-gradient(135deg,#555,#333);">⛔ Stop</button>
                        <select id="cameraSelect" style="padding:8px 12px;border-radius:8px;border:none;background:rgba(255,255,255,0.12);color:white;font-size:12px;cursor:pointer;display:none;" onchange="switchCamera()">
                            <option value="">Select Camera</option>
                        </select>
                    </div>
                    <div style="display:flex;align-items:center;gap:10px;margin:10px auto;max-width:440px;">
                        <div style="flex:1;height:1px;background:rgba(255,255,255,0.15);"></div>
                        <span style="color:rgba(255,255,255,0.4);font-size:12px;">or type manually</span>
                        <div style="flex:1;height:1px;background:rgba(255,255,255,0.15);"></div>
                    </div>
                    <div class="scanner-input-row">
                        <input type="text" class="scanner-input" id="barcodeInput" placeholder="Type Reg No. then Enter" autocomplete="off" onkeydown="if(event.key==='Enter') processBarcode()" oninput="filterAttendanceDropdown()">
                        <button class="scan-btn" onclick="processBarcode()">⚡ Clock In/Out</button>
                    </div>
                    <div class="scan-result" id="scanResult"></div>
                    <div style="margin-top:16px;">
                        <p style="font-size:12px;color:rgba(255,255,255,0.4);margin-bottom:8px;">— Quick tap to clock in/out —</p>
                        <div class="quick-picker-grid" id="quickStudentPicker"></div>
                    </div>
                </div>

                <div class="section-card">
                    <div class="section-header">
                        <h2>Attendance Records</h2>
                        <div class="actions">
                            <div class="attendance-tabs">
                                <div class="att-tab active" onclick="filterAttendance('all',this)">All</div>
                                <div class="att-tab" onclick="filterAttendance('clocked-in',this)">Present</div>
                                <div class="att-tab" onclick="filterAttendance('clocked-out',this)">Completed</div>
                            </div>
                            <button class="btn btn-outline" onclick="exportAttendance()">📥 Export CSV</button>
                        </div>
                    </div>
                    <div class="search-bar">
                        <input type="text" id="searchAttendance" placeholder="Search by name or reg no..." onkeyup="renderAttendanceTable()">
                        <input type="date" id="attendanceDate" onchange="renderAttendanceTable()">
                    </div>
                    <div class="table-responsive">
                        <table>
                            <thead><tr><th>Date</th><th>Reg No.</th><th>Name</th><th>Branch</th><th>Clock In</th><th>Clock Out</th><th>Duration</th><th>Status</th></tr></thead>
                            <tbody id="attendanceTableBody"></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <div id="adminPaymentsView" class="content-section">
                <div class="section-card">
                    <div id="paymentAlertsFull"></div>
                    <div class="section-header">
                        <h2>Payment Records</h2>
                        <button class="btn btn-primary" onclick="showModal('recordPaymentModal')">+ Record Payment</button>
                    </div>
                    <div class="stats-grid">
                        <div class="stat-card"><div class="label">Total Collected</div><div class="number" id="totalCollected">₦0</div></div>
                        <div class="stat-card"><div class="label">Pending / Partial</div><div class="number" id="pendingTotal" style="color:var(--warning)">₦0</div></div>
                        <div class="stat-card"><div class="label">Expired Plans</div><div class="number" id="expiredCount" style="color:var(--danger)">0</div></div>
                        <div class="stat-card"><div class="label">Expiring Soon</div><div class="number" id="expiringSoonCount" style="color:var(--warning)">0</div></div>
                    </div>
                    <h3 style="margin-bottom:12px;font-size:16px;color:var(--text)">Payment Expiry Status</h3>
                    <div id="paymentExpiryList" style="margin-bottom:20px"></div>
                    <div class="table-responsive">
                        <table>
                            <thead><tr><th>Date</th><th>Student</th><th>Reg No.</th><th>Plan</th><th>Amount</th><th>Expiry</th><th>Status</th></tr></thead>
                            <tbody id="paymentsTableBody"></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <div id="adminSubjectsView" class="content-section">
                <div class="section-card">
                    <div class="section-header"><h2>Department Subjects</h2></div>
                    <div class="grid-3">
                        <div class="branch-card"><h3>HUMANITIES</h3><p><strong>Compulsory:</strong> Mathematics, English, Economics</p><p><strong>Electives:</strong> Government, Literature, CRS, IRS</p></div>
                        <div class="branch-card"><h3>BUSINESS</h3><p><strong>Compulsory:</strong> Mathematics, English, Economics</p><p><strong>Electives:</strong> Accounting, Commerce, Government</p></div>
                        <div class="branch-card"><h3>SCIENCES</h3><p><strong>Compulsory:</strong> Mathematics, English, Economics</p><p><strong>Electives:</strong> Physics, Chemistry, Biology</p></div>
                    </div>
                </div>
            </div>

            <div id="adminCBTView" class="content-section">
                <div class="section-card">
                    <div class="section-header"><h2>CBT Questions</h2><button class="btn btn-primary" onclick="showModal('cbtQuestionModal')">+ Add Question</button></div>
                    <div id="cbtQuestionsContainer"></div>
                </div>
            </div>

            <div id="adminTestsView" class="content-section">
                <div class="section-card">
                    <div class="section-header">
                        <h2>Tests & Exams</h2>
                        <button class="btn btn-primary" onclick="showModal('addTestModal')">+ Create Test</button>
                    </div>
                    <div class="table-responsive">
                        <table>
                            <thead><tr><th>Test Name</th><th>Subject</th><th>Dept</th><th>Date</th><th>Duration</th><th>Questions</th><th>Submissions</th><th>Status</th><th>Actions</th></tr></thead>
                            <tbody id="testsTableBody"></tbody>
                        </table>
                    </div>
                </div>

                <div class="section-card" id="testQuestionPanel" style="display:none">
                    <div class="section-header">
                        <div>
                            <h2>Questions — <span id="testQPanelName" style="color:var(--blue)"></span></h2>
                            <p style="color:var(--muted);font-size:12px;margin-top:4px">Add multiple-choice questions with custom marks.</p>
                        </div>
                        <div class="actions">
                            <button class="btn btn-primary" onclick="showAddTestQuestionForm()">+ Add Question</button>
                            <button class="btn btn-outline" onclick="closeTestQuestionPanel()">✕ Close</button>
                        </div>
                    </div>
                    <div id="addTestQuestionForm" style="display:none;background:var(--surface);border-radius:12px;padding:16px;margin-bottom:16px;animation:fadeIn 0.2s ease">
                        <h4 style="margin-bottom:12px;color:var(--text)">✏️ New Question</h4>
                        <div class="form-group"><label>Question Text *</label><textarea id="tqText" rows="3" style="width:100%;padding:10px;border:1.5px solid var(--border);border-radius:8px;font-size:14px;"></textarea></div>
                        <div class="form-row">
                            <div class="form-group"><label>Option A *</label><input type="text" id="tqA" style="width:100%;padding:10px;border:1.5px solid var(--border);border-radius:8px;font-size:14px;"></div>
                            <div class="form-group"><label>Option B *</label><input type="text" id="tqB" style="width:100%;padding:10px;border:1.5px solid var(--border);border-radius:8px;font-size:14px;"></div>
                        </div>
                        <div class="form-row">
                            <div class="form-group"><label>Option C *</label><input type="text" id="tqC" style="width:100%;padding:10px;border:1.5px solid var(--border);border-radius:8px;font-size:14px;"></div>
                            <div class="form-group"><label>Option D *</label><input type="text" id="tqD" style="width:100%;padding:10px;border:1.5px solid var(--border);border-radius:8px;font-size:14px;"></div>
                        </div>
                        <div class="form-row">
                            <div class="form-group"><label>Correct Answer *</label>
                                <select id="tqCorrect" style="width:100%;padding:10px;border:1.5px solid var(--border);border-radius:8px;font-size:14px;">
                                    <option value="0">A</option><option value="1">B</option><option value="2">C</option><option value="3">D</option>
                                </select>
                            </div>
                            <div class="form-group"><label>Mark(s)</label><input type="number" id="tqMarks" value="1" min="1" style="width:100%;padding:10px;border:1.5px solid var(--border);border-radius:8px;font-size:14px;"></div>
                        </div>
                        <div class="form-group"><label>Explanation (shown after submission)</label><textarea id="tqExplain" rows="2" style="width:100%;padding:10px;border:1.5px solid var(--border);border-radius:8px;font-size:14px;"></textarea></div>
                        <div style="display:flex;gap:8px;">
                            <button class="btn btn-primary" onclick="saveTestQuestion()">💾 Save Question</button>
                            <button class="btn btn-outline" onclick="document.getElementById('addTestQuestionForm').style.display='none'">Cancel</button>
                        </div>
                    </div>
                    <div id="testQuestionsList"></div>
                </div>

                <div class="section-card" id="gradingPanel" style="display:none">
                    <div class="section-header">
                        <div>
                            <h2>📊 Grade Sheet — <span id="gradingTestName" style="color:var(--blue)"></span></h2>
                            <p style="color:var(--muted);font-size:12px;margin-top:4px">Auto-graded results. Post to make visible to students.</p>
                        </div>
                        <div class="actions">
                            <button class="btn btn-success" id="postResultsBtn" onclick="postTestResults()">📢 Post Results</button>
                            <button class="btn btn-outline" onclick="document.getElementById('gradingPanel').style.display='none'">✕ Close</button>
                        </div>
                    </div>
                    <div class="stats-grid" id="gradingStats" style="margin-bottom:16px"></div>
                    <div id="gradeDistribution" style="margin-bottom:16px;"></div>
                    <div class="table-responsive">
                        <table>
                            <thead><tr><th>Reg No.</th><th>Student Name</th><th>Branch</th><th>Score</th><th>Total</th><th>%</th><th>Grade</th><th>Time Taken</th><th>Posted</th></tr></thead>
                            <tbody id="gradingTableBody"></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <div id="adminBranchesView" class="content-section">
                <div class="section-card">
                    <div class="section-header"><h2>Our Branches</h2></div>
                    <div class="grid-3">
                        <div class="branch-card"><h3>Iyana Agbala</h3><p>📍 Opposite Christ Power Junction, Beside Jaagee Building, Iyana Agbala, Ibadan</p><div class="branch-stats"><span><b>Students:</b> <span id="b1count">0</span></span><span>Active ✅</span></div></div>
                        <div class="branch-card"><h3>Omi Bridge</h3><p>📍 Iyana Aba Otun Road, Omi Bridge, Alaro Junction, Ibadan</p><div class="branch-stats"><span><b>Students:</b> <span id="b2count">0</span></span><span>Active ✅</span></div></div>
                        <div class="branch-card"><h3>Ajameta</h3><p>📍 Ajameta Junction, Aroye, Ibadan</p><div class="branch-stats"><span><b>Students:</b> <span id="b3count">0</span></span><span>Active ✅</span></div></div>
                    </div>
                </div>
            </div>

            <div id="adminPaymentSettingsView" class="content-section">
                <div class="section-card">
                    <div class="section-header"><h2>💰 Payment Plan Settings</h2></div>
                    <div class="note">Update the amount and duration for each payment plan. Changes apply to new registrations and renewals.</div>
                    <form id="paymentSettingsForm" onsubmit="savePaymentSettings(event)">
                        <div class="form-row">
                            <div class="form-group">
                                <label>Full Payment Amount (₦)</label>
                                <input type="number" id="fullAmount" required min="0" step="100">
                            </div>
                            <div class="form-group">
                                <label>Full Payment Duration (days)</label>
                                <input type="number" id="fullDays" required min="1" step="1">
                            </div>
                        </div>
                        <div class="form-row">
                            <div class="form-group">
                                <label>Half Payment Amount (₦)</label>
                                <input type="number" id="halfAmount" required min="0" step="100">
                            </div>
                            <div class="form-group">
                                <label>Half Payment Duration (days)</label>
                                <input type="number" id="halfDays" required min="1" step="1">
                            </div>
                        </div>
                        <button type="submit" class="btn btn-primary" style="width:100%;padding:14px;font-size:15px">💾 Save Payment Plans</button>
                    </form>
                    <div id="paymentSettingsStatus" style="margin-top:12px;"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- ========== STUDENT DASHBOARD ========== -->
    <div id="studentDashboard" class="dashboard">
        <div class="sidebar-overlay" id="studentSidebarOverlay" onclick="toggleSidebar('student')"></div>
        <div class="sidebar" id="studentSidebar">
            <div class="sidebar-header">
                <div class="sidebar-logo">
                    <div class="brand">TESTIMONY<span>TUTORS</span></div>
                </div>
                <p>Student Portal</p>
            </div>
            <div class="sidebar-menu">
                <div class="menu-item active" onclick="showStudent('overview')"><span class="icon">📊</span> Overview</div>
                <div class="menu-item" onclick="showStudent('profile')"><span class="icon">👤</span> My Profile</div>
                <div class="menu-item" onclick="showStudent('idcard')"><span class="icon">🪪</span> My ID Card</div>
                <div class="menu-item" onclick="showStudent('subjects')"><span class="icon">📚</span> My Subjects</div>
                <div class="menu-item" onclick="showStudent('tests')"><span class="icon">📝</span> Tests</div>
                <div class="menu-item" onclick="showStudent('payments')"><span class="icon">💰</span> Payments</div>
                <div class="menu-item" onclick="showStudent('cbt')"><span class="icon">💻</span> CBT Practice</div>
            </div>
        </div>

        <div class="main-content">
            <div class="top-bar">
                <div class="top-bar-left">
                    <button class="hamburger" onclick="toggleSidebar('student')" aria-label="Toggle menu">☰</button>
                    <div class="welcome-text">
                        <h3>Welcome, <span id="studentNameDisplay">Student</span></h3>
                        <p id="studentInfoLine">Your learning portal</p>
                    </div>
                </div>
                <div class="top-right">
                    <div class="user-avatar" id="studentAvatarTop">ST</div>
                    <button class="logout-btn" onclick="logout()">Logout</button>
                </div>
            </div>

            <div id="studentOverview" class="content-section active">
                <div class="stats-grid" id="studentStatsGrid"></div>
                <div class="section-card">
                    <h2 style="margin-bottom:14px">Recent Activities</h2>
                    <div class="table-responsive">
                        <table><thead><tr><th>Date</th><th>Activity</th><th>Subject</th><th>Score</th><th>Status</th></tr></thead>
                            <tbody id="studentActivitiesTable"></tbody></table>
                    </div>
                </div>
            </div>

            <div id="studentProfile" class="content-section">
                <div class="section-card" id="studentProfileContent"></div>
            </div>

            <div id="studentIDCardSection" class="content-section">
                <div class="section-card">
                    <div class="section-header"><h2>My ID Card</h2></div>
                    <div id="idCardContainer"></div>
                    <div style="text-align:center;margin-top:16px;display:flex;gap:10px;justify-content:center;flex-wrap:wrap">
                        <button class="btn btn-primary" onclick="printIDCard()">🖨️ Print ID Card</button>
                        <button class="btn btn-success" onclick="downloadIDCard()">⬇️ Download</button>
                    </div>
                </div>
            </div>

            <div id="studentSubjectsSection" class="content-section">
                <div class="section-card">
                    <h2 style="margin-bottom:16px">My Subjects</h2>
                    <div class="subjects-grid" id="studentSubjectsGrid"></div>
                </div>
            </div>

            <div id="studentTestsSection" class="content-section">
                <div class="section-card">
                    <h2 style="margin-bottom:16px">Tests & Examinations</h2>
                    <div class="table-responsive">
                        <table><thead><tr><th>Test Name</th><th>Subject</th><th>Date</th><th>Duration</th><th>Questions</th><th>Status</th><th>Action</th></tr></thead>
                            <tbody id="studentTestsList"></tbody></table>
                    </div>
                </div>
            </div>

            <div id="studentTestInterface" class="content-section">
                <div class="section-card">
                    <div class="section-header">
                        <div>
                            <h2 id="activeTestName"></h2>
                            <p style="color:var(--muted);font-size:12px;margin-top:4px" id="activeTestMeta"></p>
                        </div>
                        <div style="display:flex;gap:8px;align-items:center;flex-wrap:wrap">
                            <div style="background:var(--danger);color:white;padding:6px 14px;border-radius:8px;font-weight:800;font-size:17px;min-width:70px;text-align:center" id="testCountdown"></div>
                            <button class="btn btn-danger" onclick="confirmExitTest()">✕ Exit</button>
                        </div>
                    </div>
                    <div class="cbt-progress" id="testProgress"></div>
                    <div id="testQuestionText" style="font-size:16px;font-weight:600;margin-bottom:14px;line-height:1.5;"></div>
                    <div id="testOptionsContainer"></div>
                    <div style="display:flex;justify-content:space-between;margin-top:20px;gap:8px;">
                        <button class="btn btn-outline" id="testPrevBtn" onclick="testNav(-1)">← Previous</button>
                        <button class="btn btn-primary" id="testNextBtn" onclick="testNav(1)">Next →</button>
                    </div>
                </div>
            </div>

            <div id="studentTestResultInterface" class="content-section">
                <div class="section-card">
                    <div class="section-header"><h2>Test Result</h2><button class="btn btn-primary" onclick="showStudent('tests')">← Back to Tests</button></div>
                    <div id="testResultContent"></div>
                </div>
            </div>

            <div id="studentPostedResults" class="content-section">
                <div class="section-card">
                    <div class="section-header"><h2>📢 My Posted Results</h2><button class="btn btn-outline" onclick="showStudent('tests')">← Back</button></div>
                    <div id="postedResultsContent"></div>
                </div>
            </div>

            <div id="studentPaymentsSection" class="content-section">
                <div class="section-card">
                    <h2 style="margin-bottom:16px">Payment History</h2>
                    <div id="studentPaymentStatus" style="margin-bottom:16px"></div>
                    <div class="table-responsive">
                        <table><thead><tr><th>Date</th><th>Plan</th><th>Amount</th><th>Expiry</th><th>Status</th></tr></thead>
                            <tbody id="studentPaymentsList"></tbody></table>
                    </div>
                </div>
            </div>

            <div id="studentCBTSection" class="content-section">
                <div class="section-card">
                    <h2 style="margin-bottom:16px">CBT Practice Center</h2>
                    <div id="cbtSubjectsList"></div>
                </div>
            </div>

            <div id="cbtPracticeInterface" class="content-section">
                <div class="section-card">
                    <div class="section-header">
                        <h2>Practice: <span id="cbtSubjectName"></span></h2>
                        <button class="btn btn-danger" onclick="showStudent('cbt')">✕ Exit</button>
                    </div>
                    <div class="cbt-timer" id="cbtTimer"></div>
                    <div class="cbt-progress" id="cbtProgress"></div>
                    <div id="cbtQuestion" style="font-size:16px;font-weight:600;margin-bottom:14px;"></div>
                    <div id="cbtOptions"></div>
                    <div style="display:flex;justify-content:space-between;margin-top:20px;gap:8px;">
                        <button class="btn btn-outline" id="prevBtn" onclick="prevQuestion()">← Previous</button>
                        <button class="btn btn-primary" id="nextBtn" onclick="nextQuestion()">Next →</button>
                    </div>
                </div>
            </div>

            <div id="cbtResultsInterface" class="content-section">
                <div class="section-card">
                    <div class="section-header"><h2>Practice Results</h2><button class="btn btn-primary" onclick="showStudent('cbt')">← Back</button></div>
                    <div id="cbtResultsContent"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- ========== MODALS ========== -->
    <div id="viewStudentModal" class="modal">
        <div class="modal-content">
            <div class="modal-header"><h3>Student Details</h3><span class="close-modal" onclick="closeModal('viewStudentModal')">×</span></div>
            <div id="viewStudentContent"></div>
        </div>
    </div>

    <div id="editStudentModal" class="modal">
        <div class="modal-content">
            <div class="modal-header"><h3>Edit Student</h3><span class="close-modal" onclick="closeModal('editStudentModal')">×</span></div>
            <form id="editStudentForm" onsubmit="updateStudent(event)">
                <input type="hidden" id="editStudentId">
                <div class="form-row">
                    <div class="form-group"><label>First Name</label><input type="text" id="editFirstName" required></div>
                    <div class="form-group"><label>Last Name</label><input type="text" id="editLastName" required></div>
                </div>
                <div class="form-row">
                    <div class="form-group"><label>Email</label><input type="email" id="editEmail" required></div>
                    <div class="form-group"><label>Phone</label><input type="tel" id="editPhone" required></div>
                </div>
                <div class="form-row">
                    <div class="form-group"><label>Branch</label><select id="editBranch" required>
                            <option>Iyana Agbala</option><option>Omi Bridge</option><option>Ajameta</option>
                        </select></div>
                    <div class="form-group"><label>Department</label><select id="editDepartment" required>
                            <option>HUMANITIES</option><option>BUSINESS</option><option>SCIENCES</option>
                        </select></div>
                </div>
                <div class="form-group">
                    <label>Exam Type</label>
                    <select id="editExamType" required>
                        <option value="NECO">NECO</option>
                        <option value="WAEC">WAEC</option>
                        <option value="JAMB">JAMB</option>
                    </select>
                </div>
                <div class="form-row">
                    <div class="form-group"><label>Payment Plan</label><select id="editPaymentPlan">
                            <option value="full">Full — ₦15,000 / 30 days</option>
                            <option value="half">Half — ₦8,000 / 14 days</option>
                        </select></div>
                    <div class="form-group"><label>Payment Date</label><input type="date" id="editPaymentDate"></div>
                </div>
                <button type="submit" class="btn btn-primary" style="width:100%">Update Student</button>
            </form>
        </div>
    </div>

    <div id="recordPaymentModal" class="modal">
        <div class="modal-content">
            <div class="modal-header"><h3>Record Payment</h3><span class="close-modal" onclick="closeModal('recordPaymentModal')">×</span></div>
            <form onsubmit="recordPayment(event)">
                <div class="form-group"><label>Student Reg No.</label><input type="text" id="paymentStudentId" placeholder="TT27001" required></div>
                <div class="form-group"><label>Payment Plan</label>
                    <select id="paymentPlanModal" required onchange="setPaymentAmount()">
                        <option value="">Select Plan</option>
                        <option value="full">Full Payment — ₦15,000 (30 days)</option>
                        <option value="half">Half Payment — ₦8,000 (14 days)</option>
                    </select>
                </div>
                <div class="form-group"><label>Amount (₦)</label><input type="number" id="paymentAmountModal" readonly></div>
                <div class="form-group"><label>Payment Date</label><input type="date" id="paymentRecordDate" required></div>
                <button type="submit" class="btn btn-primary" style="width:100%">Record Payment</button>
            </form>
        </div>
    </div>

    <div id="cbtQuestionModal" class="modal">
        <div class="modal-content">
            <div class="modal-header"><h3>Add CBT Question</h3><span class="close-modal" onclick="closeModal('cbtQuestionModal')">×</span></div>
            <form onsubmit="saveQuestion(event)">
                <div class="form-group"><label>Subject</label><select id="cbtQSubject" required>
                        <option value="">Select</option>
                        <option>Mathematics</option><option>English</option><option>Economics</option>
                        <option>Physics</option><option>Chemistry</option><option>Biology</option>
                        <option>Government</option><option>Literature</option><option>Accounting</option>
                        <option>Commerce</option><option>CRS</option>
                    </select></div>
                <div class="form-group"><label>Question</label><textarea id="cbtQText" rows="3" required></textarea></div>
                <div class="form-row">
                    <div class="form-group"><label>Option A</label><input type="text" id="cbtQA" required></div>
                    <div class="form-group"><label>Option B</label><input type="text" id="cbtQB" required></div>
                </div>
                <div class="form-row">
                    <div class="form-group"><label>Option C</label><input type="text" id="cbtQC" required></div>
                    <div class="form-group"><label>Option D</label><input type="text" id="cbtQD" required></div>
                </div>
                <div class="form-group"><label>Correct Answer</label>
                    <select id="cbtQCorrect" required><option value="0">A</option><option value="1">B</option><option value="2">C</option><option value="3">D</option></select>
                </div>
                <div class="form-group"><label>Explanation (optional)</label><textarea id="cbtQExplain" rows="2"></textarea></div>
                <button type="submit" class="btn btn-primary" style="width:100%">Save Question</button>
            </form>
        </div>
    </div>

    <div id="addTestModal" class="modal">
        <div class="modal-content">
            <div class="modal-header"><h3>Create Test</h3><span class="close-modal" onclick="closeModal('addTestModal')">×</span></div>
            <form onsubmit="addTest(event)">
                <div class="form-group"><label>Test Name</label><input type="text" id="testName" required></div>
                <div class="form-row">
                    <div class="form-group"><label>Subject</label><select id="testSubject" required>
                            <option value="">Select</option>
                            <option>Mathematics</option><option>English</option><option>Economics</option>
                            <option>Physics</option><option>Chemistry</option><option>Biology</option>
                        </select></div>
                    <div class="form-group"><label>Department</label><select id="testDepartment" required>
                            <option value="ALL">ALL</option><option>HUMANITIES</option><option>BUSINESS</option><option>SCIENCES</option>
                        </select></div>
                </div>
                <div class="form-row">
                    <div class="form-group"><label>Date</label><input type="date" id="testDate" required></div>
                    <div class="form-group"><label>Duration (mins)</label><input type="number" id="testDuration" value="120" required></div>
                </div>
                <button type="submit" class="btn btn-primary" style="width:100%">Create Test</button>
            </form>
        </div>
    </div>

    <div id="changePasswordModal" class="modal">
        <div class="modal-content">
            <div class="modal-header"><h3>🔑 Change Password</h3><span class="close-modal" onclick="closeModal('changePasswordModal')">×</span></div>
            <form onsubmit="changePassword(event)">
                <div class="form-group">
                    <label>Current Password</label>
                    <input type="password" id="currentPassword" placeholder="Enter your current password" required>
                </div>
                <div class="form-group">
                    <label>New Password</label>
                    <input type="password" id="newPassword" placeholder="Enter new password" required minlength="4">
                </div>
                <div class="form-group">
                    <label>Confirm New Password</label>
                    <input type="password" id="confirmPassword" placeholder="Confirm new password" required>
                </div>
                <button type="submit" class="btn btn-primary" style="width:100%">Update Password</button>
            </form>
        </div>
    </div>

    <div id="printArea" style="display:none"></div>

    <!-- ========== SCRIPT ========== -->
    <script>
        // =================== DATA HELPERS ===================
        function initData() {
            if (!ls('students')) ls('students', []);
            if (!ls('payments')) ls('payments', []);
            if (!ls('tests')) ls('tests', []);
            if (!ls('cbtQuestions')) ls('cbtQuestions', {});
            if (!ls('cbtResults')) ls('cbtResults', []);
            if (!ls('attendance')) ls('attendance', []);
            if (!ls('testQuestions')) ls('testQuestions', {});
            if (!ls('testResults')) ls('testResults', []);
            if (!ls('paymentPlans')) {
                ls('paymentPlans', { full: { amount: 15000, days: 30 }, half: { amount: 8000, days: 14 } });
            }
        }

        function fmtDate(d) { return d.toISOString().split('T')[0]; }

        function ls(k, v) {
            if (v !== undefined) { localStorage.setItem(k, JSON.stringify(v)); return v; }
            const r = localStorage.getItem(k);
            return r ? JSON.parse(r) : null;
        }

        // =================== PAYMENT PLANS ===================
        function getPaymentPlans() {
            const defaultPlans = { full: { amount: 15000, days: 30 }, half: { amount: 8000, days: 14 } };
            const saved = ls('paymentPlans');
            if (saved && saved.full && saved.half) return saved;
            return defaultPlans;
        }

        function getPlan(planKey) {
            const plans = getPaymentPlans();
            return plans[planKey] || null;
        }

        // =================== REG NUMBER GENERATION ===================
        function generateRegNo(students) {
            const base = 27000;
            const next = students.length + 1;
            return 'TT' + (base + next);
        }

        // =================== PAYMENT HELPERS ===================
        function getExpiryDate(student) {
            if (!student.paymentDate || !student.paymentPlan) return null;
            const plan = getPlan(student.paymentPlan);
            if (!plan) return null;
            const d = new Date(student.paymentDate + 'T00:00:00');
            if (isNaN(d.getTime())) return null;
            d.setDate(d.getDate() + plan.days);
            return d;
        }

        function getDaysLeft(student) {
            const exp = getExpiryDate(student);
            if (!exp) return null;
            const now = new Date();
            now.setHours(0, 0, 0, 0);
            return Math.ceil((exp - now) / (1000 * 60 * 60 * 24));
        }

        function getPaymentStatusBadge(student) {
            const days = getDaysLeft(student);
            if (days === null) return '<span class="badge badge-gray">No Payment</span>';
            if (days < 0) return '<span class="badge badge-red">Expired ' + Math.abs(days) + 'd ago</span>';
            if (days <= 5) return '<span class="badge badge-yellow">Expiring in ' + days + 'd</span>';
            return '<span class="badge badge-green">Active (' + days + 'd left)</span>';
        }

        function getAlertStudents() {
            const students = ls('students') || [];
            return students.filter(s => {
                const days = getDaysLeft(s);
                return days !== null && days <= 5;
            }).sort((a, b) => getDaysLeft(a) - getDaysLeft(b));
        }

        // =================== QR CODE GENERATION (FIXED) ===================
        function generateQRWhite(containerId, data, size) {
            size = size || 110;
            const container = document.getElementById(containerId);
            if (!container) {
                console.warn('QR container not found:', containerId);
                return;
            }
            container.innerHTML = '';
            try {
                if (typeof QRCode === 'undefined') {
                    throw new Error('QRCode library not loaded');
                }
                const qr = new QRCode(container, {
                    text: data,
                    width: size,
                    height: size,
                    colorDark: '#000000',
                    colorLight: '#ffffff',
                    correctLevel: QRCode.CorrectLevel.H
                });
                const canvas = container.querySelector('canvas');
                if (!canvas) throw new Error('No canvas created');
                console.log('QR generated successfully for:', data);
            } catch (e) {
                console.error('QR generation error:', e);
                container.innerHTML =
                    `<div style="background:white;padding:8px;border-radius:6px;font-family:monospace;font-size:11px;color:#1a1a2e;text-align:center;word-break:break-all;max-width:110px;margin:0 auto;">${data}</div>`;
            }
        }

        // =================== SIDEBAR TOGGLE ===================
        function toggleSidebar(type) {
            const sidebar = document.getElementById(type === 'admin' ? 'adminSidebar' : 'studentSidebar');
            const overlay = document.getElementById(type === 'admin' ? 'adminSidebarOverlay' : 'studentSidebarOverlay');
            if (!sidebar) return;
            const isOpen = sidebar.classList.toggle('open');
            if (overlay) overlay.classList.toggle('active', isOpen);
        }

        document.addEventListener('click', function(e) {
            if (e.target.classList.contains('sidebar-overlay')) {
                const type = e.target.id.includes('admin') ? 'admin' : 'student';
                toggleSidebar(type);
            }
        });

        document.querySelectorAll('#adminSidebar .menu-item, #studentSidebar .menu-item').forEach(item => {
            item.addEventListener('click', function() {
                const sidebar = this.closest('.sidebar');
                if (sidebar && window.innerWidth < 768) {
                    sidebar.classList.remove('open');
                    const overlay = document.querySelector('.sidebar-overlay.active');
                    if (overlay) overlay.classList.remove('active');
                }
            });
        });

        // =================== AUTH ===================
        function switchTab(t) {
            document.querySelectorAll('.login-tab').forEach((tab, i) => {
                tab.classList.toggle('active', (t === 'student' && i === 0) || (t === 'admin' && i === 1));
            });
            document.getElementById('studentLogin').classList.toggle('active', t === 'student');
            document.getElementById('adminLogin').classList.toggle('active', t === 'admin');
        }

        function handleStudentLogin(e) {
            e.preventDefault();
            const regNo = document.getElementById('studentRegNo').value.trim();
            const pwd = document.getElementById('studentPassword').value;
            const students = ls('students') || [];
            const student = students.find(s => s.regNo === regNo && s.password === pwd);
            if (!student) { alert('Invalid credentials. Check your registration number and password.'); return; }
            if (student.status === 'suspended') { alert('Account suspended. Contact admin.'); return; }
            ls('currentStudent', student);
            document.getElementById('loginContainer').style.display = 'none';
            document.getElementById('studentDashboard').classList.add('active');
            loadStudentDashboard();
        }

        function handleAdminLogin(e) {
            e.preventDefault();
            const id = document.getElementById('adminId').value;
            const pwd = document.getElementById('adminPassword').value;
            if (id === 'admin' && pwd === 'admin123') {
                document.getElementById('loginContainer').style.display = 'none';
                document.getElementById('adminDashboard').classList.add('active');
                loadAdminDashboard();
            } else {
                alert('Invalid admin credentials.');
            }
        }

        function logout() {
            ls('currentStudent', null);
            localStorage.removeItem('currentStudent');
            document.getElementById('loginContainer').style.display = 'flex';
            document.getElementById('studentDashboard').classList.remove('active');
            document.getElementById('adminDashboard').classList.remove('active');
            document.querySelectorAll('.sidebar').forEach(s => s.classList.remove('open'));
            document.querySelectorAll('.sidebar-overlay').forEach(o => o.classList.remove('active'));
        }

        // =================== ADMIN DASHBOARD ===================
        function loadAdminDashboard() {
            document.getElementById('adminDateLine').textContent = new Date().toDateString();
            renderAdminStats();
            renderRecentStudents();
            renderPaymentAlerts('paymentAlertsPanel');
            updateAlertBadges();
            const alerts = getAlertStudents();
            document.getElementById('paymentAlertsCard').style.display = alerts.length ? 'block' : 'none';
            renderBranchCounts();
        }

        function renderAdminStats() {
            const students = ls('students') || [];
            const payments = ls('payments') || [];
            const attendance = ls('attendance') || [];
            const todayStr = fmtDate(new Date());
            const todayAttend = attendance.filter(a => a.date === todayStr);
            const alerts = getAlertStudents();
            const totalCollected = payments.reduce((s, p) => s + (p.amount || 0), 0);
            const grid = document.getElementById('adminStatsGrid');
            grid.innerHTML = `
                <div class="stat-card" onclick="showAdmin('students')"><div class="label">Total Students</div><div class="number">${students.length}</div><div class="sub">Across all branches</div></div>
                <div class="stat-card" onclick="showAdmin('attendance')"><div class="label">Present Today</div><div class="number" style="color:var(--success)">${todayAttend.filter(a=>a.status==='clocked-in'||a.clockOut).length}</div><div class="sub">Clock-ins today</div></div>
                <div class="stat-card" onclick="showAdmin('payments')"><div class="label">Total Collected</div><div class="number">₦${(totalCollected/1000).toFixed(0)}k</div><div class="sub">All payments</div></div>
                <div class="stat-card" onclick="showAdmin('payments')" style="${alerts.length ? 'border-color:var(--warning)' : ''}"><div class="label">Payment Alerts</div><div class="number" style="color:${alerts.length ? 'var(--warning)' : 'var(--success)'}">${alerts.length}</div><div class="sub">Expiring / expired</div></div>
            `;
        }

        function renderBranchCounts() {
            const students = ls('students') || [];
            const b1 = students.filter(s => s.branch === 'Iyana Agbala').length;
            const b2 = students.filter(s => s.branch === 'Omi Bridge').length;
            const b3 = students.filter(s => s.branch === 'Ajameta').length;
            ['b1count', 'b2count', 'b3count'].forEach((id, i) => {
                const el = document.getElementById(id);
                if (el) el.textContent = [b1, b2, b3][i];
            });
        }

        function renderRecentStudents() {
            const students = (ls('students') || []).slice(-5).reverse();
            const tbody = document.getElementById('recentStudentsTable');
            tbody.innerHTML = students.map(s => `<tr>
                <td><code style="font-size:11px">${s.regNo}</code></td>
                <td><b>${s.fullName}</b></td>
                <td><span class="badge badge-blue">${s.department}</span></td>
                <td>${s.branch}</td>
                <td>${getPaymentStatusBadge(s)}</td>
                <td><span class="badge ${s.status==='active'?'badge-green':'badge-red'}">${s.status}</span></td>
                <td class="action-btns">
                    <button class="action-btn view-btn" onclick="viewStudent('${s.id}')">View</button>
                    <button class="action-btn id-btn" onclick="showAdmin('students')">ID</button>
                </td>
            </tr>`).join('');
        }

        function updateAlertBadges() {
            const alerts = getAlertStudents();
            const count = alerts.length;
            const badge = document.getElementById('paymentAlertBadge');
            const topBadge = document.getElementById('topAlertBadge');
            const topCount = document.getElementById('topAlertCount');
            if (badge) badge.textContent = count;
            if (topBadge) { topBadge.classList.toggle('visible', count > 0); }
            if (topCount) topCount.textContent = count;
        }

        function showRenewPrompt(studentId) {
            const students = ls('students') || [];
            const s = students.find(st => st.id === studentId);
            if (!s) return;
            const plans = getPaymentPlans();
            if (confirm(`Renew payment for ${s.fullName}?\n\nFull Plan: ₦${plans.full.amount.toLocaleString()} (${plans.full.days} days)\nHalf Plan: ₦${plans.half.amount.toLocaleString()} (${plans.half.days} days)\n\nClick OK to open Record Payment.`)) {
                document.getElementById('paymentStudentId').value = s.regNo;
                showModal('recordPaymentModal');
            }
        }

        function renderPaymentAlerts(containerId) {
            const alerts = getAlertStudents();
            const container = document.getElementById(containerId);
            if (!container) return;
            if (!alerts.length) { container.innerHTML = ''; return; }
            let html =
                `<div class="alert-panel"><h4>⚠️ Payment Alerts — ${alerts.length} student(s) need attention</h4>`;
            alerts.forEach(s => {
                const days = getDaysLeft(s);
                const isCritical = days < 0;
                const plan = getPlan(s.paymentPlan) || {};
                html += `<div class="alert-item ${isCritical ? 'critical' : ''}">
                    <div>
                        <div class="ai">${s.fullName} — ${s.regNo}</div>
                        <div class="ad">${s.branch} | ${s.paymentPlan? (s.paymentPlan==='full'?'Full':'Half') : 'None'} | Paid: ₦${(s.amountPaid||0).toLocaleString()}</div>
                        <div class="ad">${isCritical ? '🔴 Expired ' + Math.abs(days) + ' day(s) ago' : '🟡 Expires in ' + days + ' day(s)'}</div>
                    </div>
                    <button class="ab ${isCritical ? 'btn-danger' : 'btn-warn'}" onclick="showRenewPrompt('${s.id}')">
                        ${isCritical ? 'RENEW NOW' : 'Notify'}
                    </button>
                </div>`;
            });
            html += '</div>';
            container.innerHTML = html;
        }

        // =================== ADMIN VIEWS ===================
        let filteredStudents = null;

        function showAdmin(section) {
            document.querySelectorAll('#adminDashboard .content-section').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('#adminDashboard .menu-item').forEach(m => m.classList.remove('active'));
            const sectionMap = {
                dashboard: 'adminDashboardView',
                students: 'adminStudentsView',
                addStudent: 'adminAddStudentView',
                attendance: 'adminAttendanceView',
                payments: 'adminPaymentsView',
                subjects: 'adminSubjectsView',
                cbt: 'adminCBTView',
                tests: 'adminTestsView',
                branches: 'adminBranchesView',
                paymentSettings: 'adminPaymentSettingsView'
            };
            const el = document.getElementById(sectionMap[section]);
            if (el) el.classList.add('active');
            const idx = { dashboard: 0, students: 1, addStudent: 2, attendance: 3, payments: 4, subjects: 5, cbt: 6,
                tests: 7, branches: 8, paymentSettings: 9 };
            const items = document.querySelectorAll('#adminDashboard .menu-item');
            if (items[idx[section]]) items[idx[section]].classList.add('active');

            if (section === 'students') renderStudentsTable();
            if (section === 'payments') renderPaymentsSection();
            if (section === 'attendance') { renderAttendanceTable();
                renderQuickStudentPicker(); }
            if (section === 'tests') renderTestsTable();
            if (section === 'cbt') renderCBTQuestions();
            if (section === 'branches') renderBranchCounts();
            if (section === 'dashboard') loadAdminDashboard();
            if (section === 'addStudent') updateAutoLoginPreview();
            if (section === 'paymentSettings') loadPaymentSettings();
            if (section !== 'attendance') stopCameraScanner();
            if (section === 'attendance') {
                setTimeout(() => {
                    renderQuickStudentPicker();
                    const bi = document.getElementById('barcodeInput');
                    if (bi) bi.focus();
                }, 120);
            }
        }

        function updateAutoLoginPreview() {
            const lname = document.getElementById('lastName').value.trim();
            const students = ls('students') || [];
            const regNo = generateRegNo(students);
            document.getElementById('autoUsername').textContent = regNo;
            document.getElementById('autoPassword').textContent = lname || '[surname]';
        }

        document.getElementById('firstName')?.addEventListener('input', updateAutoLoginPreview);
        document.getElementById('lastName')?.addEventListener('input', updateAutoLoginPreview);

        // =================== PHOTO UPLOAD ===================
        let uploadedPhotoData = null;

        function previewPhoto(event) {
            const file = event.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(e) {
                const preview = document.getElementById('photoPreview');
                preview.innerHTML = `<img src="${e.target.result}" alt="Passport">`;
                uploadedPhotoData = e.target.result;
            };
            reader.readAsDataURL(file);
        }

        // =================== ADD STUDENT ===================
        function updatePaymentFields() {
            const plan = document.getElementById('paymentPlan').value;
            const amountField = document.getElementById('amountPaid');
            const summary = document.getElementById('paymentSummary');
            if (plan) {
                const p = getPlan(plan);
                if (p) {
                    amountField.value = p.amount;
                    summary.innerHTML =
                        `<div class="note">💡 ${plan==='full'?'Full':'Half'} Payment: ₦${p.amount.toLocaleString()} valid for ${p.days} days from payment date</div>`;
                }
            } else {
                amountField.value = 0;
                summary.innerHTML = '';
            }
        }

        function getSubjectsForDept(dept) {
            const base = ['Mathematics', 'English', 'Economics'];
            const extra = { HUMANITIES: ['Government', 'Literature', 'CRS', 'IRS'], BUSINESS: ['Accounting', 'Commerce',
                    'Government'
                ], SCIENCES: ['Physics', 'Chemistry', 'Biology'] };
            return [...base, ...(extra[dept] || [])];
        }

        function addStudent(e) {
            e.preventDefault();
            const students = ls('students') || [];
            const id = generateRegNo(students);
            const dept = document.getElementById('department').value;
            const payPlan = document.getElementById('paymentPlan').value;
            const lname = document.getElementById('lastName').value.trim();
            const examType = document.getElementById('examType').value;

            const planData = getPlan(payPlan);

            const student = {
                id,
                regNo: id,
                firstName: document.getElementById('firstName').value.trim(),
                lastName: lname,
                fullName: document.getElementById('firstName').value.trim() + ' ' + lname,
                dob: document.getElementById('dob').value,
                gender: document.getElementById('gender').value,
                email: document.getElementById('email').value.trim(),
                phone: document.getElementById('phone').value.trim(),
                address: document.getElementById('address').value.trim(),
                branch: document.getElementById('branch').value,
                department: dept,
                examType: examType,
                subjects: getSubjectsForDept(dept),
                regDate: document.getElementById('regDate').value,
                paymentPlan: payPlan,
                amountPaid: payPlan && planData ? planData.amount : 0,
                paymentDate: document.getElementById('paymentDate').value,
                guardianName: document.getElementById('guardianName').value.trim(),
                guardianPhone: document.getElementById('guardianPhone').value.trim(),
                username: id,
                password: lname.toLowerCase(),
                status: 'active',
                activities: [],
                cbtScores: [],
                photo: uploadedPhotoData || null
            };
            students.push(student);
            ls('students', students);

            if (payPlan && student.paymentDate && planData) {
                const payments = ls('payments') || [];
                payments.push({ id: 'PAY' + Date.now(), date: student.paymentDate, student: student.fullName, regNo: id,
                    plan: payPlan, amount: planData.amount, status: 'Paid' });
                ls('payments', payments);
            }

            alert('✅ Student ' + student.fullName + ' registered!\nReg No: ' + id + '\nUsername: ' + id +
                '\nPassword: ' + lname.toLowerCase() + '\nExam Type: ' + examType);
            document.getElementById('addStudentForm').reset();
            document.getElementById('paymentSummary').innerHTML = '';
            document.getElementById('photoPreview').innerHTML = '📷';
            uploadedPhotoData = null;
            showAdmin('students');
        }

        // =================== STUDENTS TABLE ===================
        function renderStudentsTable(data) {
            const students = data || filteredStudents || ls('students') || [];
            const tbody = document.getElementById('studentsTableBody');
            if (!students.length) {
                tbody.innerHTML =
                    '<tr><td colspan="8" style="text-align:center;color:var(--muted);padding:24px">No students found</td></tr>';
                return;
            }
            tbody.innerHTML = students.map(s => {
                const days = getDaysLeft(s);
                const daysDisplay = days === null ? '—' : days < 0 ?
                    `<span style="color:var(--danger)">Exp. ${Math.abs(days)}d ago</span>` : days <= 5 ?
                    `<span style="color:var(--warning)">${days}d</span>` :
                    `<span style="color:var(--success)">${days}d</span>`;
                return `<tr>
                    <td><code style="font-size:11px">${s.regNo}</code></td>
                    <td><b>${s.fullName}</b></td>
                    <td><span class="badge badge-blue">${s.department}</span></td>
                    <td>${s.branch}</td>
                    <td>${getPaymentStatusBadge(s)}</td>
                    <td>${daysDisplay}</td>
                    <td><span class="badge ${s.status==='active'?'badge-green':'badge-red'}">${s.status}</span></td>
                    <td class="action-btns">
                        <button class="action-btn view-btn" onclick="viewStudent('${s.id}')">View</button>
                        <button class="action-btn edit-btn" onclick="editStudent('${s.id}')">Edit</button>
                        <button class="action-btn id-btn" onclick="adminViewIDCard('${s.id}')">ID Card</button>
                        <button class="action-btn pay-btn" onclick="quickRecordPayment('${s.id}')">Payment</button>
                        <button class="action-btn delete-btn" onclick="deleteStudent('${s.id}')">Del</button>
                    </td>
                </tr>`;
            }).join('');
        }

        function searchStudents() {
            const q = document.getElementById('searchStudent').value.toLowerCase();
            const students = ls('students') || [];
            filteredStudents = students.filter(s =>
                s.fullName.toLowerCase().includes(q) || s.regNo.toLowerCase().includes(q) || s.branch.toLowerCase()
                .includes(q)
            );
            renderStudentsTable();
        }

        function filterStudentsByBranch(branch) {
            const students = ls('students') || [];
            filteredStudents = branch ? students.filter(s => s.branch === branch) : students;
            renderStudentsTable();
        }

        // =================== VIEW/EDIT/DELETE STUDENT ===================
        function viewStudent(id) {
            const s = (ls('students') || []).find(st => st.id === id);
            if (!s) return;
            const days = getDaysLeft(s);
            const exp = getExpiryDate(s);
            document.getElementById('viewStudentContent').innerHTML = `
                <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;font-size:13px;margin-bottom:14px">
                    <div style="grid-column:1/-1;text-align:center;margin-bottom:6px">
                        <div style="width:80px;height:80px;border-radius:50%;margin:0 auto;border:3px solid var(--gold);overflow:hidden;background:var(--surface);display:flex;align-items:center;justify-content:center;font-size:28px;color:var(--muted)">
                            ${s.photo ? `<img src="${s.photo}" style="width:100%;height:100%;object-fit:cover">` : (s.firstName[0]+s.lastName[0]).toUpperCase()}
                        </div>
                    </div>
                    <div><b style="color:var(--blue)">Full Name:</b><br>${s.fullName}</div>
                    <div><b style="color:var(--blue)">Reg No.:</b><br><code>${s.regNo}</code></div>
                    <div><b style="color:var(--blue)">Branch:</b><br>${s.branch}</div>
                    <div><b style="color:var(--blue)">Department:</b><br>${s.department}</div>
                    <div><b style="color:var(--blue)">Exam Type:</b><br><span class="badge badge-blue">${s.examType || 'N/A'}</span></div>
                    <div><b style="color:var(--blue)">Phone:</b><br>${s.phone}</div>
                    <div><b style="color:var(--blue)">Email:</b><br>${s.email}</div>
                    <div><b style="color:var(--blue)">Payment Plan:</b><br>${s.paymentPlan ? (s.paymentPlan==='full'?'Full':'Half') : '—'}</div>
                    <div><b style="color:var(--blue)">Amount Paid:</b><br>₦${(s.amountPaid||0).toLocaleString()}</div>
                    <div><b style="color:var(--blue)">Payment Date:</b><br>${s.paymentDate || '—'}</div>
                    <div><b style="color:var(--blue)">Expiry:</b><br>${exp ? exp.toDateString() : '—'}</div>
                    <div><b style="color:var(--blue)">Days Left:</b><br>${days === null ? '—' : days < 0 ? '🔴 Expired '+Math.abs(days)+'d ago' : days <= 5 ? '🟡 '+days+' days' : '🟢 '+days+' days'}</div>
                    <div><b style="color:var(--blue)">Status:</b><br><span class="badge ${s.status==='active'?'badge-green':'badge-red'}">${s.status}</span></div>
                    <div><b style="color:var(--blue)">Login:</b><br>${s.username} / ${s.password}</div>
                </div>
                <div><b style="color:var(--blue)">Subjects:</b><br>${s.subjects.map(sub=>'<span class="tag-pill">'+sub+'</span>').join('')}</div>
                <div style="margin-top:12px;display:flex;gap:8px;flex-wrap:wrap">
                    <button class="btn btn-primary" onclick="adminViewIDCard('${s.id}'); closeModal('viewStudentModal')">View ID Card</button>
                    <button class="btn btn-success" onclick="quickRecordPayment('${s.id}')">Record Payment</button>
                </div>`;
            showModal('viewStudentModal');
        }

        function editStudent(id) {
            const s = (ls('students') || []).find(st => st.id === id);
            if (!s) return;
            document.getElementById('editStudentId').value = id;
            document.getElementById('editFirstName').value = s.firstName;
            document.getElementById('editLastName').value = s.lastName;
            document.getElementById('editEmail').value = s.email;
            document.getElementById('editPhone').value = s.phone;
            document.getElementById('editBranch').value = s.branch;
            document.getElementById('editDepartment').value = s.department;
            document.getElementById('editExamType').value = s.examType || 'NECO';
            document.getElementById('editPaymentPlan').value = s.paymentPlan || 'full';
            document.getElementById('editPaymentDate').value = s.paymentDate || '';
            showModal('editStudentModal');
        }

        function updateStudent(e) {
            e.preventDefault();
            const students = ls('students') || [];
            const id = document.getElementById('editStudentId').value;
            const idx = students.findIndex(s => s.id === id);
            if (idx < 0) return;
            const plan = document.getElementById('editPaymentPlan').value;
            const planData = getPlan(plan);
            students[idx].firstName = document.getElementById('editFirstName').value.trim();
            students[idx].lastName = document.getElementById('editLastName').value.trim();
            students[idx].fullName = students[idx].firstName + ' ' + students[idx].lastName;
            students[idx].email = document.getElementById('editEmail').value.trim();
            students[idx].phone = document.getElementById('editPhone').value.trim();
            students[idx].branch = document.getElementById('editBranch').value;
            students[idx].department = document.getElementById('editDepartment').value;
            students[idx].examType = document.getElementById('editExamType').value;
            students[idx].subjects = getSubjectsForDept(students[idx].department);
            students[idx].paymentPlan = plan;
            students[idx].amountPaid = planData ? planData.amount : 0;
            students[idx].paymentDate = document.getElementById('editPaymentDate').value;
            if (students[idx].lastName !== students[idx].password) {
                students[idx].password = students[idx].lastName.toLowerCase();
            }
            ls('students', students);
            closeModal('editStudentModal');
            renderStudentsTable();
            alert('✅ Student updated!');
        }

        function deleteStudent(id) {
            if (!confirm('Delete this student? This action cannot be undone.')) return;
            const students = (ls('students') || []).filter(s => s.id !== id);
            ls('students', students);
            renderStudentsTable();
        }

        // =================== ID CARD (FIXED QR) ===================
        function adminViewIDCard(id) {
            const s = (ls('students') || []).find(st => st.id === id);
            if (!s) return;
            ls('currentStudent', s);
            showStudent('idcard');
            document.getElementById('adminDashboard').classList.remove('active');
            document.getElementById('studentDashboard').classList.add('active');
            loadStudentDashboard();
            showStudent('idcard');
        }

        function renderIDCard(s) {
            const container = document.getElementById('idCardContainer');
            const initials = (s.firstName[0] + s.lastName[0]).toUpperCase();
            const qrData = s.regNo.replace(/[^A-Z0-9]/g, '');
            const qrId = 'idCardQR_' + Date.now();

            container.innerHTML = `
                <div class="id-card-wrap">
                    <div class="id-card" id="printArea">
                        <div class="id-card-top"></div>
                        <div class="id-card-body">
                            <div class="id-card-header">
                                <h2>TESTIMONY TUTORS</h2>
                                <div class="slogan">"Life is practical…"</div>
                                <div class="exam-type-badge">${s.examType || 'NECO'}</div>
                            </div>
                            <div class="id-card-photo">
                                ${s.photo ? `<img src="${s.photo}" alt="Photo">` : initials}
                            </div>
                            <div class="id-card-detail"><span class="id-card-lbl">Name:</span><span class="id-card-val"><b>${s.fullName}</b></span></div>
                            <div class="id-card-detail"><span class="id-card-lbl">Reg No:</span><span class="id-card-val"><code>${s.regNo}</code></span></div>
                            <div class="id-card-detail"><span class="id-card-lbl">Dept:</span><span class="id-card-val">${s.department}</span></div>
                            <div class="id-card-detail"><span class="id-card-lbl">Branch:</span><span class="id-card-val">${s.branch}</span></div>
                            <div class="id-card-detail"><span class="id-card-lbl">Joined:</span><span class="id-card-val">${s.regDate}</span></div>
                        </div>
                        <div class="id-card-qr-section">
                            <div id="${qrId}"></div>
                            <div class="qr-id-text">${qrData}</div>
                        </div>
                    </div>
                </div>`;

            setTimeout(() => {
                generateQRWhite(qrId, qrData, 110);
            }, 150);
        }

        function printIDCard() { window.print(); }

        function downloadIDCard() { alert('To save your ID card: Right-click on the ID card → Print → Save as PDF'); }

        // =================== ATTENDANCE ===================
        let attendanceFilter = 'all';

        function processBarcode(val) {
            const input = document.getElementById('barcodeInput');
            const value = val || input.value.trim().toUpperCase();
            const result = document.getElementById('scanResult');
            if (!value) return;

            const students = ls('students') || [];
            const student = students.find(s => s.regNo.toUpperCase() === value);

            if (!student) {
                result.className = 'scan-result error';
                result.style.display = 'block';
                result.innerHTML = '❌ Student not found. Check QR code or registration number.';
                if (input) input.value = '';
                return;
            }

            const attendance = ls('attendance') || [];
            const now = new Date();
            const todayStr = fmtDate(now);
            const timeStr = now.toLocaleTimeString('en-GB', { hour: '2-digit', minute: '2-digit', second: '2-digit' });

            const todayEntry = attendance.find(a => a.regNo === student.regNo && a.date === todayStr);

            if (!todayEntry) {
                attendance.push({ id: 'ATT' + Date.now(), date: todayStr, regNo: student.regNo, name: student.fullName,
                    branch: student.branch, clockIn: timeStr, clockOut: null, status: 'clocked-in' });
                ls('attendance', attendance);
                result.className = 'scan-result success';
                result.innerHTML = `✅ CLOCKED IN — ${student.fullName} (${student.regNo})<br>Branch: ${student.branch} | Time: ${timeStr}`;
            } else if (!todayEntry.clockOut) {
                const idx = attendance.indexOf(todayEntry);
                attendance[idx].clockOut = timeStr;
                attendance[idx].status = 'clocked-out';
                const inTime = new Date(todayStr + 'T' + todayEntry.clockIn);
                const outTime = new Date(todayStr + 'T' + timeStr);
                const durMins = Math.round((outTime - inTime) / 60000);
                attendance[idx].duration = durMins > 0 ? durMins + ' min' : '—';
                ls('attendance', attendance);
                result.className = 'scan-result success';
                result.innerHTML =
                    `👋 CLOCKED OUT — ${student.fullName}<br>In: ${todayEntry.clockIn} | Out: ${timeStr} | Duration: ${attendance[idx].duration}`;
            } else {
                result.className = 'scan-result error';
                result.innerHTML = `ℹ️ ${student.fullName} already clocked in & out today.`;
            }

            result.style.display = 'block';
            if (input) input.value = '';
            renderAttendanceTable();
            renderQuickStudentPicker();
            updateAlertBadges();
            renderAdminStats();
            document.getElementById('attendanceBadge').textContent = (ls('attendance') || []).filter(a => a.date === fmtDate(
                new Date())).length;
        }

        function filterAttendance(type, el) {
            attendanceFilter = type;
            document.querySelectorAll('.att-tab').forEach(t => t.classList.remove('active'));
            if (el) el.classList.add('active');
            renderAttendanceTable();
        }

        function renderAttendanceTable() {
            let attendance = ls('attendance') || [];
            const search = (document.getElementById('searchAttendance')?.value || '').toLowerCase();
            const dateFilter = document.getElementById('attendanceDate')?.value || '';

            if (dateFilter) attendance = attendance.filter(a => a.date === dateFilter);
            if (search) attendance = attendance.filter(a => a.name.toLowerCase().includes(search) || a.regNo.toLowerCase()
                .includes(search));
            if (attendanceFilter === 'clocked-in') attendance = attendance.filter(a => a.status === 'clocked-in');
            if (attendanceFilter === 'clocked-out') attendance = attendance.filter(a => a.status === 'clocked-out');

            attendance = attendance.slice().reverse();
            const tbody = document.getElementById('attendanceTableBody');
            if (!attendance.length) {
                tbody.innerHTML =
                    '<tr><td colspan="8" style="text-align:center;color:var(--muted);padding:24px">No attendance records found</td></tr>';
                return;
            }
            tbody.innerHTML = attendance.map(a => `<tr>
                <td>${a.date}</td>
                <td><code style="font-size:11px">${a.regNo}</code></td>
                <td><b>${a.name}</b></td>
                <td>${a.branch}</td>
                <td style="color:var(--success);font-weight:600">${a.clockIn}</td>
                <td style="color:var(--muted)">${a.clockOut || '—'}</td>
                <td>${a.duration || (a.clockOut ? '—' : '<span style="color:var(--warning)">In session</span>')}</td>
                <td><span class="badge ${a.status==='clocked-in'?'badge-yellow':'badge-green'}">${a.status==='clocked-in'?'Present':'Complete'}</span></td>
            </tr>`).join('');
        }

        function exportAttendance() {
            const attendance = ls('attendance') || [];
            let csv = 'Date,Reg No,Name,Branch,Clock In,Clock Out,Duration,Status\n';
            attendance.forEach(a => { csv +=
                    `${a.date},${a.regNo},${a.name},${a.branch},${a.clockIn},${a.clockOut||''},${a.duration||''},${a.status}\n`
                    ; });
            const blob = new Blob([csv], { type: 'text/csv' });
            const url = URL.createObjectURL(blob);
            const link = document.createElement('a');
            link.href = url;
            link.download = 'attendance_' + fmtDate(new Date()) + '.csv';
            link.click();
        }

        function renderQuickStudentPicker(filter = '') {
            const students = ls('students') || [];
            const attendance = ls('attendance') || [];
            const todayStr = fmtDate(new Date());
            const picker = document.getElementById('quickStudentPicker');
            if (!picker) return;

            const filtered = filter ?
                students.filter(s => s.fullName.toLowerCase().includes(filter) || s.regNo.toLowerCase().includes(filter)) :
                students;

            picker.innerHTML = filtered.map(s => {
                const todayEntry = attendance.find(a => a.regNo === s.regNo && a.date === todayStr);
                const isIn = todayEntry && !todayEntry.clockOut;
                const isDone = todayEntry && todayEntry.clockOut;
                const color = isDone ? '#d1fae5' : isIn ? '#fef3c7' : 'rgba(255,255,255,0.12)';
                const textColor = isDone ? '#065f46' : isIn ? '#92400e' : 'rgba(255,255,255,0.9)';
                const label = isDone ? '✅ Done' : isIn ? '🟡 In' : '⬜ Clock In';
                return `<button onclick="quickClockStudent('${s.regNo}')" style="padding:6px 12px;border:none;border-radius:8px;background:${color};color:${textColor};font-size:12px;font-weight:600;cursor:pointer;transition:all 0.2s;text-align:left;line-height:1.3">
                    <div>${s.firstName} ${s.lastName[0]}.</div>
                    <div style="font-size:10px;opacity:0.75">${label}</div>
                </button>`;
            }).join('') || '<p style="color:rgba(255,255,255,0.4);font-size:12px;padding:8px;">No students found</p>';
        }

        function filterAttendanceDropdown() {
            const val = (document.getElementById('barcodeInput')?.value || '').toLowerCase();
            renderQuickStudentPicker(val);
        }

        function quickClockStudent(regNo) {
            document.getElementById('barcodeInput').value = regNo;
            processBarcode();
            renderQuickStudentPicker();
        }

        // =================== PAYMENTS ===================
        function renderPaymentsSection() {
            const payments = ls('payments') || [];
            const students = ls('students') || [];
            const total = payments.reduce((s, p) => s + (p.amount || 0), 0);
            const alerts = getAlertStudents();
            const expired = alerts.filter(s => getDaysLeft(s) < 0).length;
            const expiring = alerts.filter(s => { const d = getDaysLeft(s); return d >= 0 && d <= 5; }).length;

            const el = id => document.getElementById(id);
            if (el('totalCollected')) el('totalCollected').textContent = '₦' + total.toLocaleString();
            if (el('pendingTotal')) el('pendingTotal').textContent = students.filter(s => !s.paymentDate).length +
                ' students';
            if (el('expiredCount')) el('expiredCount').textContent = expired;
            if (el('expiringSoonCount')) el('expiringSoonCount').textContent = expiring;

            renderPaymentAlerts('paymentAlertsFull');

            const expiryList = document.getElementById('paymentExpiryList');
            if (expiryList) {
                const sorted = [...students].sort((a, b) => (getDaysLeft(a) || 9999) - (getDaysLeft(b) || 9999));
                expiryList.innerHTML = sorted.map(s => {
                    const days = getDaysLeft(s);
                    const exp = getExpiryDate(s);
                    const cls = days === null ? 'good' : days < 0 ? 'expired' : days <= 5 ? 'expiring' : 'good';
                    const planLabel = s.paymentPlan ? (s.paymentPlan === 'full' ? 'Full' : 'Half') : 'None';
                    return `<div class="expiry-row ${cls}">
                        <div class="expiry-days" style="color:${days===null?'var(--muted)':days<0?'var(--danger)':days<=5?'var(--warning)':'var(--success)'}">${days===null?'—':days < 0 ? Math.abs(days)+'d' : days+'d'}</div>
                        <div class="expiry-info">
                            <div class="name">${s.fullName} — <code style="font-size:10px">${s.regNo}</code></div>
                            <div class="detail">${s.branch} | Plan: ${planLabel} | Expiry: ${exp ? exp.toDateString() : '—'}</div>
                        </div>
                        <button class="action-btn pay-btn" onclick="quickRecordPayment('${s.id}')">Renew</button>
                    </div>`;
                }).join('');
            }

            const tbody = document.getElementById('paymentsTableBody');
            tbody.innerHTML = [...payments].reverse().map(p => {
                const s = students.find(st => st.regNo === p.regNo);
                const exp = s ? getExpiryDate(s) : null;
                const planLabel = p.plan ? (p.plan === 'full' ? 'Full' : 'Half') : 'Tuition';
                return `<tr>
                    <td>${p.date}</td>
                    <td>${p.student}</td>
                    <td><code style="font-size:11px">${p.regNo}</code></td>
                    <td>${planLabel}</td>
                    <td>₦${(p.amount||0).toLocaleString()}</td>
                    <td>${exp ? exp.toDateString() : '—'}</td>
                    <td><span class="badge badge-green">Paid</span></td>
                </tr>`;
            }).join('');
        }

        function recordPayment(e) {
            e.preventDefault();
            const regNo = document.getElementById('paymentStudentId').value.trim();
            const plan = document.getElementById('paymentPlanModal').value;
            const date = document.getElementById('paymentRecordDate').value;
            const students = ls('students') || [];
            const idx = students.findIndex(s => s.regNo === regNo);
            if (idx < 0) { alert('Student not found: ' + regNo); return; }
            const planData = getPlan(plan);
            if (!planData) { alert('Invalid payment plan'); return; }
            students[idx].paymentPlan = plan;
            students[idx].amountPaid = planData.amount;
            students[idx].paymentDate = date;
            ls('students', students);
            const payments = ls('payments') || [];
            payments.push({ id: 'PAY' + Date.now(), date, student: students[idx].fullName, regNo, plan, amount: planData
                    .amount, status: 'Paid' });
            ls('payments', payments);
            closeModal('recordPaymentModal');
            renderPaymentsSection();
            updateAlertBadges();
            alert('✅ Payment recorded! Expiry: ' + getExpiryDate(students[idx]).toDateString());
        }

        function setPaymentAmount() {
            const plan = document.getElementById('paymentPlanModal').value;
            const p = getPlan(plan);
            document.getElementById('paymentAmountModal').value = p ? p.amount : '';
        }

        function quickRecordPayment(studentId) {
            const s = (ls('students') || []).find(st => st.id === studentId);
            if (s) document.getElementById('paymentStudentId').value = s.regNo;
            showModal('recordPaymentModal');
        }

        // =================== PAYMENT SETTINGS ===================
        function loadPaymentSettings() {
            const plans = getPaymentPlans();
            document.getElementById('fullAmount').value = plans.full.amount;
            document.getElementById('fullDays').value = plans.full.days;
            document.getElementById('halfAmount').value = plans.half.amount;
            document.getElementById('halfDays').value = plans.half.days;
            document.getElementById('paymentSettingsStatus').innerHTML = '';
        }

        function savePaymentSettings(e) {
            e.preventDefault();
            const fullAmount = parseInt(document.getElementById('fullAmount').value);
            const fullDays = parseInt(document.getElementById('fullDays').value);
            const halfAmount = parseInt(document.getElementById('halfAmount').value);
            const halfDays = parseInt(document.getElementById('halfDays').value);

            if (fullAmount < 0 || fullDays < 1 || halfAmount < 0 || halfDays < 1) {
                alert('Please enter valid positive values.');
                return;
            }

            const plans = { full: { amount: fullAmount, days: fullDays }, half: { amount: halfAmount, days: halfDays } };
            ls('paymentPlans', plans);
            document.getElementById('paymentSettingsStatus').innerHTML =
                `<div style="color:var(--success);font-weight:600;">✅ Payment plans updated successfully!</div>`;
            updatePaymentPlanDropdowns();
        }

        function updatePaymentPlanDropdowns() {
            const plans = getPaymentPlans();
            const selects = document.querySelectorAll(
                'select[id$="paymentPlan"], select[id$="paymentPlanModal"], select[id$="editPaymentPlan"]');
            selects.forEach(sel => {
                const options = sel.querySelectorAll('option[value="full"], option[value="half"]');
                options.forEach(opt => {
                    if (opt.value === 'full') {
                        opt.textContent =
                            `Full Payment — ₦${plans.full.amount.toLocaleString()} (${plans.full.days} days)`;
                    } else if (opt.value === 'half') {
                        opt.textContent =
                            `Half Payment — ₦${plans.half.amount.toLocaleString()} (${plans.half.days} days)`;
                    }
                });
            });
        }

        // =================== CBT ===================
        function renderCBTQuestions() {
            const questions = ls('cbtQuestions') || {};
            const container = document.getElementById('cbtQuestionsContainer');
            let html = '';
            Object.keys(questions).forEach(subject => {
                html += `<div style="margin-bottom:16px"><h3 style="color:var(--blue);margin-bottom:8px">${subject} (${questions[subject].length} questions)</h3>`;
                html += `<div class="table-responsive"><table><thead><tr><th>Question</th><th>Options</th><th>Actions</th></tr></thead><tbody>`;
                questions[subject].forEach(q => {
                    html +=
                        `<tr><td style="max-width:200px;font-size:12px">${q.question}</td><td style="font-size:11px">${q.options.map((o,i)=>String.fromCharCode(65+i)+': '+o).join(' | ')}</td>
                        <td><button class="action-btn delete-btn" onclick="deleteCBTQuestion('${subject}','${q.id}')">Del</button></td></tr>`;
                });
                html += `</tbody></table></div></div>`;
            });
            container.innerHTML = html ||
                '<div class="empty-state"><div class="icon">📝</div><p>No questions yet. Add some!</p></div>';
        }

        function saveQuestion(e) {
            e.preventDefault();
            const questions = ls('cbtQuestions') || {};
            const subject = document.getElementById('cbtQSubject').value;
            if (!questions[subject]) questions[subject] = [];
            questions[subject].push({
                id: 'Q' + Date.now(),
                subject,
                question: document.getElementById('cbtQText').value,
                options: [document.getElementById('cbtQA').value, document.getElementById('cbtQB').value, document
                    .getElementById('cbtQC').value, document.getElementById('cbtQD').value
                ],
                correct: parseInt(document.getElementById('cbtQCorrect').value),
                explanation: document.getElementById('cbtQExplain').value
            });
            ls('cbtQuestions', questions);
            closeModal('cbtQuestionModal');
            renderCBTQuestions();
            document.getElementById('cbtQText').value = '';
        }

        function deleteCBTQuestion(subject, id) {
            if (!confirm('Delete this question?')) return;
            const questions = ls('cbtQuestions') || {};
            if (questions[subject]) questions[subject] = questions[subject].filter(q => q.id !== id);
            ls('cbtQuestions', questions);
            renderCBTQuestions();
        }

        // =================== TESTS ===================
        let activeTestId = null;

        function renderTestsTable() {
            const tests = ls('tests') || [];
            const testQuestions = ls('testQuestions') || {};
            const testResults = ls('testResults') || [];
            const tbody = document.getElementById('testsTableBody');
            if (!tests.length) {
                tbody.innerHTML =
                    '<tr><td colspan="9" style="text-align:center;color:var(--muted);padding:24px">No tests yet. Click "+ Create Test" to begin.</td></tr>';
                return;
            }
            tbody.innerHTML = tests.map(t => {
                const qCount = (testQuestions[t.id] || []).length;
                const submissions = testResults.filter(r => r.testId === t.id).length;
                const posted = t.resultsPosted ? '<span class="badge badge-green">Posted ✅</span>' :
                    '<span class="badge badge-gray">Not posted</span>';
                const statusColor = t.status === 'Active' ? 'badge-green' : t.status === 'Completed' ?
                    'badge-gray' : 'badge-yellow';
                return `<tr>
                    <td><b>${t.name}</b></td>
                    <td>${t.subject}</td>
                    <td><span class="badge badge-blue">${t.department}</span></td>
                    <td>${t.date}</td>
                    <td>${t.duration} min</td>
                    <td><b style="color:${qCount>0?'var(--success)':'var(--danger)'}">${qCount}</b></td>
                    <td><b style="color:var(--blue)">${submissions}</b> ${posted}</td>
                    <td><span class="badge ${statusColor}">${t.status}</span></td>
                    <td class="action-btns">
                        <button class="action-btn edit-btn" onclick="openTestQuestionPanel('${t.id}')">📝 Questions</button>
                        <button class="action-btn attend-btn" onclick="toggleTestStatus('${t.id}')">${t.status==='Active'?'Close':'Activate'}</button>
                        ${submissions > 0 ? `<button class="action-btn view-btn" onclick="openGradingPanel('${t.id}')">📊 Grades</button>` : ''}
                        <button class="action-btn delete-btn" onclick="deleteTest('${t.id}')">Del</button>
                    </td>
                </tr>`;
            }).join('');
        }

        function addTest(e) {
            e.preventDefault();
            const tests = ls('tests') || [];
            const newTest = {
                id: 'TEST' + Date.now(),
                name: document.getElementById('testName').value,
                subject: document.getElementById('testSubject').value,
                department: document.getElementById('testDepartment').value,
                date: document.getElementById('testDate').value,
                duration: parseInt(document.getElementById('testDuration').value),
                status: 'Upcoming',
                totalMarks: 0
            };
            tests.push(newTest);
            ls('tests', tests);
            closeModal('addTestModal');
            document.getElementById('addTestModal').querySelector('form').reset();
            renderTestsTable();
            openTestQuestionPanel(newTest.id);
        }

        function toggleTestStatus(id) {
            const tests = ls('tests') || [];
            const idx = tests.findIndex(t => t.id === id);
            if (idx < 0) return;
            const cycle = { Upcoming: 'Active', Active: 'Completed', Completed: 'Upcoming' };
            tests[idx].status = cycle[tests[idx].status] || 'Upcoming';
            ls('tests', tests);
            renderTestsTable();
        }

        function deleteTest(id) {
            if (!confirm('Delete this test and all its questions?')) return;
            ls('tests', (ls('tests') || []).filter(t => t.id !== id));
            const tq = ls('testQuestions') || {};
            delete tq[id];
            ls('testQuestions', tq);
            closeTestQuestionPanel();
            renderTestsTable();
        }

        function openTestQuestionPanel(testId) {
            activeTestId = testId;
            const tests = ls('tests') || [];
            const test = tests.find(t => t.id === testId);
            if (!test) return;
            document.getElementById('testQPanelName').textContent = test.name + ' — ' + test.subject;
            document.getElementById('testQuestionPanel').style.display = 'block';
            document.getElementById('addTestQuestionForm').style.display = 'none';
            renderTestQuestionsList(testId);
            document.getElementById('testQuestionPanel').scrollIntoView({ behavior: 'smooth' });
        }

        function closeTestQuestionPanel() {
            activeTestId = null;
            document.getElementById('testQuestionPanel').style.display = 'none';
        }

        function showAddTestQuestionForm() {
            document.getElementById('addTestQuestionForm').style.display = 'block';
            ['tqText', 'tqA', 'tqB', 'tqC', 'tqD', 'tqExplain'].forEach(id => { const el = document.getElementById(id);
                if (el) el.value = ''; });
            document.getElementById('tqCorrect').value = '0';
            document.getElementById('tqMarks').value = '1';
            document.getElementById('tqText').focus();
        }

        function saveTestQuestion() {
            const text = document.getElementById('tqText').value.trim();
            const a = document.getElementById('tqA').value.trim();
            const b = document.getElementById('tqB').value.trim();
            const c = document.getElementById('tqC').value.trim();
            const d = document.getElementById('tqD').value.trim();
            if (!text || !a || !b || !c || !d) { alert('Please fill in the question and all four options.'); return; }
            const tq = ls('testQuestions') || {};
            if (!tq[activeTestId]) tq[activeTestId] = [];
            tq[activeTestId].push({
                id: 'TQ' + Date.now(),
                question: text,
                options: [a, b, c, d],
                correct: parseInt(document.getElementById('tqCorrect').value),
                marks: parseInt(document.getElementById('tqMarks').value) || 1,
                explanation: document.getElementById('tqExplain').value.trim()
            });
            ls('testQuestions', tq);
            const tests = ls('tests') || [];
            const idx = tests.findIndex(t => t.id === activeTestId);
            if (idx >= 0) { tests[idx].totalMarks = tq[activeTestId].reduce((s, q) => s + (q.marks || 1), 0);
                ls('tests', tests); }
            document.getElementById('addTestQuestionForm').style.display = 'none';
            renderTestQuestionsList(activeTestId);
            renderTestsTable();
        }

        function renderTestQuestionsList(testId) {
            const tq = ls('testQuestions') || {};
            const questions = tq[testId] || [];
            const container = document.getElementById('testQuestionsList');
            if (!questions.length) {
                container.innerHTML =
                    '<div class="empty-state"><div class="icon">❓</div><p>No questions yet. Click "+ Add Question" to start.</p></div>';
                return;
            }
            container.innerHTML = questions.map((q, i) => `
                <div style="background:var(--surface);border:1.5px solid var(--border);border-radius:12px;padding:14px;margin-bottom:10px;">
                    <div style="display:flex;justify-content:space-between;align-items:flex-start;gap:10px;">
                        <div style="flex:1">
                            <div style="font-weight:700;font-size:14px;margin-bottom:8px;color:var(--text)">${i+1}. ${q.question}</div>
                            <div style="display:grid;grid-template-columns:1fr 1fr;gap:4px;">
                                ${q.options.map((opt, j) => `<div style="font-size:12px;padding:4px 8px;border-radius:6px;background:${j===q.correct?'#dcfce7':'white'};border:1px solid ${j===q.correct?'#86efac':'var(--border)'};">
                                    <b style="color:${j===q.correct?'var(--success)':'var(--blue)'}">${String.fromCharCode(65+j)}.</b> ${opt} ${j===q.correct?'✅':''}
                                </div>`).join('')}
                            </div>
                            ${q.explanation ? `<div style="margin-top:6px;font-size:11px;color:var(--blue);font-style:italic">💡 ${q.explanation}</div>` : ''}
                        </div>
                        <div style="display:flex;flex-direction:column;gap:4px;flex-shrink:0">
                            <span style="font-size:11px;font-weight:700;color:var(--success);background:#dcfce7;padding:2px 8px;border-radius:20px">${q.marks} mk${q.marks>1?'s':''}</span>
                            <button class="action-btn delete-btn" onclick="deleteTestQuestion('${testId}','${q.id}')">Del</button>
                        </div>
                    </div>
                </div>`).join('');
        }

        function deleteTestQuestion(testId, qId) {
            if (!confirm('Delete this question?')) return;
            const tq = ls('testQuestions') || {};
            if (tq[testId]) tq[testId] = tq[testId].filter(q => q.id !== qId);
            ls('testQuestions', tq);
            const tests = ls('tests') || [];
            const idx = tests.findIndex(t => t.id === testId);
            if (idx >= 0) { tests[idx].totalMarks = (tq[testId] || []).reduce((s, q) => s + (q.marks || 1), 0);
                ls('tests', tests); }
            renderTestQuestionsList(testId);
            renderTestsTable();
        }

        // =================== GRADING ===================
        function openGradingPanel(testId) {
            const tests = ls('tests') || [];
            const test = tests.find(t => t.id === testId);
            const testResults = ls('testResults') || [];
            const students = ls('students') || [];
            const results = testResults.filter(r => r.testId === testId);

            document.getElementById('gradingTestName').textContent = test.name + ' (' + test.subject + ')';
            document.getElementById('gradingPanel').style.display = 'block';
            document.getElementById('postResultsBtn').textContent = test.resultsPosted ? '🔄 Re-Post Results' :
                '📢 Post Results';
            activeTestId = testId;

            if (!results.length) {
                document.getElementById('gradingStats').innerHTML = '';
                document.getElementById('gradeDistribution').innerHTML = '';
                document.getElementById('gradingTableBody').innerHTML =
                    '<tr><td colspan="9" style="text-align:center;color:var(--muted);padding:24px">No submissions yet.</td></tr>';
                document.getElementById('gradingPanel').scrollIntoView({ behavior: 'smooth' });
                return;
            }

            const scores = results.map(r => r.percentage);
            const avg = Math.round(scores.reduce((a, b) => a + b, 0) / scores.length);
            const highest = Math.max(...scores);
            const passed = results.filter(r => r.percentage >= 50).length;

            document.getElementById('gradingStats').innerHTML = `
                <div class="stat-card"><div class="label">Total Submissions</div><div class="number">${results.length}</div></div>
                <div class="stat-card"><div class="label">Class Average</div><div class="number" style="color:var(--blue)">${avg}%</div></div>
                <div class="stat-card"><div class="label">Highest Score</div><div class="number" style="color:var(--success)">${highest}%</div></div>
                <div class="stat-card"><div class="label">Pass Rate (≥50%)</div><div class="number" style="color:${passed/results.length>=0.5?'var(--success)':'var(--danger)'}">${Math.round(passed/results.length*100)}%</div></div>
            `;

            const grades = { A: 0, B: 0, C: 0, D: 0, F: 0 };
            results.forEach(r => { grades[r.grade] = (grades[r.grade] || 0) + 1; });
            const gc = { A: 'var(--success)', B: '#0891b2', C: 'var(--blue)', D: 'var(--warning)', F: 'var(--danger)' };
            document.getElementById('gradeDistribution').innerHTML = `
                <div style="background:var(--surface);border-radius:12px;padding:14px;margin-bottom:14px">
                    <h4 style="margin-bottom:10px;color:var(--text);font-size:15px">📊 Grade Distribution</h4>
                    <div style="display:flex;gap:8px;flex-wrap:wrap">
                        ${Object.entries(grades).map(([g,c]) => `
                        <div style="flex:1;min-width:60px;text-align:center;padding:10px 6px;background:white;border-radius:8px;border:2px solid ${gc[g]}">
                            <div style="font-size:20px;font-weight:900;color:${gc[g]}">${g}</div>
                            <div style="font-size:18px;font-weight:800;color:var(--text)">${c}</div>
                            <div style="font-size:10px;color:var(--muted)">${results.length?Math.round(c/results.length*100):0}%</div>
                        </div>`).join('')}
                    </div>
                </div>`;

            const sorted = [...results].sort((a, b) => b.percentage - a.percentage);
            const badgeMap = { A: 'badge-green', B: 'badge-blue', C: 'badge-blue', D: 'badge-yellow', F: 'badge-red' };
            document.getElementById('gradingTableBody').innerHTML = sorted.map((r, i) => {
                const st = students.find(s => s.regNo === r.regNo);
                const mins = Math.floor(r.timeTaken / 60),
                    secs = r.timeTaken % 60;
                return `<tr style="animation:fadeIn 0.25s ease ${i*0.03}s both">
                    <td><code style="font-size:11px">${r.regNo}</code></td>
                    <td><b>${r.studentName}</b></td>
                    <td>${st ? st.branch : '—'}</td>
                    <td style="font-weight:700">${r.score}</td>
                    <td>${r.totalMarks}</td>
                    <td style="font-weight:900;color:${r.percentage>=70?'var(--success)':r.percentage>=50?'var(--warning)':'var(--danger)'}">${r.percentage}%</td>
                    <td><span class="badge ${badgeMap[r.grade]||'badge-gray'}">${r.grade}</span></td>
                    <td>${mins}m ${secs}s</td>
                    <td>${r.posted ? '<span style="color:var(--success);font-weight:700">✅ Visible</span>' : '<span style="color:var(--muted)">🔒 Hidden</span>'}</td>
                </tr>`;
            }).join('');
            document.getElementById('gradingPanel').scrollIntoView({ behavior: 'smooth' });
        }

        function postTestResults() {
            if (!activeTestId) return;
            const tests = ls('tests') || [];
            const idx = tests.findIndex(t => t.id === activeTestId);
            if (idx < 0) return;
            tests[idx].resultsPosted = true;
            tests[idx].status = 'Completed';
            ls('tests', tests);
            const testResults = ls('testResults') || [];
            testResults.forEach((r, i) => { if (r.testId === activeTestId) testResults[i].posted = true; });
            ls('testResults', testResults);
            alert('✅ Results posted! Students can now see their score, grade, and full answer review.');
            renderTestsTable();
            openGradingPanel(activeTestId);
        }

        // =================== MODAL HELPERS ===================
        function showModal(id) { document.getElementById(id).classList.add('active'); }

        function closeModal(id) { document.getElementById(id).classList.remove('active'); }

        document.addEventListener('click', function(e) {
            if (e.target.classList.contains('modal')) closeModal(e.target.id);
        });

        // =================== STUDENT DASHBOARD ===================
        function loadStudentDashboard() {
            const s = ls('currentStudent');
            if (!s) {
                document.getElementById('studentNameDisplay').textContent = 'Student';
                document.getElementById('studentInfoLine').textContent = 'Please login again.';
                return;
            }
            document.getElementById('studentNameDisplay').textContent = s.firstName;
            document.getElementById('studentInfoLine').textContent = s.regNo + ' | ' + s.branch + ' | ' + s.department;
            document.getElementById('studentAvatarTop').textContent = (s.firstName[0] + s.lastName[0]).toUpperCase();
            renderStudentStats(s);
            renderStudentActivities(s);
        }

        function renderStudentStats(s) {
            const days = getDaysLeft(s);
            const exp = getExpiryDate(s);
            const tests = ls('tests') || [];
            const results = ls('cbtResults') || [];
            const myResults = results.filter(r => r.regNo === s.regNo);
            const grid = document.getElementById('studentStatsGrid');
            grid.innerHTML = `
                <div class="stat-card"><div class="label">My Subjects</div><div class="number">${(s.subjects||[]).length}</div></div>
                <div class="stat-card"><div class="label">Upcoming Tests</div><div class="number" style="color:var(--warning)">${tests.filter(t=>t.status==='Upcoming').length}</div></div>
                <div class="stat-card" onclick="showStudent('payments')"><div class="label">Payment Expiry</div><div class="number" style="color:${days===null?'var(--muted)':days<0?'var(--danger)':days<=5?'var(--warning)':'var(--success)'}">${days===null?'—':days<0?'Expired':days+'d'}</div><div class="sub">${exp?exp.toDateString():''}</div></div>
                <div class="stat-card"><div class="label">CBT Practices</div><div class="number">${myResults.length}</div></div>
            `;
        }

        function renderStudentActivities(s) {
            const tbody = document.getElementById('studentActivitiesTable');
            const acts = s.activities || [];
            tbody.innerHTML = acts.length ? acts.map(a =>
                    `<tr><td>${a.date}</td><td>${a.activity}</td><td>${a.subject}</td><td>${a.score}</td><td><span class="badge badge-green">${a.status}</span></td></tr>`
                ).join('') :
                '<tr><td colspan="5" style="text-align:center;color:var(--muted);padding:20px">No activities yet</td></tr>';
        }

        function showStudent(section) {
            const sectionIds = {
                overview: 'studentOverview',
                profile: 'studentProfile',
                idcard: 'studentIDCardSection',
                subjects: 'studentSubjectsSection',
                tests: 'studentTestsSection',
                payments: 'studentPaymentsSection',
                cbt: 'studentCBTSection',
                cbtPractice: 'cbtPracticeInterface',
                cbtResults: 'cbtResultsInterface',
                testTake: 'studentTestInterface',
                testResult: 'studentTestResultInterface',
                postedResults: 'studentPostedResults'
            };
            document.querySelectorAll('#studentDashboard .content-section').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('#studentDashboard .menu-item').forEach(m => m.classList.remove('active'));
            const el = document.getElementById(sectionIds[section]);
            if (el) el.classList.add('active');
            const menuIdx = { overview: 0, profile: 1, idcard: 2, subjects: 3, tests: 4, payments: 5, cbt: 6 };
            const items = document.querySelectorAll('#studentDashboard .menu-item');
            if (menuIdx[section] !== undefined && items[menuIdx[section]]) items[menuIdx[section]].classList.add(
            'active');
            if (['testTake', 'testResult', 'postedResults'].includes(section) && items[4]) items[4].classList.add(
            'active');

            const s = ls('currentStudent');
            if (!s) return;
            if (section === 'idcard') renderIDCard(s);
            if (section === 'profile') renderStudentProfile(s);
            if (section === 'subjects') renderStudentSubjects(s);
            if (section === 'tests') { stopTestTimer();
                renderStudentTests(s); }
            if (section === 'payments') renderStudentPayments(s);
            if (section === 'cbt') renderStudentCBT(s);
            if (section === 'postedResults') renderPostedResults(s);

            if (window.innerWidth < 768) {
                const sidebar = document.getElementById('studentSidebar');
                if (sidebar) sidebar.classList.remove('open');
                const overlay = document.getElementById('studentSidebarOverlay');
                if (overlay) overlay.classList.remove('active');
            }
        }

        function renderStudentProfile(s) {
            document.getElementById('studentProfileContent').innerHTML = `
                <h2 style="margin-bottom:16px">My Profile</h2>
                <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;font-size:13px">
                    <div style="grid-column:1/-1;text-align:center;margin-bottom:6px">
                        <div style="width:80px;height:80px;border-radius:50%;margin:0 auto;border:3px solid var(--gold);overflow:hidden;background:var(--surface);display:flex;align-items:center;justify-content:center;font-size:28px;color:var(--muted)">
                            ${s.photo ? `<img src="${s.photo}" style="width:100%;height:100%;object-fit:cover">` : (s.firstName[0]+s.lastName[0]).toUpperCase()}
                        </div>
                    </div>
                    <div class="form-group"><label>Full Name</label><div style="padding:10px;background:var(--surface);border-radius:8px">${s.fullName}</div></div>
                    <div class="form-group"><label>Reg No.</label><div style="padding:10px;background:var(--surface);border-radius:8px"><code>${s.regNo}</code></div></div>
                    <div class="form-group"><label>Email</label><div style="padding:10px;background:var(--surface);border-radius:8px">${s.email}</div></div>
                    <div class="form-group"><label>Phone</label><div style="padding:10px;background:var(--surface);border-radius:8px">${s.phone}</div></div>
                    <div class="form-group"><label>Branch</label><div style="padding:10px;background:var(--surface);border-radius:8px">${s.branch}</div></div>
                    <div class="form-group"><label>Department</label><div style="padding:10px;background:var(--surface);border-radius:8px">${s.department}</div></div>
                    <div class="form-group"><label>Exam Type</label><div style="padding:10px;background:var(--surface);border-radius:8px"><span class="badge badge-blue">${s.examType || 'NECO'}</span></div></div>
                    <div class="form-group"><label>Date of Birth</label><div style="padding:10px;background:var(--surface);border-radius:8px">${s.dob}</div></div>
                    <div class="form-group"><label>Gender</label><div style="padding:10px;background:var(--surface);border-radius:8px">${s.gender}</div></div>
                    <div class="form-group" style="grid-column:1/-1"><label>Address</label><div style="padding:10px;background:var(--surface);border-radius:8px">${s.address}</div></div>
                    <div class="form-group"><label>Guardian</label><div style="padding:10px;background:var(--surface);border-radius:8px">${s.guardianName}</div></div>
                    <div class="form-group"><label>Guardian Phone</label><div style="padding:10px;background:var(--surface);border-radius:8px">${s.guardianPhone}</div></div>
                    <div class="form-group"><label>Login</label><div style="padding:10px;background:var(--surface);border-radius:8px">${s.username} / ${s.password}</div></div>
                    <div style="grid-column:1/-1;margin-top:8px;">
                        <button class="btn btn-primary" onclick="showModal('changePasswordModal')">🔑 Change Password</button>
                    </div>
                </div>`;
        }

        // =================== CHANGE PASSWORD ===================
        function changePassword(e) {
            e.preventDefault();
            const current = document.getElementById('currentPassword').value;
            const newPwd = document.getElementById('newPassword').value;
            const confirmPwd = document.getElementById('confirmPassword').value;

            const s = ls('currentStudent');
            if (!s) { alert('Please login first.'); return; }

            if (s.password !== current) {
                alert('❌ Current password is incorrect.');
                return;
            }
            if (newPwd.length < 4) {
                alert('❌ New password must be at least 4 characters.');
                return;
            }
            if (newPwd !== confirmPwd) {
                alert('❌ Passwords do not match.');
                return;
            }

            const students = ls('students') || [];
            const idx = students.findIndex(st => st.regNo === s.regNo);
            if (idx >= 0) {
                students[idx].password = newPwd;
                ls('students', students);
                s.password = newPwd;
                ls('currentStudent', s);
                alert('✅ Password changed successfully!');
                closeModal('changePasswordModal');
                document.getElementById('changePasswordModal').querySelector('form').reset();
            } else {
                alert('❌ Error updating password. Please contact admin.');
            }
        }

        // =================== STUDENT SUBJECTS ===================
        function renderStudentSubjects(s) {
            const grid = document.getElementById('studentSubjectsGrid');
            grid.innerHTML = (s.subjects || []).map(sub =>
                `<div class="subject-tag"><h4>${sub}</h4><p>${s.department}</p></div>`).join('');
        }

        // =================== STUDENT TESTS (with tracking) ===================
        function renderStudentTests(s) {
            const tests = ls('tests') || [];
            const testQuestions = ls('testQuestions') || {};
            const testResults = ls('testResults') || [];
            const tbody = document.getElementById('studentTestsList');
            const myTests = tests.filter(t => t.department === s.department || t.department === 'ALL');
            if (!myTests.length) {
                tbody.innerHTML =
                    '<tr><td colspan="7" style="text-align:center;color:var(--muted);padding:20px">No tests scheduled for your department yet.</td></tr>';
                return;
            }
            tbody.innerHTML = myTests.map(t => {
                const qCount = (testQuestions[t.id] || []).length;
                const myResult = testResults.find(r => r.testId === t.id && r.regNo === s.regNo);
                const canTake = t.status === 'Active' && qCount > 0 && !myResult;
                const statusColor = t.status === 'Active' ? 'badge-green' : t.status === 'Completed' ?
                    'badge-gray' : 'badge-yellow';
                let actionCell = '';
                if (myResult && myResult.posted === true) {
                    actionCell =
                        `<button class="btn btn-success" style="padding:6px 12px;font-size:12px" onclick="showStudent('postedResults')">📊 View Result</button>`;
                } else if (myResult && myResult.posted !== true) {
                    actionCell = `<span style="color:var(--warning);font-weight:600;font-size:12px">⏳ Awaiting publishing</span>`;
                } else if (canTake) {
                    actionCell =
                        `<button class="btn btn-primary" style="padding:6px 14px;font-size:12px" onclick="startTest('${t.id}')">▶ Take Test</button>`;
                } else {
                    actionCell =
                        `<span style="color:var(--muted);font-size:12px">${qCount===0?'No questions yet':t.status==='Upcoming'?'Not open yet':'Closed'}</span>`;
                }
                return `<tr>
                    <td><b>${t.name}</b></td><td>${t.subject}</td><td>${t.date}</td><td>${t.duration} min</td>
                    <td><b style="color:${qCount>0?'var(--success)':'var(--muted)'}">${qCount}</b></td>
                    <td><span class="badge ${statusColor}">${t.status}</span></td>
                    <td>${actionCell}</td>
                </tr>`;
            }).join('');
        }

        function renderPostedResults(s) {
            const testResults = ls('testResults') || [];
            const tests = ls('tests') || [];
            const myPosted = testResults.filter(r => r.regNo === s.regNo && r.posted === true);
            const container = document.getElementById('postedResultsContent');
            if (!myPosted.length) {
                container.innerHTML =
                    '<div class="empty-state"><div class="icon">📋</div><p>No results have been published yet. Check back after your teacher posts grades.</p></div>';
                return;
            }
            const gc = { A: 'var(--success)', B: '#0891b2', C: 'var(--blue)', D: 'var(--warning)', F: 'var(--danger)' };
            container.innerHTML = myPosted.map(r => {
                const mins = Math.floor(r.timeTaken / 60),
                    secs = r.timeTaken % 60;
                return `<div style="background:var(--surface);border-radius:14px;padding:20px;margin-bottom:16px;border:1.5px solid var(--border);animation:fadeIn 0.3s ease">
                    <div style="display:flex;justify-content:space-between;align-items:flex-start;flex-wrap:wrap;gap:10px;margin-bottom:14px">
                        <div><h3 style="font-size:17px;color:var(--text)">${r.testName}</h3><p style="color:var(--muted);font-size:12px;margin-top:2px">${r.subject} • ${r.date} • ${mins}m ${secs}s</p></div>
                        <div style="text-align:center;background:white;padding:12px 16px;border-radius:10px;border:2px solid ${gc[r.grade]||'var(--border)'}">
                            <div style="font-size:28px;font-weight:900;color:${gc[r.grade]}">${r.grade}</div>
                            <div style="font-size:18px;font-weight:800">${r.percentage}%</div>
                            <div style="font-size:11px;color:var(--muted)">${r.score}/${r.totalMarks} marks</div>
                        </div>
                    </div>
                    <hr style="border-color:var(--border);margin:12px 0">
                    <h4 style="font-size:14px;margin-bottom:8px">Answer Review</h4>
                    ${(r.review||[]).map((q,i)=>`
                    <div style="margin-bottom:8px;padding:10px;background:white;border-radius:8px;border-left:4px solid ${q.isCorrect?'var(--success)':'var(--danger)'}">
                        <div style="font-weight:600;font-size:13px;margin-bottom:6px">${i+1}. ${q.question} <span style="font-size:10px;color:var(--muted)">(${q.marks}mk)</span></div>
                        <div style="display:grid;grid-template-columns:1fr 1fr;gap:3px">
                            ${q.options.map((opt,j)=>`<div style="font-size:12px;padding:4px 8px;border-radius:5px;background:${j===q.correct?'#dcfce7':j===q.selected&&!q.isCorrect?'#fee2e2':'transparent'};border:1px solid ${j===q.correct?'#86efac':j===q.selected&&!q.isCorrect?'#fca5a5':'var(--border)'}">${String.fromCharCode(65+j)}. ${opt} ${j===q.correct?'✅':j===q.selected&&!q.isCorrect?'❌':''}</div>`).join('')}
                        </div>
                        ${q.explanation?`<div style="margin-top:4px;font-size:11px;color:var(--blue);font-style:italic">💡 ${q.explanation}</div>`:''}
                    </div>`).join('')}
                </div>`;
            }).join('');
        }

        function renderStudentPayments(s) {
            const days = getDaysLeft(s);
            const exp = getExpiryDate(s);
            const status = document.getElementById('studentPaymentStatus');
            if (status) {
                if (days === null) {
                    status.innerHTML =
                        '<div class="alert-panel" style="background:#fee2e2;border-color:var(--danger)"><h4 style="color:var(--danger)">⚠️ No active payment plan. Please contact admin.</h4></div>';
                } else if (days < 0) {
                    status.innerHTML =
                        `<div class="alert-panel" style="background:#fee2e2;border-color:var(--danger)"><h4 style="color:var(--danger)">🔴 Your plan has EXPIRED (${Math.abs(days)} days ago). Please renew at the front desk.</h4></div>`;
                } else if (days <= 5) {
                    status.innerHTML =
                        `<div class="alert-panel"><h4 style="color:var(--warning)">🟡 Your plan expires in ${days} day(s) (${exp.toDateString()}). Please renew soon.</h4></div>`;
                } else {
                    status.innerHTML =
                        `<div style="background:#f0fdf4;border:1.5px solid #86efac;border-radius:10px;padding:12px 16px;color:var(--success);font-weight:600">✅ Active Plan — ${days} day(s) remaining (expires ${exp.toDateString()})</div>`;
                }
            }
            const payments = (ls('payments') || []).filter(p => p.regNo === s.regNo);
            const tbody = document.getElementById('studentPaymentsList');
            tbody.innerHTML = payments.length ? [...payments].reverse().map(p => {
                const planData = getPlan(p.plan);
                const expD = planData ? new Date(new Date(p.date).setDate(new Date(p.date).getDate() + planData
                    .days)) : null;
                const planLabel = p.plan ? (p.plan === 'full' ? 'Full' : 'Half') : 'Tuition';
                return `<tr><td>${p.date}</td><td>${planLabel}</td><td>₦${(p.amount||0).toLocaleString()}</td><td>${expD ? expD.toDateString() : '—'}</td><td><span class="badge badge-green">Paid</span></td></tr>`;
            }).join('') :
                '<tr><td colspan="5" style="text-align:center;color:var(--muted);padding:20px">No payment records</td></tr>';
        }

        function renderStudentCBT(s) {
            const questions = ls('cbtQuestions') || {};
            const subjects = [...new Set([...Object.keys(questions), ...(s.subjects || [])])].filter(sub => questions[
                sub] && questions[sub].length > 0);
            const container = document.getElementById('cbtSubjectsList');
            container.innerHTML = subjects.length ?
                `<div class="subjects-grid">${subjects.map(sub => `<div class="subject-tag" onclick="startCBT('${sub}')"><h4>${sub}</h4><p>${(questions[sub]||[]).length} questions</p><p style="color:var(--blue);margin-top:4px;font-size:12px;font-weight:600">▶ Start Practice</p></div>`).join('')}</div>` :
                '<div class="empty-state"><div class="icon">📚</div><p>No practice questions available yet.</p></div>';
        }

        // =================== CBT PRACTICE ===================
        let cbtState = {};

        function startCBT(subject) {
            const allQ = ls('cbtQuestions') || {};
            const questions = allQ[subject] || [];
            if (!questions.length) { alert('No questions for ' + subject); return; }
            cbtState = { subject, questions, current: 0, answers: [], startTime: Date.now() };
            showStudent('cbtPractice');
            document.getElementById('cbtSubjectName').textContent = subject;
            renderCBTQuestion();
        }

        function renderCBTQuestion() {
            const { questions, current, answers } = cbtState;
            if (!questions.length) return;
            document.getElementById('cbtProgress').textContent = `Question ${current + 1} of ${questions.length}`;
            const q = questions[current];
            document.getElementById('cbtQuestion').textContent = q.question;
            document.getElementById('cbtOptions').innerHTML = q.options.map((opt, i) => `
                <div class="cbt-option ${answers[current] === i ? 'selected' : ''}" onclick="selectAnswer(${i})">
                    <span style="font-weight:700;color:var(--blue);min-width:20px">${String.fromCharCode(65+i)}.</span> ${opt}
                </div>`).join('');
            document.getElementById('prevBtn').disabled = current === 0;
            document.getElementById('nextBtn').textContent = current === questions.length - 1 ? 'Submit ✓' : 'Next →';
        }

        function selectAnswer(i) { cbtState.answers[cbtState.current] = i;
            renderCBTQuestion(); }

        function prevQuestion() { if (cbtState.current > 0) { cbtState.current--;
                renderCBTQuestion(); } }

        function nextQuestion() {
            const { questions, current } = cbtState;
            if (current < questions.length - 1) { cbtState.current++;
                renderCBTQuestion(); } else { submitCBT(); }
        }

        function submitCBT() {
            const { subject, questions, answers, startTime } = cbtState;
            let score = 0;
            const review = questions.map((q, i) => {
                const correct = q.correct;
                const selected = answers[i];
                if (selected === correct) score++;
                return { question: q.question, options: q.options, correct, selected, explanation: q.explanation };
            });
            const pct = Math.round((score / questions.length) * 100);
            const s = ls('currentStudent');
            const results = ls('cbtResults') || [];
            const result = { id: 'RES' + Date.now(), regNo: s?.regNo, subject, score, total: questions.length,
                percentage: pct, date: fmtDate(new Date()), timeTaken: Math.round((Date.now() - startTime) / 1000) };
            results.push(result);
            ls('cbtResults', results);

            showStudent('cbtResults');
            document.getElementById('cbtResultsContent').innerHTML = `
                <div style="text-align:center;padding:20px 0">
                    <div style="font-size:48px;margin-bottom:6px">${pct >= 75 ? '🏆' : pct >= 50 ? '📈' : '📚'}</div>
                    <div style="font-size:36px;font-weight:900;color:${pct>=75?'var(--success)':pct>=50?'var(--warning)':'var(--danger)'}">${pct}%</div>
                    <div style="color:var(--muted);font-size:14px;margin-top:4px">${score} / ${questions.length} correct — ${subject}</div>
                </div>
                <hr style="margin:16px 0;border-color:var(--border)">
                ${review.map((r, i) => `
                <div style="margin-bottom:14px;padding:12px;background:${r.selected===r.correct?'#f0fdf4':'#fff7ed'};border-radius:10px;border:1.5px solid ${r.selected===r.correct?'#86efac':'#fed7aa'}">
                    <div style="font-weight:700;margin-bottom:8px;font-size:13px">${i+1}. ${r.question}</div>
                    ${r.options.map((opt, j) => `<div style="font-size:12px;padding:3px 8px;border-radius:5px;margin:2px 0;background:${j===r.correct?'#dcfce7':j===r.selected&&j!==r.correct?'#fee2e2':'transparent'}">${String.fromCharCode(65+j)}. ${opt} ${j===r.correct?'✅':j===r.selected&&j!==r.correct?'❌':''}</div>`).join('')}
                    ${r.explanation ? `<div style="margin-top:6px;font-size:11px;color:var(--blue);font-style:italic">💡 ${r.explanation}</div>` : ''}
                </div>`).join('')}`;
        }

        // =================== TEST-TAKING (STUDENT) ===================
        let testState = {};
        let testTimerInterval = null;

        function startTest(testId) {
            const tests = ls('tests') || [];
            const test = tests.find(t => t.id === testId);
            const tq = ls('testQuestions') || {};
            const questions = tq[testId] || [];
            if (!questions.length) { alert('No questions available for this test yet.'); return; }
            if (!confirm(`Start "${test.name}"?\n\n⏱ Duration: ${test.duration} minutes\n📝 Questions: ${questions.length}\n\nOnce started, the timer cannot be paused.`))
                return;

            testState = {
                testId,
                test,
                questions,
                current: 0,
                answers: new Array(questions.length).fill(null),
                startTime: Date.now(),
                secondsLeft: test.duration * 60
            };

            document.getElementById('activeTestName').textContent = test.name;
            document.getElementById('activeTestMeta').textContent = test.subject + ' | ' + test.department + ' | ' + test
                .duration + ' mins | ' + questions.length + ' questions';

            showStudent('testTake');
            renderTestQuestion();
            startTestTimer();
        }

        function renderTestQuestion() {
            const { questions, current, answers } = testState;
            const q = questions[current];
            document.getElementById('testProgress').textContent =
                `Question ${current + 1} of ${questions.length}  •  Marks: ${q.marks || 1}`;
            document.getElementById('testQuestionText').textContent = q.question;
            document.getElementById('testOptionsContainer').innerHTML = q.options.map((opt, i) => `
                <div class="cbt-option ${answers[current] === i ? 'selected' : ''}" onclick="selectTestAnswer(${i})" style="cursor:pointer">
                    <span style="font-weight:700;color:var(--blue);min-width:22px;font-size:15px">${String.fromCharCode(65+i)}.</span>
                    <span style="font-size:14px">${opt}</span>
                </div>`).join('');
            document.getElementById('testPrevBtn').disabled = current === 0;
            document.getElementById('testNextBtn').textContent = current === questions.length - 1 ? '✓ Submit Test' :
                'Next →';
        }

        function selectTestAnswer(i) {
            testState.answers[testState.current] = i;
            renderTestQuestion();
        }

        function testNav(dir) {
            const { questions, current } = testState;
            if (dir === 1 && current === questions.length - 1) { submitTest(); return; }
            const next = current + dir;
            if (next >= 0 && next < questions.length) {
                testState.current = next;
                renderTestQuestion();
            }
        }

        function startTestTimer() {
            stopTestTimer();
            testTimerInterval = setInterval(() => {
                testState.secondsLeft--;
                const el = document.getElementById('testCountdown');
                if (!el) { stopTestTimer(); return; }
                const m = Math.floor(testState.secondsLeft / 60);
                const s = testState.secondsLeft % 60;
                el.textContent = `${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
                el.style.background = testState.secondsLeft <= 60 ? 'var(--warning)' : 'var(--danger)';
                if (testState.secondsLeft <= 0) { stopTestTimer();
                    submitTest(true); }
            }, 1000);
        }

        function stopTestTimer() {
            if (testTimerInterval) { clearInterval(testTimerInterval);
                testTimerInterval = null; }
            const el = document.getElementById('testCountdown');
            if (el) el.textContent = '';
        }

        function confirmExitTest() {
            if (confirm('Exit this test? Your answers will NOT be saved.')) {
                stopTestTimer();
                showStudent('tests');
            }
        }

        function submitTest(autoSubmit = false) {
            if (!autoSubmit && !confirm('Submit your test now? You cannot change answers after submission.')) return;
            stopTestTimer();

            const { testId, test, questions, answers, startTime } = testState;
            let score = 0,
                totalMarks = 0;
            const review = questions.map((q, i) => {
                totalMarks += (q.marks || 1);
                const selected = answers[i];
                const isCorrect = selected === q.correct;
                if (isCorrect) score += (q.marks || 1);
                return { question: q.question, options: q.options, correct: q.correct, selected, isCorrect,
                    explanation: q.explanation, marks: q.marks || 1 };
            });

            const pct = Math.round((score / totalMarks) * 100);
            const grade = pct >= 80 ? 'A' : pct >= 70 ? 'B' : pct >= 60 ? 'C' : pct >= 50 ? 'D' : 'F';
            const timeTaken = Math.round((Date.now() - startTime) / 1000);
            const s = ls('currentStudent');

            const testResults = ls('testResults') || [];
            testResults.push({ id: 'TR' + Date.now(), testId, testName: test.name, subject: test.subject, regNo: s
                    .regNo, studentName: s.fullName, score, totalMarks, percentage: pct, grade, timeTaken,
                date: fmtDate(new Date()), review, posted: false
            });
            ls('testResults', testResults);

            const students = ls('students') || [];
            const si = students.findIndex(st => st.regNo === s.regNo);
            if (si >= 0) {
                if (!students[si].activities) students[si].activities = [];
                students[si].activities.push({ date: fmtDate(new Date()), activity: test.name, subject: test.subject,
                    score: pct + '%', status: 'Awaiting publishing' });
                ls('students', students);
                ls('currentStudent', students[si]);
            }

            showStudent('testResult');
            document.getElementById('testResultContent').innerHTML = `
                <div style="text-align:center;padding:20px 0 14px">
                    <div style="font-size:52px;margin-bottom:4px">${pct>=70?'🏆':pct>=50?'📈':'📚'}</div>
                    <div style="font-size:38px;font-weight:900;color:${pct>=70?'var(--success)':pct>=50?'var(--warning)':'var(--danger)'}">${pct}%</div>
                    <div style="font-size:18px;font-weight:700;color:var(--text);margin-top:4px">Grade: ${grade}</div>
                    <div style="color:var(--muted);font-size:14px;margin-top:6px">${score} / ${totalMarks} marks • ${test.name}</div>
                    <div style="color:var(--muted);font-size:12px;margin-top:3px">Time taken: ${Math.floor(timeTaken/60)}m ${timeTaken%60}s</div>
                    <div style="margin-top:12px;padding:10px;background:#fef3c7;border-radius:8px;color:var(--warning);font-weight:600;font-size:14px;">⏳ Results are pending admin approval. Check back after publishing.</div>
                </div>
                <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(120px,1fr));gap:10px;margin:14px 0;padding:14px;background:var(--surface);border-radius:10px">
                    <div style="text-align:center"><div style="font-size:20px;font-weight:800;color:var(--success)">${review.filter(r=>r.isCorrect).length}</div><div style="font-size:11px;color:var(--muted)">Correct</div></div>
                    <div style="text-align:center"><div style="font-size:20px;font-weight:800;color:var(--danger)">${review.filter(r=>!r.isCorrect).length}</div><div style="font-size:11px;color:var(--muted)">Wrong</div></div>
                    <div style="text-align:center"><div style="font-size:20px;font-weight:800;color:var(--muted)">${review.filter(r=>r.selected===null).length}</div><div style="font-size:11px;color:var(--muted)">Skipped</div></div>
                    <div style="text-align:center"><div style="font-size:20px;font-weight:800;color:var(--blue)">${totalMarks}</div><div style="font-size:11px;color:var(--muted)">Total Marks</div></div>
                </div>
                <h3 style="margin:16px 0 12px;font-size:16px">Answer Review</h3>
                ${review.map((r, i) => `
                <div style="margin-bottom:12px;padding:12px;background:${r.isCorrect?'#f0fdf4':'#fff7ed'};border-radius:10px;border:1.5px solid ${r.isCorrect?'#86efac':'#fed7aa'}">
                    <div style="font-weight:700;font-size:13px;margin-bottom:8px">${i+1}. ${r.question} <span style="font-size:11px;font-weight:600;color:var(--muted)">(${r.marks} mk${r.marks>1?'s':''})</span></div>
                    <div style="display:grid;grid-template-columns:1fr 1fr;gap:4px">
                        ${r.options.map((opt, j) => `<div style="font-size:12px;padding:4px 8px;border-radius:5px;background:${j===r.correct?'#dcfce7':j===r.selected&&!r.isCorrect?'#fee2e2':'white'};border:1px solid ${j===r.correct?'#86efac':j===r.selected&&!r.isCorrect?'#fca5a5':'var(--border)'}">
                            <b>${String.fromCharCode(65+j)}.</b> ${opt} ${j===r.correct?'✅':j===r.selected&&!r.isCorrect?'❌':''}
                        </div>`).join('')}
                    </div>
                    ${r.explanation ? `<div style="margin-top:6px;font-size:11px;color:var(--blue);font-style:italic">💡 ${r.explanation}</div>` : ''}
                </div>`).join('')}`;
        }

        // =================== CAMERA SCANNER ===================
        let codeReader = null;
        let scannerActive = false;
        let currentCameraId = null;

        async function startCameraScanner() {
            const startBtn = document.getElementById('startScanBtn');
            const stopBtn = document.getElementById('stopScanBtn');
            const viewport = document.getElementById('scannerViewport');
            const camSelect = document.getElementById('cameraSelect');
            const result = document.getElementById('scanResult');
            result.style.display = 'none';

            if (typeof ZXing === 'undefined') {
                result.className = 'scan-result error';
                result.style.display = 'block';
                result.innerHTML = '⚠️ Scanner library not loaded. Check internet connection and refresh.';
                return;
            }
            try {
                codeReader = new ZXing.BrowserMultiFormatReader();
                const cameras = await codeReader.listVideoInputDevices();
                if (!cameras.length) {
                    result.className = 'scan-result error';
                    result.style.display = 'block';
                    result.innerHTML = '📵 No camera detected. Use manual input or student picker below.';
                    return;
                }
                if (cameras.length > 1) {
                    camSelect.innerHTML = cameras.map((c, i) =>
                        `<option value="${c.deviceId}">${c.label||'Camera '+(i+1)}</option>`).join('');
                    camSelect.style.display = 'block';
                }
                viewport.style.display = 'block';
                startBtn.style.display = 'none';
                stopBtn.style.display = 'block';
                scannerActive = true;

                const preferred = cameras.find(c => c.label.toLowerCase().includes('back') || c.label.toLowerCase()
                    .includes('rear')) || cameras[cameras.length - 1];
                currentCameraId = preferred.deviceId;
                await codeReader.decodeFromVideoDevice(currentCameraId, document.getElementById('scannerVideo'), (
                res, err) => {
                    if (res && scannerActive) {
                        const code = res.getText();
                        const line = document.getElementById('scanLine');
                        if (line) { line.style.background =
                                'linear-gradient(90deg,transparent,#00ff88,transparent)';
                            setTimeout(() => { if (line) line.style.background =
                                    'linear-gradient(90deg,transparent,var(--gold),transparent)'; },
                            400); }
                        document.getElementById('barcodeInput').value = code;
                        processBarcode();
                        scannerActive = false;
                        setTimeout(() => { scannerActive = true; }, 2500);
                    }
                });
            } catch (err) {
                result.className = 'scan-result error';
                result.style.display = 'block';
                result.innerHTML = '❌ Camera error: ' + (err.message ||
                    'Permission denied. Allow camera access then try again.');
                stopCameraScanner();
            }
        }

        function stopCameraScanner() {
            if (codeReader) { try { codeReader.reset(); } catch (e) {} codeReader = null; }
            scannerActive = false;
            document.getElementById('scannerViewport').style.display = 'none';
            document.getElementById('stopScanBtn').style.display = 'none';
            document.getElementById('cameraSelect').style.display = 'none';
            document.getElementById('startScanBtn').style.display = 'block';
        }

        function switchCamera() {
            const select = document.getElementById('cameraSelect');
            const newId = select.value;
            if (!newId || newId === currentCameraId) return;
            currentCameraId = newId;
            if (codeReader) { codeReader.reset(); }
            if (currentCameraId) {
                codeReader.decodeFromVideoDevice(currentCameraId, document.getElementById('scannerVideo'), (res,
                err) => {
                    if (res && scannerActive) {
                        document.getElementById('barcodeInput').value = res.getText();
                        processBarcode();
                        scannerActive = false;
                        setTimeout(() => { scannerActive = true; }, 2500);
                    }
                });
            }
        }

        // =================== INIT ===================
        initData();
        document.getElementById('adminDateLine') && (document.getElementById('adminDateLine').textContent = new Date()
            .toDateString());

        updatePaymentPlanDropdowns();

        (function ensureShowAdmin() {
            const original = window.showAdmin;
            if (original) {
                window.showAdmin = function(section) {
                    original(section);
                    if (section === 'attendance') {
                        setTimeout(() => {
                            renderQuickStudentPicker();
                            const bi = document.getElementById('barcodeInput');
                            if (bi) bi.focus();
                        }, 120);
                    }
                    if (section === 'paymentSettings') loadPaymentSettings();
                };
            }
        })();

        console.log('✅ Testimony Tutors system initialized.');
    </script>
</body>
</html>
