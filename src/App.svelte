<script>
  import ArtistHero from './app/ArtistHero.svelte';

  let artistPromise = null;
  let lastQuery = "";
  let query = "";

  async function getArtist() {
    const artistUrl = `https://rest.bandsintown.com/artists/${encodeURIComponent(query)}?app_id=test`;
    const resp = await fetch(artistUrl);
    const artist = await resp.json();

    return artist;
  }

  function queryChange(ev) {
    query = ev.target.value;
  };

  function search(ev) {
    ev.preventDefault();

    lastQuery = query;
    artistPromise = getArtist();
  };
</script>

<form on:submit={search}>
  <input type="text" on:change={queryChange}/>
  <button type="submit">Search</button>
</form>

{#if artistPromise}
  {#await artistPromise}
    <p>Searching...</p>
  {:then artist}
    {#if artist}
      <ArtistHero {artist}/>
    {:else}
      <p>Artist "{lastQuery}" not found</p>
    {/if}
  {/await}
{/if}

<style>
  :root {
    background: #ededed;
    font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
    font-size: 16px;
    line-height: 1.25;
  }
</style>
