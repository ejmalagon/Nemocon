<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reserva de Asientos - Viaje a Nemocón</title>
    <script type="module">
        // Importar Firebase versión 9
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js";
        import { getDatabase, ref, onValue, set, remove } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js";

        // Configuración de Firebase (reemplaza con tus datos)
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

        // Inicializar Firebase
        const app = initializeApp(firebaseConfig);
        const db = getDatabase(app);

        document.addEventListener("DOMContentLoaded", function() {
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

                    // Referencia al asiento en la base de datos
                    let seatRef = ref(db, `bus${busNumber}/seat${i}`);

                    // Escuchar cambios en tiempo real
                    onValue(seatRef, (snapshot) => {
                        let data = snapshot.val();
                        if (data) {
                            seat.innerText = data;
                            seat.classList.add("reserved");
                        } else {
                            seat.innerText = i;
                            seat.classList.remove("reserved");
                        }
                    });

                    // Evento de reserva/liberación de asiento
                    seat.onclick = function () {
                        if (seat.classList.contains("reserved")) {
                            let confirmDelete = confirm("¿Deseas liberar este asiento?");
                            if (confirmDelete) {
                                remove(seatRef);
                            }
                        } else {
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
            width: 100px;
            height: 50px;
            margin: 5px;
            display: inline-block;
            border: 1px solid gray;
            text-align: center;
            line-height: 50px;
            cursor: pointer;
            background-color: lightgray;
            font-size: 14px;
            overflow: hidden;
            white-space: nowrap;
        }
        .reserved {
            background-color: red;
            c
