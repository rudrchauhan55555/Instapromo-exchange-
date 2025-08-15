<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>InstaPromo Exchange</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-firestore-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-storage-compat.js"></script>
    <style>
        /* Modern CSS with Animations */
        :root {
            --primary: #6c5ce7;
            --secondary: #a29bfe;
            --accent: #fd79a8;
            --dark: #2d3436;
            --light: #f5f6fa;
            --success: #00b894;
            --warning: #fdcb6e;
            --danger: #d63031;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', sans-serif;
        }
        
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap');
        
        body {
            background-color: var(--light);
            color: var(--dark);
            line-height: 1.6;
        }
        
        header {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            padding: 1rem 2rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 4px 20px rgba(108, 92, 231, 0.3);
            position: sticky;
            top: 0;
            z-index: 100;
            animation: fadeInDown 0.8s;
        }
        
        .logo {
            font-size: 1.8rem;
            font-weight: bold;
            display: flex;
            align-items: center;
        }
        
        .logo i {
            margin-right: 10px;
            font-size: 1.5rem;
        }
        
        nav ul {
            display: flex;
            list-style: none;
        }
        
        nav ul li {
            margin-left: 1.5rem;
            position: relative;
        }
        
        nav ul li a {
            color: white;
            text-decoration: none;
            font-weight: 500;
            transition: all 0.3s ease;
            padding: 0.5rem 0;
        }
        
        nav ul li a:hover {
            opacity: 0.8;
        }
        
        nav ul li a::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 0;
            height: 2px;
            background-color: white;
            transition: width 0.3s ease;
        }
        
        nav ul li a:hover::after {
            width: 100%;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 2rem;
            min-height: calc(100vh - 200px);
        }
        
        .page {
            display: none;
        }
        
        .page.active {
            display: block;
            animation: fadeIn 0.5s;
        }

        .profile-page.active {
            display: block !important;
        }
        
        /* Home Page Styles */
        .hero {
            text-align: center;
            padding: 5rem 0 3rem;
            position: relative;
            overflow: hidden;
        }
        
        .hero::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(108, 92, 231, 0.1) 0%, rgba(255,255,255,0) 70%);
            z-index: -1;
            animation: pulse 8s infinite alternate;
        }
        
        .hero h1 {
            font-size: 3rem;
            margin-bottom: 1.5rem;
            color: var(--dark);
            animation: fadeInUp 0.8s 0.2s both;
        }
        
        .hero p {
            font-size: 1.2rem;
            margin-bottom: 2.5rem;
            color: #666;
            max-width: 700px;
            margin-left: auto;
            margin-right: auto;
            animation: fadeInUp 0.8s 0.4s both;
        }
        
        .btn {
            display: inline-block;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            padding: 0.8rem 2rem;
            border-radius: 30px;
            text-decoration: none;
            font-weight: 600;
            margin: 0 0.5rem;
            border: none;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(108, 92, 231, 0.4);
            animation: fadeInUp 0.8s 0.6s both;
        }
        
        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(108, 92, 231, 0.5);
        }
        
        .btn-outline {
            background: transparent;
            border: 2px solid var(--primary);
            color: var(--primary);
            box-shadow: none;
        }
        
        .btn-outline:hover {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
        }
        
        .how-it-works {
            display: flex;
            justify-content: space-between;
            margin: 5rem 0;
            flex-wrap: wrap;
        }
        
        .step {
            flex: 1;
            min-width: 250px;
            padding: 2rem;
            text-align: center;
            background: white;
            border-radius: 15px;
            margin: 1rem;
            box-shadow: 0 5px 25px rgba(0,0,0,0.05);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
            z-index: 1;
        }
        
        .step:hover {
            transform: translateY(-10px);
            box-shadow: 0 15px 30px rgba(108, 92, 231, 0.1);
        }
        
        .step::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(90deg, var(--primary), var(--accent));
        }
        
        .step i {
            font-size: 2.5rem;
            color: var(--primary);
            margin-bottom: 1.5rem;
            background: linear-gradient(135deg, var(--primary), var(--accent));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        .step h3 {
            margin: 1rem 0;
            color: var(--dark);
            font-size: 1.3rem;
        }
        
        .step p {
            color: #666;
            font-size: 0.95rem;
        }
        
        /* About Us Page Styles */
        .about-content {
            display: flex;
            align-items: center;
            gap: 3rem;
            margin-bottom: 3rem;
        }
        
        .about-text {
            flex: 1;
        }
        
        .about-image {
            flex: 1;
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
        }
        
        .about-image img {
            width: 100%;
            height: auto;
            display: block;
        }
        
        .team {
            display: flex;
            flex-wrap: wrap;
            gap: 2rem;
            justify-content: center;
        }
        
        .team-member {
            flex: 1;
            min-width: 250px;
            max-width: 300px;
            background: white;
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 5px 25px rgba(0,0,0,0.05);
            text-align: center;
        }
        
        .team-member img {
            width: 150px;
            height: 150px;
            border-radius: 50%;
            object-fit: cover;
            margin: 0 auto 1rem;
            border: 5px solid var(--light);
        }
        
        /* How It Works Page Styles */
        .steps {
            display: flex;
            flex-wrap: wrap;
            gap: 2rem;
            justify-content: center;
        }
        
        .step-card {
            flex: 1;
            min-width: 250px;
            max-width: 350px;
            background: white;
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 5px 25px rgba(0,0,0,0.05);
            text-align: center;
            transition: all 0.3s ease;
        }
        
        .step-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 15px 30px rgba(108, 92, 231, 0.1);
        }
        
        .step-number {
            display: inline-block;
            width: 50px;
            height: 50px;
            background: linear-gradient(135deg, var(--primary), var(--accent));
            color: white;
            border-radius: 50%;
            line-height: 50px;
            font-size: 1.5rem;
            font-weight: bold;
            margin-bottom: 1rem;
        }
        
        /* Brands Page Styles */
        .brand-list {
            display: flex;
            flex-wrap: wrap;
            gap: 2rem;
            justify-content: center;
        }
        
        .brand-item {
            flex: 1;
            min-width: 200px;
            max-width: 250px;
            background: white;
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.05);
            display: flex;
            justify-content: center;
            align-items: center;
            transition: all 0.3s ease;
        }
        
        .brand-item:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(108, 92, 231, 0.1);
        }
        
        .brand-item img {
            max-width: 100%;
            height: auto;
            filter: grayscale(100%);
            opacity: 0.7;
            transition: all 0.3s ease;
        }
        
        .brand-item:hover img {
            filter: grayscale(0%);
            opacity: 1;
        }
        
        /* FAQ Page Styles */
        .faq-item {
            background: white;
            margin-bottom: 1rem;
            border-radius: 10px;
            box-shadow: 0 3px 15px rgba(0,0,0,0.05);
            overflow: hidden;
        }
        
        .faq-question {
            padding: 1.5rem;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            transition: all 0.3s ease;
        }
        
        .faq-question:hover {
            background-color: #f9f9f9;
        }
        
        .faq-question i {
            transition: transform 0.3s ease;
        }
        
        .faq-answer {
            padding: 0 1.5rem;
            max-height: 0;
            overflow: hidden;
            transition: all 0.3s ease;
        }
        
        .faq-item.active .faq-question {
            background-color: rgba(108, 92, 231, 0.05);
        }
        
        .faq-item.active .faq-question i {
            transform: rotate(180deg);
        }
        
        .faq-item.active .faq-answer {
            padding: 0 1.5rem 1.5rem;
            max-height: 500px;
        }
        
        /* Contact Page Styles */
        .contact-container {
            display: flex;
            gap: 3rem;
            flex-wrap: wrap;
        }
        
        .contact-info {
            flex: 1;
            min-width: 300px;
        }
        
        .contact-card {
            background: white;
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 5px 25px rgba(0,0,0,0.05);
            margin-bottom: 2rem;
        }
        
        .contact-card h3 {
            margin-bottom: 1.5rem;
            color: var(--primary);
        }
        
        .contact-detail {
            display: flex;
            align-items: center;
            margin-bottom: 1rem;
        }
        
        .contact-detail i {
            width: 40px;
            height: 40px;
            background: rgba(108, 92, 231, 0.1);
            color: var(--primary);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 1rem;
        }
        
        .contact-form {
            flex: 1;
            min-width: 300px;
            background: white;
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 5px 25px rgba(0,0,0,0.05);
        }
        
        /* Updated Profile Page Styles */
        .profile-page {
            display: none;
            animation: fadeIn 0.5s;
        }
        
        .profile-header {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            padding: 2.5rem;
            border-radius: 15px;
            margin-bottom: 2rem;
            position: relative;
            overflow: hidden;
        }
        
        .profile-header::after {
            content: '';
            position: absolute;
            top: -50%;
            right: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,255,255,0.1) 0%, rgba(255,255,255,0) 70%);
            z-index: 0;
        }
        
        .profile-header-content {
            position: relative;
            z-index: 1;
        }
        
        .profile-header h1 {
            font-size: 2rem;
            margin-bottom: 0.5rem;
        }
        
        .profile-header p {
            font-size: 1.2rem;
            opacity: 0.9;
        }
        
        .stats {
            display: flex;
            justify-content: space-between;
            margin: 2.5rem 0;
            flex-wrap: wrap;
        }
        
        .stat-card {
            flex: 1;
            min-width: 200px;
            background: white;
            padding: 1.5rem;
            border-radius: 15px;
            margin: 1rem;
            text-align: center;
            box-shadow: 0 5px 25px rgba(0,0,0,0.05);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .stat-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 30px rgba(108, 92, 231, 0.1);
        }
        
        .stat-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(90deg, var(--primary), var(--accent));
        }
        
        .stat-card h3 {
            color: var(--primary);
            margin-bottom: 0.5rem;
            font-size: 1.1rem;
        }
        
        .stat-card p {
            font-size: 2rem;
            font-weight: 700;
            margin-bottom: 0.5rem;
        }
        
        .stat-details {
            display: flex;
            justify-content: space-around;
            font-size: 0.8rem;
            color: #666;
        }
        
        .available-balance {
            display: flex;
            gap: 1rem;
            margin: 2rem 0;
        }
        
        .balance-card {
            flex: 1;
            background: white;
            padding: 1.5rem;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.05);
            text-align: center;
        }
        
        .balance-card h3 {
            font-size: 1.1rem;
            margin-bottom: 0.5rem;
            color: var(--primary);
        }
        
        .balance-amount, .withdrawn-amount {
            font-size: 2rem;
            font-weight: 700;
            margin-bottom: 0.5rem;
        }
        
        .balance-status {
            font-size: 0.9rem;
            color: #666;
        }
        
        .withdrawal-form {
            background: white;
            padding: 2rem;
            border-radius: 15px;
            margin: 2.5rem 0;
            box-shadow: 0 5px 25px rgba(0,0,0,0.05);
        }
        
        .withdrawal-form h2 {
            margin-bottom: 1.5rem;
            color: var(--dark);
            position: relative;
            padding-bottom: 0.5rem;
        }
        
        .withdrawal-form h2::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 50px;
            height: 3px;
            background: linear-gradient(90deg, var(--primary), var(--accent));
            border-radius: 3px;
        }
        
        .payment-methods {
            display: flex;
            margin: 1.5rem 0;
            flex-wrap: wrap;
        }
        
        .payment-method {
            margin-right: 1.5rem;
            margin-bottom: 1rem;
            display: flex;
            align-items: center;
        }
        
        .payment-method input {
            margin-right: 0.5rem;
            cursor: pointer;
        }
        
        .payment-method label {
            cursor: pointer;
        }
        
        .payment-details {
            display: none;
            margin-top: 1.5rem;
            animation: fadeIn 0.5s;
        }
        
        .payment-details.active {
            display: block;
        }
        
        .payment-details h4 {
            margin-bottom: 1rem;
            color: var(--dark);
        }
        
        .history-section {
            margin-top: 2rem;
        }
        
        .history-section h2 {
            margin-bottom: 1rem;
            font-size: 1.3rem;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 2rem;
        }
        
        th, td {
            padding: 0.8rem;
            text-align: left;
            border-bottom: 1px solid #eee;
            font-size: 0.9rem;
        }
        
        th {
            color: var(--primary);
            font-weight: 600;
        }
        
        tr:hover {
            background-color: #f9f9f9;
        }
        
        .status {
            padding: 0.3rem 0.8rem;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 500;
        }
        
        .status.paid {
            background-color: rgba(0, 184, 148, 0.1);
            color: var(--success);
        }
        
        .status.pending {
            background-color: rgba(253, 203, 110, 0.1);
            color: #e17055;
        }
        
        .status.rejected {
            background-color: rgba(214, 48, 49, 0.1);
            color: var(--danger);
        }
        
        /* Submission Forms */
        .submission-sections {
            display: flex;
            gap: 2rem;
            margin: 2.5rem 0;
            flex-wrap: wrap;
        }
        
        .submission-form {
            flex: 1;
            min-width: 300px;
            background: white;
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 5px 25px rgba(0,0,0,0.05);
        }
        
        .submission-form h2 {
            margin-bottom: 1.5rem;
            color: var(--dark);
            position: relative;
            padding-bottom: 0.5rem;
        }
        
        .submission-form h2::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 50px;
            height: 3px;
            background: linear-gradient(90deg, var(--primary), var(--accent));
            border-radius: 3px;
        }
        
        .submission-form small {
            display: block;
            margin-top: 0.5rem;
            color: #999;
            font-size: 0.8rem;
        }
        
        /* Earning Page Styles */
        .earning-content {
            display: flex;
            flex-direction: column;
            gap: 2rem;
            margin-top: 2rem;
        }

        .content-card {
            background: white;
            border-radius: 15px;
            padding: 2rem;
            box-shadow: 0 5px 25px rgba(0,0,0,0.05);
            transition: all 0.3s ease;
        }

        .content-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 30px rgba(108, 92, 231, 0.1);
        }

        .content-header h3 {
            color: var(--primary);
            margin-bottom: 0.5rem;
        }

        .content-description {
            color: #666;
            margin-bottom: 1.5rem;
        }

        .content-type {
            margin-bottom: 1.5rem;
        }

        .content-badge {
            display: inline-block;
            padding: 0.3rem 1rem;
            background: rgba(108, 92, 231, 0.1);
            color: var(--primary);
            border-radius: 20px;
            font-weight: 500;
        }

        .content-preview {
            width: 100%;
            height: 100%;
            background: #f1f1f1;
            margin: 1.5rem 0;
            border-radius: 10px;
            overflow: hidden;
            position: relative;
        }

        .content-preview img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .content-preview i {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 3rem;
            color: white;
            background: rgba(0,0,0,0.5);
            border-radius: 50%;
            padding: 1rem;
        }

        .content-actions {
            display: flex;
            gap: 1rem;
            margin: 1.5rem 0;
        }

        .content-url {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: #f9f9f9;
            padding: 1rem;
            border-radius: 8px;
        }

        .content-url span {
            color: #666;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            padding-right: 1rem;
        }

        .copy-link-btn {
            background: none;
            border: none;
            color: var(--primary);
            cursor: pointer;
            font-size: 1rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .copy-link-btn:hover {
            opacity: 0.8;
        }
        
        /* Profile Page Submissions Section */
        .submission-history {
        margin-top: 2rem;
        width: 100%;
        overflow-x: auto;
        }
        
        .submission-history h2 {
        margin-bottom: 1rem;
        font-size: 1.3rem;
        }
        
        .table-container {
        width: 100%;
        overflow-x: auto;
        -webkit-overflow-scrolling: touch;
        border-radius: 10px;
        box-shadow: 0 2px 10px rgba(0,0,0,0.05);
        }
        
        table {
        width: 100%;
        border-collapse: collapse;
        margin-bottom: 2rem;
        min-width: 600px; /* Minimum width before scrolling */
        }
        
        th, td {
        padding: 0.8rem;
        text-align: left;
        border-bottom: 1px solid #eee;
        font-size: 0.9rem;
        }
        
        th {
        color: var(--primary);
        font-weight: 600;
        position: sticky;
        top: 0;
        background: white;
        }
        
        tr:hover {
        background-color: #f9f9f9;
        }
        
        .status {
        padding: 0.3rem 0.8rem;
        border-radius: 20px;
        font-size: 0.8rem;
        font-weight: 500;
        display: inline-block;
        white-space: nowrap;
        }
        
        .status.paid {
        background-color: rgba(0, 184, 148, 0.1);
        color: var(--success);
        }
        
        .status.pending {
        background-color: rgba(253, 203, 110, 0.1);
        color: #e17055;
        }
        
        .status.rejected {
        background-color: rgba(214, 48, 49, 0.1);
        color: var(--danger);
        }
        
        /* Mobile Styles */
        @media (max-width: 768px) {
        .submission-history {
        margin-top: 1.5rem;
        }
        
        th, td {
        padding: 0.6rem;
        font-size: 0.85rem;
        }
        
        table {
        min-width: 100%;
        }
        
        .table-container {
        -webkit-overflow-scrolling: touch;
        -ms-overflow-style: -ms-autohiding-scrollbar;
        }
        
        .table-container::-webkit-scrollbar {
        height: 5px;
        }
        
        .table-container::-webkit-scrollbar-thumb {
        background-color: rgba(108, 92, 231, 0.3);
        border-radius: 10px;
        }
        }

        /* Brand Promotion Page Styles */
        .promotion-form-container {
            display: flex;
            gap: 3rem;
            margin-top: 2rem;
        }

        .promotion-form {
            flex: 2;
            background: white;
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 5px 25px rgba(0,0,0,0.05);
        }

        .promotion-benefits {
            flex: 1;
            position: sticky;
            top: 20px;
        }

        .benefit-item {
            background: white;
            padding: 1.5rem;
            border-radius: 10px;
            margin-bottom: 1rem;
            box-shadow: 0 3px 15px rgba(0,0,0,0.05);
        }

        .benefit-item i {
            font-size: 1.5rem;
            color: var(--primary);
            margin-bottom: 0.5rem;
            display: inline-block;
        }

        .benefit-item h4 {
            margin: 0.5rem 0;
            color: var(--dark);
        }

        /* Auth Forms */
        .auth-container {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.7);
            z-index: 1000;
            justify-content: center;
            align-items: center;
            backdrop-filter: blur(5px);
            animation: fadeIn 0.3s;
        }
        
        .auth-box {
            background: white;
            padding: 2.5rem;
            border-radius: 15px;
            width: 90%;
            max-width: 450px;
            position: relative;
            box-shadow: 0 10px 40px rgba(0,0,0,0.1);
            transform: translateY(20px);
            opacity: 0;
            animation: fadeInUp 0.4s 0.1s forwards;
        }
        
        .close-btn {
            position: absolute;
            top: 1.5rem;
            right: 1.5rem;
            font-size: 1.5rem;
            cursor: pointer;
            color: #666;
            transition: all 0.3s ease;
        }
        
        .close-btn:hover {
            color: var(--danger);
            transform: rotate(90deg);
        }
        
        .auth-tabs {
            display: flex;
            margin-bottom: 2rem;
            border-bottom: 1px solid #eee;
        }
        
        .auth-tab {
            flex: 1;
            text-align: center;
            padding: 1rem 0;
            cursor: pointer;
            position: relative;
            font-weight: 500;
            color: #666;
            transition: all 0.3s ease;
        }
        
        .auth-tab.active {
            color: var(--primary);
        }
        
        .auth-tab.active::after {
            content: '';
            position: absolute;
            bottom: -1px;
            left: 0;
            width: 100%;
            height: 3px;
            background: linear-gradient(90deg, var(--primary), var(--accent));
            border-radius: 3px;
        }
        
        .auth-form {
            display: none;
        }
        
        .auth-form.active {
            display: block;
            animation: fadeIn 0.5s;
        }
        
        .form-group {
            margin-bottom: 1.5rem;
            position: relative;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 0.8rem;
            font-weight: 500;
            color: var(--dark);
        }
        
        .form-group input, .form-group select, .form-group textarea {
            width: 100%;
            padding: 1rem;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 1rem;
            transition: all 0.3s ease;
        }
        
        .form-group input:focus, .form-group select:focus, .form-group textarea:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(108, 92, 231, 0.2);
            outline: none;
        }
        
        .form-group i {
            position: absolute;
            right: 1rem;
            top: 3.4rem;
            color: #999;
        }
        
        /* Footer */
        footer {
            background: var(--dark);
            color: white;
            padding: 3rem 0;
            text-align: center;
        }
        
        .footer-content {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 2rem;
        }
        
        .footer-logo {
            font-size: 1.8rem;
            font-weight: bold;
            margin-bottom: 1.5rem;
            display: inline-block;
        }
        
        .footer-links {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            margin: 1.5rem 0;
        }
        
        .footer-links a {
            color: white;
            text-decoration: none;
            margin: 0 1rem;
            opacity: 0.7;
            transition: all 0.3s ease;
        }
        
        .footer-links a:hover {
            opacity: 1;
        }
        
        .social-links {
            margin: 1.5rem 0;
        }
        
        .social-links a {
            display: inline-block;
            width: 40px;
            height: 40px;
            background: rgba(255,255,255,0.1);
            border-radius: 50%;
            margin: 0 0.5rem;
            color: white;
            line-height: 40px;
            transition: all 0.3s ease;
        }
        
        .social-links a:hover {
            background: var(--primary);
            transform: translateY(-3px);
        }
        
        .copyright {
            opacity: 0.7;
            font-size: 0.9rem;
            margin-top: 1.5rem;
        }
        
        /* Mobile Menu Button */
        .menu-toggle {
            display: none;
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 999;
            width: 60px;
            height: 60px;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            border-radius: 50%;
            border: none;
            cursor: pointer;
            box-shadow: 0 4px 20px rgba(108, 92, 231, 0.3);
            transition: all 0.3s ease;
        }
        
        .menu-toggle:hover {
            transform: scale(1.1);
        }
        
        .menu-toggle i {
            font-size: 1.5rem;
        }
        
        /* Mobile Menu */
        .mobile-menu {
            position: fixed;
            bottom: 90px;
            right: 20px;
            width: 250px;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            z-index: 998;
            display: none;
            overflow: hidden;
            transform: translateY(20px);
            opacity: 0;
            transition: all 0.3s ease;
        }
        
        .mobile-menu.active {
            display: block;
            transform: translateY(0);
            opacity: 1;
            animation: fadeInUp 0.3s;
        }
        
        .mobile-menu ul {
            list-style: none;
            padding: 1rem 0;
        }
        
        .mobile-menu ul li a {
            display: block;
            padding: 0.70rem 1.5rem;
            color: var(--dark);
            text-decoration: none;
            transition: all 0.3s ease;
        }
        
        .mobile-menu ul li a:hover {
            background-color: rgba(108, 92, 231, 0.1);
            color: var(--primary);
        }
        
        .mobile-menu ul li a i {
            margin-right: 10px;
            width: 20px;
            text-align: center;
        }
        
        /* Hide desktop nav on mobile */
        @media (max-width: 768px) {
            nav ul {
                display: none;
            }
            
            .menu-toggle {
                display: flex;
                align-items: center;
                justify-content: center;
            }

            .content-actions {
                flex-direction: column;
            }
            
            .content-actions .btn {
                width: 100%;
            }

            .stats {
                flex-direction: column;
            }
            
            .stat-card {
                margin: 1rem 0;
            }
            
            .available-balance {
                flex-direction: column;
            }
            
            .stat-details {
                flex-direction: column;
                gap: 0.3rem;
            }
            
            .payment-methods {
                flex-direction: column;
            }
            
            .payment-method {
                margin: 0.5rem 0;
            }
            
            .submission-sections {
                flex-direction: column;
            }
            
            .about-content {
                flex-direction: column;
            }
            
            .contact-container {
                flex-direction: column;
            }
            
            .promotion-form-container {
                flex-direction: column;
            }
        }
        
        /* Animations */
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        @keyframes fadeInUp {
            from { 
                opacity: 0;
                transform: translateY(20px);
            }
            to { 
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        @keyframes fadeInDown {
            from { 
                opacity: 0;
                transform: translateY(-20px);
            }
            to { 
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        @keyframes pulse {
            0% { transform: scale(1); opacity: 0.5; }
            50% { transform: scale(1.1); opacity: 0.8; }
            100% { transform: scale(1); opacity: 0.5; }
        }
    </style>
</head>
<body>
    <!-- Header -->
    <header>
        <div class="logo">
            <i class="fas fa-rocket"></i>
            InstaPromo Exchange
        </div>
        <nav>
            <ul>
                <li><a href="#" class="nav-link" data-page="home">Home</a></li>
                <li><a href="#" class="nav-link" data-page="about">About Us</a></li>
                <li><a href="#" class="nav-link" data-page="how-it-works">How It Works</a></li>
                <li><a href="#" class="nav-link" data-page="brands">Brands</a></li>
                <li><a href="#" class="nav-link" data-page="earning">Earning</a></li>
                <li><a href="#" class="nav-link" data-page="brand-promotion">Brand Promotion</a></li>
                <li><a href="#" class="nav-link" data-page="faq">FAQ</a></li>
                <li><a href="#" class="nav-link" data-page="contact">Contact</a></li>
                <li><a href="#" class="login-link">Login</a></li>
                <li><a href="#" class="signup-link">Sign Up</a></li>
                <li><a href="#" class="profile-link" style="display: none;">Profile</a></li>
            </ul>
        </nav>
        
        <!-- Mobile Menu Button -->
        <button class="menu-toggle">
            <i class="fas fa-bars"></i>
        </button>
        
        <!-- Mobile Menu -->
        <div class="mobile-menu">
            <ul>
                <li><a href="#" class="nav-link" data-page="home"><i class="fas fa-home"></i> Home</a></li>
                <li><a href="#" class="login-link"><i class="fas fa-sign-in-alt"></i> Login</a></li>
                <li><a href="#" class="signup-link"><i class="fas fa-user-plus"></i> Sign Up</a></li>
                <li><a href="#" class="nav-link" data-page="brands"><i class="fas fa-tags"></i> Brands</a></li>
                <li><a href="#" class="nav-link" data-page="earning"><i class="fas fa-rupee-sign"></i> Earning</a></li>
                
                <li><a href="#" class="nav-link" data-page="how-it-works"><i class="fas fa-cogs"></i> How It Works</a></li>
                
                <li><a href="#" class="nav-link" data-page="brand-promotion"><i class="fas fa-bullhorn"></i> Brand Promotion</a></li>
                <li><a href="#" class="nav-link" data-page="faq"><i class="fas fa-question-circle"></i> FAQ</a></li>
                <li><a href="#" class="nav-link" data-page="contact"><i class="fas fa-envelope"></i> Contact</a></li>
                <li><a href="#" class="nav-link" data-page="about"><i class="fas fa-info-circle"></i> About Us</a></li>
                
                <li><a href="#" class="profile-link" style="display: none;"><i class="fas fa-user"></i> Profile</a></li>
            </ul>
        </div>
    </header>

    <!-- Main Content -->
    <div class="container">
        <!-- Home Page -->
        <div class="page active" id="home-page">
            <div class="hero">
                <h1>Earn Money by Sharing Instagram Stories & Reels</h1>
                <p>Join thousands of creators who are earning daily by promoting brands they love. No minimum followers required!</p>
                <a href="#" class="btn signup-link">Start Earning Now</a>
              
            </div>
            
            <div class="how-it-works">
                <div class="step">
                    <i class="fas fa-user-plus"></i>
                    <h3>Create Your Account</h3>
                    <p>Sign up in just 30 seconds with your Instagram profile</p>
                </div>
                <div class="step">
                    <i class="fas fa-download"></i>
                    <h3>Get Campaign Content</h3>
                    <p>Download ready-to-post videos and images from brands</p>
                </div>
                <div class="step">
                    <i class="fas fa-share-alt"></i>
                    <h3>Share & Earn</h3>
                    <p>Post on your Instagram story and get paid instantly</p>
                </div>
            </div>
        </div>
        
        <!-- About Us Page -->
        <div class="page" id="about-page">
            <div class="page-header">
                <h1>About InstaPromo Exchange</h1>
                <p>Connecting brands with authentic social media promoters since 2023</p>
            </div>
            
            <div class="about-content">
                <div class="about-text">
                    <h2>Our Story</h2>
                    <p>Founded in 2023, InstaPromo Exchange was born out of a simple idea: to create a platform where brands can get authentic promotion from real people, and where anyone can earn money by sharing content they love.</p>
                    <p>We noticed that traditional influencer marketing was becoming expensive and inauthentic, while small businesses struggled to get visibility. At the same time, millions of social media users wanted to monetize their online presence but didn't know how.</p>
                    <p>InstaPromo Exchange bridges this gap by creating a win-win ecosystem where everyone benefits.</p>
                </div>
                <div class="about-image">
                    <img src="https://source.unsplash.com/random/600x400/?team,meeting" alt="Our Team">
                </div>
            </div>
            
            <div class="section">
                <h2>Our Mission</h2>
                <p>To democratize social media marketing by making it accessible to businesses of all sizes, while providing earning opportunities for social media users worldwide.</p>
            </div>
            
            <div class="section">
                <h2>Meet The Team</h2>
                <div class="team">
                    <div class="team-member">
                        <img src="https://randomuser.me/api/portraits/men/32.jpg" alt="Team Member">
                        <h3>Rahul Sharma</h3>
                        <p>Founder & CEO</p>
                    </div>
                    <div class="team-member">
                        <img src="https://randomuser.me/api/portraits/women/44.jpg" alt="Team Member">
                        <h3>Priya Patel</h3>
                        <p>Marketing Director</p>
                    </div>
                    <div class="team-member">
                        <img src="https://randomuser.me/api/portraits/men/67.jpg" alt="Team Member">
                        <h3>Arjun Singh</h3>
                        <p>Tech Lead</p>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- How It Works Page -->
        <div class="page" id="how-it-works-page">
            <div class="page-header">
                <h1>How It Works</h1>
                <p>Simple steps to start earning with InstaPromo Exchange</p>
            </div>
            
            <div class="steps">
                <div class="step-card">
                    <div class="step-number">1</div>
                    <h3>Sign Up</h3>
                    <p>Create your free account in minutes. Connect your Instagram profile and verify your identity.</p>
                </div>
                <div class="step-card">
                    <div class="step-number">2</div>
                    <h3>Browse Campaigns</h3>
                    <p>Explore available campaigns from brands. Choose ones that match your interests and audience.</p>
                </div>
                <div class="step-card">
                    <div class="step-number">3</div>
                    <h3>Download Content</h3>
                    <p>Get ready-to-post images, videos, and captions. Everything is pre-approved by the brand.</p>
                </div>
                <div class="step-card">
                    <div class="step-number">4</div>
                    <h3>Post & Earn</h3>
                    <p>Share the content on your Instagram profile or story. Submit proof and get paid automatically.</p>
                </div>
            </div>
            
            <div class="section">
                <h2>For Brands</h2>
                <div class="steps">
                    <div class="step-card">
                        <div class="step-number">1</div>
                        <h3>Create Campaign</h3>
                        <p>Set up your promotion with content, budget, and targeting preferences.</p>
                    </div>
                    <div class="step-card">
                        <div class="step-number">2</div>
                        <h3>Get Authentic Promotion</h3>
                        <p>Real users share your content with their genuine followers.</p>
                    </div>
                    <div class="step-card">
                        <div class="step-number">3</div>
                        <h3>Track Results</h3>
                        <p>Monitor engagement and ROI through our dashboard.</p>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Brands Page -->
        <div class="page" id="brands-page">
            <div class="page-header">
                <h1>Our Partner Brands</h1>
                <p>Trusted by businesses of all sizes across industries</p>
            </div>
            
            <div class="section">
                <h2>Featured Brands</h2>
                <div class="brand-list">
                    <div class="brand-item">
                        <img src="https://via.placeholder.com/150x60?text=Fashion+Brand" alt="Brand Logo">
                    </div>
                    <div class="brand-item">
                        <img src="https://via.placeholder.com/150x60?text=Fitness+Brand" alt="Brand Logo">
                    </div>
                    <div class="brand-item">
                        <img src="https://via.placeholder.com/150x60?text=Food+Brand" alt="Brand Logo">
                    </div>
                    <div class="brand-item">
                        <img src="https://via.placeholder.com/150x60?text=Tech+Brand" alt="Brand Logo">
                    </div>
                    <div class="brand-item">
                        <img src="https://via.placeholder.com/150x60?text=Beauty+Brand" alt="Brand Logo">
                    </div>
                    <div class="brand-item">
                        <img src="https://via.placeholder.com/150x60?text=Travel+Brand" alt="Brand Logo">
                    </div>
                </div>
            </div>
            
            <div class="section">
                <h2>Why Brands Love Us</h2>
                <div class="steps">
                    <div class="step-card">
                        <i class="fas fa-rupee-sign"></i>
                        <h3>Cost Effective</h3>
                        <p>Get authentic promotion at a fraction of traditional influencer marketing costs.</p>
                    </div>
                    <div class="step-card">
                        <i class="fas fa-users"></i>
                        <h3>Real Engagement</h3>
                        <p>Genuine shares from real users drive meaningful engagement.</p>
                    </div>
                    <div class="step-card">
                        <i class="fas fa-chart-line"></i>
                        <h3>Measurable Results</h3>
                        <p>Track performance with our detailed analytics dashboard.</p>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Brand Promotion Page -->
        <div class="page" id="brand-promotion-page">
            <div class="page-header">
                <h1>Brand Promotion Request</h1>
                <p>Submit your brand details to get promoted by thousands of Instagram users</p>
            </div>

            <div class="promotion-form-container">
                <div class="promotion-form">
                    <h2>Company Information</h2>
                    <form id="brandPromotionForm">
                        <div class="form-group">
                            <label>Company/Brand Name*</label>
                            <input type="text" id="companyName" name="companyName" required placeholder="Your company or brand name">
                        </div>

                        <div class="form-group">
                            <label>Industry*</label>
                            <select id="industry" name="industry" required>
                                <option value="">Select Industry</option>
                                <option value="fashion">Fashion & Apparel</option>
                                <option value="beauty">Beauty & Cosmetics</option>
                                <option value="fitness">Fitness & Wellness</option>
                                <option value="food">Food & Beverage</option>
                                <option value="tech">Technology</option>
                                <option value="travel">Travel & Hospitality</option>
                                <option value="education">Education</option>
                                <option value="other">Other</option>
                            </select>
                        </div>

                        <div class="form-group">
                            <label>Website URL</label>
                            <input type="url" id="website" name="website" placeholder="https://yourcompany.com">
                        </div>

                        <div class="form-group">
                            <label>Instagram Handle</label>
                            <div class="input-with-icon">
                                <span class="icon"></span>
                                <input type="text" id="instagram" name="instagram" placeholder="yourbrandhandle">
                            </div>
                        </div>

                        <div class="form-group checkbox-group">
                            <input type="checkbox" id="agreeTerms" name="agreeTerms" required>
                            <label for="agreeTerms">I agree to the <a href="#" class="terms-link">Terms of Service</a> and <a href="#" class="privacy-link">Privacy Policy</a></label>
                        </div>

                        <button type="submit" class="btn submit-promotion">Submit Promotion Request</button>
                    </form>
                </div>

                <div class="promotion-benefits">
                    <h3>Why Promote With Us?</h3>
                    <div class="benefit-item">
                        <i class="fas fa-users"></i>
                        <h4>Reach Real People</h4>
                        <p>Get authentic promotion from real Instagram users with engaged followers</p>
                    </div>
                    <div class="benefit-item">
                        <i class="fas fa-rupee-sign"></i>
                        <h4>Cost Effective</h4>
                        <p>Pay only for completed shares at rates you set</p>
                    </div>
                    <div class="benefit-item">
                        <i class="fas fa-chart-line"></i>
                        <h4>Track Results</h4>
                        <p>Get detailed reports on shares and engagement</p>
                    </div>
                    <div class="benefit-item">
                        <i class="fas fa-bolt"></i>
                        <h4>Quick Setup</h4>
                        <p>Start getting shares within 24 hours of approval</p>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Earning Page -->
        <div class="page" id="earning-page">
            <div class="page-header">
                <h1>Earning Opportunities</h1>
                <p>Promote these brands and earn money for each share</p>
            </div>

            <div class="earning-content">
                <!-- Video Content -->
                <div class="content-card">
                    <div class="content-header">
                        <h3>Nike Air Max</h3>
                        <p class="content-description">Promote our latest sneaker collection with #AirMaxDay</p>
                    </div>
                    
                    <div class="content-type">
                        <span class="content-badge">Video</span>
                    </div>
                    
                    <div class="content-preview video-preview">
                        <img src="https://source.unsplash.com/random/600x400/?sneakers" alt="Nike Air Max Video">
                        <i class="fas fa-play"></i>
                    </div>
                    
                    <div class="content-actions">
                        <button class="btn download-btn"><i class="fas fa-download"></i> Download</button>
                        <button class="btn copy-url-btn"><i class="fas fa-copy"></i> Copy URL</button>
                    </div>
                    
                    <div class="content-url">
                        <span>https://instapromo.exchange/nike-airmax</span>
                        <button class="copy-link-btn"><i class="fas fa-copy"></i> Copy Link</button>
                    </div>
                    
                </div>

                <!-- Image Content -->
                <div class="content-card">
                    <div class="content-header">
                        <h3>Starbucks Summer</h3>
                        <p class="content-description">Share our new summer menu with #StarbucksSummer</p>
                    </div>
                    
                    <div class="content-type">
                        <span class="content-badge">Story</span>
                    </div>
                    
                    <div class="content-preview image-preview">
                        <img src="https://images.unsplash.com/photo-1604311795833-25e1d5c128c6?q=80&w=1227&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D" alt="Starbucks Summer Image">
                    </div>
                    
                    <div class="content-actions">
                        <button class="btn download-btn"><i class="fas fa-download"></i> Download</button>
                        <button class="btn copy-url-btn"><i class="fas fa-copy"></i> Copy URL</button>
                    </div>
                    
                    <div class="content-url">
                        <span>https://instapromo.exchange/starbucks-summer</span>
                        <button class="copy-link-btn"><i class="fas fa-copy"></i> Copy Link</button>
                    </div>
                </div>

                <!-- Content -->
                <div class="content-card">
                    <div class="content-header">
                        <h3>Apple iPhone 15</h3>
                        <p class="content-description">Showcase the new iPhone features with #iPhone15Pro</p>
                    </div>
                    
                    <div class="content-type">
                        <span class="content-badge">Video</span>
                    </div>
                    
                    <div class="content-preview video-preview">
                        <iframe src="https://assets.pinterest.com/ext/embed.html?id=1100778333932577287" height="500" width="250" frameborder="0" scrolling="no" ></iframe>
                        
                    </div>
                    
                    <div class="content-actions">
                        <button class="btn download-btn"><i class="fas fa-download"></i> Download</button>
                        <button class="btn copy-url-btn"><i class="fas fa-copy"></i> Copy URL</button>
                    </div>
                    
                    <div class="content-url">
                        <span>https://instapromo.exchange/iphone-15</span>
                        <button class="copy-link-btn"><i class="fas fa-copy"></i> Copy Link</button>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- FAQ Page -->
        <div class="page" id="faq-page">
            <div class="page-header">
                <h1>Frequently Asked Questions</h1>
                <p>Find answers to common questions about InstaPromo Exchange</p>
            </div>
            
            <div class="faq-section">
                <h2>For Creators</h2>
                <div class="faq-item">
                    <div class="faq-question">
                        <span>How do I earn money with InstaPromo Exchange?</span>
                        <i class="fas fa-chevron-down"></i>
                    </div>
                    <div class="faq-answer">
                        <p>You earn money by sharing brand content on your Instagram profile or stories. Each campaign specifies the payment amount per share. After you complete the required action and submit proof, we process your payment.</p>
                    </div>
                </div>
                <div class="faq-item">
                    <div class="faq-question">
                        <span>Do I need a certain number of followers to join?</span>
                        <i class="fas fa-chevron-down"></i>
                    </div>
                    <div class="faq-answer">
                        <p>No! We welcome all Instagram users regardless of follower count. Some campaigns may have specific requirements, but many are open to everyone.</p>
                    </div>
                </div>
                <div class="faq-item">
                    <div class="faq-question">
                        <span>How and when do I get paid?</span>
                        <i class="fas fa-chevron-down"></i>
                    </div>
                    <div class="faq-answer">
                        <p>Payments are processed within 48 hours after your submission is approved. You can withdraw your earnings via UPI, Paytm, or bank transfer once you reach the minimum withdrawal amount of ₹200.</p>
                    </div>
                </div>
            </div>
            
            <div class="faq-section">
                <h2>For Brands</h2>
                <div class="faq-item">
                    <div class="faq-question">
                        <span>How does InstaPromo Exchange benefit my brand?</span>
                        <i class="fas fa-chevron-down"></i>
                    </div>
                    <div class="faq-answer">
                        <p>Our platform helps you get authentic promotion from real users at scale. Unlike traditional influencer marketing, you pay only for completed shares and can reach diverse audiences through micro-influencers.</p>
                    </div>
                </div>
                <div class="faq-item">
                    <div class="faq-question">
                        <span>What types of content can I promote?</span>
                        <i class="fas fa-chevron-down"></i>
                    </div>
                    <div class="faq-answer">
                        <p>You can promote product images, promotional videos, discount codes, event announcements, and more. All content goes through our approval process to ensure quality.</p>
                    </div>
                </div>
                <div class="faq-item">
                    <div class="faq-question">
                        <span>How do I track the performance of my campaigns?</span>
                        <i class="fas fa-chevron-down"></i>
                    </div>
                    <div class="faq-answer">
                        <p>Our dashboard provides real-time analytics showing how many users shared your content, estimated reach, and engagement metrics. You'll also get periodic reports via email.</p>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Contact Page -->
        <div class="page" id="contact-page">
            <div class="page-header">
                <h1>Contact Us</h1>
                <p>We'd love to hear from you! Get in touch with our team</p>
            </div>
            
            <div class="contact-container">
                <div class="contact-info">
                    <div class="contact-card">
                        <h3>Contact Information</h3>
                        <div class="contact-detail">
                            <i class="fas fa-map-marker-alt"></i>
                            <div>
                                <h4>Address</h4>
                                <p>123 Business Park, Sector 22<br>Mumbai, Maharashtra 400001</p>
                            </div>
                        </div>
                        <div class="contact-detail">
                            <i class="fas fa-phone"></i>
                            <div>
                                <h4>Phone</h4>
                                <p>+91 98765 43210</p>
                            </div>
                        </div>
                        <div class="contact-detail">
                            <i class="fas fa-envelope"></i>
                            <div>
                                <h4>Email</h4>
                                <p>hello@instapromoexchange.com</p>
                            </div>
                        </div>
                    </div>
                    
                    <div class="contact-card">
                        <h3>Business Hours</h3>
                        <div class="contact-detail">
                            <i class="fas fa-clock"></i>
                            <div>
                                <p>Monday - Friday: 9:00 AM - 6:00 PM</p>
                                <p>Saturday: 10:00 AM - 4:00 PM</p>
                                <p>Sunday: Closed</p>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="contact-form">
                    <h3>Send Us a Message</h3>
                    <form id="contactForm">
                        <div class="form-group">
                            <label for="name">Your Name</label>
                            <input type="text" id="name" name="name" required>
                        </div>
                        <div class="form-group">
                            <label for="email">Email Address</label>
                            <input type="email" id="email" name="email" required>
                        </div>
                        <div class="form-group">
                            <label for="subject">Subject</label>
                            <select id="subject" name="subject">
                                <option value="general">General Inquiry</option>
                                <option value="creator">Creator Support</option>
                                <option value="brand">Brand Partnership</option>
                                <option value="technical">Technical Support</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label for="message">Message</label>
                            <textarea id="message" name="message" required></textarea>
                        </div>
                        <button type="submit" class="btn">Send Message</button>
                    </form>
                </div>
            </div>
        </div>
        
        <!-- Updated Profile Page -->
        <div class="page profile-page" id="profile-page">
            <div class="profile-header">
                <div class="profile-header-content">
                    <h1>Welcome to your Dashboard!</h1>
                    <p>Hello, <span class="username">Username</span></p>
                </div>
            </div>
            
            <div class="stats">
                <div class="stat-card">
                    <h3>Videos Submitted</h3>
                    <p class="videos-count">5</p>
                    <div class="stat-details">
                        <span>1 approved</span>
                        <span>2 pending</span>
                        <span>1 rejected</span>
                    </div>
                </div>
                <div class="stat-card">
                    <h3>Stories Shared</h3>
                    <p class="stories-count">1</p>
                    <div class="stat-details">
                        <span>1 verified</span>
                        <span>0 pending</span>
                        <span>0 rejected</span>
                    </div>
                </div>
                <div class="stat-card">
                    <h3>Total Earnings</h3>
                    <p class="total-earnings">₹550</p>
                </div>
            </div>
            
            <div class="available-balance">
                <div class="balance-card">
                    <h3>Available</h3>
                    <p class="balance-amount">₹950</p>
                    <p class="balance-status">Ready for withdrawal</p>
                </div>
                <div class="balance-card">
                    <h3>Withdrawn</h3>
                    <p class="withdrawn-amount">₹250</p>
                    <p class="balance-status">Total Amount Withdrawn</p>
                </div>
            </div>
            
            <!-- New Submission Sections -->
            <div class="submission-sections">
                <!-- Reels Submission Form -->
                <div class="submission-form">
                    <h2>Submit Reels Proof</h2>
                    <div class="form-group">
                        <label>Company/Brand Name</label>
                        <input type="text" class="company-name" placeholder="Which brand are you submitting for?">
                    </div>
                    <div class="form-group">
                        <label>Reels URL</label>
                        <input type="url" class="reels-url" placeholder="Paste Instagram reels URL">
                        <small>Example: https://www.instagram.com/reel/CvJgHC9gF5H/</small>
                    </div>
                    <button class="btn submit-reels-btn">Submit Reels</button>
                </div>
                
                <!-- Story Submission Form -->
                <div class="submission-form">
                    <h2>Submit Story Proof</h2>
                    <div class="form-group">
                        <label>Company/Brand Name</label>
                        <input type="text" class="company-name" placeholder="Which brand are you submitting for?">
                    </div>
                    <div class="form-group">
                        <label>Story URL</label>
                        <input type="url" class="story-url" placeholder="Paste Instagram story URL">
                        <small>Note: Make sure your profile is public when submitting</small>
                    </div>
                    <button class="btn submit-story-btn">Submit Story</button>
                </div>
            </div>
            
            <!-- Submission History Table -->
            <!-- Updated Profile Page Submissions Section -->
            <div class="submission-history">
            <h2>Your Submissions</h2>
            <div class="table-container">
            <table>
            <thead>
            <tr>
            <th>Date</th>
            <th>Type</th>
            <th>Brand</th>
            <th>URL</th>
            <th>Status</th>
            <th>Amount</th>
            </tr>
            </thead>
            <tbody class="submission-body">
            <tr>
            <td>15/08/2023</td>
            <td>Reels</td>
            <td>Nike</td>
            <td><a href="#" target="_blank">instagram.com/reel/xyz</a></td>
            <td><span class="status paid">Approved (+₹50)</span></td>
            <td>₹50</td>
            </tr>
            <tr>
            <td>14/08/2023</td>
            <td>Story</td>
            <td>Starbucks</td>
            <td><a href="#" target="_blank">instagram.com/stories/abc</a></td>
            <td><span class="status pending">Pending</span></td>
            <td>-</td>
            </tr>
            <tr>
            <td>13/08/2023</td>
            <td>Reels</td>
            <td>Adidas</td>
            <td><a href="#" target="_blank">instagram.com/reel/def</a></td>
            <td><span class="status rejected">Rejected</span></td>
            <td>-</td>
            </tr>
            </tbody>
            </table>
            </div>
            </div>
            
            <div class="withdrawal-form">
                <h2>Request Withdrawal</h2>
                <div class="form-group">
                    <label>Amount (₹)</label>
                    <input type="number" class="withdrawal-amount" placeholder="Enter amount">
                </div>
                
                <h3>Payment Method</h3>
                <div class="payment-methods">
                    <div class="payment-method">
                        <input type="radio" name="payment" id="upi" checked>
                        <label for="upi">UPI</label>
                    </div>
                    <div class="payment-method">
                        <input type="radio" name="payment" id="bank">
                        <label for="bank">Bank Transfer</label>
                    </div>
                </div>
                
                <div class="payment-details upi-details active">
                    <div class="form-group">
                        <label>Enter your UPI ID</label>
                        <input type="text" class="upi-id" placeholder="yourname@upi">
                    </div>
                </div>
                
                <div class="payment-details bank-details">
                    <div class="form-group">
                        <label>Account Holder Name</label>
                        <input type="text" class="bank-name" placeholder="Enter name as per bank records">
                    </div>
                    <div class="form-group">
                        <label>Account Number</label>
                        <input type="text" class="bank-account" placeholder="Enter account number">
                    </div>
                    <div class="form-group">
                        <label>Phone Number</label>
                        <input type="tel" class="bank-phone" placeholder="Registered mobile number">
                    </div>
                </div>
                
                <button class="btn request-withdrawal">Request Withdrawal</button>
            </div>
        </div>
        
        <!-- Home Page After Login -->
        <div class="page" id="loggedin-home-page">
            <div class="brand-reels">
                <h2>Available Brand Reels</h2>
                
                <div class="reel-card">
                    <h3>ABC Fitness Gym</h3>
                    <p>Share our gym tour video on your Instagram story with #BestGymInCity</p>
                    <span class="reward">₹10 per share</span>
                    
                    <div class="video-preview">
                        <img src="https://source.unsplash.com/random/600x400/?gym" alt="Gym Video">
                        <i class="fas fa-play"></i>
                    </div>
                    
                    <div class="reel-actions">
                        <button class="btn download-btn"><i class="fas fa-download"></i> Download Video</button>
                        <button class="btn submit-btn"><i class="fas fa-check-circle"></i> Submit Proof</button>
                    </div>
                </div>
                
                <div class="reel-card">
                    <h3>XYZ Cafe</h3>
                    <p>Post our new coffee menu on your story with #BestCoffeeInTown</p>
                    <span class="reward">₹15 per share</span>
                    
                    <div class="video-preview">
                        <img src="https://source.unsplash.com/random/600x400/?coffee" alt="Coffee Menu">
                    </div>
                    
                    <div class="reel-actions">
                        <button class="btn download-btn"><i class="fas fa-download"></i> Download Image</button>
                        <button class="btn submit-btn"><i class="fas fa-check-circle"></i> Submit Proof</button>
                    </div>
                </div>
                
                <div class="reel-card">
                    <h3>Fashion Store</h3>
                    <p>Share our new collection video with #SummerFashion2023</p>
                    <span class="reward">₹20 per share</span>
                    
                    <div class="video-preview">
                        <img src="https://source.unsplash.com/random/600x400/?fashion" alt="Fashion Video">
                        <i class="fas fa-play"></i>
                    </div>
                    
                    <div class="reel-actions">
                        <button class="btn download-btn"><i class="fas fa-download"></i> Download Video</button>
                        <button class="btn submit-btn"><i class="fas fa-check-circle"></i> Submit Proof</button>
                    </div>
                </div>
            </div>
            
            <div class="submission-forms">
                <div class="reel-submission">
                    <h2>Submit Reel Proof</h2>
                    <div class="form-group">
                        <label>Company/Brand</label>
                        <input type="text" class="company-name" placeholder="Which brand are you submitting for?">
                    </div>
                    <div class="form-group">
                        <label>Reel Link</label>
                        <input type="url" class="reel-link" placeholder="Paste Instagram reel URL">
                        <small>Example: https://www.instagram.com/reel/CvJgHC9gF5H/</small>
                    </div>
                    <button class="btn submit-reel-btn">Submit Reel Proof</button>
                </div>

                <div class="story-submission">
                    <h2>Submit Story Proof</h2>
                    <div class="form-group">
                        <label>Company/Brand</label>
                        <input type="text" class="company-name" placeholder="Which brand are you submitting for?">
                    </div>
                    <div class="form-group">
                        <label>Story Link</label>
                        <input type="url" class="story-link" placeholder="Paste Instagram story URL">
                        <small>Note: Make sure your profile is public when submitting</small>
                    </div>
                    <button class="btn submit-story-btn">Submit Story Proof</button>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Auth Forms -->
    <div class="auth-container">
        <div class="auth-box">
            <span class="close-btn">&times;</span>
            
            <div class="auth-tabs">
                <div class="auth-tab login-tab active">Login</div>
                <div class="auth-tab signup-tab">Sign Up</div>
            </div>
            
            <div class="auth-form login-form active">
                <div class="form-group">
                    <label>Email</label>
                    <input type="email" class="login-email" placeholder="Enter your email">
                    <i class="fas fa-envelope"></i>
                </div>
                <div class="form-group">
                    <label>Password</label>
                    <input type="password" class="login-password" placeholder="Enter your password">
                    <i class="fas fa-lock"></i>
                </div>
                <button class="btn login-submit">Login</button>
                <p style="text-align: center; margin-top: 1rem;">Forgot password? <a href="#" style="color: var(--primary);">Click here</a></p>
            </div>
            
            <div class="auth-form signup-form">
                <div class="form-group">
                    <label>Full Name</label>
                    <input type="text" class="signup-name" placeholder="Enter your name">
                    <i class="fas fa-user"></i>
                </div>
                <div class="form-group">
                    <label>Email</label>
                    <input type="email" class="signup-email" placeholder="Enter your email">
                    <i class="fas fa-envelope"></i>
                </div>
                <div class="form-group">
                    <label>Phone Number</label>
                    <input type="tel" class="signup-phone" placeholder="Enter your phone">
                    <i class="fas fa-phone"></i>
                </div>
                <div class="form-group">
                    <label>Instagram Profile</label>
                    <input type="url" class="signup-instagram" placeholder="Paste your Instagram link">
                    <i class="fab fa-instagram"></i>
                </div>
                <div class="form-group">
                    <label>Password</label>
                    <input type="password" class="signup-password" placeholder="Create a password">
                    <i class="fas fa-lock"></i>
                </div>
                <button class="btn signup-submit">Sign Up</button>
                <p style="text-align: center; margin-top: 1rem;">Already have an account? <a href="#" class="switch-to-login" style="color: var(--primary);">Login here</a></p>
            </div>
        </div>
    </div>
    
    <!-- Footer -->
    <footer>
        <div class="footer-content">
            <div class="footer-logo">InstaPromo Exchange</div>
            <div class="footer-links">
                <a href="#" class="footer-link" data-page="home">Home</a>
                <a href="#" class="footer-link" data-page="about">About Us</a>
                <a href="#" class="footer-link" data-page="how-it-works">How It Works</a>
                <a href="#" class="footer-link" data-page="brands">Brands</a>
                <a href="#" class="footer-link" data-page="earning">Earning</a>
                <a href="#" class="footer-link" data-page="brand-promotion">Brand Promotion</a>
                <a href="#" class="footer-link" data-page="faq">FAQ</a>
                <a href="#" class="footer-link" data-page="contact">Contact</a>
            </div>
            <div class="social-links">
                <a href="https://www.instagram.com/rudra_chauhan05?igsh=dG4zanB4Y2huMmJ5"><i class="fab fa-instagram"></i></a>
                <a href="#"><i class="fab fa-facebook-f"></i></a>
                <a href="#"><i class="fab fa-twitter"></i></a>
                <a href="#"><i class="fab fa-linkedin-in"></i></a>
            </div>
            <div class="copyright">
                &copy; 2023 InstaPromo Exchange. All rights reserved.
            </div>
        </div>
    </footer>
    
    <script>
        // Initialize Firebase
        const firebaseConfig = {
            apiKey: "AIzaSyDUeLMfctEj_tyvo3oZC_XfxRc1BhqSLRw",
            authDomain: "instapromoexchange.firebaseapp.com",
            projectId: "instapromoexchange",
            storageBucket: "instapromoexchange.appspot.com",
            messagingSenderId: "536549261065",
            appId: "1:536549261065:web:10961ec3565823eb1a1dd8"
        };
        
        firebase.initializeApp(firebaseConfig);
        
        // Initialize Firebase services
        const auth = firebase.auth();
        const db = firebase.firestore();
        const storage = firebase.storage();
        
        // JavaScript for enhanced functionality
        document.addEventListener('DOMContentLoaded', function() {
            // Page Navigation
            const navLinks = document.querySelectorAll('.nav-link, .footer-link');
            const pages = document.querySelectorAll('.page');
            
            function showPage(pageId) {
                pages.forEach(page => {
                    page.style.display = 'none';
                    page.classList.remove('active');
                });
                
                const activePage = document.getElementById(`${pageId}-page`);
                if (activePage) {
                    activePage.style.display = 'block';
                    activePage.classList.add('active');
                }
                
                // Update active link
                navLinks.forEach(link => {
                    link.classList.remove('active');
                    if (link.dataset.page === pageId) {
                        link.classList.add('active');
                    }
                });
                
                // Scroll to top
                window.scrollTo(0, 0);
            }
            
            navLinks.forEach(link => {
                link.addEventListener('click', function(e) {
                    e.preventDefault();
                    showPage(this.dataset.page);
                });
            });
            
            // Show home page by default
            showPage('home');
            
            // FAQ Accordion
            const faqItems = document.querySelectorAll('.faq-item');
            faqItems.forEach(item => {
                const question = item.querySelector('.faq-question');
                question.addEventListener('click', () => {
                    item.classList.toggle('active');
                });
            });
            
            // Contact Form Submission
            const contactForm = document.getElementById('contactForm');
            if (contactForm) {
                contactForm.addEventListener('submit', function(e) {
                    e.preventDefault();
                    alert('Thank you for your message! We will get back to you soon.');
                    this.reset();
                });
            }
            
            // Auth Modal
            const authContainer = document.querySelector('.auth-container');
            const closeBtn = document.querySelector('.close-btn');
            const loginTab = document.querySelector('.login-tab');
            const signupTab = document.querySelector('.signup-tab');
            const loginForm = document.querySelector('.login-form');
            const signupForm = document.querySelector('.signup-form');
            const loginLinks = document.querySelectorAll('.login-link');
            const signupLinks = document.querySelectorAll('.signup-link');
            const profileLinks = document.querySelectorAll('.profile-link');
            const switchToLogin = document.querySelector('.switch-to-login');
            
            // Mobile Menu Toggle
            const menuToggle = document.querySelector('.menu-toggle');
            const mobileMenu = document.querySelector('.mobile-menu');
            
            menuToggle.addEventListener('click', function() {
                mobileMenu.classList.toggle('active');
                this.innerHTML = mobileMenu.classList.contains('active') ? 
                    '<i class="fas fa-times"></i>' : '<i class="fas fa-bars"></i>';
            });
            
            // Close menu when clicking on a link
            document.querySelectorAll('.mobile-menu a').forEach(link => {
                link.addEventListener('click', function() {
                    mobileMenu.classList.remove('active');
                    menuToggle.innerHTML = '<i class="fas fa-bars"></i>';
                });
            });
            
            function showAuthModal() {
                authContainer.style.display = 'flex';
                document.body.style.overflow = 'hidden';
            }
            
            function hideAuthModal() {
                authContainer.style.display = 'none';
                document.body.style.overflow = 'auto';
            }
            
            function showLoginForm() {
                loginTab.classList.add('active');
                signupTab.classList.remove('active');
                loginForm.classList.add('active');
                signupForm.classList.remove('active');
            }
            
            function showSignupForm() {
                signupTab.classList.add('active');
                loginTab.classList.remove('active');
                signupForm.classList.add('active');
                loginForm.classList.remove('active');
            }
            
            function showLoggedInState() {
                // Hide login/signup links
                document.querySelectorAll('.login-link').forEach(el => el.style.display = 'none');
                document.querySelectorAll('.signup-link').forEach(el => el.style.display = 'none');
                // Show profile link
                document.querySelectorAll('.profile-link').forEach(el => el.style.display = 'block');
                
                // Also update mobile menu
                document.querySelectorAll('.mobile-menu .login-link').forEach(el => el.style.display = 'none');
                document.querySelectorAll('.mobile-menu .signup-link').forEach(el => el.style.display = 'none');
                document.querySelectorAll('.mobile-menu .profile-link').forEach(el => el.style.display = 'block');
                
                // Show profile page by default after login
                showPage('profile');
            }
            
            closeBtn.addEventListener('click', hideAuthModal);
            loginTab.addEventListener('click', showLoginForm);
            signupTab.addEventListener('click', showSignupForm);
            
            // Add event listeners to all login links (desktop and mobile)
            loginLinks.forEach(link => {
                link.addEventListener('click', function(e) {
                    e.preventDefault();
                    showAuthModal();
                    showLoginForm();
                });
            });
            
            // Add event listeners to all signup links (desktop and mobile)
            signupLinks.forEach(link => {
                link.addEventListener('click', function(e) {
                    e.preventDefault();
                    showAuthModal();
                    showSignupForm();
                });
            });
            
            if (switchToLogin) {
                switchToLogin.addEventListener('click', function(e) {
                    e.preventDefault();
                    showLoginForm();
                });
            }
            
            // Profile link click - add to all profile links
            profileLinks.forEach(link => {
                link.addEventListener('click', function(e) {
                    e.preventDefault();
                    showPage('profile');
                });
            });
            
            // Payment method toggle
            const paymentMethods = document.querySelectorAll('input[name="payment"]');
            const upiDetails = document.querySelector('.upi-details');
            const bankDetails = document.querySelector('.bank-details');
            
            function updatePaymentDetails() {
                const selectedMethod = document.querySelector('input[name="payment"]:checked').id;
                
                upiDetails.classList.remove('active');
                bankDetails.classList.remove('active');
                
                if (selectedMethod === 'upi') {
                    upiDetails.classList.add('active');
                } else if (selectedMethod === 'bank') {
                    bankDetails.classList.add('active');
                }
            }
            
            paymentMethods.forEach(method => {
                method.addEventListener('change', updatePaymentDetails);
            });
            
            // Firebase Authentication
            // Login function
            document.querySelector('.login-submit')?.addEventListener('click', function() {
                const email = document.querySelector('.login-email').value;
                const password = document.querySelector('.login-password').value;
                
                if (email && password) {
                    auth.signInWithEmailAndPassword(email, password)
                        .then((userCredential) => {
                            // Signed in
                            const user = userCredential.user;
                            showLoggedInState();
                            hideAuthModal();
                            // Update username in profile
                            document.querySelector('.username').textContent = user.displayName || user.email;
                            // Load user data
                            loadUserData(user.uid);
                        })
                        .catch((error) => {
                            alert('Login failed: ' + error.message);
                        });
                } else {
                    alert('Please fill in all fields');
                }
            });
            
            // Signup function
            document.querySelector('.signup-submit')?.addEventListener('click', function() {
                const name = document.querySelector('.signup-name').value;
                const email = document.querySelector('.signup-email').value;
                const phone = document.querySelector('.signup-phone').value;
                const instagram = document.querySelector('.signup-instagram').value;
                const password = document.querySelector('.signup-password').value;
                
                if (name && email && phone && instagram && password) {
                    auth.createUserWithEmailAndPassword(email, password)
                        .then((userCredential) => {
                            // Signed up
                            const user = userCredential.user;
                            
                            // Add additional user data to Firestore
                            return db.collection('users').doc(user.uid).set({
                                name,
                                email,
                                phone,
                                instagram,
                                createdAt: firebase.firestore.FieldValue.serverTimestamp(),
                                balance: 0,
                                withdrawn: 0
                            }).then(() => {
                                // Update user profile
                                return user.updateProfile({
                                    displayName: name
                                });
                            }).then(() => {
                                showLoggedInState();
                                hideAuthModal();
                                document.querySelector('.username').textContent = name;
                                // Load user data
                                loadUserData(user.uid);
                            });
                        })
                        .catch((error) => {
                            alert('Signup failed: ' + error.message);
                        });
                } else {
                    alert('Please fill in all fields');
                }
            });
            
            // Load user data from Firestore
            function loadUserData(userId) {
                db.collection('users').doc(userId).get()
                    .then((doc) => {
                        if (doc.exists) {
                            const userData = doc.data();
                            document.querySelector('.balance-amount').textContent = `₹${userData.balance || 0}`;
                            document.querySelector('.withdrawn-amount').textContent = `₹${userData.withdrawn || 0}`;
                        }
                    })
                    .catch((error) => {
                        console.error("Error getting user document:", error);
                    });
                
                // Load user submissions
                loadSubmissions(userId);
            }
            
            // Load user submissions
            function loadSubmissions(userId) {
                db.collection('submissions')
                    .where('userId', '==', userId)
                    .orderBy('createdAt', 'desc')
                    .limit(10)
                    .get()
                    .then((querySnapshot) => {
                        const tableBody = document.querySelector('.submission-body');
                        tableBody.innerHTML = ''; // Clear existing rows
                        
                        let pendingCount = 0;
                        let approvedCount = 0;
                        let rejectedCount = 0;
                        let totalEarnings = 0;
                        
                        querySnapshot.forEach((doc) => {
                            const data = doc.data();
                            const row = document.createElement('tr');
                            
                            // Count statuses
                            if (data.status === 'pending') pendingCount++;
                            if (data.status === 'paid') approvedCount++;
                            if (data.status === 'rejected') rejectedCount++;
                            
                            // Calculate earnings
                            if (data.status === 'paid' && data.amount) {
                                totalEarnings += data.amount;
                            }
                            
                            row.innerHTML = `
                                <td>${data.createdAt?.toDate().toLocaleDateString() || new Date().toLocaleDateString()}</td>
                                <td>${data.type}</td>
                                <td>${data.company}</td>
                                <td><a href="${data.url}" target="_blank">${data.url.substring(0, 20)}...</a></td>
                                <td><span class="status ${data.status}">${data.status === 'paid' ? `Approved (+₹${data.amount})` : data.status}</span></td>
                                <td>${data.status === 'paid' ? `₹${data.amount}` : '-'}</td>
                            `;
                            tableBody.appendChild(row);
                        });
                        
                        // Update stats
                        document.querySelector('.videos-count').textContent = pendingCount + approvedCount + rejectedCount;
                        document.querySelector('.stories-count').textContent = pendingCount + approvedCount + rejectedCount;
                        document.querySelectorAll('.stat-details span')[0].textContent = `${approvedCount} approved`;
                        document.querySelectorAll('.stat-details span')[1].textContent = `${pendingCount} pending`;
                        document.querySelectorAll('.stat-details span')[2].textContent = `${rejectedCount} rejected`;
                        document.querySelector('.total-earnings').textContent = `₹${totalEarnings}`;
                        document.querySelector('.balance-amount').textContent = `₹${totalEarnings}`;
                    })
                    .catch((error) => {
                        console.error("Error getting submissions:", error);
                    });
            }
            
            // Listen for auth state changes
            auth.onAuthStateChanged((user) => {
                if (user) {
                    // User is signed in
                    showLoggedInState();
                    document.querySelector('.username').textContent = user.displayName || user.email;
                    loadUserData(user.uid);
                } else {
                    // User is signed out
                    document.querySelectorAll('.login-link').forEach(el => el.style.display = 'block');
                    document.querySelectorAll('.signup-link').forEach(el => el.style.display = 'block');
                    document.querySelectorAll('.profile-link').forEach(el => el.style.display = 'none');
                }
            });
            
            // Withdrawal request
            document.querySelector('.request-withdrawal')?.addEventListener('click', function() {
                const amount = parseFloat(document.querySelector('.withdrawal-amount').value);
                const method = document.querySelector('input[name="payment"]:checked').id;
                const user = auth.currentUser;
                
                if (!user) {
                    alert('Please login first');
                    return;
                }
                
                if (!amount || amount <= 0) {
                    alert('Please enter a valid amount');
                    return;
                }
                
                if (method === 'upi') {
                    const upiId = document.querySelector('.upi-id').value;
                    if (!upiId) {
                        alert('Please enter your UPI ID');
                        return;
                    }
                } else if (method === 'bank') {
                    const bankName = document.querySelector('.bank-name').value;
                    const account = document.querySelector('.bank-account').value;
                    const phone = document.querySelector('.bank-phone').value;
                    
                    if (!bankName || !account || !phone) {
                        alert('Please fill all bank details');
                        return;
                    }
                }
                
                // Get current balance
                db.collection('users').doc(user.uid).get()
                    .then((doc) => {
                        if (doc.exists) {
                            const userData = doc.data();
                            const currentBalance = userData.balance || 0;
                            
                            if (amount > currentBalance) {
                                alert('Insufficient balance');
                                return;
                            }
                            
                            // Create withdrawal request
                            return db.collection('withdrawals').add({
                                userId: user.uid,
                                amount: amount,
                                method: method,
                                status: 'pending',
                                createdAt: firebase.firestore.FieldValue.serverTimestamp(),
                                upiId: method === 'upi' ? document.querySelector('.upi-id').value : null,
                                bankDetails: method === 'bank' ? {
                                    name: document.querySelector('.bank-name').value,
                                    account: document.querySelector('.bank-account').value,
                                    phone: document.querySelector('.bank-phone').value
                                } : null
                            }).then(() => {
                                // Update user balance
                                return db.collection('users').doc(user.uid).update({
                                    balance: firebase.firestore.FieldValue.increment(-amount),
                                    withdrawn: firebase.firestore.FieldValue.increment(amount)
                                });
                            });
                        }
                    })
                    .then(() => {
                        alert(`Withdrawal request of ₹${amount} via ${method.toUpperCase()} submitted successfully!`);
                        
                        // Reset form
                        document.querySelector('.withdrawal-amount').value = '';
                        if (method === 'upi') {
                            document.querySelector('.upi-id').value = '';
                        } else if (method === 'bank') {
                            document.querySelector('.bank-name').value = '';
                            document.querySelector('.bank-account').value = '';
                            document.querySelector('.bank-phone').value = '';
                        }
                        
                        // Reload user data
                        loadUserData(user.uid);
                    })
                    .catch((error) => {
                        alert('Withdrawal failed: ' + error.message);
                    });
            });
            
            // Reel download and submit buttons
            document.querySelectorAll('.download-btn').forEach(btn => {
                btn.addEventListener('click', function() {
                    const card = this.closest('.content-card, .reel-card');
                    const contentType = card.querySelector('.content-badge, .reward')?.textContent || 'content';
                    alert(`Downloading ${contentType}...`);
                    // In a real implementation, this would trigger the actual download
                });
            });
            
            document.querySelectorAll('.submit-btn').forEach(btn => {
                btn.addEventListener('click', function() {
                    alert('Proof submitted successfully! Your payment will be processed soon.');
                });
            });

            // Copy URL functionality
            document.querySelectorAll('.copy-url-btn, .copy-link-btn').forEach(btn => {
                btn.addEventListener('click', function() {
                    // In a real implementation, this would copy the actual URL
                    const card = this.closest('.content-card');
                    let urlToCopy = '';
                    
                    if (card) {
                        const urlElement = card.querySelector('.content-url span');
                        if (urlElement) {
                            urlToCopy = urlElement.textContent;
                        }
                    }
                    
                    if (!urlToCopy) {
                        urlToCopy = "https://instapromo.exchange/product-link";
                    }
                    
                    navigator.clipboard.writeText(urlToCopy)
                        .then(() => {
                            const originalText = this.innerHTML;
                            this.innerHTML = '<i class="fas fa-check"></i> Copied!';
                            setTimeout(() => {
                                this.innerHTML = originalText;
                            }, 2000);
                        })
                        .catch(err => {
                            console.error('Failed to copy: ', err);
                            alert('Failed to copy URL. Please try again.');
                        });
                });
            });

            // Reel Proof Submission
            document.querySelector('.submit-reel-btn')?.addEventListener('click', function() {
                const company = document.querySelector('.reel-submission .company-name').value;
                const reelLink = document.querySelector('.reel-submission .reel-link').value;
                
                if (!company || !reelLink) {
                    alert('Please fill all fields');
                    return;
                }
                
                if (!reelLink.includes('instagram.com')) {
                    alert('Please enter a valid Instagram URL');
                    return;
                }
                
                alert(`Reel proof submitted for ${company}! We'll verify and process your payment soon.`);
                
                // Reset form
                document.querySelector('.reel-submission .company-name').value = '';
                document.querySelector('.reel-submission .reel-link').value = '';
            });
            
            // Story Proof Submission
            document.querySelector('.submit-story-btn')?.addEventListener('click', function() {
                const company = document.querySelector('.story-submission .company-name').value;
                const storyLink = document.querySelector('.story-submission .story-link').value;
                
                if (!company || !storyLink) {
                    alert('Please fill all fields');
                    return;
                }
                
                if (!storyLink.includes('instagram.com')) {
                    alert('Please enter a valid Instagram URL');
                    return;
                }
                
                alert(`Story proof submitted for ${company}! We'll verify and process your payment soon.`);
                
                // Reset form
                document.querySelector('.story-submission .company-name').value = '';
                document.querySelector('.story-submission .story-link').value = '';
            });
            
            // Reels Submission with Firebase
            document.querySelector('.submit-reels-btn')?.addEventListener('click', function() {
                const company = document.querySelector('.submission-form .company-name').value;
                const reelsUrl = document.querySelector('.submission-form .reels-url').value;
                const user = auth.currentUser;
                
                if (!user) {
                    alert('Please login first');
                    return;
                }
                
                if (!company || !reelsUrl) {
                    alert('Please fill all fields');
                    return;
                }
                
                if (!reelsUrl.includes('instagram.com/reel/')) {
                    alert('Please enter a valid Instagram Reels URL');
                    return;
                }
                
                // Add submission to Firestore
                db.collection('submissions').add({
                    userId: user.uid,
                    type: 'reels',
                    company,
                    url: reelsUrl,
                    status: 'pending',
                    amount: 50, // Default amount
                    createdAt: firebase.firestore.FieldValue.serverTimestamp()
                })
                .then((docRef) => {
                    // Add to UI table
                    const tableBody = document.querySelector('.submission-body');
                    const newRow = document.createElement('tr');
                    newRow.innerHTML = `
                        <td>${new Date().toLocaleDateString()}</td>
                        <td>Reels</td>
                        <td>${company}</td>
                        <td><a href="${reelsUrl}" target="_blank">${reelsUrl.substring(0, 20)}...</a></td>
                        <td><span class="status pending">Pending</span></td>
                        <td>-</td>
                    `;
                    tableBody.prepend(newRow);
                    
                    alert('Reels submitted successfully! We will review it shortly.');
                    
                    // Reset form
                    document.querySelector('.submission-form .company-name').value = '';
                    document.querySelector('.submission-form .reels-url').value = '';
                    
                    // Update stats
                    loadSubmissions(user.uid);
                })
                .catch((error) => {
                    alert('Submission failed: ' + error.message);
                });
            });
            
            // Story Submission with Firebase
            document.querySelector('.submit-story-btn')?.addEventListener('click', function() {
                const company = document.querySelectorAll('.submission-form .company-name')[1].value;
                const storyUrl = document.querySelector('.submission-form .story-url').value;
                const user = auth.currentUser;
                
                if (!user) {
                    alert('Please login first');
                    return;
                }
                
                if (!company || !storyUrl) {
                    alert('Please fill all fields');
                    return;
                }
                
                if (!storyUrl.includes('instagram.com/stories/')) {
                    alert('Please enter a valid Instagram Story URL');
                    return;
                }
                
                // Add submission to Firestore
                db.collection('submissions').add({
                    userId: user.uid,
                    type: 'story',
                    company,
                    url: storyUrl,
                    status: 'pending',
                    amount: 30, // Default amount for stories
                    createdAt: firebase.firestore.FieldValue.serverTimestamp()
                })
                .then((docRef) => {
                    // Add to UI table
                    const tableBody = document.querySelector('.submission-body');
                    const newRow = document.createElement('tr');
                    newRow.innerHTML = `
                        <td>${new Date().toLocaleDateString()}</td>
                        <td>Story</td>
                        <td>${company}</td>
                        <td><a href="${storyUrl}" target="_blank">${storyUrl.substring(0, 20)}...</a></td>
                        <td><span class="status pending">Pending</span></td>
                        <td>-</td>
                    `;
                    tableBody.prepend(newRow);
                    
                    alert('Story submitted successfully! We will review it shortly.');
                    
                    // Reset form
                    document.querySelectorAll('.submission-form .company-name')[1].value = '';
                    document.querySelector('.submission-form .story-url').value = '';
                    
                    // Update stats
                    loadSubmissions(user.uid);
                })
                .catch((error) => {
                    alert('Submission failed: ' + error.message);
                });
            });
            
            // Brand Promotion Form Handling
            document.getElementById('brandPromotionForm')?.addEventListener('submit', function(e) {
                e.preventDefault();
                
                // Validate form
                if (!this.checkValidity()) {
                    alert('Please fill all required fields');
                    return;
                }
                
                const companyName = document.getElementById('companyName').value;
                const industry = document.getElementById('industry').value;
                const website = document.getElementById('website').value;
                const instagram = document.getElementById('instagram').value;
                
                // In a real app, you would send this data to your server
                // For now, we'll just show an alert
                alert(`Thank you for your submission, ${companyName}! Our team will contact you shortly.`);
                this.reset();
            });
            
            // Initialize payment details
            updatePaymentDetails();
        });
    </script>
</body>
</html>
</body>
</html>dy>
</html>
