# Hydroponik-
3d
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>3D Hydroponic NFT System</title>
  <style>
    body { margin: 0; }
    canvas { display: block; }
  </style>
</head>
<body>
  <script src="https://cdn.jsdelivr.net/npm/three@0.157.0/build/three.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/three@0.157.0/examples/js/controls/OrbitControls.js"></script>
  <script>
    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0xf5f5f5);

    const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.set(120, 100, 200);

    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);

    const controls = new THREE.OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;

    // Lighting
    scene.add(new THREE.AmbientLight(0xffffff, 0.4));
    const dirLight = new THREE.DirectionalLight(0xffffff, 0.8);
    dirLight.position.set(50, 100, 50);
    scene.add(dirLight);

    // Frame (rangka)
    const frameMaterial = new THREE.MeshStandardMaterial({ color: 0x555555 });
    const legGeometry = new THREE.BoxGeometry(2, 90, 2);
    const legs = [];
    for (let x of [-45, 45]) {
      for (let z of [-25, 25]) {
        const leg = new THREE.Mesh(legGeometry, frameMaterial);
        leg.position.set(x, 45, z);
        scene.add(leg);
      }
    }

    const base = new THREE.Mesh(new THREE.BoxGeometry(100, 2, 52), frameMaterial);
    base.position.set(0, 1, 0);
    scene.add(base);

    // NFT Channels (talang)
    const pipeMaterial = new THREE.MeshStandardMaterial({ color: 0x4da6ff });
    const pipeGeometry = new THREE.CylinderGeometry(3, 3, 90, 32);
    const netpotMaterial = new THREE.MeshStandardMaterial({ color: 0x222222 });
    const rockwoolMaterial = new THREE.MeshStandardMaterial({ color: 0x99cc66 });

    for (let i = 0; i < 6; i++) {
      const pipe = new THREE.Mesh(pipeGeometry, pipeMaterial);
      pipe.rotation.z = Math.PI / 2;
      pipe.position.set(0, 85 - i * 15, 0);
      scene.add(pipe);

      // Netpots
      for (let j = -35; j <= 35; j += 15) {
        const netpot = new THREE.Mesh(new THREE.CylinderGeometry(1.2, 1.2, 1, 16), netpotMaterial);
        netpot.position.set(j, pipe.position.y + 2, 0);
        scene.add(netpot);

        const rockwool = new THREE.Mesh(new THREE.CylinderGeometry(0.8, 0.8, 0.5, 12), rockwoolMaterial);
        rockwool.position.set(j, pipe.position.y + 2.7, 0);
        scene.add(rockwool);
      }
    }

    // Reservoir (penampungan)
    const reservoir = new THREE.Mesh(new THREE.BoxGeometry(40, 20, 40), new THREE.MeshStandardMaterial({ color: 0x888888 }));
    reservoir.position.set(0, 10, 60);
    scene.add(reservoir);

    // Pipa masuk (pompa)
    const inPipe = new THREE.Mesh(new THREE.CylinderGeometry(0.7, 0.7, 60, 12), new THREE.MeshStandardMaterial({ color: 0x00cc00 }));
    inPipe.rotation.z = Math.PI / 2;
    inPipe.position.set(0, 85, 30);
    scene.add(inPipe);

    // Pipa keluar (gravitasi balik ke reservoir)
    const outPipe = new THREE.Mesh(new THREE.CylinderGeometry(0.7, 0.7, 60, 12), new THREE.MeshStandardMaterial({ color: 0xcc0000 }));
    outPipe.rotation.z = Math.PI / 2;
    outPipe.position.set(0, 0, 30);
    scene.add(outPipe);

    // Render
    function animate() {
      requestAnimationFrame(animate);
      controls.update();
      renderer.render(scene, camera);
    }

    animate();

    window.addEventListener("resize", () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    });
  </script>
</body>
</html>
