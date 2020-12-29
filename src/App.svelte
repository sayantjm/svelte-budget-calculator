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
	let setName = '';
	let setAmount = null;
	let setId = null;
	let isFormOpen = false;

	// reactive
	$: isEditing = setId ? true : false;
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
	function setModifiedExpense(id) {
		let expense = expenses.find(item => item.id === id);
		setId = expense.id;
		setName = expense.name;
		setAmount = expense.amount;
		showForm();
	}
	function editExpense({name, amount}) {
		expenses = expenses.map(item => {
			return item.id === setId ? {...item, name:name, amount:amount}:{...item}
		});
		setId = null;
		setName = '';
		setAmount = null;
	}
	function showForm() {
		isFormOpen = true;
	}
	function hideForm() {
		isFormOpen = false;
		setName = '';
		setAmount = null;
		setId = null;
	}
 

	// context
	setContext('remove', removeExpense);
	setContext('modify', setModifiedExpense);
</script>

<!-- <style></style> -->
<!-- CSS/STYLING -->

<NavBar {showForm}/>
<main class="content">
{#if isFormOpen}
	<ExpenseForm {addExpense} 
		name={setName} 
		amount={setAmount} 
		{isEditing} 
		{editExpense} 
		{hideForm}
		/>
{/if}
	<Totals title="total expenses" {total} />
	<ExpensesList {expenses} {removeExpense}/>
	<button type="button" class="btn btn-primary btn-block" on:click={clearExpenses}>
		clear expenses
	</button>
</main>