<script>
  import VerifiedIcon from './VerifiedIcon.svelte';

  export let artist;
  let following = false;
  let numberFormatter = new Intl.NumberFormat('en-US');

  function toggleFollowing() {
    following = !following;
  }
</script>

<div class="background" style="background-image: url({artist.image_url})">
  <div class="blur">
    <div class="container">
      <div class="thumbnail">
        <img src={artist.thumb_url} alt="{artist.anem} thumbnail image"/>
      </div>
      <div class="info">
        <div class="name">
          <h1>{artist.name}</h1>
          <VerifiedIcon/>
        </div>
        <div class="details">
          <p>{numberFormatter.format(artist.tracker_count)} Followers</p>
          <p class="divider">•</p>
          <p>{numberFormatter.format(artist.upcoming_event_count)} Upcoming {artist.upcoming_event_count == 1 ? "Concert" : "Concerts"}</p>
        </div>
      </div>

      <button on:click={toggleFollowing}>
        {following ? 'Following' : 'Follow'}
      </button>
    </div>
  </div>
</div>

<style>

  .background {
    background-repeat: no-repeat;
    background-size: cover;
    max-width: 546px;
    border-radius: 2px;
    margin: 0 auto;
    overflow: hidden;
  }

  .blur {
    backdrop-filter: blur(10px);
  }

  .container {
    background-color: rgba(0,0,0,.4);
    color: #ffffff;
    display: flex;
    flex-direction: column;
    align-content: center;
    align-items: center;
    justify-content: center;
    min-height: 195px;
    padding: 30px 50px;
  }

  .thumbnail {
    padding: 0 0 20px;
  }

  .thumbnail img {
    height: 150px;
    width: 150px;
    border-radius: 50%;
  }

  .info {
    text-align: center;
    padding: 0 0 30px;
  }

  .info h1 {
    font-size: 26px;
    margin: 0 6px 0 0;
  }

  .info p {
    line-height: 24px;
    margin: 0;
  }

  .name {
    display: flex;
    align-content: center;
    align-items: center;
    padding-bottom: 5px;
  }

  .details {
    padding: 5px 0 0;
  }

  .divider {
    display: none;
  }

  button {
    cursor: pointer;
    background-color: #00b4b3;
    color: #ffffff;
    font-size: 16px;
    line-height: 40px;
    height: 40px;
    min-width: 162px;
    padding: 0 24px;
    border: none;
    border-radius: 2px;
  }

  @media only screen and (min-width: 1024px) {
    .background {
      max-width: 1000px;
    }

    .container {
      flex-direction: row;
    }

    .thumbnail, .info {
      padding: 0 30px 0 0;
    }

    .info {
      flex: 1;
      text-align: left;
    }

    .details {
      display: flex;
    }

    .divider {
      display: block;
      padding: 0 6px;
    }
  }
</style>
