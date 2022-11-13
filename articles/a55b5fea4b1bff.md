---
title: "Next.js のページに three.js を載せてみる"
emoji: "😸"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Next.js", "three.js"]
published: true
---

## 完成品

![](/images/cube-animation.gif)

https://next-profile-page.vercel.app/threeSample

## ソースコード

基本的にこちらの記事とthree.js 公式、ICS MEDIAさんを参考にさせていただきました。ありがとうございます。

https://zenn.dev/hironorioka28/articles/5674b57278effd

https://threejs.org/docs/index.html#manual/ja/introduction/Creating-a-scene

https://ics.media/tutorial-three/material_variation/

既存の Next.js のプロジェクトの `src/pages` 配下に以下のようなファイルを作成します。

```TypeScript:threeSample.tsx
import { NextPage } from "next";
import { useEffect, useRef } from "react";
import * as THREE from "three";

const ThreeSample: NextPage = () => {
  const mountRef = useRef<HTMLDivElement>(null);
  useEffect(() => {
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);

    const el = mountRef.current;

    el?.appendChild(renderer.domElement);

    const geometry = new THREE.BoxGeometry(1, 1, 1);
    const material = new THREE.MeshNormalMaterial();
    const cube = new THREE.Mesh(geometry, material);
    scene.add(cube);
    camera.position.z = 5;

    const animate = () => {
      requestAnimationFrame(animate);
      cube.rotation.x += 0.01;
      cube.rotation.y += 0.01;
      renderer.render(scene, camera);
    };
    animate();
  });

  return <div ref={mountRef} />;
};

export default ThreeSample;
```

three.js のコード部分は公式チュートリアル通りで、マテリアルだけ ICS MEDIA さんの一覧を見て適当に選びました。

## 感想

three.js 面白いので色々やってみたいです。ついでに Next.js (React) も一緒に学んでいきたいです。
