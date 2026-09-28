# masquerade-audit

Code and recorded measurements for:

> **Blaming the Wrong Sensor: Auditing Misalignment Confounds in Multimodal Reliability Estimation**
> Vanlalhmangaihsanga Ralte, Lalengmawia Chhangte
> Department of Computer Science and Engineering, National Institute of Technology Mizoram

The two-knob audit tests whether a multimodal reliability signal attributes failure to the correct cause. Knob 1 genuinely degrades a sensor; Knob 2 breaks only the cross-modal relationship, either semantically (2a, mispaired correspondence) or geometrically (2b, drifted extrinsic calibration). The ratio of the two sensitivities measures whether a signal blames the right cause.

## Repository layout

```
step*.ipynb  (root)  Pilot: proof of concept on UCI Handwritten and a 200-frame KITTI pilot
scale_1k/   1,000-frame development scale, including all robustness studies
full/       Full 7,481-frame KITTI instantiation (main results)
```

Each notebook is saved **with its outputs**. These outputs are the recorded measurements reported in the paper.

## Which notebook produces which result

| Paper | Folder | Notebook |
|---|---|---|
| Sec. IV-A (dataset, per-view accuracy, knob preconditions) | `full/` | `step7_kitti_data.ipynb` |
| Table I, Figs. 3 and 4 (isolated, JS, projection consistency) | `full/` | `step8_kitti_panel.ipynb` |
| Sec. IV-D Lock 2, Sec. V-C, Sec. VI (ECML) | `full/` | `step9_ecml_audit.ipynb` |
| Sec. IV-D behavioral certification, Sec. V-B (PDF) | `full/` | `step10_pdf_audit.ipynb` |
| Figs. 2 and 5 (qualitative galleries) | `full/` | `step12_paper_figures.ipynb`, `step12b_qualitative_figure.ipynb` |
| Sec. VI-C scale trajectory (development scale) | `scale_1k/` | `step7` to `step10` |
| **Sec. VII, Figs. 7 and 8** (drift caps, corruption family B, seeds, slope estimators) | `scale_1k/` | `step11_stress_tests.ipynb` |
| TMC reproduction on the pilot benchmark (Sec. IV-C) | `pilot/` | Handwritten notebooks |

## Setup

Tested with Python 3.12.

```bash
pip install -r requirements.txt
```

A CUDA GPU is recommended for the KITTI notebooks but not required.

## Data

**UCI Handwritten (pilot):** downloaded automatically by the pilot notebooks.

**KITTI:** not redistributed. Download from the [KITTI 3D Object Detection benchmark](https://www.cvlibs.net/datasets/kitti/eval_object.php?obj_benchmark=3d) (free registration):

- Left color images (`data_object_image_2.zip`)
- Velodyne point clouds (`data_object_velodyne.zip`)
- Camera calibration matrices (`data_object_calib.zip`)
- Training labels (`data_object_label_2.zip`)

Then set `KITTI_ROOT` and `ZIP_DIR` in the config cell of each `step7` notebook to your local paths. `step7` extracts only the frames needed for its scale (`N_FRAMES`).

## Running order

Within each of `scale_1k/` and `full/`, run the notebooks in step order. Each step saves artifacts (`.pt` files) into its own folder that the next step loads. Keep each scale in its own folder so artifacts from different scales never overwrite each other.

## Published methods

Two published methods are audited through faithful ports:

- **ECML** (Reliable Conflictive Multi-View Learning, Xu et al., AAAI 2024): loss and conflict computation included verbatim from the authors' official repository.
- **PDF** (Predictive Dynamic Fusion, Cao et al., ICML 2024): confidence, Holo-Confidence, and calibration computations transcribed from the official repository at commit `8648674`.

Please cite the original works if you use these components.

## Citation

```bibtex
@article{ralte2026blaming,
  title   = {Blaming the Wrong Sensor: Auditing Misalignment Confounds in
             Multimodal Reliability Estimation},
  author  = {Ralte, Vanlalhmangaihsanga and Chhangte, Lalengmawia},
  year    = {2026},
  note    = {Under review}
}
```

## License

Code is released under the MIT License. KITTI data is subject to its own license (CC BY-NC-SA 3.0) and is not included.
