<template>
    <transition name="p-message" appear>
        <div v-show="visible" :class="containerClass" role="alert">
            <div class="p-message-wrapper">
                <span :class="iconClass"></span>
                <div class="p-message-text">
                    <slot></slot>
                </div>
                <button v-if="closable" v-ripple class="p-message-close p-link" @click="close($event)" type="button">
                    <i class="p-message-close-icon pi pi-times"></i>
                </button>
            </div>
        </div>
    </transition>
</template>

<script>
import Ripple from 'primevue/ripple';

export default {
    name: 'Message',
    emits: ['close'],
    props: {
        severity: {
            type: String,
            default: 'info'
        },
        closable: {
            type: Boolean,
            default: true
        },
        sticky: {
            type: Boolean,
            default: true
        },
        life: {
            type: Number,
            default: 3000
        },
        icon: {
            type: String,
            default: null
        }
    },
    timeout: null,
    data() {
        return {
            visible: true
        };
    },
    mounted() {
        if (!this.sticky) {
            setTimeout(() => {
                this.visible = false;
            }, this.life);
        }
    },
    methods: {
        close(event) {
            this.visible = false;
            this.$emit('close', event);
        }
    },
    computed: {
        containerClass() {
            return 'p-message p-component p-message-' + this.severity;
        },
        iconClass() {
            return [
                'p-message-icon pi',
                this.icon
                    ? this.icon
                    : {
                          'pi-info-circle': this.severity === 'info',
                          'pi-check': this.severity === 'success',
                          'pi-exclamation-triangle': this.severity === 'warn',
                          'pi-times-circle': this.severity === 'error'
                      }
            ];
        }
    },
    directives: {
        ripple: Ripple
    }
};
</script>

<style>
.p-message-wrapper {
    display: flex;
    align-items: center;
}

.p-message-close {
    display: flex;
    align-items: center;
    justify-content: center;
}

.p-message-close.p-link {
    margin-left: auto;
    overflow: hidden;
    position: relative;
}

.p-message-enter-from {
    opacity: 0;
}

.p-message-enter-active {
    transition: opacity 0.3s;
}

.p-message.p-message-leave-from {
    max-height: 1000px;
}

.p-message.p-message-leave-to {
    max-height: 0;
    opacity: 0;
    margin: 0 !important;
}

.p-message-leave-active {
    overflow: hidden;
    transition: max-height 0.3s cubic-bezier(0, 1, 0, 1), opacity 0.3s, margin 0.15s;
}

.p-message-leave-active .p-message-close {
    display: none;
}
</style>
