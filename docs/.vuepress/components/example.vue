<template>
  <div class="example" :style="{ width, height: height + 'px' }">
    <div class="mask" v-show="!showIframe"></div>
    <iframe :id="iframeId" class="iframe" :src="iframeLink" />
  </div>
</template>

<script>
export default {
  props: {
    href: {
      type: String,
    },
    width: {
      type: String | Number,
      default: '100%',
    },
    height: {
      type: Number,
      default: 600,
    },
    // 一个md中有多个iframe时，需要进行区分
    iframeId: {
      type: String,
      default: 'example-iframe'
    }
  },
  data() {
    return {
      iframeLink: '',
      showIframe: false,
    };
  },
  computed: {
    iframeHeight() {
      if (this.height) {
        return 600;
      }
      return 600;
    },
  },
  mounted() {
    const { base } = this.$router.options;
    let host = '';
    if (process.env.NODE_ENV === 'development') {
      host = 'http://localhost:3000';
    }
    if (this.href) {
      this.iframeLink = host + base.slice(0, -1) + this.href;
    } else {
      this.iframeLink =
        host +
        location.pathname.replace('guide', 'examples/#').replace('.html', '');
    }
    document.getElementById(this.iframeId).onload = () => {
      this.showIframe = true;
    };
  },
};
</script>
<style scoped>
.example {
  position: relative;
}
.mask,
.iframe {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
}
.mask {
  background-color: rgb(255, 255, 255);
  animation: example-mask-animation 2s linear 0s infinite alternate;
  z-index: 1;
}
.iframe {
  border: none;
  outline: none;
  z-index: 0;
}
@keyframes example-mask-animation {
  from {
    background-color: rgb(231, 230, 230);
  }
  to {
    background-color: rgb(253, 253, 253);
  }
}
</style>