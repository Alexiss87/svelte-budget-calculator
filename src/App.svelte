<script>
  //import Github from "./Github.svelte";
  import GithubAwait from "./GithubAwait.svelte";

  import Navbar from "./NavBar.svelte";
  import Title from "./Title.svelte";
  import Totals from "./Totals.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  import ExpenseList from "./ExpenseList.svelte";
  import Modal from "./Modal.svelte";
  //import expenseData from "./expenses";
  import { setContext, onMount, afterUpdate } from "svelte";

  //data
  //let expenses = [...expenseData];
  let expenses = [];
  //set editing variables
  let setName = "";
  let setAmount = null;
  let setId = null;
  //toggle form variables
  let isFormOpen = false;

  //reactive
  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((acc, curr) => {
    //console.log({ acc, amount: curr.amount });
    return (acc += curr.amount);
  }, 0);

  //functions
  function showForm() {
    isFormOpen = true;
  }
  function hideForm() {
    isFormOpen = false;
    setAmount = null;
    setId = null;
    setName = "";
  }
  //can be used for prop drilling function or context
  function removeExpense(id) {
    //console.log(`Expense with ${id} deleted`);
    expenses = expenses.filter(item => item.id !== id);
    //setLocalStorage();
  }
  function addExpense({ name, amount }) {
    let id = Math.random() * Date.now();
    let expense = {
      id,
      name,
      amount
    };
    expenses = [expense, ...expenses];
    //console.log(expenses);
    //setLocalStorage();
  }
  function setModifiedExpense(id) {
    let expense = expenses.find(item => item.id === id);
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    showForm();
    //console.log(expense);
  }
  function editExpense({ name, amount }) {
    //console.log(name, amount);;
    expenses = expenses.map(item => {
      return item.id === setId ? { ...item, name, amount } : { ...item };
    });
    setId = null;
    let setName = "";
    let setAmount = null;
    //setLocalStorage();
  }
  function clearExpenses() {
    expenses = [];
    //setLocalStorage();
  }

  //context
  const state = {
    name: "simple name",
    removeExpense: removeExpense,
    modify: setModifiedExpense
  };
  //setContext('remove', removeExpense)
  setContext("state", state);
  //local storage
  function setLocalStorage() {
    localStorage.setItem("expenses", JSON.stringify(expenses));
  }
  onMount(() => {
    expenses = localStorage.getItem("expenses")
      ? JSON.parse(localStorage.getItem("expenses"))
      : [];
  });
  afterUpdate(() => {
    console.log("after update");
    setLocalStorage();
  });
</script>

<style>

</style>

<Navbar {showForm} />
<main class="content">
  <!-- <Github /> -->
  <!-- <GithubAwait /> -->
  {#if isFormOpen}
    <Modal>
      <ExpenseForm
        {addExpense}
        name={setName}
        amount={setAmount}
        {isEditing}
        {editExpense}
        {hideForm} />
    </Modal>
  {/if}

  <Totals title="total expenses" {total} />
  <ExpenseList {expenses} />
  <button
    type="button"
    class="btn btn-primary btn-block"
    on:click={clearExpenses}>
    clear expenses
  </button>
</main>
