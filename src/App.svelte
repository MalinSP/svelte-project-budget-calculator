<script>
  // import expensesData from './expenses'
  let expenses = []

  let setName = ''
  let setAmount = null
  let setId = null

  let isFormOpen = false

  $: isEditing = setId ? true : false
  $: total = expenses.reduce((acc, curr) => {
    return (acc += curr.amount)
  }, 0)

  import { setContext } from 'svelte'

  import Navbar from './Navbar.svelte'
  import ExpensesList from './ExpenseList.svelte'
  import Totals from './Totals.svelte'
  import FormExpense from './FormExpense.svelte'

  function showForm() {
    isFormOpen = true
  }
  function hideForm() {
    isFormOpen = false
    setName = ''
    setAmount = null
    setId = null
  }
  function removeExpense(id) {
    expenses = expenses.filter((item) => item.id !== id)
  }
  function clearexpenses() {
    expenses = []
  }
  function addExpense({ name, amount }) {
    let expense = { id: Math.random() * Date.now(), name, amount }
    expenses = [expense, ...expenses]
  }
  function setModifiedExpense(id) {
    let expense = expenses.find((item) => item.id === id)
    setId = expense.id
    setName = expense.name
    setAmount = expense.amount
    showForm()
  }
  function editExpense({ name, amount }) {
    expenses = expenses.map((item) => {
      return item.id === setId ? { ...item, name, amount } : { ...item }
    })
    setId = null
    setAmount = null
    setName = ''
  }

  setContext('remove', removeExpense)
  setContext('modify', setModifiedExpense)
</script>

<Navbar {showForm} />
<main class="content">
  {#if isFormOpen}
    <FormExpense
      {addExpense}
      name={setName}
      amount={setAmount}
      {isEditing}
      {editExpense}
      {hideForm}
    />
  {/if}
  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} />
  <button
    type="button"
    class="btn btn-primary btn-block"
    on:click={clearexpenses}>clear expense</button
  >
</main>
