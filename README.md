<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Om Singh | Full Stack Developer</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r134/three.min.js"></script>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:Inter,Arial}
body{background:#050505;color:#fff;overflow-x:hidden}
#loader{position:fixed;width:100%;height:100%;background:black;display:flex;justify-content:center;align-items:center;z-index:9999}
.spinner{width:60px;height:60px;border:5px solid #0ff;border-top:transparent;border-radius:50%;animation:spin 1s linear infinite}
@keyframes spin{100%{transform:rotate(360deg)}}
.cursor{position:fixed;width:140px;height:140px;background:radial-gradient(circle,#0ff55,transparent);pointer-events:none;transform:translate(-50%,-50%);z-index:999}
header{position:sticky;top:0;background:#0009;padding:1rem 2rem;display:flex;justify-content:space-between;align-items:center}
header h2{color:#0ff}
nav a{color:white;margin-left:20px;text-decoration:none}
.hero{height:100vh;display:flex;justify-content:center;align-items:center;text-align:center;position:relative}
.typing{border-right:2px solid #0ff;white-space:nowrap;overflow:hidden}
canvas#three{position:absolute;top:0;left:0;z-index:-2}
.section{padding:4rem 2rem;text-align:center}
.projects{display:flex;gap:20px;flex-wrap:wrap;justify-content:center}
.card{padding:20px;width:260px;background:#0ff1;border:1px solid #0ff;border-radius:10px}
button{margin-top:10px;padding:10px;border:1px solid #0ff;background:transparent;color:#0ff;cursor:pointer}
</style>
</head>
<body>

<div id="loader"><div class="spinner"></div></div>
<div class="cursor" id="cursor"></div>

<header>
<h2>Om Singh</h2>
<nav>
<a href="#projects">Projects</a>
<a href="#contact">Contact</a>
</nav>
</header>

<section class="hero">
<canvas id="three"></canvas>
<div>
<h1 id="typing" class="typing"></h1>
<p>Full Stack Developer</p>
</div>
</section>

<section id="projects" class="section">
<h2>Real Projects (GitHub)</h2>
<div class="projects" id="projectsContainer"></div>
</section>

<section id="contact" class="section">
<h2>Contact Me</h2>
<form id="contactForm">
<input type="text" id="name" placeholder="Name" required><br>
<input type="email" id="email" placeholder="Email" required><br>
<textarea id="message" placeholder="Message" required></textarea><br>
<button type="submit">Send</button>
</form>
</section>

<script>
// LOADER
window.onload=()=>document.getElementById("loader").style.display="none";

// CURSOR
const cursor=document.getElementById("cursor");
document.addEventListener("mousemove",e=>{
cursor.style.left=e.clientX+"px";
cursor.style.top=e.clientY+"px";
});

// TYPING
const text="Hi, I'm Om Singh | Full Stack Developer";
let i=0;
function type(){
if(i<text.length){document.getElementById("typing").innerHTML+=text[i++];setTimeout(type,40)}
}
type();

// THREE JS
const scene=new THREE.Scene();
const camera=new THREE.PerspectiveCamera(75,window.innerWidth/window.innerHeight,0.1,1000);
const renderer=new THREE.WebGLRenderer({canvas:document.getElementById('three'),alpha:true});
renderer.setSize(window.innerWidth,window.innerHeight);
const geometry=new THREE.TorusKnotGeometry(10,3,100,16);
const material=new THREE.MeshBasicMaterial({color:0x00ffff,wireframe:true});
const mesh=new THREE.Mesh(geometry,material);
scene.add(mesh);
camera.position.z=30;
function animate(){requestAnimationFrame(animate);mesh.rotation.x+=0.01;mesh.rotation.y+=0.01;renderer.render(scene,camera);}animate();

// GITHUB PROJECTS (REAL DATA)
fetch("https://api.github.com/users/yourusername/repos")
.then(res=>res.json())
.then(data=>{
const container=document.getElementById("projectsContainer");
data.slice(0,6).forEach(repo=>{
const div=document.createElement("div");
div.className="card";
div.innerHTML=`<h3>${repo.name}</h3><p>${repo.description||"No description"}</p><a href="${repo.html_url}" target="_blank">View</a>`;
container.appendChild(div);
});
});

// CONTACT BACKEND (FORM API)
document.getElementById("contactForm").addEventListener("submit",function(e){
e.preventDefault();
fetch("https://formspree.io/f/yourid",{
method:"POST",
headers:{"Content-Type":"application/json"},
body:JSON.stringify({
name:document.getElementById("name").value,
email:document.getElementById("email").value,
message:document.getElementById("message").value
})
}).then(()=>alert("Message sent successfully"));
});

// TESTS
console.assert(document.getElementById("projectsContainer"),"Projects container exists");
</script>

</body>
</html>
