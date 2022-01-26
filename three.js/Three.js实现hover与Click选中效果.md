## 二、如何实现

1. 使用Three.js提供的`光线投射Raycaster`模块
2. 通过Raycaster将物体在场景中的位置与鼠标的位置进行对比
3. 当鼠标的平面坐标与场景中物体的平面坐标发生重叠时认为选中了物体

## 三、代码实现

```js
<script type="module">
      import * as THREE from "./node_modules/three/build/three.module.js";
      import { OrbitControls } from "./node_modules/three/examples/jsm/controls/OrbitControls.js";

      let scene, camera, renderer;

      // 光线投射（获取鼠标点击对象）
      const raycaster = new THREE.Raycaster();
      const mouse = new THREE.Vector2();
      let cuurrentObj = null;

      const init = () => {
        // 场景
        scene = new THREE.Scene();
        // 相机
        camera = new THREE.PerspectiveCamera(
          70,
          window.innerWidth / window.innerHeight,
          1,
          100000
        );
        camera.position.set(50, 50, 50);
        camera.position.y = 50;
        // 渲染器
        renderer = new THREE.WebGLRenderer({
          antialias: true,
        });
        renderer.setPixelRatio(window.devicePixelRatio);
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);
        // 环境光
        const light = new THREE.AmbientLight(0xffffff, 0.8);
        light.layers.enable(0);
        light.layers.enable(1);
        scene.add(light);
        // 控制器
        const controls = new OrbitControls(camera, renderer.domElement);
        window.onresize = () => {
          renderer.setSize(window.innerWidth, window.innerHeight);
          camera.aspect = window.innerWidth / window.innerHeight;
          camera.updateProjectionMatrix();
        };
      };

      const drawBoxs = () => {
        // 渲染参数
        const range = 50;
        const width = 4;
        // 添加渲染区域网格
        const helper = new THREE.BoxHelper(
          new THREE.Mesh(
            new THREE.BoxBufferGeometry(
              range + width / 2,
              range + width / 2,
              range + width / 2
            )
          )
        );
        helper.material.color.setHex(0xffffff);
        scene.add(helper);
        // 绘制50个均匀分布的正方体
        const geometry = new THREE.BoxGeometry(width, width, width);
        for (let i = 0; i < 50; i++) {
          const material = new THREE.MeshLambertMaterial({
            color: 0xffffff * Math.random(),
          });
          const mesh = new THREE.Mesh(geometry, material);
          mesh.position.x = (Math.random() - 0.5) * range;
          mesh.position.y = (Math.random() - 0.5) * range;
          mesh.position.z = (Math.random() - 0.5) * range;
          scene.add(mesh);
        }
      };

      const mouseEvent = (e) => {
        // 将鼠标位置归一化为设备坐标。x 和 y 方向的取值范围是 (-1 to +1)
        mouse.x = (e.clientX / window.innerWidth) * 2 - 1;
        mouse.y = -(e.clientY / window.innerHeight) * 2 + 1;
        raycaster.setFromCamera(mouse, camera);
        // 计算物体和射线的焦点
        const intersects = raycaster.intersectObjects(scene.children, true);

        // 清除上一个选中对象
        cuurrentObj
          ? cuurrentObj.object.scale.set(1, 1, 1)
          : (cuurrentObj = null);

        if (intersects.length) {
          // 处理选中的最上层对象
          if (intersects[0].object.isMesh) {
            cuurrentObj = intersects[0];
            intersects[0].object.scale.set(3, 3, 3);
          }
        }
      };

      const main = () => {
        drawBoxs();
        window.addEventListener("mousemove", mouseEvent, false);
        // window.addEventListener("click", mouseEvent, false);
      };

      const render = () => {
        renderer.render(scene, camera);
        requestAnimationFrame(render);
      };
      init();
      main();
      render();
    </script>
```

## 四、注意点

1. 在场景中的所有对象都会被raycaster都会被选中，在处理时应该判断那些是自己需要处理的对象
2. 在`raycaster`判断物体是否被选中时，重叠在一起的物体也会被选中，并按照层级顺序存放在数组中