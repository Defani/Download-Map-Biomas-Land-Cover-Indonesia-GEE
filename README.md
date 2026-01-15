# Download Tutupan Lahan MapBiomas Indonesia di Google Earth Engine (GEE)<img width="1872" height="433" alt="mapbiomas-logo-country-DfzM8WLo" src="https://github.com/user-attachments/assets/6a4c937e-a5c0-494c-b23e-d9b7ec1a1ff5" />


Tutorial singkat untuk **mengunduh peta tutupan lahan MapBiomas Indonesia** menggunakan **Google Earth Engine (GEE)** dan mengekspornya ke **Google Drive (GeoTIFF)**.

 **AOI ditentukan dari geometry yang digambar langsung di GEE**  




https://github.com/user-attachments/assets/082a8e0f-eb97-40ed-8475-838c03e2fa8d


**Tahun data fleksibel (tersedia sejak 1990)**
Ganti kode  classification_2024 menjadi classification_1990, 2000, 2010, dst termasuk nama variabel yang terdapat tahun
var TutupanLahan = PL_MapBiomas.select('classification_2024');

---

## 🔗 Link Google Earth Engine Code
 https://code.earthengine.google.com/5c832c9e8a9b6b6af80ea73184d4a3fd?hl=id

---

## 🌐 Referensi Resmi MapBiomas
- Website: https://landy.mapbiomas.id/en  
- Web Map (GUI): https://platform.indonesia.mapbiomas.org  

---

## 🗺️ Kode Google Earth Engine (1 TAB – Langsung Copy)

```javascript
// Load MapBiomas Indonesia Collection 4
var PL_MapBiomas = ee.Image(
  'projects/mapbiomas-public/assets/indonesia/' +
  'lulc/collection4/' +
  'mapbiomas_indonesia_collection4_coverage_v2'
);

// AOI dari geometry yang DIGAMBAR di GEE
var AOI = geometry;

// Pilih tahun tutupan lahan
// Ganti classification_2024 menjadi classification_1990, 2000, 2010, dst
var TutupanLahan = PL_MapBiomas.select('classification_2024');

// Clip ke AOI
var TutupanLahanClip = TutupanLahan.clip(AOI);

// Kelas & palet warna 
var Kelas = [3,5,76,13,40,35,9,21,30,24,25,31,33,27];

var PaletWarna = [
  '1f8d49','04381d','2f7360','d89f5c','c71585','9065d0',
  '7a5900','ffefc3','9c0027','d4271e','db4d4f','091077',
  '2532e4','ffffff'
];

// Remap untuk visualisasi
var TutupanLahanRemap = TutupanLahanClip.remap(
  Kelas,
  ee.List.sequence(0, Kelas.length - 1)
);

// Tampilkan peta
Map.centerObject(AOI, 5);
Map.addLayer(TutupanLahanRemap, {
  min: 0,
  max: Kelas.length - 1,
  palette: PaletWarna
}, 'Tutupan Lahan MapBiomas');

// Export ke Google Drive
Export.image.toDrive({
  image: TutupanLahanClip,
  description: 'MapBiomas_Indonesia_2024',
  folder: 'MapBiomas',
  region: AOI,
  scale: 30,
  crs: 'EPSG:4326',
  maxPixels: 1e13,
  fileFormat: 'GeoTIFF'
});
