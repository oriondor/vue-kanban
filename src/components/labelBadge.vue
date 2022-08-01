<template>
    <div :style="wrapClass">
        <div class="v-kaban-label" :style="{...style, ...rotateClass}">
            <div>{{ text }}</div>
        </div>
    </div>
</template>

<script>
export default {
    name: "labelBadge",
    props: {
        text: {
            type: String,
            default: ''
        },
        style: {
            type: Object,
            default: Object
        },
        rotate: {
            type: Boolean,
            default: false
        }
    },
    computed: {
        wrapClass(){
            return {
                    'margin-top': !this.rotate ? '0.9rem' : 0,
                    'margin-bottom': !this.rotate ? '0.9rem' : 0,
                    'margin-left': !this.rotate ? '0' : '0.5rem',
                    'margin-right': !this.rotate ? '0' : '0.5rem',
            }
        },
        rotateClass() {
            return {
                'min-width': !this.rotate ? '100%' : '',
                height: !this.rotate ? '20px' : '',
                margin: !this.rotate ? 'calc(50% - 8px) -8px' : 'unset',
                transform: !this.rotate ? 'rotate(90deg)' : 'rotate(0)',
                '-ms-transform': !this.rotate ? 'rotate(90deg)' : 'rotate(0)', /* IE 9 */
                '-webkit-transform': !this.rotate ? 'rotate(90deg)' : 'rotate(0)' /* Chrome, Safari, Opera */
            }
        }
    }
}
</script>

<style scoped>
.v-kaban-label {
    font-weight: 700;
    font-size: 10pt;
    height: 20px;
    border-radius: 13px;
    padding: 2px 8px 1px 8px;
    white-space: nowrap;
}
</style>