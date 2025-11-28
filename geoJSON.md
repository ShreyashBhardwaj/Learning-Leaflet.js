# 📘 **GeoJSON — Complete Guide for Developers**

## **ALWAYS REMEBER FOR GEOJSON it is [longitude,latitude] for LEAFLET is is [latitude, longitude]**

## 📍 Introduction

GeoJSON is a lightweight, JSON-based standard for representing **geographic data**.
It is used by Leaflet, Mapbox, OpenLayers, GIS tools, and backend map APIs.

GeoJSON supports both **geometric shapes** and **attribute data** (metadata).

---

# 🧱 **1. Top-Level GeoJSON Types**

GeoJSON supports **three** top-level structures:

### ### **1. Feature**

Represents **one geographic shape** + optional metadata.

```json
{
  "type": "Feature",
  "geometry": { ... },
  "properties": { ... }
}
```

---

### **2. FeatureCollection**

A collection of multiple `Feature` objects.

```json
{
  "type": "FeatureCollection",
  "features": [
    { "type": "Feature", ... },
    { "type": "Feature", ... }
  ]
}
```

---

### **3. Geometry**

A geometry object describing a shape.
These can also appear inside Features.

---

# 🧱 **2. Geometry Types (The 7 Core Types)**

GeoJSON defines **exactly seven** geometry types.

These are the only shapes you can represent:

1. **Point**
2. **MultiPoint**
3. **LineString**
4. **MultiLineString**
5. **Polygon**
6. **MultiPolygon**
7. **GeometryCollection**

Below is an explanation of each with examples.

---

# 🟦 **1. Point**

Represents a **single location**.

Coordinates:

```
[longitude, latitude]
```

```json
{
  "type": "Point",
  "coordinates": [77.209, 28.6139]
}
```

---

# 🟩 **2. MultiPoint**

Represents **multiple points**.

```json
{
  "type": "MultiPoint",
  "coordinates": [
    [77.21, 28.61],
    [77.25, 28.63]
  ]
}
```

---

# 🟧 **3. LineString**

Represents a **path** (a line made of multiple coordinates).

```json
{
  "type": "LineString",
  "coordinates": [
    [77.21, 28.61],
    [77.22, 28.62],
    [77.23, 28.63]
  ]
}
```

---

# 🟨 **4. MultiLineString**

Multiple lines in one geometry.

```json
{
  "type": "MultiLineString",
  "coordinates": [
    [
      [77.21, 28.61],
      [77.22, 28.62]
    ],
    [
      [77.23, 28.63],
      [77.24, 28.64]
    ]
  ]
}
```

---

# 🟥 **5. Polygon**

Represents a **filled area**.

Coordinates follow this structure:

```
[
  [ outer boundary coordinates ],
  [ optional hole 1 ],
  [ optional hole 2 ]
]
```

⚠ **Important:**

- Coordinates are always **[longitude, latitude]**
- First and last coordinate must be the **same**

```json
{
  "type": "Polygon",
  "coordinates": [
    [
      [77.21, 28.65],
      [77.25, 28.65],
      [77.25, 28.62],
      [77.21, 28.62],
      [77.21, 28.65]
    ]
  ]
}
```

---

# 🟪 **6. MultiPolygon**

Represents multiple polygons.

```json
{
  "type": "MultiPolygon",
  "coordinates": [
    [
      [
        [77.2, 28.6],
        [77.22, 28.6],
        [77.22, 28.58],
        [77.2, 28.58],
        [77.2, 28.6]
      ]
    ],
    [
      [
        [77.25, 28.65],
        [77.28, 28.65],
        [77.28, 28.62],
        [77.25, 28.62],
        [77.25, 28.65]
      ]
    ]
  ]
}
```

---

# 🟫 **7. GeometryCollection**

Contains **mixed geometry types**.

```json
{
  "type": "GeometryCollection",
  "geometries": [
    {
      "type": "Point",
      "coordinates": [77.21, 28.61]
    },
    {
      "type": "LineString",
      "coordinates": [
        [77.2, 28.6],
        [77.22, 28.62]
      ]
    }
  ]
}
```

    Correct — and let’s get this nailed down with absolute clarity so you can architect your GeoJSON with confidence.

---

# ✅ **Yes. `GeometryCollection` can appear inside BOTH:**

### ✔ A **Feature**

### ✔ A **FeatureCollection**

But with **different purposes**.

Let’s break it down clearly and professionally.

---

# 🧱 **1. GeometryCollection inside a Feature**

This is **fully valid**.

```json
{
  "type": "Feature",
  "geometry": {
    "type": "GeometryCollection",
    "geometries": [
      { "type": "Point", "coordinates": [77.21, 28.61] },
      {
        "type": "Polygon",
        "coordinates": [
          [
            [77.2, 28.6],
            [77.25, 28.6],
            [77.25, 28.58],
            [77.2, 28.58],
            [77.2, 28.6]
          ]
        ]
      }
    ]
  },
  "properties": {
    "name": "Mixed region"
  }
}
```

### ✔ Why use this?

When you want **one logical object** (one Feature) containing multiple shapes:

- A building footprint + its entrance point
- A region + its label location
- A school boundary + bus stop point

This keeps your data **grouped** instead of split into multiple Features.

---

# 🧱 **2. GeometryCollection inside a FeatureCollection**

Also **completely valid**.

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "GeometryCollection",
        "geometries": [
          { "type": "Point", "coordinates": [77.21, 28.61] },
          { "type": "LineString", "coordinates": [[77.20,28.60],[77.22,28.62]] }
        ]
      },
      "properties": { "id": 1 }
    },
    {
      "type": "Feature",
      "geometry": { "type": "Polygon", "coordinates": [ ... ] },
      "properties": { "id": 2 }
    }
  ]
}
```

### ✔ Why use this?

A FeatureCollection is just a list of Features.
Each Feature can use _any_ geometry — including a GeometryCollection.

This is common in real GIS datasets.

---

# 🧭 **Corporate-Grade Summary**

### ✔ Geometry types (Point, Polygon, MultiPolygon, GeometryCollection, etc.)

can appear **only in `geometry.type`**.

### ✔ A `Feature` always contains exactly **one geometry**

—but that geometry **can be a GeometryCollection**, allowing multiple shapes inside one Feature.

### ✔ A `FeatureCollection` contains many Features

—and each Feature’s geometry can be anything:

- Point
- Polygon
- MultiPolygon
- GeometryCollection
- …etc.

---

# 🎯 **Final Answer**

**Yes — `GeometryCollection` is a valid geometry type and can be used inside:**

- **Feature** (one Feature with multiple geometries)
- **FeatureCollection** (multiple Features, each allowed to use GeometryCollection)

Both scenarios are valid GeoJSON.

---

If you want, I can create a **visual diagram** showing how Features, FeatureCollections, and GeometryCollections relate architecturally — that usually makes everything click instantly.

---

# 🧱 **3. Properties (Optional Metadata)**

A Feature can store attributes in `properties`.

```json
{
  "type": "Feature",
  "geometry": { "type": "Point", "coordinates": [77.21, 28.61] },
  "properties": {
    "name": "New Delhi",
    "population": 21800000,
    "zone": 5
  }
}
```

Leaflet will render geometry, and `properties` remain accessible for popups, labels, etc.

---

# 🌍 **4. Coordinate Order**

GeoJSON strictly requires:

```
[longitude, latitude]
```

Leaflet markers use the opposite order:

```
[latitude, longitude]
```

Remember this distinction — mixing these up is a common source of bugs.

---

# 📦 **5. Complete Example: FeatureCollection**

Good for grouping multiple shapes.

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": { "type": "Point", "coordinates": [77.21, 28.61] },
      "properties": { "name": "Marker A" }
    },
    {
      "type": "Feature",
      "geometry": {
        "type": "Polygon",
        "coordinates": [
          [
            [77.21, 28.65],
            [77.25, 28.65],
            [77.25, 28.62],
            [77.21, 28.62],
            [77.21, 28.65]
          ]
        ]
      },
      "properties": { "zone": "North Delhi" }
    }
  ]
}
```

---

# 📍 **6. How Leaflet Loads GeoJSON**

In Leaflet:

```js
L.geoJSON(geoData).addTo(map);
```

Leaflet will:

- Render polygons as shapes
- Render points as markers
- Render lines as polylines
- Use properties in popups (if you add them)

---

# 🧩 **7. Why GeoJSON Is Important**

- Industry standard for mapping
- Works with Leaflet, Mapbox, QGIS, ArcGIS
- Easy to store, send, or modify
- Perfect for full-stack mapping systems

---

# 🎯 **Final Summary**

GeoJSON gives you:

- **7 geometry types**
- **Feature** (1 geometry)
- **FeatureCollection** (many features)
- Strict coordinate rules
- Powerful metadata via `properties`
- Easy integration with Leaflet

It is the backbone of professional map development.

---

If you want, I can also generate a **cheat-sheet version**, or a **diagram-heavy version**, or even a **PDF export**.
