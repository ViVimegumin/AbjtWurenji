<template>
    <div ref="container" class="model-container"></div>
  </template>
  
  <script>
  import * as THREE from 'three';
  import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';
  import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
  
  export default {
    name: 'GLTFModel',
    mounted() {
      this.init();
    },
    methods: {
      init() {

        const container = this.$refs.container;

        // 创建一个新的 THREE.js 场景
        const scene = new THREE.Scene();
        
        // 创建一个透视相机，设置视场角、宽高比、近裁面和远裁面
        const camera = new THREE.PerspectiveCamera(75, container.clientWidth / container.clientHeight, 0.1, 1000);
        camera.position.set(0,5,35);

        // 创建一个 WebGL 渲染器
        const renderer = new THREE.WebGLRenderer();
        renderer.setSize(window.innerWidth, window.innerHeight);
        container.appendChild(renderer.domElement);
        
        
        // 启用阴影映射
        renderer.shadowMap.enabled = true; //开启阴影
        renderer.shadowMap.type = THREE.PCFSoftShadowMap; //设置阴影类型为PCF软阴影

        // 创建一个平面作为地面
        const planeGeometry = new THREE.PlaneGeometry(200, 200, 320, 320);
        const planeMaterial = new THREE.MeshStandardMaterial({ color: 0xcccccc });
        const plane = new THREE.Mesh(planeGeometry, planeMaterial);
        plane.rotation.x = -Math.PI / 2;
        plane.receiveShadow = true; // 允许地面接收阴影
        scene.add(plane);


        renderer.setClearColor(0xb9d3ff, 1); //设置背景颜色

        //导入gltf模型
        const loader = new GLTFLoader();
        loader.load('static/ji/无人机.glb', (gltf) => {
          const model = gltf.scene;
          model.position.set(0, 2, 0); // 设置模型的位置，x, y, z 是你想要设置的坐标值
          model.castShadow = true; // 允许模型投射阴影
          scene.add(model);
        }, undefined, (error) => {
          console.error(error);
        });

        // 创建平行光
        const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
        directionalLight.position.set(10, 10, 10);
        directionalLight.castShadow = true; // 允许平行光投射阴影
        directionalLight.shadow.camera.near = 0.5;
        directionalLight.shadow.camera.far = 50;
        directionalLight.shadow.camera.left = -10;
        directionalLight.shadow.camera.right = 10;
        directionalLight.shadow.camera.top = 10;
        directionalLight.shadow.camera.bottom = -10;
        scene.add(directionalLight);

        // 添加一个环境光
        const ambientLight = new THREE.AmbientLight(0x404040);
        scene.add(ambientLight);
      
  
        // 创建一个轨道控制器
        const controls = new OrbitControls(camera, renderer.domElement); // 创建轨道控制器实例
        controls.enableDamping = true; // 启用阻尼效果
        controls.dampingFactor = 0.25; // 设置阻尼系数
        controls.enableZoom = true; // 启用缩放

        const animate = () => {
          requestAnimationFrame(animate);

          controls.update(); // 更新控制器

          renderer.render(scene, camera);
        };
  
        animate();
      }
    }
  };
  </script>
  
  <style scoped>
  .model-container {
    width: 100%;
    height: 100%;
  }
  </style>