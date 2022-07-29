<template>
    <div class="v-kaban-chevron-wrap" :style="rotateClass">
        <div class="v-kaban-chevron"></div>
    </div>
</template>

<script>
export default {
    name: "collapseChevron",
    props: {
        rotate: {
            type: Boolean,
            default: false,
        }
    },
    computed: {
        rotateClass() {
            return {
                transform: !this.rotate ? 'rotate(-90deg)' : 'rotate(0)',
                '-ms-transform': !this.rotate ? 'rotate(-90deg)' : 'rotate(0)', /* IE 9 */
                '-webkit-transform': !this.rotate ? 'rotate(-90deg)' : 'rotate(0)' /* Chrome, Safari, Opera */
            }
        }
    }
}
</script>

<style scoped>
.v-kaban-chevron-wrap {
    height: 13px;
    width: 13px;
    -webkit-transition: all 180ms linear;
    -moz-transition: all 180ms linear;
    -ms-transition: all 180ms linear;
    -o-transition: all 180ms linear;
    transition: all 180ms linear;

    white-space: nowrap;
    display: inline-block;
}
.v-kaban-chevron {
    position: relative;
    text-align: center;
    padding: 1px;
    margin-bottom: 6px;
}

.v-kaban-chevron:before {
    content: '';
    position: absolute;
    top: 5px;
    left: 0;
    height: 100%;
    width: 50%;
    background: grey;
    transform: skew(0deg, 28deg);
}

.v-kaban-chevron:after {
    content: '';
    position: absolute;
    top: 5px;
    right: 0;
    height: 100%;
    width: 50%;
    background: grey;
    transform: skew(0deg, -28deg);
}
</style>