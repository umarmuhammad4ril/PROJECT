<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css">
    <title>Employee Record System</title>
</head>
<body>
    <div class="container">
        <p>&nbsp;</p>
       <center><p><img src="taraba1.jpg"/></p></center> 
       <h3> TARABA STATE UNIVERSITY</h3>
       <h1>      
          EMPLOYEE RECORD MANAGEMENT SYSTEM
         </h1>
        <form id="employeeForm">
            <input type="hidden" id="employeeIndex" value="">
            <input type="text" id="name" placeholder="Name" required>
            <input type="email" id="email" placeholder="Email" required>
            <input type="text" id="position" placeholder="Position" required>
            <input type="text" id="department" placeholder="Department" required>
            <input type="date" id="dateOfJoining" required>
            <button type="submit">Add Employee</button>
        </form>

        <h2>Employee List</h2>
        <table id="employeeTable">
            <thead>
                <tr>
                    <th>Name</th>
                    <th>Email</th>
                    <th>Position</th>
                    <th>Department</th>
                    <th>Date of Joining</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody></tbody>
        </table>
    </div>

    <script src="script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    margin: 0;
    padding: 20px;
}
p {
    width: 365px;
    height: 296;
    align-content: center;
}
.container {
    width: 80%;
    margin: auto;
}
h3 {
   font-size: 15px;
   font-weight: bold;
   color:#FF0000;
   text-align: center;
}   
h1 {
    color:#0000FF;
    text-align: center;
    font-size: 20px;
    
}


form {
    display: flex;
    flex-direction: column;
}

input {
    margin: 10px 0;
    padding: 10px;
}

button {
    padding: 10px;
    background-color: #007BFF;
    color: white;
    border: none;
    cursor: pointer;
}

button:hover {
    background-color: #0056b3;
}

table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
}

table, th, td {
    border: 1px solid black;
}

th, td {
   padding: 10px;
   text-align: left;
}
document.addEventListener('DOMContentLoaded', () => {
    const employeeForm = document.getElementById('employeeForm');
    const employeeTableBody = document.querySelector('#employeeTable tbody');

    // Load employees from localStorage
    loadEmployees();

    // Add event listener to form submission
    employeeForm.addEventListener('submit', (e) => {
        e.preventDefault();
        
        const index = document.getElementById('employeeIndex').value;

        const employee = {
            name: document.getElementById('name').value,
            email: document.getElementById('email').value,
            position: document.getElementById('position').value,
            department: document.getElementById('department').value,
            dateOfJoining: document.getElementById('dateOfJoining').value,
        };

        if (index) {
            // Update existing employee
            updateEmployee(index, employee);
        } else {
            // Add new employee
            saveEmployee(employee);
        }
        
        // Clear form fields
        employeeForm.reset();
        loadEmployees();
    });
});

// Function to save employee to localStorage
function saveEmployee(employee) {
    let employees = JSON.parse(localStorage.getItem('employees')) || [];
    
    employees.push(employee);
    
    localStorage.setItem('employees', JSON.stringify(employees));
}

// Function to load employees from localStorage and display them in the table
function loadEmployees() {
    const employeeTableBody = document.querySelector('#employeeTable tbody');
    
    // Clear existing rows
    employeeTableBody.innerHTML = '';

    let employees = JSON.parse(localStorage.getItem('employees')) || [];

    employees.forEach((employee, index) => {
        const row = document.createElement('tr');
        
        row.innerHTML = `
            <td>${employee.name}</td>
            <td>${employee.email}</td>
            <td>${employee.position}</td>
            <td>${employee.department}</td>
            <td>${employee.dateOfJoining}</td>
            <td><button onclick="editEmployee(${index})">Edit</button><button onclick="deleteEmployee(${index})">Delete</button></td>
        `;
        
        employeeTableBody.appendChild(row);
    });
}

// Function to delete an employee from localStorage
function deleteEmployee(index) {
    let employees = JSON.parse(localStorage.getItem('employees')) || [];
    
    employees.splice(index, 1);
    
    localStorage.setItem('employees', JSON.stringify(employees));
    
    loadEmployees();
}

// Function to edit an employee's details
function editEmployee(index) {
    let employees = JSON.parse(localStorage.getItem('employees')) || [];
    
    const employee = employees[index];

    document.getElementById('name').value = employee.name;
    document.getElementById('email').value = employee.email;
    document.getElementById('position').value = employee.position;
    document.getElementById('department').value = employee.department;
    document.getElementById('dateOfJoining').value = employee.dateOfJoining;

    // Set the hidden input field with the index of the employee being edited
    document.getElementById('employeeIndex').value = index;
}
