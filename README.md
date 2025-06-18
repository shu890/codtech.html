<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Responsive Weather App</title>
  <style>
    * {
      box-sizing: border-box;
    }

    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      margin: 0;
      padding: 0;
      background: linear-gradient(to right, #83a4d4, #b6fbff);
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }

    .container {
      background: white;
      padding: 2rem;
      border-radius: 10px;
      box-shadow: 0 8px 16px rgba(0,0,0,0.15);
      width: 100%;
      max-width: 400px;
      text-align: center;
    }

    input, button {
      padding: 10px;
      margin: 0.5rem 0;
      width: 100%;
      font-size: 1rem;
      border: 1px solid #ccc;
      border-radius: 5px;
    }

    button {
      background-color: #007BFF;
      color: white;
      cursor: pointer;
      transition: background 0.3s ease;
    }

    button:hover {
      background-color: #0056b3;
    }

    .weather-info {
      margin-top: 1.5rem;
    }

    .weather-info h2 {
      margin-bottom: 0.5rem;
    }

    .weather-icon {
      width: 60px;
      height: 60px;
    }

    @media (max-width: 480px) {
      .container {
        padding: 1rem;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Weather App</h1>
    <input type="text" id="cityInput" placeholder="Enter city name" />
    <button onclick="getWeather()">Get Weather</button>
    <div id="weather" class="weather-info"></div>
  </div>

  <script>
    const apiKey = 'YOUR_API_KEY_HERE'; // <-- Replace with your actual API key

    async function getWeather() {
      const city = document.getElementById('cityInput').value.trim();
      const output = document.getElementById('weather');

      if (!city) {
        output.innerHTML = '<p>Please enter a city name.</p>';
        return;
      }

      output.innerHTML = '<p>Loading...</p>';

      const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${encodeURIComponent(city)}&units=metric&appid=${apiKey}`;

      try {
        const response = await fetch(apiUrl);
        const data = await response.json();

        if (!response.ok) {
          output.innerHTML = `<p>Error: ${data.message}</p>`;
          return;
        }

        const iconUrl = `https://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;

        output.innerHTML = `
          <h2>${data.name}, ${data.sys.country}</h2>
          <img class="weather-icon" src="${iconUrl}" alt="${data.weather[0].description}">
          <p><strong>${data.weather[0].main}</strong> - ${data.weather[0].description}</p>
          <p>🌡️ Temp: ${data.main.temp} °C</p>
          <p>💨 Wind: ${data.wind.speed} m/s</p>
          <p>💧 Humidity: ${data.main.humidity}%</p>
        `;
      } catch (error) {
        output.innerHTML = `<p>Failed to fetch weather data.</p>`;
      }
    }
  </script>
</body>
</html>
