<template>
  <div ref="vaptcha">
    <div class="vaptcha-init-main">
      <div class="vaptcha-init-loading">
        <a href="/" target="_blank">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            width="48px"
            height="60px"
            viewBox="0 0 24 30"
            style="enable-background: new 0 0 50 50; width: 14px; height: 14px; vertical-align: middle"
            xml:space="preserve"
          >
            <rect x="0" y="9.22656" width="4" height="12.5469" fill="#CCCCCC">
              <animate attributeName="height" attributeType="XML" values="5;21;5" begin="0s" dur="0.6s" repeatCount="indefinite"></animate>
              <animate attributeName="y" attributeType="XML" values="13; 5; 13" begin="0s" dur="0.6s" repeatCount="indefinite"></animate>
            </rect>
            <rect x="10" y="5.22656" width="4" height="20.5469" fill="#CCCCCC">
              <animate attributeName="height" attributeType="XML" values="5;21;5" begin="0.15s" dur="0.6s" repeatCount="indefinite"></animate>
              <animate attributeName="y" attributeType="XML" values="13; 5; 13" begin="0.15s" dur="0.6s" repeatCount="indefinite"></animate>
            </rect>
            <rect x="20" y="8.77344" width="4" height="13.4531" fill="#CCCCCC">
              <animate attributeName="height" attributeType="XML" values="5;21;5" begin="0.3s" dur="0.6s" repeatCount="indefinite"></animate>
              <animate attributeName="y" attributeType="XML" values="13; 5; 13" begin="0.3s" dur="0.6s" repeatCount="indefinite"></animate>
            </rect>
          </svg>
        </a>
        <span class="vaptcha-text">Vaptcha启动中...</span>
      </div>
    </div>
  </div>
</template>

<script>
import { loadV2Script, optionsMerge } from "./util";

export default {
  name: "Vaptcha",
  props: {
    vid: {
      type: String,
      default: ""
    },
    type: {
      type: String,
      default: "click"
    },
    scene: {
      type: String,
      default: ""
    },
    vpStyle: {
      type: String,
      default: ""
    },
    color: {
      type: String,
      default: ""
    },
    lang: {
      type: String,
      default: ""
    }
  },
  data() {
    return {
      vaptcha: null
    };
  },
  mounted() {
    const config = {
      container: this.$refs.vaptcha,
      style: this.vpStyle
    };
    Object.assign(config, this.$props);
    this.$vaptcha && optionsMerge(config, this.$vaptcha.options);
    loadV2Script().then(() => {
      window.vaptcha(config).then(obj => {
        obj.listen("pass", () => {
          this.$emit("input", obj.getToken());
        });
        obj.render();
        this.vaptcha = obj;
      });
    });
  },
  destroyed() {
    const { vaptcha } = this;
    vaptcha && vaptcha.destroy();
  }
};
</script>

<style lang="less">
.vaptcha-init-main {
  display: table;
  width: 100%;
  height: 100%;
  min-height: 36px;
  background-color: #eeeeee;
  .vaptcha-init-loading {
    display: table-cell;
    vertical-align: middle;
    text-align: center;
    a {
      display: inline-block;
      width: 18px;
      height: 18px;
      img {
        vertical-align: middle;
      }
    }
    .vaptcha-text {
      font-family: sans-serif;
      font-size: 12px;
      color: #cccccc;
      vertical-align: middle;
    }
  }
}
</style>
