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

<div class="toolbar">
  <form on:submit={search}>
    <input type="text" placeholder="Search for artists" on:change={queryChange}/>
    <button type="submit">Search</button>
  </form>
</div>
<div class="view">
  {#if artistPromise}
    {#await artistPromise}
    <div class="searching">
      <p>Searching for "{lastQuery}"</p>
    </div>
    {:then artist}
      {#if artist}
        <ArtistHero {artist}/>
      {:else}
        <div class="error">
          <p>Artist "{lastQuery}" not found</p>
        </div>
      {/if}
    {/await}
  {/if}
</div>

<style>
  :root {
    background: #ededed;
    font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
    font-size: 16px;
    line-height: 1.25;
  }

  * {
    box-sizing: border-box;
  }

  .toolbar {
    position: fixed;
    top: 0;
    left: 0;
    height: 72px;
    width: 100vw;
    display: flex;
    align-content: center;
    align-items: center;
    justify-content: center;
    background: #ffffff;
    padding: 0 12px;
    border-bottom: 1px solid #cdcdcd;
  }

  form {
    width: 100%;
    max-width: 546px;
    border: 1px solid #cdcdcd;
    border-radius: 2px;
    display: flex;
    overflow: hidden;
  }

  input {
    flex: 1;
    border: none;
    font-size: 16px;
    line-height: 36px;
    height: 46px;
    padding: 0 24px;
  }

  button {
    cursor: pointer;
    background-color: #00b4b3;
    color: #ffffff;
    font-size: 16px;
    padding: 0 24px;
    border: none;
  }

  .view {
    padding: 120px 12px 48px;
  }

  .searching, .error {
    background: #ffffff;
    text-align: center;
    max-width: 546px;
    padding: 12px 24px;
    border: 1px solid #cdcdcd;
    border-radius: 2px;
    margin: 0 auto;
  }

  .error {
    color: red;
    border-color: red;
  }
</style>
