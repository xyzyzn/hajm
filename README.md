# hajm
این یک سایت برای بدست آوردن حجم و مساحت کل اشکال هندسی است
<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>محاسبه حجم و مساحت اشکال هندسی</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128/examples/js/controls/OrbitControls.js"></script>
    <style>
        :root {
            --primary: #3498db;
            --secondary: #2ecc71;
            --dark: #2c3e50;
            --light: #ecf0f1;
            --danger: #e74c3c;
            --warning: #f39c12;
            --purple: #9b59b6;
            --teal: #1abc9c;
        }
        
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #1a2a6c);
            color: var(--light);
            line-height: 1.6;
            padding: 20px;
            min-height: 100vh;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
        }
        
        @media (max-width: 900px) {
            .container {
                grid-template-columns: 1fr;
            }
        }
        
        header {
            text-align: center;
            padding: 20px 0;
            margin-bottom: 30px;
            background: rgba(44, 62, 80, 0.7);
            border-radius: 15px;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
        }
        
        h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            color: var(--secondary);
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
        }
        
        .subtitle {
            font-size: 1.2rem;
            color: var(--light);
            opacity: 0.9;
        }
        
        .card {
            background: rgba(44, 62, 80, 0.85);
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
            margin-bottom: 30px;
            transition: transform 0.3s ease;
        }
        
        .card:hover {
            transform: translateY(-5px);
        }
        
        .card-title {
            font-size: 1.5rem;
            margin-bottom: 20px;
            color: var(--primary);
            border-bottom: 2px solid var(--secondary);
            padding-bottom: 10px;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 500;
        }
        
        select, input {
            width: 100%;
            padding: 14px;
            border-radius: 10px;
            border: 2px solid var(--primary);
            background: rgba(236, 240, 241, 0.1);
            color: var(--light);
            font-size: 16px;
            transition: all 0.3s ease;
        }
        
        select:focus, input:focus {
            outline: none;
            border-color: var(--secondary);
            background: rgba(236, 240, 241, 0.15);
            box-shadow: 0 0 0 3px rgba(46, 204, 113, 0.3);
        }
        
        .dimensions {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }
        
        button {
            width: 100%;
            padding: 16px;
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        
        button:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }
        
        button:active {
            transform: translateY(1px);
        }
        
        .result-card {
            background: rgba(44, 62, 80, 0.9);
        }
        
        .results {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }
        
        .result-box {
            background: rgba(0, 0, 0, 0.2);
            padding: 20px;
            border-radius: 10px;
            text-align: center;
        }
        
        .result-value {
            font-size: 2rem;
            font-weight: 700;
            color: var(--secondary);
            margin-top: 10px;
        }
        
        .result-label {
            font-size: 1.1rem;
            color: var(--primary);
        }
        
        #shape-container {
            height: 400px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 10px;
            overflow: hidden;
            position: relative;
        }
        
        #shape-canvas {
            width: 100%;
            height: 100%;
        }
        
        .shape-info {
            position: absolute;
            bottom: 15px;
            left: 15px;
            background: rgba(0, 0, 0, 0.6);
            padding: 10px 15px;
            border-radius: 10px;
            font-size: 14px;
        }
        
        .instructions {
            position: absolute;
            top: 15px;
            right: 15px;
            background: rgba(0, 0, 0, 0.6);
            padding: 8px 12px;
            border-radius: 10px;
            font-size: 14px;
            text-align: center;
        }
        
        .shape-types {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            margin-top: 20px;
        }
        
        .shape-type {
            flex: 1;
            min-width: 120px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 10px;
            padding: 15px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .shape-type:hover {
            background: rgba(52, 152, 219, 0.3);
            transform: scale(1.05);
        }
        
        .shape-type.active {
            background: var(--primary);
            transform: scale(1.05);
        }
        
        .shape-icon {
            font-size: 2.5rem;
            margin-bottom: 10px;
        }
        
        footer {
            text-align: center;
            padding: 20px;
            margin-top: 30px;
            background: rgba(44, 62, 80, 0.7);
            border-radius: 15px;
        }
        
        .help-text {
            color: var(--warning);
            font-size: 0.9rem;
            margin-top: 8px;
            display: block;
        }
        
        .polygon-name {
            display: inline-block;
            padding: 4px 8px;
            background: var(--purple);
            border-radius: 5px;
            margin-top: 5px;
            font-size: 0.9rem;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>محاسبه حجم و مساحت اشکال هندسی</h1>
            <p class="subtitle">اطلاعات شکل خود را وارد کنید و حجم و مساحت را به همراه نمایش سه بعدی دریافت نمایید</p>
        </header>
        
        <div class="card">
            <h2 class="card-title">انتخاب شکل و ابعاد</h2>
            
            <div class="form-group">
                <label for="shape-type">نوع شکل هندسی:</label>
                <select id="shape-type">
                    <option value="cube">مکعب مستطیل</option>
                    <option value="sphere">کره</option>
                    <option value="cylinder">استوانه</option>
                    <option value="cone">مخروط</option>
                    <option value="pyramid">هرم مربعی</option>
                    <option value="polygon-pyramid">هرم چندضلعی</option>
                    <option value="prism">منشور چندضلعی</option>
                </select>
            </div>
            
            <div class="form-group" id="cube-dimensions">
                <label>ابعاد مکعب مستطیل:</label>
                <div class="dimensions">
                    <div>
                        <label for="length">طول (cm)</label>
                        <input type="number" id="length" min="1" value="5" step="0.1">
                    </div>
                    <div>
                        <label for="width">عرض (cm)</label>
                        <input type="number" id="width" min="1" value="4" step="0.1">
                    </div>
                    <div>
                        <label for="height">ارتفاع (cm)</label>
                        <input type="number" id="height" min="1" value="3" step="0.1">
                    </div>
                </div>
            </div>
            
            <div class="form-group" id="sphere-dimensions" style="display: none;">
                <label>ابعاد کره:</label>
                <div class="dimensions">
                    <div>
                        <label for="radius">شعاع (cm)</label>
                        <input type="number" id="radius" min="1" value="3" step="0.1">
                    </div>
                </div>
            </div>
            
            <div class="form-group" id="cylinder-dimensions" style="display: none;">
                <label>ابعاد استوانه:</label>
                <div class="dimensions">
                    <div>
                        <label for="cylinder-radius">شعاع (cm)</label>
                        <input type="number" id="cylinder-radius" min="1" value="3" step="0.1">
                    </div>
                    <div>
                        <label for="cylinder-height">ارتفاع (cm)</label>
                        <input type="number" id="cylinder-height" min="1" value="6" step="0.1">
                    </div>
                </div>
            </div>
            
            <div class="form-group" id="cone-dimensions" style="display: none;">
                <label>ابعاد مخروط:</label>
                <div class="dimensions">
                    <div>
                        <label for="cone-radius">شعاع (cm)</label>
                        <input type="number" id="cone-radius" min="1" value="3" step="0.1">
                    </div>
                    <div>
                        <label for="cone-height">ارتفاع (cm)</label>
                        <input type="number" id="cone-height" min="1" value="7" step="0.1">
                    </div>
                </div>
            </div>
            
            <div class="form-group" id="pyramid-dimensions" style="display: none;">
                <label>ابعاد هرم مربعی:</label>
                <div class="dimensions">
                    <div>
                        <label for="base">طول پایه (cm)</label>
                        <input type="number" id="base" min="1" value="4" step="0.1">
                    </div>
                    <div>
                        <label for="pyramid-height">ارتفاع (cm)</label>
                        <input type="number" id="pyramid-height" min="1" value="6" step="0.1">
                    </div>
                </div>
            </div>
            
            <div class="form-group" id="polygon-pyramid-dimensions" style="display: none;">
                <label>ابعاد هرم چندضلعی:</label>
                <div class="dimensions">
                    <div>
                        <label for="polygon-base">طول ضلع پایه (cm)</label>
                        <input type="number" id="polygon-base" min="1" value="3" step="0.1">
                    </div>
                    <div>
                        <label for="polygon-sides">تعداد اضلاع قاعده</label>
                        <select id="polygon-sides">
                            <option value="3">3 (مثلثی)</option>
                            <option value="4">4 (مربعی)</option>
                            <option value="5">5 (پنج ضلعی)</option>
                            <option value="6">6 (شش ضلعی)</option>
                            <option value="7">7 (هفت ضلعی)</option>
                            <option value="8">8 (هشت ضلعی)</option>
                        </select>
                        <span class="polygon-name" id="pyramid-poly-name">مثلثی</span>
                    </div>
                    <div>
                        <label for="polygon-pyramid-height">ارتفاع (cm)</label>
                        <input type="number" id="polygon-pyramid-height" min="1" value="7" step="0.1">
                    </div>
                </div>
            </div>
            
            <div class="form-group" id="prism-dimensions" style="display: none;">
                <label>ابعاد منشور چندضلعی:</label>
                <div class="dimensions">
                    <div>
                        <label for="prism-base">طول ضلع پایه (cm)</label>
                        <input type="number" id="prism-base" min="1" value="3" step="0.1">
                    </div>
                    <div>
                        <label for="prism-sides">تعداد اضلاع قاعده</label>
                        <select id="prism-sides">
                            <option value="3">3 (مثلثی)</option>
                            <option value="4">4 (مربعی)</option>
                            <option value="5">5 (پنج ضلعی)</option>
                            <option value="6">6 (شش ضلعی)</option>
                            <option value="7">7 (هفت ضلعی)</option>
                            <option value="8">8 (هشت ضلعی)</option>
                        </select>
                        <span class="polygon-name" id="prism-poly-name">مثلثی</span>
                    </div>
                    <div>
                        <label for="prism-height">ارتفاع (cm)</label>
                        <input type="number" id="prism-height" min="1" value="8" step="0.1">
                    </div>
                </div>
            </div>
            
            <button id="calculate-btn">محاسبه حجم و مساحت</button>
            <span class="help-text">برای چرخش شکل سه بعدی، آن را با ماوس یا انگشت بکشید</span>
        </div>
        
        <div class="card result-card">
            <h2 class="card-title">نتایج محاسبات</h2>
            
            <div class="results">
                <div class="result-box">
                    <div class="result-label">حجم</div>
                    <div class="result-value" id="volume-result">60.00 cm³</div>
                </div>
                
                <div class="result-box">
                    <div class="result-label">مساحت کل</div>
                    <div class="result-value" id="area-result">94.00 cm²</div>
                </div>
            </div>
            
            <div class="card-title" style="margin-top: 30px;">نمایش سه بعدی</div>
            <div id="shape-container">
                <canvas id="shape-canvas"></canvas>
                <div class="instructions">برای چرخش: ماوس را بکشید<br>برای زوم: غلتک ماوس</div>
                <div class="shape-info" id="shape-info">مکعب مستطیل (5×4×3)</div>
            </div>
        </div>
        
        <div class="card">
            <h2 class="card-title">اشکال هندسی</h2>
            <div class="shape-types">
                <div class="shape-type active" data-shape="cube">
                    <div class="shape-icon">◻️</div>
                    <div>مکعب مستطیل</div>
                </div>
                <div class="shape-type" data-shape="sphere">
                    <div class="shape-icon">🔵</div>
                    <div>کره</div>
                </div>
                <div class="shape-type" data-shape="cylinder">
                    <div class="shape-icon">🔼</div>
                    <div>استوانه</div>
                </div>
                <div class="shape-type" data-shape="cone">
                    <div class="shape-icon">📐</div>
                    <div>مخروط</div>
                </div>
                <div class="shape-type" data-shape="pyramid">
                    <div class="shape-icon">🔺</div>
                    <div>هرم مربعی</div>
                </div>
                <div class="shape-type" data-shape="polygon-pyramid">
                    <div class="shape-icon">🔺🔺</div>
                    <div>هرم چندضلعی</div>
                </div>
                <div class="shape-type" data-shape="prism">
                    <div class="shape-icon">⬢</div>
                    <div>منشور</div>
                </div>
            </div>
        </div>
    </div>
    
    <footer>
        <p>برنامه محاسبه حجم و مساحت اشکال هندسی - ایجاد شده با Three.js</p>
        <p>برای چرخش شکل سه بعدی، آن را با ماوس یا انگشت خود بکشید</p>
    </footer>

    <script>
        // تنظیمات Three.js
        let scene, camera, renderer, controls, shapeMesh;
        let container = document.getElementById('shape-container');
        
        // نام‌های چندضلعی‌ها
        const polygonNames = {
            3: 'مثلثی',
            4: 'مربعی',
            5: 'پنج ضلعی',
            6: 'شش ضلعی',
            7: 'هفت ضلعی',
            8: 'هشت ضلعی'
        };
        
        // تنظیمات اولیه
        function initThreeJS() {
            // ایجاد صحنه
            scene = new THREE.Scene();
            scene.background = new THREE.Color(0x1a2a6c);
            
            // ایجاد دوربین
            camera = new THREE.PerspectiveCamera(
                75, 
                container.clientWidth / container.clientHeight, 
                0.1, 
                1000
            );
            camera.position.z = 10;
            
            // ایجاد رندرر
            renderer = new THREE.WebGLRenderer({ 
                canvas: document.getElementById('shape-canvas'),
                antialias: true 
            });
            renderer.setSize(container.clientWidth, container.clientHeight);
            renderer.setPixelRatio(window.devicePixelRatio);
            
            // افزودن نور
            const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
            scene.add(ambientLight);
            
            const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
            directionalLight.position.set(1, 1, 1);
            scene.add(directionalLight);
            
            // افزودن کنترل‌های چرخش
            controls = new THREE.OrbitControls(camera, renderer.domElement);
            controls.enableDamping = true;
            controls.dampingFactor = 0.05;
            
            // ایجاد شبکه جهت نمایش
            const gridHelper = new THREE.GridHelper(20, 20);
            gridHelper.rotation.x = Math.PI / 2;
            scene.add(gridHelper);
            
            // ایجاد محورها
            const axesHelper = new THREE.AxesHelper(5);
            scene.add(axesHelper);
            
            // ایجاد شکل اولیه
            createShape('cube', {length: 5, width: 4, height: 3});
            
            // تنظیم اندازه هنگام تغییر سایز پنجره
            window.addEventListener('resize', onWindowResize);
            
            // شروع انیمیشن
            animate();
        }
        
        // به‌روزرسانی اندازه رندرر هنگام تغییر سایز پنجره
        function onWindowResize() {
            camera.aspect = container.clientWidth / container.clientHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(container.clientWidth, container.clientHeight);
        }
        
        // تابع انیمیشن
        function animate() {
            requestAnimationFrame(animate);
            controls.update();
            renderer.render(scene, camera);
        }
        
        // ایجاد شکل هندسی بر اساس نوع
        function createShape(type, dimensions) {
            // حذف شکل قبلی اگر وجود دارد
            if (shapeMesh) {
                scene.remove(shapeMesh);
            }
            
            let geometry;
            
            switch(type) {
                case 'cube':
                    geometry = new THREE.BoxGeometry(
                        dimensions.length, 
                        dimensions.height, 
                        dimensions.width
                    );
                    break;
                    
                case 'sphere':
                    geometry = new THREE.SphereGeometry(
                        dimensions.radius, 
                        32, 
                        32
                    );
                    break;
                    
                case 'cylinder':
                    geometry = new THREE.CylinderGeometry(
                        dimensions.radius, 
                        dimensions.radius, 
                        dimensions.height, 
                        32
                    );
                    break;
                    
                case 'cone':
                    geometry = new THREE.ConeGeometry(
                        dimensions.radius, 
                        dimensions.height, 
                        32
                    );
                    break;
                    
                case 'pyramid':
                    geometry = new THREE.ConeGeometry(
                        dimensions.base * 0.8, 
                        dimensions.height, 
                        4
                    );
                    break;
                    
                case 'polygon-pyramid':
                    geometry = new THREE.ConeGeometry(
                        dimensions.base * 0.5, 
                        dimensions.height, 
                        dimensions.sides
                    );
                    break;
                    
                case 'prism':
                    geometry = new THREE.CylinderGeometry(
                        dimensions.base * 0.5, 
                        dimensions.base * 0.5, 
                        dimensions.height, 
                        dimensions.sides
                    );
                    break;
            }
            
            // ایجاد متریال با شفافیت
            const material = new THREE.MeshPhongMaterial({
                color: 0x3498db,
                wireframe: false,
                transparent: true,
                opacity: 0.85,
                shininess: 100,
                specular: 0x1188ff
            });
            
            // ایجاد مش
            shapeMesh = new THREE.Mesh(geometry, material);
            scene.add(shapeMesh);
            
            // تنظیم موقعیت دوربین بر اساس اندازه شکل
            let size = 0;
            switch(type) {
                case 'cube':
                    size = Math.max(dimensions.length, dimensions.width, dimensions.height);
                    break;
                case 'sphere':
                    size = dimensions.radius * 2;
                    break;
                case 'cylinder':
                case 'cone':
                    size = Math.max(dimensions.radius * 2, dimensions.height);
                    break;
                case 'pyramid':
                    size = Math.max(dimensions.base * 1.5, dimensions.height);
                    break;
                case 'polygon-pyramid':
                case 'prism':
                    size = Math.max(dimensions.base * 2, dimensions.height);
                    break;
            }
            
            camera.position.z = size * 2.5;
            controls.update();
        }
        
        // محاسبه حجم و مساحت
        function calculateVolumeAndArea() {
            const shapeType = document.getElementById('shape-type').value;
            let volume = 0;
            let area = 0;
            let dimensionsText = '';
            
            switch(shapeType) {
                case 'cube':
                    const length = parseFloat(document.getElementById('length').value);
                    const width = parseFloat(document.getElementById('width').value);
                    const height = parseFloat(document.getElementById('height').value);
                    
                    volume = length * width * height;
                    area = 2 * (length*width + length*height + width*height);
                    dimensionsText = `مکعب مستطیل (${length}×${width}×${height})`;
                    createShape('cube', {length, width, height});
                    break;
                    
                case 'sphere':
                    const radius = parseFloat(document.getElementById('radius').value);
                    
                    volume = (4/3) * Math.PI * Math.pow(radius, 3);
                    area = 4 * Math.PI * Math.pow(radius, 2);
                    dimensionsText = `کره (شعاع: ${radius})`;
                    createShape('sphere', {radius});
                    break;
                    
                case 'cylinder':
                    const cylinderRadius = parseFloat(document.getElementById('cylinder-radius').value);
                    const cylinderHeight = parseFloat(document.getElementById('cylinder-height').value);
                    
                    volume = Math.PI * Math.pow(cylinderRadius, 2) * cylinderHeight;
                    area = 2 * Math.PI * cylinderRadius * (cylinderRadius + cylinderHeight);
                    dimensionsText = `استوانه (شعاع: ${cylinderRadius}, ارتفاع: ${cylinderHeight})`;
                    createShape('cylinder', {radius: cylinderRadius, height: cylinderHeight});
                    break;
                    
                case 'cone':
                    const coneRadius = parseFloat(document.getElementById('cone-radius').value);
                    const coneHeight = parseFloat(document.getElementById('cone-height').value);
                    const slantHeight = Math.sqrt(Math.pow(coneRadius, 2) + Math.pow(coneHeight, 2));
                    
                    volume = (1/3) * Math.PI * Math.pow(coneRadius, 2) * coneHeight;
                    area = Math.PI * coneRadius * (coneRadius + slantHeight);
                    dimensionsText = `مخروط (شعاع: ${coneRadius}, ارتفاع: ${coneHeight})`;
                    createShape('cone', {radius: coneRadius, height: coneHeight});
                    break;
                    
                case 'pyramid':
                    const base = parseFloat(document.getElementById('base').value);
                    const pyramidHeight = parseFloat(document.getElementById('pyramid-height').value);
                    const baseArea = Math.pow(base, 2);
                    const lateralArea = 2 * base * Math.sqrt(Math.pow(base/2, 2) + Math.pow(pyramidHeight, 2));
                    
                    volume = (1/3) * baseArea * pyramidHeight;
                    area = baseArea + lateralArea;
                    dimensionsText = `هرم مربعی (پایه: ${base}, ارتفاع: ${pyramidHeight})`;
                    createShape('pyramid', {base, height: pyramidHeight});
                    break;
                    
                case 'polygon-pyramid':
                    const polygonBase = parseFloat(document.getElementById('polygon-base').value);
                    const polygonSides = parseInt(document.getElementById('polygon-sides').value);
                    const polygonHeight = parseFloat(document.getElementById('polygon-pyramid-height').value);
                    
                    // محاسبه مساحت قاعده
                    const polygonBaseArea = (polygonSides * Math.pow(polygonBase, 2)) / (4 * Math.tan(Math.PI / polygonSides));
                    
                    // محاسبه مساحت جانبی
                    const apothem = polygonBase / (2 * Math.tan(Math.PI / polygonSides));
                    const slantHeightPyramid = Math.sqrt(Math.pow(apothem, 2) + Math.pow(polygonHeight, 2));
                    const lateralAreaPyramid = (polygonSides * polygonBase * slantHeightPyramid) / 2;
                    
                    volume = (1/3) * polygonBaseArea * polygonHeight;
                    area = polygonBaseArea + lateralAreaPyramid;
                    dimensionsText = `هرم ${polygonNames[polygonSides]} (ضلع: ${polygonBase}, ارتفاع: ${polygonHeight})`;
                    createShape('polygon-pyramid', {base: polygonBase, height: polygonHeight, sides: polygonSides});
                    break;
                    
                case 'prism':
                    const prismBase = parseFloat(document.getElementById('prism-base').value);
                    const prismSides = parseInt(document.getElementById('prism-sides').value);
                    const prismHeight = parseFloat(document.getElementById('prism-height').value);
                    
                    // محاسبه مساحت قاعده
                    const prismBaseArea = (prismSides * Math.pow(prismBase, 2)) / (4 * Math.tan(Math.PI / prismSides));
                    
                    // محاسبه مساحت جانبی
                    const lateralAreaPrism = prismSides * prismBase * prismHeight;
                    
                    volume = prismBaseArea * prismHeight;
                    area = 2 * prismBaseArea + lateralAreaPrism;
                    dimensionsText = `منشور ${polygonNames[prismSides]} (ضلع: ${prismBase}, ارتفاع: ${prismHeight})`;
                    createShape('prism', {base: prismBase, height: prismHeight, sides: prismSides});
                    break;
            }
            
            // نمایش نتایج
            document.getElementById('volume-result').textContent = volume.toFixed(2) + ' cm³';
            document.getElementById('area-result').textContent = area.toFixed(2) + ' cm²';
            document.getElementById('shape-info').textContent = dimensionsText;
        }
        
        // تغییر نمایش فرم ورودی بر اساس نوع شکل
        function toggleDimensionsInputs() {
            const shapeType = document.getElementById('shape-type').value;
            
            // مخفی کردن همه بخش‌ها
            document.getElementById('cube-dimensions').style.display = 'none';
            document.getElementById('sphere-dimensions').style.display = 'none';
            document.getElementById('cylinder-dimensions').style.display = 'none';
            document.getElementById('cone-dimensions').style.display = 'none';
            document.getElementById('pyramid-dimensions').style.display = 'none';
            document.getElementById('polygon-pyramid-dimensions').style.display = 'none';
            document.getElementById('prism-dimensions').style.display = 'none';
            
            // نمایش بخش مربوطه
            document.getElementById(`${shapeType}-dimensions`).style.display = 'block';
            
            // به‌روزرسانی شکل‌های سریع
            document.querySelectorAll('.shape-type').forEach(el => {
                el.classList.remove('active');
                if (el.dataset.shape === shapeType) {
                    el.classList.add('active');
                }
            });
            
            // محاسبه مجدد
            calculateVolumeAndArea();
        }
        
        // رویدادهای صفحه
        document.getElementById('shape-type').addEventListener('change', toggleDimensionsInputs);
        document.getElementById('calculate-btn').addEventListener('click', calculateVolumeAndArea);
        
        // رویدادهای شکل‌های سریع
        document.querySelectorAll('.shape-type').forEach(el => {
            el.addEventListener('click', () => {
                document.getElementById('shape-type').value = el.dataset.shape;
                toggleDimensionsInputs();
            });
        });
        
        // به‌روزرسانی نام چندضلعی‌ها
        document.getElementById('polygon-sides').addEventListener('change', function() {
            document.getElementById('pyramid-poly-name').textContent = polygonNames[this.value];
            calculateVolumeAndArea();
        });
        
        document.getElementById('prism-sides').addEventListener('change', function() {
            document.getElementById('prism-poly-name').textContent = polygonNames[this.value];
            calculateVolumeAndArea();
        });
        
        // مقداردهی اولیه
        window.addEventListener('load', () => {
            initThreeJS();
            calculateVolumeAndArea();
        });
    </script>
</body>
</html>
