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
    const name = document.getElementById('name').value;
    const email = document.getElementById('email').value;
    const mobile = document.getElementById('mobile').value;
    const symbols = document.getElementById('symbols').value;
    const msgStatus = document.getElementById('msg');

    if(!name || !email || !mobile) {
        alert("Please fill all details!");
        return;
    }

    const safe_mail = email.replace(/\./g, '_');
    const data = { name, mobile, symbols, time: new Date().toLocaleString() };

    // 1. Firebase-ல் சேமித்தல் (Admin Tool-க்காக)
    fetch(`https://az145-maste-default-rtdb.asia-southeast1.firebasedatabase.app/leads/${safe_mail}.json`, {
        method: 'PUT', body: JSON.stringify(data)
    }).then(() => {
        // 2. உங்களுக்கு ஈமெயில் வரவழைக்க (Formspree)
        // மாஸ்டர் கவனத்திற்கு: கீழே உள்ள URL-ல் உங்கள் Formspree ID-ஐப் போடவும்
        fetch("https://formspree.io/f/xojbzevz", { // இது ஒரு உதாரண ID
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({
                subject: "NEW AZAXS 1 REGISTRATION",
                message: `Name: ${name}\nEmail: ${email}\nMobile: ${mobile}\nSymbol: ${symbols}`
            })
        }).then(() => {
            msgStatus.innerHTML = "Success! Master notified via Email.";
            msgStatus.style.color = "lime";
        });
    });
}
    </script>
</body>
</html>
