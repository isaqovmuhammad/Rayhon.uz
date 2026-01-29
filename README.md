<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<title>Rayhon Savdo Markazi</title>
<style>
:root {
  --green: #2ecc71;
  --dark: rgba(0,0,0,.55);
  --glass: rgba(255,255,255,.15);
  --header-color: #fff;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', 'Segoe UI', sans-serif;
}

body {
  height: 100vh;
  color: #fff;
  overflow: hidden;
}

/* VIDEO BACKGROUND */
video {
  position: fixed;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: -2;
}

.overlay {
  position: fixed;
  inset: 0;
  background: linear-gradient(to bottom, rgba(0, 0, 0, 0.42), rgba(0,0,0,.5));
  z-index: -1;
}

/* HEADER */
header {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 18px;
  margin-top: 50px;
  text-align: center;
}

header img {
  width: 90px;
}

header h1 {
  font-size: 52px;
  font-weight: 800;
  color: var(--header-color);
  letter-spacing: 5px;
  padding-right: 70px;
  padding-top: 30px;
}

header span {
  display: block;
  font-size: 20px;
  color: #a5a5a5;
  margin-top: 4px;
}

/* SEARCH CARD */
.search-card {
  margin: 150px auto;
  width: 500px;
  padding: 40px;
  border-radius: 24px;
  background: var(--glass);
  backdrop-filter: blur(16px);
  box-shadow: 0 40px 100px rgba(0,0,0,.5);
  text-align: center;
}

.search-card h2 {
  margin-bottom: 25px;
  font-weight: 700;
  font-size: 28px;
  color: #fff;
}

.search {
  display: flex;
  gap: 12px;
}

.search input {
  flex: 1;
  padding: 18px;
  border-radius: 16px;
  border: none;
  font-size: 16px;
  outline: none;
}

.search button {
  padding: 18px 28px;
  border: none;
  border-radius: 16px;
  background: var(--green);
  font-weight: 700;
  cursor: pointer;
  transition: 0.3s;
}

.search button:hover {
  background: #27ae60;
}

/* MODAL */
.modal {
  position: fixed;
  inset: 0;
  display: none;
  justify-content: center;
  align-items: center;
  background: rgba(0,0,0,.65);
}

.modal-box {
  width: 540px;
  max-height: 75vh;
  overflow-y: auto;
  padding: 35px;
  border-radius: 26px;
  background: var(--glass);
  backdrop-filter: blur(20px);
}

.modal-box h3 {
  text-align: center;
  margin-bottom: 18px;
  font-size: 24px;
}

.item {
  padding: 16px;
  margin-bottom: 14px;
  border-radius: 18px;
  background: rgba(0,0,0,.45);
  border-left: 5px solid var(--green);
  font-size: 16px;
}

.close {
  float: right;
  font-size: 28px;
  cursor: pointer;
}

/* FOOTER NOTE */
.footer-note {
  position: fixed;
  bottom: 12px;
  width: 100%;
  text-align: center;
  font-size: 12px;
  color: #ccc;
  font-weight: 500;
  backdrop-filter: blur(6px);
  background: rgba(0,0,0,0.3);
  padding: 6px 0;
  pointer-events: none;
}

/* MOBILE RESPONSIVE */
@media(max-width:600px){
  .search-card{width:90%;margin-top:120px;}
  header h1{font-size:38px;}
  .search input{padding:14px;}
  .search button{padding:14px 22px;}
}
</style>
</head>
<body>

<!-- VIDEO BACKGROUND -->
<video autoplay muted loop>
  <source src="./istockphoto-1492564115-640_adpp_is.mp4" type="video/mp4">
</video>
<div class="overlay"></div>

<!-- HEADER -->
<header>
  <img src="logo.png" alt="">
  <h1>Rayhon savdo markazi</h1>
</header>

<!-- SEARCH CARD -->
<div class="search-card">
  <h2>Mahsulotni topish</h2>
  <div class="search">
    <input id="searchInput" placeholder="Masalan: choy, kolbasa...">
    <button onclick="search()">Qidirish</button>
  </div>
</div>

<!-- MODAL -->
<div class="modal" id="modal">
  <div class="modal-box">
    <span class="close" onclick="closeModal()">×</span>
    <h3>Natijalar</h3>
    <div id="results"></div>
  </div>
</div>

<!-- FOOTER NOTE -->
<div class="footer-note">
  © 2026 Rayhon savdo markazi. Barcha huquqlar himoyalangan. Ushbu datur Rayhon hodimlariga yordam uchun yaratilgan
</div>

<script>
// 1️⃣ Mahsulotlar massivi (boshida 100 ta)
const products = [
  { name: "Pechenye, Maxliyo cake qulupnayli", code: "2800219" },
  { name: "Tort, Benazir Malinali chiskeyk", code: "2800268" },
  { name: "Keks, Waffles donali", code: "2800269" },
  { name: "Ovoş salat barg", code: "2800330" },
  { name: "Pecheno, Tvorojnoe Damashniy", code: "2800500" },
  { name: "Pirojnoe, Benazir barxat arzoni", code: "2800616" },
  { name: "Pirojnoe, Benazir Rulet yong'oqli", code: "2800629" },
  { name: "Tvorojnaya, Damashniy", code: "2800644" },
  { name: "Krishka, Mechta xozayayka Beka orzusi qizil dona", code: "2800655" },
  { name: "Kolbasa, Osiyo Millano", code: "2800678" },
  { name: "Pecheno, Napaleon", code: "2800700" },
  { name: "Ovoş, Chesnok", code: "2800701" },
  { name: "Ovoş, Janduk", code: "2800702" },
  { name: "Kukat, osh-kuk ukrop kuk piez", code: "2800703" },
  { name: "Pechenye, Bek paxlava avgon donali", code: "2800704" },
  { name: "Pirojnoe, Chiskeyk malina fistashka", code: "2800705" },
  { name: "Pirojnoe, Chiskeyk karamelniy", code: "2800706" },
  { name: "Pirojnoe, Chiskeyk yagodny", code: "2800707" },
  { name: "Pirojnoe, Biliniy", code: "2800708" },
  { name: "Pirojnoe, Trayfil krasny", code: "2800709" },
  { name: "Pirojnoe, Trayfil shoko", code: "2800710" },
  { name: "Pirojnoe, Srikers", code: "2800711" },
  { name: "Pirojnoe, Tvorojniy", code: "2800712" },
  { name: "Pirojnoe, Tort Milka", code: "2800713" },
  { name: "Pirojnoe, Tort Rafaello", code: "2800714" },
  { name: "Pirojnoe, Tort Mevali", code: "2800715" },
  { name: "Pirojnoe, Tort Medovik", code: "2800716" },
  { name: "Vafli, Veniski dona", code: "2800717" },
  { name: "Pirojnoe, Baxo cakes tvorokli", code: "2800718" },
  { name: "Ovoş, salat barg myata+pudrasha", code: "2800719" },
  { name: "Zelen, Myata", code: "2800720" },
  { name: "Ovoş, Rediska", code: "2800721" },
  { name: "Ovoş, Gul Karam", code: "2800722" },
  { name: "Ovoş, Shalg'am Yangi vog'li", code: "2800723" },
  { name: "Ovoş, Ok Balgar kg", code: "2800724" },
  { name: "Ovoş, Pudra", code: "2800725" },
  { name: "Ovoş, Sitifil", code: "2800726" },
  { name: "Medovik, Ok", code: "2800727" },
  { name: "Ovoş, Dumbul", code: "2800728" },
  { name: "Pirojnoe, Benazir Snickers Dollor", code: "2800729" },
  { name: "Pirojnoe, Andijon tort", code: "2800730" },
  { name: "Pirojnoe, Ptich chiskeyk", code: "2800731" },
  { name: "Pirojnoe, Yojik", code: "2800732" },
  { name: "Chesnok, Chesnok", code: "2800733" },
  { name: "Prozhniy, Maxroviy", code: "2800734" },
  { name: "Medovik, Ok", code: "2800743" },
  { name: "Ovoş, Janduk", code: "2800821" },
  { name: "Pecheno, Pirozhni", code: "2800823" },
  { name: "Pirojnoe, Medovik", code: "2800835" },
  { name: "Prozhniy, Nodira cherniy bumer", code: "2800836" },
  { name: "Pirozhny, Mevali", code: "2800846" },
  { name: "Ovoş, Sitifil kabachki", code: "2800847" },
  { name: "Pirozhny, Pistali kaymokli", code: "2800851" },
  { name: "Pechenye, Biskvit Choko", code: "2800867" },
  { name: "Pirojnoe, Benazir Cheripashka", code: "2800878" },
  { name: "Idish Adnarazviy Kosa", code: "2800879" },
  { name: "Pirozhny, Yong'oqli", code: "2800885" },
  { name: "Tort, Benazir Medovik +(chokopay)+(barxat)+medovik malina", code: "2800899" },
  { name: "Tort, Benazir Malinali chiskeyk", code: "2800900" },
  { name: "Rulet, Benazir Snickers", code: "2800901" },
  { name: "Tort, Benazir Napalyon+Raduga", code: "2800902" },
  { name: "Chiskeyk, Benazir Pitichi+Kofeli+Aricha", code: "2800904" },
  { name: "Qalampir Achchiq", code: "2800921" },
  { name: "Andijon Napalyon molakoli", code: "2800933" },
  { name: "Tort, Andijon Qoravoy", code: "2800934" },
  { name: "Pirozhni, Nodira shokoladlik", code: "2800936" },
  { name: "Pirojniy, Benazir Tarnado", code: "2800953" },
  { name: "Napalyon, Nodira shirinliklari", code: "2800973" },
  { name: "Pirojnoe, Benazir Tarnado+Yojik+Mars", code: "2800977" },
  { name: "Kurasan, Smile", code: "2800983" },
  { name: "Ovoş, Tok Barg", code: "2800990" },
  { name: "Chay, Imron Tea 88-10", code: "2900001" },
  { name: "Chay, Gold tea j2 9501 kiloli", code: "2900002" },
  { name: "Chay, Oolong j4 9500 kiloli", code: "2900003" },
  { name: "Chay, Gold tea 93-71 zira kiloli", code: "2900004" },
  { name: "Chay, Rizq", code: "2900005" },
  { name: "Chay, Oolong 3505 kiloli", code: "2900006" },
  { name: "Chay, Oolong Eron qora kiloli", code: "2900007" },
  { name: "Korm, Melvin kg", code: "2900011" },
  { name: "Korm, Felix kg", code: "2900012" },
  { name: "Korm, Melwin Snejok kuritsa i indeyka", code: "2900013" },
  { name: "Shokolad, Patchi Lux Nok", code: "2900014" },
  { name: "Shokolad, Patchi Dubay", code: "2900015" },
  { name: "Batonchik, Strobar kg", code: "2900016" },
  { name: "Shokolad, Chocolala Dubay kg", code: "2900017" },
  { name: "Shokolad, Chocolala original kg", code: "2900018" },
  { name: "Shokolad, Snickers assorti kg", code: "2900019" },
  { name: "Shokolad, Socado kg", code: "2900020" },
  { name: "Konfety, Picola kg", code: "2900021" },
  { name: "Shokolad, Lyuks kg", code: "2900022" },
  { name: "Shokolad, Milagro kg", code: "2900023" },
  { name: "Konfety, Roshen assortiment kg", code: "2900024" },
  { name: "Konfety, Roshen pchelka kg", code: "2900025" },
  { name: "Konfety, Roshen CoffeeLike", code: "2900026" },
  { name: "Konfety, Roshen Peppinezz", code: "2900027" },
  { name: "Konfety, Roshen Krabs", code: "2900028" },
  { name: "Konfety, Roshen Mentol kg (beri)", code: "2900029" },
  { name: "Konfety, Roshen Butter-Milk", code: "2900030" },
  { name: "Konfety, Roshen Kaplya kg", code: "2900031" }
];

// 2️⃣ Krill → Lotin harflarga o‘giruvchi funksiyasi
function toLatin(text) {
  const map = {
    "ш": "sh", "ч": "ch", "ё": "yo", "ж": "j", "щ": "shch",
    "ю": "yu", "я": "ya", "и": "i", "ы": "i", "э": "e", "е": "e",
    "х": "h", "ц": "ts", "ў": "o", "ғ": "g", "қ": "q", "ь": "",
    "ъ": "", "ё": "yo", "ґ":"g"
  };
  return text.split("").map(c => map[c] || c).join("").toLowerCase();
}

// 3️⃣ So‘zga asoslangan qidiruv
function searchProducts(query) {
  const searchWords = toLatin(query).split(" "); 
  return products.filter(product => {
    const name = toLatin(product.name);
    return searchWords.every(word => name.includes(word));
  });
}

// 4️⃣ Search tugmasi bilan bog‘lash
function search() {
  const q = searchInput.value.trim();
  results.innerHTML = "";
  if (!q) return;

  const found = searchProducts(q);

  results.innerHTML = found.length
    ? found.map(p => `<div class="item"><b>${p.name}</b><br>Kod: ${p.code}</div>`).join("")
    : "<p>Mahsulot topilmadi</p>";

  modal.style.display = "flex";
}

function closeModal() {
  modal.style.display = "none";
}
</script>

</body>
</html>
# Rayhon.uz
