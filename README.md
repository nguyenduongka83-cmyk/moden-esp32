<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Điều khiển ESP32</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Arial,sans-serif;
}

body{
    background:linear-gradient(135deg,#4b6cb7,#182848);
    min-height:100vh;
    display:flex;
    justify-content:center;
    align-items:center;
    padding:20px;
}

.box{
    width:100%;
    max-width:400px;
    background:#fff;
    border-radius:20px;
    padding:35px 30px;
    text-align:center;
    box-shadow:0 10px 30px rgba(0,0,0,.25);
}

h1{
    color:#333;
    margin-bottom:25px;
}

.ip-box{
    text-align:left;
    margin-bottom:20px;
}

.ip-box label{
    display:block;
    margin-bottom:8px;
    font-weight:bold;
    color:#555;
}

input{
    width:100%;
    padding:12px;
    border:1px solid #ccc;
    border-radius:8px;
    font-size:16px;
}

button{
    width:100%;
    border:none;
    border-radius:12px;
    padding:20px;
    font-size:24px;
    font-weight:bold;
    color:white;
    cursor:pointer;
    margin-top:12px;
    transition:.2s;
}

button:active{
    transform:scale(.97);
}

.on{
    background:#22c55e;
}

.off{
    background:#ef4444;
}

button:hover{
    opacity:.9;
}

#status{
    margin-top:20px;
    font-size:15px;
    color:#444;
}

.note{
    margin-top:25px;
    font-size:13px;
    color:#777;
    line-height:1.6;
}
</style>

</head>
<body>

<div class="box">

<h1>🔧 ĐIỀU KHIỂN THIẾT BỊ</h1>

<div class="ip-box">
<label>📍 Địa chỉ IP ESP32</label>
<input
type="text"
id="ipInput"
placeholder="Ví dụ: 192.168.1.100">
</div>

<button class="on" onclick="sendCommand('on')">
🔌 BẬT
</button>

<button class="off" onclick="sendCommand('off')">
⛔ TẮT
</button>

<div id="status">
Sẵn sàng...
</div>

<div class="note">
💡 Nhập IP ESP32 rồi nhấn BẬT hoặc TẮT.<br>
⚠️ Điện thoại và ESP32 phải cùng mạng WiFi.
</div>

</div>

<script>

// Tự điền IP đã lưu
window.onload=function(){
    const savedIP=localStorage.getItem("esp32_ip");
    if(savedIP){
        document.getElementById("ipInput").value=savedIP;
    }
};

function sendCommand(cmd){

    const ip=document.getElementById("ipInput").value.trim();

    if(ip==""){
        alert("Vui lòng nhập IP ESP32!");
        return;
    }

    localStorage.setItem("esp32_ip",ip);

    document.getElementById("status").innerHTML="Đang kết nối...";

    fetch("http://"+ip+"/"+cmd)
    .then(response=>{

        if(response.ok){

            document.getElementById("status").innerHTML=
            (cmd=="on") ?
            "✅ Thiết bị đang BẬT" :
            "⛔ Thiết bị đã TẮT";

        }else{

            document.getElementById("status").innerHTML=
            "❌ ESP32 phản hồi lỗi";

        }

    })

    .catch(error=>{

        document.getElementById("status").innerHTML=
        "❌ Không kết nối được ESP32";

    });

}

</script>

</body>
</html>

