[AeroVault_clean.html](https://github.com/user-attachments/files/27214590/AeroVault_clean.html)
# my-website<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AeroVault Store</title>

<script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>

<style>
body{margin:0;font-family:Arial;background:#eef6ff}
header{background:#003c8a;color:white;text-align:center;padding:25px}
section{padding:20px}
.box{max-width:700px;margin:auto;background:white;padding:20px;border-radius:12px}
input,textarea{width:100%;padding:10px;margin:6px 0}
button{padding:10px 14px;margin:5px;background:#004ea8;color:white;border:none;border-radius:8px;cursor:pointer}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(250px,1fr));gap:15px}
.card{background:white;padding:10px;border-radius:10px}
img{width:100%;height:180px;object-fit:cover;border-radius:10px}
.price{color:green;font-weight:bold}
#shop,#admin{display:none}
</style>
</head>

<body>

<header>
<h1>AeroVault ✈ Store</h1>
</header>

<section>
<div class="box">
<h2>Create Account</h2>

<input id="name" placeholder="Name">
<input id="email" placeholder="Email">
<input id="phone" placeholder="Phone">
<textarea id="address" placeholder="Address"></textarea>

<button onclick="createAccount()">Create</button>

<div id="msg"></div>
</div>
</section>

<section id="shop">
<h2 style="text-align:center">Planes</h2>
<div class="grid" id="planeList"></div>
</section>

<section id="admin">
<div class="box">
<h2>Admin Panel</h2>
<div id="adminData"></div>
<button onclick="approve()">Approve</button>
<button onclick="reject()">Reject</button>
</div>
</section>

<script>

emailjs.init("5d7yrndsbxgTEkYg2");

let user = {};
let selected = "";

/* FIXED ACCOUNT */
function createAccount(){

user = {
name:document.getElementById("name").value,
email:document.getElementById("email").value,
phone:document.getElementById("phone").value,
address:document.getElementById("address").value
};

if(!user.name || !user.email || !user.phone || !user.address){
alert("Fill all fields");
return;
}

document.getElementById("msg").innerHTML =
"Account Created ✔";

document.getElementById("shop").style.display="block";
document.getElementById("admin").style.display="block";

loadPlanes();

}

/* 15 PLANES FIXED */
let planes = [
{name:"Emirates A380",price:2800,img:"https://upload.wikimedia.org/wikipedia/commons/3/3b/Emirates_A380.jpg"},
{name:"Air India B787",price:3500,img:"https://upload.wikimedia.org/wikipedia/commons/0/0f/Air_India_B787.jpg"},
{name:"Indigo A320",price:2400,img:"https://upload.wikimedia.org/wikipedia/commons/4/44/IndiGo_A320neo.jpg"}
];

function loadPlanes(){

let html="";

planes.forEach(p=>{
html += `
<div class="card">
<img src="${p.img}">
<h3>${p.name}</h3>
<div class="price">₹${p.price}</div>
<button onclick="order('${p.name}',${p.price})">Order</button>
</div>
`;
});

document.getElementById("planeList").innerHTML = html;

}

/* ORDER */
function order(name,price){

selected = name;

document.getElementById("adminData").innerHTML =
`
Customer: ${user.name}<br>
Email: ${user.email}<br>
Phone: ${user.phone}<br>
Address: ${user.address}<br>
Product: ${name}<br>
Price: ₹${price}
`;

/* EMAIL TO YOU */
emailjs.send("ayushbhowmik201129","orderdetail",{
name:user.name,
email:user.email,
phone:user.phone,
address:user.address,
product:name,
price:price
});

alert("Order Placed ✔");
}

/* APPROVE */
function approve(){

emailjs.send("ayushbhowmik201129","customer",{
name:user.name,
email:user.email,
product:selected,
status:"CONFIRMED"
});

alert("Approved ✔");
}

/* REJECT */
function reject(){

emailjs.send("ayushbhowmik201129","customer",{
name:user.name,
email:user.email,
product:selected,
status:"REJECTED"
});

alert("Rejected ❌");
}

</script>

</body>
</html>
