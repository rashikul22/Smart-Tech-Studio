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
