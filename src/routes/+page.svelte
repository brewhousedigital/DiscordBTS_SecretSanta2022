<script>
  import { onMount } from 'svelte';
  import { fade } from 'svelte/transition';

  const storageKeyName = "presents";

  let isPageReady = false;

  let presents = [
    {day: "1", url: "", status: 0},
    {day: "2", url: "", status: 0},
    {day: "3", url: "", status: 0},
    {day: "4", url: "", status: 0},
    {day: "5", url: "", status: 0},
    {day: "6", url: "", status: 0},
    {day: "7", url: "", status: 0},
    {day: "8", url: "", status: 0},
    {day: "9", url: "", status: 0},
    {day: "10", url: "", status: 0},
    {day: "11", url: "", status: 0},
    {day: "12", url: "", status: 0},
    {day: "13", url: "", status: 0},
    {day: "14", url: "", status: 0},
    {day: "15", url: "", status: 0},
    {day: "16", url: "", status: 0},
    {day: "17", url: "", status: 0},
    {day: "18", url: "", status: 0},
    {day: "19", url: "", status: 0},
    {day: "20", url: "", status: 0},
    {day: "21", url: "", status: 0},
    {day: "22", url: "", status: 0},
    {day: "23", url: "", status: 0},
    {day: "24", url: "", status: 0},
    {day: "25", url: "", status: 0},
  ]

	let modalContent = {};

  const handleOpenPresent = (day) => {
    const thisPresent = presents.map(item => {
      if(item.day === day) {
        item.status = 1;
      }

      return item
    })

    presents = [...thisPresent];
    updateStorage();
  }

  const handleModal = (day) => {
    modalContent = presents.find(item => item.day === day);

    const myModal = new bootstrap.Modal("#exampleModal")
		myModal.show();
	}

  const updateStorage = () => {
    localStorage.setItem(storageKeyName, JSON.stringify(presents))
  }

  onMount(() => {
    let checkForLocalStorage = localStorage.getItem(storageKeyName);

    if(checkForLocalStorage === null) {
      updateStorage();
      checkForLocalStorage = localStorage.getItem(storageKeyName);
    }

    const json = JSON.parse(checkForLocalStorage);

    presents = [...json];

    isPageReady = true;
  })
</script>


{#if isPageReady}
	<div class="container">
	<div class="grid" transition:fade="{{duration: 300}}">
		{#each presents as present}
			<div class="present">
				<!--<h1>Day {present.day}</h1>-->

				{#if present.status === 0}
					<button class="btn btn-present" on:click={() => {handleOpenPresent(present.day)}}>
						<img src="/present.png" alt="A present!" class="img-fluid" />
					</button>
				{/if}

				{#if present.status === 1}
					<button class="btn btn-present" on:click={() => {handleModal(present.day)}}>
						<img src="/present-open.png" alt="A present!" class="img-fluid btn-present-open" />
						<img src="https://i.giphy.com/media/IzXVviFZfeudTMxmOx/giphy.webp" alt="gif" class="img-fluid gift-gif" />
					</button>
				{/if}
			</div>
		{/each}
	</div>
	</div>
{:else}
	<div class="super-loader">
		<div class="spinner-border text-light" role="status">
			<span class="visually-hidden">Loading...</span>
		</div>
	</div>
{/if}



<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
	<div class="modal-dialog modal-dialog-centered">
		<div class="modal-content">
			<div class="modal-header">
				<!--<h1 class="modal-title fs-5" id="exampleModalLabel">Day {modalContent?.day}</h1>-->
				<button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
			</div>
			<div class="modal-body">
				<!--<img src={modalContent?.url} alt="BTS gif" class="img-fluid" />-->
				<img src="https://i.giphy.com/media/IzXVviFZfeudTMxmOx/giphy.webp" alt="gif" class="img-fluid" />
			</div>
			<div class="modal-footer">
				<button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
			</div>
		</div>
	</div>
</div>


<style>
    .modal {
        --bs-modal-bg: #222;
        --bs-modal-border-color: transparent;
        --bs-modal-header-border-color: transparent;
        --bs-modal-footer-border-color: transparent;
		}
    .super-loader {
        width: 100vw;
        height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .grid {
        padding: 12px;
        display: grid;
        grid-template-columns: repeat(5, 1fr);
        grid-template-rows: repeat(5, 1fr);
    }

    .present {
        position: relative;
    }

    .present h1 {
        position: absolute;
        top: 0;
        left: 0;
				z-index: 2;
				background-color: red;
				padding: 4px 12px;
				line-height: 1em;
				font-weight: 700;
				border-radius: 4px;
    }

    .present:nth-of-type(even) h1 {
        background-color: green;
		}

		.btn-present {
				position: relative;
		}

    .btn-present-open {
        position: relative;
				z-index: 0;
    }

		.gift-gif {
				position: absolute;
				top: 0;
				left: 0;
				z-index: 4;
				padding: 48px;
		}
</style>