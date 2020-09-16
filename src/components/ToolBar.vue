<template>
  <div id="toolBar">
    <div class="m-2">
        <b-tabs content-class="mt-3 p-2">
            <b-tab title="Infos" active>
                <b-form-group
                    id="product-title-group"
                    label="Nome do produto:"
                    label-for="product-title"
                    
                >
                    <b-form-input id="product-title" type="text" v-model="bottleOptions.label.title" @input="changeTitle">
                    </b-form-input> 
                </b-form-group>
                <b-form-group
                    id="product-quantity-group"
                    label="Quantidade de cápsulas:"
                    label-for="product-quantity"
                    
                >
                    <b-form-input id="product-quantity" type="number" v-model="bottleOptions.label.capsNumber" @input="changeTitle">
                    </b-form-input> 
                </b-form-group>
                <b-form-group
                    id="product-description-group"
                    label="Descrição:"
                    label-for="product-description"
                    
                >
                    <b-form-input id="product-description" type="text" v-model="bottleOptions.label.subtitle" @input="changeTitle">
                    </b-form-input> 
                </b-form-group>
                
                
            </b-tab>
            <b-tab title="Rótulo">
                <div class="tool-item">
                    <b-row>
                        <b-col><label>Cor do titulo</label></b-col>
                        <b-col><div class="color-box" @click="changeColor('titleColor')"><div class="color-button" :style="{ backgroundColor: bottleOptions.label.titleColor}"></div></div></b-col>
                    </b-row>
                </div>
                <div class="tool-item">
                    <b-row>
                        <b-col><label>Cor do subtitulo</label></b-col>
                        <b-col><div class="color-box" @click="changeColor('subtitleColor')"><div class="color-button" :style="{ backgroundColor: bottleOptions.label.subtitleColor}"></div></div></b-col>
                    </b-row>
                </div>

                <chrome-picker v-click-outside="closePicker" :value="colors" @input="updateValue" v-if="pickTitleColor" class="pickTitleColor"/>
            </b-tab>
            <b-tab title="Pote">
                <div class="tool-item">
                    <b-row>
                        <b-col><label>Cor da tampa</label></b-col>
                        <b-col><div class="color-box" @click="changeBottleColor()"><div class="color-button" :style="{ backgroundColor: bottleOptions.bottle.bottleColor}"></div></div></b-col>
                    </b-row>
                </div>
            </b-tab>
        </b-tabs>
    </div>
  </div>
</template>

<script>
import { Chrome } from 'vue-color'

export default {
    components: {
        'chrome-picker': Chrome,
    },
    props: {
        value: {

        }
    },
    data(){
        return{
            pickTitleColor: false,
            currentColorData: null,
            colors: {
                hex: '#194d33'
            },
            bottleOptions: {
                
            }
        }
    },
    methods:{
        closePicker: function(e){
            if( e.target.className != 'color-button' && e.target.className != 'color-box' ){
                this.pickTitleColor = false;
            }
        },
        updateValue: function(e){
            this.bottleOptions.label[this.currentColorData] = e.hex
            this.$emit('input', this.bottleOptions)
        },
        changeTitle: function(){
            this.$emit('input', this.bottleOptions)
        },
        changeBottleColor: function(){
            this.bottleOptions.bottle.bottleLidColor = "#FFFFFF"
            this.bottleOptions.bottle.bottleColor = "#FFFFFF"
            this.colors.hex = this.bottleOptions.bottle.bottleColor
        },
        changeColor: function(colorEl){
            this.colors.hex = this.bottleOptions.label[colorEl]
            this.currentColorData = colorEl
            this.pickTitleColor = !this.pickTitleColor
        }
    },
    created: function(){
        this.bottleOptions = this.value
    }
}
</script>

<style lang="scss" scope>
#toolBar{
    width: 300px;
    position: absolute;
    height: 100%;
    padding-right: 0px;
    border-right: 1px solid #ddd;
}
.color-box{
    .color-button{
        width: 100%;
        height: 100%;
    }
    cursor: pointer;
    float: right;
    padding: 3px;
    border: 1px solid #ddd;
    height: 27px;
    width: 27px;
    border-radius: 3px;;
}
.tool-item label, label{
    font-size: 12px;
    color: #6d7882;
    font-family: 'Roboto';
}
#toolBar .form-control{
    font-size: 12px;
    color: #6d7882;
    font-family: 'Roboto'; 
    outline: 0;
}
#toolBar small{
    font-size: 10px;
}
.pickTitleColor{
    position: absolute;
    z-index: 200;
}
</style>