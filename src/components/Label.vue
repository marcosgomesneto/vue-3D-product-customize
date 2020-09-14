<template>
  <div>
      <canvas id="canvasLabel" ref="canvasLabel" height="200"></canvas>
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
    data(){
        return{
            canvas: null,
            canvasTitle: new fabric.Text(this.bottleOptions.title, {
                fontSize: 40,
                textAlign: 'center',
                fontWeight: 'bold',
                left: 400,
                top: 100,
                angle: 0,
                originX: 'center',
                originY: 'center',
            })
        }
    },
    watch:{
        bottleOptions: {
            handler(options){
                this.canvasTitle.set('text', options.title)
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
            height: 195,
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
            width: 120,
            height: 150,
            left: 20,
            top: 20,
            stroke: '#6b5700',
            strokeWidth: 2,
            fill: '#888',
            selectable: true,
            originX: 'left',
            originY: 'top'
        });

        var rect3 = new fabric.Rect({
            width: 120,
            height: 15,
            left: 20,
            top: 15,
            stroke: '#6b5700',
            strokeWidth: 2,
            fill: '#888',
            selectable: true,
            originX: 'left',
            originY: 'top'
        });

        var nutriTable = new fabric.Group([rect2, rect3],{
            left: 20,
            top: 20
        });
        canvas.add(nutriTable);



        this.canvasTitle.setSelectionStyles({
            fontStyle: 'normal',
            fill: '',
            stroke: 'red',
            strokeWidth: 2
        });


        canvas.add(this.canvasTitle);
        this.canvas = canvas

        this.$emit('input', {
            canvas: canvas,
            html: document.getElementById('canvasLabel')
        });
    }
}
</script>

<style>

</style>