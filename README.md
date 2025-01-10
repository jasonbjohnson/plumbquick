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
            
         
