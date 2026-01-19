# hsd<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>HagraSocial</title>
  <style>
    /* Dégradé de fond rose → violet */
    body {
      font-family: 'Arial', sans-serif;
      margin: 0;
      padding: 0;
      min-height: 100vh;
      background: linear-gradient(135deg, #ff9ec6, #a84cff);
      display: flex;
      justify-content: center;
      align-items: center;
    }

    /* Conteneur principal */
    .container {
      background: rgba(255, 255, 255, 0.95);
      padding: 40px 30px;
      border-radius: 25px;
      width: 420px;
      box-shadow: 0 15px 30px rgba(0,0,0,0.2);
      transition: transform 0.3s;
    }

    .container:hover {
      transform: scale(1.02);
    }

    h1, h2 {
      text-align: center;
      color: #a84cff;
      margin: 0 0 20px 0;
    }

    input, button, textarea {
      width: 100%;
      padding: 12px;
      margin: 10px 0;
      border-radius: 12px;
      border: 1px solid #ccc;
      font-size: 16px;
      transition: 0.3s;
    }

    input:focus, textarea:focus {
      border-color: #a84cff;
      outline: none;
    }

    button {
      background: #a84cff;
      color: white;
      border: none;
      cursor: pointer;
      font-weight: bold;
      transition: 0.3s;
    }

    button:hover {
      background: #8a2aff;
    }

    .post {
      background: linear-gradient(120deg, #ffe6f0, #f2ccff);
      padding: 15px;
      margin-top: 15px;
      border-radius: 15px;
      box-shadow: 0 6px 12px rgba(0,0,0,0.1);
      word-wrap: break-word;
    }

    /* Barre du nom utilisateur */
    #network h2 span {
      color: #ff69b4;
    }
  </style>
</head>
<body>

<div class="container">

  <h1>HagraSocial</h1>

  <!-- Code d'accès -->
  <div id="access">
    <h2>Code d'accès</h2>
    <input id="accessCode" placeholder="Entre le code">
    <button onclick="checkAccess()">Valider</button>
  </div>

  <!-- Inscription -->
  <div id="register" style="display:none;">
    <h2>S'inscrire</h2>
    <input id="regUser" placeholder="Pseudo">
    <input id="regPass" type="password" placeholder="Mot de passe">
    <input id="regAge" type="number" placeholder="Âge" min="1">
    <button onclick="register()">S'inscrire</button>
  </div>

  <!-- Connexion -->
  <div id="login" style="display:none;">
    <h2>Connexion</h2>
    <input id="loginUser" placeholder="Pseudo">
    <input id="loginPass" type="password" placeholder="Mot de passe">
    <button onclick="login()">Se connecter</button>
  </div>

  <!-- Réseau social -->
  <div id="network" style="display:none;">
    <h2>Bienvenue, <span id="currentUser"></span></h2>
    <p>Personnes en ligne : <span id="activeCount">0</span></p>
    <textarea id="postText" placeholder="Écris un post..."></textarea>
    <button onclick="publish()">Publier</button>
    <div id="posts"></div>
    <button onclick="logout()">Se déconnecter</button>
  </div>

</div>

<script>
let users = [];
let posts = [];
let currentUser = null;
let activeUsers = 0; // Nombre d'utilisateurs connectés

// Met à jour l'affichage du nombre de personnes actives
function updateActiveCount() {
  document.getElementById("activeCount").innerText = activeUsers;
}

// Code d'accès
const ACCESS_CODE = "778899";
function checkAccess() {
  const code = document.getElementById("accessCode").value;
  if(code === ACCESS_CODE) {
    document.getElementById("access").style.display = "none";
    document.getElementById("register").style.display = "block";
  } else {
    alert("Code incorrect !");
  }
}

// Inscription
function register() {
  const user = document.getElementById("regUser").value;
  const pass = document.getElementById("regPass").value;
  const age = parseInt(document.getElementById("regAge").value);

  if(!user || !pass || !age) { 
    alert("Remplis tout !"); 
    return; 
  }

  if(age < 13) {
    alert("Désolé, tu dois avoir au moins 13 ans pour t'inscrire !");
    return;
  }

  if(users.find(u => u.user === user)) { 
    alert("Pseudo déjà pris !"); 
    return; 
  }

  users.push({user, pass, age});
  alert("Inscription réussie ! Connecte-toi.");
  document.getElementById("register").style.display = "none";
  document.getElementById("login").style.display = "block";
}

// Connexion
function login() {
  const user = document.getElementById("loginUser").value;
  const pass = document.getElementById("loginPass").value;
  const found = users.find(u => u.user === user && u.pass === pass);
  if(!found) { alert("Pseudo ou mot de passe incorrect !"); return; }
  currentUser = user;
  activeUsers++; // Incrémente le nombre d'utilisateurs actifs
  updateActiveCount(); // Met à jour l'affichage
  document.getElementById("currentUser").innerText = currentUser;
  document.getElementById("login").style.display = "none";
  document.getElementById("network").style.display = "block";
  displayPosts();
}

// Publier un post
function publish() {
  const text = document.getElementById("postText").value;
  if(!text) return;
  posts.unshift({user: currentUser, text});
  document.getElementById("postText").value = "";
  displayPosts();
}

// Afficher les posts
function displayPosts() {
  const container = document.getElementById("posts");
  container.innerHTML = "";
  posts.forEach(p => {
    const div = document.createElement("div");
    div.className = "post";
    div.innerHTML = `<strong>${p.user}</strong><br>${p.text}`;
    container.appendChild(div);
  });
}

// Déconnexion
function logout() {
  currentUser = null;
  activeUsers = Math.max(activeUsers - 1, 0); // Décrémente mais jamais en dessous de 0
  updateActiveCount();
  document.getElementById("network").style.display = "none";
  document.getElementById("login").style.display = "block";
}
</script>

</body>
</html>
