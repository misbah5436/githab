<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Grade 9 Digital Mark List</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.4.0/jspdf.umd.min.js"></script>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            text-align: center;
            background: linear-gradient(to right, #4facfe, #00f2fe);
            color: white;
            padding: 20px;
        }

        #school-name {
            font-size: 26px;
            font-weight: bold;
            margin-bottom: 20px;
            animation: fadeIn 2s ease-in-out infinite alternate;
        }

        @keyframes fadeIn {
            from { opacity: 0.3; }
            to { opacity: 1; }
        }

        #login-form, #mark-list, #history-list, #change-password {
            background: rgba(255, 255, 255, 0.15);
            padding: 25px;
            border-radius: 15px;
            box-shadow: 0px 4px 15px rgba(0, 0, 0, 0.3);
            width: 50%;
            margin: auto;
            backdrop-filter: blur(10px);
        }

        h2 {
            color: #ffd700;
        }

        input {
            width: 80%;
            padding: 12px;
            margin: 10px 0;
            border-radius: 8px;
            border: none;
            text-align: center;
        }

        button {
            padding: 12px 18px;
            margin-top: 10px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            width: 100%;
            font-weight: bold;
            transition: 0.3s;
        }

        .btn-primary {
            background-color: #ff5722;
            color: white;
        }

        .btn-primary:hover {
            background-color: #e64a19;
        }

        .btn-secondary {
            background-color: #03a9f4;
            color: white;
        }

        .btn-secondary:hover {
            background-color: #0288d1;
        }
    </style>
</head>
<body>
    <div id="school-name">YECKA ABADO SECONDARY SCHOOL</div>

    <!-- Login Form -->
    <div id="login-form">
        <h2>Login</h2>
        <input type="text" id="username" placeholder="Username"><br>
        <input type="password" id="password" placeholder="Password"><br>
        <button class="btn-primary" onclick="login()">Login</button>
    </div>

    <!-- Mark Entry Form -->
    <div id="mark-list" style="display: none;">
        <h2>Grade 9 Digital Mark List</h2>
        <input type="text" id="student-name" placeholder="Student Name">
        <input type="text" id="section" placeholder="Section">
        <input type="text" id="roll-no" placeholder="Roll No">
        
        <h3>Enter Marks</h3>
        <input type="number" id="math" placeholder="Mathematics">
        <input type="number" id="physics" placeholder="Physics">
        <input type="number" id="biology" placeholder="Biology">
        <input type="number" id="chemistry" placeholder="Chemistry">
        <input type="number" id="english" placeholder="English">
        <input type="number" id="amharic" placeholder="Amharic">
        <input type="number" id="civic" placeholder="Civic & Ethical Education">
        <input type="number" id="geography" placeholder="Geography">
        <input type="number" id="history" placeholder="History">
        
        <button class="btn-primary" onclick="calculateMarks()">Calculate</button>
        <button class="btn-secondary" onclick="saveHistory()">Save History</button>
        <button class="btn-primary" onclick="generateCertificate()">Generate Certificate</button>
        <button class="btn-secondary" onclick="showHistory()">Show History</button>
        <button class="btn-secondary" onclick="generateHistoryPDF()">Download History PDF</button>
        <button class="btn-secondary" onclick="showChangePassword()">Change Password</button>
        <button class="btn-secondary" onclick="logout()">Logout</button>

        <h3 id="result"></h3>
        <h3 id="average"></h3>
    </div>

    <!-- Change Password Form -->
    <div id="change-password" style="display: none;">
        <h2>Change Password</h2>
        <input type="password" id="new-password" placeholder="New Password"><br>
        <button class="btn-primary" onclick="changePassword()">Save New Password</button>
        <button class="btn-secondary" onclick="hideChangePassword()">Cancel</button>
    </div>

    <!-- Student History -->
    <div id="history-list" style="display: none;">
        <h2>Student History</h2>
        <ul id="history"></ul>
    </div>

    <script>
        let storedPassword = "abado";

        function login() {
            const username = document.getElementById("username").value;
            const password = document.getElementById("password").value;
            if (username === "abado" && password === storedPassword) {
                document.getElementById("login-form").style.display = "none";
                document.getElementById("mark-list").style.display = "block";
            } else {
                alert("Invalid Username or Password");
            }
        }

        function showChangePassword() {
            document.getElementById("change-password").style.display = "block";
        }

        function hideChangePassword() {
            document.getElementById("change-password").style.display = "none";
        }

        function changePassword() {
            storedPassword = document.getElementById("new-password").value;
            alert("Password changed successfully!");
            hideChangePassword();
        }

        function calculateMarks() {
            const marks = {
                Math: parseInt(document.getElementById("math").value),
                Physics: parseInt(document.getElementById("physics").value),
                Biology: parseInt(document.getElementById("biology").value),
                Chemistry: parseInt(document.getElementById("chemistry").value),
                English: parseInt(document.getElementById("english").value),
                Amharic: parseInt(document.getElementById("amharic").value),
                Civic: parseInt(document.getElementById("civic").value),
                Geography: parseInt(document.getElementById("geography").value),
                History: parseInt(document.getElementById("history").value)
            };

            const totalMarks = Object.values(marks).reduce((a, b) => a + b, 0);
            const averageMarks = totalMarks / Object.keys(marks).length;

            document.getElementById("result").textContent = `Total Marks: ${totalMarks}`;
            document.getElementById("average").textContent = `Average Marks: ${averageMarks.toFixed(2)}`;
        }

        function saveHistory() {
            const studentName = document.getElementById("student-name").value;
            const section = document.getElementById("section").value;
            const rollNo = document.getElementById("roll-no").value;
            const marks = {
                Math: document.getElementById("math").value,
                Physics: document.getElementById("physics").value,
                Biology: document.getElementById("biology").value,
                Chemistry: document.getElementById("chemistry").value,
                English: document.getElementById("english").value,
                Amharic: document.getElementById("amharic").value,
                Civic: document.getElementById("civic").value,
                Geography: document.getElementById("geography").value,
                History: document.getElementById("history").value
            };

            let history = JSON.parse(localStorage.getItem("studentHistory")) || [];
            history.push({ studentName, section, rollNo, marks });
            localStorage.setItem("studentHistory", JSON.stringify(history));
            alert("History saved!");
        }

        function showHistory() {
            const historyList = document.getElementById("history");
            historyList.innerHTML = "";
            const history = JSON.parse(localStorage.getItem("studentHistory")) || [];

            if (history.length === 0) {
                historyList.innerHTML = "<li>No history found.</li>";
            } else {
                history.forEach(entry => {
                    const li = document.createElement("li");
                    li.textContent = `${entry.studentName} (Section: ${entry.section}, Roll No: ${entry.rollNo}) - Marks: ${JSON.stringify(entry.marks)}`;
                    historyList.appendChild(li);
                });
            }

            document.getElementById("history-list").style.display = "block";
        }

        function generateCertificate() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();

            doc.setFontSize(18);
            doc.text("YECKA ABADO SECONDARY SCHOOL", 50, 20);
            doc.setFontSize(14);
            doc.text("Certificate of Achievement", 60, 40);
            doc.setFontSize(12);
            doc.text("This is to certify that", 70, 50);
            doc.text(document.getElementById("student-name").value, 85, 60);
            doc.text("has successfully completed Grade 9 with the following marks:", 40, 70);
            
            let subjects = ["Math", "Physics", "Biology", "Chemistry", "English", "Amharic", "Civic", "Geography", "History"];
            let y = 80;
            subjects.forEach(subject => {
                doc.text(`${subject}: ` + document.getElementById(subject.toLowerCase()).value, 50, y);
                y += 10;
            });

            doc.text("Congratulations!", 80, y + 10);
            doc.save("certificate.pdf");
        }

        function generateHistoryPDF() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();
            doc.setFontSize(16);
            doc.text("YECKA ABADO SECONDARY SCHOOL", 50, 20);
            doc.setFontSize(14);
            doc.text("Student Mark History", 60, 30);

            let history = JSON.parse(localStorage.getItem("studentHistory")) || [];
            let y = 40;

            if (history.length === 0) {
                doc.text("No history available.", 50, y);
            } else {
                history.forEach((entry, index) => {
                    doc.text(`${index + 1}. ${entry.studentName} - Section: ${entry.section}, Roll No: ${entry.rollNo}`, 10, y);
                    y += 10;
                    Object.keys(entry.marks).forEach(subject => {
                        doc.text(`${subject}: ${entry.marks[subject]}`, 20, y);
                        y += 6;
                    });
                    y += 10;
                });
            }

            doc.save("Student_History.pdf");
        }

        function logout() {
            document.getElementById("mark-list").style.display = "none";
            document.getElementById("history-list").style.display = "none";
            document.getElementById("login-form").style.display = "block";
        }
    </script>
</body>
</html>
