<!DOCTYPE html>
<html>
<head>
  <title>Obter Localização</title>
  <script>
    function getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(showPosition, showError);
      } else {
        alert("Geolocalização não é suportada por este navegador.");
      }
    }

    function showPosition(position) {
      const latitude = position.coords.latitude;
      const longitude = position.coords.longitude;

      document.getElementById("location").innerHTML = 
        "Latitude: " + latitude + "<br>Longitude: " + longitude;

      fetch('https://SEU-SERVIDOR-AQUI.com/receber_localizacao', {
        method: 'POST',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify({ latitude: latitude, longitude: longitude })
      })
      .then(response => response.json())
      .then(data => console.log(data))
      .catch(error => console.error('Erro:', error));
    }

    function showError(error) {
      switch (error.code) {
        case error.PERMISSION_DENIED:
          alert("Usuário negou a solicitação de Geolocalização.");
          break;
        case error.POSITION_UNAVAILABLE:
          alert("Informações de localização estão indisponíveis.");
          break;
        case error.TIMEOUT:
          alert("A requisição para obter a localização expirou.");
          break;
        case error.UNKNOWN_ERROR:
          alert("Um erro desconhecido ocorreu.");
          break;
      }
    }
  </script>
</head>
<body>
  <h2>Clique no botão abaixo para permitir o acesso à sua localização:</h2>
  <button onclick="getLocation()">Obter Localização</button>
  <p id="location"></p>
</body>
</html>
