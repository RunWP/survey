<template>

    <h2>{{ question + " ?" + (required ? " (required)" : "") }}</h2>
    <div class="choices">
        <div class="box" v-for="(choice, index) in choices" :key="index">
            <input
                :type="type"
                :id="uid + '-' + index"
                :value="index" v-model="result"
                :checked="check(index)"
                @change="changed"
            />
            <label :for="uid + '-' + index">{{ choice }}</label>
        </div>
    </div>

</template>

<script>

    export default {
        name: 'Question',
        props: {
            uid: Number,
            question: String,
            choices: Array,
            type: String,
            selected: [Number, Array],
            required: Boolean
        },
        data() {
            return {
                result: []
            }
        },
        emits: ['changed'],
        methods: {
            changed() {
                let data = { "uid": this.uid, "selected": this.result }
                this.$emit('changed', data);
            },
            check(idx) {
                if (typeof(this.result) === "number") {
                    if (this.result === idx) {return "true";}
                } else {
                    return this.result.includes(idx);
                }
            }
        },
        mounted() {
            this.result = this.selected;
        }
    }

</script>

<style scoped>

    h2 {
        margin-top: 50px;
    }

    .choices {
        display: flex;
        flex-direction: column;
        width: max-content;
        margin: 0px auto;
    }

    .box {
        text-align: left;
        margin: 3px;
    }

    .box label {
        margin-left: 10px;
    }

</style>
