<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Royal Mail Receipt</title>
    <style>
        @page { size: 80mm auto; margin: 0; }
        body { width: 76mm; margin: 0 auto; font-family: 'Letter Gothic', 'Courier New', monospace; font-size: 9pt; padding: 5px; }
        
        .ocr-font { font-family: 'Letter Gothic', 'Courier New', monospace; font-size: 10pt; font-weight: bold; letter-spacing: 0.5px; }
        
        .post-office-header { text-align: center; font-weight: bold; margin-bottom: 10px; border-bottom: 2px solid #000; padding-bottom: 5px; text-transform: uppercase; }
        .address-box { margin-bottom: 15px; }
        .address-line { margin: 1px 0; }

        .top-info { margin-left: 10px; margin-right: 15px; margin-bottom: 20px; }
        .top-row { display: flex; justify-content: space-between; margin: 2px 0; }
        
        .header-info { border-bottom: 2px solid #000; margin-bottom: 5px; padding-bottom: 5px; }
        .row { display: flex; justify-content: space-between; margin-top: 2px; }
        .entry { margin-top: 8px; padding-bottom: 2px; }
        .center { text-align: center; margin-top: 5px; }
        .bold { font-weight: bold; }
        
        .extra-bold { font-weight: 900; }
        
        .footer-note { font-size: 7.5pt; margin-top: 5px; text-align: center; line-height: 1.1; font-style: italic; }
        .footer-line { border-top: 1px dashed #000; margin-top: 5px; margin-bottom: 10px; }
        
        .no-print { margin-bottom: 20px; padding: 10px; border-bottom: 2px solid #000; background: #f0f0f0; }
        @media print { .no-print { display: none; } }
    </style>
</head>
<body>

    <div class="no-print">
        <h3>Receipt Generator</h3>
        <button onclick="generateRealTimeData()" style="background: #4CAF50; color: white; padding: 10px; width: 100%; margin-bottom: 5px;">Generate Random & Real-Time Data</button>
        <input type="text" id="postDate" placeholder="Date"><br>
        <input type="text" id="postTime" placeholder="Time"><br>
        <input type="text" id="sessId" placeholder="Session ID"><br>
        <hr>
        <input type="text" id="qty" placeholder="Quantity"><br>
        <input type="text" id="price" placeholder="Price"><br>
        <input type="text" id="weight" placeholder="Weight"><br>
        <input type="text" id="ref" placeholder="Reference Code"><br>
        <input type="text" id="bld" placeholder="Building No"><br>
        <input type="text" id="pc" placeholder="Postcode"><br>
        <button onclick="addEntry()">Add to Receipt</button>
        <button onclick="addFooter()" style="background: #2196F3; color: white;">Add Footer</button>
        <button onclick="addTerms()" style="background: #e91e63; color: white;">Add Terms</button>
        <button onclick="window.print()">Print Receipt</button>
    </div>

    <div id="receipt-container">
        <div class="post-office-header">
            Post Office Ltd.<br>
            CERTIFICATE OF POSTING
        </div>
        <div class="address-box">
            <div class="address-line">Crowndale Road</div>
            <div class="address-line">18-22 Crowndale Road</div>
            <div class="address-line">London</div>
            <div class="address-line">Greater London</div>
            <div class="address-line">NW1 1TT</div>
        </div>

        <div class="top-info">
            <div class="top-row">
                <span>Posting date:</span>
                <span><span id="dispDate"></span> <span id="dispTime"></span></span>
            </div>
            <div class="top-row">
                <span>Session ID:</span>
                <span id="dispSess"></span>
            </div>
            <div class="top-row">
                <span>After last acceptance time?</span>
                <span>N</span>
            </div>
        </div>
    </div>

    <script>
        function generateRealTimeData() {
            // Real Time & Date
            const now = new Date().toLocaleString("en-GB", {timeZone: "Europe/London"});
            const parts = now.split(', ');
            document.getElementById('postDate').value = parts[0];
            document.getElementById('postTime').value = parts[1].substring(0, 5);
            document.getElementById('sessId').value = "2-" + Math.floor(100000 + Math.random() * 900000);
            
            // Random Reference
            const prefixes = ["SA", "SF", "MZ", "SG"];
            document.getElementById('ref').value = prefixes[Math.floor(Math.random() * prefixes.length)] + Math.floor(100000000 + Math.random() * 900000000) + "GB";
            
            // Random Building No
            document.getElementById('bld').value = Math.floor(Math.random() * 90) + 10;
            
            // Random UK Postcode Generator
            const letters = "ABCDEFGHJKLMNPQRSTUVWXYZ";
            const randomLetter = () => letters.charAt(Math.floor(Math.random() * letters.length));
            document.getElementById('pc').value = randomLetter() + randomLetter() + Math.floor(Math.random()*9) + " " + Math.floor(Math.random()*9) + randomLetter() + randomLetter();
        }

        function addEntry() {
            const container = document.getElementById('receipt-container');
            document.getElementById('dispDate').innerText = document.getElementById('postDate').value;
            document.getElementById('dispTime').innerText = document.getElementById('postTime').value;
            document.getElementById('dispSess').innerText = document.getElementById('sessId').value;

            const qty = document.getElementById('qty').value;
            const price = document.getElementById('price').value;
            const weight = document.getElementById('weight').value;
            const bld = document.getElementById('bld').value;
            const pc = document.getElementById('pc').value;
            const ref = document.getElementById('ref').value;

            if (qty && price) {
                container.innerHTML += `
                <div class="header-info">
                    <div class="row"><span>Destination Country</span> <span>UK (EU)</span></div>
                    <div class="row"><span>Address Validated?</span> <span>N</span></div>
                    <div class="row"><span class="bold">${qty} Special D by 1</span> <span>£${price}</span></div>
                    <div class="row"><span>Small Parcel</span> <span></span></div>
                    <div class="row"><span>Weight</span> <span>${weight}</span></div>
                </div>`;
            }

            container.innerHTML += `
                <div class="entry">
                    <div class="center">
                        <div class="bold">Reference number</div>
                        <div class="ocr-font">${ref}</div>
                    </div>
                    <div class="row" style="margin-top:5px;">
                        <span class="extra-bold">Building Name or Number</span> <span class="extra-bold">Postcode</span>
                    </div>
                    <div class="row">
                        <span class="ocr-font">${bld}</span> 
                        <span class="ocr-font">${pc}</span>
                    </div>
                </div>`;
        }
        
        function addFooter() {
            document.getElementById('receipt-container').innerHTML += `
                <div class="footer-note">
                    Next day guaranteed delivery service.<br>
                    Tracking and signature at royalmail.com.
                </div>
                <div class="footer-line"></div>`;
        }

        function addTerms() {
            document.getElementById('receipt-container').innerHTML += `
                <div style="margin-top: 10px; text-align: center; font-size: 8pt; font-family: 'Letter Gothic', 'Courier New', monospace;">
                    <p>PLEASE REFER TO SEPARATE TERMS AND CONDITIONS</p>
                    <p>For information about Royal Mail services or if you purchased a service that offers tracking, proof of delivery with signature or online delivery confirmation, please visit www.royalmail.com</p>
                    <p>PLEASE RETAIN AS YOUR PROOF OF POSTING<br>
                    This is not a financial receipt<br>
                    Thank You</p>
                </div>`;
        }
    </script>
</body>
</html>
