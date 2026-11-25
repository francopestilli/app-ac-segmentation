# AC Segmentation

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Segments axons in volumetric light-microscopy OME-Zarr data with the Allen Institute ac_segmentation RSUNet model and outputs a labelled OME-Zarr volume.**

Runs the Allen Institute's segment_array.py with its bundled pretrained RSUNet checkpoint inside the gamorosino/ac-segmentation container, on CPU, using the first resolution level of the input OME-Zarr and an optional mask. Use it for automated axon segmentation of light-sheet or similar microscopy volumes.

| | |
|---|---|
| 📥 **Inputs** | `neuro/ome-zarr` · `neuro/ome-zarr` (optional) |
| 📤 **Outputs** | `neuro/ome-zarr` |
| 🐳 **Container** | `gamorosino/ac-segmentation:latest` |
| ⚙️ **Resources** | 4 GB memory · 1 h walltime |
| 🔗 **Run it** | [doi.org/10.25663/brainlife.app.968](https://doi.org/10.25663/brainlife.app.968) |

#### 📥 Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `input` | `neuro/ome-zarr` | yes | Volumetric light-microscopy image (OME-Zarr); resolution level 0 is segmented. |
| `mask` | `neuro/ome-zarr` | no | Optional OME-Zarr mask restricting the segmentation. |

#### 📤 Outputs

| Output | Datatype | Description |
|---|---|---|
| `output` | `neuro/ome-zarr` | Segmented volume (uint8-labeled OME-Zarr) produced by the RSUNet model. |

<details>
<summary><b>⚙️ Parameters</b> (2)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `dsfactor` | number | `1` | Mask downsample factor (segment_array.py --dsfactor). |
| `filter_max_intensity` | number | `30000` | Maximum intensity threshold used for contrast preprocessing before segmentation (segment_array.py --filter_max_intensity). |

</details>

<!-- brainlife-app:end -->

Automated axonal segmentation of volumetric light-microscopy Zarr data, using the Allen Institute's ac_segmentation RSUNet model (segment_array.py) with the model's bundled pretrained checkpoint.

## Inputs

- `input` (neuro/ome-zarr)
- `mask` (neuro/ome-zarr) -- optional

## Outputs

- `output` (neuro/ome-zarr) -- Segmented volume (uint8-labeled OME-Zarr) produced by the RSUNet model.

## Usage

Brainlife.io: run via `braise-app-run`/`braise-app-pipeline` (once registered
with `braise-app-create`), or the web UI.

Locally (outside brainlife): copy `config.json.example` to `config.json`,
fill in real file paths, then run `./main` from this directory. `main` pulls
its own container (`singularity exec docker://...`) -- no local install of
the underlying tool needed, only Singularity itself.

Entrypoint: `segment_array.py`, run from inside the pinned container
(`docker://gamorosino/ac-segmentation:latest`) -- not vendored into this repo,
see `segment_array.py` here for why.

Runs on CPU only: `segment_array.py`'s own `--gpu_device` argument is not
actually wired to the underlying pipeline as of the upstream code this app
pins, so it always executes on CPU regardless of what's passed. This app is
registered with `requires_gpu: false` to match.

## Credits

Wraps [AllenInstitute/ac_segmentation](https://github.com/AllenInstitute/ac_segmentation)
(`segment_array.py`, RSUNet model + bundled checkpoint), licensed GPLv3 by
the Allen Institute. That code is not redistributed here -- it runs inside
the pinned container image built from ac_segmentation's own Dockerfile.

## Authors

- Gabriele Amorosino <g.amorosino@gmail.com>

## License

This wrapper (`main`, `_brainlife_run.py`, config/docs) is MIT, see
`LICENSE`. The underlying tool it invokes (Allen Institute's
ac_segmentation) is GPLv3 -- see its own repository for terms.
