# Clima <!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Clima y Hora Actual</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <div class="datetime">
            <h2 id="time">Cargando...</h2>
            <p id="date">Cargando...</p>
        </div>
        <div class="weather">
            <h3 id="city">Cargando...</h3>
            <p id="temperature">Cargando...</p>
            <p id="description">Cargando...</p>
            <img id="weather-icon" src="" alt="Clima">
        </div>
    </div>

    <script src="script.js"></script>
</body>
</html>
body {
    font-family: sans-serif;
    margin: 0;
    background-color: #f0f0f0;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
}

.container {
    background-color: #fff;
    padding: 30px;
    border-radius: 8px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    text-align: center;
}

.datetime h2 {
    font-size: 2.5em;
    margin-bottom: 5px;
    color: #333;
}

.datetime p {
    font-size: 1.1em;
    color: #777;
}

.weather {
    margin-top: 20px;
}

.weather h3 {
    font-size: 1.8em;
    color: #333;
    margin-bottom: 5px;
}

.weather p {
    font-size: 1.2em;
    color: #555;
    margin-bottom: 8px;
}

.weather img {
    width: 70px;
    height: 70px;
    margin-top: 10px;
}
function updateTime() {
    const now = new Date();
    const hours = String(now.getHours()).padStart(2, '0');
    const minutes = String(now.getMinutes()).padStart(2, '0');
    const seconds = String(now.getSeconds()).padStart(2, '0');
    const day = String(now.getDate()).padStart(2, '0');
    const month = String(now.getMonth() + 1).padStart(2, '0');
    const year = now.getFullYear();

    document.getElementById('time').textContent = `${hours}:${minutes}:${seconds}`;
    document.getElementById('date').textContent = `${day}/${month}/${year}`;
}

function getWeather() {
    // ¡IMPORTANTE! Reemplaza 'TU_API_KEY' con tu propia API key de OpenWeatherMap
    const apiKey = 'TU_API_KEY';
    // Coordenadas de Sicuani, Cusco, Perú
    const latitude = -14.2703;
    const longitude = -71.2640;

    const apiUrl = `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=${apiKey}&units=metric&lang=es`;

    fetch(apiUrl)
        .then(response => {
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            return response.json();
        })
        .then(data => {
            document.getElementById('city').textContent = data.name;
            document.getElementById('temperature').textContent = `${Math.round(data.main.temp)}°C`;
            document.getElementById('description').textContent = data.weather[0].description;
            const iconCode = data.weather[0].icon;
            document.getElementById('weather-icon').src = `https://openweathermap.org/img/wn/${iconCode}@2x.png`;
        })
        .catch(error => {
            console.error('Error fetching weather:', error);
            document.getElementById('city').textContent = 'Error al cargar el clima';
            document.getElementById('temperature').textContent = '';
            document.getElementById('description').textContent = '';
            document.getElementById('weather-icon').src = '';
        });
}

// Actualizar la hora cada segundo
setInterval(updateTime, 1000);

// Obtener el clima al cargar la página y luego actualizarlo periódicamente (opcional)
getWeather();
// setInterval(getWeather, 3600000); // Actualizar cada hora (en milisegundos)
 
