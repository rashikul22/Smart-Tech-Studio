<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Smart Tech Studio</title>
  <style>
    body { font-family: Arial, sans-serif; margin:0; padding:0; background:#f8f8f8; }
    header { background:#1a73e8; color:white; padding:20px; text-align:center; font-size:24px; font-weight:bold; position:relative; }
    /* Announcement Bar */
    #announcement { background:#ffeb3b; color:#000; white-space:nowrap; overflow:hidden; position:relative; padding:10px 0; font-weight:bold; }
    #announcement span { display:inline-block; padding-left:100%; animation:scroll 15s linear infinite; }
    @keyframes scroll { 0% { transform:translateX(0%);} 100% { transform:translateX(-100%);} }
    /* Profile & Balance icons */
    #icons { position:absolute; top:20px; right:20px; display:flex; gap:15px; }
    .icon { width:30px; height:30px; background:#fff; border-radius:50%; display:flex; justify-content:center; align-items:center; cursor:pointer; }
    /* Profile Modal */
    #profileModal { display:none; position:fixed; top:50%; left:50%; transform:translate(-50%, -50%); background:white; padding:20px; border-radius:8px; box-shadow:0 0 10px rgba(0,0,0,0.3); width:300px; }
    #profileModal input { width:100%; margin:5px 0; padding:8px; }
    #profileModal button { margin-top:10px; padding:10px; width:100%; background:#1a73e8; color:white; border:none; cursor:pointer; }
    #overlay { display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.5); }
  </style>
</head>
<body>

  <!-- Header -->
  <header>
    Smart Tech Studio
    <div id="icons">
      <div class="icon" id="profileIcon">👤</div>
      <div class="icon" id="balanceIcon">💰 <span id="balanceAmount">0</span></div>
    </div>
  </header>

  <!-- Announcement -->
  <div id="announcement">
    <span id="announcementText">স্বাগতম! এখানে আপনার নতুন সার্ভিসের তথ্য দেখতে পাবেন।</span>
  </div>

  <!-- Profile Modal -->
  <div id="overlay"></div>
  <div id="profileModal">
    <h3>প্রোফাইল এডিট</h3>
    <input type="text" id="profileName" placeholder="নাম">
    <input type="text" id="profileMobile" placeholder="মোবাইল">
    <input type="email" id="profileEmail" placeholder="ই-মেইল">
    <button id="saveProfile">Save</button>
    <button id="logoutProfile" style="background:red; margin-top:5px;">Logout</button>
  </div>

<script>
  // Announcement editable (admin)
  const announcementText = document.getElementById('announcementText');
  function updateAnnouncement(newText){
    announcementText.textContent = newText;
  }
  // Example admin edit
  // updateAnnouncement("নতুন এনাউন্সমেন্ট টেক্সট!"); // Call this to change

  // Profile Modal Logic
  const profileIcon = document.getElementById('profileIcon');
  const profileModal = document.getElementById('profileModal');
  const overlay = document.getElementById('overlay');
  profileIcon.addEventListener('click', () => { profileModal.style.display='block'; overlay.style.display='block'; });

  overlay.addEventListener('click', () => { profileModal.style.display='none'; overlay.style.display='none'; });

  document.getElementById('saveProfile').addEventListener('click', ()=>{
    alert('Profile Saved!');
    profileModal.style.display='none'; overlay.style.display='none';
  });

  document.getElementById('logoutProfile').addEventListener('click', ()=>{
    alert('Logged Out!');
    profileModal.style.display='none'; overlay.style.display='none';
  });

  // Balance (example, later Firebase)
  const balanceAmount = document.getElementById('balanceAmount');
  function updateBalance(amount){
    balanceAmount.textContent = amount;
  }
  // Example: updateBalance(500); // Call this to update balance dynamically

</script>

</body>
</html>l
<!-- Menu Section -->
<div style="text-align:center; margin:20px 0; font-weight:bold; font-size:20px;">সেবা সমূহ</div>

<div id="menu" style="display:flex; flex-wrap:wrap; gap:15px; justify-content:center; margin-bottom:30px;">
  <button onclick="openService('nid')" style="padding:15px; cursor:pointer;">NID সেবা</button>
  <button onclick="openService('biometric')" style="padding:15px; cursor:pointer;">বায়োমেট্রিক সেবা</button>
  <button onclick="openService('tin')" style="padding:15px; cursor:pointer;">TIN সার্টিফিকেট</button>
  <button onclick="openService('vaccine')" style="padding:15px; cursor:pointer;">করোনা ভ্যাকসিন</button>
</div>

<!-- Service Modal Template -->
<div id="serviceModal" style="display:none; position:fixed; top:50%; left:50%; transform:translate(-50%,-50%); background:white; padding:20px; border-radius:8px; box-shadow:0 0 10px rgba(0,0,0,0.3); width:350px;">
  <h3 id="serviceTitle"></h3>
  <div id="serviceForm"></div>
  <button onclick="closeService()" style="margin-top:10px; padding:10px; width:100%; background:#1a73e8; color:white; border:none;">Close</button>
</div>

<script type="module">
  // Firebase Config (পূর্বে কপি করা কোড)
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.9.0/firebase-app.js";
  import { getFirestore, doc, setDoc, getDoc, updateDoc, collection, addDoc } from "https://www.gstatic.com/firebasejs/10.9.0/firebase-firestore.js";

  const firebaseConfig = {
    apiKey: "...",
    authDomain: "...",
    projectId: "...",
    storageBucket: "...",
    messagingSenderId: "...",
    appId: "..."
  };

  const app = initializeApp(firebaseConfig);
  const db = getFirestore(app);

  // Services Data
  const services = {
    nid: { title:'NID সেবা', options:[
      {name:'সার্ভার কপি', price:100},
      {name:'সাইন কপি', price:120},
      {name:'আইডি কার্ড', price:150},
      {name:'আইডি কার্ড মেইক', price:10}
    ], fields:['name','idNumber','dob']},
    
    biometric: { title:'বায়োমেট্রিক সেবা', options:[
      {name:'গ্রামিনফোন', price:100},
      {name:'বাংলালিংক', price:120},
      {name:'রবি/এয়ারটেল', price:150}
    ], fields:['mobileNumber']},

    tin: { title:'TIN সার্টিফিকেট', options:[
      {name:'হারানো টিন উত্তোলন', price:100}
    ], fields:['idNumber','tinNumber','mobileNumber']},

    vaccine: { title:'করোনা ভ্যাকসিন', options:[
      {name:'হারানো সনদ ডাউনলোড', price:100},
      {name:'২ ডোস নতুন সনদ', price:120},
      {name:'৩ ডোস নতুন সনদ', price:150},
      {name:'৪ ডোস নতুন সনদ', price:10}
    ], fields:['name','fatherName','motherName','idNumber','dob','mobile','address','center','age','vaccine','bloodGroup']}
  };

  const serviceModal = document.getElementById('serviceModal');
  const serviceTitle = document.getElementById('serviceTitle');
  const serviceFormDiv = document.getElementById('serviceForm');

  function openService(key){
    const svc = services[key];
    serviceTitle.textContent = svc.title;
    serviceFormDiv.innerHTML = '';

    // কাজের ধরন dropdown
    let optionHTML = '<select id="serviceOption">';
    svc.options.forEach((o,i)=>{ optionHTML += `<option value="${i}">${o.name} - ${o.price} টাকা</option>`; });
    optionHTML += '</select>';
    serviceFormDiv.innerHTML += `<div><strong>কাজের ধরন:</strong><br>${optionHTML}</div><br>`;

    // Fields
    svc.fields.forEach(f=>{
      serviceFormDiv.innerHTML += `<input type="text" id="${f}" placeholder="${f}" style="width:100%; margin:5px 0; padding:8px;">`;
    });

    // Submit Button
    serviceFormDiv.innerHTML += `<button style="margin-top:10px; width:100%; padding:10px; background:#1a73e8; color:white; border:none;" onclick="submitService('${key}')">Submit</button>`;

    serviceModal.style.display='block';
    document.getElementById('overlay').style.display='block';
  }

  function closeService(){
    serviceModal.style.display='none';
    document.getElementById('overlay').style.display='none';
  }

  async function submitService(key){
    const svc = services[key];
    const selectedOption = document.getElementById('serviceOption').value;
    const price = svc.options[selectedOption].price;

    // Collect Field Data
    const data = {};
    svc.fields.forEach(f=>{
      data[f] = document.getElementById(f).value;
    });
    data['service'] = svc.options[selectedOption].name;
    data['price'] = price;
    data['status'] = 'Pending';

    // Add to Firestore
    try{
      await addDoc(collection(db, 'orders'), data);
      alert('Order Submitted! Balance will be deducted after admin approval.');
      closeService();
    }catch(e){ console.error(e); alert('Error submitting order'); }
  }
</script>
<!-- Admin Panel Modal -->
<div id="adminModal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.7);">
  <div style="background:white; width:90%; max-width:600px; margin:50px auto; padding:20px; border-radius:8px; overflow-y:auto; max-height:80vh;">
    <h2>Admin Dashboard</h2>
    <div id="balanceAdmin" style="margin-bottom:20px;">
      <h3>Update Balance</h3>
      <input type="text" id="adminUser" placeholder="User Name" style="width:40%; padding:5px;">
      <input type="number" id="adminAmount" placeholder="Amount" style="width:40%; padding:5px;">
      <button onclick="updateUserBalance()" style="padding:5px; background:#1a73e8; color:white;">Add/Deduct</button>
    </div>
    <div id="ordersAdmin">
      <h3>All Orders</h3>
      <table border="1" width="100%" style="border-collapse:collapse; text-align:center;">
        <thead>
          <tr>
            <th>User</th>
            <th>Service</th>
            <th>Price</th>
            <th>Status</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody id="ordersTable"></tbody>
      </table>
    </div>
    <button onclick="closeAdmin()" style="margin-top:10px; padding:10px; width:100%; background:red; color:white;">Close</button>
  </div>
</div>

<script type="module">
  // Firebase imports & initialization Step 2 অনুযায়ী থাকবে

  const adminModal = document.getElementById('adminModal');

  // Admin open (placeholder: later authentication)
  function openAdmin(){ adminModal.style.display='block'; loadOrders(); }
  function closeAdmin(){ adminModal.style.display='none'; }

  // Load Orders
  async function loadOrders(){
    const tbody = document.getElementById('ordersTable');
    tbody.innerHTML = '';
    const querySnapshot = await getDocs(collection(db,'orders'));
    querySnapshot.forEach(docSnap=>{
      const data = docSnap.data();
      const tr = document.createElement('tr');
      tr.innerHTML = `
        <td>${data.name || data.mobileNumber || 'User'}</td>
        <td>${data.service}</td>
        <td>${data.price}</td>
        <td>${data.status}</td>
        <td>
          ${data.status==='Pending'?`<button onclick="markSuccess('${docSnap.id}',${data.price})">Mark Success</button>`:''}
        </td>
      `;
      tbody.appendChild(tr);
    });
  }

  // Mark Success & Deduct Balance
  async function markSuccess(docId, price){
    try{
      // Update Order Status
      const orderRef = doc(db,'orders',docId);
      await updateDoc(orderRef, {status:'Completed'});

      // Deduct from User Balance (Example: auto user in order data)
      const userRef = doc(db,'users','TestUser'); // Later: map order -> user
      const userSnap = await getDoc(userRef);
      let currentBalance = userSnap.data().balance || 0;
      await updateDoc(userRef,{balance:currentBalance - price});

      alert('Order Completed & Balance Deducted!');
      loadOrders();
      updateBalance(currentBalance - price); // Update top balance icon
    }catch(e){ console.error(e); alert('Error updating order'); }
  }

  // Update User Balance manually
  async function updateUserBalance(){
    const userName = document.getElementById('adminUser').value;
    const amount = parseInt(document.getElementById('adminAmount').value);
    if(!userName || !amount) return alert('Enter User & Amount');
    const userRef = doc(db,'users','TestUser'); // Later: map by name
    const userSnap = await getDoc(userRef);
    let currentBalance = userSnap.data().balance || 0;
    await updateDoc(userRef,{balance:currentBalance + amount});
    updateBalance(currentBalance + amount);
    alert('Balance Updated!');
  }
</script>
<!-- Admin Price Edit & File Upload Section -->
<div id="priceFileAdmin" style="margin:20px 0; text-align:center;">
  <h3>Price Edit & File Upload</h3>
  <div>
    <label>Service: 
      <select id="adminServiceSelect">
        <option value="nid">NID</option>
        <option value="biometric">Biometric</option>
        <option value="tin">TIN</option>
        <option value="vaccine">Vaccine</option>
      </select>
    </label>
  </div>
  <div>
    <input type="text" id="adminOptionName" placeholder="Option Name" style="margin:5px 0; padding:5px;">
    <input type="number" id="adminOptionPrice" placeholder="New Price" style="margin:5px 0; padding:5px;">
    <button onclick="updatePrice()">Update Price</button>
  </div>
  <div style="margin-top:10px;">
    <input type="file" id="adminFileUpload">
    <input type="text" id="orderIdForFile" placeholder="Order ID">
    <button onclick="uploadFile()">Upload File</button>
  </div>
</div>

<script type="module">
  import { getStorage, ref, uploadBytes, getDownloadURL } from "https://www.gstatic.com/firebasejs/10.9.0/firebase-storage.js";

  const storage = getStorage();

  // Update Price
  async function updatePrice(){
    const service = document.getElementById('adminServiceSelect').value;
    const optionName = document.getElementById('adminOptionName').value;
    const newPrice = parseInt(document.getElementById('adminOptionPrice').value);
    if(!optionName || !newPrice) return alert('Enter Option Name & Price');
    
    const serviceDocRef = doc(db, 'services', service);
    const serviceSnap = await getDoc(serviceDocRef);
    let options = serviceSnap.data()?.options || [];
    let updated = false;
    options = options.map(o=>{
      if(o.name === optionName){
        updated = true;
        return {...o, price:newPrice};
      }
      return o;
    });
    if(!updated) options.push({name:optionName, price:newPrice});
    
    await setDoc(serviceDocRef, {options}, {merge:true});
    alert('Price Updated Successfully!');
  }

  // Upload File for Order
  async function uploadFile(){
    const orderId = document.getElementById('orderIdForFile').value;
    const file = document.getElementById('adminFileUpload').files[0];
    if(!orderId || !file) return alert('Select Order ID & File');

    const storageRef = ref(storage, `orderFiles/${orderId}_${file.name}`);
    try{
      await uploadBytes(storageRef, file);
      const url = await getDownloadURL(storageRef);
      // Save URL to Firestore order
      const orderRef = doc(db,'orders',orderId);
      await updateDoc(orderRef,{fileURL:url, status:'Completed'});
      alert('File Uploaded & Order Completed!');
      loadOrders(); // Refresh Admin Order Table
    }catch(e){ console.error(e); alert('Upload Failed'); }
  }

  // Customer Download Button
  async function customerDownload(orderId){
    const orderRef = doc(db,'orders',orderId);
    const orderSnap = await getDoc(orderRef);
    if(orderSnap.exists()){
      const url = orderSnap.data().fileURL;
      if(url) window.open(url,'_blank');
      else alert('File not uploaded yet');
    }else{ alert('Order not found'); }
  }
</script>
<style>
  body{font-family:sans-serif; margin:0; padding:0;}
  header, footer{background:#1a73e8; color:white; padding:15px; text-align:center;}
  #menu button{flex:1; min-width:100px; padding:12px; margin:5px; border:none; border-radius:5px; background:#2196f3; color:white; cursor:pointer;}
  input, select, button{border-radius:5px; border:1px solid #ccc; box-sizing:border-box;}
  @media(max-width:600px){
    #menu{flex-direction:column;}
    input, select, button{width:100%;}
  }
</style>

<header>
  <h1>Smart Tech Studio</h1>
  <marquee behavior="scroll" direction="left" id="announcement">Welcome! Admin can edit announcements anytime.</marquee>
  <div style="display:flex; justify-content:space-between; margin-top:10px;">
    <button onclick="openProfile()">Profile</button>
    <button onclick="openBalance()">Balance: <span id="balanceDisplay">0</span> ৳</button>
    <button onclick="openAdmin()">Admin Panel</button>
  </div>
</header>

<!-- Service Menu + Modals: Step 2 -->
<div id="menu"> <!-- Step 2 Menu Buttons --> </div>
<div id="serviceModal"> <!-- Step 2 Service Modal --> </div>
<div id="overlay" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.5);"></div>

<!-- Admin Modal + Step 3 + Step 4 -->
<div id="adminModal"> <!-- Step 3 Admin Panel --> </div>
<div id="priceFileAdmin"> <!-- Step 4 Price Edit + File Upload --> </div>

<footer>
  <small>Developed by: Md. Rashikul Islam | Proprietor Smart Tech Studio | Newashi, Nageswari, Kurigram</small>
  <div style="display:flex; justify-content:space-between; margin-top:5px;">
    <button onclick="joinWhatsapp()">Join Whatsapp Group</button>
    <button onclick="viewAdmin()">View Admin</button>
  </div>
</footer>

<script type="module">
  // Firebase Config + Firestore + Storage Imports (Step 2–4)
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.9.0/firebase-app.js";
  import { getFirestore, doc, collection, addDoc, getDoc, getDocs, updateDoc, setDoc } from "https://www.gstatic.com/firebasejs/10.9.0/firebase-firestore.js";
  import { getStorage, ref, uploadBytes, getDownloadURL } from "https://www.gstatic.com/firebasejs/10.9.0/firebase-storage.js";

  const firebaseConfig = { /* Your Firebase Config */ };
  const app = initializeApp(firebaseConfig);
  const db = getFirestore(app);
  const storage = getStorage();

  // Announcement Edit (Admin)
  function editAnnouncement(newText){ document.getElementById('announcement').textContent=newText; }

  // Balance Display Update
  function updateBalance(val){ document.getElementById('balanceDisplay').textContent = val; }

  // Join Whatsapp / View Admin Placeholder
  function joinWhatsapp(){ alert('Open Whatsapp Group Link'); }
  function viewAdmin(){ alert('Show Admin Info'); }

  // Profile Modal Placeholder
  function openProfile(){ alert('Open Profile Modal'); }
  function openBalance(){ alert('Open Balance Info'); }

  // Service Modal + Submit + Firestore (Step 2)
  // Admin Panel + Orders + File Upload + Price Edit (Step 3 & 4)
  // … Step 2–4 Code সব এখানে থাকবে (copy paste)

</script>
