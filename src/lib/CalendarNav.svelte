<script>
    export let currentYear;
    export let currentMonth;
    export let monthNames;
    export let changeMonth;
  </script>
  
  <nav class="navbar navbar-light bg-light mb-3">
    <div class="container d-flex justify-content-between">
      <button class="btn btn-outline-primary" on:click={() => changeMonth(-1)}>&lt;</button>
      <h3 class="m-0">{$currentYear} - {monthNames[$currentMonth]}</h3>
      <button class="btn btn-outline-primary" on:click={() => changeMonth(1)}>&gt;</button>
    </div>
  </nav>
  