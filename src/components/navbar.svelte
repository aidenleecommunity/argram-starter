<script>
  // imports
  import { profile } from "../store.js";
  import { Othent } from "permawebjs/auth";

  // variable to store appended user first and last name
  $: name = $profile ? $profile.given_name + " " + $profile.family_name : "";

  async function handleConnect() {
    $profile = await Othent.logIn();
  }

  async function handleDisconnect() {
    await Othent.logOut();
    $profile = null;
  }
</script>

<div class="navbar bg-base-100 drop-shadow-md mb-10">
  <div class="navbar-start">
    <div class="dropdown dropdown-hover">
      <button class="btn btn-square btn-ghost">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          class="inline-block w-5 h-5 stroke-current"
          ><path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M4 6h16M4 12h16M4 18h16"
          /></svg
        >
      </button>
      <!-- dropdown for switching pages -->
      <ul class="dropdown-content menu p-2 shadow bg-base-100 rounded-box w-52">
        <li><a href="/upload">Upload</a></li>
        <li><a href="/view">View</a></li>
      </ul>
    </div>
  </div>
  <div class="navbar-center md:flex">
    <a href="/" class="btn btn-ghost normal-case text-2xl">ArGram</a>
  </div>
  <div class="navbar-end md:flex gap-4">
    {#if $profile}
      <!-- displays connected user's name from variable 'name' -->
      <!-- doubles as the log out button calling handle disconnect on click -->
      <button class="btn btn-ghost" on:click={handleDisconnect}>{name}</button>
    {:else}
      <!-- connect button to handle log in -->
      <button on:click={handleConnect} class="btn btn-ghost">Connect</button>
    {/if}
  </div>
</div>
