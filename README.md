<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <外科补液导师 - 水电解质与酸碱平衡模拟器
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', 'Microsoft YaHei', sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            padding: 20px;
            color: #333;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background-color: white;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }
        
        header {
            background: linear-gradient(to right, #1a6dbb, #2c8bd1);
            color: white;
            padding: 25px 30px;
            text-align: center;
        }
        
        h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
        }
        
        .subtitle {
            font-size: 1.2rem;
            opacity: 0.9;
        }
        
        .main-content {
            display: flex;
            flex-wrap: wrap;
            padding: 20px;
        }
        
        .input-section {
            flex: 1;
            min-width: 300px;
            padding: 20px;
            border-right: 1px solid #eee;
        }
        
        .output-section {
            flex: 2;
            min-width: 500px;
            padding: 20px;
        }
        
        .section-title {
            font-size: 1.5rem;
            color: #1a6dbb;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 2px solid #eaeaea;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #444;
        }
        
        input, select {
            width: 100%;
            padding: 12px 15px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 1rem;
            transition: border 0.3s;
        }
        
        input:focus, select:focus {
            outline: none;
            border-color: #2c8bd1;
            box-shadow: 0 0 0 2px rgba(44, 139, 209, 0.2);
        }
        
        .button-group {
            display: flex;
            gap: 15px;
            margin-top: 30px;
        }
        
        button {
            padding: 14px 25px;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            flex: 1;
        }
        
        #calculate-btn {
            background-color: #1a6dbb;
            color: white;
        }
        
        #calculate-btn:hover {
            background-color: #155a9e;
            transform: translateY(-2px);
        }
        
        #reset-btn {
            background-color: #f0f0f0;
            color: #555;
        }
        
        #reset-btn:hover {
            background-color: #e0e0e0;
        }
        
        .result-box {
            background-color: #f8f9fa;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 25px;
            border-left: 5px solid #1a6dbb;
        }
        
        .result-title {
            font-size: 1.3rem;
            color: #1a6dbb;
            margin-bottom: 15px;
        }
        
        .diagnosis {
            font-size: 1.4rem;
            font-weight: 700;
            color: #d35400;
            margin: 10px 0;
            padding: 10px;
            background-color: #fff8e1;
            border-radius: 5px;
        }
        
        .parameter-display {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 15px;
            margin: 20px 0;
        }
        
        .parameter {
            background: white;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.05);
            text-align: center;
        }
        
        .param-name {
            font-size: 0.9rem;
            color: #666;
            margin-bottom: 5px;
        }
        
        .param-value {
            font-size: 1.5rem;
            font-weight: 700;
        }
        
        .normal {
            color: #27ae60;
        }
        
        .abnormal {
            color: #e74c3c;
        }
        
        .warning {
            color: #f39c12;
        }
        
        /* 体液平衡可视化对比图 */
        .comparison-container {
            margin: 20px 0;
            padding: 15px;
            background: #f8f9fa;
            border-radius: 10px;
        }
        
        .comparison-row {
            display: flex;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .comparison-label {
            width: 100px;
            font-weight: 600;
            color: #333;
        }
        
        .comparison-bar-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        
        .comparison-bar {
            height: 40px;
            border-radius: 8px;
            position: relative;
            transition: width 1s ease;
        }
        
        .normal-bar {
            background: linear-gradient(to right, #4CAF50, #8BC34A);
            border: 2px dashed #388E3C;
        }
        
        .current-bar {
            background: linear-gradient(to right, #2196F3, #03A9F4);
            border: 2px solid #1976D2;
        }
        
        .bar-label {
            position: absolute;
            left: 10px;
            top: 50%;
            transform: translateY(-50%);
            color: white;
            font-weight: 600;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
        }
        
        .comparison-legend {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 20px;
            padding-top: 15px;
            border-top: 1px solid #ddd;
        }
        
        .legend-item {
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .legend-color {
            width: 20px;
            height: 20px;
            border-radius: 4px;
        }
        
        .normal-legend {
            background: #4CAF50;
            border: 1px dashed #388E3C;
        }
        
        .deficit-legend {
            background: #F44336;
        }
        
        .shift-legend {
            background: #FF9800;
        }
        
        .fluid-shift-explanation {
            margin-top: 20px;
            padding: 15px;
            background: #E3F2FD;
            border-radius: 8px;
            border-left: 4px solid #2196F3;
        }
        
        .fluid-shift-explanation h4 {
            color: #1976D2;
            margin-bottom: 10px;
        }
        
        .fluid-shift-explanation ul {
            margin-left: 20px;
            margin-top: 10px;
        }
        
        .fluid-shift-explanation li {
            margin-bottom: 5px;
        }
        
        .treatment-plan {
            background-color: #e8f5e9;
            border-radius: 10px;
            padding: 20px;
            margin-top: 20px;
            border-left: 5px solid #4caf50;
        }
        
        .treatment-title {
            font-size: 1.3rem;
            color: #2e7d32;
            margin-bottom: 15px;
        }
        
        .treatment-step {
            margin-bottom: 10px;
            padding-left: 20px;
            position: relative;
        }
        
        .treatment-step:before {
            content: "•";
            color: #2e7d32;
            font-size: 1.5rem;
            position: absolute;
            left: 0;
        }
        
        .clinical-manifestations {
            background-color: #fff3e0;
            border-radius: 10px;
            padding: 20px;
            margin-top: 20px;
            border-left: 5px solid #f57c00;
        }
        
        .manifestations-title {
            font-size: 1.3rem;
            color: #e65100;
            margin-bottom: 15px;
        }
        
        .manifestation-item {
            margin-bottom: 8px;
            padding-left: 15px;
            position: relative;
        }
        
        .manifestation-item:before {
            content: "→";
            color: #f57c00;
            position: absolute;
            left: 0;
        }
        
        .reference {
            font-size: 0.9rem;
            color: #666;
            margin-top: 30px;
            padding-top: 15px;
            border-top: 1px dashed #ccc;
        }
        
        @media (max-width: 900px) {
            .input-section, .output-section {
                flex: 100%;
                border-right: none;
                border-bottom: 1px solid #eee;
            }
        }
        
        .info-note {
            background-color: #e3f2fd;
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 20px;
            font-size: 0.9rem;
            color: #1565c0;
        }
        
        .diagnostic-process {
            background-color: #f3e5f5;
            border-radius: 10px;
            padding: 20px;
            margin-top: 20px;
            border-left: 5px solid #9c27b0;
        }
        
        .diagnostic-title {
            font-size: 1.3rem;
            color: #7b1fa2;
            margin-bottom: 15px;
        }
        
        .diagnostic-step {
            margin-bottom: 12px;
            padding-left: 20px;
            position: relative;
        }
        
        .diagnostic-step:before {
            content: "✓";
            color: #7b1fa2;
            font-weight: bold;
            position: absolute;
            left: 0;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>外科补液导师</h1>
            <p class="subtitle">水、电解质代谢紊乱和酸碱平衡失调临床模拟教学系统</p>
            <p class="subtitle">适用对象：临床医学三年级本科生 | 内容类别：临床技能训练</p>
        </header>
        
        <div class="main-content">
            <div class="input-section">
                <h2 class="section-title">患者信息输入</h2>
                
                <div class="info-note">
                    <strong>教学提示：</strong> 请根据模拟病例输入患者信息。系统将根据临床诊断思维方法[1]和酸碱平衡诊断原理[6]进行分析。
                </div>
                
                <div class="form-group">
                    <label for="age">患者年龄（岁）</label>
                    <input type="number" id="age" min="1" max="120" value="45">
                </div>
                
                <div class="form-group">
                    <label for="gender">患者性别</label>
                    <select id="gender">
                        <option value="male">男性</option>
                        <option value="female">女性</option>
                    </select>
                </div>
                
                <div class="form-group">
                    <label for="weight">体重（kg）</label>
                    <input type="number" id="weight" min="20" max="200" value="70">
                </div>
                
                <div class="form-group">
                    <label for="fluid-loss">体液丢失量（mL）</label>
                    <input type="number" id="fluid-loss" min="0" max="5000" value="1500">
                    <small>常见范围：轻度脱水500-1000mL，中度1000-2000mL，重度>2000mL</small>
                </div>
                
                <div class="form-group">
                    <label for="loss-type">丢失液类型</label>
                    <select id="loss-type">
                        <option value="isotonic">等渗性液体（如肠液）</option>
                        <option value="hypotonic">低渗性液体（如汗液）</option>
                        <option value="hypertonic">高渗性液体（如尿液）</option>
                        <option value="mixed">混合性丢失</option>
                    </select>
                </div>
                
                <h3 class="section-title">实验室检查结果</h3>
                
                <div class="form-group">
                    <label for="na">血清钠（Na⁺, mmol/L）</label>
                    <input type="number" id="na" min="100" max="180" value="142">
                    <small>正常范围：135-145 mmol/L</small>
                </div>
                
                <div class="form-group">
                    <label for="k">血清钾（K⁺, mmol/L）</label>
                    <input type="number" id="k" min="2.0" max="8.0" value="4.0">
                    <small>正常范围：3.5-5.5 mmol/L</small>
                </div>
                
                <div class="form-group">
                    <label for="cl">血清氯（Cl⁻, mmol/L）</label>
                    <input type="number" id="cl" min="80" max="120" value="102">
                    <small>正常范围：96-106 mmol/L</small>
                </div>
                
                <div class="form-group">
                    <label for="ph">动脉血pH</label>
                    <input type="number" id="ph" min="6.8" max="7.8" step="0.01" value="7.40">
                    <small>正常范围：7.35-7.45[6]</small>
                </div>
                
                <div class="form-group">
                    <label for="hco3">HCO₃⁻（mmol/L）</label>
                    <input type="number" id="hco3" min="10" max="40" value="24">
                    <small>正常范围：22-27 mmol/L[6]</small>
                </div>
                
                <div class="form-group">
                    <label for="pco2">PaCO₂（mmHg）</label>
                    <input type="number" id="pco2" min="10" max="80" value="40">
                    <small>正常范围：35-45 mmHg[6]</small>
                </div>
                
                <div class="button-group">
                    <button id="calculate-btn">分析内环境变化</button>
                    <button id="reset-btn">重置为默认值</button>
                </div>
                
                <div class="reference">
                    <p><strong>参考教材：</strong></p>
                    <p>1. 《诊断学》（第10版）第七篇：诊断疾病的步骤和临床思维方法[1]</p>
                    <p>2. 《内科学》（第10版）相关章节</p>
                    <p>3. 《诊断学》（第10版）酸碱平衡诊断卡[6]</p>
                </div>
            </div>
            
            <div class="output-section">
                <h2 class="section-title">内环境变化分析与补液方案</h2>
                
                <div class="result-box">
                    <div class="result-title">诊断分析</div>
                    <div id="diagnosis-result" class="diagnosis">等待分析...</div>
                    <div id="pathophysiology">病理生理机制将在此显示...</div>
                </div>
                
                <div class="diagnostic-process">
                    <div class="diagnostic-title">临床诊断思维过程[1]</div>
                    <div id="diagnostic-steps">
                        <div class="diagnostic-step">1. 收集临床资料：病史、体格检查、实验室检查</div>
                        <div class="diagnostic-step">2. 分析、综合和评价临床资料</div>
                        <div class="diagnostic-step">3. 提出初步诊断（诊断假设）</div>
                        <div class="diagnostic-step">4. 验证和修正诊断</div>
                    </div>
                </div>
                
                <div class="result-box">
                    <div class="result-title">关键参数状态</div>
                    <div class="parameter-display" id="parameter-display">
                        <!-- 参数将通过JavaScript动态生成 -->
                    </div>
                </div>
                
                <!-- 改进的体液平衡可视化对比图 -->
                <div class="result-box">
                    <div class="result-title">体液平衡状态对比图</div>
                    <div class="comparison-container">
                        <div class="comparison-row">
                            <div class="comparison-label">正常状态</div>
                            <div class="comparison-bar-container">
                                <div class="comparison-bar normal-bar" style="width: 100%">
                                    <div class="bar-label">细胞外液 (20%)</div>
                                </div>
                                <div class="comparison-bar normal-bar" style="width: 100%">
                                    <div class="bar-label">细胞内液 (40%)</div>
                                </div>
                            </div>
                        </div>
                        
                        <div class="comparison-row">
                            <div class="comparison-label">当前状态</div>
                            <div class="comparison-bar-container">
                                <div class="comparison-bar current-bar" id="ecf-bar" style="width: 80%">
                                    <div class="bar-label" id="ecf-label">细胞外液: 减少20%</div>
                                </div>
                                <div class="comparison-bar current-bar" id="icf-bar" style="width: 90%">
                                    <div class="bar-label" id="icf-label">细胞内液: 减少10%</div>
                                </div>
                            </div>
                        </div>
                        
                        <div class="comparison-legend">
                            <div class="legend-item">
                                <div class="legend-color normal-legend"></div>
                                <span>正常范围</span>
                            </div>
                            <div class="legend-item">
                                <div class="legend-color deficit-legend"></div>
                                <span>体液缺失</span>
                            </div>
                            <div class="legend-item">
                                <div class="legend-color shift-legend"></div>
                                <span>体液转移</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="fluid-shift-explanation">
                        <h4>体液分布变化说明：</h4>
                        <div id="shift-explanation">
                            <p>根据临床诊断思维方法[1]，体液分布变化需要结合病史资料和体格检查结果综合考虑：</p>
                            <ul>
                                <li>等渗性脱水：细胞内外液等比例减少</li>
                                <li>低渗性脱水：水分向细胞内转移，细胞外液减少更明显</li>
                                <li>高渗性脱水：水分从细胞内移出，细胞内脱水更严重</li>
                            </ul>
                        </div>
                    </div>
                </div>
                
                <div class="clinical-manifestations">
                    <div class="manifestations-title">预计临床表现</div>
                    <div id="manifestations-list">
                        <div class="manifestation-item">根据输入参数计算中...</div>
                    </div>
                </div>
                
                <div class="treatment-plan">
                    <div class="treatment-title">补液治疗建议</div>
                    <div id="treatment-steps">
                        <div class="treatment-step">1. 等待分析完成后生成个性化补液方案</div>
                        <div class="treatment-step">2. 补液方案将基于患者体重、丢失量和电解质水平计算</div>
                        <div class="treatment-step">3. 包括液体类型、速度、电解质补充和监测要点</div>
                    </div>
                </div>
                
                <div class="result-box">
                    <div class="result-title">教学要点提示</div>
                    <div id="teaching-points">
                        <p><strong>学习目标：</strong>通过本模拟器，理解临床诊断思维方法[1]和酸碱平衡诊断原理[6]。</p>
                        <p><strong>核心概念：</strong></p>
                        <ul>
                            <li>临床诊断步骤：收集资料→分析评价→提出初步诊断→验证修正[1]</li>
                            <li>酸碱平衡诊断：根据pH、PaCO₂、HCO₃⁻判断酸碱失调类型[6]</li>
                            <li>脱水分类：根据血清钠浓度分为等渗、低渗和高渗性脱水</li>
                            <li>补液原则：先快后慢、先盐后糖、见尿补钾、适时补碱</li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // 获取DOM元素
        const calculateBtn = document.getElementById('calculate-btn');
        const resetBtn = document.getElementById('reset-btn');
        const diagnosisResult = document.getElementById('diagnosis-result');
        const pathophysiology = document.getElementById('pathophysiology');
        const parameterDisplay = document.getElementById('parameter-display');
        const manifestationsList = document.getElementById('manifestations-list');
        const treatmentSteps = document.getElementById('treatment-steps');
        const diagnosticSteps = document.getElementById('diagnostic-steps');
        
        // 默认参数值
        const defaultValues = {
            age: 45,
            gender: 'male',
            weight: 70,
            fluidLoss: 1500,
            lossType: 'isotonic',
            na: 142,
            k: 4.0,
            cl: 102,
            ph: 7.40,
            hco3: 24,
            pco2: 40
        };
        
        // 重置按钮功能
        resetBtn.addEventListener('click', function() {
            document.getElementById('age').value = defaultValues.age;
            document.getElementById('gender').value = defaultValues.gender;
            document.getElementById('weight').value = defaultValues.weight;
            document.getElementById('fluid-loss').value = defaultValues.fluidLoss;
            document.getElementById('loss-type').value = defaultValues.lossType;
            document.getElementById('na').value = defaultValues.na;
            document.getElementById('k').value = defaultValues.k;
            document.getElementById('cl').value = defaultValues.cl;
            document.getElementById('ph').value = defaultValues.ph;
            document.getElementById('hco3').value = defaultValues.hco3;
            document.getElementById('pco2').value = defaultValues.pco2;
            
            // 重置结果显示
            diagnosisResult.textContent = "等待分析...";
            diagnosisResult.style.color = "#d35400";
            pathophysiology.textContent = "病理生理机制将在此显示...";
            parameterDisplay.innerHTML = "";
            
            // 重置体液平衡对比图
            const ecfBar = document.getElementById('ecf-bar');
            const icfBar = document.getElementById('icf-bar');
            const ecfLabel = document.getElementById('ecf-label');
            const icfLabel = document.getElementById('icf-label');
            
            ecfBar.style.width = "80%";
            icfBar.style.width = "90%";
            ecfLabel.textContent = "细胞外液: 减少20%";
            icfLabel.textContent = "细胞内液: 减少10%";
            
            // 重置说明
            document.getElementById('shift-explanation').innerHTML = `
                <p>根据临床诊断思维方法[1]，体液分布变化需要结合病史资料和体格检查结果综合考虑：</p>
                <ul>
                    <li>等渗性脱水：细胞内外液等比例减少</li>
                    <li>低渗性脱水：水分向细胞内转移，细胞外液减少更明显</li>
                    <li>高渗性脱水：水分从细胞内移出，细胞内脱水更严重</li>
                </ul>
            `;
            
            manifestationsList.innerHTML = '<div class="manifestation-item">根据输入参数计算中...</div>';
            treatmentSteps.innerHTML = `
                <div class="treatment-step">1. 等待分析完成后生成个性化补液方案</div>
                <div class="treatment-step">2. 补液方案将基于患者体重、丢失量和电解质水平计算</div>
                <div class="treatment-step">3. 包括液体类型、速度、电解质补充和监测要点</div>
            `;
            
            diagnosticSteps.innerHTML = `
                <div class="diagnostic-step">1. 收集临床资料：病史、体格检查、实验室检查</div>
                <div class="diagnostic-step">2. 分析、综合和评价临床资料</div>
                <div class="diagnostic-step">3. 提出初步诊断（诊断假设）</div>
                <div class="diagnostic-step">4. 验证和修正诊断</div>
            `;
        });
        
        // 体液平衡可视化更新函数
        function updateFluidComparison(na, fluidLoss, weight) {
            // 计算细胞外液减少百分比（基于脱水类型）
            let ecfLossPercentage = 0;
            let icfChangePercentage = 0;
            
            // 根据血钠水平判断脱水类型
            if (na < 135) { // 低渗性脱水
                ecfLossPercentage = Math.min(30, (fluidLoss / (weight * 15)) * 100); // 细胞外液占体重15%
                icfChangePercentage = Math.min(10, ecfLossPercentage * 0.3); // 水分向细胞内转移
            } else if (na > 145) { // 高渗性脱水
                ecfLossPercentage = Math.min(20, (fluidLoss / (weight * 15)) * 100);
                icfChangePercentage = -Math.min(15, ecfLossPercentage * 0.5); // 细胞内液减少
            } else { // 等渗性脱水
                ecfLossPercentage = Math.min(25, (fluidLoss / (weight * 15)) * 100);
                icfChangePercentage = 0; // 细胞内液基本不变
            }
            
            // 更新条形图
            const ecfBar = document.getElementById('ecf-bar');
            const icfBar = document.getElementById('icf-bar');
            const ecfLabel = document.getElementById('ecf-label');
            const icfLabel = document.getElementById('icf-label');
            
            const ecfWidth = Math.max(20, 100 - ecfLossPercentage);
            const icfWidth = Math.max(30, 100 + icfChangePercentage);
            
            ecfBar.style.width = `${ecfWidth}%`;
            icfBar.style.width = `${icfWidth}%`;
            
            // 更新标签
            ecfLabel.textContent = `细胞外液: ${ecfLossPercentage > 0 ? '减少' + ecfLossPercentage.toFixed(1) + '%' : '正常'}`;
            icfLabel.textContent = `细胞内液: ${icfChangePercentage > 0 ? '增加' + icfChangePercentage.toFixed(1) + '%' : 
                           icfChangePercentage < 0 ? '减少' + (-icfChangePercentage).toFixed(1) + '%' : '正常'}`;
            
            // 更新颜色指示
            if (ecfLossPercentage > 20) {
                ecfBar.style.background = 'linear-gradient(to right, #F44336, #EF5350)';
            } else if (ecfLossPercentage > 10) {
                ecfBar.style.background = 'linear-gradient(to right, #FF9800, #FFB74D)';
            } else {
                ecfBar.style.background = 'linear-gradient(to right, #2196F3, #03A9F4)';
            }
            
            // 更新病理生理说明
            const shiftExplanation = document.getElementById('shift-explanation');
            if (na < 135) {
                shiftExplanation.innerHTML = `
                    <p>根据<strong>低渗性脱水</strong>的病理生理机制：</p>
                    <ul>
                        <li>细胞外液减少（Na⁺丢失＞失水，血Na⁺＜135mmol/L）</li>
                        <li>血浆渗透压降低，水分向细胞内转移</li>
                        <li>血浆容量减少，血液浓缩，组织间液进入血管补偿</li>
                        <li>组织间液减少更明显，易发生循环衰竭</li>
                    </ul>
                `;
            } else if (na > 145) {
                shiftExplanation.innerHTML = `
                    <p>根据<strong>高渗性脱水</strong>的病理生理机制：</p>
                    <ul>
                        <li>失水多于失钠，血清钠浓度＞145mmol/L</li>
                        <li>血浆渗透压升高，细胞外液高渗</li>
                        <li>水从细胞内移出，导致细胞内脱水</li>
                        <li>明显口渴，皮肤黏膜干燥，严重时出现神经精神症状</li>
                    </ul>
                `;
            } else {
                shiftExplanation.innerHTML = `
                    <p>根据<strong>等渗性脱水</strong>的病理生理机制：</p>
                    <ul>
                        <li>细胞外液等渗性减少，血钠浓度正常</li>
                        <li>临床最常见的外科脱水类型</li>
                        <li>口渴、尿少、皮肤弹性降低</li>
                        <li>及时补充等渗溶液可有效纠正</li>
                    </ul>
                `;
            }
        }
        
        // 酸碱平衡诊断函数[6]
        function analyzeAcidBaseBalance(ph, hco3, pco2) {
            let acidBaseDiagnosis = "";
            let acidBaseExplanation = "";
            
            // 根据pH判断酸中毒或碱中毒[6]
            if (ph < 7.35) {
                acidBaseDiagnosis = "酸中毒";
                acidBaseExplanation = "pH＜7.35说明存在酸中毒[6]。";
            } else if (ph > 7.45) {
                acidBaseDiagnosis = "碱中毒";
                acidBaseExplanation = "pH＞7.45说明存在碱中毒[6]。";
            } else {
                acidBaseDiagnosis = "酸碱平衡正常";
                acidBaseExplanation = "pH在正常范围内通常表示不存在酸碱平衡失调或存在代偿性的酸碱平衡失调[6]。";
            }
            
            // 判断原发因素[6]
            if (ph < 7.35 && hco3 < 22) {
                acidBaseDiagnosis = "代谢性酸中毒";
                acidBaseExplanation = "pH降低伴HCO₃⁻降低，提示原发性HCO₃⁻减少[6]。";
            } else if (ph > 7.45 && hco3 > 27) {
                acidBaseDiagnosis = "代谢性碱中毒";
                acidBaseExplanation = "pH升高伴HCO₃⁻升高，提示原发性HCO₃⁻增多[6]。";
            } else if (ph < 7.35 && pco2 > 45) {
                acidBaseDiagnosis = "呼吸性酸中毒";
                acidBaseExplanation = "pH降低伴PaCO₂升高，提示原发性H₂CO₃增多[6]。";
            } else if (ph > 7.45 && pco2 < 35) {
                acidBaseDiagnosis = "呼吸性碱中毒";
                acidBaseExplanation = "pH升高伴PaCO₂降低，提示原发性H₂CO₃减少[6]。";
            }
            
            return { diagnosis: acidBaseDiagnosis, explanation: acidBaseExplanation };
        }
        
        // 计算按钮功能
        calculateBtn.addEventListener('click', function() {
            console.log("按钮点击事件触发");
            
            // 获取输入值
            const age = parseInt(document.getElementById('age').value);
            const gender = document.getElementById('gender').value;
            const weight = parseFloat(document.getElementById('weight').value);
            const fluidLoss = parseInt(document.getElementById('fluid-loss').value);
            const lossType = document.getElementById('loss-type').value;
            const na = parseFloat(document.getElementById('na').value);
            const k = parseFloat(document.getElementById('k').value);
            const cl = parseFloat(document.getElementById('cl').value);
            const ph = parseFloat(document.getElementById('ph').value);
            const hco3 = parseFloat(document.getElementById('hco3').value);
            const pco2 = parseFloat(document.getElementById('pco2').value);
            
            // 1. 诊断分析
            let diagnosis = "";
            let pathoText = "";
            
            // 判断脱水类型
            if (na < 135) {
                diagnosis = "低渗性脱水";
                pathoText = "血清钠<135mmol/L，细胞外液低渗，水向细胞内转移。";
            } else if (na > 145) {
                diagnosis = "高渗性脱水";
                pathoText = "血清钠>145mmol/L，细胞外液高渗，水从细胞内移出。";
            } else {
                diagnosis = "等渗性脱水";
                pathoText = "血清钠在正常范围，细胞外液等渗性减少。";
            }
            
            // 酸碱平衡分析[6]
            const acidBaseResult = analyzeAcidBaseBalance(ph, hco3, pco2);
            diagnosis += "伴" + acidBaseResult.diagnosis;
            pathoText += " " + acidBaseResult.explanation;
            
            // 判断钾代谢紊乱
            if (k < 3.5) {
                diagnosis += " + 低钾血症";
                pathoText += " 血清钾<3.5mmol/L，神经肌肉兴奋性降低。";
            } else if (k > 5.5) {
                diagnosis += " + 高钾血症";
                pathoText += " 血清钾>5.5mmol/L，心肌自律性、传导性、兴奋性均降低。";
            }
            
            diagnosisResult.textContent = diagnosis;
            pathophysiology.textContent = pathoText;
            
            // 2. 更新临床诊断思维过程[1]
            diagnosticSteps.innerHTML = `
                <div class="diagnostic-step" style="color: #4CAF50; font-weight: bold;">1. 收集临床资料：病史、体格检查、实验室检查 ✓</div>
                <div class="diagnostic-step" style="color: #4CAF50; font-weight: bold;">2. 分析、综合和评价临床资料 ✓</div>
                <div class="diagnostic-step" style="color: #4CAF50; font-weight: bold;">3. 提出初步诊断：${diagnosis} ✓</div>
                <div class="diagnostic-step">4. 验证和修正诊断（需要临床实践检验）[1]</div>
            `;
            
            // 3. 更新参数显示
            parameterDisplay.innerHTML = `
                <div class="parameter">
                    <div class="param-name">血清钠 (Na⁺)</div>
                    <div class="param-value ${na < 135 || na > 145 ? 'abnormal' : 'normal'}">${na} mmol/L</div>
                    <div class="param-name">${na < 135 ? '偏低' : na > 145 ? '偏高' : '正常'}</div>
                </div>
                <div class="parameter">
                    <div class="param-name">血清钾 (K⁺)</div>
                    <div class="param-value ${k < 3.5 || k > 5.5 ? 'abnormal' : 'normal'}">${k} mmol/L</div>
                    <div class="param-name">${k < 3.5 ? '偏低' : k > 5.5 ? '偏高' : '正常'}</div>
                </div>
                <div class="parameter">
                    <div class="param-name">动脉血pH</div>
                    <div class="param-value ${ph < 7.35 || ph > 7.45 ? 'abnormal' : 'normal'}">${ph.toFixed(2)}</div>
                    <div class="param-name">${ph < 7.35 ? '酸中毒[6]' : ph > 7.45 ? '碱中毒[6]' : '正常'}</div>
                </div>
                <div class="parameter">
                    <div class="param-name">HCO₃⁻</div>
                    <div class="param-value ${hco3 < 22 || hco3 > 27 ? 'abnormal' : 'normal'}">${hco3} mmol/L</div>
                    <div class="param-name">${hco3 < 22 ? '偏低' : hco3 > 27 ? '偏高' : '正常'}</div>
                </div>
                <div class="parameter">
                    <div class="param-name">PaCO₂</div>
                    <div class="param-value ${pco2 < 35 || pco2 > 45 ? 'abnormal' : 'normal'}">${pco2} mmHg</div>
                    <div class="param-name">${pco2 < 35 ? '偏低' : pco2 > 45 ? '偏高' : '正常'}</div>
                </div>
                <div class="parameter">
                    <div class="param-name">体液丢失</div>
                    <div class="param-value ${fluidLoss > 2000 ? 'abnormal' : fluidLoss > 1000 ? 'warning' : 'normal'}">${fluidLoss} mL</div>
                    <div class="param-name">${fluidLoss > 2000 ? '重度' : fluidLoss > 1000 ? '中度' : '轻度'}</div>
                </div>
            `;
            
            // 4. 更新体液平衡可视化对比图
            updateFluidComparison(na, fluidLoss, weight);
            
            // 5. 更新临床表现
            let manifestationsHTML = "";
            
            // 根据脱水类型添加临床表现
            if (na < 135) {
                manifestationsHTML += `
                    <div class="manifestation-item">口渴感不明显（细胞外液低渗）</div>
                    <div class="manifestation-item">早期尿量正常或增多（抗利尿激素分泌减少）</div>
                    <div class="manifestation-item">严重时出现循环衰竭表现（休克体征）</div>
                `;
            } else if (na > 145) {
                manifestationsHTML += `
                    <div class="manifestation-item">明显口渴（细胞外液高渗）</div>
                    <div class="manifestation-item">皮肤黏膜干燥、弹性差</div>
                    <div class="manifestation-item">烦躁不安，严重时出现谵妄、昏迷</div>
                `;
            } else {
                manifestationsHTML += `
                    <div class="manifestation-item">口渴、尿少</div>
                    <div class="manifestation-item">皮肤弹性降低、眼窝凹陷</div>
                    <div class="manifestation-item">乏力、厌食、恶心</div>
                `;
            }
            
            // 根据酸碱平衡紊乱添加临床表现[6]
            const acidBaseType = acidBaseResult.diagnosis;
            if (acidBaseType.includes("代谢性酸中毒")) {
                manifestationsHTML += `
                    <div class="manifestation-item">呼吸深快（Kussmaul呼吸）</div>
                    <div class="manifestation-item">面部潮红、心率加快</div>
                    <div class="manifestation-item">严重时出现嗜睡、昏迷</div>
                `;
            } else if (acidBaseType.includes("代谢性碱中毒")) {
                manifestationsHTML += `
                    <div class="manifestation-item">呼吸浅慢</div>
                    <div class="manifestation-item">神经肌肉兴奋性增高（手足搐搦）</div>
                    <div class="manifestation-item">低钾血症相关表现（如肌无力）</div>
                `;
            } else if (acidBaseType.includes("呼吸性酸中毒")) {
                manifestationsHTML += `
                    <div class="manifestation-item">呼吸困难、气促</div>
                    <div class="manifestation-item">头痛、嗜睡、意识障碍</div>
                    <div class="manifestation-item">皮肤潮红、多汗</div>
                `;
            } else if (acidBaseType.includes("呼吸性碱中毒")) {
                manifestationsHTML += `
                    <div class="manifestation-item">呼吸急促、深大</div>
                    <div class="manifestation-item">手足口周麻木感</div>
                    <div class="manifestation-item">头晕、意识改变</div>
                `;
            }
            
            // 根据钾水平添加临床表现
            if (k < 3.5) {
                manifestationsHTML += `
                    <div class="manifestation-item">肌无力、腱反射减弱</div>
                    <div class="manifestation-item">腹胀、肠麻痹</div>
                    <div class="manifestation-item">心电图：T波低平、出现U波</div>
                `;
            } else if (k > 5.5) {
                manifestationsHTML += `
                    <div class="manifestation-item">肌无力、甚至弛缓性麻痹</div>
                    <div class="manifestation-item">心率减慢、心律失常</div>
                    <div class="manifestation-item">心电图：T波高尖、QRS波增宽</div>
                `;
            }
            
            manifestationsList.innerHTML = manifestationsHTML;
            
            // 6. 生成补液治疗建议
            let treatmentHTML = "";
            
            // 计算补液总量
            const maintenance = weight * 30; // 每日生理需要量
            const deficit = fluidLoss * 1.5; // 已丢失量，乘以1.5以补充第三间隙丢失
            const totalFluid = maintenance + deficit;
            
            treatmentHTML += `
                <div class="treatment-step"><strong>第一步：评估与诊断</strong> - ${diagnosis}</div>
                <div class="treatment-step"><strong>第二步：计算补液总量</strong> - 约${Math.round(totalFluid)}mL/日（生理需要${maintenance}mL + 累计缺失${Math.round(deficit)}mL）</div>
            `;
            
            // 根据脱水类型选择液体
            if (na < 135) {
                treatmentHTML += `
                    <div class="treatment-step"><strong>第三步：选择液体类型</strong> - 首选等渗或高渗盐水，纠正低钠血症</div>
                    <div class="treatment-step"><strong>第四步：补液速度</strong> - 先快后慢，第一个8小时补充总量的1/2，剩余在16-24小时内补充</div>
                `;
            } else if (na > 145) {
                treatmentHTML += `
                    <div class="treatment-step"><strong>第三步：选择液体类型</strong> - 首选5%葡萄糖或0.45%氯化钠溶液，缓慢纠正高钠</div>
                    <div class="treatment-step"><strong>第四步：补液速度</strong> - 不宜过快，24-48小时内缓慢纠正，防止脑水肿</div>
                `;
            } else {
                treatmentHTML += `
                    <div class="treatment-step"><strong>第三步：选择液体类型</strong> - 首选平衡盐溶液或生理盐水</div>
                    <div class="treatment-step"><strong>第四步：补液速度</strong> - 第一个8小时补充总量的1/2，剩余在16小时内补充</div>
                `;
            }
            
            // 钾补充建议
            if (k < 3.5) {
                const kDeficit = (3.5 - k) * weight * 0.3; // 简化计算公式
                treatmentHTML += `
                    <div class="treatment-step"><strong>第五步：补钾方案</strong> - 需补钾约${Math.round(kDeficit)}mmol，见尿补钾，浓度<40mmol/L，速度<20mmol/h</div>
                `;
            } else if (k > 5.5) {
                treatmentHTML += `
                    <div class="treatment-step"><strong>第五步：高钾处理</strong> - 1. 钙剂拮抗心肌毒性 2. 促进钾向细胞内转移 3. 促进钾排出</div>
                `;
            } else {
                treatmentHTML += `
                    <div class="treatment-step"><strong>第五步：钾维持</strong> - 每日生理需要量约40-60mmol，加入常规补液中</div>
                `;
            }
            
            // 酸碱平衡纠正建议[6]
            if (acidBaseType.includes("代谢性酸中毒")) {
                treatmentHTML += `
                    <div class="treatment-step"><strong>第六步：纠正酸中毒</strong> - 5%碳酸氢钠${Math.round((24 - hco3) * weight * 0.3)}mL，先给1/2量，根据血气调整</div>
                `;
            } else if (acidBaseType.includes("代谢性碱中毒")) {
                treatmentHTML += `
                    <div class="treatment-step"><strong>第六步：纠正碱中毒</strong> - 补充氯化钾，严重者可用精氨酸或稀盐酸</div>
                `;
            }
            
            treatmentHTML += `
                <div class="treatment-step"><strong>第七步：监测指标</strong> - 每小时尿量、血压、心率；每4-6小时复查电解质、血气分析[6]</div>
                <div class="treatment-step"><strong>第八步：调整方案</strong> - 根据临床反应和实验室结果动态调整补液方案[1]</div>
            `;
            
            treatmentSteps.innerHTML = treatmentHTML;
        });
        
        // 页面加载时初始化
        window.onload = function() {
            // 初始化体液平衡对比图
            updateFluidComparison(defaultValues.na, defaultValues.fluidLoss, defaultValues.weight);
        };
    </script>
</body>
</html>
