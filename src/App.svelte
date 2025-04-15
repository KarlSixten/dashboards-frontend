<script>
  import { onMount } from 'svelte';
  import MetricCard from './components/MetricCard.svelte';

  let weeklyCalls = null;
  let weeklyContactedLeads = null;

  onMount(async () => {
  const res = await fetch('http://localhost:8080/api/dashboard-data');
  const json = await res.json();
  weeklyCalls = json.data.weeklyCalls;
  weeklyContactedLeads = json.data.weeklyContactedLeads;
});
</script>

{#if weeklyCalls !== null && weeklyContactedLeads !== null}
  <MetricCard title="Outbound Calls This Week" value={weeklyCalls} />
  <MetricCard title="Leads Contacted This Week" value={weeklyContactedLeads} />
{:else}
  <p>Loading...</p>
{/if}