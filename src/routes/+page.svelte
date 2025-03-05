<script>
    import { onMount } from "svelte";
    import { writable } from "svelte/store";
    import CalendarNav from "$lib/CalendarNav.svelte"; 

  
    // Az aktuális dátum tárolása
    let today = new Date();
    let currentMonth = writable(today.getMonth());
    let currentYear = writable(today.getFullYear());
  
    // Hónapok nevei
    const monthNames = [
      "Január", "Február", "Március", "Április", "Május", "Június",
      "Július", "Augusztus", "Szeptember", "Október", "November", "December"
    ];
  
    // Napok nevei
    const daysOfWeek = ["H", "K", "SZE", "CS", "P", "SZO", "V"];
  
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

    <!-- Navigációs sáv (Most már komponensként) -->
    <CalendarNav 
        {currentYear} 
        {currentMonth} 
        {monthNames} 
        {changeMonth} 
    />

    <!-- Naptár megjelenítés -->
    <div class="calendar-container">
        <div class="calendar-grid mb-2">
        {#each daysOfWeek as day}
            <div class="day-header">{day}</div>
        {/each}
    </div>

    <div class="calendar-grid">
        {#each generateCalendar($currentYear, $currentMonth) as day}
            <div class="day-box {day && day === today.getDate() && $currentMonth === today.getMonth() && $currentYear === today.getFullYear() ? 'today' : ''}">
                {day ? day : ""}
            </div>
        {/each}
    </div>
</div>
  
  <style>
    .calendar-container {
      max-width: 600px;
      margin: auto;
    }
  
    .calendar-grid {
      display: grid;
      grid-template-columns: repeat(7, 1fr);
      gap: 5px;
    }
  
    .day-box {
      padding: 15px;
      border: 1px solid #ddd;
      border-radius: 8px;
      text-align: center;
      font-size: 18px;
    }
  
    .day-header {
      font-weight: bold;
      background: #f8f9fa;
      padding: 10px;
      border-radius: 5px;
    }
  
    .today {
      background-color: #0d6efd !important;
      color: white;
      font-weight: bold;
    }
  </style>