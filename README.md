# Car-rental<!DOCTYPE html><html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>كراء السيارات</title>
  <style>
    body {font-family: Arial; margin:0; background:#f4f4f4;}
    header {background:#111; color:#fff; padding:15px; text-align:center;}
    .container {padding:20px;}
    input, select {width:100%; padding:10px; margin:10px 0; border-radius:8px; border:1px solid #ccc;}
    .cars {display:grid; grid-template-columns:repeat(auto-fit,minmax(250px,1fr)); gap:20px;}
    .car {background:#fff; border-radius:12px; overflow:hidden; box-shadow:0 4px 10px rgba(0,0,0,0.1);}
    .car img {width:100%;}
    .car-content {padding:15px;}
    button {width:100%; padding:10px; background:black; color:white; border:none; border-radius:8px; cursor:pointer;}
    .form-box {background:white; padding:15px; margin-top:20px; border-radius:12px;}
  </style>
</head>
<body><header>
  <h1>🚗 موقع كراء السيارات</h1>
</header><div class="container">
  <input type="text" id="search" placeholder="ابحث عن سيارة...">
  <div class="cars" id="cars"></div>  <div class="form-box" id="bookingBox" style="display:none;">
    <h3>📅 حجز السيارة</h3>
    <input type="text" id="name" placeholder="اسمك">
    <input type="date" id="start">
    <input type="date" id="end">
    <button onclick="confirmBooking()">تأكيد الحجز</button>
  </div>
</div><script>
const cars = [
  { name:"Clio 4", price:6000, img:"https://via.placeholder.com/300" },
  { name:"Golf 7", price:9000, img:"https://via.placeholder.com/300" },
  { name:"Hyundai i10", price:5000, img:"https://via.placeholder.com/300" }
];

let selectedCar = "";

const carsContainer = document.getElementById("cars");
const searchInput = document.getElementById("search");

function displayCars(list){
  carsContainer.innerHTML="";
  list.forEach(car=>{
    carsContainer.innerHTML+=`
    <div class="car">
      <img src="${car.img}">
      <div class="car-content">
        <h3>${car.name}</h3>
        <p>${car.price} دج / اليوم</p>
        <button onclick="bookCar('${car.name}')">احجز الآن</button>
      </div>
    </div>`;
  });
}

function bookCar(name){
  selectedCar = name;
  document.getElementById("bookingBox").style.display="block";
  window.scrollTo(0,document.body.scrollHeight);
}

function confirmBooking(){
  const user = document.getElementById("name").value;
  const start = document.getElementById("start").value;
  const end = document.getElementById("end").value;

  if(!user || !start || !end){
    alert("املأ جميع المعلومات");
    return;
  }

  const booking = {user, car:selectedCar, start, end};

  let bookings = JSON.parse(localStorage.getItem("bookings")) || [];
  bookings.push(booking);
  localStorage.setItem("bookings", JSON.stringify(bookings));

  const msg = `حجز جديد:%0Aالاسم: ${user}%0Aالسيارة: ${selectedCar}%0Aمن: ${start}%0Aإلى: ${end}`;
  window.open(`https://wa.me/?text=${msg}`);

  alert("تم الحجز بنجاح ✅");
}

searchInput.addEventListener("input",()=>{
  const value = searchInput.value.toLowerCase();
  const filtered = cars.filter(c=>c.name.toLowerCase().includes(value));
  displayCars(filtered);
});

displayCars(cars);
</script></body>
</html>
