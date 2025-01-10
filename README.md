# plumbquick
Plumbing Service Time Reporting
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Plumber Service Tracker</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Plumber Service Tracker</h1>
        
        <form id="serviceForm">
            <div class="day-form" id="monday">
                <h3>Monday</h3>
                <label for="monday-date">Date:</label>
                <input type="date" id="monday-date" required>
                <label for="monday-start-time">Start Time:</label>
                <input type="time" id="monday-start-time" required>
                <label for="monday-end-time">End Time:</label>
                <input type="time" id="monday-end-time" required>
                <label for="monday-location">Location:</label>
                <input type="text" id="monday-location" required>
                <label for="monday-miles">Miles Driven:</label>
                <input type="number" id="monday-miles" required>
            </div>
            
            <div class="day-form" id="tuesday">
                <h3>Tuesday</h3>
                <label for="tuesday-date">Date:</label>
                <input type="date" id="tuesday-date" required>
                <label for="tuesday-start-time">Start Time:</label>
                <input type="time" id="tuesday-start-time" required>
                <label for="tuesday-end-time">End Time:</label>
                <input type="time" id="tuesday-end-time" required>
                <label for="tuesday-location">Location:</label>
                <input type="text" id="tuesday-location" required>
                <label for="tuesday-miles">Miles Driven:</label>
                <input type="number" id="tuesday-miles" required>
            </div>

            <!-- Repeat similar blocks for Wednesday, Thursday, Friday -->

            <button type="button" onclick="calculateTotals()">Calculate Totals</button>
            <button type="button" id="reportButton" onclick="generateReport()">Generate Report</button>
        </form>

        <div class="results">
            <h3>Total Hours & Miles</h3>
            <p id="total-hours">Total Hours Worked: 0</p>
            <p id="total-miles">Total Miles Driven: 0</p>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/0.4.1/html2canvas.min.js"></script>
    <script src="script.js"></script>
</body>
</html>



body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f9;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
}

.container {
    background-color: white;
    padding: 20px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    width: 90%;
    max-width: 600px;
}

h1 {
    text-align: center;
}

.day-form {
    margin-bottom: 20px;
}

label {
    display: block;
    margin: 5px 0;
}

input[type="date"], input[type="time"], input[type="text"], input[type="number"] {
    width: 100%;
    padding: 8px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

button {
    width: 100%;
    padding: 10px;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
}

button:hover {
    background-color: #45a049;
}

.results {
    margin-top: 20px;
    padding: 10px;
    background-color: #f9f9f9;
    border-radius: 4px;
}

.results h3 {
    margin-bottom: 10px;
}

.results p {
    font-size: 18px;
    font-weight: bold;
}



function calculateTotals() {
    let totalHours = 0;
    let totalMiles = 0;

    // Loop through days to calculate hours and miles
    for (let day of ['monday', 'tuesday', 'wednesday', 'thursday', 'friday']) {
        let startTime = document.getElementById(`${day}-start-time`).value;
        let endTime = document.getElementById(`${day}-end-time`).value;
        let miles = parseFloat(document.getElementById(`${day}-miles`).value);

        if (startTime && endTime && !isNaN(miles)) {
            let start = new Date(`1970-01-01T${startTime}:00`);
            let end = new Date(`1970-01-01T${endTime}:00`);
            let hoursWorked = (end - start) / (1000 * 60 * 60);
            totalHours += hoursWorked;
            totalMiles += miles;
        }
    }

    // Display results
    document.getElementById('total-hours').textContent = `Total Hours Worked: ${totalHours.toFixed(2)}`;
    document.getElementById('total-miles').textContent = `Total Miles Driven: ${totalMiles}`;
}

function generateReport() {
    html2canvas(document.querySelector("#serviceForm")).then(canvas => {
        // Convert the canvas to an image (PNG)
        const imgData = canvas.toDataURL("image/png");

        // Send this image via SMS (Twilio API)
        sendSms(imgData);
    });
}

function sendSms(imageData) {
    fetch('/send-sms', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            image: imageData,
            to: '2148507296' // Replace with your number
        })
    })
    .then(response => response.json())
    .then(data => alert('SMS sent successfully'))
    .catch(error => console.error('Error:', error));
}
