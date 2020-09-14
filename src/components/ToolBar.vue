<template>
  <div>
    <input type="text" v-model="bottleOptions.title" @input="changeTitle">
    {{bottleOptions.title}}
  </div>
</template>

<script>
export default {
    props: {
        value: {

        }
    },
    data(){
        return{
            bottleOptions: {
                
            }
        }
    },
    methods:{
        changeTitle: function(){
            this.$emit('input', this.bottleOptions);
        }
    },
    created: function(){
        this.bottleOptions = this.value
    }
}
</script>

<style>

</style>