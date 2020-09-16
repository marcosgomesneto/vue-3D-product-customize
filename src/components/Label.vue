<template>
  <div id="labelEdit">
      <canvas id="canvasLabel" ref="canvasLabel" height="250"></canvas>
  </div>
</template>

<script>
import { fabric } from "fabric"

export default {
    props:{
        bottleOptions: {
            required: true
        }
    },
    computed: {
        capsNum: function(){
            return "Contém " + this.bottleOptions.label.capsNumber + " cápsulas de 500mg"
        }
    },
    data(){
        return{
            canvas: null,
            canvasTitle: null,
            canvasCapsNumber: null,
            canvasSubtitle: null
        }
    },
    watch:{
        bottleOptions: {
            handler(options){
                this.canvasSubtitle.set('text', options.label.subtitle)
                this.canvasSubtitle.set('fill', options.label.subtitleColor)

                this.canvasTitle.set('text', options.label.title)
                this.canvasTitle.set('fill', options.label.titleColor)

                this.canvasCapsNumber.set('text', this.capsNum)

                this.canvas.renderAll();
            },
            deep: true
        }
    },
    mounted(){
        var canvas = new fabric.Canvas('canvasLabel', {
            backgroundColor: 'white'
        });
        canvas.setWidth(800);

        var border = new fabric.Rect({
            width: 795,
            height: 245,
            left: 0,
            top: 0,
            stroke: '#6b5700',
            strokeWidth: 5,
            fill: '#f1f1f1',
            selectable: false,
            originX: 'left',
            originY: 'top'
        });
        canvas.add(border);

        var rect2 = new fabric.Rect({
            width: 150,
            height: 150,
            left: 0,
            top: 0,
            stroke: '#6b5700',
            strokeWidth: 1,
            fill: '#888',
            selectable: true,
            originX: 'left',
            originY: 'top'
        });

        var rect3 = new fabric.Rect({
            width: 150,
            height: 15,
            left: 0,
            top: 0,
            stroke: '#6b5700',
            strokeWidth: 1,
            fill: '#6b5700',
            selectable: true,
            originX: 'left',
            originY: 'top'
        });

        let infoNutri01 = new fabric.Text("Informação nutricional", {
            fontSize: 11,
            textAlign: 'center',
            fill: '#fff',
            fontWeight: 'bold',
            left: 2,
            top: 1,
            angle: 0,
            originX: 'left',
            originY: 'top',
            fontFamily: "Arial"
        });

        var nutriTable = new fabric.Group([rect2, rect3, infoNutri01],{
            left: 20,
            top: 20
        });
        canvas.add(nutriTable);

        this.canvasCapsNumber = new fabric.Text(this.capsNum, {
            fontSize: 15,
            textAlign: 'center',
            fill: '#333',
            fontWeight: 'bold',
            left: 250,
            top: 150,
            angle: 0,
            originX: 'left',
            originY: 'center',
            fontFamily: "Arial"
        });

        let infoText = new fabric.Text("COLORIDO ARTIFICIALMENTE", {
            fontSize: 10,
            textAlign: 'center',
            fill: '#000',
            fontWeight: 'bold',
            fontStyle: "italic",
            left: 250,
            top: 125,
            angle: 0,
            originX: 'left',
            originY: 'center',
            fontFamily: "Arial"
        });

        this.canvasTitle = new fabric.Text(this.bottleOptions.label.title, {
            fontSize: 40,
            textAlign: 'center',
            fontWeight: 'bold',
            left: 250,
            top: 50,
            angle: 0,
            originX: 'left',
            originY: 'center',
            fontFamily: "Arial"
        });

        this.canvasSubtitle = new fabric.Text(this.bottleOptions.label.subtitle, {
            fontSize: 15,
            textAlign: 'center',
            fill: this.bottleOptions.label.subtitleColor,
            fontWeight: 'bold',
            left: 250,
            top: 95,
            angle: 0,
            originX: 'left',
            originY: 'center',
            fontFamily: "Arial"
        });



        canvas.add(this.canvasTitle);
        canvas.add(this.canvasSubtitle);
        canvas.add(infoText);
        canvas.add(this.canvasCapsNumber);
        this.canvas = canvas

        this.$emit('input', {
            canvas: canvas,
            html: document.getElementById('canvasLabel')
        });
    }
}
</script>
<style>
.canvas-container{
    margin: 0 auto;
}
#labelEdit{
    border-bottom: 1px solid #ddd;
}
</style>