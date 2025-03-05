<script>
    import { onMount } from "svelte";
    import { writable } from "svelte/store";
  
    // Az aktuális dátum tárolása
    let today = new Date();
    let currentMonth = writable(today.getMonth());
    let currentYear = writable(today.getFullYear());
  
    // Napok nevei
    const daysOfWeek = ["Hé", "Ke", "Sze", "Csü", "Pé", "Szo", "Vas"];
  
    // A hónap napjainak generálása
    function generateCalendar(year, month) {
      let firstDay = new Date(year, month, 1).getDay();
      firstDay = firstDay === 0 ? 6 : firstDay - 1; // Hétfő legyen az első nap
      let daysInMonth = new Date(year, month + 1, 0).getDate();
  
      let calendarDays = [];
      for (let i = 0; i < firstDay; i++) {
        calendarDays.push(null); // Üres napok az első hét elején
      }
  
      for (let day = 1; day <= daysInMonth; day++) {
        calendarDays.push(day);
      }
  
      return calendarDays;
    }
  
    // Hónapváltás funkció
    function changeMonth(direction) {
      currentMonth.update((m) => {
        let newMonth = m + direction;
        if (newMonth < 0) {
          currentYear.update((y) => y - 1);
          return 11;
        } else if (newMonth > 11) {
          currentYear.update((y) => y + 1);
          return 0;
        }
        return newMonth;
      });
    }
  </script>
  
  <style>
    .calendar {
      display: grid;
      grid-template-columns: repeat(7, 1fr);
      gap: 5px;
      text-align: center;
      max-width: 400px;
      margin: auto;
    }
  
    .day {
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 5px;
    }
  
    .header {
      font-weight: bold;
      background: #f4f4f4;
    }
  
    .controls {
      display: flex;
      justify-content: space-between;
      margin-bottom: 10px;
    }
  </style>
  
  <!-- Navigáció a hónapok között -->
  <div class="controls">
    <button on:click={() => changeMonth(-1)}>◀ Előző</button>
    <h2>{$currentYear} / {$currentMonth + 1}</h2>
    <button on:click={() => changeMonth(1)}>Következő ▶</button>
  </div>
  
  <!-- Naptár megjelenítés -->
  <div class="calendar">
    {#each daysOfWeek as day}
      <div class="header">{day}</div>
    {/each}
  
    {#each generateCalendar($currentYear, $currentMonth) as day}
      <div class="day">{day ? day : ""}</div>
    {/each}
  </div>
  