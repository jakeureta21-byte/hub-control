<!DOCTYPE html>

<html>
<head>
  <title>IoT Hub Control</title>
</head>
<body>

<h2>🏠 IoT Hub Control Panel</h2>

<h3>🔌 Controls</h3>
<button onclick="setData('lock',1)">Lock OPEN</button>
<button onclick="setData('lock',0)">Lock CLOSE</button><br><br>

<button onclick="setData('light',1)">Light ON</button> <button onclick="setData('light',0)">Light OFF</button><br><br>

<button onclick="setData('fan',1)">Fan ON</button> <button onclick="setData('fan',0)">Fan OFF</button><br><br>

<button onclick="setData('alarm',1)">Alarm ON</button> <button onclick="setData('alarm',0)">Alarm OFF</button>

<h3>📡 Sensor Status</h3>
<p>Door: <span id="door">--</span></p>
<p>Voltage: <span id="voltage">--</span> V</p>
<p>Current: <span id="current">--</span> A</p>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
import { getDatabase, ref, set, onValue } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js";

const firebaseConfig = {
  apiKey: "AIzaSyBn-EqDz2hqYn6gchSHYljjJLzCEaEED2Y",
  authDomain: "hub-control-logic.firebaseapp.com",
  databaseURL: "https://hub-control-logic-default-rtdb.firebaseio.com",
  projectId: "hub-control-logic"
};

const app = initializeApp(firebaseConfig);
const db = getDatabase(app);

// CONTROL FUNCTION
window.setData = function(device, state) {
  set(ref(db, "control/" + device), state);
};

// SENSOR DISPLAY
onValue(ref(db, "sensor/door"), (snap) => {
  document.getElementById("door").innerText = snap.val() == 1 ? "OPEN" : "CLOSED";
});

onValue(ref(db, "sensor/voltage"), (snap) => {
  document.getElementById("voltage").innerText = snap.val();
});

onValue(ref(db, "sensor/current"), (snap) => {
  document.getElementById("current").innerText = snap.val();
});

</script>

</body>
</html>
