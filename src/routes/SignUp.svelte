<script>
  let background = "img/mayak_zakat.jpg"

  import { User } from "sveltefire"
  import "firebase/auth"
  import "firebase/firestore"
  import { getContext } from "svelte"
  import { userData } from "../components/store"
  import { navigate, Link } from "svelte-routing"

  const app = getContext("firebase").getFirebase()
  const firestore = app.firestore()
  const auth = app.auth()

  let email, password, homeButton
  let yes = false

  const DEBUG = true

  const storeUser = (event) => {
    const user = event.detail.user

    console.log("Юзер авторизовался", user)
    if (user && !user.lastLoginAt) {
      // user authorized and info came from firebase server
      const { uid, email } = user

      let userRef = firestore.doc(`/Users/${uid}`)
      userRef.get().then((doc) => {
        DEBUG && console.log("DEBUG: requesting user data")

        if (doc && doc.exists) {
          // user is old
          let data = doc.data()
          userData.set(data)

          DEBUG && console.log("user is old", $userData)
          homeButton.click()
          // navigate('/', { replace: true })
        } else {
          // user is new
          let data = { uid, email }

          userRef.set(data).then(() => {
            userData.set(data)

            DEBUG && console.log("user is new", $userData)
            homeButton.click()
            // navigate('/', { replace: true })
          })
        }
      })
    }
  }

  function createUser(event) {
    // TODO SIGNUP проверить заполненность полей и совпадение паролей
    event.preventDefault()
    auth.createUserWithEmailAndPassword(email, password)
  }
</script>

<User persist={localStorage} let:user on:user={storeUser} let:auth>
  <!-- TODO SIGNUP fix navigate function -->
  <button
    class="text-white"
    on:click={() => navigate("profile", { replace: true })}>ПРОФИЛЬ</button
  >
  <Link class="text-white" to="/"
    ><span bind:this={homeButton}> ДОМОЙ </span>
  </Link>
  <button class="text-white" on:click={() => homeButton.click()}>_ДОМОЙ</button>

  <div slot="signed-out">
    <section
      class="relative py-20 2xl:py-40 bg-local bg-cover"
      style="background-image: url('{background}')"
    >
      <div
        class="absolute top-0 left-0 lg:bottom-0 h-full lg:h-auto w-full lg:w-8/12  pt-11 lg:overflow-hidden"
      >
        <img
          class="hidden lg:block mt-64 ml-112"
          src="zospace-assets/lines/circle.svg"
          alt=""
        />
      </div>
      <div class="relative container px-4 mx-auto">
        <div class="max-w-5xl mx-auto">
          <div class="flex flex-wrap items-center -mx-4">
            <div class="w-full lg:w-1/2 px-4 mb-16 lg:mb-0">
              <div class="lg:max-w-md md:w-full">
                <span class="text-lg text-pale-white font-bold"
                  >Регистрация учетной записи</span
                >
                <h2
                  class="mt-8 mb-12 md:text-5xl xs:text-2xl font-bold font-heading text-white"
                >
                  Начните пользоваться дополнительными функциями сервиса
                </h2>
                <p class="text-lg text-gray-200">
                  <span>Вы не пожалеете</span>
                </p>
              </div>
            </div>
            <div class="w-full lg:w-1/2 px-4">
              <div
                class="lg:max-w-lg mx-auto px-6 lg:px-20 py-12 lg:py-24 bg-strange-gray rounded-2xl"
              >
                <form action="#">
                  <h3 class="mb-10 text-2xl text-white font-bold font-heading">
                    Регистрация аккаунта
                  </h3>
                  <div class="flex items-center pl-6 mb-3 bg-white rounded-2xl">
                    <span class="inline-block pr-3 border-r border-gray-50">
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
                          stroke-width="1"
                          d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"
                        />
                      </svg>
                    </span>
                    <input
                      bind:value={email}
                      class="w-full pr-6 pl-4 py-4 font-bold placeholder-gray-400 placeholder-opacity-75 rounded-r-full focus:outline-none"
                      type="email"
                      placeholder="best_user@example.tru"
                    />
                  </div>
                  <div class="flex items-center pl-6 mb-3 bg-white rounded-2xl">
                    <span class="inline-block pr-3 border-r border-gray-50">
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
                          stroke-width="1"
                          d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z"
                        />
                      </svg>
                    </span>
                    <input
                      bind:value={password}
                      class="w-full pr-6 pl-4 py-4 font-bold placeholder-gray-400 placeholder-opacity-75 rounded-r-full focus:outline-none"
                      type="password"
                      placeholder="Пароль"
                    />
                  </div>
                  <div class="flex items-center pl-6 mb-6 bg-white rounded-2xl">
                    <span class="inline-block pr-3 border-r border-gray-50">
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
                          stroke-width="1"
                          d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z"
                        />
                      </svg>
                    </span>
                    <input
                      class="w-full pr-6 pl-4 py-4 font-bold placeholder-gray-400 placeholder-opacity-75 rounded-r-full focus:outline-none"
                      type="password"
                      placeholder="Повторите пароль"
                    />
                  </div>
                  <div class="inline-flex mb-10">
                    <input class="mr-4" type="checkbox" bind:checked={yes} />
                    <p class="-mt-2 text-sm text-pale-white">
                      Регистрируясь, Вы соглашаетесь с нашими <a
                        class="text-gray-400"
                        href="#">Условиями, Политикой обработки данных</a
                      >
                      и <a class="text-gray-400" href="#">Файлами cookie.</a>
                    </p>
                  </div>
                  <button
                    on:click={createUser}
                    disabled={!yes}
                    class="py-4 w-full bg-red-600 hover:bg-red-500 active:bg-red-400 text-strange-black font-bold rounded-2xl transition duration-200"
                    >Начнем</button
                  >
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</User>
