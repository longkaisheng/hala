<template>
  <div class="ha-input-wrapper" :class="{'has-floating-label': floatingLabel == 'true'}">
    <label class="item item-input thin-border ha-input" :class="{'item-floating-label': floatingLabel == 'true'}">
      <span v-if="label != ''" class="input-label" :class="{'has-input': floatingLabel == 'true' && !!value}" v-text="label"></span>
      <!-- text -->
      <input v-if="type == 'text'" type="text" :placeholder="placeholder"
        ref="input" :value="value" @change="updateValue($event.target.value)">

      <!-- password -->
      <input v-if="type == 'password'" type="password" :placeholder="placeholder"
        ref="input" :value="value" @change="updateValue($event.target.value)">

      <!-- email -->
      <input v-if="type == 'email'" type="email" :placeholder="placeholder"
        ref="input" :value="value" @change="updateValue($event.target.value)">

      <!-- tel -->
      <input v-if="type == 'tel'" type="tel" :placeholder="placeholder"
        ref="input" :value="value" @change="updateValue($event.target.value)">
    </label>

    <span class="input-clear" :class="{'active': showClearButton}" @click="clear()"></span>
  </div>
</template>
<script type="text/ecmascript-6">
  import config from '../config';
  export default {
    name: config.prefix + 'Input',
    props: {
      type: {
        type: String,
        validator(val) {
          return val == 'text' || val == 'password' || val == 'email' || val == 'tel'
        },
        default: 'text'
      },
      label: {
        type: String,
        default: ''
      },
      placeholder: {
        type: String,
        default: ''
      },
      value:{
        type: [String, Number],
        required: true
      },
      floatingLabel: {
        type: String,
        default: 'false',
        validator(val) {
          return val == 'true' || val == 'false'
        }
      }
    },

    data() {
      return {
        showClearButton: false
      }
    },

    methods: {
      clear() {
        this.$refs.input.value = ''
        this.$emit('input', '')
        this.showClearButton = false
      },

      updateValue(value) {
        this.$refs.input.value = value
        this.$emit('input', value)
      }
    },

    watch: {
      value(newValue) {
        this.showClearButton = !!newValue
      }
    }
  }
</script>
<style lang="scss" rel="stylesheet/scss" scoped>
  .ha-input-wrapper {
    position: relative;

    &.has-floating-label {
      .input-clear {
        top: 14px + 28px;
      }
    }

    .ha-input {
      height: 50px;
      padding: 13px 15px;
      position: relative;

      .input-label {
        font-size: 14px;
        line-height: 20px;
      }

      input {
        font-size: 14px;
      }

    }

    .input-clear {
      position: absolute;
      top: 14px;
      right: 9px;
      z-index: 3;

      width: 24px;
      height: 24px;
      background-image: url(../../../app/assets/images/clear@3x.png);
      background-size: 12px 12px;
      background-repeat: no-repeat;
      background-position: 5px 5px;

      visibility: hidden;
      &.active {
        visibility: visible;
      }
    }

    .item-floating-label {
      height: 75px;
      background: #FFF;
      .input-label {
        padding-top: 2px;
        font-size: 16px;
        line-height: 20px;
        font-weight: 400;
      }
    }
  }
</style>
