<script>
  import { Link } from "svelte-routing"
</script>

<nav class="sm:max-w-screen-xl  m-auto ">
  <div
    class="h-10 mx-5 border-2 border-strange-gray mb-10 pt-2 text-center space-x-5 rounded-b-xl"
  >
    <Link to="admin/tours">
      <button class="w-16 border border-darkest-gray rounded-md"> Туры </button>
    </Link>
    <Link to="admin/clients">
      <button class="w-16 border border-darkest-gray rounded-md">
        Клиенты
      </button>
    </Link>
    <Link to="admin/applications">
      <button class="w-16 border border-darkest-gray rounded-md">
        Заявки
      </button>
    </Link>
    <Link to="admin/booking">
      <button class="w-16 border border-darkest-gray rounded-md">
        Брони
      </button>
    </Link>
  </div>
</nav>
