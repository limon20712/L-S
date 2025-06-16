<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Premium Base Converter</title>
  <style>
    body {
      background: #1c1c1c;
      color: #ffd700;
      font-family: 'Segoe UI', sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }

    .converter {
      background: #2b2b2b;
      padding: 30px;
      border-radius: 12px;
      box-shadow: 0 0 20px #ffd70066;
      width: 420px;
    }

    h2 {
      text-align: center;
      margin-bottom: 20px;
    }

    label {
      font-weight: bold;
      display: block;
      margin-top: 15px;
      color: #f1c40f;
    }

    input, select {
      width: 100%;
      padding: 10px;
      margin-top: 5px;
      border: 1px solid #ffd700;
      border-radius: 8px;
      background: #1c1c1c;
      color: #fffacd;
    }

    .output {
      margin-top: 20px;
      background: #000;
      border: 1px solid #ffd700;
      border-radius: 8px;
      padding: 15px;
    }

    .output div {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 12px;
    }

    .copy-btn {
      background: #ffd700;
      border: none;
      color: #000;
      padding: 5px 10px;
      border-radius: 5px;
      cursor: pointer;
      font-size: 0.9em;
    }

    .footer {
      text-align: center;
      margin-top: 20px;
      font-size: 0.85em;
      color: #888;
    }
  </style>
</head>
<body>
  <div class="converter">
    <h2>Base Converter</h2>
    <label for="inputNumber">Enter Number</label>
    <input type="text" id="inputNumber" placeholder="e.g. 10.56 or A3.F">

    <label for="inputBase">Input Base</label>
    <select id="inputBase">
      <option value="2">Binary (2)</option>
      <option value="8">Octal (8)</option>
      <option value="10" selected>Decimal (10)</option>
      <option value="16">Hexadecimal (16)</option>
    </select>

    <div class="output" id="outputSection">
      <div><strong>Binary:</strong> <span id="binResult">-</span> <button class="copy-btn" onclick="copyToClipboard('binResult')">Copy</button></div>
      <div><strong>Octal:</strong> <span id="octResult">-</span> <button class="copy-btn" onclick="copyToClipboard('octResult')">Copy</button></div>
      <div><strong>Decimal:</strong> <span id="decResult">-</span> <button class="copy-btn" onclick="copyToClipboard('decResult')">Copy</button></div>
      <div><strong>Hexadecimal:</strong> <span id="hexResult">-</span> <button class="copy-btn" onclick="copyToClipboard('hexResult')">Copy</button></div>
    </div>

    <div class="footer">SS PRIVATE CARE</div>
  </div>

  <script>
    const input = document.getElementById('inputNumber');
    const baseSelect = document.getElementById('inputBase');

    function convert() {
      const numStr = input.value.trim();
      const base = parseInt(baseSelect.value);

      if (!numStr) return;

      let decimalValue;
      try {
        if (base === 10) {
          decimalValue = parseFloat(numStr);
        } else {
          const parts = numStr.split(".");
          const intPart = parseInt(parts[0], base);
          let fracPart = 0;
          if (parts[1]) {
            for (let i = 0; i < parts[1].length; i++) {
              fracPart += parseInt(parts[1][i], base) / Math.pow(base, i + 1);
            }
          }
          decimalValue = intPart + fracPart;
        }

        document.getElementById('decResult').innerText = decimalValue.toString(10);
        document.getElementById('binResult').innerText = convertDecimalToBase(decimalValue, 2);
        document.getElementById('octResult').innerText = convertDecimalToBase(decimalValue, 8);
        document.getElementById('hexResult').innerText = convertDecimalToBase(decimalValue, 16).toUpperCase();

      } catch (e) {
        alert("Invalid number or base.");
      }
    }

    function convertDecimalToBase(num, base) {
      const intPart = Math.floor(num);
      let fracPart = num - intPart;
      let result = intPart.toString(base);

      if (fracPart > 0) {
        result += ".";
        for (let i = 0; i < 10 && fracPart !== 0; i++) {
          fracPart *= base;
          const digit = Math.floor(fracPart);
          result += digit.toString(base);
          fracPart -= digit;
        }
      }

      return result;
    }

    function copyToClipboard(elementId) {
      const text = document.getElementById(elementId).innerText;
      navigator.clipboard.writeText(text).then(() => {
        alert(`Copied: ${text}`);
      });
    }

    input.addEventListener('input', convert);
    baseSelect.addEventListener('change', convert);
  </script>
</body>
</html>
