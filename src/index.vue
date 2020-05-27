<template>
  <div class="component signature">
    <VueSignaturePad :width='width' :height='height' :options='options' ref="signaturePad" />
    <div v-show="showControl" class="controlsss">
      <button @click="undo">撤销</button>
      <button @click="clear">清除</button>
      <button @click="setPenColor('red')">红色画笔</button>
      <button @click="setPenColor('blue')">蓝色画笔</button>
      <button @click="setPenColor('yellow')">黄色画笔</button>
      <button @click="setDotSize(1)">较细笔触</button>
      <button @click="setDotSize(5)">较粗笔触</button>
    </div>
  </div>
</template>

<script>
  import {VueExtend} from 'godspen-lib'
  import Vue from 'vue'
  import SignaturePad from 'vue-signature-pad'
  import debounce from 'lodash/debounce'

  Vue.use(SignaturePad)

  export default {
    mixins: [VueExtend.mixin],
    name: 'hand-painter',
    label: process.env.LABEL,
    style: process.env.STYLE,
    leaf: true,
    props: {
      color: {
        type: String,
        editor: {
          type: 'color',
          label: '画笔颜色'
        },
        default: 'red'
      },
      dot: {
        type: Number,
        editor: {
          type: 'number',
          label: '笔触大小,1-50'
        },
        default: 1.5
      },
      showControl: {
        type: Boolean,
        editor: {
          label: '显示演示控制按钮'
        },
        default: true
      }
    },
    data () {
      return {
        width: '100%',
        height: '100%',
        custom: {
          penColor: null,
          dotSize: null,
        }
      }
    },
    computed: {
      wh () {
        return `${this.width}_${this.height}`
      },
      penColor () {
        return this.custom.penColor || this.color
      },
      dotSize () {
        return this.custom.dotSize || this.dot
      },
      minWidth () {
        return this.dotSize / 3
      },
      maxWidth () {
        return this.dotSize / 3 * 5
      },
      options () {
        return {
          penColor: this.penColor,
          dotSize: this.dotSize,
          minWidth: this.minWidth,
          maxWidth: this.maxWidth,
        }
      }
     },
    editorMethods: {
      setDotSize: {
        label: '设置笔触大小',
        params: [
          {
            label: '大小，输入数字 1-50',
            type: 'number'
          }
        ]
      },
      setPenColor: {
        label: '设置画笔颜色',
        params: [
          {
            label: '颜色',
            type: 'string'
          }
        ]
      },
      undo: {
        label: '撤销一步',
        params: []
      },
      clear: {
        label: '清空画布',
        params: []
      },
    },
    watch: {
      wh: debounce(function () {
        this.resize()
      }, 500, { leading: true, trailing: true }),
    },
    mounted () {
      this.watchParent()
    },
    methods: {
      watchParent () {
        const $parent = process.env.NODE_ENV !== 'production' ? '$parent' : '$parent.nodeInfo'
        this.$watch(`${$parent}.style.height`, () => this.$nextTick(() => {
          this.height = this.$el.clientHeight + 'px'
        }), { immediate: true })
        this.$watch(`${$parent}.style.width`, () => this.$nextTick(() => {
          this.width = this.$el.clientWidth + 'px'
        }), { immediate: true })
      },
      undo () {
        const $pad = this.$refs.signaturePad
        if (!$pad) return
        $pad.undoSignature()
      },
      resize () {
        const $pad = this.$refs.signaturePad
        if (!$pad) return
        $pad.resizeCanvas()
      },
      save () {
        const $pad = this.$refs.signaturePad
        if (!$pad) return
        const { isEmpty, data } = $pad.saveSignature()
        return { isEmpty, data }
      },
      clear () {
        const $pad = this.$refs.signaturePad
        if (!$pad) return
        $pad.clearSignature()
      },
      lock () {
        const $pad = this.$refs.signaturePad
        if (!$pad) return
        $pad.lockSignaturePad()
      },
      setPenColor (color) {
        this.custom.penColor = color
      },
      setDotSize (size) {
        this.custom.dotSize = size
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus" type="text/stylus" scoped>
  .component {
    width: 100%;
    height: 100%;
    position relative
    .controlsss {
      position absolute
      bottom: 0
      left: 0
    }
  }
</style>
