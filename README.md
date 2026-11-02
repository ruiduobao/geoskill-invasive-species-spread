# Invasive Species Spread Monitoring (geoskill-invasive-species-spread)

> Detects newly invaded pixels via multi-temporal index threshold classification, computes the relative rate of areal spread, and predicts invasion risk from environmental suitability × distance decay. Monitors invasive species spread from multi-temporal classification and risk prediction. Outputs GeoTIFFs of new invasion and risk.

---

## 1. Overview

Two epochs of remote sensing indices are classified by thresholds to derive the t0 presence area and the t1 newly invaded area; the spread rate is r = (A1−A0)/(A0×Δt); risk prediction = environmental suitability × spread accessibility exp(−d/λ), where d is the Euclidean distance to the nearest known invaded pixel (scipy distance transform) and λ is the spread scale (default 5 km). Risk ∈ [0,1]. Use cases: dynamic monitoring of invasive alien species (e.g., smooth cordgrass, Canada goldenrod) and control-priority ranking.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-invasive-species-spread.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `new_invasion.tif` | GeoTIFF (float32) | Newly invaded pixels (0/1), EPSG:4326 |
| `invasion_risk.tif` | GeoTIFF (float32) | Invasion risk ∈ [0,1] |
| `invasive_params.json` | JSON | Area change, spread rate, and risk statistics |
| `output-manifest.json` | JSON | Run manifest (input/output/QA/software versions) |

## 6. Technical Principle

(See SKILL.md for details)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
