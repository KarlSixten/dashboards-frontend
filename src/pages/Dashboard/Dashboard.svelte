<script>
    import logo from "./assets/logo.png";
    import { onMount, onDestroy } from "svelte";
    import MetricCard from "../../components/MetricCard.svelte";
    import TopThreeCard from "../../components/TopThreeCard.svelte";
    import GraphCard from "../../components/GraphCard.svelte";

    export let group;

    let weeklyCalls;
    let weeklyContactedLeads;
    let weeklyTopCallers;
    let weeklyValueWon;
    let yearlyValueWon;
    let weeklyEmailsSent;
    let weeklyTopEmailers;
    let weeklyValueWonPerDay = [];

    let lastUpdated;

    $: dataReady =
        weeklyCalls != null &&
        weeklyContactedLeads != null &&
        weeklyTopCallers != null &&
        weeklyValueWon != null &&
        yearlyValueWon != null &&
        weeklyEmailsSent != null &&
        weeklyTopEmailers != null && 
        weeklyValueWonPerDay.length > 0;

    async function fetchDashboardData() {
        const res = await fetch(`http://localhost:8080/api/dashboard-data/${group}`);
        const json = await res.json();
        const data = json.data;

        weeklyCalls = data.weeklyCalls;
        weeklyContactedLeads = data.weeklyContactedLeads;
        weeklyTopCallers = data.weeklyTopCallers;
        weeklyValueWon = data.weeklyValueWon.total;
        weeklyValueWonPerDay = data.weeklyValueWon.perDay.map(item => ({
            label: item.day,
            value: item.value
        })) || [];
        yearlyValueWon = data.yearlyValueWon;
        weeklyEmailsSent = data.weeklyEmailsSent;
        weeklyTopEmailers = data.weeklyTopEmailers;

        lastUpdated = new Date();
    }

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
            minute: "2-digit",
        });
    }

    function formatValueNumber(value) {
        const rounded = Math.floor(value / 100);

        if (rounded >= 1_000_000)
            return (rounded / 1_000_000).toFixed(1).replace(/\.0$/, "") + "M";
        if (rounded >= 1_000)
            return (rounded / 1_000).toFixed(1).replace(/\.0$/, "") + "K";
        return rounded.toString();
    }
</script>

<div class="dashboard-header">
    <h2 id="group">{group.toUpperCase()}</h2>
    <img src={logo} alt="Logo" class="dashboard-logo" />
    <div class="last-updated">Last updated: {formatTime(lastUpdated)}</div>
</div>

{#if dataReady}
    <div class="grid">

        <!--Calls-->
        <MetricCard title="Outbound Calls This Week" value={weeklyCalls} />
        <TopThreeCard
            title="Top Callers This Week"
            topThree={weeklyTopCallers}
        />

        <!--Emails-->
        <MetricCard title="Emails Sent This Week" value={weeklyEmailsSent} />
        <TopThreeCard
            title="Top Emailers This Week"
            topThree={weeklyTopEmailers}
        />

        <!--Leads-->
        <MetricCard
            title="Leads Contacted This Week"
            value={weeklyContactedLeads}
        />

        <!--Value-->
        <MetricCard
            title="Weekly Value Won Total"
            value={formatValueNumber(weeklyValueWon)}
        />
        <GraphCard title="Weekly Value Won" dataPoints={weeklyValueWonPerDay} valueDescription="Value" />

        <MetricCard
            title="Yearly Value Won🤑"
            value={formatValueNumber(yearlyValueWon)}
        />
    </div>
{:else}
    <p>Loading...</p>
{/if}

<style>
    .grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
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
    #group {
        color: #666;
    }
</style>
