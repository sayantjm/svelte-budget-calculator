<script>
	// components
	import NavBar from './NavBar.svelte';
	import Title from './Title.svelte';
	import ExpensesList from './ExpensesList.svelte';

	// data
	import expensesData from './expenses';

	// variables
	let expenses = [...expensesData];
	// functions
	function removeExpense(id) {
		expenses = expenses.filter(item => item.id !== id);
	}
</script>

<!-- <style></style> -->
<!-- CSS/STYLING -->

<NavBar />
<main class="content">
	<ExpensesList {expenses} {removeExpense}/>
</main>