<script>
  import Github from "./Github.svelte"
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

  import { setContext, onMount, afterUpdate } from 'svelte'

  import Navbar from './Navbar.svelte'
  import ExpensesList from './ExpenseList.svelte'
  import Totals from './Totals.svelte'
  import FormExpense from './FormExpense.svelte'
  import Modal from './Modal.svelte'
import GithubAwait from "./GithubAwait.svelte";

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

  function setLocalStorage() {
    localStorage.setItem("expenses", JSON.stringify(expenses))
  }
  onMount(()=> {
    expenses = localStorage.getItem("expenses") ? JSON.parse(localStorage.getItem("expenses")) : []
  })
  afterUpdate(()=> {
    setLocalStorage()
  })
</script>

<Navbar {showForm} />
<main class="content">
 
  {#if isFormOpen}
  <Modal>
     <FormExpense
      {addExpense}
      name={setName}
      amount={setAmount}
      {isEditing}
      {editExpense}
      {hideForm}
    />
  </Modal>
   {/if}
  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} />
  <button
    type="button"
    class="btn btn-primary btn-block"
    on:click={clearexpenses}>clear expense</button
  >
</main>

<!-- <Modal >
  <h1 slot="header">hello world</h1>
  <p slot="footer">Lorem ipsum dolor sit amet consectetur adipisicing elit. Labore repellat rem nostrum voluptatibus voluptate maiores odio placeat cupiditate ex quae?</p>
</Modal> -->
