<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<title>Thatsana Search</title>

<style>
* {margin:0;padding:0;box-sizing:border-box;font-family:sans-serif;}

body {
    height:100vh;
    display:flex;
    justify-content:center;
    align-items:center;
    background:linear-gradient(135deg,#0f172a,#020617);
    color:white;
    transition:0.4s;
}

body.light {
    background:#f1f5f9;
    color:black;
}

.container {
    text-align:center;
    width:90%;
    max-width:600px;
}

.logo {
    width:80px;
    margin-bottom:10px;
}

h1 {
    font-size:40px;
}

.search-box {
    display:flex;
    margin-top:20px;
    background:rgba(255,255,255,0.1);
    padding:10px;
    border-radius:50px;
}

input {
    flex:1;
    padding:12px;
    border:none;
    background:transparent;
    color:inherit;
    outline:none;
}

button {
    border:none;
    padding:10px 15px;
    border-radius:20px;
    cursor:pointer;
    margin-left:5px;
}

.engine-select button {
    margin:5px;
}

.history {
    margin-top:20px;
    text-align:left;
    max-height:150px;
    overflow:auto;
}

.history div {
    padding:5px;
    cursor:pointer;
}

.history div:hover {
    background:rgba(255,255,255,0.1);
}

.top-bar {
    position:absolute;
    top:20px;
    right:20px;
}

</style>
</head>

<body>

<div class="top-bar">
    <button onclick="toggleTheme()">🌙/☀️</button>
</div>

<div class="container">

    <!-- โลโก้ -->
    <img src="https://cdn-icons-png.flaticon.com/512/891/891419.png" class="logo">

    <h1>Thatsana Search</h1>
    <p>โดย ทัศนะ ศรีประเสริฐ</p>

    <!-- เลือก Engine -->
    <div class="engine-select">
        <button onclick="setEngine('google')">Google</button>
        <button onclick="setEngine('youtube')">YouTube</button>
        <button onclick="setEngine('tiktok')">TikTok</button>
    </div>

    <!-- Search -->
    <div class="search-box">
        <input id="searchInput" placeholder="พิมพ์เพื่อค้นหา...">
        <button onclick="search()">🔎</button>
        <button onclick="voiceSearch()">🎙️</button>
    </div>

    <!-- History -->
    <div class="history" id="history"></div>

</div>

<script>
let engine = "google";

// เลือก engine
function setEngine(e) {
    engine = e;
    alert("เลือก: " + e);
}

// ค้นหา
function search() {
    let q = document.getElementById("searchInput").value;
    if(!q) return;

    saveHistory(q);

    let url = "";
    if(engine === "google") {
        url = "https://www.google.com/search?q=" + q;
    }
    if(engine === "youtube") {
        url = "https://www.youtube.com/results?search_query=" + q;
    }
    if(engine === "tiktok") {
        url = "https://www.tiktok.com/search?q=" + q;
    }

    window.open(url, "_blank");
}

// Enter search
document.getElementById("searchInput").addEventListener("keypress", e=>{
    if(e.key==="Enter") search();
});

// Voice Search
function voiceSearch() {
    let recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
    recognition.lang = "th-TH";

    recognition.onresult = function(event) {
        let text = event.results[0][0].transcript;
        document.getElementById("searchInput").value = text;
        search();
    }

    recognition.start();
}

// History
function saveHistory(q) {
    let h = JSON.parse(localStorage.getItem("history") || "[]");
    h.unshift(q);
    localStorage.setItem("history", JSON.stringify(h));
    showHistory();
}

function showHistory() {
    let h = JSON.parse(localStorage.getItem("history") || "[]");
    let html = "";
    h.slice(0,10).forEach(item=>{
        html += `<div onclick="quickSearch('${item}')">${item}</div>`;
    });
    document.getElementById("history").innerHTML = html;
}

function quickSearch(q) {
    document.getElementById("searchInput").value = q;
    search();
}

showHistory();

// Theme
function toggleTheme() {
    document.body.classList.toggle("light");
}
</script>

</body>
</html>
