<script>
	import {setContext } from 'svelte';
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
	function clearExpenses() {
		expenses=[];
	}
	// context
	setContext('remove', removeExpense);
</script>

<!-- <style></style> -->
<!-- CSS/STYLING -->

<NavBar />
<main class="content">
	<ExpensesList {expenses} {removeExpense}/>
	<button type="button" class="btn btn-primary btn-block" on:click={clearExpenses}>
		clear expenses
	</button>
</main>