# EUN_TOM
From app that looks like chatgbt
<!DOCTYPE html><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>EUN_TOM Chat</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#eef2f7;color:#17202a}
header{background:#075e54;color:white;padding:22px 16px;text-align:center}
header h1{margin:0;font-size:28px}
header p{margin:8px 0 0}
main{max-width:500px;margin:25px auto;padding:16px}
.card{background:white;padding:22px;border-radius:16px;box-shadow:0 3px 14px #0001}
label{display:block;margin:12px 0 6px}
input,button{width:100%;padding:13px;border-radius:8px;font-size:16px}
input{border:1px solid #ccd3da}
button{border:0;background:#087f70;color:white;margin-top:12px;font-weight:bold}
#chat{display:none}
#messages{height:260px;overflow:auto;background:#f6f8fa;padding:10px;border-radius:8px}
.msg{background:#d9fdd3;padding:10px;margin:8px 0;border-radius:9px;overflow-wrap:anywhere}
.small{font-size:13px;color:#64748b;text-align:center;margin-top:18px}
</style>
</head>
<body>
<header>
<h1>EUN_TOM 💬</h1>
<p>Connect and chat with friends</p>
</header>
<main>
<section class="card" id="login">
<h2>Welcome to EUN_TOM</h2>
<p>Enter a display name to try the app.</p>
<label for="username">Your display name</label>
<input id="username" placeholder="Enter your name">
<button onclick="enterChat()">Enter Chat</button>
<p class="small">Demo version — messages are stored only on this device.</p>
</section>
<section class="card" id="chat">
<h2 id="welcome"></h2>
<div id="messages"></div>
<label for="message">Your message</label>
<input id="message" placeholder="Write a message..." maxlength="1000">
<button onclick="sendMessage()">Send Message</button>
<button onclick="leaveChat()" style="background:#64748b">Log Out</button>
<p class="small">Demo only. Messages are not sent to other users.</p>
</section>
</main>
<script>
let currentUser="";
function enterChat(){
const name=document.getElementById("username").value.trim();
if(!name){alert("Please enter your display name.");return;}
currentUser=name;
document.getElementById("login").style.display="none";
document.getElementById("chat").style.display="block";
document.getElementById("welcome").textContent="Hello, "+currentUser+"!";
showMessages();
}
function showMessages(){
const box=document.getElementById("messages");
box.innerHTML="";
try{
const list=JSON.parse(localStorage.getItem("euntom_demo_messages")||"[]");
list.forEach(item=>{
const div=document.createElement("div");
div.className="msg";
const strong=document.createElement("strong");
strong.textContent=item.name+": ";
div.appendChild(strong);
div.appendChild(document.createTextNode(item.text));
box.appendChild(div);
});
box.scrollTop=box.scrollHeight;
}catch(e){}
}
function sendMessage(){
const input=document.getElementById("message");
const text=input.value.trim();
if(!text)return;
try{
const list=JSON.parse(localStorage.getItem("euntom_demo_messages")||"[]");
list.push({name:currentUser,text:text});
localStorage.setItem("euntom_demo_messages",JSON.stringify(list));
input.value="";
showMessages();
}catch(e){alert("Unable to save this message on this device.");}
}
function leaveChat(){
currentUser="";
document.getElementById("chat").style.display="none";
document.getElementById("login").style.display="block";
}
document.getElementById("message").addEventListener("keydown",function(e){
if(e.key==="Enter")sendMessage();
});
</script>
</body>
</html>
