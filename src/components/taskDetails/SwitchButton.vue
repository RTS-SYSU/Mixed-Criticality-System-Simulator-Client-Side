<template>
    <div class="m-switch-wrap">
      <div @click="disabled ? e => e.preventDefault() : onSwitch()" :class="['m-switch', { 'switch-checked': checkedVal, 'disabled': disabled }]">
        <div :class="['u-switch-inner', checkedVal ? 'inner-checked' : 'inner-unchecked' ]">{{ checkedVal ? checkedInfo : uncheckedInfo }}</div>
        <div :class="['u-node', { 'node-checked': checkedVal }]"></div>
      </div>
    </div>
</template>

<script>
    export default {
        name: 'SwitchButton',

        model: {
            prop: 'checked',
            event: 'change'
        },

        props: {
            defaultChecked: { // 初始是否选中
                type: Boolean,
                default: false
            },
            checkedInfo: { // 选中时的内容
                type: [Number, String],
                default: null
            },
            uncheckedInfo: { // 未选中时的内容
                type: [Number, String],
                default: null
            },
            disabled: { // 是否禁用
                type: Boolean,
                default: false
            },
            checked: { // （v-model）指定当前是否选中
                type: Boolean,
                default: null
            }
        },

        data () {
            return {
                checkedVal: null
            }
        },

        watch: {
            checked () {
                this.initSwitcher()
            },

            defaultChecked () {
                this.initSwitcher()
            }
        },

        created () {
            this.initSwitcher()
        },

        methods: {
            initSwitcher () {
                if (typeof this.checked === 'boolean') {
                    this.checkedVal = this.checked
                } else if (typeof this.checked === 'object') {
                    this.checkedVal = this.defaultChecked
                }
            },

            onSwitch () {
                this.checkedVal = !this.checkedVal
                this.$emit('change', this.checkedVal)
            }
        }
    }
</script>

<style scoped>
    .m-switch-wrap {
        height: 22px;
        min-width: 44px;
    }

    .m-switch {
        position: relative;
        height: 22px;
        color: rgba(0,0,0,.65);
        font-size: 14px;
        background: rgba(0,0,0,.25);
        border-radius: 100px;
        cursor: pointer;
        transition: background .36s;
            
    }

    .switch-checked {
        background: #8ce196;;
    }

    .disabled {
        cursor: not-allowed;
        opacity: .4;
    }

    .u-switch-inner {
        display: inline-block;
        color: #fff;
        font-size: 14px;
        line-height: 22px;
        padding: 0 8px;
        transition: all .36s;
    }

    .inner-checked {
        margin-right: 18px;
    }

    .inner-unchecked {
        margin-left: 18px;
    }

    .u-node {
        position: absolute;
        top: 2px;
        left: 2px;
        width: 18px;
        height: 18px;
        background: #FFF;
        border-radius: 100%;
        cursor: pointer;
        transition: all .36s;
    }

    .node-checked {
        left: 100%;
        margin-left: -2px;
        transform: translateX(-100%);
    }
</style>