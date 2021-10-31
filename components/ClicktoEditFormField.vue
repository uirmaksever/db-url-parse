<template>
    <div>
        <span v-if="editable === false" @click="editable = true">{{ db_parameter_value }}</span>
        <div v-if="editable === true">
            <b-form-input :value="db_parameter_value" @input="parameter_value = $event">
            </b-form-input>
            <b-icon-x-circle @click="onUpdate()"></b-icon-x-circle>
        </div>
    </div>
</template>

<script>
export default {
    data: function () {
        return {
            editable: false,
            parameter_value: this.db_parameter_value,
        }
    },
    props: ["db_parameter_value"],
    methods: {
        onUpdate() {
            this.editable = false;
            this.$emit("update_db_parameter_value", this.parameter_value)
        }
    }
}
</script>
