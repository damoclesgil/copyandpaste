<template>
  <div class="text-white text-base">
    <!-- <img
      src="@/assets/logo.svg"
      class="imgdoida m-auto mb-10"
      title="Chupa Cabra"
      alt="clipboard"
    /> -->

    <IconClipBoard/>
    <form
      submit.prevent
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
      <span :id="`i${index}`" class="fontepequna">{{ link.link }} </span>

      <div
        class="flex flex items-center justify-center max-w-md m-auto text-center"
      >
        <p>{{ link.name }}</p>
        <button
          :title="link.link"
          :data-clipboard-target="`#i${index}`"
          class="clipboard p-1 rounded-sm ml-1"
        >
          <img src="@/assets/clippy.svg" width="20" height="20" />
        </button>
      </div>
    </div>
  </div>
</template>
<script>
import { defineComponent, ref, watchEffect } from "vue";
import IconClipBoard from './Icons/IconClipboard'
import Clipboard from "clipboard";
new Clipboard(".clipboard");

const STORAGE_KEY = "links";
const storage = {
  fetch: function() {
    let links = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
    return links;
  },
  save: function(todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
  }
};
export default defineComponent({
  components: {
    IconClipBoard,
  },
  setup() {
    let name = ref("");
    let link = ref("");
    let links = ref(storage.fetch());

    watchEffect(() => {
      storage.save(links.value);
    });

    function addLink() {
      links.value.push({
        name: name.value,
        link: link.value
      });
      link.value = "";
      name.value = "";
    }

    function deleteLink(index) {
      return links.value.splice(index, 1);
    }

    return {
      link,
      name,
      addLink,
      links,
      deleteLink
    };
  }
});
</script>

<style lang="postcss">
.imgdoida {
  width: 100px;
  height: 100px;
}

.fontepequna {
  font-size: 0rem;
}
</style>
