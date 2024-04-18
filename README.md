<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Attendance Tracker</title>
    <style>
        table {
            border-collapse: collapse;
            width:100%;
        }
        td {
            width: 30px;
            height: 30px;
            border: 1px solid black;
            text-align: center;
            cursor: pointer;
        }
        .attended {
            background-color: green;
        }
        .missed {
            background-color: red;
        }
    </style>
</head>
<body>
    <h1>Attendance Tracker</h1>

    <table id="attendanceTable">
        <tr>
            <th>Sun</th>
            <th>Mon</th>
            <th>Tue</th>
            <th>Wed</th>
            <th>Thu</th>
            <th>Fri</th>
            <th>Sat</th>
        </tr>
        <!-- Days will be populated dynamically -->
    </table>

    <div id="statistics">
        <h2>Statistics</h2>
        <p>Number of days attended: <span id="daysAttended">0</span></p>
        <p>75% criteria: <span id="attendanceCriteria"></span></p>
        <p id="bunkOrAttend"></p>
    </div>

    <script>
        const daysInMonth = 30; // Assuming 30 days in a month
        const table = document.getElementById('attendanceTable');
        const daysAttendedSpan = document.getElementById('daysAttended');
        const attendanceCriteriaSpan = document.getElementById('attendanceCriteria');
        const bunkOrAttendParagraph = document.getElementById('bunkOrAttend');

        let daysAttended = 0;

        for (let i = 1; i <= daysInMonth; i++) {
            const dayOfWeek = (i + 5) % 7; // Start with Saturday (0) to align days
            if (dayOfWeek === 0) {
                // Start a new row for the next week
                const newRow = document.createElement('tr');
                table.appendChild(newRow);
            }
            const dayCell = document.createElement('td');
            dayCell.textContent = i;
            dayCell.addEventListener('click', toggleAttendance);
            table.lastChild.appendChild(dayCell);
        }

        function toggleAttendance(event) {
            const cell = event.target;
            if (cell.classList.contains('attended')) {
                cell.classList.remove('attended');
                daysAttended--;
            } else {
                cell.classList.add('attended');
                daysAttended++;
            }
            updateStatistics();
        }

        function updateStatistics() {
            daysAttendedSpan.textContent = daysAttended;
            const totalClasses = daysInMonth;
            const attendancePercentage = (daysAttended / totalClasses) * 100;
            attendanceCriteriaSpan.textContent = `${attendancePercentage.toFixed(2)}%`;

            const classesRequiredFor75Percent = Math.ceil(totalClasses * 0.75);

            if (attendancePercentage >= 75) {
                const classesToBunk = daysAttended - classesRequiredFor75Percent;
                bunkOrAttendParagraph.textContent = `You can still bunk for ${classesToBunk} more classes and maintain 75% attendance.`;
            } else {
                const classesToAttend = classesRequiredFor75Percent - daysAttended;
                bunkOrAttendParagraph.textContent = `You need to attend ${classesToAttend} more classes to maintain 75% attendance.`;
            }
        }

        // Initial update of statistics
        updateStatistics();
    </script>
</body>
</html>
