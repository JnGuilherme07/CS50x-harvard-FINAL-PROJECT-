# Ecoverse
#### Video Demo: [https://youtu.be/TutAP_Qllqc](https://youtu.be/TutAP_Qllqc)
#### Description:

Ecoverse is a web-based project developed as part of the CS50x course, designed to raise awareness about air quality and environmental health in Brazil. The platform allows users to select a Brazilian state and instantly generate an air quality report using real-time or simulated environmental data. The goal is to inform the population about pollution levels and demonstrate how artificial intelligence and web technologies can be used to monitor environmental conditions effectively.

---

### Project Overview

The project consists of two main pages:

1. **index.html** – The home page introduces the Ecoverse initiative, presenting its purpose and inviting users to explore the air quality monitoring tool.
2. **projeto.html** – The project page explains the environmental goal behind Ecoverse and includes a dynamic report generator that allows users to visualize air quality data by state.

Both pages are styled for a modern, eco-friendly interface using **CSS3**, and the functionality is powered by **JavaScript (gerador.js)**, which interacts with external APIs when available.

---

### How It Works

- The user selects a state (e.g., São Paulo, Rio de Janeiro, Minas Gerais, Distrito Federal, Paraná) from a dropdown menu.
- Upon clicking the “Generate Report” button, a JavaScript script fetches real environmental data from the **OpenAQ API**.  
- If real-time data is not available for the selected state, the system generates simulated values (air quality index, main pollutant, pollution percentage) to ensure full functionality.
- The report is displayed dynamically with a clear visual presentation, including a description of the selected location’s air quality level.

The platform is fully responsive and optimized for both desktop and mobile devices, ensuring accessibility and an engaging user experience.

---

### File Structure and Contents

- **index.html**  
  Main landing page containing the general presentation of Ecoverse, navigation buttons, and a link to the project page.

- **projeto.html**  
  Dedicated page for generating and displaying air quality reports. Includes a detailed explanation of the project and its purpose.

- **style.css / styleProjeto.css**  
  These files define the overall visual identity of the project.  
  The design uses a clean, nature-inspired palette with green tones, rounded elements, and soft animations.

- **gerador.js**  
  The JavaScript logic behind the report generator.  
  It handles user interactions, fetches or simulates air quality data, and updates the interface dynamically.

- **imgs/**  
  Folder containing visual assets like the Ecoverse logo and icons used throughout the interface.

---

### Technologies Used

- **HTML5** – for semantic and accessible structure  
- **CSS3** – for styling and responsive layout  
- **JavaScript (ES6)** – for dynamic content generation  
- **OpenAQ API** – for accessing real environmental data (via user-provided API token)

---

## Explaining Code

- **INDEX.html** -

```<header>
  <a href="index-en.html" class="logo">
    <img src="imgs/Group 1.png" alt="Ecoverse Logo">
  </a>

  <i class="bx bx-menu" id="menu-icon"></i>

  <div class="header-icon">
    <i class="bx bx-cart-alt"></i>
    <i class="bx bx-search" id="search-icon"></i>
  </div>

  <div class="search-box">
    <input type="search" name="" id="" placeholder="Search here...">
  </div>

  <a href="index.html" class="translate-btn">
    <img src="imgs/brazil-flag.png" alt="Portuguese" class="flag-icon"> PT
  </a>
</header>
```
Clickable logo.
Menu, cart, and search icons.
Button to change language to Portuguese.

```<section class="home" id="home">
  <div class="home-text">
    <h1>Welcome to <br>Ecoverse</h1>
    <p>At Ecoverse, we believe that innovation and sustainability should go hand in hand. 
       We develop technological solutions to transform the way the world interacts with the environment.</p>
    <a href="projeto-en.html" class="btn">Discover the Project</a>
  </div>

  <div class="hero-img">
    <div class="hero-circle"></div>
    <img src="imgs/logo_environmental_smooth 1.png" alt="Sustainability Icon">
  </div>
</section>
```
Welcome text and company mission.
Button to learn about the project.

- **PROJETO.html** -

```<section class="projeto">
  <div class="descricao">
    <h1>About the Project</h1>
    <p>...</p>
  </div>

  <div class="gerador">
    <h2>Generate Air Quality Report</h2>
    <select id="estado">...</select>
    <button id="gerar">Generate Report</button>
    <div id="relatorio"></div>
  </div>
</section>
```
Generator block:
-Dropdown menu to select the status.
-Button to generate the report.
-Area where the report will be displayed dynamically.

- **GERADOR.js** -

```const API_TOKEN = "e062e063a0aa1a65708306581d969c011a8519a6";```
Access key for the AQICN (Air Quality Index) API.

```document.addEventListener("DOMContentLoaded", () => {
    const gerarBtn = document.getElementById("gerar");
    const relatorioDiv = document.getElementById("relatorio");
```
Wait for the page to load to activate the scripts.
Capture the button and the area where the report will be displayed.

-Report generation logic

```gerarBtn.addEventListener("click", async () => {
  const estado = document.getElementById("estado").value;

  if (!estado) {
    relatorioDiv.innerHTML = "<p style='color:red'>Please select a state.</p>";
    return;
  }
```
Check if the state has been selected.
If not, display an error message.

```const mapEstadoParaCidade = {
  "RJ": "sao-paulo",
  "SP": "rio-de-janeiro",
  "MG": "belo-horizonte",
  "DF": "brasilia",
  "PR": "curitiba"
};
```
Maps the selected state to a city

```const url = `https://api.waqi.info/feed/${cidade}/?token=${API_TOKEN}`; ```

Build the request URL.
${cidade} is the name of the city (such as "curitiba") that was selected based on the state.
${API_TOKEN} is the API access key.

```
const resp = await fetch(url);
const data = await resp.json();
```
Makes a request to the API using **fetch** and waits for the response with await.

fetch(url) makes the HTTP request to the API URL.
await waits for the API response before continuing the code.The response (resp) is still in raw format — it's not the final content, just the data package.

Convert the response (resp) to JSON, which is the data format that the API returns.
Now, data contains the actual data that you can use, such as:data.data.aqi → air quality index (AQI)data.data.dominentpol → dominant pollutant

```
const aqi = data.data.aqi;
const dominantes = data.data.dominentpol || "N/A";

const airQuality = (() => {
  if (aqi <= 50) return "Good";
  if (aqi <= 100) return "Moderate";
  if (aqi <= 150) return "Unhealthy for Sensitive Groups";
  if (aqi <= 200) return "Unhealthy";
  return "Very Unhealthy";
})();
```
Extracts the Air Quality Index (AQI).
Defines the classification based on the value.

```const reportText = `
  <h3>Air Quality Report - ${estado}</h3>
  <p><b>AQI Index:</b> ${aqi}</p>
  <p><b>Air Quality:</b> ${airQuality}</p>
  <p><b>Dominant Pollutant:</b> ${dominantes}</p>
  <p>Data provided by the station in ${cidade.replace(/-/g," ")} via AQICN API.</p>
`;```

```
    relatorioDiv.innerHTML = reportText;
    relatorioDiv.style.background = "#e8f5e9";
    relatorioDiv.style.padding = "20px";
    relatorioDiv.style.borderRadius = "10px";
    relatorioDiv.style.marginTop = "20px";
```

Builds the report in HTML.Applies styles directly via JavaScript.

---

### Design Choices

The project focuses on **clarity, simplicity, and environmental aesthetics**.  
The interface is inspired by sustainable design principles, prioritizing green color palettes and intuitive navigation. The choice to integrate a real API (OpenAQ) was made to provide users with genuine, data-driven insights about air pollution in Brazil. However, a simulation fallback system ensures that every state selected by the user returns meaningful results, even if live data is unavailable.

Another important design aspect is the **dual-language system** (Portuguese and English).  
A translation button allows users to seamlessly switch between both languages, making Ecoverse accessible to an international audience.

---

### Future Improvements

Potential future developments include:

- Implementing geolocation to automatically detect the user’s location and display relevant air quality data.
- Expanding the data source to include more environmental APIs.
- Adding charts or visual indicators to better represent pollution levels.
- Developing a mobile app version of Ecoverse using React Native or Flutter.

---

### Conclusion

Ecoverse demonstrates how modern web technologies can be applied to environmental awareness and education.  
By combining **data visualization**, **API integration**, and **user interaction**, the project highlights the importance of technology in addressing climate and public health issues.

This project is part of the CS50x curriculum, showcasing technical learning, creative design, and social responsibility through software development.
