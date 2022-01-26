一整个机器人通过一个组对象`Group`表示，然后一条腿用一个组对象`Group`表示，一条腿假设包含大腿和小腿两个网格模型`Mesh`，大腿和小腿两个网格模型可以作为父对象腿`Group`的两个字对象，`Group`表示的两条腿又可以作为机器人`Group`的两个子对象，这样的话就构成了`机器人——腿——大腿、小腿`三个层级，就像一颗树一样可以一直分叉，如果根对象机器人的位置变化，那么腿也会跟着变化。

对于Threejs中一样，如果`Mesh`是`Group`的子对象，如果`Group`平移变化，`Mesh`的位置同样跟着父对象`Group`平移变化。

`Group`的基类是`Object3D`，自然`Group`的方法和属性可以查看文档中`Object3D`的介绍。在Three.js编程指南中会通过`Object3D`创建一个父对象，这两个类用哪个都行，`Group`相比较`Object3D`更语义化，建议使用`Group`作为点、线、网格等模型的父对象，用来构建一个层级模型。

### `.add()`方法

> 场景对象`Scene`的方法`.add()`，用来把模型对象、光源对象添加到场景中。

组对象`Group`和场景对象`Scene`一样，`.add()`方法都继承自基类`Object3D`。

```JavaScript
var mesh = new THREE.Mesh();
var group = new THREE.Group();
//网格模型添加到组中，网格模型是group的子对象
group.add(mesh);
//group添加到场景中,group作为场景对象的子对象
scene.add(group);
```

调用Threejs的API手动创建一个机器人层级模型，可以通过`.log()`在浏览器控制台查看机器人`personGroup`的结构,通过`.children`属性可以查看访问父对象的所有子对象。

```JavaScript
// 头部网格模型和组
var headMesh = sphereMesh(10, 0, 0, 0);
headMesh.name = "脑壳"
var leftEyeMesh = sphereMesh(1, 8, 5, 4);
leftEyeMesh.name = "左眼"
var rightEyeMesh = sphereMesh(1, 8, 5, -4);
rightEyeMesh.name = "右眼"
var headGroup = new THREE.Group();
headGroup.name = "头部"
headGroup.add(headMesh, leftEyeMesh, rightEyeMesh);
// 身体网格模型和组
var neckMesh = cylinderMesh(3, 10, 0, -15, 0);
neckMesh.name = "脖子"
var bodyMesh = cylinderMesh(14, 30, 0, -35, 0);
bodyMesh.name = "腹部"
var leftLegMesh = cylinderMesh(4, 60, 0, -80, -7);
leftLegMesh.name = "左腿"
var rightLegMesh = cylinderMesh(4, 60, 0, -80, 7);
rightLegMesh.name = "右腿"
var legGroup = new THREE.Group();
legGroup.name = "腿"
legGroup.add(leftLegMesh, rightLegMesh);
var bodyGroup = new THREE.Group();
bodyGroup.name = "身体"
bodyGroup.add(neckMesh, bodyMesh, legGroup);
// 人Group
var personGroup = new THREE.Group();
personGroup.name = "机器人"
personGroup.add(headGroup, bodyGroup)
personGroup.translateY(50)
scene.add(personGroup);

console.log('控制台查看机器人结构',personGroup);
```

### 递归遍历方法`.traverse()`

Threejs层级模型就是一个树结构，可以通过递归遍历的算法去遍历Threejs一个模型对象的所有后代

```js
scene.traverse(function(obj) {
  if (obj.type === "Group") {
    console.log(obj.name);
  }
  if (obj.type === "Mesh") {
    console.log('  ' + obj.name);
    obj.material.color.set(0xffff00);
  }
  if (obj.name === "左眼" | obj.name === "右眼") {
    obj.material.color.set(0x000000)
  }
  // 打印id属性
  console.log(obj.id);
  // 打印该对象的父对象
  console.log(obj.parent);
  // 打印该对象的子对象
  console.log(obj.children);
})
```

### 查找某个具体的模型

```js
// 遍历查找对象的子对象，返回name对应的对象（name是可以重名的，返回第一个）
var nameNode = scene.getObjectByName ( "左腿" );
nameNode.material.color.set(0xff0000);
// 遍历查找对象的子对象，并返回id对应的对象
var idNode = scene.getObjectById ( 4 );
console.log(idNode);
```

