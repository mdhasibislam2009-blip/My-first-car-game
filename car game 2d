<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>3D Highway Speed</title>
    <style>
        body { margin: 0; overflow: hidden; background: #87CEEB; touch-action: none; }
        #ui { position: fixed; top: 15px; width: 100%; text-align: center; color: yellow; font-family: sans-serif; font-size: 25px; text-shadow: 2px 2px 4px #000; z-index: 10; }
    </style>
</head>
<body>

    <div id="ui">Score: <span id="score">0</span></div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

    <script>
        let scene, camera, renderer, player, enemy, score = 0, speed = 0.4;
        let trees = [];

        function init() {
            // ১. সিন এবং ক্যামেরা
            scene = new THREE.Scene();
            scene.background = new THREE.Color(0x87CEEB);
            camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            camera.position.set(0, 4, 10);
            camera.lookAt(0, 0, -5);

            // ২. রেন্ডারার
            renderer = new THREE.WebGLRenderer({ antialias: true });
            renderer.setSize(window.innerWidth, window.innerHeight);
            document.body.appendChild(renderer.domElement);

            // ৩. আলো
            const light = new THREE.DirectionalLight(0xffffff, 1.5);
            light.position.set(5, 10, 5);
            scene.add(light);
            scene.add(new THREE.AmbientLight(0x707070));

            // ৪. রাস্তা ও ঘাস
            const road = new THREE.Mesh(new THREE.PlaneGeometry(12, 1000), new THREE.MeshPhongMaterial({color: 0x333333}));
            road.rotation.x = -Math.PI / 2;
            scene.add(road);

            const grass = new THREE.Mesh(new THREE.PlaneGeometry(100, 1000), new THREE.MeshPhongMaterial({color: 0x348C31}));
            grass.rotation.x = -Math.PI / 2;
            grass.position.y = -0.1;
            scene.add(grass);

            // ৫. রিয়েল গাড়ি তৈরির ফাংশন
            function makeCar(color) {
                const group = new THREE.Group();
                const body = new THREE.Mesh(new THREE.BoxGeometry(1.5, 0.6, 2.8), new THREE.MeshPhongMaterial({color: color}));
                body.position.y = 0.5;
                group.add(body);
                // জানালা
                const win = new THREE.Mesh(new THREE.BoxGeometry(1.2, 0.5, 1.2), new THREE.MeshPhongMaterial({color: 0x111111}));
                win.position.set(0, 0.9, 0);
                group.add(win);
                // হেডলাইট
                const h = new THREE.Mesh(new THREE.BoxGeometry(0.3, 0.2, 0.1), new THREE.MeshBasicMaterial({color: 0xffff00}));
                h.position.set(-0.5, 0.5, -1.4); group.add(h);
                const h2 = h.clone(); h2.position.x = 0.5; group.add(h2);
                return group;
            }

            player = makeCar(0x0077ff); scene.add(player);
            enemy = makeCar(0xff2200); resetEnemy(); scene.add(enemy);

            // ৬. গাছপালা
            for(let i=0; i<30; i++) {
                addTree(8, -i*15); addTree(-8, -i*15);
            }

            function addTree(x, z) {
                const t = new THREE.Mesh(new THREE.CylinderGeometry(0.2, 0.2, 1.5), new THREE.MeshPhongMaterial({color: 0x5d4037}));
                const l = new THREE.Mesh(new THREE.ConeGeometry(2, 4, 8), new THREE.MeshPhongMaterial({color: 0x006400}));
                l.position.y = 2.5;
                const tree = new THREE.Group(); tree.add(t); tree.add(l);
                tree.position.set(x, 0.7, z); scene.add(tree); trees.push(tree);
            }

            // ৭. কন্ট্রোল
            const move = (e) => {
                let x = e.touches ? e.touches[0].clientX : e.clientX;
                player.position.x = ((x / window.innerWidth) * 2 - 1) * 6;
            };
            window.addEventListener('mousemove', move);
            window.addEventListener('touchmove', move);

            animate();
        }

        function resetEnemy() {
            enemy.position.z = -80;
            enemy.position.x = (Math.random() - 0.5) * 10;
        }

        function animate() {
            requestAnimationFrame(animate);
            enemy.position.z += speed;
            if (enemy.position.z > 15) {
                resetEnemy(); score++;
                document.getElementById('score').innerText = score;
                speed += 0.03; // গতি বৃদ্ধি
            }
            trees.forEach(t => {
                t.position.z += speed;
                if(t.position.z > 20) t.position.z = -150;
            });
            if (Math.abs(player.position.x - enemy.position.x) < 1.5 && Math.abs(player.position.z - enemy.position.z) < 2.5) {
                alert("Game Over! Score: " + score);
                location.reload();
            }
            renderer.render(scene, camera);
        }

        init();
    </script>
</body>
</html>
