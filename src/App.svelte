<script>
  import { onMount } from 'svelte';
  import MetricCard from './components/MetricCard.svelte';
  import TopCallersCard from './components/TopCallersCard.svelte';

  let weeklyCalls = null;
  let weeklyContactedLeads = null;
  let weeklyTopCallers = null;


  onMount(async () => {
  const res = await fetch('http://localhost:8080/api/dashboard-data');
  const json = await res.json();
  const data = json.data;

  weeklyCalls = data.weeklyCalls;
  weeklyContactedLeads = data.weeklyContactedLeads;
  weeklyTopCallers = data.weeklyTopCallers;
});
</script>

{#if weeklyCalls !== null && weeklyContactedLeads !== null && weeklyTopCallers !== null}
  <div class="grid">
    <MetricCard title="Outbound Calls This Week" value={weeklyCalls} />
    <MetricCard title="Leads Contacted This Week" value={weeklyContactedLeads} />
    <TopCallersCard callers={weeklyTopCallers} />
  </div>
{:else}
  <p>Loading...</p>
{/if}

<style>
  .grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1.5rem;
    margin-top: 2rem;
  }
</style>