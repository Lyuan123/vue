<template>
  <div class="container" id="app">
    <global-header :user="currentUser"></global-header>
    <h1>{{ error.message }}</h1>
    <loader v-if="isLoading" text="拼命加载中"></loader>
    <router-view></router-view>
    <footer class="text-center py-4 text-secondary bg-light mt-6">
      <small>
        <ul class="list-inline mb-0">
          <li class="list-inline-item">© 2020 者也专栏</li>
          <li class="list-inline-item">课程</li>
          <li class="list-inline-item">文档</li>
          <li class="list-inline-item">联系</li>
          <li class="list-inline-item">更多</li>
        </ul>
      </small>
    </footer>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, ref, computed, watch } from "vue";
import { useStore } from "vuex";
import "bootstrap/dist/css/bootstrap.min.css";
import GlobalHeader from "./components/GlobalHeader.vue";
import Loader from "./components/Loader.vue";
import createMessage from "./components/createMessage";

export default defineComponent({
  name: "App",
  components: {
    GlobalHeader,
    Loader,
  },
  setup() {
    const store = useStore();
    const currentUser = computed(() => store.state.user);
    const isLoading = computed(() => store.state.loading);
    const error = computed(() => store.state.error);
    watch(
      () => error.value.status,
      () => {
        const { status, message } = error.value;
        if (status && message) {
          createMessage(message, "error");
        }
      }
    );
    return {
      currentUser,
      isLoading,
      error,
    };
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
