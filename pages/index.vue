<template>
    <div>
        <div ref="stage"></div>
        <input type="range" min="0.00" max="100">
    </div>
</template>

<script>
import * as THREE from "three";

export default {
    data() {
        let scene;
        let object_cube;
        let light_ambient, light_direction;
        let camera;
        let renderer;
        let width = 600; // 全画面にしたい場合：window.innerWidth;
        let height = 400; // 全画面にしたい場合：window.innerHeight;

        return {
            scene: scene,
            object_cube: object_cube,
            light_ambient: light_ambient,
            light_direction: light_direction,
            camera: camera,
            renderer: renderer,
            width: width,
            height: height
        }
    },
    mounted() {
        this.init();
    },
    methods: {
        init() {
                        // scene ---------------------------------------------------
            // シーンに対してオブジェクト、ライト、カメラを配置する。
            this.scene = new THREE.Scene();

            // scene.fog : 霧効果・・・奥に行くほど霧がかって映る
            // scene.fog = new THREE.Fog(0x990000, 1000,2000);

            // object -----------------------------------------------
            // THREE.Mesh ： ポリゴンメッシュオブジェクトを生成
            // new THREE.Mesh(Geometry：形,Material：素材)
            this.object_cube = new THREE.Mesh(
                new THREE.BoxGeometry(50, 50, 50), //
                new THREE.MeshLambertMaterial({ color: 0x00a0e8 })
            );
            this.object_cube.position.set(0, 0, 0);
            this.scene.add(this.object_cube);

            // light -----------------------------------------------

            // 環境光 ************
            // THREE.AmbientLight ： 環境光・・・オブジェクトの全ての面を均等に照らすライト。影が出来ない。
            // new THREE.AmbientLight(光の色,光の強さ[省略可])
            this.light_ambient = new THREE.AmbientLight(0xffffff, 1);
            this.scene.add(this.light_ambient);

            // 平行光源 ************
            // THREE.DirectionalLight ： 平行光源・・・特定の方向へのライト。影が出来る。
            // new THREE.DirectionalLight(光の色,光の強さ[省略可])
            this.light_direction = new THREE.DirectionalLight(0xffffff, 1);
            // DirectionalLightの位置
            this.light_direction.position.set(50, 10, 5);
            // DirectionalLightの対象オブジェクト
            this.light_direction.target = this.object_cube;
            this.scene.add(this.light_direction);

            // camera --------------------------------------------------
            this.camera = new THREE.PerspectiveCamera(45, this.width / this.height, 1, 1000);
            this.camera.position.set(200, 100, 300);
            this.camera.lookAt(this.scene.position);

            // renderer ------------------------------------------------
            this.renderer = new THREE.WebGLRenderer({ antialias: true });
            this.renderer.setSize(this.width, this.height);
            this.renderer.setClearColor(0xefefef);
            this.renderer.setPixelRatio(window.devicePixelRatio);
            this.$refs.stage.appendChild(this.renderer.domElement);
            this.renderer.render(this.scene, this.camera);

            this.animate()
        },
        animate() {
            this.object_cube.rotation.x += 0.01;
            this.object_cube.rotation.y += 0.01;
            this.renderer.render(this.scene, this.camera);
            requestAnimationFrame(this.animate);
        }
    }
};
</script>