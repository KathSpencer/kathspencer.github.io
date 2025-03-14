layout: page
title: "One Million Steps"
permalink: /OneMillionSteps

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@20,400,0,0&icon_names=footprint" />
    <title>Step Tracker</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
      body {
        font-family: poppins, Arial, sans-serif;
        text-align: center;
        margin: 20px;
        font-size: 20px;
        text-transform: uppercase;
        background-color: rgb(248, 248, 248);
        color: #1c5235;
      }
      .container{
        background-color: white;
        width: 90%;
        margin: 40px auto;
        padding: .7em .7em 2.5em ;
        border-radius: 20px;
        box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px;
      }
      h1 {
        font-size: 1em;
        font-weight: 400;
      }
      p{
        margin: 0px;
      }
      #total-steps{
        font-size: 3.25em;
        margin: 0px !important;
      }
      .note{
        font-size: .7em;
      }
      .progress-container {
        width: 80%;
        margin: 30px auto 20px;
        background-color: #ddd;
        border-radius: 60px;
        overflow: hidden;
        box-shadow: rgba(0, 0, 0, 0.08) 0px 2px 4px 0px inset;
      }
      .progress-bar {
        height: 30px;
        background-color: #1c5235;
        text-align: center;
        line-height: 30px;
        color: white;
        width: 0%;
      }
    </style>
  </head>
  <body>
    <div class="container">
        <h1>Kath's Step Count</h1>
        <p>
        <span id="total-steps">Loading...</span>
        </p>
        <p>
            <span class="material-symbols-outlined" style="vertical-align: text-bottom">footprint</span> Total steps
        </p>
        <div class="progress-container">
        <div class="progress-bar" id="progress-bar">0%</div>
        </div>
        <p class="note">
        Last updated <span id="last-updated">Loading...</span>
        </p>
        <script>
        const SHEET_API_URL =
            "https://script.google.com/macros/s/AKfycbxKolLXlSKNyRXLA2BLFYzjmNXjxiMO_-Tpif682goUMNby5fEPtFBfUilhO6wYPQDcFA/exec";

        async function fetchStepData() {
            try {
            let response = await fetch(SHEET_API_URL);
            let data = await response.json();

            document.getElementById("total-steps").innerText = data.totalSteps.toLocaleString();
            document.getElementById("last-updated").innerText = formatDate(data.date);

            let progress = (data.totalSteps / 1000000) * 100;
            document.getElementById("progress-bar").style.width = progress + "%";
            document.getElementById("progress-bar").innerText = Math.round(progress) + "%";

            drawChart(data.totalSteps);
            } catch (error) {
            console.error("Error fetching step data:", error);
            }
        }
        function formatDate(dateString) {
            let date = new Date(dateString);
            let options = { month: 'long', day: 'numeric', hour: 'numeric', minute: '2-digit', hour12: true };
            return date.toLocaleString('en-US', options);
        }
        fetchStepData();
        </script>
    </div>
  </body>
</html>
