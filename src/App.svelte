<script>
import Navbar from "./Navbar.svelte";
import ExpensesList from "./ExpenseList.svelte"

import expensesData from "./expenses"
let expenses = [...expensesData]
</script>

<Navbar />
<main class="content">
	<ExpensesList />
</main>

