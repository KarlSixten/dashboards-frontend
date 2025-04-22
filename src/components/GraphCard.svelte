<script>
    export let title;
    export let dataPoints = [];

    import { onMount, onDestroy } from "svelte";
    import Chart from "chart.js/auto";

    let chartCanvas;
    let chartInstance;

    onMount(() => {
        if (chartCanvas && dataPoints.length > 0) {
            chartInstance = new Chart(chartCanvas, {
                type: "bar",
                data: {
                    labels: dataPoints.map((dp) => dp.label),
                    datasets: [
                        {
                            label: title,
                            data: dataPoints.map((dp) => dp.value),
                            backgroundColor: "#ff5c28", // Clerk orange
                        },
                    ],
                },
                options: {
                    scales: {
                        y: {
                            beginAtZero: true,
                        },
                    },
                },
            });
        }
    });

    $: if (chartInstance) {
        chartInstance.data.labels = dataPoints.map((dp) => dp.label);
        chartInstance.data.datasets[0].data = dataPoints.map((dp) => dp.value);
        chartInstance.update();
    }

    onDestroy(() => {
        if (chartInstance) {
            chartInstance.destroy();
        }
    });
</script>

<div class="graph-card">
    <h3 class="title">{title}</h3>
    <canvas bind:this={chartCanvas}></canvas>
</div>

<style>
    .graph-card {
        background: #f9f9f9;
        border-radius: 1rem;
        padding: 0.5rem;
        text-align: center;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }
    .title {
        text-align: center;
        margin: 0 0 1rem;
        font-size: 1.2rem;
        color: #444;
    }
</style>
