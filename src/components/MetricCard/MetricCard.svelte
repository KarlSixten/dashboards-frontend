<script>
  export let title;
  export let value;
</script>

<div class="metric-card">
  <h2 class="title">{title}</h2>
  <p>{value}</p>
</div>

<style>
  .metric-card {
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
  p {
    margin: 0.5rem 0 0;
    font-size: 4rem;
    font-weight: bold;
    color: #ff5c28;
  }
</style>
