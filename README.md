# Goruntu_Duzenleyici
import json

html_content = """%%html
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>JS Görüntü Düzenleyici</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #1e1e2f;
            color: #ffffff;
            display: flex;
            justify-content: center;
            padding: 20px;
            margin: 0;
        }
        .container {
            background-color: #2a2a40;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 8px 16px rgba(0,0,0,0.5);
            width: 100%;
            max-width: 800px;
            text-align: center;
        }
        .canvas-wrapper {
            margin: 20px 0;
            background-color: #1a1a24;
            border: 2px dashed #444;
            border-radius: 8px;
            min-height: 300px;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
        }
        canvas {
            max-width: 100%;
            max-height: 500px;
        }
        .tools {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        .slider-group, .button-group {
            display: flex;
            justify-content: center;
            gap: 15px;
            align-items: center;
            flex-wrap: wrap; 
        }
        button, input[type="file"] {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.2s;
        }
        button:hover:not(:disabled) {
            background-color: #45a049;
        }
        button:disabled, input[type="range"]:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        label {
            min-width: 120px; 
            text-align: right;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Görüntü Düzenleyici</h1>
        
        <div class="top-controls">
            <input type="file" id="uploadInput" accept="image/*">
            <button id="downloadBtn" disabled>Resmi İndir</button>
        </div>

        <div class="canvas-wrapper">
            <canvas id="imageCanvas"></canvas>
        </div>

        <div class="tools">
            <div class="slider-group">
                <label for="brightness">Parlaklık:</label>
                <input type="range" id="brightness" min="-100" max="100" value="0" disabled>
            </div>
            
            <div class="slider-group">
                <label for="contrast">Kontrast:</label>
                <input type="range" id="contrast" min="-100" max="100" value="0" disabled>
            </div>
            
            <div class="slider-group">
                <label for="shadows">Gölgeleri Aç (HDR):</label>
                <input type="range" id="shadows" min="0" max="100" value="0" disabled>
            </div>

            <div class="button-group">
                <button id="btn-bw" disabled>Siyah & Beyaz</button>
                <button id="btn-sepia" disabled>Sepya</button>
                <button id="btn-reset" disabled>Orijinale Dön</button>
            </div>
        </div>
    </div>

    <script>
        const uploadInput = document.getElementById('uploadInput');
        const canvas = document.getElementById('imageCanvas');
        const ctx = canvas.getContext('2d');
        
        const brightnessSlider = document.getElementById('brightness');
        const contrastSlider = document.getElementById('contrast');
        const shadowsSlider = document.getElementById('shadows'); 
        const btnBw = document.getElementById('btn-bw');
        const btnSepia = document.getElementById('btn-sepia');
        const btnReset = document.getElementById('btn-reset');
        const downloadBtn = document.getElementById('downloadBtn');

        let originalImage = null;

        let currentFilters = {
            brightness: 0,
            contrast: 0,
            shadows: 0, 
            isBw: false,
            isSepia: false
        };

        uploadInput.addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (event) => {
                    const img = new Image();
                    img.onload = () => {
                        originalImage = img;
                        canvas.width = img.width;
                        canvas.height = img.height;
                        ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                        enableControls();
                    };
                    img.src = event.target.result;
                };
                reader.readAsDataURL(file);
            }
        });

        function enableControls() {
            downloadBtn.disabled = false;
            brightnessSlider.disabled = false;
            contrastSlider.disabled = false;
            shadowsSlider.disabled = false;
            btnBw.disabled = false;
            btnSepia.disabled = false;
            btnReset.disabled = false;
        }

        brightnessSlider.addEventListener('input', (e) => { currentFilters.brightness = parseInt(e.target.value); applyFilters(); });
        contrastSlider.addEventListener('input', (e) => { currentFilters.contrast = parseInt(e.target.value); applyFilters(); });
        shadowsSlider.addEventListener('input', (e) => { currentFilters.shadows = parseInt(e.target.value); applyFilters(); }); 
        btnBw.addEventListener('click', () => { currentFilters.isBw = true; currentFilters.isSepia = false; applyFilters(); });
        btnSepia.addEventListener('click', () => { currentFilters.isSepia = true; currentFilters.isBw = false; applyFilters(); });
        
        btnReset.addEventListener('click', () => {
            currentFilters = { brightness: 0, contrast: 0, shadows: 0, isBw: false, isSepia: false };
            brightnessSlider.value = 0;
            contrastSlider.value = 0;
            shadowsSlider.value = 0;
            applyFilters();
        });

        downloadBtn.addEventListener('click', () => {
            const link = document.createElement('a');
            link.download = 'duzenlenmis_resim.png';
            link.href = canvas.toDataURL('image/png'); 
            link.click();
        });

        function applyFilters() {
            if (!originalImage) return;

            ctx.drawImage(originalImage, 0, 0, canvas.width, canvas.height);
            
            const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
            const data = imageData.data; 

            const factor = (259 * (currentFilters.contrast + 255)) / (255 * (259 - currentFilters.contrast));
            const shadowStrength = currentFilters.shadows / 100; 

            for (let i = 0; i < data.length; i += 4) {
                let r = data[i];
                let g = data[i + 1];
                let b = data[i + 2];

                if (currentFilters.shadows > 0) {
                    const luma = 0.299 * r + 0.587 * g + 0.114 * b; 
                    const shadowMask = 1 - (luma / 255); 
                    
                    r += (255 - r) * shadowMask * shadowStrength;
                    g += (255 - g) * shadowMask * shadowStrength;
                    b += (255 - b) * shadowMask * shadowStrength;
                }

                r += currentFilters.brightness;
                g += currentFilters.brightness;
                b += currentFilters.brightness;

                r = factor * (r - 128) + 128;
                g = factor * (g - 128) + 128;
                b = factor * (b - 128) + 128;

                if (currentFilters.isBw) {
                    const gray = 0.3 * r + 0.59 * g + 0.11 * b;
                    r = gray; g = gray; b = gray;
                }

                if (currentFilters.isSepia) {
                    const tr = (r * 0.393) + (g * 0.769) + (b * 0.189);
                    const tg = (r * 0.349) + (g * 0.686) + (b * 0.168);
                    const tb = (r * 0.272) + (g * 0.534) + (b * 0.131);
                    r = tr; g = tg; b = tb;
                }

                data[i] = Math.min(255, Math.max(0, r));
                data[i + 1] = Math.min(255, Math.max(0, g));
                data[i + 2] = Math.min(255, Math.max(0, b));
            }

            ctx.putImageData(imageData, 0, 0);
        }
    </script>
</body>
</html>
"""

notebook = {
    "cells": [
        {
            "cell_type": "code",
            "execution_count": None,
            "metadata": {},
            "outputs": [],
            "source": [line + "\n" for line in html_content.split("\n")]
        }
    ],
    "metadata": {
        "colab": {
            "name": "Goruntu_Duzenleyici.ipynb",
            "provenance": []
        },
        "kernelspec": {
            "display_name": "Python 3",
            "name": "python3"
        }
    },
    "nbformat": 4,
    "nbformat_minor": 0
}

with open("Goruntu_Duzenleyici.ipynb", "w", encoding="utf-8") as f:
    json.dump(notebook, f, ensure_ascii=False, indent=4)
    
print("File created.")
