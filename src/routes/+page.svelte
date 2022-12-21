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

  const showModal = () => {
    const myModal = new bootstrap.Modal("#exampleModal")
    myModal.show();
	}

  const handleOpenPresent = (day) => {
    const thisPresent = presents.map(item => {
      if(item.day === day) {
        item.status = 1;

        setTimeout(() => {
          showModal();
				}, 200)
      }

      return item
    })

    presents = [...thisPresent];
    updateStorage();
  }

  const handleModal = (day) => {
    modalContent = presents.find(item => item.day === day);

    showModal();
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
	<div class="custom-container">
		<h1>❄ Merry Christmas, TV ❄</h1>

		<div class="grid" transition:fade="{{duration: 300}}">
			{#each presents as present}
				<div class="present">
					{#if present.status === 0}
						<button class="btn btn-present" on:click={() => {handleOpenPresent(present.day)}}>
							<h2>Day {present.day}</h2>
						</button>
					{/if}

					{#if present.status === 1}
						<button class="btn btn-present" on:click={() => {handleModal(present.day)}}>
							<span class="d-block">
								<img src="https://i.giphy.com/media/IzXVviFZfeudTMxmOx/giphy.webp" alt="gif" class="img-fluid gift-gif" />
							</span>
						</button>
					{/if}
				</div>
			{/each}
		</div>

		<p class="text-center">Love, Squid</p>
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
		.custom-container {
				text-align: center;
				max-width: 100%;
				padding: 24px 0;
		}

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
				grid-gap: 24px;
    }

    .btn-present {
        position: relative;
				display: flex;
				height: 100px;
				align-items: center;
				justify-content: center;
				color: #fff;
				border: 1px solid #fff;
				width: 100%;
        overflow: hidden;
    }

		.present h2 {
				color: #fff;
		}

    .btn-present img {
        display: block;
    }

		@media screen and (min-width: 800px) {
        .custom-container {
            max-width: 75vw;
        }
		}

		@media screen and (min-width: 1200px) {
        .custom-container {
            max-width: 50vw;
        }
		}
</style>