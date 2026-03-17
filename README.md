<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Hesap Makinesi</title>
<link rel="manifest" href="manifest.json">
<style>
/* Tam ekran ayarı */
html, body {
  width:100%;
  height:100%;
  margin:0;
  padding:0;
  overflow:hidden;
  font-family:-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  background:#000;
  display:flex;
  justify-content:center;
  align-items:center;
  -webkit-tap-highlight-color: transparent;
}

/* Hesap makinesi kutusu */
.calculator {
  width:360px;
  padding:20px;
  border-radius:20px;
  background:#000;
  box-shadow:0 10px 20px rgba(0,0,0,0.5);
}

/* Ekran */
#display {
  width:100%;
  height:100px;
  background:#000;
  color:white;
  border:none;
  font-size:48px;
  text-align:right;
  padding:10px;
  overflow-x:auto;
  white-space: nowrap;
  border-radius:15px;
  margin-bottom:15px;
}

/* Tuşlar */
.buttons {
  display:grid;
  grid-template-columns:repeat(4,1fr);
  gap:12px;
}
button {
  height:70px;
  font-size:28px;
  border:none;
  border-radius:50%;
  transition: all 0.1s ease;
  user-select:none;
  display:flex;
  justify-content:center;
  align-items:center;
  cursor:pointer;
  -webkit-tap-highlight-color: transparent;
}
button:active {
  transform: scale(0.92);
  box-shadow:0 2px #666 inset;
}

/* Renkler */
.gray {background:#a5a5a5; color:black;}
.dark {background:#333; color:white;}
.orange {background:#ff9500; color:white;}
.zero {grid-column:span 2; border-radius:50px; text-align:left; padding-left:25px;}
svg {width:28px; height:28px;}
</style>
</head>
<body>
<div class="calculator">
<input id="display" value="0" disabled>
<div class="buttons">
  <button class="gray" onclick="clearAll(); vibrateButton()">AC</button>
  <button class="gray" onclick="deleteLastWithVibrate()">
    <svg viewBox="0 0 24 24"><path d="M20 11H7.83l5.59-5.59L12 4l-8 8 8 8 1.41-1.41L7.83 13H20v-2z" fill="black"/></svg>
  </button>
  <button class="gray" onclick="toggleParenthesis(); vibrateButton()">( )</button>
  <button class="orange" onclick="operatorClick('/'); vibrateButton()">/</button>

  <button class="dark" onclick="numberClick(7); vibrateButton()">7</button>
  <button class="dark" onclick="numberClick(8); vibrateButton()">8</button>
  <button class="dark" onclick="numberClick(9); vibrateButton()">9</button>
  <button class="orange" onclick="operatorClick('*'); vibrateButton()">*</button>

  <button class="dark" onclick="numberClick(4); vibrateButton()">4</button>
  <button class="dark" onclick="numberClick(5); vibrateButton()">5</button>
  <button class="dark" onclick="numberClick(6); vibrateButton()">6</button>
  <button class="orange" onclick="operatorClick('-'); vibrateButton()">-</button>

  <button class="dark" onclick="numberClick(1); vibrateButton()">1</button>
  <button class="dark" onclick="numberClick(2); vibrateButton()">2</button>
  <button class="dark" onclick="numberClick(3); vibrateButton()">3</button>
  <button class="orange" onclick="operatorClick('+'); vibrateButton()">+</button>

  <button class="dark zero" onclick="numberClick(0); vibrateButton()">0</button>
  <button class="dark" onclick="dotClick(); vibrateButton()">.</button>
  <button class="orange" onclick="calculate(); vibrateButton()">=</button>
</div>
</div>

<script>
let display = document.getElementById("display");
let expression = "";
let currentNumber = "0";

function vibrateButton(){if(navigator.vibrate){navigator.vibrate(15);}}
function numberClick(num){if(currentNumber==="0")currentNumber=String(num);else currentNumber+=String(num);expression+=String(num);display.value=currentNumber;adjustFont();}
function operatorClick(op){expression+=op;currentNumber="0";display.value=currentNumber;adjustFont();}
function dotClick(){if(!currentNumber.includes(".")){currentNumber+=".";expression+=".";display.value=currentNumber;}}
function clearAll(){currentNumber="0";expression="";display.value=currentNumber;adjustFont();}
function deleteLastWithVibrate(){deleteLast(); vibrateButton();}
function deleteLast(){if(expression.length>0){expression=expression.slice(0,-1);currentNumber=currentNumber.slice(0,-1);if(currentNumber==="")currentNumber="0";display.value=currentNumber;adjustFont();}}
function toggleParenthesis(){let open=(expression.match(/\(/g)||[]).length;let close=(expression.match(/\)/g)||[]).length;let lastChar=expression.slice(-1);let paren="";if(open>close&&(/\d|\)/.test(lastChar))){expression+=")";paren=")";}else if(lastChar===""||/[+\-*/(]/.test(lastChar)){expression+="(";paren="("; }display.value=paren;adjustFont();}
function calculate(){try{let open=(expression.match(/\(/g)||[]).length;let close=(expression.match(/\)/g)||[]).length;for(let i=0;i<open-close;i++)expression+=")";let result=Function("return "+expression)();currentNumber=String(result);expression=currentNumber;display.value=currentNumber;adjustFont();}catch{display.value="Error";currentNumber="0";expression="";}}
function adjustFont(){let len=currentNumber.length;if(len<=10)display.style.fontSize='48px';else if(len<=15)display.style.fontSize='36px';else display.style.fontSize='28px';}
</script>
</body>
</html>
