<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>sub stration calculation</title>
    <style>
        * {
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        body {
            background-color: #f0f4f8;
            margin: 0;
            padding: 20px;
            color: #2d3748;
        }
        .container {
            max-width: 950px;
            margin: 0 auto;
            background: #ffffff;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }
        h1 {
            text-align: center;
            color: #1a365d;
            margin-bottom: 20px;
            text-transform: uppercase;
        }
        .card {
            background: #f7fafc;
            border: 1px solid #e2e8f0;
            border-radius: 8px;
            padding: 20px;
            margin-bottom: 20px;
        }
        .card h2 {
            margin-top: 0;
            color: #2b6cb0;
            font-size: 1.2rem;
            border-bottom: 2px solid #e2e8f0;
            padding-bottom: 8px;
        }
        .grid-form {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
        }
        @media (max-width: 600px) {
            .grid-form {
                grid-template-columns: 1fr;
            }
        }
        .form-group {
            display: flex;
            flex-direction: column;
        }
        label {
            font-weight: 600;
            margin-bottom: 5px;
            font-size: 0.9rem;
        }
        input, select {
            padding: 10px;
            border: 1px solid #cbd5e0;
            border-radius: 5px;
            font-size: 0.95rem;
        }
        .btn-calc {
            width: 100%;
            padding: 12px;
            background-color: #2b6cb0;
            color: white;
            border: none;
            border-radius: 6px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            margin-top: 15px;
        }
        .btn-calc:hover {
            background-color: #2c5282;
        }
        .action-buttons {
            display: none;
            gap: 10px;
            margin-top: 20px;
        }
        .btn-pdf {
            flex: 1;
            padding: 12px;
            background-color: #e53e3e;
            color: white;
            border: none;
            border-radius: 6px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
        }
        .btn-pdf:hover {
            background-color: #c53030;
        }
        .btn-print {
            flex: 1;
            padding: 12px;
            background-color: #38a169;
            color: white;
            border: none;
            border-radius: 6px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
        }
        .btn-print:hover {
            background-color: #2f855a;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
        }
        
        /* Table Text Bold Styling */
        th, td {
            border: 1px solid #cbd5e0;
            padding: 8px 10px;
            text-align: left;
            font-size: 0.88rem;
            font-weight: bold; /* টেবিলের সকল টেক্সট বোল্ড করা হলো */
        }
        th {
            background-color: #2b6cb0;
            color: white;
            font-weight: 700;
        }
        tr:nth-child(even) {
            background-color: #f7fafc;
        }
        
        /* Custom Color Rules for Selected Mode */
        tr.highlight-green {
            background-color: #c6f6d5 !important; /* Soft Green */
        }
        tr.highlight-yellow {
            background-color: #fefcbf !important; /* Soft Yellow */
        }

        .user-badge {
            background-color: #cbd5e0;
            color: #2d3748;
            padding: 2px 6px;
            border-radius: 4px;
            font-size: 0.75rem;
            font-weight: bold;
            margin-left: 5px;
        }
        .display-input-list {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        .display-input-list li {
            padding: 6px 0;
            border-bottom: 1px dashed #e2e8f0;
        }
        #output-section {
            display: none;
        }

        /* Print Rules to retain colors in PDF */
        @media print {
            body {
                background-color: #fff;
                padding: 0;
                -webkit-print-color-adjust: exact !important;
                print-color-adjust: exact !important;
            }
            .container {
                box-shadow: none;
                padding: 0;
                max-width: 100%;
            }
            .input-card, .btn-calc, .action-buttons {
                display: none !important;
            }
            #output-section {
                display: block !important;
            }
            tr.highlight-green {
                background-color: #c6f6d5 !important;
            }
            tr.highlight-yellow {
                background-color: #fefcbf !important;
            }
            tr {
                page-break-inside: avoid !important;
            }
        }
    </style>
    <!-- HTML to PDF Library -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
</head>
<body>

<div class="container" id="pdf-content">
    <h1>sub stration calculation</h1>
    
    <!-- User Input Form Card -->
    <div class="card input-card">
        <h2>User Inputs</h2>
        <div class="grid-form">
            <div class="form-group">
                <label for="maxLoadDemand">1. Maximum Load Demand (kW):</label>
                <input type="number" id="maxLoadDemand" placeholder="e.g. 150" step="any" required>
            </div>
            <div class="form-group">
                <label for="cableFactor">8. Cable Material Factor (K):</label>
                <select id="cableFactor">
                    <option value="0.143">Copper (K = 0.143)</option>
                    <option value="0.098">Aluminium (K = 0.098)</option>
                </select>
            </div>
            <div class="form-group">
                <label for="riskFactor">11. Risk Factor:</label>
                <input type="number" id="riskFactor" placeholder="e.g. 1.2" step="any" required>
            </div>
            <div class="form-group">
                <label for="percentageImpedance">16. Percentage Impedance (%):</label>
                <input type="number" id="percentageImpedance" placeholder="e.g. 5" step="any" required>
            </div>
            <!-- Color Mode Option Dropdown -->
            <div class="form-group" style="grid-column: span 2;">
                <label for="colorOption">Table Color Style:</label>
                <select id="colorOption">
                    <option value="no-color">No Color (Default)</option>
                    <option value="color">Color Output (Green & Yellow Highlights)</option>
                </select>
            </div>
        </div>
        <button class="btn-calc" onclick="calculateSubstation()" type="button">Calculate Substation</button>
    </div>

    <!-- Output Section -->
    <div id="output-section">
        <!-- User Inputs Summary Card -->
        <div class="card">
            <h2>User Inputs Summary</h2>
            <ul class="display-input-list">
                <li><strong>1. Maximum Load Demand:</strong> <span id="disp-load">-</span> kW</li>
                <li><strong>8. Cable Material Factor (K):</strong> <span id="disp-k">-</span></li>
                <li><strong>11. Risk Factor:</strong> <span id="disp-risk">-</span></li>
                <li><strong>16. Percentage Impedance (%):</strong> <span id="disp-imp">-</span> %</li>
            </ul>
        </div>

        <!-- Full 1 to 21 Calculation Table -->
        <div class="card">
            <h2>Full Calculation Table (Items 1 to 21)</h2>
            <table>
                <thead>
                    <tr>
                        <th style="width: 8%;">SL</th>
                        <th style="width: 54%;">Calculation Parameter</th>
                        <th style="width: 38%;">Calculated Value / Rating</th>
                    </tr>
                </thead>
                <tbody id="result-table-body">
                    <!-- Dynamic Table Content -->
                </tbody>
            </table>
        </div>
    </div>
</div>

<!-- Download / Print Options -->
<div style="max-width: 950px; margin: 0 auto;">
    <div class="action-buttons" id="action-buttons">
        <button class="btn-pdf" onclick="downloadPDF()">Download PDF (1-21 Table)</button>
        <button class="btn-print" onclick="window.print()">Print / Save PDF (Browser)</button>
    </div>
</div>

<script>
    // Logic 6: Standard Transformer Rating
    function getStandardTransformerRating(val) {
        if (val <= 110) return 100;
        if (val <= 165) return 150;
        if (val <= 220) return 200;
        if (val <= 270) return 250;
        if (val <= 320) return 315;
        if (val <= 400) return 400;
        if (val <= 520) return 500;
        if (val <= 650) return 630;
        if (val <= 820) return 800;
        if (val <= 1030) return 1000;
        if (val <= 1300) return 1250;
        if (val <= 1700) return 1630;
        if (val <= 2050) return 2000;
        if (val <= 2600) return 2500;
        if (val <= 3100) return 3000;
        return 3000;
    }

    // Logic 14: Circuit Breaker Size
    function getCircuitBreakerSize(val) {
        if (val <= 174) return "CB 200 A";
        if (val <= 261) return "CB 250 A";
        if (val <= 400) return "CB 400 A";
        if (val <= 500) return "CB 500 A";
        if (val <= 600) return "CB 600 A";
        if (val <= 800) return "CB 800 A";
        if (val <= 1100) return "CB 1000 A";
        if (val <= 1300) return "CB 1250 A";
        if (val <= 1600) return "CB 1600 A";
        if (val <= 2000) return "CB 2000 A";
        if (val <= 2500) return "CB 2500 A";
        if (val <= 3000) return "CB 3000 A";
        if (val <= 4000) return "CB 4000 A";
        if (val <= 5000) return "CB 5000 A";
        if (val <= 6300) return "CB 6300 A";
        return "Custom High Size CB";
    }

    // Logic 19: Primary Cable Size (HT)
    function getPrimaryCableSize(rating) {
        switch(rating) {
            case 100: case 150: case 200: case 250: case 315: case 400: return "50 RM";
            case 500: return "70 RM";
            case 630: return "95 RM";
            case 800: case 1000: return "120 RM";
            case 1250: return "150 RM";
            case 1630: return "185 RM";
            case 2000: return "240 RM";
            case 2500: return "300 RM";
            case 3000: return "400 RM";
            default: return "50 RM";
        }
    }

    // Logic 20: Secondary Cable Size (LT)
    function getSecondaryCableSize(rating) {
        switch(rating) {
            case 100: return "50 RM";
            case 150: return "95 RM";
            case 200: return "150 RM";
            case 250: return "240 RM";
            case 315: return "400 RM";
            case 400: return "500 RM";
            case 500: return "2(1CX300) RM";
            case 630: return "2(1CX300) RM";
            case 800: return "2(1CX500) RM";
            case 1000: return "3(1CX400) RM";
            case 1250: return "3(1CX400) RM";
            case 1630: return "4(1CX500) RM";
            case 2000: return "5(1CX500) RM";
            case 2500: return "6(1CX500) RM";
            case 3000: return "8(1CX500) RM";
            default: return "N/A";
        }
    }

    function calculateSubstation() {
        const maxLoadDemand = parseFloat(document.getElementById('maxLoadDemand').value); // Item 1
        const cableFactor = parseFloat(document.getElementById('cableFactor').value); // Item 8
        const riskFactor = parseFloat(document.getElementById('riskFactor').value); // Item 11
        const percentageImpedance = parseFloat(document.getElementById('percentageImpedance').value); // Item 16
        const colorOption = document.getElementById('colorOption').value; // Selected Color Option

        if (isNaN(maxLoadDemand) || isNaN(riskFactor) || isNaN(percentageImpedance)) {
            alert("অনুগ্রহ করে সকল ইনপুট সঠিকভাবে লিখুন!");
            return;
        }

        // Formula Calculations
        const powerFactor = 0.8; // Item 2
        const maxApparentPower = maxLoadDemand / powerFactor; // Item 3
        const loadingPercentage = 0.85; // Item 4
        const calcTransformerRating = maxApparentPower / loadingPercentage; // Item 5
        const stdTransformerRating = getStandardTransformerRating(calcTransformerRating); // Item 6
        const shortCircuitImpedanceZ = 6; // Item 7
        const faultClearingTimeT = 3; // Item 9
        const primaryCurrent = stdTransformerRating * 0.05249; // Item 10
        const secondaryCurrent = stdTransformerRating * 1.39; // Item 12
        const finalCurrentSecondary = riskFactor * secondaryCurrent; // Item 13
        const circuitBreakerSize = getCircuitBreakerSize(finalCurrentSecondary); // Item 14
        const safetyFactor = 10; // Item 15
        const shortCircuitCurrentKA = ((primaryCurrent / percentageImpedance) * safetyFactor) / 1000; // Item 17
        const primaryCableHTCalculated = (shortCircuitCurrentKA * 1.732050) / cableFactor; // Item 18
        const primaryCableSizeUsing = getPrimaryCableSize(stdTransformerRating); // Item 19
        const secondaryCableSizeUsing = getSecondaryCableSize(stdTransformerRating); // Item 20
        const pfi = stdTransformerRating * 0.6; // Item 21

        // Display Summary
        document.getElementById('disp-load').innerText = maxLoadDemand;
        document.getElementById('disp-k').innerText = cableFactor + (cableFactor === 0.143 ? " (Copper)" : " (Aluminium)");
        document.getElementById('disp-risk').innerText = riskFactor;
        document.getElementById('disp-imp').innerText = percentageImpedance;

        // Rows Array (1-21)
        const displayData = [
            { sl: 1, name: "Maximum Load Demand", val: maxLoadDemand + " kW", isInput: true },
            { sl: 2, name: "Power Factor", val: powerFactor, isInput: false },
            { sl: 3, name: "Maximum Apparent Power (1 / 2)", val: maxApparentPower.toFixed(2) + " kVA", isInput: false },
            { sl: 4, name: "Loading Percentage", val: (loadingPercentage * 100) + "%", isInput: false },
            { sl: 5, name: "Calculated Transformer Rating (3 / 4)", val: calcTransformerRating.toFixed(2) + " kVA", isInput: false },
            { sl: 6, name: "Selected Standard Transformer Rating", val: stdTransformerRating + " kVA", isInput: false },
            { sl: 7, name: "Short Circuit Impedance (%) Z", val: shortCircuitImpedanceZ + "%", isInput: false },
            { sl: 8, name: "Cable Factor (K)", val: cableFactor, isInput: true },
            { sl: 9, name: "Fault Clearing Time (T)", val: faultClearingTimeT + " sec", isInput: false },
            { sl: 10, name: "Primary Current, Ip (6 * 0.05249)", val: primaryCurrent.toFixed(2) + " A", isInput: false },
            { sl: 11, name: "Risk Factor", val: riskFactor, isInput: true },
            { sl: 12, name: "Secondary Current, Ic (6 * 1.39)", val: secondaryCurrent.toFixed(2) + " A", isInput: false },
            { sl: 13, name: "Final Current Rating For Secondary (11 * 12)", val: finalCurrentSecondary.toFixed(2) + " A", isInput: false },
            { sl: 14, name: "Circuit Breaker Size", val: circuitBreakerSize, isInput: false },
            { sl: 15, name: "Safety Factor", val: safetyFactor, isInput: false },
            { sl: 16, name: "Percentage Impedance (%)", val: percentageImpedance + "%", isInput: true },
            { sl: 17, name: "Short Circuit Current, Ish = ((10 / 16) * 15) / 1000", val: shortCircuitCurrentKA.toFixed(4) + " kA", isInput: false },
            { sl: 18, name: "Calculated HT Cable Size = (17 * 1.732050) / 8", val: primaryCableHTCalculated.toFixed(2) + " sq.mm", isInput: false },
            { sl: 19, name: "Primary Cable Size For Using (HT)", val: primaryCableSizeUsing, isInput: false },
            { sl: 20, name: "Secondary Cable Size For Using (LT)", val: secondaryCableSizeUsing, isInput: false },
            { sl: 21, name: "PFI Rating (6 * 0.6)", val: pfi.toFixed(2) + " KVAR", isInput: false }
        ];

        // Specific Item Groups For Colors
        const greenSLs = [6, 10, 13, 14, 19, 20];
        const yellowSLs = [3, 5, 12, 17, 18];

        // Populate Table Rows with Bold Text
        const tbody = document.getElementById('result-table-body');
        tbody.innerHTML = "";
        displayData.forEach(row => {
            const tr = document.createElement('tr');
            
            // Apply selected color
            if (colorOption === 'color') {
                if (greenSLs.includes(row.sl)) {
                    tr.classList.add('highlight-green');
                } else if (yellowSLs.includes(row.sl)) {
                    tr.classList.add('highlight-yellow');
                }
            }

            const badge = row.isInput ? `<span class="user-badge">Input</span>` : "";
            tr.innerHTML = `
                <td><strong>${row.sl}</strong></td>
                <td><strong>${row.name}</strong> ${badge}</td>
                <td><strong>${row.val}</strong></td>
            `;
            tbody.appendChild(tr);
        });

        // Show Output and Action Buttons
        document.getElementById('output-section').style.display = 'block';
        document.getElementById('action-buttons').style.display = 'flex';
    }

    // PDF Download Engine
    function downloadPDF() {
        const element = document.getElementById('pdf-content');
        
        // Hide Form inputs temporarily for clean PDF output
        const inputCard = document.querySelector('.input-card');
        inputCard.style.display = 'none';

        const opt = {
            margin:       [10, 10, 10, 10],
            filename:     'Substation_Calculation_Report.pdf',
            image:        { type: 'jpeg', quality: 0.98 },
            html2canvas:  { scale: 2, useCORS: true, logging: false },
            jsPDF:        { unit: 'mm', format: 'a4', orientation: 'portrait' },
            pagebreak:    { mode: ['avoid-all', 'css'] }
        };

        html2pdf().set(opt).from(element).save().then(() => {
            // Restore Form display after PDF generation
            inputCard.style.display = 'block';
        });
    }
</script>

</body>
</html># Tranasformar-1
Good calculator 
