<script>
  // imports
  import {
    readContractWOthent,
    writeContractWOthent,
  } from "permawebjs/contract";
  import { profile } from "../store";
  import { onMount } from "svelte";

  // id variable to get the transaction id of an asset (post) from the view page
  export let id = "";

  // object to store likes information for a post
  let likes = {};

  async function likePost() {
    const res = await writeContractWOthent({
      othentFunction: "sendTransaction",
      data: {
        toContractId: id,
        toContractFunction: "likePost",
        txnData: {
          function: "likePost",
        },
      },
    });

    likes = await readLikes();
  }

  async function readLikes() {
    const res = await readContractWOthent({
      contractTxId: id,
    });

    return res.state["likes"];
  }

  onMount(async () => {
    likes = await readLikes();
  });
</script>

<section>
  <!-- Calculates number of keys object -->
  <!-- number keys corresponds to the number of likes for a post -->
  <p class="text-center">{Object.keys(likes).length}</p>
  <!-- button to like a post -->
  <!-- button is disabled if user has already liked post -->
  <button
    class="btn btn-block"
    disabled={Object.keys(likes).includes($profile.contract_id)}
    on:click|preventDefault={likePost}>Like</button
  >
</section>
