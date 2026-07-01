<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>bKash Payment</title>
    <style>
        :root {
            --header-green: #005e3d;
            --bg-light: #fdfdfd;
            --text-red: #d32f2f;
            --bkash-pink: #d12053;
            --border-gray: #dddddd;
            --text-dark: #333333;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Arial, sans-serif; }

        body { background-color: #f2f2f2; color: var(--text-dark); }

        /* হেডার */
        .header {
            background-color: var(--header-green);
            color: white;
            padding: 12px 16px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: sticky;
            top: 0;
            z-index: 100;
        }
        .header-left .amount { font-size: 20px; font-weight: bold; }
        .header-left .sub { font-size: 12px; display: block; margin-top: 2px; }
        
        .header-right { display: flex; align-items: center; gap: 10px; }
        .pay-label { background: white; color: var(--header-green); padding: 2px 6px; border-radius: 4px; font-weight: 900; font-size: 14px; }

        /* মেইন কন্টেনার */
        .container { max-width: 450px; margin: 0 auto; background: var(--bg-light); min-height: 100vh; position: relative; box-shadow: 0 0 10px rgba(0,0,0,0.1); }

        .content { padding: 20px 16px; }

        /* সতর্কতা এরিয়া */
        .top-warning {
            color: var(--text-red);
            font-size: 15px;
            font-weight: bold;
            text-align: center;
            line-height: 1.4;
            margin-bottom: 25px;
            padding: 0 5px;
            animation: nagadBlink 1.2s linear infinite; 
        }

        @keyframes nagadBlink {
            50% { opacity: 0.3; transform: scale(0.98); }
        }

        /* বিকাশ সেকশন */
        .bkash-header {
            background-color: var(--bkash-pink);
            color: white;
            padding: 12px 15px;
            border-radius: 6px;
            display: flex;
            align-items: center;
            margin-bottom: 20px;
        }
        .bkash-logo-bg {
            background: white;
            border-radius: 50%;
            width: 45px;
            height: 45px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 15px;
            overflow: hidden;
        }
        .bkash-logo-bg img { 
            width: 100%; 
            height: 100%; 
            object-fit: cover;
        }
        .bkash-header span { font-size: 18px; font-weight: bold; }

        /* ইনপুট গ্রুপ */
        .input-group { margin-bottom: 20px; }
        .label-text { font-size: 15px; font-weight: bold; margin-bottom: 6px; display: block; color: #444; }
        .hint-text { font-size: 13px; color: #666; margin-bottom: 10px; display: block; }

        .copy-box {
            display: flex;
            border: 1px solid var(--border-gray);
            background: #f9f9f9;
            border-radius: 4px;
            overflow: hidden;
            align-items: center;
        }
        .number-field {
            flex-grow: 1;
            padding: 12px;
            font-size: 20px;
            font-weight: bold;
            letter-spacing: 1px;
            color: #000;
            user-select: all;
        }
        .copy-trigger {
            background: none;
            border: none;
            padding: 10px 15px;
            border-left: 1px solid var(--border-gray);
            cursor: pointer;
        }
        .copy-trigger img { width: 22px; opacity: 0.7; }

        /* ট্রানজেকশন ইনপুট */
        .trx-label { font-size: 15px; font-weight: bold; margin-bottom: 5px; display: inline-block; }
        .req-star { color: var(--text-red); font-size: 14px; }
        .how-link { font-size: 13px; color: #007bff; text-decoration: none; display: block; margin-bottom: 10px; }

        .form-control {
            width: 100%;
            padding: 14px;
            border: 1.5px solid var(--text-red);
            border-radius: 5px;
            font-size: 16px;
            margin-bottom: 10px;
            outline: none;
        }

        /* বড় এবং স্পষ্ট বাটন */
        .submit-btn {
            width: 100%;
            padding: 16px;
            background: #2c3e50;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 18px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 15px;
            margin-bottom: 30px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.2);
            transition: 0.2s;
            opacity: 1;
        }
        .submit-btn:hover { background: #1a252f; }
        .submit-btn:active { transform: scale(0.98); }

        /* ফুটো গাইডলাইন */
        .footer-guide { border-top: 1px solid #eee; padding-top: 20px; }
        .guide-title { font-size: 18px; font-weight: bold; margin-bottom: 12px; }
        
        .moving-warning { 
            color: var(--text-red); 
            font-size: 13px; 
            font-weight: bold; 
            margin-bottom: 15px; 
            display: block; 
            line-height: 1.4;
            padding: 5px 0;
            animation: nagadBlink 1.5s infinite;
        }
        
        .guide-p { font-size: 13.5px; color: #444; line-height: 1.6; margin-bottom: 15px; }
        .guide-link { color: #008000; font-weight: bold; text-decoration: underline; cursor: pointer; }

        /* Success Modal */
        .success-overlay {
            display: none; position: fixed; top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0,0,0,0.8); z-index: 1000; align-items: center; justify-content: center;
        }
        .success-overlay.active { display: flex !important; }
        
        .modal-card { background: white; width: 90%; max-width: 320px; padding: 30px; border-radius: 15px; text-align: center; }
        .modal-card h2 { color: #005e3d; margin: 15px 0 10px; }
        .modal-card p { font-size: 14px; color: #666; margin-bottom: 20px; }
        .done-btn { background: var(--header-green); color: white; border: none; padding: 12px 30px; border-radius: 5px; font-weight: bold; cursor: pointer; }
    </style>
</head>
<body oncontextmenu="return false;">

<div class="container">
    <div class="header">
        <div class="header-left">
            <span class="amount">bKash Payment</span>
            <span class="sub">Send Money to Agent</span>
        </div>
        <div class="header-right">
            <span class="pay-label">Pay</span>
            <span style="font-weight:bold; font-size:14px; letter-spacing:1px;">SERVICE</span>
        </div>
    </div>

    <div class="content">
        <p class="top-warning">
            নিচের নাম্বারে যেকোনো পরিমাণ টাকা সেন্ড মানি করুন
        </p>

        <div class="bkash-header">
            <div class="bkash-logo-bg">
                <img src="https://files.catbox.moe/k47vog.png" alt="bkash">
            </div>
            <span>bKash &nbsp; Deposit</span>
        </div>

        <div class="input-group">
            <label class="label-text">বিকাশ এজেন্ট নাম্বার (Send money)</label>
            <span class="hint-text">এই BKASH নাম্বারে সেন্ড মানি করুন</span>
            <div class="copy-box">
                <div class="number-field" id="walletNumberDisplay">01864042589</div>
                <button type="button" class="copy-trigger" id="copyBtn">
                    <img src="https://cdn-icons-png.flaticon.com/512/1621/1621635.png" alt="copy">
                </button>
            </div>
        </div>

        <form id="depositSubmitForm">
            <div class="input-group">
                <label class="trx-label">Transaction ID <span class="req-star">* (required)</span></label>
                <a href="#" class="how-link">ট্রানজেকশন আইডি কীভাবে পাবেন?</a>
                <input type="text" id="trxInput" class="form-control" placeholder="Transaction ID" required>
            </div>

            <button type="submit" class="submit-btn" id="submitBtn">নিশ্চিত করুন</button>
        </form>

        <div class="footer-guide">
            <div class="guide-title">সতর্কতা</div>
            <div class="moving-warning">
                লেনদেন আইডি সঠিকভাবে পূরণ করতে হবে, অন্যথায় স্কোর ব্যর্থ হবে! !
            </div>
            
            <p class="guide-p">
                অনুগ্রহ করে নিশ্চিত হয়ে নিন যে আপনি BKASH ওয়ালেট নাম্বারে সেন্ড মানি করছেন। এই নাম্বারের অন্য কোন ওয়ালেট থেকে সেন্ড মানি করলে সেই টাকা পাওয়ার কোন সম্ভাবনা নাই
            </p>

            <p class="guide-p" style="border-top: 1px solid #eee; padding-top: 15px;">
                আপনি ট্রানজেকশন আইডি দেখতে পারেন <span class="guide-link">এখানে ক্লিক করুন</span> এবং নিশ্চিত যে আপনি একই ওয়ালেট থেকে পেমেন্ট করেন।
            </p>
        </div>
    </div>
</div>

<div class="success-overlay" id="successModal">
    <div class="modal-card">
        <div style="font-size: 50px;">✅</div>
        <h2>Submitted!</h2>
        <p>আপনার অনুরোধটি সফলভাবে জমা হয়েছে। ২-৫ মিনিটের মধ্যে যাচাই করা হবে।</p>
        <button type="button" class="done-btn" id="finalDoneBtn">ঠিক আছে</button>
    </div>
</div>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.10/firebase-app.js";
    import { getDatabase, ref, push, set } from "https://www.gstatic.com/firebasejs/9.6.10/firebase-database.js";

    const firebaseConfig = {
      apiKey: "AIzaSyAsZlkP9qo1uXu7i55e6bV9Xg7pHedlOHk",
      authDomain: "dk-club-1d649.firebaseapp.com",
      databaseURL: "https://dk-club-1d649-default-rtdb.firebaseio.com",
      projectId: "dk-club-1d649",
      storageBucket: "dk-club-1d649.firebasestorage.app",
      messagingSenderId: "1097896403968",
      appId: "1:1097896403968:web:ac1b8c071ee225cab3a4e5",
      measurementId: "G-RT3FLNP8FG"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    // আপনার নাম্বার
    const walletNumber = "01864042589";
    document.getElementById('walletNumberDisplay').innerText = walletNumber;

    // কপি লজিক - সব ব্রাউজারে কাজ করবে
    document.getElementById('copyBtn').addEventListener('click', function() {
        // প্রথমে আধুনিক মেথড চেষ্টা করব
        if (navigator.clipboard && navigator.clipboard.writeText) {
            navigator.clipboard.writeText(walletNumber).then(() => {
                alert("নাম্বার কপি হয়েছে: " + walletNumber);
            }).catch(() => {
                fallbackCopy();
            });
        } else {
            // পুরনো মেথড
            fallbackCopy();
        }
    });

    // ফলব্যাক কপি ফাংশন
    function fallbackCopy() {
        const textarea = document.createElement('textarea');
        textarea.value = walletNumber;
        textarea.style.position = 'fixed';
        textarea.style.opacity = '0';
        document.body.appendChild(textarea);
        textarea.select();
        textarea.setSelectionRange(0, 99999);
        
        try {
            const successful = document.execCommand('copy');
            if (successful) {
                alert("নাম্বার কপি হয়েছে: " + walletNumber);
            } else {
                alert("কপি করা যায়নি। দয়া করে ম্যানুয়ালি কপি করুন।");
            }
        } catch (err) {
            alert("কপি করা যায়নি। দয়া করে ম্যানুয়ালি কপি করুন।");
        }
        
        document.body.removeChild(textarea);
    }

    // সাবমিশন লজিক
    document.getElementById('depositSubmitForm').addEventListener('submit', async function(e) {
        e.preventDefault();
        const submitBtn = document.getElementById('submitBtn');
        const trxInput = document.getElementById('trxInput');
        const trxId = trxInput.value.trim();

        if(!trxId) return alert("দয়া করে ট্রানজেকশন আইডি লিখুন!");

        submitBtn.disabled = true;
        submitBtn.innerText = "প্রসেসিং...";
        submitBtn.style.opacity = "0.8";

        try {
            const depositsRef = ref(db, 'deposits');
            const newDepositRef = push(depositsRef);
            
            await set(newDepositRef, {
                phone: "Guest",
                amount: "Any",
                method: "bKash",
                receiver: walletNumber,
                txid: trxId,
                status: "Pending",
                date: new Date().toLocaleString()
            });

            document.getElementById('successModal').style.display = 'flex';
            document.getElementById('successModal').classList.add('active');
            trxInput.value = "";
        } catch (err) { 
            console.error("Submission Error:", err);
            alert("Error: " + err.message); 
        } finally { 
            submitBtn.disabled = false; 
            submitBtn.innerText = "নিশ্চিত করুন"; 
            submitBtn.style.opacity = "1";
        }
    });

    // গেমের লিংক
    document.getElementById('finalDoneBtn').addEventListener('click', function() {
        window.location.href = 'https://gold-club-71.vercel.app/';
    });
</script>

</body>
</html>
