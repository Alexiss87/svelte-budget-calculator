<script>
  import { blur, slide, scale, fade, fly } from "svelte/transition";
  import { getContext, createEventDispatcher } from "svelte";
  import { quintOut } from "svelte/easing";
  export let id;
  export let name = "";
  export let amount = 0;
  // export let removeExpense;
  let displayAmount = false;

  function toggleAmount() {
    displayAmount = !displayAmount;
  }

  //context
  //const removeExpense = getContext("remove");
  const { removeExpense } = getContext("state");
  const { modify } = getContext("state");

  //Event dispatcher
  // const dispatch = createEventDispatcher();
</script>

<article class="single-expense">
  <div class="expense-info">
    <h2>
      {name}
      <button class="amount-btn" on:click={toggleAmount}>
        <i class="fas fa-caret-down" />
      </button>
    </h2>
    {#if displayAmount}
      <h4 transition:slide>amount: ${amount}</h4>
      <!-- <h4
        in:fly={{ x: -100, y: -100, duration: 2000, delay: 500, easing: quintOut }}
        out:slide>
        amount: ${amount}
      </h4> -->
    {/if}
  </div>

  <div class="expense-button">
    <button class="expense-btn edit-btn" on:click={() => modify(id)}>
      <i class="fas fa-pen" />
    </button>
    <button class="expense-btn delete-btn" on:click={() => removeExpense(id)}>
      <i class="fas fa-trash" />
    </button>
    <!-- <button class="expense-btn delete-btn" on:click={() => dispatch('removeExpense',id)}>
      <i class="fas fa-trash" />
    </button> -->
  </div>
</article>
