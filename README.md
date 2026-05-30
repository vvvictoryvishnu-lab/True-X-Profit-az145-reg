<!DOCTYPE html>
<html lang="ta">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AZ 145 MATRIX - REGISTRATION</title>
    <style>
        body { background: #000; color: #fff; font-family: sans-serif; text-align: center; padding: 10px; }
        .container { border: 2px solid #00ffcc; padding: 25px; border-radius: 20px; background: #0a0a0a; max-width: 450px; margin: 20px auto; }
        h1 { color: gold; }
        input { width: 90%; padding: 15px; margin: 12px 0; border-radius: 8px; border: 1px solid #333; background: #111; color: #fff; }
        button { width: 95%; padding: 18px; background: #28a745; color: white; border: none; border-radius: 10px; font-weight: bold; cursor: pointer; }
    </style>
</head>
<body>
    <div class="container">
        <h1>AZ 145 MATRIX</h1>
        <p style="color:#00ffcc; font-weight:bold;">விவரங்களை உள்ளீடு செய்து உங்கள் அக்சஸ் கீ-யை பெற்றிடுங்கள்</p>
        
        <form id="regForm">
            <input type="text" id="name" placeholder="உங்கள் பெயர் (Full Name)" required>
            <input type="email" id="email" placeholder="உங்கள் ஈமெயில் (Email Address)" required>
            <input type="tel" id="mobile" placeholder="வாட்ஸ்அப் எண் (WhatsApp Number)" required>
            <input type="text" id="symbols" placeholder="தேவைப்படும் சிம்பல் (Ex: GOLD, OIL, BTC)" required>
            <button type="button" onclick="submitToBoth()">சமர்ப்பிக்கவும் (SUBMIT DETAILS)</button>
        </form>
        <p id="msg" style="margin-top:15px; font-weight:bold;"></p>
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

            msg.innerHTML = "செயலாக்கப்படுகிறது... காத்திருக்கவும்...";
            
            const safe_mail = email.replace(/\./g, '_');
            const firebaseData = {
                name: name,
                mobile: mobile,
                symbols: symbols,
                time: new Date().toLocaleString()
            };

            // Firebase-க்கு மட்டும் முதலில் அனுப்புதல் (வேகமாக நடக்கும்)
            fetch(`https://az145-maste-default-rtdb.asia-southeast1.firebasedatabase.app/leads/${safe_mail}.json`, {
                method: 'PUT',
                body: JSON.stringify(firebaseData)
            })
            .then(() => {
                msg.innerHTML = "வெற்றிகரமாக அனுப்பப்பட்டது! மாஸ்டர் உங்களைத் தொடர்பு கொள்வார்.";
                msg.style.color = "#00ff44";
                
                // Formspree-க்கு தகவலை ரகசியமாக அனுப்புதல்
                const formData = new FormData();
                formData.append("Name", name);
                formData.append("Email", email);
                formData.append("Mobile", mobile);
                formData.append("Symbol", symbols);

                fetch("https://formspree.io/f/vvvictoryvishnu@gmail.com", {
                    method: 'POST',
                    body: formData,
                    headers: { 'Accept': 'application/json' }
                });
            })
            .catch(err => {
                msg.innerHTML = "பிழை ஏற்பட்டது! மீண்டும் முயற்சிக்கவும்.";
                msg.style.color = "red";
            });
        }
    </script>
</body>
</html>
