<script>
  // TODO markdown description
  // npm i marked
  // import marked from 'marked'
  // {@html marked(description)}
  import { getContext } from "svelte"

  import { TourView } from "./"

  export let tour

  const app = getContext("firebase").getFirebase()
  const firestore = app.firestore()

  function deleteTour() {
    let tourRef = firestore.doc(`/Tours/${tour.id}`)

    tourRef
      .delete()
      .then(() => console.log("=== TourView: tour deleted "))
      .catch((error) => console.error("=== TourView: tour NOT deleted ", error))
  }

  let view = false
</script>

<tr>
  <td class="px-6 py-4 whitespace-nowrap">
    <div class="flex items-center">
      <div class="flex-shrink-0 h-10 w-10">
        <img class="h-10 w-10 rounded-full" src={tour.imageURL} alt="" />
      </div>
      <div class="ml-4">
        <div class="text-sm font-medium text-gray-900">
          <span>{tour.name}</span>
        </div>
      </div>
    </div>
  </td>
  <td class="px-6 py-4 whitespace-nowrap">
    <div class="text-sm text-gray-900">
      <span>{tour.country}</span>
    </div>
  </td>
  <td class="px-6 py-4 whitespace-nowrap">
    <div class="text-sm text-gray-900">
      <span>{tour.area}</span>
    </div>
  </td>
  <td class="px-6 py-4 whitespace-nowrap">
    <div class="text-sm text-gray-900">
      <span>{tour.city}</span>
    </div>
  </td>
  <td class="px-6 py-4 whitespace-nowrap">
    <div class="text-sm text-gray-900">
      <span>{tour.hotel}</span>
    </div>
  </td>

  <td
    class="px-6 inline-flex text-xs leading-5 font-semibold rounded-full whitespace-nowrap"
  >
    {#if tour.visa}
      <div class="px-3 py-1 text-xs bg-blue-200 text-blue-800 rounded-full">
        да
      </div>
    {:else}
      <div class="px-3 py-1 text-xs bg-green-200 text-green-800 rounded-full">
        нет
      </div>
    {/if}
  </td>

  <td class="px-6 py-4 whitespace-nowrap">
    <div class="text-sm text-gray-900">
      <span>{tour.rating}</span>
    </div>
  </td>

  <td
    class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium flex flex-row"
  >
    <a
      href="#"
      class="text-indigo-600 hover:text-indigo-900"
      on:click={() => {
        view = true
      }}>Изменить</a
    >&nbsp;
    <a
      href="#"
      class="mx-2 text-red-600 hover:text-red-900"
      on:click={deleteTour}
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-6 w-6"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
        />
      </svg>
    </a>
  </td>
  {#if view}
    <TourView {tour} closeFunction={() => (view = false)} />
  {/if}
</tr>
