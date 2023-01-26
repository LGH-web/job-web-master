<template>
    <div ref="chartDom" ></div>
</template>

<script>
    import * as echarts from 'echarts';
    import debounce from "lodash/debounce";
    import { addListener, removeListener} from "resize-detector";

    export default {
        props: {
            option: {
                type: Object,
                default: ()=> {}
            }
        },
        watch: {
            // option(val) {
            //   this.chart.setOption(val);
            // },
            option: {
                handler(val) {
                    this.chart.setOption(val);
                },
                deep: true
            }
        },
        created() {
            this.resize = debounce(this.resize, 300);
        },
        mounted() {
            this.renderChart();
            addListener(this.$refs.chartDom, this.resize);
        },
        beforeDestroy() {
            removeListener(this.$refs.chartDom, this.resize);
            this.chart.dispose();
            this.chart = null;
        },
        methods:{
            resize(){
                this.chart.resize();
            },
            renderChart() {
                this.chart = echarts.init(this.$refs.chartDom);
                this.chart.setOption(this.option);
            }
        },
    }
</script>
<style>
</style>
