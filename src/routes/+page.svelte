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
    <div class="calendar-grid">
        {#each generateCalendar($currentYear, $currentMonth) as day, index}
            <div class="day-box {day && (index % 7 === 5 || index % 7 === 6)} 
                    {day && day === today.getDate() && $currentMonth === today.getMonth() && $currentYear === today.getFullYear() ? 'today' : ''}">
                
                <!-- A hónap első hetében az első sorba írjuk a napok neveit -->
                {#if index < 7}
                <div class="day-name">{daysOfWeek[index]}</div>
                {/if}

                <div class="day-number">{day ? day : ""}</div>
            </div>
        {/each}

    </div>
</div>
  
<style>
    .calendar-container {
        margin: auto;
        padding: 0 20px; /* Két oldali behúzás */
    }
  
    .calendar-grid {
        display: grid;
        grid-template-columns: repeat(7, 1fr);
        gap: 2px;
    }
  
    .day-box {
        width: 100%;
        aspect-ratio: 1;
        display: flex;
        flex-direction: column;
        align-items: flex-start; /* Felső sarokba helyezzük a tartalmat */
        justify-content: flex-start;
        padding: 8px;
        border: 1px solid #ddd;
        border-radius: 8px;
        text-align: left;
        font-size: 16px;
        background-color: #ffffff;
        position: relative;
    }
  
    .day-name {
        font-weight: bold;
        font-size: 14px;
        color: #333;
        position: absolute;
        top: 5px;
        left: 10px;
    }
  
    .day-number {
        font-size: 18px;
        font-weight: normal;
        position: absolute;
        top: 25px;
        left: 10px;
    }
  
    .today {
        background-color: #0d6efd !important;
        color: white;
        font-weight: bold;
    }
  </style>
  
  