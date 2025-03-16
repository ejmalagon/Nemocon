<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reserva de Asientos - Viaje a Nemocón</title>
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
            width: 40px;
            height: 40px;
            margin: 5px;
            display: inline-block;
            border: 1px solid gray;
            text-align: center;
            line-height: 40px;
            cursor: pointer;
            background-color: lightgray;
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
                seat.onclick = function () {
                    let name = prompt("Ingresa tu nombre para reservar el asiento");
                    if (name) {
                        seat.innerText = name;
                        seat.classList.add("reserved");
                        seat.onclick = null; // Evita cambios posteriores
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
