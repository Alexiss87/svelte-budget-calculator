<script>
  import Title from "./Title.svelte";
  import { fly } from "svelte/transition";
  import { flip } from "svelte/animate";
  export let expenses = [];
  //export let removeExpense
  import ExpenseItem from "./ExpenseItem.svelte";
</script>

<style>
  h2 {
    text-transform: capitalize;
  }
</style>

<section>
  <Title title="expense list" />
  <ul>
    {#each expenses as expenseObj, index (expenseObj.id)}
      <div
        in:fly={{ x: 200, delay: (index + 0.5) * 700 }}
        out:fly={{ x: -200 }}
        animate:flip>
        <ExpenseItem {...expenseObj} />
      </div>
    {:else}
      <h2>Currently you have no expenses</h2>
    {/each}
  </ul>

</section>
