# mapa-interactivo
Mapa del territorio de Margaritas
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mapa Interactivo</title>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js"></script>
    <style>
        body { text-align: center; font-family: Arial, sans-serif; }
        #mapa-container { position: relative; display: inline-block; }
        #mapa { width: 90%; max-width: 800px; cursor: pointer; }
        .marcador {
            position: absolute;
            width: 20px; height: 20px;
            background-color: red;
            border-radius: 50%;
            transform: translate(-50%, -50%);
        }
    </style>
</head>
<body>
    <h2>Mapa Interactivo</h2>
    <div id="mapa-container">
        <img id="mapa" src="https://drive.google.com/file/d/1sCLRJPlAJ1-fuoABMRI_eJo5YXJmbnfc/view?usp=drive_link" alt="Mapa Interactivo">
    </div>

    <script>
        // Configurar Firebase
       const firebaseConfig = {
  apiKey: "AIzaSyCpCSFy621DXXmsORl8HffbFvDdaP0Y7XE",
  authDomain: "mapa-margaritas.firebaseapp.com",
  databaseURL: "https://mapa-margaritas-default-rtdb.firebaseio.com",
  projectId: "mapa-margaritas",
  storageBucket: "mapa-margaritas.firebasestorage.app",
  messagingSenderId: "474984702533",
  appId: "1:474984702533:web:7be228872db83b357db92c",
  measurementId: "G-TVM15DZW7W"
};
        firebase.initializeApp(firebaseConfig);
        const database = firebase.database();

        const mapa = document.getElementById("mapa");
        const mapaContainer = document.getElementById("mapa-container");

        // Función para agregar marcador
        function agregarMarcador(x, y) {
            const marcador = document.createElement("div");
            marcador.classList.add("marcador");
            marcador.style.left = `${x * 100}%`;
            marcador.style.top = `${y * 100}%`;
            mapaContainer.appendChild(marcador);
        }

        // Cargar marcadores desde Firebase
        function cargarMarcadores() {
            database.ref("marcadores").on("value", snapshot => {
                mapaContainer.querySelectorAll(".marcador").forEach(m => m.remove());
                snapshot.forEach(child => {
                    const { x, y } = child.val();
                    agregarMarcador(x, y);
                });
            });
        }

        // Evento de clic en el mapa
        mapa.addEventListener("click", event => {
            const rect = mapa.getBoundingClientRect();
            const x = (event.clientX - rect.left) / rect.width;
            const y = (event.clientY - rect.top) / rect.height;
            database.ref("marcadores").push({ x, y });
        });

        cargarMarcadores();
    </script>
</body>
</html>
