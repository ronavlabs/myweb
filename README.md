<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Ronav's Hub</title>

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Arial,sans-serif;
}

body{
background:#f4f6f9;
}

header{
background:linear-gradient(135deg,#2196F3,#673AB7);
color:white;
text-align:center;
padding:25px;
}

.page{
padding:20px;
}

.hidden{
display:none;
}

button{
padding:12px 18px;
border:none;
border-radius:10px;
cursor:pointer;
margin:5px;
color:white;
}

.blue{background:#2196F3;}
.green{background:#4CAF50;}
.orange{background:#FF9800;}
.red{background:#F44336;}
.purple{background:#9C27B0;}

input{
width:100%;
padding:12px;
margin:8px 0;
border:1px solid #ccc;
border-radius:8px;
}

.link-btn{
display:block;
width:320px;
margin:15px auto;
padding:18px;
background:white;
color:#333;
text-decoration:none;
font-size:20px;
font-weight:bold;
border-radius:15px;
box-shadow:0 4px 12px rgba(0,0,0,.2);
}

#peopleGrid{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
gap:20px;
margin-top:20px;
}

.card{
background:white;
border-radius:15px;
overflow:hidden;
box-shadow:0 2px 10px rgba(0,0,0,.2);
cursor:pointer;
}

.card img{
width:100%;
height:180px;
object-fit:cover;
}

.card h3{
padding:15px;
text-align:center;
}

</style>
</head>

<body>

<header>

<h1>🚀 Ronav's Hub</h1>

</header>

<div id="homePage" class="page">

<a class="link-btn" href="ballershub.html">
🏀 Ballers Hub
</a>

<a class="link-btn" href="footballershub.html">
⚽ Footballers Hub
</a>

<a class="link-btn" href="indianportal.html">
🇮🇳 Indian Portal
</a>

<button
class="purple"
onclick="showFavouritePeople()">
⭐ Favourite People
</button>

<a class="link-btn" href="mygame.html">
🎮 My Game
</a>

</div>

<div
id="favouritePage"
class="page hidden">

<h2>⭐ Favourite People</h2>

<input
type="text"
id="search"
placeholder="🔍 Search"
onkeyup="renderPeople()">

<button
class="blue"
onclick="showAddPage()">
➕ Add Person
</button>

<div id="peopleGrid"></div>

<button
class="orange"
onclick="goHome()">
🏠 Home
</button>

</div>

<div
id="addPage"
class="page hidden">

<h2>Add Person</h2>

<input
type="text"
id="name"
placeholder="Name">

<input
type="text"
id="address"
placeholder="Address">

<input
type="tel"
id="phone"
placeholder="Phone">

<input
type="email"
id="email"
placeholder="Email">

<input
type="file"
id="image"
accept="image/*">

<button
class="green"
onclick="savePerson()">
💾 Save
</button>

<button
class="red"
onclick="showFavouritePeople()">
❌ Cancel
</button>

</div>

<div
id="profilePage"
class="page hidden">
</div>

<script>

let people =
JSON.parse(
localStorage.getItem("people")
) || [];

function saveData(){

localStorage.setItem(
"people",
JSON.stringify(people)
);

}

function goHome(){

homePage.classList.remove("hidden");
favouritePage.classList.add("hidden");
addPage.classList.add("hidden");
profilePage.classList.add("hidden");

}

function showFavouritePeople(){

homePage.classList.add("hidden");
favouritePage.classList.remove("hidden");
addPage.classList.add("hidden");
profilePage.classList.add("hidden");

renderPeople();

}

function showAddPage(){

homePage.classList.add("hidden");
favouritePage.classList.add("hidden");
addPage.classList.remove("hidden");
profilePage.classList.add("hidden");

}
function savePerson(){

const name =
document.getElementById("name").value;

const address =
document.getElementById("address").value;

const phone =
document.getElementById("phone").value;

const email =
document.getElementById("email").value;

const image =
document.getElementById("image").files[0];

if(
!name ||
!address ||
!phone ||
!email ||
!image
){
alert("Please fill all fields.");
return;
}

const reader =
new FileReader();

reader.onload =
function(e){

people.push({

name:name,
address:address,
phone:phone,
email:email,
image:e.target.result

});

saveData();

document.getElementById("name").value="";
document.getElementById("address").value="";
document.getElementById("phone").value="";
document.getElementById("email").value="";
document.getElementById("image").value="";

showFavouritePeople();

};

reader.readAsDataURL(image);

}

function renderPeople(){

const grid =
document.getElementById("peopleGrid");

const search =
document.getElementById("search")
.value
.toLowerCase();

grid.innerHTML = "";

people.forEach((person,index)=>{

if(
!person.name
.toLowerCase()
.includes(search)
){
return;
}

grid.innerHTML += `

<div
class="card"
onclick="openProfile(${index})">

<img src="${person.image}">

<h3>${person.name}</h3>

</div>

`;

});

}

function openProfile(index){

const p =
people[index];

homePage.classList.add("hidden");

favouritePage.classList.add("hidden");

addPage.classList.add("hidden");

profilePage.classList.remove("hidden");

profilePage.innerHTML = `

<div class="card">

<img
src="${p.image}"
style="
width:100%;
height:300px;
object-fit:cover;
">

<div style="padding:20px;">

<h2>${p.name}</h2>

<p>
📍 ${p.address}
</p>

<br>

<p>
📞 ${p.phone}
</p>

<br>

<p>
📧 ${p.email}
</p>

<br>

<button
class="green"
onclick="openMaps(${index})">
📍 Maps
</button>

<button
class="orange"
onclick="callPerson('${p.phone}')">
📞 Call
</button>

<button
class="purple"
onclick="emailPerson('${p.email}')">
📧 Email
</button>

<button
class="blue"
onclick="editPerson(${index})">
✏️ Edit
</button>

<button
class="red"
onclick="deletePerson(${index})">
🗑 Delete
</button>

<button
class="blue"
onclick="showFavouritePeople()">
⬅ Back
</button>

</div>

</div>

`;

}
function openMaps(index){

window.open(
"https://www.google.com/maps/search/?api=1&query=" +
encodeURIComponent(
people[index].address
),
"_blank"
);

}

function callPerson(phone){

window.location.href =
"tel:" + phone;

}

function emailPerson(email){

window.location.href =
"mailto:" + email;

}

function editPerson(index){

let newName =
prompt(
"Edit Name",
people[index].name
);

if(newName === null) return;

let newAddress =
prompt(
"Edit Address",
people[index].address
);

if(newAddress === null) return;

let newPhone =
prompt(
"Edit Phone",
people[index].phone
);

if(newPhone === null) return;

let newEmail =
prompt(
"Edit Email",
people[index].email
);

if(newEmail === null) return;

people[index].name =
newName;

people[index].address =
newAddress;

people[index].phone =
newPhone;

people[index].email =
newEmail;

saveData();

openProfile(index);

}

function deletePerson(index){

if(
confirm(
"Delete this person?"
)
){

people.splice(index,1);

saveData();

showFavouritePeople();

}

}

renderPeople();

</script>

</body>
</html>    
}
