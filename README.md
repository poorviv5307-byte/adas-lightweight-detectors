# Comparative Benchmarking of Lightweight Object Detectors for Real-Time Vehicle Detection on Edge Devices

ADAS project by Poorvika V,T. Vishal, Yoogi Suchiith K,
Department of Robotics and AI,
Dayananda Sagar University.

## What this project does
Advanced driver assistance systems need fast, small and accurate vehicle detectors that run on low-power hardware. This project fine-tunes pretrained lightweight detectors on the **KITTI** dataset and compares them on **accuracy vs FPS vs model size**, then screens their feasibility for **Jetson Nano / Raspberry Pi** class devices.

**Models:** YOLOv8n, YOLOv9-tiny, SSDLite-MobileNetV3 (MobileNet-SSD), NanoDet-Plus-m
**Dataset:** KITTI (Ultralytics YOLO-format copy). BDD100K is planned as future work.

## Repository contents
| Path | Purpose |
|---|---|
| `notebooks/adas_main_colab.ipynb` | Fine-tunes and evaluates YOLOv8n, YOLOv9-tiny and SSDLite on KITTI; builds the comparison table, plots and feasibility screening |
| `notebooks/adas_nanodet_colab.ipynb` | Same pipeline for NanoDet-Plus-m (separate notebook because its code needs an older software stack) |
| `notebooks/adas_simulation_colab.ipynb` | Dashcam-style ADAS demo: detection, FPS counter, ego-lane forward-collision warning, GPU vs limited-CPU run |
| `results/` | Result tables |
| `figures/` | Figures |

## How to run
1. Open a notebook in Google Colab (File > Upload notebook, or open it from this repo).
2. Set `Runtime > Change runtime type > T4 GPU`.
3. Run the cells top to bottom. In the main notebook, set `QUICK = True` first for a short test, then `QUICK = False`.
4. Results are saved to `adas_benchmark/` in your Google Drive.

All models share the same KITTI split and are scored by one common mAP evaluator (TorchMetrics). Latency is measured at batch size 1, with warm-up excluded and pre- and post-processing included.

## Results so far
The full 4-model accuracy benchmark is **in progress**. The only numbers available so far come from the ADAS simulation (120 KITTI validation images, COCO-pretrained weights, batch size 1, Tesla T4 GPU and a 2-thread CPU as an edge stand-in):

| Model | Device | Latency (ms) | FPS |
|---|---|---|---|
| YOLOv8n | GPU (T4) | 10.6 | 94.3 |
| YOLOv8n | CPU, 2 threads | 62.8 | 15.9 |
| YOLOv9-tiny | GPU (T4) | 21.1 | 47.3 |
| YOLOv9-tiny | CPU, 2 threads | 94.7 | 10.6 |

![FPS comparison](figures/fig_fps_gpu_vs_edge_cpu.png)

These are single-run descriptive figures. They are not accuracy results, and they were not measured on a physical Jetson Nano or Raspberry Pi.

## Limitations
- Speeds come from Colab hardware, which is much faster than a real Jetson Nano or Raspberry Pi.
- The collision warning is a visual heuristic (box height inside an ego-lane zone), not a calibrated distance estimate.
- Models use different input sizes; see the notebook configuration.
- Single training run per model, so no confidence intervals yet.

## Acknowledgment
We are deeply grateful to our guide, Prof Nikhil Jindal, for his guidance, encouragement and support throughout this project.

## References
- A. Geiger, P. Lenz, R. Urtasun, "Are we ready for autonomous driving? The KITTI vision benchmark suite," CVPR 2012.
- Ultralytics YOLO: https://github.com/ultralytics/ultralytics
- NanoDet: https://github.com/RangiLyu/nanodet
