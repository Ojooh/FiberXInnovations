<script>
    import { RouterView } from 'vue-router';
    import { content_data } from './content/en_data';
    import HelperFunctions from './utils/helpers';
    import CommonMethods from './utils/common_methods';

    import Loader from './components/Base/Loader.vue';
    import Alert from './components/Base/Alert.vue';
    import NavBar from './components/Base/NavBar.vue';
    import FooterArea from './components/Base/FooterArea.vue';
    import ModalBase from './components/Base/ModalBase.vue';
    import Scroller from './components/Base/Scroller.vue';

    export default {
        data() { 
          return {
              content_data, C_METH: new CommonMethods(),
              hp: new HelperFunctions(), content: content_data.App,
              loading: false, state: false, modal_state: false, white_page: false,
              msg: '', status : '', modal_status: '', 
              load_string : 'LOADING', store_id: 'STR-00001',
              y: 0,
          }
        },
        components: { RouterView, Loader, Alert, NavBar, FooterArea, ModalBase, Scroller },
        methods: {
          showAlert(status, msg, time=2000, reload=false, remove=false,  replace_url='') {
            this.status = status;
            this.msg = msg;
            this.state = true;
            const vim = this;
            const clean_url = replace_url.replace('/', '')

            if (remove) { 
              setTimeout(()=> { 
                vim.state = false;
            
                if (replace_url.length > 0) { location.replace(`${window.location.origin}/${clean_url}`);}
                else if (reload) { vim.$router.go(); }
            }, time) }
          },

          showModal(status, data={}) {
            if (data && Object.keys(data).length > 0) { this.$refs.ModalBase.modal_data = data; }
            this.modal_state = true;
            this.modal_status = status.replace('_', '');  
          },
        },
        created: async function() {
        },
        mounted: function() {
          const mainSection = document.getElementById('ScrollArea');
          const vim = this;
          mainSection.addEventListener('scroll', function() { vim.y = mainSection.scrollTop; });

        }
    }
</script>

<template>
  <section id="AppBody" class="relative w-screen h-screen overflow-x-hidden">
      <ModalBase :modal_status="modal_status" :modal_state="modal_state" ref="ModalBase" />
      <Alert :state="state" :msg="msg" :status="status" />
      <Loader :state="loading" :loadingString="load_string" />
      <header>
          <NavBar :y="y" :white_page="white_page"  :content="content.NavBar"  />
      </header>
      <main id="ScrollArea" :class="y <= 4 ? 'h-[85vh]' : 'h-[100vh]'" class="w-full">
          <RouterView />
      </main>
      <footer>
          <FooterArea  :content="content.FooterArea" :current_year="new Date().getFullYear()" />
      </footer>
      <Scroller :content="content.Scroller" />
  </section>
</template>