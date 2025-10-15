<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Personal Finance Budget Tracker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background-color: #f4f6f9;
            color: #333;
        }
        h1, h2, h3 {
            color: #2c3e50;
        }
        h1 {
            text-align: center;
            margin-bottom: 1rem;
        }
        section {
            margin-bottom: 2rem;
            padding: 1rem;
            background: #fff;
            border-radius: 12px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 0.5rem;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 0.5rem;
            text-align: left;
        }
        th {
            background-color: #e8eef3;
        }
        .amount {
            text-align: right;
        }
        .remove-btn {
            background: #e74c3c;
            color: #fff;
            border: none;
            padding: 4px 8px;
            cursor: pointer;
            border-radius: 4px;
        }
        .remove-btn:hover {
            background: #c0392b;
        }
        input[type="text"], input[type="number"], input[type="date"] {
            padding: 0.5rem;
            margin-right: 0.5rem;
            margin-bottom: 0.5rem;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        button {
            padding: 0.5rem 1rem;
            margin-right: 0.5rem;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            color: #fff;
        }
        button.add-btn {
            background-color: #4a90e2;
        }
        button.add-btn:hover {
            background-color: #357ab8;
        }
        button.calc-btn {
            background-color: #27ae60;
        }
        button.calc-btn:hover {
            background-color: #1e8449;
        }
        button.print-btn {
            background-color: #f39c12;
        }
        button.print-btn:hover {
            background-color: #d68910;
        }
        #results-section {
            display: none;
        }
        #monthFilter {
            padding: 0.5rem;
            margin-top: 1rem;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        .late {
            color: #e74c3c;
        }
        /* Responsive adjustments for smaller screens */
        @media (max-width: 700px) {
            body {
                margin: 10px;
            }
            section {
                margin-bottom: 1.5rem;
                padding: 0.75rem;
            }
            h1 {
                font-size: 1.5rem;
            }
            h2 {
                font-size: 1.25rem;
            }
            h3 {
                font-size: 1.1rem;
            }
            label {
                display: block;
                margin-bottom: 0.5rem;
            }
            input[type="text"],
            input[type="number"],
            input[type="date"],
            select {
                width: 100%;
                margin-right: 0;
                margin-bottom: 0.75rem;
            }
            button {
                width: 100%;
                margin: 0.25rem 0;
            }
            .remove-btn {
                padding: 4px 6px;
            }
            table {
                font-size: 0.85rem;
                display: block;
                overflow-x: auto;
            }
            thead, tbody {
                width: 100%;
            }
            th, td {
                padding: 0.4rem;
            }
        }
    </style>
</head>
<body>
    <h1>Personal Finance Budget Tracker</h1>
    <p>This tool helps you track your income and expenses, assign each expense to the most appropriate paycheck, and view monthly summaries. Enter your paychecks and bills below, click <strong>Calculate Assignments</strong>, and review the results. Use the <strong>Print</strong> button to save or print a PDF of your current view.</p>

    <section id="income-section">
        <h2>Add Income (Paycheck)</h2>
        <label>Date: <input type="date" id="incomeDate"></label>
        <label>Description: <input type="text" id="incomeDesc" placeholder="e.g., Salary"></label>
        <label>Amount ($): <input type="number" step="0.01" id="incomeAmt"></label>
        <button class="add-btn" onclick="addIncome()">Add Income</button>
        <h3>Income List</h3>
        <table id="incomeTable">
            <thead>
                <tr><th>Date</th><th>Description</th><th class="amount">Amount ($)</th><th>Actions</th></tr>
            </thead>
            <tbody></tbody>
        </table>
    </section>

    <section id="expense-section">
        <h2>Add Expense</h2>
        <label>Date: <input type="date" id="expenseDate"></label>
        <label>Description: <input type="text" id="expenseDesc" placeholder="e.g., Mortgage"></label>
        <label>Amount ($): <input type="number" step="0.01" id="expenseAmt"></label>
        <button class="add-btn" onclick="addExpense()">Add Expense</button>
        <h3>Expense List</h3>
        <table id="expenseTable">
            <thead>
                <tr><th>Date</th><th>Description</th><th class="amount">Amount ($)</th><th>Paid?</th><th>Recurring?</th><th>Actions</th></tr>
            </thead>
            <tbody></tbody>
        </table>
    </section>

    <!-- Credit Card Section -->
    <section id="credit-card-section">
        <h2>Add Credit Card</h2>
        <p>Use this section to track the available credit on your cards. You can either select an existing card from the dropdown or enter a new card name. Enter the current available credit and click <strong>Add Credit Card</strong> to save it. Cards you add will appear in the list below and will also be available in the dropdown for future entries.</p>
        <label for="cardNameSelect">Select existing card:
            <select id="cardNameSelect">
                <option value="" disabled selected>Select existing card</option>
            </select>
        </label>
        <label>Or new card name: <input type="text" id="cardNameInput" placeholder="Card Name"></label>
        <label>Available Credit ($): <input type="number" step="0.01" id="cardAmount"></label>
        <button class="add-btn" onclick="addCreditCard()">Add Credit Card</button>
        <h3>Credit Card List</h3>
        <table id="creditCardTable">
            <thead>
                <tr><th>Card Name</th><th class="amount">Available Credit ($)</th><th>Actions</th></tr>
            </thead>
            <tbody></tbody>
        </table>
    </section>

    <section id="control-section">
        <button class="calc-btn" onclick="calculateAssignments()">Calculate Assignments</button>
        <button class="print-btn" onclick="window.print()">Print Current View</button>
        <button onclick="clearAll()">Clear All Data</button>
    </section>

    <!-- Monthly Planner Section -->
    <section id="planner-section">
        <h2>Monthly Planner</h2>
        <p>Select a month to view and plan how expenses will be covered by the paychecks in that month. This will automatically calculate assignments for all data and filter the results to your chosen month.</p>
        <label for="planMonthSelect">Select month:</label>
        <select id="planMonthSelect">
            <option value="">Select month</option>
        </select>
        <button class="calc-btn" onclick="planForMonth()">Plan This Month</button>
        <p id="plannerMessage" style="margin-top:0.5rem;font-style:italic;color:#555;"></p>
    </section>

    <section id="results-section">
        <h2>Assignment Results</h2>
        <p>Select a month to filter the results. When \"All Months\" is selected, all data is shown.</p>
        <label for="monthFilter">Filter by month:</label>
        <select id="monthFilter" onchange="filterByMonth()">
            <option value="">All Months</option>
        </select>
        <h3>Paycheck Balances</h3>
        <table id="calcIncomeTable">
            <thead>
                <tr><th>Date</th><th>Description</th><th class="amount">Amount ($)</th><th class="amount">Remaining ($)</th></tr>
            </thead>
            <tbody></tbody>
        </table>
        <h3>Expense Assignments</h3>
        <table id="calcExpenseTable">
            <thead>
                <tr><th>Date</th><th>Description</th><th class="amount">Amount ($)</th><th>Paid From Paycheck</th><th>Notes</th></tr>
            </thead>
            <tbody></tbody>
        </table>
        <h3>Monthly Summary</h3>
        <table id="monthlySummary">
            <thead>
                <tr><th>Month</th><th class="amount">Income ($)</th><th class="amount">Expenses ($)</th><th class="amount">Net ($)</th></tr>
            </thead>
            <tbody></tbody>
        </table>
        <h3>Suggestions</h3>
        <ul id="suggestions"></ul>
    </section>

    <script>
        // Arrays to hold income, expense, and credit card entries
        let incomes = [];
        let expenses = [];
        let creditCards = [];

        // Month names for consistent labels (avoids time zone issues when formatting dates)
        const monthNamesShort = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
        const monthNamesLong = ['January','February','March','April','May','June','July','August','September','October','November','December'];

        // Preload existing income and expense data. Feel free to modify or remove these entries.
        incomes = [
            { date: '2025-10-15', description: 'Bianca payment', amount: 225.00 },
            { date: '2025-10-17', description: 'Salary (Ntsikelelo Bonga)', amount: 1565.00 },
            { date: '2025-10-23', description: 'Unemployment (Lesley)', amount: 1106.00 },
            { date: '2025-10-31', description: 'Salary (Ntsikelelo Bonga)', amount: 1600.00 },
            { date: '2025-10-31', description: 'Bianca payment', amount: 180.00 },
            { date: '2025-11-01', description: 'Mason payment', amount: 560.00 },
            { date: '2025-11-06', description: 'Unemployment (Lesley)', amount: 1106.00 },
            { date: '2025-11-14', description: 'Salary (Ntsikelelo Bonga)', amount: 1600.00 },
            { date: '2025-11-15', description: 'Bianca payment', amount: 225.00 },
            { date: '2025-11-20', description: 'Unemployment (Lesley)', amount: 1106.00 },
            { date: '2025-11-28', description: 'Salary (Ntsikelelo Bonga)', amount: 1600.00 },
            { date: '2025-11-30', description: 'Bianca payment', amount: 225.00 },
            { date: '2025-12-01', description: 'Mason payment', amount: 560.00 },
            { date: '2025-12-04', description: 'Unemployment (Lesley)', amount: 1106.00 },
            { date: '2025-12-12', description: 'Salary (Ntsikelelo Bonga)', amount: 1600.00 },
            { date: '2025-12-15', description: 'Bianca payment', amount: 225.00 },
            { date: '2025-12-18', description: 'Unemployment (Lesley)', amount: 1106.00 },
            { date: '2025-12-26', description: 'Salary (Ntsikelelo Bonga)', amount: 1600.00 },
            { date: '2025-12-31', description: 'Bianca payment', amount: 225.00 },
            { date: '2026-01-01', description: 'Mason payment', amount: 560.00 },
            { date: '2026-01-01', description: 'Unemployment (Lesley)', amount: 1106.00 },
            { date: '2026-01-09', description: 'Salary (Ntsikelelo Bonga)', amount: 1600.00 },
            { date: '2026-01-15', description: 'Bianca payment', amount: 225.00 },
            { date: '2026-01-15', description: 'Unemployment (Lesley)', amount: 1106.00 },
            { date: '2026-01-23', description: 'Salary (Ntsikelelo Bonga)', amount: 1600.00 },
            { date: '2026-01-29', description: 'Unemployment (Lesley)', amount: 1106.00 },
            { date: '2026-01-31', description: 'Bianca payment', amount: 225.00 },
            { date: '2026-02-01', description: 'Mason payment', amount: 560.00 },
            { date: '2026-02-06', description: 'Salary (Ntsikelelo Bonga)', amount: 1600.00 },
            { date: '2026-02-12', description: 'Unemployment (Lesley)', amount: 1106.00 },
            { date: '2026-02-15', description: 'Bianca payment', amount: 225.00 },
            { date: '2026-02-20', description: 'Salary (Ntsikelelo Bonga)', amount: 1600.00 },
            { date: '2026-02-26', description: 'Unemployment (Lesley)', amount: 1106.00 },
            { date: '2026-02-28', description: 'Bianca payment', amount: 225.00 }
        ];
        // Preloaded recurring expenses based on your highlighted bills
        expenses = [
            { date: '2025-10-31', description: 'Mortgage', amount: 1245.00, paid: false, recurring: true },
            { date: '2025-10-18', description: 'Geico', amount: 232.68, paid: false, recurring: true },
            { date: '2025-10-28', description: 'Carvana', amount: 418.00, paid: false, recurring: true },
            { date: '2025-10-27', description: 'Verizon', amount: 271.40, paid: false, recurring: true },
            { date: '2025-11-01', description: 'Water', amount: 105.71, paid: false, recurring: true },
            { date: '2025-11-04', description: 'Electricity', amount: 305.77, paid: false, recurring: true },
            { date: '2025-10-15', description: 'Internet', amount: 97.34, paid: false, recurring: true },
            { date: '2025-10-24', description: 'Car Payment Kia', amount: 219.06, paid: false, recurring: true },
            { date: '2025-11-05', description: 'YouTube', amount: 13.99, paid: false, recurring: true },
            { date: '2025-11-12', description: 'PlayStation', amount: 16.45, paid: false, recurring: true },
            { date: '2025-10-19', description: 'L Apple', amount: 21.94, paid: false, recurring: true },
            { date: '2025-10-21', description: 'Z ChatGPT', amount: 21.94, paid: false, recurring: true },
            { date: '2025-10-21', description: 'Apple One', amount: 37.95, paid: false, recurring: true }
        ];
        // No preloaded credit cards; you can add them manually
        creditCards = [];

        document.addEventListener('DOMContentLoaded', function() {
            renderIncomeTable();
            renderExpenseTable();
            renderCreditCardTable();
            updateCardDropdown();
            updatePlanMonthOptions();
        });

        // Render the income list table
        function renderIncomeTable() {
            const tbody = document.getElementById('incomeTable').querySelector('tbody');
            tbody.innerHTML = '';
            incomes.forEach((inc, index) => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td>${inc.date}</td>
                    <td>${inc.description}</td>
                    <td class="amount">${inc.amount.toFixed(2)}</td>
                    <td><button class="remove-btn" onclick="removeIncome(${index})">Remove</button></td>
                `;
                tbody.appendChild(tr);
            });
        }

        // Add an income entry
        function addIncome() {
            const date = document.getElementById('incomeDate').value;
            const desc = document.getElementById('incomeDesc').value.trim();
            const amt = parseFloat(document.getElementById('incomeAmt').value);
            if (!date || !desc || isNaN(amt)) {
                alert('Please enter a valid date, description, and amount for the income.');
                return;
            }
            incomes.push({ date, description: desc, amount: amt });
            document.getElementById('incomeDate').value = '';
            document.getElementById('incomeDesc').value = '';
            document.getElementById('incomeAmt').value = '';
            renderIncomeTable();
            updatePlanMonthOptions();
        }

        // Remove an income entry
        function removeIncome(index) {
            incomes.splice(index, 1);
            renderIncomeTable();
            updatePlanMonthOptions();
        }

        // Add an expense entry
        function addExpense() {
            const date = document.getElementById('expenseDate').value;
            const desc = document.getElementById('expenseDesc').value.trim();
            const amt = parseFloat(document.getElementById('expenseAmt').value);
            if (!date || !desc || isNaN(amt)) {
                alert('Please enter a valid date, description, and amount for the expense.');
                return;
            }
            expenses.push({ date, description: desc, amount: amt, paid: false, recurring: false });
            document.getElementById('expenseDate').value = '';
            document.getElementById('expenseDesc').value = '';
            document.getElementById('expenseAmt').value = '';
            renderExpenseTable();
            updatePlanMonthOptions();
        }

        // Remove an expense entry
        function removeExpense(index) {
            expenses.splice(index, 1);
            renderExpenseTable();
            updatePlanMonthOptions();
        }

        // Toggle the paid status of an expense
        function toggleExpensePaid(index) {
            expenses[index].paid = !expenses[index].paid;
            renderExpenseTable();
        }

        // Toggle the recurring status of an expense
        function toggleExpenseRecurring(index) {
            expenses[index].recurring = !expenses[index].recurring;
            renderExpenseTable();
        }

        // Render the expense list table
        function renderExpenseTable() {
            const tbody = document.getElementById('expenseTable').querySelector('tbody');
            tbody.innerHTML = '';
            expenses.forEach((exp, index) => {
                const tr = document.createElement('tr');
                const paidChecked = exp.paid ? 'checked' : '';
                const recurringChecked = exp.recurring ? 'checked' : '';
                tr.innerHTML = `
                    <td>${exp.date}</td>
                    <td>${exp.description}</td>
                    <td class="amount">${exp.amount.toFixed(2)}</td>
                    <td><input type="checkbox" ${paidChecked} onchange="toggleExpensePaid(${index})"></td>
                    <td><input type="checkbox" ${recurringChecked} onchange="toggleExpenseRecurring(${index})"></td>
                    <td><button class="remove-btn" onclick="removeExpense(${index})">Remove</button></td>
                `;
                tbody.appendChild(tr);
            });
        }

        // Render credit card table
        function renderCreditCardTable() {
            const tbody = document.getElementById('creditCardTable').querySelector('tbody');
            tbody.innerHTML = '';
            creditCards.forEach((card, index) => {
                const row = document.createElement('tr');
                row.innerHTML = `<td>${card.name}</td><td class="amount">${card.available.toFixed(2)}</td>` +
                    `<td><button onclick="deleteCreditCard(${index})">Delete</button></td>`;
                tbody.appendChild(row);
            });
        }

        // Update credit card dropdown
        function updateCardDropdown() {
            const select = document.getElementById('cardNameSelect');
            select.innerHTML = '<option value="" disabled selected>Select existing card</option>';
            creditCards.forEach(card => {
                const opt = document.createElement('option');
                opt.value = card.name;
                opt.textContent = card.name;
                select.appendChild(opt);
            });
        }

        // Add a new credit card entry
        function addCreditCard() {
            let selectedName = document.getElementById('cardNameSelect').value;
            const newName = document.getElementById('cardNameInput').value.trim();
            const amountInput = document.getElementById('cardAmount').value;
            const amountVal = parseFloat(amountInput);
            let cardName = newName ? newName : selectedName;
            if (cardName && !isNaN(amountVal)) {
                creditCards.push({ name: cardName, available: amountVal });
                document.getElementById('cardNameInput').value = '';
                document.getElementById('cardAmount').value = '';
                document.getElementById('cardNameSelect').value = '';
                renderCreditCardTable();
                updateCardDropdown();
            } else {
                alert('Please enter a card name and a valid available credit amount.');
            }
        }

        // Delete a credit card entry
        function deleteCreditCard(index) {
            creditCards.splice(index, 1);
            renderCreditCardTable();
            updateCardDropdown();
        }

        // Plan for a selected month
        function planForMonth() {
            const selected = document.getElementById('planMonthSelect').value;
            if (!selected) {
                alert('Please select a month to plan.');
                return;
            }
            calculateAssignments();
            const monthFilter = document.getElementById('monthFilter');
            monthFilter.value = selected;
            filterByMonth();
            updatePlannerMessage(selected);
        }

        // Compute and display a summary message for the selected month
        function updatePlannerMessage(monthKey) {
            const parts = monthKey.split('-');
            const year = parseInt(parts[0]);
            const monthIndex = parseInt(parts[1]) - 1;
            const label = monthNamesLong[monthIndex] + ' ' + year;
            const monthlyIncome = incomes
                .filter(inc => inc.date.substring(0, 7) === monthKey)
                .reduce((sum, inc) => sum + inc.amount, 0);
            const unpaidExpTotal = expenses
                .filter(exp => exp.date.substring(0, 7) === monthKey && !exp.paid)
                .reduce((sum, exp) => sum + exp.amount, 0);
            const paidExpTotal = expenses
                .filter(exp => exp.date.substring(0, 7) === monthKey && exp.paid)
                .reduce((sum, exp) => sum + exp.amount, 0);
            const relevantExp = unpaidExpTotal > 0 ? unpaidExpTotal : paidExpTotal;
            const net = monthlyIncome - relevantExp;
            let message = `Showing assignments and summary for ${label}.`;
            message += ` Total income: $${monthlyIncome.toFixed(2)}.`;
            if (unpaidExpTotal > 0) {
                message += ` Unpaid expenses: $${unpaidExpTotal.toFixed(2)}.`;
            } else if (paidExpTotal > 0) {
                message += ` Paid expenses: $${paidExpTotal.toFixed(2)}.`;
            } else {
                message += ` No expenses recorded for this month.`;
            }
            message += ` Net balance: $${net.toFixed(2)}.`;
            const msgEl = document.getElementById('plannerMessage');
            msgEl.textContent = message;
        }

        // Clear all data
        function clearAll() {
            if (confirm('This will remove all incomes, expenses, and credit cards. Continue?')) {
                incomes = [];
                expenses = [];
                creditCards = [];
                calcIncomes = [];
                calcExpenses = [];
                monthOptions.clear();
                document.getElementById('results-section').style.display = 'none';
                renderIncomeTable();
                renderExpenseTable();
                renderCreditCardTable();
                updateCardDropdown();
                renderCalcTables();
                updatePlanMonthOptions();
            }
        }

        // Data structures used in calculations
        let calcIncomes = [];
        let calcExpenses = [];
        let monthOptions = new Set();

        // Populate monthly planner dropdown
        function updatePlanMonthOptions() {
            const select = document.getElementById('planMonthSelect');
            if (!select) return;
            const months = new Set();
            incomes.forEach(inc => months.add(inc.date.substring(0,7)));
            expenses.forEach(exp => months.add(exp.date.substring(0,7)));
            const monthArray = Array.from(months).sort();
            select.innerHTML = '<option value=\"\">Select month</option>';
            monthArray.forEach(m => {
                const parts = m.split('-');
                const year = parseInt(parts[0]);
                const monthIndex = parseInt(parts[1]) - 1;
                const label = monthNamesShort[monthIndex] + ' ' + year;
                const opt = document.createElement('option');
                opt.value = m;
                opt.textContent = label;
                select.appendChild(opt);
            });
        }

        // Update month filter dropdown
        function updateMonthFilter(monthArray) {
            const monthFilter = document.getElementById('monthFilter');
            monthFilter.innerHTML = '<option value=\"\">All Months</option>';
            monthArray.forEach(m => {
                const parts = m.split('-');
                const year = parseInt(parts[0]);
                const monthIndex = parseInt(parts[1]) - 1;
                const label = monthNamesShort[monthIndex] + ' ' + year;
                const opt = document.createElement('option');
                opt.value = m;
                opt.textContent = label;
                monthFilter.appendChild(opt);
            });
        }

        // Calculate assignments
        function calculateAssignments() {
            if (incomes.length === 0) {
                alert('Please add at least one income (paycheck) before calculating.');
                return;
            }
            calcIncomes = incomes.map((inc, index) => ({ id: index, date: inc.date, description: inc.description, amount: inc.amount, remaining: inc.amount, month: inc.date.substring(0,7) })).sort((a,b) => new Date(a.date) - new Date(b.date));
            let calcExpensesBase = expenses.filter(exp => !exp.paid).map((exp, index) => ({
                id: index,
                date: exp.date,
                description: exp.description,
                amount: exp.amount,
                assignedTo: null,
                late: false,
                month: exp.date.substring(0,7),
                recurring: exp.recurring
            }));
            let lastIncomeDate = null;
            if (calcIncomes.length > 0) {
                lastIncomeDate = new Date(calcIncomes[calcIncomes.length - 1].date);
            } else if (incomes.length > 0) {
                lastIncomeDate = new Date(Math.max.apply(null, incomes.map(inc => new Date(inc.date))));
            }
            let recurringInstances = [];
            if (lastIncomeDate) {
                calcExpensesBase.forEach(exp => {
                    if (exp.recurring) {
                        const originalDate = new Date(exp.date);
                        let nextDate = new Date(originalDate.getTime());
                        nextDate.setMonth(nextDate.getMonth() + 1);
                        while (nextDate <= lastIncomeDate) {
                            const year = nextDate.getFullYear();
                            const month = (nextDate.getMonth() + 1).toString().padStart(2, '0');
                            const day = nextDate.getDate().toString().padStart(2, '0');
                            const dateStr = `${year}-${month}-${day}`;
                            recurringInstances.push({
                                id: exp.id,
                                date: dateStr,
                                description: exp.description,
                                amount: exp.amount,
                                assignedTo: null,
                                late: false,
                                month: `${year}-${month}`,
                                recurring: true
                            });
                            nextDate.setMonth(nextDate.getMonth() + 1);
                        }
                    }
                });
            }
            calcExpenses = calcExpensesBase.concat(recurringInstances);
            calcExpenses.sort((a,b) => new Date(a.date) - new Date(b.date));
            monthOptions.clear();
            calcIncomes.forEach(item => monthOptions.add(item.month));
            calcExpenses.forEach(item => monthOptions.add(item.month));
            const monthArray = Array.from(monthOptions).sort();
            updateMonthFilter(monthArray);
            calcExpenses.forEach(exp => {
                let assignedIndex = -1;
                for (let i = 0; i < calcIncomes.length; i++) {
                    const pay = calcIncomes[i];
                    if (new Date(pay.date) <= new Date(exp.date) && pay.remaining >= exp.amount) {
                        assignedIndex = i;
                        break;
                    }
                }
                let late = false;
                if (assignedIndex === -1) {
                    for (let i = 0; i < calcIncomes.length; i++) {
                        const pay = calcIncomes[i];
                        if (new Date(pay.date) > new Date(exp.date) && pay.remaining >= exp.amount) {
                            assignedIndex = i;
                            late = true;
                            break;
                        }
                    }
                }
                if (assignedIndex >= 0) {
                    calcIncomes[assignedIndex].remaining -= exp.amount;
                    exp.assignedTo = assignedIndex;
                    exp.late = late;
                }
            });
            renderCalcTables();
            document.getElementById('results-section').style.display = 'block';
            document.getElementById('results-section').scrollIntoView({ behavior: 'smooth' });
        }

        // Render calculated tables
        function renderCalcTables() {
            const incomeTbody = document.getElementById('calcIncomeTable').querySelector('tbody');
            incomeTbody.innerHTML = '';
            calcIncomes.forEach((pay, idx) => {
                const tr = document.createElement('tr');
                tr.setAttribute('data-month', pay.month);
                tr.innerHTML = `
                    <td>${pay.date}</td>
                    <td>${pay.description}</td>
                    <td class=\"amount\">${pay.amount.toFixed(2)}</td>
                    <td class=\"amount\">${pay.remaining.toFixed(2)}</td>
                `;
                incomeTbody.appendChild(tr);
            });
            const expTbody = document.getElementById('calcExpenseTable').querySelector('tbody');
            expTbody.innerHTML = '';
            calcExpenses.forEach(exp => {
                const tr = document.createElement('tr');
                tr.setAttribute('data-month', exp.month);
                let notes = '';
                let payDesc = '';
                if (exp.assignedTo !== null) {
                    const pay = calcIncomes[exp.assignedTo];
                    payDesc = `${pay.date} - ${pay.description}`;
                    if (exp.late) {
                        notes = '<span class=\"late\">Paid from later paycheck</span>';
                    }
                } else {
                    payDesc = '<span class=\"late\">Unfunded</span>';
                    notes = '<span class=\"late\">No paycheck with enough funds</span>';
                }
                if (exp.recurring) {
                    notes += (notes ? ' ' : '') + '(Recurring)';
                }
                tr.innerHTML = `
                    <td>${exp.date}</td>
                    <td>${exp.description}</td>
                    <td class=\"amount\">${exp.amount.toFixed(2)}</td>
                    <td>${payDesc}</td>
                    <td>${notes}</td>
                `;
                expTbody.appendChild(tr);
            });
            const monthSummaryBody = document.getElementById('monthlySummary').querySelector('tbody');
            monthSummaryBody.innerHTML = '';
            const summary = {};
            calcIncomes.forEach(pay => {
                if (!summary[pay.month]) summary[pay.month] = { income: 0, expense: 0 };
                summary[pay.month].income += pay.amount;
            });
            calcExpenses.forEach(exp => {
                if (!summary[exp.month]) summary[exp.month] = { income: 0, expense: 0 };
                summary[exp.month].expense += exp.amount;
            });
            Object.keys(summary).sort().forEach(m => {
                const parts = m.split('-');
                const year = parseInt(parts[0]);
                const monthIndex = parseInt(parts[1]) - 1;
                const label = monthNamesLong[monthIndex] + ' ' + year;
                const net = summary[m].income - summary[m].expense;
                const tr = document.createElement('tr');
                tr.setAttribute('data-month', m);
                tr.innerHTML = `
                    <td>${label}</td>
                    <td class=\"amount\">${summary[m].income.toFixed(2)}</td>
                    <td class=\"amount\">${summary[m].expense.toFixed(2)}</td>
                    <td class=\"amount\">${net.toFixed(2)}</td>
                `;
                monthSummaryBody.appendChild(tr);
            });
            const sugList = document.getElementById('suggestions');
            sugList.innerHTML = '';
            const totalUnpaid = expenses.filter(exp => !exp.paid).reduce((sum, exp) => sum + exp.amount, 0);
            const totalPaid = expenses.filter(exp => exp.paid).reduce((sum, exp) => sum + exp.amount, 0);
            const suggestions = [];
            if (totalUnpaid > 0) {
                suggestions.push(`Ntsikelelo Bonga has unpaid expenses totaling $${totalUnpaid.toFixed(2)}. Consider allocating funds to these debts.`);
            } else if (totalPaid > 0) {
                suggestions.push('All recorded expenses are marked as paid. Great job, Ntsikelelo!');
            }
            suggestions.push(
                'Prioritize paying bills with the nearest due dates to avoid late fees.',
                'Allocate any surplus funds towards high-interest debt to reduce interest costs.',
                'Keep an emergency fund to handle unexpected expenses without stress.',
                'Consider splitting large bills across multiple paychecks if possible.',
                'Review your budget regularly and adjust your spending to align with your financial goals.'
            );
            suggestions.forEach(text => {
                const li = document.createElement('li');
                li.textContent = text;
                sugList.appendChild(li);
            });
        }

        // Filter tables by selected month
        function filterByMonth() {
            const selected = document.getElementById('monthFilter').value;
            const incomeRows = document.getElementById('calcIncomeTable').querySelectorAll('tbody tr');
            const expenseRows = document.getElementById('calcExpenseTable').querySelectorAll('tbody tr');
            const summaryRows = document.getElementById('monthlySummary').querySelectorAll('tbody tr');
            if (!selected) {
                incomeRows.forEach(row => row.style.display = '');
                expenseRows.forEach(row => row.style.display = '');
                summaryRows.forEach(row => row.style.display = '');
            } else {
                incomeRows.forEach(row => {
                    row.style.display = row.getAttribute('data-month') === selected ? '' : 'none';
                });
                expenseRows.forEach(row => {
                    row.style.display = row.getAttribute('data-month') === selected ? '' : 'none';
                });
                summaryRows.forEach(row => {
                    row.style.display = row.getAttribute('data-month') === selected ? '' : 'none';
                });
            }
        }
    </script>
</body>
</html>
# Budget-APP
