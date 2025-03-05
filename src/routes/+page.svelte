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
        let firstDay = new Date(year, month, 1).getDay(); // Az első nap hétindexe (0 = Vasárnap)
        firstDay = firstDay === 0 ? 0 : firstDay; // Ha már vasárnappal kezdünk, akkor nincs eltolás
        let daysInMonth = new Date(year, month + 1, 0).getDate();

        let prevMonthDays = new Date(year, month, 0).getDate(); // Az előző hónap napjainak száma
        let calendarDays = [];

        // **Előző hónap napjai** (Ha szükséges az első sor kitöltéséhez)
        for (let i = firstDay - 1; i >= 0; i--) {
            calendarDays.push({
            day: prevMonthDays - i,
            isCurrentMonth: false,
            });
        }

        // **Aktuális hónap napjai**
        for (let day = 1; day <= daysInMonth; day++) {
            calendarDays.push({
            day: day,
            isCurrentMonth: true,
            });
        }

        // **Következő hónap napjai** (Kitöltjük a rácsot teljes 6 soros megjelenítésig)
        while (calendarDays.length % 7 !== 0) {
            calendarDays.push({
            day: calendarDays.length % 7 + 1,
            isCurrentMonth: false,
            });
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
        {#each generateCalendar($currentYear, $currentMonth) as dayObj, index}
        <div class="day-box 
          {dayObj.isCurrentMonth ? '' : 'faded'} 
          {dayObj.isToday ? 'today' : ''}">
          
          <!-- Ha az első héten vagyunk, kiírjuk a napok nevét -->
          {#if index < 7}
            <div class="day-name">{daysOfWeek[index]}</div>
          {/if}
      
          <div class="day-number">{dayObj.day}</div>
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
    .faded {
        opacity: 0.5;
        color: #aaa;
    }
  </style>
  
  