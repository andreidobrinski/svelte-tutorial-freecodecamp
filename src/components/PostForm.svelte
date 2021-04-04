<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let editingPost;

  $: title = editingPost.title;
  $: body = editingPost.body;
  let loading = false;

  const apiBase = "https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev";

  async function onSubmit(event) {
    event.preventDefault();

    if (title.trim() === "" || body.trim() === "") {
      return;
    }

    loading = true;

    const newPost = { title, body };

    let url, method;
    if (editingPost.id) {
      url = `${apiBase}/post/${id}`;
      method = "PUT";
    } else {
      url = `${apiBase}/post`;
      method = "POST";
    }

    const res = await fetch(url, {
      method,
      body: JSON.stringify(newPost),
    });

    const post = await res.JSON();
    dispatch("postCreated", post);
    title = body = "";
    loading = false;
  }
</script>

{#if !loading}
  <form on:submit={onSubmit}>
    <div class="input-field">
      <label for="title">Title</label>
      <input type="text" bind:value={editingPost.title} />
    </div>
    <div class="input-field">
      <label for="body">Body</label>
      <input type="text" bind:value={editingPost.body} />
    </div>
    <button type="submit" class="waves-effect waves-light btn">
      {editingPost.id ? "Update" : "Add"}
    </button>
  </form>
{:else}
  <div class="progress">
    <div class="indeterminate" />
  </div>
{/if}

<style>
  form {
    margin: 50px;
  }
  .progress {
    margin: 100px 0;
  }
</style>
