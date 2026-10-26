# password-generator
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Password Generator</title>
<style>
body{margin:0;min-height:100vh;display:grid;place-items:center;background:#101827;color:white;font-family:Arial}
.card{width:min(380px,85vw);padding:25px;background:#1e293b;border-radius:18px}
input[type=number]{width:65px;padding:8px}
#password{width:100%;box-sizing:border-box;padding:14px;background:#0f172a;color:#7ef9c6;border:1px solid #475569;border-radius:8px;margin:15px 0}
label{display:block;margin:12px 0}
button{padding:12px;border:0;border-radius:8px;background:#34d399;color:#062319;font-weight:bold;cursor:pointer}
</style>
</head>
<body>
<div class="card">
<h1>🔐 Password Generator</h1>
<p>Create a random password for your accounts.</p>
<label>Password length: <input id="length" type="number" min="8" max="64" value="16"></label>
<label><input id="upper" type="checkbox" checked> Uppercase letters</label>
<label><input id="lower" type="checkbox" checked> Lowercase letters</label>
<label><input id="numbers" type="checkbox" checked> Numbers</label>
<label><input id="symbols" type="checkbox" checked> Symbols</label>
<button onclick="generate()">Generate</button>
<input id="password" readonly placeholder="Your password appears here">
<button onclick="copyPassword()">Copy Password</button>
<p id="message" aria-live="polite"></p>
</div>
<script>
function generate(){
const length=Number(document.getElementById("length").value);
let chars="";
if(document.getElementById("upper").checked)chars+="ABCDEFGHIJKLMNOPQRSTUVWXYZ";
if(document.getElementById("lower").checked)chars+="abcdefghijklmnopqrstuvwxyz";
if(document.getElementById("numbers").checked)chars+="0123456789";
if(document.getElementById("symbols").checked)chars+="!@#$%^&*_-+=";
const out=document.getElementById("password");
const msg=document.getElementById("message");
if(!Number.isInteger(length)||length<8||length>64){msg.textContent="Choose a length from 8 to 64.";return}
if(!chars){msg.textContent="Select at least one character type.";return}
let result="";
const random=new Uint32Array(length);
crypto.getRandomValues(random);
for(let i=0;i<length;i++)result+=chars[random[i]%chars.length];
out.value=result;msg.textContent="Password generated.";
}
async function copyPassword(){
const p=document.getElementById("password");
const msg=document.getElementById("message");
if(!p.value){msg.textContent="Generate a password first.";return}
try{await navigator.clipboard.writeText(p.value);msg.textContent="Copied to clipboard."}
catch(e){p.select();msg.textContent="Select the password and copy it manually."}
}
</script>
</body>
</html>
