<script>
	import {setContext } from 'svelte';
	// components
	import NavBar from './NavBar.svelte';
	import Title from './Title.svelte';
	import ExpensesList from './ExpensesList.svelte';
	import Totals from './Totals.svelte';
	import ExpenseForm from './ExpenseForm.svelte';

	// data
	import expensesData from './expenses';

	// variables
	let expenses = [...expensesData];

	// reactive
	$: total = expenses.reduce((acc, curr) => {
		return (acc += curr.amount)
	}, 0);

	// functions
	function removeExpense(id) {
		expenses = expenses.filter(item => item.id !== id);
	}
	function clearExpenses() {
		expenses=[];
	}
	function addExpense({name, amount}) {
		let expense = {
			id:Math.random() * Date.now(),
			name,
			amount
		};
		expenses = [expense, ...expenses];
	}


	// context
	setContext('remove', removeExpense);
</script>

<!-- <style></style> -->
<!-- CSS/STYLING -->

<NavBar />
<main class="content">
	<ExpenseForm {addExpense}/>
	<Totals title="total expenses" {total} />
	<ExpensesList {expenses} {removeExpense}/>
	<button type="button" class="btn btn-primary btn-block" on:click={clearExpenses}>
		clear expenses
	</button>
</main>