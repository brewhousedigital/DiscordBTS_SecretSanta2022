<script>
	export let present = {};

  const getImage = (present) => {
    return `/presents/${present.day}.${present.type}`
  }
</script>


{#if present.type === "mp4"}
	<video width="100" height="100" autoplay muted loop class="img-fluid">
		<source src={getImage(present)} type="video/mp4">
	</video>
{:else}
	<img src={getImage(present)} alt="gif" class="img-fluid" />
{/if}


<style>
	video {
			width: 100%;
	}
</style>