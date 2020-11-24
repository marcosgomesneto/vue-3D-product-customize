<template>
    <b-form-group
        id="logo-image-group"
        label="Logo marca:"
    >
        <input type="file" id="uploadLogoFile" @change="uploadFileToServer" accept="image/png, image/jpeg" style="display:none"/>
        <div class="image-content" @click="selectFile">
            <div class="hover-background"></div>
            <div class="upload-button">Escolher imagem</div>
        </div>
    </b-form-group>
</template>

<script>
export default {
    methods: {
        selectFile: function(){
            document.getElementById('uploadLogoFile').click();
        },
        uploadFileToServer: function(e){
            console.log(e);
        }
    }    
}
</script>

<style lang="scss">
.image-content{
    &:hover > .upload-button{
        
        transform: translateY(-30px);
    }
    &:hover > .hover-background{
        display: block;
    }
    .upload-button{
        position: absolute;
        width: 100%;
        height: 30px;
        bottom: -30px;
        color: #eee;
        line-height: 28px;
        text-align: center;
        opacity: 0.8;
        background-color: #555;
        transition: transform 0.3s;
        z-index: 2;
    }
    .hover-background{
        display: none;
        position: absolute;
        width: 100%;
        height: 100%;
        background-color: #555;
        opacity: 0.3;
    }
    font-size: 12px;
    font-family: 'Roboto';
    position: relative;
    overflow: hidden;
    cursor: pointer;
    background-image: -webkit-linear-gradient(45deg,#ddd 25%,transparent 0,transparent 75%,#ddd 0,#ddd),-webkit-linear-gradient(45deg,#ddd 25%,transparent 0,transparent 75%,#ddd 0,#ddd);
    background-image: -o-linear-gradient(45deg,#ddd 25%,transparent 25%,transparent 75%,#ddd 75%,#ddd),-o-linear-gradient(45deg,#ddd 25%,transparent 25%,transparent 75%,#ddd 75%,#ddd);
    background-image: linear-gradient(45deg,#ddd 25%,transparent 0,transparent 75%,#ddd 0,#ddd),linear-gradient(45deg,#ddd 25%,transparent 0,transparent 75%,#ddd 0,#ddd);
    -webkit-background-size: 16px 16px;
    background-size: 16px 16px;
    background-position: 0 0,8px 8px;
    background-color: #f6f6f6;
    width: 100%;
    border: 1px solid #ddd;
    height: 120px;
}
</style>