<template>
    <div id="idBackgroud">
        <div id="idBar" v-bind:style="stl">
        </div>
    </div>
</template>

<script>

export default {
    name: 'ProgressBar',
    props: {
        value: Number
    },
    data() {
        return {
            stl: "width: 0%; background-color: #FF0000;"
        }
    },
    watch: {

        value: function(newVal, oldVal) {

            if (newVal !== oldVal) {

                let colr = "";
                if (newVal < 25) { colr = "#F04040"; }
                else if (newVal < 50) { colr = "#A0A020"; }
                else if (newVal < 75) { colr = "#0080FF"; }
                else { colr = "#40C040"; }

                this.stl = `width: ${newVal}%; background-color: ${colr};`
            }
        }
    }
}

</script>

<style scoped>

    #idBackgroud {
        border: 3px solid #404040;
        background-color: #808080;
        border-radius: 10px;
        width: 500px;
        height: 10px;
    }
    #idBar {
        border-radius: 10px;
        width: 50%;
        height: 100%;
    }

    p {
        color:#2c3e50;
    }

</style>
