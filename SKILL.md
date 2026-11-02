---
name: geoskill-invasive-species-spread
description: '多时相指数阈值分类检测入侵新增像元，计算面积相对扩散速率，用环境适宜性×距离衰减预测入侵风险。Monitors invasive species spread from multi-temporal classification and risk prediction. 输出新增入侵与风险 GeoTIFF。'
---

# 入侵物种扩散监测 | Invasive Species Spread Monitoring

Two epochs of remote sensing indices are classified by threshold to obtain the t0 presence zone and the t1 newly invaded zone; spread rate r = (A1−A0)/(A0×Δt); risk prediction = environmental suitability × spread accessibility exp(−d/λ), where d is the Euclidean distance to the nearest known invaded pixel (scipy distance transform) and λ is the dispersal scale (default 5 km). Risk ∈ [0,1].

Use cases: dynamic monitoring of invasive alien species (e.g., smooth cordgrass, Canada goldenrod) and priority ranking for prevention and control.

## Dependencies / 依赖

```bash
pip install numpy rasterio scipy scikit-learn
```

## Usage / 使用方法

### Example 1: synthetic two-epoch scenario (default 5-year interval)

```bash
python geoskill-invasive-species-spread.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./output
```

### Example 2: real 3-band input (index_t0, index_t1, suitability)

```bash
python geoskill-invasive-species-spread.py --input invasive_inputs.tif --output-dir ./real
```

### Example 3: stricter threshold + 3-year interval

```bash
python geoskill-invasive-species-spread.py --bbox 116 39 117 40 --synthetic --threshold 0.2 --dt-years 3 --output-dir ./tuned
```

### Example 4: strongly spreading species (large dispersal scale)

```bash
python geoskill-invasive-species-spread.py --bbox 121 31 122 32 --synthetic --dispersal-scale 10000 --output-dir ./strong
```

### Example 5: quiet batch run

```bash
python geoskill-invasive-species-spread.py --bbox 113 23 114 24 --synthetic --quiet --output-dir ./batch
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `new_invasion.tif` | GeoTIFF (float32) | Newly invaded pixels (0/1), EPSG:4326 |
| `invasion_risk.tif` | GeoTIFF (float32) | Invasion risk ∈ [0,1] |
| `invasive_params.json` | JSON | Area change, spread rate, and risk statistics |
| `output-manifest.json` | JSON | Run manifest (input/output/QA/software versions) |

## Data Source / 数据源 / Source

Local GeoTIFF (3 bands: two-epoch indices + suitability, optional); synthetic mode generates expanding invasion patches and an environmental suitability gradient locally, with no external data source.

## Privacy / 隐私声明 / Privacy

- Runs fully offline by default; makes no network requests
- `--synthetic` mode reads no external data
- All computation is done locally; no user data is uploaded

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
