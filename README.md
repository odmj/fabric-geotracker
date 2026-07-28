# fabric-geotracker
# Flujo de datos de ruta GPS de end-to-end y análisis con Fabric, PySpark y Power BI.
![Microsoft Fabric](https://img.shields.io/badge/Microsoft%20Fabric-Data%20Engineering-blue)
![PySpark](https://img.shields.io/badge/PySpark-Delta%20Lake-orange)
![Power BI](https://img.shields.io/badge/Power%20BI-DirectLake-yellow)
![Architecture](https://img.shields.io/badge/Architecture-Medallion%20(Bronze--Silver--Gold)-green)

---

## Visión General del Proyecto
Este proyecto implementa una solución **End-to-End de Ingeniería de Datos** en **Microsoft Fabric** para procesar, limpiar, reconstruir y visualizar datos de geolocalización obtenidos mediante la aplicación de móvil geotracker durante un viaje en coche por Europa.
El objetivo principal fue transformar lecturas de GPS reales (con imprecisiones, descuidos que dan lugar a datos poco relevantes, paradas no deseadas y pérdidas temporales de señal) en un modelo de datos optimizado para análisis analítico y visualización interactiva.

---

## 🏗️ Arquitectura de Datos (Medallion Architecture)

El pipeline sigue la arquitectura Medallón (*Bronze*, *Silver*, *Gold*) utilizando **Delta Lake** y procesado distribuido con **PySpark**:

[ Archivos .GPX ]

       │
       ▼
 🥉 CAPA BRONZE  ──► Ingesta de datos crudos (Latitud, Longitud, Elevación, Timestamp)
     
       │
       ▼
       
 🥈 CAPA SILVER  ──► Limpieza de ruido (filtrado < 5 km/h, eliminación de data recogida en días de descanso), desduplicación
                     e imputación/interpolación matemática de puntos perdidos (puntos no recogidos por pérdida de señal o no activación de la apk). Aplicación de la Fórmula de Haversine para obtener distancias relativas y así determinar                         velocidades medidas.
                     
       │
       ▼
       
 🥇 CAPA GOLD    ──► Agregaciones diarias (Distancia total, Velocidad media real,
                     Tiempo activo en movimiento, Desnivel acumulado)
                     
       │
       ▼
       
 📊 POWER BI     ──► Visualización interactiva mediante conexión DirectLake: mapa, gráfico circular y tabla.

 ## Captura de pantalla de Panel Power BI con DirectLake

 ![Product_Screenshot](powerbi/powerbi.png)
