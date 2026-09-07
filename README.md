<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Share Current Location</title>
  <style>
    body {
      margin: 0;
      min-height: 100vh;
      display: grid;
      place-items: center;
      font-family: Arial, sans-serif;
      background: #f5f5f5;
    }
    .card {
      width: min(90%, 420px);
      padding: 28px;
      text-align: center;
      background: white;
      border-radius: 18px;
      box-shadow: 0 8px 30px rgba(0,0,0,.1);
    }
    button {
      border: 0;
      border-radius: 12px;
      padding: 14px 20px;
      font-size: 16px;
      cursor: pointer;
      background: #25D366;
      color: white;
    }
    #status { margin: 16px 0; color: #555; min-height: 24px; }
  </style>
</head>
<body>
  <main class="card">
    <h2>📍 Share Your Location</h2>
    <p id="status">Click the button and allow location access.</p>
    <button id="shareBtn">Share Current Location on WhatsApp</button>
  </main>

  <script>
    const WHATSAPP_NUMBER = "919241296570";

    document.getElementById("shareBtn").addEventListener("click", () => {
      const status = document.getElementById("status");

      if (!navigator.geolocation) {
        status.textContent = "Geolocation is not supported by this browser.";
        return;
      }

      status.textContent = "Requesting your current location...";

      navigator.geolocation.getCurrentPosition(
        (position) => {
          const { latitude, longitude, accuracy } = position.coords;

          const mapLink =
            `https://www.google.com/maps?q=${latitude},${longitude}`;

          const message =
            `📍 My current location:\n${mapLink}\n\n` +
            `Accuracy: approximately ${Math.round(accuracy)} meters.`;

          const whatsappURL =
            `https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(message)}`;

          status.textContent = "Opening WhatsApp...";
          window.location.href = whatsappURL;
        },
        (error) => {
          const messages = {
            1: "Location permission was denied. Please allow location access and try again.",
            2: "Your location could not be determined. Please try again.",
            3: "Location request timed out. Please try again."
          };
          status.textContent = messages[error.code] || "Unable to get your location.";
        },
        {
          enableHighAccuracy: true,
          timeout: 15000,
          maximumAge: 0
        }
      );
    });
