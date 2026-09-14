<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="theme-color" content="#2563eb">
<title>Room Finder</title>

<style>
*{box-sizing:border-box}
html{scroll-behavior:smooth}
body{
  margin:0;
  font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif;
  background:#f5f7fb;
  color:#172033;
}
header{
  position:sticky;
  top:0;
  z-index:20;
  background:linear-gradient(135deg,#2563eb,#4f46e5);
  color:#fff;
  padding:16px 14px 18px;
  box-shadow:0 4px 18px rgba(37,99,235,.22);
}
.header-inner{max-width:760px;margin:auto}
.brand{display:flex;align-items:center;gap:10px}
.logo{
  width:44px;height:44px;border-radius:14px;
  background:rgba(255,255,255,.18);
  display:grid;place-items:center;font-size:25px;
}
header h1{margin:0;font-size:23px}
.subtitle{margin:4px 0 0;font-size:13px;opacity:.9}

.container{max-width:760px;margin:auto;padding:14px 12px 90px}

.card{
  background:#fff;
  padding:16px;
  margin:12px 0;
  border-radius:18px;
  border:1px solid #e7eaf0;
  box-shadow:0 5px 18px rgba(20,30,55,.07);
}
.card h2{font-size:18px;margin:0 0 12px}
.search-wrap{position:relative}
.search-wrap span{
  position:absolute;left:13px;top:13px;font-size:19px
}
input,select,button{
  width:100%;
  min-height:48px;
  padding:12px 14px;
  margin:6px 0;
  border-radius:12px;
  border:1px solid #d7dce5;
  font-size:16px;
  background:#fff;
}
#search{padding-left:42px}
input:focus,select:focus{
  outline:3px solid rgba(37,99,235,.12);
  border-color:#2563eb;
}
button{
  border:0;
  color:#fff;
  background:#2563eb;
  font-weight:700;
  cursor:pointer;
}
button:active{transform:scale(.99)}
.secondary{background:#eef2ff;color:#3730a3}
.location{background:#0891b2}
.success{background:#16a34a}
.edit{background:#f59e0b}
.book{background:#7c3aed}
.danger{background:#dc2626}
.dark{background:#334155}

.grid{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.grid input{margin:0}

.location-box{
  background:#ecfeff;
  border:1px solid #a5f3fc;
  padding:12px;
  border-radius:14px;
  margin-top:8px;
  font-size:14px;
}
.location-on{color:#047857;font-weight:800}
.location-off{color:#64748b}

.toolbar{
  display:flex;
  align-items:center;
  justify-content:space-between;
  gap:8px;
  margin:16px 2px 8px;
}
.count{font-weight:800;font-size:17px}
.nearest{
  background:#fff;
  border:1px solid #d7dce5;
  color:#334155;
  width:auto;
  min-height:42px;
  padding:8px 12px;
  margin:0;
}

.room-card{position:relative;overflow:hidden}
.room-top{
  display:flex;justify-content:space-between;gap:10px;align-items:flex-start
}
.room-name{font-size:19px;font-weight:800;margin:0}
.distance{
  white-space:nowrap;
  background:#eff6ff;
  color:#1d4ed8;
  border-radius:20px;
  padding:6px 9px;
  font-size:12px;
  font-weight:800;
}
.info{
  margin:9px 0;
  line-height:1.45;
  color:#475569;
  font-size:14px;
}
.info b{color:#172033}
.rent{
  font-size:21px;
  font-weight:900;
  color:#111827;
}
.status{
  display:inline-block;
  padding:6px 10px;
  border-radius:20px;
  color:#fff;
  font-size:12px;
  font-weight:800;
}
.green{background:#16a34a}
.red{background:#dc2626}
.actions{display:grid;grid-template-columns:1fr 1fr;gap:7px;margin-top:10px}
.actions button{margin:0;font-size:14px}
.full{grid-column:1/-1}
.empty{text-align:center;color:#64748b;padding:25px 10px}
.small{font-size:12px;color:#64748b;line-height:1.5}
.hidden{display:none}

.bottom-nav{
  position:fixed;bottom:0;left:0;right:0;z-index:30;
  background:rgba(255,255,255,.96);
  backdrop-filter:blur(10px);
  border-top:1px solid #e5e7eb;
  padding:8px 10px calc(8px + env(safe-area-inset-bottom));
}
.bottom-inner{
  max-width:760px;margin:auto;display:grid;grid-template-columns:1fr 1fr 1fr;gap:7px
}
.bottom-nav button{
  margin:0;background:transparent;color:#475569;min-height:42px;font-size:13px
}
.bottom-nav button:first-child{color:#2563eb}

@media(max-width:430px){
  .container{padding:10px 9px 90px}
  .card{padding:14px;border-radius:16px}
  .grid{grid-template-columns:1fr}
  .room-top{display:block}
  .distance{display:inline-block;margin-top:8px}
  .actions{grid-template-columns:1fr}
  .full{grid-column:auto}
}
</style>
</head>

<body>

<header>
  <div class="header-inner">
    <div class="brand">
      <div class="logo">🏠</div>
      <div>
        <h1>Room Finder</h1>
        <div class="subtitle">आपके पास के किराए के Rooms आसानी से खोजें</div>
      </div>
    </div>
  </div>
</header>

<div class="container">

  <section class="card" id="searchBox">
    <h2>🔍 Room खोजें</h2>

    <div class="search-wrap">
      <span>🔎</span>
      <input id="search" placeholder="Area, Room या Address खोजें" oninput="showRooms()">
    </div>

    <div class="grid">
      <select id="radius" onchange="showRooms()">
        <option value="all">📍 सभी दूरी</option>
        <option value="2">2 km के अंदर</option>
        <option value="5">5 km के अंदर</option>
        <option value="10">10 km के अंदर</option>
        <option value="25">25 km के अंदर</option>
        <option value="50">50 km के अंदर</option>
      </select>

      <button class="location" onclick="getLocation()">
        📍 मेरी Location लो
      </button>
    </div>

    <div id="locationInfo" class="location-box location-off">
      📍 Location चालू करने पर सबसे पास के Rooms ऊपर दिखेंगे।
    </div>
  </section>

  <section class="card" id="addBox">
    <h2>➕ नया Room जोड़ें</h2>

    <input id="name" placeholder="Room का नाम">
    <input id="address" placeholder="पूरा Home Address">
    <div class="grid">
      <input id="rent" type="number" inputmode="numeric" placeholder="किराया ₹">
      <input id="phone" type="tel" inputmode="tel" placeholder="Mobile Number">
    </div>
    <input id="pin" type="password" inputmode="numeric" maxlength="6"
           placeholder="Owner PIN (4-6 digit)">

    <button class="location" onclick="getRoomLocation()">
      📍 इस Room की Location लो
    </button>

    <div id="roomLocationInfo" class="small">
      Room की सही location सेव करने के लिए यह बटन दबाएँ।
    </div>

    <input id="lat" type="hidden">
    <input id="lon" type="hidden">

    <button class="success" onclick="addRoom()">✅ Room Save करें</button>

    <div class="small">
      ⚠️ यह अभी prototype है। असली public website में PIN और room data को secure server/database में रखना चाहिए।
    </div>
  </section>

  <div class="toolbar">
    <div class="count" id="count">🏠 Rooms</div>
    <button class="nearest" onclick="sortNearest()">↕️ पास वाले पहले</button>
  </div>

  <div id="rooms"></div>

</div>

<nav class="bottom-nav">
  <div class="bottom-inner">
    <button onclick="scrollToTop()">🏠 Home</button>
    <button onclick="document.getElementById('search').focus()">🔍 Search</button>
    <button onclick="document.getElementById('addBox').scrollIntoView({behavior:'smooth'})">➕ Add Room</button>
  </div>
</nav>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/12.2.1/firebase-app.js";
import { getFirestore, collection, addDoc, getDocs, updateDoc, deleteDoc, doc, query, orderBy } from "https://www.gstatic.com/firebasejs/12.2.1/firebase-firestore.js";
import { getAuth, signInAnonymously } from "https://www.gstatic.com/firebasejs/12.2.1/firebase-auth.js";

const firebaseConfig = {
  apiKey: "AIzaSyAyzLc7_inhFQFSZjA9JzDe3bO4r2iXEvI",
  authDomain: "room-finder-168d4.firebaseapp.com",
  projectId: "room-finder-168d4",
  storageBucket: "room-finder-168d4.firebasestorage.app",
  messagingSenderId: "496563614505",
  appId: "1:496563614505:web:e2275db45f71b891f03d38",
  measurementId: "G-JQYKT5GCW3"
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);
const auth = getAuth(app);
const roomsRef = collection(db, "rooms");

let rooms = [];
let userLat = null;
let userLon = null;
let currentUser = null;

async function startApp(){
  try{
    const cred = await signInAnonymously(auth);
    currentUser = cred.user;
    await loadRooms();
  }catch(error){
    console.error(error);
    document.getElementById("rooms").innerHTML = `<div class="card empty"><h3>⚠️ Firebase connection बाकी है</h3><div>Firebase Authentication में Anonymous sign-in चालू करना और Firestore Rules सेट करना जरूरी है।</div></div>`;
  }
}

async function loadRooms(){
  try{
    const snap = await getDocs(roomsRef);
    rooms = snap.docs.map(d => ({id:d.id, ...d.data()}));
    showRooms();
  }catch(error){
    console.error(error);
    document.getElementById("rooms").innerHTML = `<div class="card empty"><h3>⚠️ Rooms load नहीं हुए</h3><div>Firestore Rules की setting अभी बाकी हो सकती है।</div></div>`;
  }
}

function distanceKm(lat1, lon1, lat2, lon2){
  const R = 6371;
  const dLat = (lat2-lat1) * Math.PI/180;
  const dLon = (lon2-lon1) * Math.PI/180;
  const a = Math.sin(dLat/2)**2 + Math.cos(lat1*Math.PI/180)*Math.cos(lat2*Math.PI/180)*Math.sin(dLon/2)**2;
  return R * 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
}

function getLocation(){
  if(!navigator.geolocation){ alert("❌ आपके मोबाइल में Location सुविधा उपलब्ध नहीं है।"); return; }
  navigator.geolocation.getCurrentPosition(function(position){
    userLat = position.coords.latitude;
    userLon = position.coords.longitude;
    document.getElementById("locationInfo").innerHTML = "✅ <span class='location-on'>आपकी Location मिल गई। सबसे पास के Rooms ऊपर दिखाए जाएंगे।</span>";
    showRooms();
  }, function(){ alert("❌ Location नहीं मिली। मोबाइल की Location/GPS चालू करके फिर कोशिश करें।"); }, {enableHighAccuracy:true,timeout:10000,maximumAge:60000});
}

function getRoomLocation(){
  if(!navigator.geolocation){ alert("❌ Location सुविधा उपलब्ध नहीं है।"); return; }
  navigator.geolocation.getCurrentPosition(function(position){
    document.getElementById("lat").value = position.coords.latitude;
    document.getElementById("lon").value = position.coords.longitude;
    document.getElementById("roomLocationInfo").innerHTML = "✅ Room की Location मिल गई।";
  }, function(){ alert("❌ Room की Location नहीं मिली। GPS/Location चालू करें।"); }, {enableHighAccuracy:true,timeout:10000,maximumAge:60000});
}

async function addRoom(){
  const name = document.getElementById("name").value.trim();
  const address = document.getElementById("address").value.trim();
  const rent = document.getElementById("rent").value.trim();
  const phone = document.getElementById("phone").value.trim();
  const pin = document.getElementById("pin").value.trim();
  const lat = document.getElementById("lat").value;
  const lon = document.getElementById("lon").value;

  if(!name || !address || !rent || !phone || !pin){ alert("कृपया सभी जानकारी भरें!"); return; }
  if(!/^\d{4,6}$/.test(pin)){ alert("PIN 4 से 6 digit का होना चाहिए।"); return; }
  if(!lat || !lon){ if(!confirm("Room की Location नहीं ली गई है। फिर भी Room Save करें?")) return; }
  if(!currentUser){ alert("Firebase login अभी तैयार नहीं है।"); return; }

  try{
    await addDoc(roomsRef, {name,address,rent,phone,pin,lat:lat||null,lon:lon||null,status:"Available",ownerId:currentUser.uid,createdAt:Date.now()});
    ["name","address","rent","phone","pin","lat","lon"].forEach(id => document.getElementById(id).value = "");
    document.getElementById("roomLocationInfo").innerHTML = "Room की सही location सेव करने के लिए यह बटन दबाएँ।";
    alert("✅ Room online database में सेव हो गया!");
    await loadRooms();
  }catch(error){ console.error(error); alert("❌ Room save नहीं हुआ। Firestore Rules/Authentication की setting बाकी हो सकती है।"); }
}

function showRooms(){
  const box = document.getElementById("rooms");
  const search = document.getElementById("search").value.toLowerCase().trim();
  const radiusValue = document.getElementById("radius").value;
  const radius = radiusValue === "all" ? Infinity : Number(radiusValue);

  let list = rooms.map(room => {
    let distance = null;
    if(userLat !== null && userLon !== null && room.lat && room.lon) distance = distanceKm(userLat,userLon,Number(room.lat),Number(room.lon));
    return {...room,distance};
  });
  list = list.filter(room => {
    const matches = String(room.name||"").toLowerCase().includes(search) || String(room.address||"").toLowerCase().includes(search);
    const inRadius = radius===Infinity || (room.distance!==null && room.distance<=radius);
    return matches && inRadius;
  });
  if(userLat!==null && userLon!==null) list.sort((a,b)=>a.distance===null?1:b.distance===null?-1:a.distance-b.distance);

  document.getElementById("count").textContent = `🏠 ${list.length} Room${list.length===1?"":"s"}`;
  if(list.length===0){ box.innerHTML=`<div class="card empty"><div style="font-size:35px">🏠</div><h3>कोई Room नहीं मिला</h3><div>Search बदलें या Location लेकर पास के Rooms देखें।</div></div>`; return; }
  box.innerHTML="";
  list.forEach(room=>{
    const statusClass = room.status === "Booked" ? "red" : "green";
    const distanceText = room.distance!==null ? `<span class="distance">📍 ${formatDistance(room.distance)}</span>` : "";
    const locationButton = room.lat && room.lon ? `<button class="location" onclick="openLocation(${Number(room.lat)},${Number(room.lon)})">📍 Google Maps</button>` : "";
    box.innerHTML += `<div class="card room-card"><div class="room-top"><div><p class="room-name">${escapeHtml(room.name)}</p><div class="rent">₹${escapeHtml(room.rent)} <span class="small">/ month</span></div></div>${distanceText}</div><div class="info">🏠 <b>Address:</b> ${escapeHtml(room.address)}</div><div class="info">📞 <b>Owner Mobile:</b> <a href="tel:${escapeAttr(room.phone)}">${escapeHtml(room.phone)}</a></div><div><span class="status ${statusClass}">${room.status === "Booked" ? "🔴 BOOKED" : "🟢 AVAILABLE"}</span></div><div class="actions"><button onclick="callOwner('${escapeAttr(room.phone)}')">📞 Call Owner</button>${locationButton}<button class="edit" onclick="editRoom('${escapeAttr(room.id)}')">✏️ Edit / Rent</button><button class="book" onclick="toggleBooking('${escapeAttr(room.id)}')">${room.status === "Booked" ? "🟢 Available करें" : "🔴 BOOKED करें"}</button><button class="danger full" onclick="deleteRoom('${escapeAttr(room.id)}')">🗑️ Delete Room</button></div></div>`;
  });
}

function formatDistance(km){ return km<1 ? Math.round(km*1000)+" m दूर" : km.toFixed(1)+" km दूर"; }
function sortNearest(){ if(userLat===null){alert("पहले 📍 मेरी Location लो दबाएँ।");return;} showRooms(); }
function callOwner(phone){ window.location.href="tel:"+phone; }
function openLocation(lat,lon){ window.open("https://www.google.com/maps?q="+lat+","+lon,"_blank"); }
function verifyPin(room){ const entered=prompt("🔐 Owner PIN डालें:"); if(entered===null)return false; if(entered!==room.pin){alert("❌ PIN गलत है!");return false;} return true; }

async function editRoom(id){
  const room=rooms.find(r=>r.id===id); if(!room||!verifyPin(room))return;
  const name=prompt("Room का नाम:",room.name); if(name===null)return;
  const address=prompt("Home Address:",room.address); if(address===null)return;
  const rent=prompt("नया Rent ₹:",room.rent); if(rent===null)return;
  const phone=prompt("Mobile Number:",room.phone); if(phone===null)return;
  try{ await updateDoc(doc(db,"rooms",id),{name:name.trim(),address:address.trim(),rent:rent.trim(),phone:phone.trim()}); alert("✅ Room की जानकारी बदल दी गई!"); await loadRooms(); }
  catch(error){console.error(error);alert("❌ Update नहीं हुआ। Firestore Rules की setting जाँचें।");}
}

async function toggleBooking(id){
  const room=rooms.find(r=>r.id===id); if(!room||!verifyPin(room))return;
  const newStatus=room.status==="Booked"?"Available":"Booked";
  try{ await updateDoc(doc(db,"rooms",id),{status:newStatus}); alert(newStatus==="Booked"?"🔴 Room BOOKED कर दिया गया!":"🟢 Room AVAILABLE कर दिया गया!"); await loadRooms(); }
  catch(error){console.error(error);alert("❌ Status बदल नहीं पाया। Firestore Rules की setting जाँचें।");}
}

async function deleteRoom(id){
  const room=rooms.find(r=>r.id===id); if(!room||!verifyPin(room))return;
  if(!confirm("क्या आप यह Room हमेशा के लिए Delete करना चाहते हैं?"))return;
  try{ await deleteDoc(doc(db,"rooms",id)); alert("🗑️ Room Delete हो गया!"); await loadRooms(); }
  catch(error){console.error(error);alert("❌ Delete नहीं हुआ। Firestore Rules की setting जाँचें।");}
}

function escapeHtml(value){return String(value??"").replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#039;"}[c]));}
function escapeAttr(value){return String(value??"").replace(/['"]/g,"");}
function scrollToTop(){window.scrollTo({top:0,behavior:"smooth"});}

document.getElementById("search").addEventListener("input",showRooms);
document.getElementById("radius").addEventListener("change",showRooms);
showRooms();
startApp();
</script>

</body>
</html>
