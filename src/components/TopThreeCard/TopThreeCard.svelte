<script>
  import firstMedal from "./assets/1st-place-medal.png";
  import secondMedal from "./assets/2nd-place-medal.png";
  import thirdMedal from "./assets/3rd-place-medal.png";

  const medals = [firstMedal, secondMedal, thirdMedal];
  export let title;
  export let topThree = [];
</script>

<div class="top-three-card">
  <h2 class="title">{title}</h2>
  <ul>
    {#each topThree as person, i}
      <li>
        <div class="name-and-medal">
          <img src={medals[i]} class="medal" alt="{i + 1}. place medal" />
          <span class="name">{person.name}</span>
        </div>
        <span class="count">{person.count}</span>
      </li>
    {/each}
  </ul>
</div>

<style>
  .top-three-card {
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

.name-and-medal {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.name {
  flex-grow: 1;
  text-align: left;
}

.count {
  color: #ff5c28;
  font-weight: normal;
}

.medal {
  height: 25px;
}
</style>
