# Tourism-Camton
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Camp-Ton | Hotel & Camp Booking</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: url('https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=1600&q=80') no-repeat center center fixed;
      background-size: cover;
      color: #333;
    }
    header {
      background: rgba(0, 102, 204, 0.85);
      color: white;
      padding: 1rem;
      text-align: center;
    }
    nav {
      display: flex;
      justify-content: center;
      background: rgba(0, 73, 153, 0.85);
    }
    nav button {
      background: transparent;
      border: none;
      color: white;
      padding: 1rem;
      cursor: pointer;
      font-size: 1rem;
    }
    nav button.active {
      background: rgba(0, 102, 204, 0.9);
    }
    .tab {
      display: none;
      padding: 2rem;
    }
    .tab.active {
      display: block;
    }
    .card {
      border: 1px solid #ddd;
      padding: 1rem;
      margin: 1rem 0;
      border-radius: 8px;
      background: rgba(255, 255, 255, 0.95);
    }
    .card img {
      width: 100%;
      max-height: 200px;
      object-fit: cover;
      border-radius: 6px;
      margin-bottom: 0.5rem;
    }
    .btn {
      background: #0066cc;
      color: white;
      border: none;
      padding: 0.5rem 1rem;
      border-radius: 4px;
      cursor: pointer;
    }
    .btn:hover { background: #004999; }
    /* Login screens */
    .center-screen {
      display:flex;
      justify-content:center;
      align-items:center;
      height:100vh;
      background: rgba(0,0,0,0.55) url('https://images.unsplash.com/photo-1506748686214-e9df14d4d9d0?auto=format&fit=crop&w=1600&q=80') no-repeat center center/cover;
      background-blend-mode: darken;
      color: white;
      flex-direction: column;
      text-align: center;
      padding: 1rem;
    }
    .auth-form {
      background: rgba(255,255,255,0.06);
      padding: 1rem;
      border-radius: 8px;
      width: 320px;
      display:flex;
      flex-direction:column;
      gap:0.6rem;
    }
    .auth-form input {
      padding: .5rem;
      border-radius: 4px;
      border: none;
    }
    /* location grid */
    #himachalLocations {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 1rem;
    }
    #himachalLocations img { height: 180px; object-fit:cover; border-radius:8px; width:100%; }
    /* modal */
    .modal-backdrop {
      display:none;
      position:fixed; inset:0; background: rgba(0,0,0,0.5);
      justify-content:center; align-items:center;
    }
    .modal {
      background:white; padding:1.5rem; border-radius:8px; min-width:320px; text-align:center;
    }
  </style>
</head>
<body>
  <!-- Login choice -->
  <div id="loginChoice" class="center-screen">
    <h1>Welcome to Camp-Ton</h1>
    <p>Explore Himachal Pradesh’s Hotels & Camps</p>
    <div style="display:flex; gap:12px; margin-top:12px;">
      <button class="btn" onclick="showGuestLogin()">Login as Guest</button>
      <button class="btn" onclick="showHostLogin()">Login as Host</button>
    </div>
  </div>

  <!-- Guest Login -->
  <div id="guestLogin" class="center-screen" style="display:none;">
    <h2>Guest Login</h2>
    <form class="auth-form" onsubmit="guestLogin(event)">
      <input id="guestEmail" type="email" placeholder="Email" required>
      <input id="guestPhone" type="tel" placeholder="Phone number" required>
      <input id="guestPassword" type="password" placeholder="Password" required>
      <div style="display:flex; gap:8px; justify-content:space-between;">
        <button class="btn" type="submit">Enter as Guest</button>
        <button class="btn" type="button" onclick="backToChoice()">Back</button>
      </div>
    </form>
  </div>

  <!-- Host Login -->
  <div id="hostLogin" class="center-screen" style="display:none;">
    <h2>Host Login</h2>
    <form class="auth-form" onsubmit="hostLogin(event)">
      <input id="hostEmail" type="email" placeholder="Email" required>
      <input id="hostPhone" type="tel" placeholder="Phone number" required>
      <input id="hostPassword" type="password" placeholder="Password" required>
      <div style="display:flex; gap:8px; justify-content:space-between;">
        <button class="btn" type="submit">Enter as Host</button>
        <button class="btn" type="button" onclick="backToChoice()">Back</button>
      </div>
    </form>
  </div>

  <!-- Main Website -->
  <div id="mainSite" style="display:none;">
    <header>
      <h1>Camp-Ton</h1>
    </header>

    <nav>
      <button class="tablink active" onclick="openTab(event,'locations')">Locations</button>
      <button class="tablink" onclick="openTab(event,'browse')">Browse</button>
      <button class="tablink" onclick="openTab(event,'host')">Host</button>
      <button class="tablink" onclick="openTab(event,'bookings')">My Bookings</button>
      <button class="tablink" onclick="openTab(event,'adventures')">Adventures</button>
    </nav>

    <!-- Locations -->
    <section id="locations" class="tab active">
      <h2>Explore Himachal Pradesh</h2>
      <div id="himachalLocations"></div>
    </section>

    <!-- Browse -->
    <section id="browse" class="tab">
      <h2>Available Stays</h2>
      <div id="listingContainer"></div>
    </section>

    <!-- Host form -->
    <section id="host" class="tab">
      <h2>Host Your Property</h2>
      <form id="hostForm">
        <label>Name: <input type="text" id="name" required></label><br><br>
        <label>Type:
          <select id="type">
            <option>Hotel</option>
            <option>Camp</option>
            <option>Home Stay</option>
          </select>
        </label><br><br>
        <label>Location:
          <select id="location" required>
            <option value="">Select Location</option>
            <option>Manali</option><option>Shimla</option><option>Kullu</option><option>Dharamshala</option>
            <option>Dalhousie</option><option>Kasauli</option><option>Chamba</option><option>Spiti Valley</option>
            <option>Kasol</option><option>Bir Billing</option><option>Kinnaur</option><option>Palampur</option>
            <option>Mcleodganj</option><option>Key Monastery</option><option>Rohtang Pass</option>
          </select>
        </label><br><br>
        <label>Price per Night: <input type="number" id="price" required></label><br><br>
        <label>Upload Image: <input type="file" id="image" accept="image/*"></label><br><br>
        <button type="submit" class="btn">Add Listing</button>
      </form>
    </section>

    <!-- Bookings -->
    <section id="bookings" class="tab">
      <h2>My Bookings</h2>
      <div id="bookingContainer"></div>
    </section>

    <!-- Adventures -->
    <section id="adventures" class="tab">
      <h2>Adventure Activities</h2>
      <div id="adventureContainer"></div>
    </section>
  </div>

  <!-- Location Options Modal -->
  <div id="locationOptionsModal" class="modal-backdrop" style="display:none;">
    <div class="modal">
      <h2 id="selectedLocationName"></h2>
      <div style="display:flex; gap:8px; justify-content:center;">
        <button class="btn" onclick="showListings('Home Stay')">Home Stays</button>
        <button class="btn" onclick="showListings('Hotel')">Hotels</button>
        <button class="btn" onclick="showListings('Camp')">Camps</button>
      </div>
      <br/>
      <button class="btn" onclick="closeLocationOptions()">Close</button>
    </div>
  </div>

<script>
  // Keep a list of tab buttons (exists after DOM load, script runs at end)
  const tabs = document.querySelectorAll('.tablink');

  function openTab(event, id) {
    document.querySelectorAll('.tab').forEach(tab => tab.classList.remove('active'));
    const targetTab = document.getElementById(id);
    if (targetTab) targetTab.classList.add('active');

    // remove active from all nav buttons
    document.querySelectorAll('.tablink').forEach(btn => btn.classList.remove('active'));

    // If called via real event (click), use event.target; otherwise try to find nav button that opens this id
    if (event && event.target) {
      event.target.classList.add('active');
    } else {
     const btn = document.querySelector(`.tablink[onclick*='${id}']`);


      if (btn) btn.classList.add('active');
    }
  }

  // Login flow (Guest/Host credential prompts)
  function showGuestLogin() {
    document.getElementById('loginChoice').style.display = 'none';
    document.getElementById('guestLogin').style.display = 'flex';
  }
  function showHostLogin() {
    document.getElementById('loginChoice').style.display = 'none';
    document.getElementById('hostLogin').style.display = 'flex';
  }
  function backToChoice() {
    document.getElementById('guestLogin').style.display = 'none';
    document.getElementById('hostLogin').style.display = 'none';
    document.getElementById('loginChoice').style.display = 'flex';
  }

  function guestLogin(e) {
    e.preventDefault();
    const email = document.getElementById('guestEmail').value.trim();
    const phone = document.getElementById('guestPhone').value.trim();
    const pass = document.getElementById('guestPassword').value;
    if (!email || !phone || !pass) { alert('Please fill all fields'); return; }
    sessionStorage.setItem('currentUser', JSON.stringify({role:'Guest', email, phone}));
    enterSite('Guest');
  }

  function hostLogin(e) {
    e.preventDefault();
    const email = document.getElementById('hostEmail').value.trim();
    const phone = document.getElementById('hostPhone').value.trim();
    const pass = document.getElementById('hostPassword').value;
    if (!email || !phone || !pass) { alert('Please fill all fields'); return; }
    sessionStorage.setItem('currentUser', JSON.stringify({role:'Host', email, phone}));
    enterSite('Host');
  }

  function enterSite(role) {
    // hide all login UI
    document.getElementById('loginChoice').style.display = 'none';
    document.getElementById('guestLogin').style.display = 'none';
    document.getElementById('hostLogin').style.display = 'none';
    // show main app
    document.getElementById('mainSite').style.display = 'block';
    // render UI (in case new)
    renderLocations();
    renderListings();
    renderBookings();
    renderAdventures();
    alert(`${role} logged in successfully!`);


  }

  // Data
  const locations = [
    { name: "Manali", img: "https://upload.wikimedia.org/wikipedia/commons/f/f6/Manali_City.jpg" },
    { name: "Shimla", img: "https://upload.wikimedia.org/wikipedia/commons/d/d3/The_Ridge%2C_Shimla.jpg" },
    { name: "Kullu", img: "https://upload.wikimedia.org/wikipedia/commons/6/6b/Kullu_valley.jpg" },
    { name: "Dharamshala", img: "https://upload.wikimedia.org/wikipedia/commons/5/57/Dharamshala_valley.jpg" },
    { name: "Dalhousie", img: "https://upload.wikimedia.org/wikipedia/commons/5/54/Dalhousie_Himachal.jpg" },
    { name: "Kasauli", img: "https://upload.wikimedia.org/wikipedia/commons/4/4c/Kasauli_Himachal.jpg" },
    { name: "Chamba", img: "https://upload.wikimedia.org/wikipedia/commons/3/3e/Chamba_Town.jpg" },
    { name: "Spiti Valley", img: "https://upload.wikimedia.org/wikipedia/commons/c/c9/Spiti_Valley_Himachal.jpg" },
    { name: "Kasol", img: "https://upload.wikimedia.org/wikipedia/commons/2/2b/Kasol_Village.jpg" },
    { name: "Bir Billing", img: "https://upload.wikimedia.org/wikipedia/commons/f/fd/Bir_Billing_Paragliding.jpg" },
    { name: "Kinnaur", img: "https://upload.wikimedia.org/wikipedia/commons/5/58/Kinnaur_valley.jpg" },
    { name: "Palampur", img: "https://upload.wikimedia.org/wikipedia/commons/9/95/Palampur_Tea_Garden.jpg" },
    { name: "Mcleodganj", img: "https://upload.wikimedia.org/wikipedia/commons/f/fd/Mcleodganj.jpg" },
    { name: "Key Monastery", img: "https://upload.wikimedia.org/wikipedia/commons/6/65/Key_Monastery_Spiti.jpg" },
    { name: "Rohtang Pass", img: "https://upload.wikimedia.org/wikipedia/commons/e/ea/Rohtang_Pass.jpg" }
  ];

  const adventures = [
    { location: "Manali", name: "Paragliding", price: 2500, img: "https://upload.wikimedia.org/wikipedia/commons/3/3c/Paragliding_in_Manali.jpg" },
    { location: "Manali", name: "River Rafting", price: 1500, img: "https://upload.wikimedia.org/wikipedia/commons/7/7a/Rafting_in_Manali.jpg" },
    { location: "Kasol", name: "Trekking", price: 1200, img: "https://upload.wikimedia.org/wikipedia/commons/7/7e/Kasol_Trek.jpg" },
    { location: "Kasol", name: "Camping", price: 1000, img: "https://upload.wikimedia.org/wikipedia/commons/1/1a/Camping_in_Kasol.jpg" }
  ];

  // Listings / Bookings persistence
  let listings = JSON.parse(localStorage.getItem('listings')) || [];
  let bookings = JSON.parse(localStorage.getItem('bookings')) || [];

  // Renders
  function renderLocations() {
    const container = document.getElementById('himachalLocations');
    if (!container) return;
    container.innerHTML = '';
    locations.forEach(loc => {
      const card = document.createElement('div');
      card.className = 'card';
      card.innerHTML = `
        <img src="${loc.img}" alt="${loc.name}">
        <h3>${loc.name}</h3>
      `;
      card.style.cursor = 'pointer';
      card.onclick = () => openLocationOptions(loc.name);
      container.appendChild(card);
    });
  }

  function renderListings() {
    const container = document.getElementById('listingContainer');
    if (!container) return;
    container.innerHTML = '';
    listings.forEach((item, index) => {
      const card = document.createElement('div');
      card.className = 'card';
      card.innerHTML = `
        <img src="${item.image || 'https://via.placeholder.com/300x200?text=No+Image'}" alt="${item.name}">
        <h3>${item.name}</h3>
        <p>Type: ${item.type}</p>
        <p>Location: ${item.location}</p>
        <p>Price: ₹${item.price}/night</p>
        <button class='btn' onclick='book(${index})'>Book Now</button>
      `;
      container.appendChild(card);
    });
  }

  function renderBookings() {
    const container = document.getElementById('bookingContainer');
    if (!container) return;
    container.innerHTML = '';
    if (bookings.length === 0) {
      container.innerHTML = '<p>No bookings yet.</p>';
      return;
    }
    bookings.forEach((b, i) => {
      const card = document.createElement('div');
      card.className = 'card';
      card.innerHTML = `
        <img src="${b.image || 'https://via.placeholder.com/300x200?text=No+Image'}" alt="${b.name}">
        <h3>${b.name}</h3>
        <p>Type: ${b.type}</p>
        <p>Location: ${b.location}</p>
        <p>Price: ₹${b.price}${b.type === 'Adventure' ? '' : '/night'}</p>
        <button class='btn' onclick='cancelBooking(${i})'>Cancel</button>
      `;
      container.appendChild(card);
    });
  }

  function renderAdventures() {
    const container = document.getElementById('adventureContainer');
    if (!container) return;
    container.innerHTML = '';
    adventures.forEach((adv, i) => {
      const card = document.createElement('div');
      card.className = 'card';
      card.innerHTML = `
        <img src="${adv.img}" alt="${adv.name}">
        <h3>${adv.name} - ${adv.location}</h3>
        <p>Price: ₹${adv.price}</p>
        <button class="btn" onclick="bookAdventure(${i})">Book Adventure</button>
      `;
      container.appendChild(card);
    });
  }

  // Location modal
  function openLocationOptions(locationName) {
    document.getElementById('selectedLocationName').textContent = locationName;
    document.getElementById('locationOptionsModal').style.display = 'flex';
    window.selectedLocation = locationName;
  }
  function closeLocationOptions() { document.getElementById('locationOptionsModal').style.display = 'none'; }

  function showListings(type) {
    closeLocationOptions();
    openTab({ target: document.querySelector(".tablink[onclick*='browse']") }, 'browse');
    const container = document.getElementById('listingContainer');
    container.innerHTML = '';
    listings
      .filter(item => item.type === type && item.location === window.selectedLocation)
      .forEach((item) => {
        const originalIndex = listings.indexOf(item);
        const card = document.createElement('div');
        card.className = 'card';
        card.innerHTML = `
          <img src="${item.image || 'https://via.placeholder.com/300x200?text=No+Image'}" alt="${item.name}">
          <h3>${item.name}</h3>
          <p>Type: ${item.type}</p>
          <p>Location: ${item.location}</p>
          <p>Price: ₹${item.price}/night</p>
          <button class='btn' onclick='book(${originalIndex})'>Book Now</button>
        `;
        container.appendChild(card);
      });
  }

  // Host form handling (image as dataURL)
  document.getElementById('hostForm').addEventListener('submit', function(e) {
    e.preventDefault();
    const name = document.getElementById('name').value.trim();
    const type = document.getElementById('type').value;
    const price = document.getElementById('price').value;
    const location = document.getElementById('location').value;
    const imageFile = document.getElementById('image').files[0];

    if (!name || !type || !price || !location) { alert('Please fill all fields'); return; }

    if (imageFile) {
      const reader = new FileReader();
      reader.onload = function(evt) {
        saveListing(name, type, price, location, evt.target.result);
      };
      reader.readAsDataURL(imageFile);
    } else {
      saveListing(name, type, price, location, null);
    }
    this.reset();
  });

  function saveListing(name, type, price, location, image) {
    listings.push({ name, type, price, location, image });
    localStorage.setItem('listings', JSON.stringify(listings));
    renderListings();
    alert('Property added successfully!');
  }

  // Book functions
  function book(index) {
    if (index < 0 || index >= listings.length) { alert('Invalid listing'); return; }
    const listing = listings[index];
    const booking = { name: listing.name, type: listing.type, location: listing.location, price: listing.price, image: listing.image };
    bookings.push(booking);
    localStorage.setItem('bookings', JSON.stringify(bookings));
    renderBookings();
    alert('Booking confirmed!');
  }

  function cancelBooking(index) {
    bookings.splice(index, 1);
    localStorage.setItem('bookings', JSON.stringify(bookings));
    renderBookings();
  }

  function bookAdventure(index) {
    const adv = adventures[index];
    const booking = { name: adv.name, type: 'Adventure', location: adv.location, price: adv.price, image: adv.img };
    bookings.push(booking);
    localStorage.setItem('bookings', JSON.stringify(bookings));
    renderBookings();
    openTab({ target: document.querySelector(".tablink[onclick*='bookings']") }, 'bookings');
   alert(`Adventure booked: ${adv.name} in ${adv.location}`);

  }

  // Initial render (keeps original functionalities ready)
  function init() {
    renderLocations();
    renderListings();
    renderBookings();
    renderAdventures();
  }
  init();
</script>
</body>
</html>
