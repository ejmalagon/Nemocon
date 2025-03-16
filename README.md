<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reserva de Asientos - Viaje a Nemocón</title>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
        }
        .bus {
            display: inline-block;
            margin: 20px;
            padding: 10px;
            border: 2px solid black;
        }
        .seat {
            width: 80px;
            height: 40px;
            margin: 5px;
            display: inline-block;
            border: 1px solid gray;
            text-align: center;
            line-height: 40px;
            cursor: pointer;
            background-color: lightgray;
            font-size: 12px;
            overflow: hidden;
            white-space: nowrap;
        }
        .reserved {
            background-color: red;
            color: white;
        }
        .images {
            margin-top: 20px;
        }
        .images img {
            width: 300px;
            margin: 10px;
        }
    </style>
</head>
<body>
    <h1>Reserva tu asiento para el viaje a Nemocón</h1>
    <p>Haz clic en un asiento para reservarlo</p>
    <div id="buses"></div>
    
    <div class="images">
        <h2>Imágenes de Nemocón</h2>
        <img src="imagen1.jpg" alt="Nemocón 1">
        <img src="imagen2.jpg" alt="Nemocón 2">
    </div>

    <script>
    const firebaseConfig = {
  apiKey: "AIzaSyAKP4w62Q3lPQYr30zzGf4rs3iF83uJCuc",
  authDomain: "buses-32e31.firebaseapp.com",
  databaseURL: "https://buses-32e31-default-rtdb.firebaseio.com",
  projectId: "buses-32e31",
  storageBucket: "buses-32e31.firebasestorage.app",
  messagingSenderId: "914612323057",
  appId: "1:914612323057:web:8dde205f394adcb9845f50",
  measurementId: "G-GT9Z1BVNFQ"
};
        
        firebase.initializeApp(firebaseConfig);
        const db = firebase.database();
        const busesContainer = document.getElementById("buses");
        const busCount = 2; // Número de buses disponibles
        const seatsPerBus = 40;
        
        function createBus(busNumber) {
            let busDiv = document.createElement("div");
            busDiv.classList.add("bus");
            let title = document.createElement("h3");
            title.innerText = `Bus ${busNumber}`;
            busDiv.appendChild(title);
            
            for (let i = 1; i <= seatsPerBus; i++) {
                let seat = document.createElement("div");
                seat.classList.add("seat");
                seat.innerText = i;
                
                db.ref(`bus${busNumber}/seat${i}`).on("value", snapshot => {
                    let data = snapshot.val();
                    if (data) {
                        seat.innerText = data;
                        seat.classList.add("reserved");
                    } else {
                        seat.innerText = i;
                        seat.classList.remove("reserved");
                    }
                });
                
                seat.onclick = function () {
                    if (seat.classList.contains("reserved")) {
                        let confirmDelete = confirm("¿Deseas liberar este asiento?");
                        if (confirmDelete) {
                            db.ref(`bus${busNumber}/seat${i}`).remove();
                        }
                    } else {
                        let name = prompt("Ingresa tu nombre para reservar el asiento");
                        if (name) {
                            db.ref(`bus${busNumber}/seat${i}`).set(name);
                        }
                    }
                };
                busDiv.appendChild(seat);
            }
            busesContainer.appendChild(busDiv);
        }
        
        for (let i = 1; i <= busCount; i++) {
            createBus(i);
        }
    </script>
</body>
</html>
