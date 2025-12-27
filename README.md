<!DOCTYPE html>
<html>
<head>
  <title>Secret Santa 🎅</title>

  <style>
    body {
      background: linear-gradient(135deg, #b11226, #d62828);
      font-family: Arial, sans-serif;
      color: white;
      text-align: center;
      animation: fadeIn 1s ease-in;
    }

    .box {
      background: white;
      color: #b11226;
      width: 360px;
      margin: 90px auto;
      padding: 30px;
      border-radius: 14px;
      box-shadow: 0 15px 30px rgba(0,0,0,0.35);
      animation: popIn 0.8s ease;
    }

    input, button {
      width: 90%;
      padding: 12px;
      margin: 10px 0;
      font-size: 16px;
      border-radius: 6px;
    }

    input {
      border: 2px solid #b11226;
    }

    button {
      background: #b11226;
      color: white;
      border: none;
      cursor: pointer;
      transition: transform 0.2s, background 0.2s;
    }

    button:hover {
      background: #8e0f1f;
      transform: scale(1.05);
    }

    #result {
      font-size: 20px;
      margin-top: 20px;
      font-weight: bold;
      animation: fadeIn 0.6s ease;
    }

    .shake {
      animation: shake 0.4s;
    }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    @keyframes popIn {
      from { transform: scale(0.8); opacity: 0; }
      to { transform: scale(1); opacity: 1; }
    }

    @keyframes shake {
      0% { transform: translateX(0); }
      25% { transform: translateX(-5px); }
      50% { transform: translateX(5px); }
      75% { transform: translateX(-5px); }
      100% { transform: translateX(0); }
    }
  </style>
</head>

<body>

<div class="box" id="box">
  <h2>🎄 Secret Santa 🎅</h2>
  <input type="text" id="psno" placeholder="Enter PS No" />
  <button onclick="draw()">Draw Name</button>
  <div id="result"></div>
</div>

<script>
// ===== CONFIGURATION =====

const psToNameMap = {
  "PS001": "Amit Sharma",
  "PS002": "Neha Gupta",
  "PS003": "Rahul Verma",
  "PS004": "Pooja Mehta",
  "PS005": "Suresh Iyer",
  "PS006": "Anita Rao",
  "PS007": "Karan Singh",
  "PS008": "Ritu Malhotra",
  "PS009": "Vikas Jain",
  "PS010": "Sneha Kulkarni",
  "PS011": "Manoj Patil",
  "PS012": "Rashmi Shah",
  "PS013": "Deepak Nair",
  "PS014": "Swati Deshpande",
  "PS015": "Arjun Kapoor",
  "PS016": "Nikhil Joshi",
  "PS017": "Priya Bansal",
  "PS018": "Rohit Mehra",
  "PS019": "Kavita Das",
  "PS020": "Sanjay Kumar"
};

// ==========================

let deviceLocked = localStorage.getItem("deviceUsed");
let assignedPS = localStorage.getItem("devicePS");

let availableNames =
  JSON.parse(localStorage.getItem("availableNames")) ||
  Object.values(psToNameMap);

let log =
  JSON.parse(localStorage.getItem("log")) || {};

function draw() {
  const box = document.getElementById("box");
  box.classList.remove("shake");

  let ps = document.getElementById("psno").value.trim();

  if (deviceLocked) {
    showError("❌ This device has already participated");
    return;
  }

  if (!psToNameMap[ps]) {
    showError("❌ Invalid PS No");
    return;
  }

  if (log[ps]) {
    showError("❌ This PS No is already used");
    return;
  }

  let selfName = psToNameMap[ps];
  let validNames = availableNames.filter(n => n !== selfName);

  if (validNames.length === 0) {
    showError("❌ No valid names left");
    return;
  }

  let chosen = validNames[Math.floor(Math.random() * validNames.length)];

  availableNames = availableNames.filter(n => n !== chosen);
  log[ps] = chosen;

  localStorage.setItem("availableNames", JSON.stringify(availableNames));
  localStorage.setItem("log", JSON.stringify(log));
  localStorage.setItem("deviceUsed", "true");
  localStorage.setItem("devicePS", ps);

  document.getElementById("result").innerHTML =
    "🎁 Your Secret Santa is:<br><b>" + chosen + "</b>";
}

function showError(msg) {
  const box = document.getElementById("box");
  box.classList.add("shake");
  document.getElementById("result").innerHTML = msg;
}

// Lock refresh/back
window.onbeforeunload = () => "Are you sure?";
</script>

</body>
</html>
