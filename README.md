<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reserva de Asientos - Campaña de predicación a Nemocón</title>
    <title>Sábado 3 de mayo</title>
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js";
        import { getDatabase, ref, onValue, set } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js";

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

        const app = initializeApp(firebaseConfig);
        const db = getDatabase(app);

        document.addEventListener("DOMContentLoaded", () => {
            const busesContainer = document.getElementById("buses");
            const busCount = 2;
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
                    
                    const seatRef = ref(db, `bus${busNumber}/seat${i}`);
                    onValue(seatRef, snapshot => {
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
                        if (!seat.classList.contains("reserved")) {
                            let name = prompt("Ingresa tu nombre para reservar el asiento");
                            if (name) {
                                set(seatRef, name);
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
        });
    </script>
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
    </style>
</head>
<body>
    <h1>Reserva tu asiento para la campaña a Nemocón</h1>
    <p>La campaña se realizará el sábado 3 de mayo, recuerda que debes llevar los alimentos que vas a consumir. Haz clic en un asiento para reservarlo.</p>
    <div id="buses"></div>
</body>
</html>
