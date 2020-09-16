<template>
    <div id="bottle3d">          
        <div id="bottleCanvas" ref="bottleCanvas">
            
        </div>
    </div>
</template>

<script>
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';

export default {
    props: {
        canvasLabel: {
            required: true
        },
        bottleOptions: {
            required: true
        }
    },
    data(){
        return {
            renderCalls: [],
            scene: null,
            camera: null,
            renderer: null,
            bottleObj: null,
            bottleLidObj: null
        }
    },
    watch: {
        bottleOptions: {
            handler(options){              
                console.log(options.bottleLidColor)
                this.bottleLidObj.children[0].material.color.setStyle(options.bottleLidColor)
                this.bottleObj.children[0].material.color.setStyle(options.bottleLidColor)
            },
            deep: true
        }
    },
    methods: {
        render: function(){
            requestAnimationFrame(this.render)
            this.renderCalls.forEach((callback) => { callback(); })
        },
        renderScene: function(){
            this.renderer.render(this.scene, this.camera);
        },
        loadBottle: function(data){
            var canvasTexture = new THREE.CanvasTexture(this.canvasLabel.html);
            canvasTexture.wrapS = THREE.RepeatWrapping;
            canvasTexture.wrapT = THREE.RepeatWrapping;

            let label = new THREE.CylinderGeometry(50.50, 51, 110, 100, 100, true);
            //let label_texture = new THREE.TextureLoader().load('./assets/materials/wandr.png');
            //let label_material = new THREE.MeshBasicMaterial({ map: label_texture });
            var canvas_material = new THREE.MeshBasicMaterial({ map: canvasTexture });
            var label_mesh = new THREE.Mesh(label, canvas_material);
            label_mesh.rotation.y = 9.4;
            label_mesh.position.set(-18, 7, 0);


            this.canvasLabel.canvas.on("after:render", function() {
                label_mesh.material.map.needsUpdate = true;
            });

            var bottle = data.scene;
            bottle.add(label_mesh);
            bottle.position.set(19, 0, 0);

            //var obj = bottle.getObjectByName('Tube001');
            this.bottleLidObj = bottle.getObjectByName('ChamferCyl001');
            this.bottleObj = bottle.getObjectByName('Line001');            

            
            this.bottleLidObj.children[0].material.color = new THREE.Color("#2B2300");
            this.bottleObj.children[0].material.color = new THREE.Color("#010101");

            this.scene.add(bottle);
        }
    },
    mounted: function(){
        this.scene = new THREE.Scene()
        let cw = 780;
        let ch = 540;
        this.camera = new THREE.PerspectiveCamera(75, cw / ch, 0.1, 1000)
        this.renderer = new THREE.WebGLRenderer({ antialias: true,  alpha: true})
        this.renderer.setSize(cw, ch)

        
        this.$refs.bottleCanvas.appendChild(this.renderer.domElement)

        this.renderCalls.push(this.renderScene);

        var controls = new OrbitControls(this.camera, this.renderer.domElement);
        this.renderCalls.push(function () {
            controls.update()
        });

        this.camera.position.z = 200;

        var light2 = new THREE.AmbientLight(0x20202A, 20, 100);
        light2.position.set(30, -10, 30);
        this.scene.add(light2);

        
        var loader = new GLTFLoader();
        loader.crossOrigin = true;


        loader.load('./assets/models/bottle/scene.gltf', this.loadBottle );



        this.render();
    }
}
</script>

<style>
    #bottleCanvas canvas{
        outline: none;
    }
    #bottle3d{
        background-image: url(../assets/nykax-logo-t2.png);
    }
</style>