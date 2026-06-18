# Actividad3-PCA-ExpresionGenica-IMC
Análisis bioestadístico de expresión génica y obesidad mediante PCA y regresión logística realizado en R. Proyecto de la asignatura Estadística y R para Ciencias de la Salud.

# Análisis bioestadístico de expresión génica asociado al IMC mediante PCA

# Descripción
Este repositorio contiene el análisis desarrollado para la Actividad 3 de la asignatura Estadística y R para Ciencias de la Salud.

Se estudiaron 37 genes relacionados con el metabolismo y la obesidad en 59 individuos utilizando técnicas de reducción de dimensionalidad mediante Análisis de Componentes Principales (PCA), clustering y modelos de regresión logística.

## Objetivos

- Explorar patrones de expresión génica.
- Reducir la dimensionalidad de los datos mediante PCA.
- Visualizar agrupamientos entre individuos y genes.
- Evaluar la asociación entre componentes principales y categorías de IMC.

## Estructura del repositorio

├── datos/
├── scripts/
├── resultados/
├── figuras/
├── tablas/
├── Actividad3.Rmd
├── Actividad3.html
└── README.md

# Principales resultados


<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>QR Code - Repositorio GitHub</title>
<style>
  body { font-family: Arial, sans-serif; display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 100vh; margin: 0; background: #f5f5f5; }
  .card { background: white; padding: 40px; border-radius: 16px; box-shadow: 0 4px 20px rgba(0,0,0,0.12); text-align: center; max-width: 380px; }
  h2 { font-size: 1.1rem; color: #333; margin: 0 0 8px 0; }
  p { font-size: 0.8rem; color: #666; word-break: break-all; margin: 0 0 20px 0; }
  #qr { margin: 0 auto 20px auto; }
  .label { font-size: 0.75rem; color: #888; }
</style>
</head>
<body>
<div class="card">
  <h2>📁 Repositorio GitHub</h2>
  <p>Actividad3-PCA-ExpresionGenica-IMC</p>
  <div id="qr"></div>
  <p style="font-size:0.7rem;color:#aaa;">https://github.com/CarenMoreno/<br>Actividad3-PCA-ExpresionGenica-IMC</p>
  <div class="label">Escanear para acceder al repositorio</div>
</div>

<script>
// Minimal QR Code generator (Version 2, Error Correction L)
// Based on open-source QR encoding logic

// Simple QR code using canvas via a pure-JS library loaded inline
// We'll use a data URI approach for the QR matrix

// Since we need a real QR, let's use the Google Charts API approach
// But since network might be blocked, let's draw a placeholder with the URL

const url = "https://github.com/CarenMoreno/Actividad3-PCA-ExpresionGenica-IMC";

// Try to load QR from a CDN
const img = document.createElement('img');
img.width = 200;
img.height = 200;
img.alt = url;
img.onerror = function() {
  // Fallback: show text
  const div = document.getElementById('qr');
  div.innerHTML = '<div style="width:200px;height:200px;border:3px solid #333;display:flex;align-items:center;justify-content:center;font-size:0.65rem;color:#555;padding:10px;box-sizing:border-box;word-break:break-all;">QR Code<br><br>' + url + '</div>';
};
img.src = 'https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=' + encodeURIComponent(url);
document.getElementById('qr').appendChild(img);
</script>
</body>
</html>
