<script>
  let artistPromise = null;
  let lastQuery = "";
  let numberFormatter = new Intl.NumberFormat('en-US');
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
      <h1>{artist.name}</h1>
      <p>{numberFormatter.format(artist.tracker_count)} Followers</p>
      <p>{numberFormatter.format(artist.upcoming_event_count)} Upcoming {artist.upcoming_event_count == 1 ? "Concert" : "Concerts"}</p>
    {:else}
      <p>Artist "{lastQuery}" not found</p>
    {/if}
  {/await}
{/if}
