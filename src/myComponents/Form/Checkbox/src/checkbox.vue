<template lang="html">
  <div class="form-check__box">
    <input
      type="checkbox"
      name="form_checkbox"
      ref="checkbox"
      :id="`id${checkID}`"
      v-model.lazy="model"
      @click="handeleChange"
      :disabled="disable"
    >
    <label :for="`id${checkID}`" ref="label">
      <slot></slot>
    </label>
  </div>
</template>

<script>
import { addClass, addCssStyle } from '@/common/js/dom.js'

export default {
  name: 'bao-checkbox',
  data: () => ({
    checkID: 'checkbox'
  }),
  props: {
    value: {}, // value属性值就是父组件中v-model的值
    shape: { // 复选框的形状['circle', 'square']-圆形和方形两种
      type: String,
      default: 'circle'
    },
    disable: { // 是否禁用该复选框
      type: Boolean,
      default: false
    }
  },
  computed: {
    model: { // 双向绑定
      get() {
        return this.value
      },
      set(val) {
        this.$emit('input', val)
        this.$refs.checkbox && (this.$refs.checkbox.checked = this.model === this.value)
      }
    }
  },
  methods: {
    handeleChange() {
      this.$emit('change', this.model)
    }
  },
  created() {

  },
  mounted() {
    this.$nextTick(() => {
      let cssStyle = `.form-check__box label:before {border-radius: 0 !important;}`
      if(this.shape === 'square') {
        addCssStyle(cssStyle)
      }
      if(this.disable) {
        let labelDom = this.$refs.label
        addClass(labelDom, 'check_disabled')
      }
    })
  }
}
</script>

<style lang="less" scoped>
.form-check__box {
  position: relative;
  height: 30px;
}
.form-check__box input[type='checkbox'] {
  position: absolute;
  left: 0;
  top: 0;
  width: 20px;
  height: 20px;
  opacity: 0;
}
.form-check__box label {
  position: absolute;
  left: 30px;
  top: 0;
  height: 20px;
  line-height: 20px;
  cursor: pointer;
  -webkit-tap-highlight-color: transparent;
}
.form-check__box label:before {
  content: '';
  position: absolute;
  left: -30px;
  top: 0;
  width: 20px;
  height: 20px;
  border: 1px solid #ddd;
  border-radius: 50%;
  transition: all 0.3s ease;
  -webkit-transition: all 0.3s ease;
  -moz-transition: all 0.3s ease;
}

.form-check__box label:after {
  content: '';
  position: absolute;
  left: -22px;
  top: 3px;
  width: 6px;
  height: 12px;
  border: 0;
  border-right: 1px solid #fff;
  border-bottom: 1px solid #fff;
  background: #fff;
  transform: rotate(45deg);
  -webkit-transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transition: all 0.3s ease;
  -webkit-transition: all 0.3s ease;
  -moz-transition: all 0.3s ease;
}
.form-check__box input[type='checkbox']:checked + label {
  color: #409eff;
}
.form-check__box input[type='checkbox']:checked + label:before {
  background: #409eff;
  border-color: #409eff;
}
.form-check__box input[type='checkbox']:checked + label:after {
  background: #409eff;
}
.check_disabled {
  cursor: not-allowed !important;
	background-image: none !important;
	filter: opacity(.58) !important;
}
</style>
