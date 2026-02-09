<!DOCTYPE html>
<html lang="ms">
<head>
<meta charset="UTF-8">
<title>Kad Raya 3D FULL PRO</title>
<style>
body, html { margin:0; padding:0; overflow:hidden; font-family:'Segoe UI', sans-serif; background:#020811; }
#container { position:absolute; top:0; left:0; width:100%; height:100%; }
#ui { position:absolute; top:0; left:0; width:100%; text-align:center; z-index:10; color:#fff; }
input, textarea, button {
  width:80%; max-width:400px;
  padding:12px; margin:5px auto; border-radius:10px; border:none; font-size:16px; display:block;
}
button { background:#f5c542; font-weight:bold; cursor:pointer; }
#shareLink { background:#25D366; color:#fff; padding:10px 20px; border-radius:50px; text-decoration:none; display:inline-block; margin-top:10px; }
</style>
</head>
<body>

<div id="container"></div>

<div id="ui">
  <div id="form">
    <h2>🎉 Kad Raya 3D FULL PRO 🎉</h2>
    <input type="text" id="nama" placeholder="Nama penerima">
    <textarea id="ucapan" placeholder="Ucapan raya anda"></textarea>
    <button onclick="paparKad()">Siap & Papar Kad</button>
  </div>
  <div id="shareDiv" style="display:none;">
    <a id="shareLink" target="_blank">📲 Share WhatsApp</a>
  </div>
</div>

<!-- Three.js -->
<script src="https://cdn.jsdelivr.net/npm/three@0.158.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.158.0/examples/js/controls/OrbitControls.js"></script>
<!-- GSAP -->
<script src="https://cdn.jsdelivr.net/npm/gsap@3.15.0/dist/gsap.min.js"></script>

<script>
// Scene setup
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(60, window.innerWidth/window.innerHeight, 0.1, 1000);
camera.position.z = 6;

const renderer = new THREE.WebGLRenderer({antialias:true, alpha:true});
renderer.setSize(window.innerWidth, window.innerHeight);
document.getElementById('container').appendChild(renderer.domElement);

// Controls
const controls = new THREE.OrbitControls(camera, renderer.domElement);
controls.enableZoom = false;

// Lighting
const ambient = new THREE.AmbientLight(0xffffff, 0.8);
scene.add(ambient);
const dirLight = new THREE.DirectionalLight(0xffffff, 0.5);
dirLight.position.set(5,5,5);
scene.add(dirLight);

// Masjid 3D (simple placeholder, upgrade boleh pakai GLTF loader)
const masjidGeo = new THREE.BoxGeometry(3,1.5,3);
const masjidMat = new THREE.MeshStandardMaterial({color:0xffd700});
const masjid = new THREE.Mesh(masjidGeo, masjidMat);
masjid.position.y = -1.2;
scene.add(masjid);

// Particles / ketupat
const ketupatGroup = new THREE.Group();
for(let i=0;i<20;i++){
  const geo = new THREE.OctahedronGeometry(0.1,0);
  const mat = new THREE.MeshStandardMaterial({color:0x00ff00});
  const ketupat = new THREE.Mesh(geo, mat);
  ketupat.position.set(Math.random()*6-3, Math.random()*3, Math.random()*2-1);
  ketupat.rotation.set(Math.random()*Math.PI, Math.random()*Math.PI, Math.random()*Math.PI);
  ketupatGroup.add(ketupat);
}
scene.add(ketupatGroup);

// Particles light / glitter
const particleGeo = new THREE.SphereGeometry(0.03,6,6);
for(let i=0;i<50;i++){
  const mat = new THREE.MeshStandardMaterial({color:0xfff8c5});
  const p = new THREE.Mesh(particleGeo, mat);
  p.position.set(Math.random()*6-3, Math.random()*4-1, Math.random()*2-1);
  scene.add(p);
}

// Animate
function animate(){
  requestAnimationFrame(animate);
  ketupatGroup.children.forEach(k=>{
    k.rotation.x += 0.01;
    k.rotation.y += 0.01;
    k.position.y -= 0.005;
    if(k.position.y < -2) k.position.y = 3;
  });
  renderer.render(scene, camera);
}
animate();

// Resize
window.addEventListener('resize', ()=>{
  camera.aspect = window.innerWidth/window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});

// Kad function
function paparKad(){
  const nama = document.getElementById('nama').value || "Sahabatku";
  const ucapan = document.getElementById('ucapan').value || "Semoga Aidilfitri membawa keberkatan & kebahagiaan.";

  // Typewriter effect
  const textDiv = document.createElement('div');
  textDiv.style.position='absolute'; textDiv.style.top='50px'; textDiv.style.width='100%';
  textDiv.style.textAlign='center'; textDiv.style.fontSize='28px'; textDiv.style.color='#ffd700';
  document.body.appendChild(textDiv);

  const fullText = `Salam Aidilfitri\n${nama}\n${ucapan}`;
  let i=0;
  function typeWriter(){
    if(i<fullText.length){
      textDiv.innerHTML += fullText.charAt(i)=='\n'?'<br>':fullText.charAt(i);
      i++;
      setTimeout(typeWriter,50);
    }
  }
  typeWriter();

  document.getElementById('form').style.display='none';
  document.getElementById('shareDiv').style.display='block';

  const text = encodeURIComponent(`🎉 Salam Aidilfitri 🎉\n\n${nama}\n${ucapan}\n\nLink kad: ${window.location.href}`);
  document.getElementById('shareLink').href=`https://wa.me/?text=${text}`;

  // Background music
  const audio = document.createElement('audio');
  audio.src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_9e6db1c1c5.mp3"; // instrumental royalty-free
  audio.loop=true; audio.autoplay=true; audio.controls=true;
  document.body.appendChild(audio);
}
</script>

</body>
</html>
