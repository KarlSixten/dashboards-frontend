<script>
  export let callers = [];

  const medals = ["🥇", "🥈", "🥉"];
</script>

<div class="top-callers-card">
  <h2 class="title">Top Callers This Week</h2>
  <ul>
    {#each callers as caller, i}
      <li>
        <span class="name">
          {medals[i] || `#${i + 1}`} {caller.name}
        </span>
        <span class="calls">
          {caller.calls}
        </span>
      </li>
    {/each}
  </ul>
</div>

<style>
  .top-callers-card {
    background: #f9f9f9;
      border-radius: 1rem;
      padding: .5rem;
      text-align: center;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
  }

  .title {
    text-align: center;
    margin: 0 0 1rem;
    font-size: 1.2rem;
    color: #444;
  }

  ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 1rem;
    padding: 0.4rem 0;
    color: #ff5c28;
    font-weight: bold;
  }

  .name {
    flex-grow: 1;
    text-align: left;
  }

  .calls {
    color: #ff5c28;
    font-weight: normal;
  }
</style>