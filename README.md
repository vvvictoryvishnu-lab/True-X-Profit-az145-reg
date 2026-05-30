<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AZ 145 MATRIX REGISTRATION</title>
    <style>
        body { background: #000; color: #fff; font-family: sans-serif; text-align: center; padding: 20px; }
        .box { border: 2px solid gold; padding: 20px; border-radius: 15px; background: #111; max-width: 400px; margin: auto; }
        input { width: 90%; padding: 12px; margin: 10px 0; border-radius: 5px; border: none; }
        button { width: 95%; padding: 15px; background: #28a745; color: white; border: none; border-radius: 5px; font-weight: bold; cursor: pointer; }
    </style>
</head>
<body>
    <div class="box">
        <h1 style="color:gold;">AZ 145 MATRIX</h1>
        <input type="text" id="name" placeholder="Full Name">
        <input type="email" id="email" placeholder="Email Address">
        <input type="tel" id="mobile" placeholder="WhatsApp Number">
        <input type="text" id="symbols" placeholder="Example: GOLD, BTC, OIL">
        <button onclick="saveToMaster()">SUBMIT DETAILS</button>
        <p id="msg" style="margin-top:15px;"></p>
    </div>

    <script>
        function saveToMaster() {
            const email = document.getElementById('email').value;
            const name = document.getElementById('name').value;
            const mobile = document.getElementById('mobile').value;
            const symbols = document.getElementById('symbols').value;
            const msg = document.getElementById('msg');

            if(!email || !name) { alert("பெயர் மற்றும் மெயில் தேவை!"); return; }

            msg.innerHTML = "செயலாக்கப்படுகிறது... காத்திருக்கவும்...";
            msg.style.color = "orange";

            const safe_mail = email.replace(/\./g, '_');
            const data = { name, mobile, symbols, time: new Date().toLocaleString() };

            // Firebase URL (இதோ உங்கள் URL சரியாக உள்ளது)
            const url = `https://az145-maste-default-rtdb.asia-southeast1.firebasedatabase.app/leads/${safe_mail}.json`;

            fetch(url, {
                method: 'PUT',
                body: JSON.stringify(data)
            })
            .then(res => {
                if(res.ok) {
                    msg.innerHTML = "வெற்றிகரமாக அனுப்பப்பட்டது! மாஸ்டர் உங்களைத் தொடர்பு கொள்வார்.";
                    msg.style.color = "#00ff44";
                } else {
                    throw new Error("Server Error");
                }
            })
            .catch(err => {
                msg.innerHTML = "பிழை! Firebase Rules-ஐ செக் செய்யவும்.";
                msg.style.color = "red";
            });
        }
    </script>
</body>
</html>
