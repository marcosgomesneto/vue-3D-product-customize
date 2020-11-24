<template>
    <div id="bottle3d" ref="bottleCanvas">          

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
                this.bottleObj.children[0].material.color.setStyle(options.bottleColor)
                if( options.transparent ){
                    this.bottleObj.children[0].material.transparent = true;
                    this.bottleObj.children[0].material.opacity = 0.6;
                }else{
                    this.bottleObj.children[0].material.transparent = false;
                    this.bottleObj.children[0].material.opacity = 1;                    
                }
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
            bottle.position.set(19, -25, 0);

            //var obj = bottle.getObjectByName('Tube001');
            this.bottleLidObj = bottle.getObjectByName('ChamferCyl001');
            this.bottleObj = bottle.getObjectByName('Line001');            

            this.bottleObj.children[0].material = new THREE.MeshPhongMaterial({color: 0xff0000, transparent: this.bottleOptions.transparent, opacity: 0.6, shininess: 100});
            this.bottleLidObj.children[0].material = new THREE.MeshPhongMaterial({color: 0xff0000, transparent: false, opacity: 1, shininess: 100});

            this.bottleLidObj.children[0].material.color = new THREE.Color(this.bottleOptions.bottleLidColor);
            this.bottleObj.children[0].material.color = new THREE.Color(this.bottleOptions.bottleColor);

            this.scene.add(bottle);
        }
    },
    mounted: function(){
        this.scene = new THREE.Scene()

        var element = document.getElementById('bottle3d');
        var positionInfo = element.getBoundingClientRect();
        let cw = positionInfo.width;
        let ch = positionInfo.height;


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

        var light2 = new THREE.AmbientLight(0x202020, 7, 100);
        
        this.scene.add(light2);

        var light = new THREE.PointLight( 0xffffcc, 7, 200 );
        light.position.set( -150, 0, 100 );
        this.scene.add( light );

        var light3 = new THREE.PointLight( 0xffffcc, 7, 200 );
        light3.position.set( -150, -100, 100 );
        this.scene.add( light3 );

        var light4 = new THREE.PointLight( 0xffffcc, 7, 200 );
        light4.position.set( 150, 200, -100 );
        this.scene.add( light4 );
        
        var loader = new GLTFLoader();
        loader.crossOrigin = true;



        loader.load('./assets/models/bottle/scene.gltf', this.loadBottle );



        this.render();
    }
}
</script>

<style>
    #bottle3d canvas{
        outline: none;
    }
    #bottle3d{
        
        display: block;
        height: calc(100% - 251px);
        position: absolute;
        width: calc(100% - 300px);
        background-image: url(../assets/nykax-logo-t2.png);
    }
</style>