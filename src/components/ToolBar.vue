<template>
  <div id="toolBar">
    <div class="m-2">
        <b-tabs content-class="mt-3 p-2">
            <b-tab title="Infos" active>
                <ImageInput/>
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
                    <div class="texture-box">
                           
                    </div>
                </div>
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
            </b-tab>
            <b-tab title="Pote">
                <div class="tool-item">
                    <b-row>
                        <b-col><label>Cor da tampa</label></b-col>
                        <b-col><div class="color-box" @click="changeColor('bootleLidColor')"><div class="color-button" :style="{ backgroundColor: bottleOptions.bottle.bottleLidColor}"></div></div></b-col>
                    </b-row>
                </div>
                <div class="tool-item">
                    <b-row>
                        <b-col><label>Cor do pote</label></b-col>
                        <b-col><div class="color-box" @click="changeColor('bootleColor')"><div class="color-button" :style="{ backgroundColor: bottleOptions.bottle.bottleColor}"></div></div></b-col>
                    </b-row>
                </div>

                <div class="tool-item">
                    <b-row>
                        <b-col><label>Pote transparente</label></b-col>
                        <b-col><input type="checkbox" v-model="bottleOptions.bottle.transparent" :class="{active: bottleOptions.bottle.transparent}" class="custom-check"/></b-col>
                    </b-row>
                </div>
            </b-tab>
        </b-tabs>
        <chrome-picker v-click-outside="closePicker" :value="colors" @input="updateValue" v-if="pickTitleColor" class="pickTitleColor"/>
    </div>
  </div>
</template>

<script>
import { Chrome } from 'vue-color'
import ImageInput from './inputs/ImageInput.vue'

export default {
    components: {
        'chrome-picker': Chrome,
        'ImageInput': ImageInput
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
            switch(this.currentColorData){
                case 'titleColor':
                    this.bottleOptions.label.titleColor = e.hex
                break;
                case 'subtitleColor':
                    this.bottleOptions.label.subtitleColor = e.hex
                break;
                case 'bootleLidColor':
                    this.bottleOptions.bottle.bottleLidColor  = e.hex
                break;
                case 'bootleColor':
                    this.bottleOptions.bottle.bottleColor  = e.hex
                break;
            }
            
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
            switch(colorEl){
                case 'titleColor':
                    this.colors.hex = this.bottleOptions.label.titleColor
                break;
                case 'subtitleColor':
                     this.colors.hex = this.bottleOptions.label.subtitleColor
                break;
                case 'bootleLidColor':
                     this.colors.hex = this.bottleOptions.bottle.bootleLidColor
                break;
                case 'bootleColor':
                     this.colors.hex = this.bottleOptions.bottle.bootleColor
                break;
            }
            
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
.tool-item label, label, legend.col-form-label{
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

.custom-check{
    border: 0;
    position: relative;
    width: 0;
    height: 0;
    cursor:pointer;
    display: block;
    float: right;
    margin-right: 50px;
    margin-top: 2px;
    &:before {
        content: "Não";
        
        text-align: right;
        padding-right: 7px;
        color: #7d7c7c;
        line-height: 20px;
        width: 50px;
        height: 20px;
        border-radius: 15px;
        display: block;
        /* background: black; */
        font-size: 7px;
        text-transform: uppercase;
        background: #eceeef;
        -webkit-box-shadow: inset 0 1px 2px rgba(0,0,0,.12), inset 0 0 2px rgba(0,0,0,.15);
        box-shadow: inset 0 1px 2px rgba(0,0,0,.12), inset 0 0 2px rgba(0,0,0,.15);
        -webkit-transition: .15s ease-out;
        -o-transition: .15s ease-out;
        transition: .15s ease-out;
        -webkit-transition-property: opacity,background;
        -o-transition-property: opacity,background;
        transition-property: opacity,background;
    }
    &.active:before{
        content: "Sim";
        color: #fff;
        background: #71d7f7;
        padding-left: 7px;
        text-align: left;
        -webkit-box-shadow: inset 0 1px 2px rgba(0,0,0,.15), inset 0 0 3px rgba(0,0,0,.2);
        box-shadow: inset 0 1px 2px rgba(0,0,0,.15), inset 0 0 3px rgba(0,0,0,.2);   
    }
    &.active:after{
        left: 31px;
    }
    &:after{
        content: "";
        display: block;
        position: absolute;
        top: 1px;
        left: 1px;
        width: 18px;
        height: 18px;
        background: #fff;
        -webkit-border-radius: 10px;
        border-radius: 10px;
        -webkit-transition: left .15s ease-out;
        -o-transition: left .15s ease-out;
        transition: left .15s ease-out;
    }
}
.texture-box{
    width: 100%;
    height: 150px;
    border: 1px solid #ddd;
}
.tool-item{
    margin-bottom: 10px;
}
</style>