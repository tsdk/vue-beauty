<template>
    <div class="ant-tooltip-wrapper" style="display:inline-block" @mouseenter="handleShowPopper" @mouseleave="handleClosePopper">
        <div style="display:inline-block" ref="reference">
            <slot></slot>
        </div>
        <transition name="fade">
            <div :class="`ant-tooltip  ant-tooltip-placement-${placement}`" ref="popper" v-show="!disabled && (visible || always)">
                <div class="ant-tooltip-content">
                    <div class="ant-tooltip-arrow"></div>
                    <div class="ant-tooltip-inner"><slot name="content">{{ content }}</slot></div>
                </div>
            </div>
        </transition>
    </div>
</template>
<script>
    import Popper from '../../mixins/popper';

    const placements = ['top', 'topLeft', 'topRight', 'bottom', 'bottomLeft', 'bottomRight', 'left', 'leftTop', 'leftBottom', 'right', 'rightTop', 'rightBottom'];

    export default {
        name: 'Tooltip',
        mixins: [Popper],
        props: {
            placement: {
                validator(value) {
                    return placements.includes(value);
                },
                default: 'top',
            },
            content: {
                type: [String, Number],
                default: '',
            },
            delay: {
                type: Number,
                default: 0,
            },
            disabled: {
                type: Boolean,
                default: false,
            },
            controlled: {    // under this prop,Tooltip will not close when mouseleave
                type: Boolean,
                default: false,
            },
            initVisible: {
                type: Boolean,
                default: false,
            },
            always: {
                type: Boolean,
                default: false,
            },
        },
        data() {
            return {
                prefixCls: 'ant-tooltip',
            };
        },
        methods: {
            handleShowPopper() {
                this.timeout = setTimeout(() => {
                    this.visible = true;
                }, this.delay);
            },
            handleClosePopper() {
                clearTimeout(this.timeout);
                if (!this.controlled) {
                    this.visible = false;
                }
            },
        },
        mounted() {
            this.visible = this.initVisible;
        },
    };
</script>

