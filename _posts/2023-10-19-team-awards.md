---
toc: true
comments: false
layout: post
title: "Team Awards"
description: Get the list of teams' awards they've won
type: tangibles
courses: { compsci: {week: 0} }
---

# Team Awards
> Get any team's awards

On this page, you can:

- Enter any FRC team's team number and get all the awards they've ever won any year.
- Parse through the data specifically for our Outreach department

Limitations (as of 10/19/2023):

- You must refresh the page every time you want a new team's data. If this is not done, data will stack and you won't be able to tell whose data is whose.

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Team Awards</title>
    <style>
        p, table, thead, tr, th, td, tbody {
            color: white;
        }
        table {
            border-collapse: collapse;
            width: 100%;
            margin-top: 20px;
        }
        thead {
            background-color: #0b2262;
            color: white;
        }
        th, td {
            border: 1px solid #091b4f;
            padding: 8px;
            text-align: left;
        }
        tr:nth-child(even) {
            background-color: #091b4f;
        }
        .winner-background {
            background-color: #FFA500;
        }
        .impact-background {
            background-color: #61C0BF;
        }
        .rei-background {
            background-color: green;
        }
    </style>
</head>
<body>
    <p>Enter a team number:</p>
    <input id="teamNumber" type="text">
    <button onclick="fetchAwards()">Get Awards</button>
    <button onclick="parseAwards()">Parse Awards</button>
    <table id="data-table">
        <thead>
            <tr>
                <th>Award Name</th>
                <th>Event Name</th>
                <th>Year</th>
            </tr>
        </thead>
        <tbody>
            <!-- Data will be displayed here -->
        </tbody>
    </table>
    <script>
        function fetchAwards() {
            // Clear existing rows in the table (except headers)
            const dataTable = document.getElementById("data-table");
            const tbody = dataTable.querySelector("tbody");
            tbody.innerHTML = ""; // Remove all rows
            const teamNumber = document.getElementById("teamNumber").value;
            const apiKey = "IJGdHobToWBkfqCzNHRKGWKyy66mMiOl7A7IOs1WjcgfS4d6sIryBqQWsALTPTVv";
            const apiUrl = "https://www.thebluealliance.com/api/v3";
            const teamKey = "frc" + teamNumber; // Replace with the team key you want to retrieve awards for
            // Define the endpoint and parameters
            const endpoint = `/team/${teamKey}/awards`;
            const requestUrl = `${apiUrl}${endpoint}`;
            fetch(requestUrl, {
                headers: {
                    "X-TBA-Auth-Key": apiKey,
                }
            })
            .then(response => response.json())
            .then(data => {
                if (Array.isArray(data)) {
                    console.log(data);
                    // Iterate through the award data and create table rows
                    data.forEach(award => {
                        const row = dataTable.insertRow();         
                        // Create cells for each piece of data
                        const awardNameCell = row.insertCell(0);
                        const eventNameCell = row.insertCell(1);
                        const yearCell = row.insertCell(2);
                        // Populate the cells with award data
                        awardNameCell.textContent = award.name;
                        eventNameCell.textContent = award.event_name;
                        yearCell.textContent = award.year;
                    });
                } else {
                    console.error("Data received from the API is not an array.");
                }
            })
            .catch(error => {
                console.error("Error fetching data:", error);
            });
        }
        function parseAwards() {
            const dataTable = document.getElementById("data-table");
            const rows = dataTable.querySelectorAll("tr");
            rows.forEach(row => {
                const cells = row.querySelectorAll("td");
                cells.forEach(cell => {
                    const text = cell.textContent;
                    if (text.includes("Regional Winners")) {
                        cell.classList.add("winner-background");
                    } else if (text.includes("Regional Chairman's Award") || text.includes("Regional FIRST Impact Award")) {
                        cell.classList.add("impact-background");
                    } else if (text.includes("Regional Engineering Inspiration Award")) {
                        cell.classList.add("rei-background");
                    }
                });
            });
        }
    </script>
</body>
</html>
