<!DOCTYPE html>
<html lang="ta">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AZ 145 - MATRIX REGISTRATION</title>
    <style>
        body { background: #000; color: #fff; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; text-align: center; padding: 10px; }
        .container { border: 2px solid #00ffcc; padding: 25px; border-radius: 20px; background: #0a0a0a; max-width: 450px; margin: 20px auto; box-shadow: 0 0 20px #00ffcc; }
        h1 { color: gold; font-size: 28px; text-shadow: 2px 2px #000; }
        input { width: 90%; padding: 15px; margin: 12px 0; border-radius: 8px; border: 1px solid #333; background: #111; color: #fff; font-size: 16px; }
        button { width: 95%; padding: 18px; background: #28a745; color: white; border: none; border-radius: 10px; font-weight: bold; font-size: 18px; cursor: pointer; transition: 0.3s; }
        button:hover { background: #218838; transform: scale(1.02); }
        .footer { margin-top: 20px; font-size: 12px; color: #666; }
    </style>
</head>
<body>

    <div class="container">
        <h1>AZ 145 MATRIX</h1>
        <p style="color:#00ffcc; font-weight:bold;">இலவசமாக பதிவு செய்து ரகசிய கீ-யை பெற்றிடுங்கள்</p>
        
        <!-- Formspree ID: உங்கள் மெயிலுக்கு தகவல் வர இங்கு உங்கள் ஈமெயிலை மாற்றவும் -->
        <form id="regForm" action="https://formspree.io/f/vvvictoryvishnu@gmail.com" method="POST">
            <input type="text" name="Customer_Name" id="name" placeholder="உங்கள் பெயர் (Full Name)" required>
            <input type="email" name="Customer_Email" id="email" placeholder="உங்கள் ஈமெயில் (Email Address)" required>
            <input type="tel" name="WhatsApp_Number" id="mobile" placeholder="வாட்ஸ்அப் எண் (WhatsApp Number)" required>
            <input type="text" name="Target_Symbol" id="symbols" placeholder="தேவைப்படும் சிம்பல் (Ex: GOLD, OIL, BTC)" required>
            
            <button type="button" onclick="submitToBoth()">இப்போதே பதிவு செய் (REGISTER NOW)</button>
        </form>
        
        <p id="msg" style="margin-top:15px; font-weight:bold;"></p>
        <div class="footer">YouTube: True X Profit | Master Karthik</div>
    </div>

    <script>
        function submitToBoth() {
            const name = document.getElementById('name').value;
            const email = document.getElementById('email').value;
            const mobile = document.getElementById('mobile').value;
            const symbols = document.getElementById('symbols').value;
            const msg = document.getElementById('msg');

            if(!name || !email || !mobile || !symbols) {
                alert("அனைத்து விவரங்களையும் பூர்த்தி செய்யவும்!");
                return;
            }

            msg.innerHTML = "பதிவாகிறது... தயவுசெய்து காத்திருக்கவும்...";
            msg.style.color = "orange";

            // 1. Firebase-க்கு தகவலை அனுப்புதல் (அட்மின் டூலில் பார்க்க)
            const safe_mail = email.replace(/\./g, '_');
            const firebaseData = {
                name: name,
                mobile: mobile,
                symbols: symbols,
                time: new Date().toLocaleString(),
                status: "Pending"
            };

            fetch(`https://az145-maste-default-rtdb.asia-southeast1.firebasedatabase.app/leads/${safe_mail}.json`, {
                method: 'PUT',
                body: JSON.stringify(firebaseData)
            })
            .then(() => {
                // 2. Formspree மூலம் ஈமெயில் அனுப்புதல்
                const form = document.getElementById('regForm');
                const formData = new FormData(form);

                fetch(form.action, {
                    method: 'POST',
                    body: formData,
                    headers: { 'Accept': 'application/json' }
                }).then(response => {
                    if (response.ok) {
                        msg.innerHTML = "வாழ்த்துக்கள்! உங்கள் விவரங்கள் மாஸ்டருக்கு அனுப்பப்பட்டது.";
                        msg.style.color = "#00ff44";
                        
                        // போனஸ்: தானாகவே வாட்ஸ்அப் விண்டோ திறக்க
                        setTimeout(() => {
                            window.location.href = `https://wa.me/919677275371உங்கள்_வாட்ஸ்அப்_எண்?text=வணக்கம்_மாஸ்டர்_நான்_${name}_பதிவு_செய்துள்ளேன்_எனக்கான_கீ_தரவும்`;
                        }, 2000);
                    }
                });
            });
        }
    </script>
</body>
</html>
