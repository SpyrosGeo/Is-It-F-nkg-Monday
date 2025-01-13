<div class="container">
<h2 class="day-title">
      Is it Fucking Monday? {isMonday ? "Yeah" : "No"}
</h2>
<h3 class="day-subtitle">
{#if isMonday}
{:else}
    but it is <span class="day">{getDayName()}</span>
{/if}
</h3>
<div class="date-selector">
    <div class="select-container">
        <span>Month</span>
   <select name="month" id="month" bind:value={selectedMonth}>
    {#each months as month }
    <option value={month.value}>{month.name.toUpperCase()}</option>    
    {/each}
   </select> 
</div>
<div class="select-container">
        <span>Day</span>
   <select name="day" id="day" bind:value={selectedDay}>
    {#each days as day }
    <option value={day}>{day}</option>    
    {/each}
   </select> 
</div>
<div class="select-container">
        <span>Year</span>
   <select name="year" id="year" bind:value={selectedYear}>
    {#each years as year }
    <option value={year}>{year}</option>    
    {/each}
   </select> 
</div>
</div>
</div>

<script lang="ts">
  import { onMount } from 'svelte';
    interface Month {
        name: string;
        value: number;
    }
  const months:Month[] = [
    { name: "January", value: 1 },
    { name: "February", value: 2 },
    { name: "March", value: 3 },
    { name: "April", value: 4 },
    { name: "May", value: 5 },
    { name: "June", value: 6 },
    { name: "July", value: 7 },
    { name: "August", value: 8 },
    { name: "September", value: 9 },
    { name: "October", value: 10 },
    { name: "November", value: 11 },
    { name: "December", value: 12 }
  ];

  const years = Array.from({ length: 2071 - 2025 }, (_, i) => 2025 + i); 

  let selectedMonth = months[0].value; 
  let selectedDay = 1; 
  let selectedYear = years[0]; 
  let isMonday = false; 
  let days:number[] = []; 

  function getDaysInMonth(year:number, month:number) {
    return new Date(year, month, 0).getDate(); 
  }

  function updateDays() {
    const totalDays = getDaysInMonth(selectedYear, selectedMonth);
    days = Array.from({ length: totalDays }, (_, i) => i + 1); 
    if (selectedDay > totalDays) {
      selectedDay = totalDays; 
    }
  }

  function checkIfMonday() {
    const date = new Date(selectedYear, selectedMonth - 1, selectedDay);
    isMonday = date.getDay() === 1; 
  }
  function getDayName(){
    const date = new Date(selectedYear, selectedMonth - 1, selectedDay); 
    return date.toLocaleDateString('en-US', { weekday: 'long' });
  }
  function getCurrentDate(){
    const date = new Date(); 
    selectedYear = date.getFullYear(); 
    selectedMonth = date.getMonth() + 1; 
    selectedDay = date.getDate();
  }

  onMount(() => {
    updateDays();
    getCurrentDate();
  });
  // reactive state checks
  $: if (selectedMonth ||selectedYear) {
    updateDays();
  }
  $: updateDays();
  $: if(selectedDay){
    checkIfMonday();
  }
  // $: checkIfMonday();
</script>


<style>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  height: 100vh;
  width: 100%;
  gap:10px;
}
.date-selector {
  display: flex;
  justify-content: center;
  align-items: center;
    gap:8px;
}
.day-title {
  font-size: 3.5rem;
  font-weight: 700;
  margin: 0;
  padding: 0;
}
.day-subtitle {
  font-size: 3.2rem;
  font-weight: 500;
  margin: 0;
  padding: 0;
}
.select-container{
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;
}

.day{
    color:forestgreen;
}
</style>