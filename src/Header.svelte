<script>
  export let page = "home"

  const toolsPage = () => {page = "tools"}
  const skateLessonsPage = () => {page = "skateLessons"}
  const philosophyPage = () => {page = "philosophy"}
  const forumPage = () => {page = "forum"}
</script>


<div class="header">
  <div class="p-4 row align-items-center title-row">
    <h1 class="col">ProgE</h1>
    <h5 class="col text-end align-middle">(469) 258-6258</h5>
  </div>
  <div class="row align-items-center nav">
    <span class="p-2 col text-center nav-option" on:click={philosophyPage}>Philosophy</span>
    <span class="p-2 col text-center nav-option" on:click={toolsPage}>Tools</span>
    <!--span class="p-2 col text-center nav-option" on:click={skateLessonsPage}>Skate Lessons</span-->
    <span class="p-2 col text-center nav-option" on:click={forumPage}>Forum</span>
  </div>
</div>

<style>
  .header {
    background: #eeeeee;
  }

  .title-row {
    border-bottom: black 1px solid;
  }

  .row{
    margin-right: 0;
    margin-left: 0;
  }

  .nav-option:hover {
    background: #ccc;
    transition: all .3s;
  }
</style>