<template>
  <div class="text-white text-base">
    <img
      src="https://www.arcgis.com/sharing/rest/content/items/5d40534d091540de826778706d00c04e/resources/1574707480127.jpeg?w=1600"
      class="imgdoida m-auto mb-10"
    />

    <form
      @submit.prevent=""
      class="flex items-center flex-col m-auto max-w-md mt-3"
    >
      <input
        type="text"
        v-model="name"
        placeholder="Nome do Link"
        class="bg-transparent mb-4 text-gray-300 focus:outline-none focus:shadow-outline border border-gray-300 rounded-lg py-2 px-4 block w-full appearance-none leading-normal"
      />
      <input
        type="text"
        v-model="link"
        @change="addLink"
        placeholder="Link"
        class="bg-transparent text-gray-300 focus:outline-none focus:shadow-outline border border-gray-300 rounded-lg py-2 px-4 block w-full appearance-none leading-normal"
      />

      <button
        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mt-3"
      >
        Salvar
      </button>
    </form>

    <div v-for="(link, index) in links" :key="index" class="mt-3">
      <div
        v-clipboard="link.link"
        @success="handleSuccess"
        @error="handleError"
        class="bg-gray-200 text-gray-800 mb-2 p-1 rounded flex items-center max-w-md m-auto justify-center"
      >
        <p class="mr-4">{{ link.name }}</p>
        <button :title="link.link">
          <img src="@/assets/clippy.svg" width="20" height="20" />
        </button>
      </div>
    </div>
  </div>
</template>
<script>
import { defineComponent, ref, watchEffect } from "vue";
import { clipboard } from "vue-clipboards";

const STORAGE_KEY = "links";
const storage = {
  fetch: function () {
    let links = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
    return links;
  },
  save: function (todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
  },
};
export default defineComponent({
  directives: { clipboard },
  data() {
    return {
      data: [{ name: "T", link: "https://" }],
      copyData: "copy data",
    };
  },
  setup() {
    let name = ref("");
    let link = ref("");
    let links = ref(storage.fetch());
    // let links = ref([]);

    // console.log(doido.value);

    watchEffect(() => {
      storage.save(links.value);
    });

    function addLink() {
      links.value.push({
        name: name.value,
        link: link.value,
      });
      link.value = "";
      name.value = "";
    }

    function deleteLink(index) {
      return links.value.splice(index, 1);
    }

    function handleSuccess(e) {
      console.log(e);
    }
    function handleError(e) {
      console.log(e);
    }

    return {
      link,
      name,
      addLink,
      links,
      deleteLink,
      handleSuccess,
      handleError,
    };
  },
});
</script>

<style lang="postcss">
.imgdoida {
  width: 100px;
  height: 100px;
}
</style>
