<script>
  import { onMount } from "svelte";

  import PostForm from "../components/PostForm.svelte";

  const apiBase = "https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev";

  let posts = [];
  let editingPost = {
    body: "",
    title: "",
    id: null,
  };

  onMount(async () => {
    const res = await fetch(apiBase + "/posts");
    posts = await res.json();
  });

  function editPost(post) {
    editingPost = post;
  }

  function deletePost(id) {
    if (confirm("Are you sure?")) {
      fetch(`${apiBase}/post/${id}`, {
        method: "DELETE",
      })
        .then((res) => {
          return res.json();
        })
        .then(() => {
          posts = posts.filter((p) => p.id !== id);
        });
    }
  }

  function addPost({ detail: post }) {
    posts = [post, ...posts];
  }
</script>

<div class="row">
  <div class="col s6">
    <PostForm on:postCreated={addPost} {editingPost} />
  </div>
</div>
<div class="row">
  {#if posts.length === 0}
    <h1>loading posts...</h1>
  {:else}
    {#each posts as post}
      <div class="col s6">
        <div class="card">
          <div class="card-content">
            <p class="card-title">{posts.title}</p>
            <p class="timestamp">{post.createdAt}</p>
            <p>{post.body}</p>
          </div>
          <div class="card-action">
            <a href="#" on:click={() => editPost(post)}>Edit</a>
            <a href="#" class="delete-btn" on:click={() => deletePost(post.id)}>
              Delete
            </a>
          </div>
        </div>
      </div>
    {/each}
  {/if}
</div>

<style>
  .delete-btn {
    color: red !important;
  }
  .card .card-content .card-title {
    margin-bottom: 0;
  }
  .card .card-content p.timestamp {
    color: #999;
    margin-bottom: 10px;
  }
</style>
