<script>
  import { onMount } from 'svelte';
  import MetricCard from './components/MetricCard.svelte';

  let outboundCalls = null;
  let leadsContacted = null;

  onMount(async () => {
  const resCalls = await fetch('http://localhost:8080/api/calls-this-week');
  const jsonCalls = await resCalls.json();
  outboundCalls = jsonCalls.data;

  const resLeads = await fetch('http://localhost:8080/api/contacted-leads-this-week');
  const jsonLeads = await resLeads.json();
  leadsContacted = jsonLeads.data;
});
</script>

{#if outboundCalls !== null}
  <MetricCard title="Outbound Calls This Week" value={outboundCalls} />
{:else}
  <p>Loading...</p>
{/if}

{#if leadsContacted !== null}
  <MetricCard title="Leads contacted This Week" value={leadsContacted} />
{:else}
  <p>Loading...</p>
{/if}