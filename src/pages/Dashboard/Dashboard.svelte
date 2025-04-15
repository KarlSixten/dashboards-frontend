<script>
    import logo from "./assets/logo.png";
    import { onMount, onDestroy } from "svelte";
    import MetricCard from "../../components/MetricCard.svelte";
    import TopCallersCard from "../../components/TopCallersCard.svelte";

    let weeklyCalls;
    let weeklyContactedLeads;
    let weeklyTopCallers;

    let lastUpdated;

    async function fetchDashboardData() {
        const res = await fetch("http://localhost:8080/api/dashboard-data");
        const json = await res.json();
        const data = json.data;

        weeklyCalls = data.weeklyCalls;
        weeklyContactedLeads = data.weeklyContactedLeads;
        weeklyTopCallers = data.weeklyTopCallers;

        lastUpdated = new Date();
    };

    onMount(() => {
        fetchDashboardData();

        const interval = setInterval(
            () => {
                fetchDashboardData();
            },
            5 * 60 * 1000, // 5 minutes
        );

        onDestroy(() => {
            clearInterval(interval);
        });
    });

    function formatTime(date) {
        return date?.toLocaleTimeString([], {
            hour: "2-digit",
            minute: "2-digit"
        });
    };
</script>

<div class="dashboard-header">
    <div class="last-updated">Last updated: {formatTime(lastUpdated)}</div>
    <img src={logo} alt="Logo" class="dashboard-logo" />
</div>

{#if weeklyCalls && weeklyContactedLeads && weeklyTopCallers}
    <div class="grid">
        <MetricCard title="Outbound Calls This Week" value={weeklyCalls} />
        <MetricCard
            title="Leads Contacted This Week"
            value={weeklyContactedLeads}
        />
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
    .dashboard-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 2rem;
    position: relative;
  }

  .dashboard-logo {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
  }

  .last-updated {
    font-size: 0.9rem;
    color: #666;
  }
</style>
