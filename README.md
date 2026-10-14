# UrbanEV Blog

A Jekyll / GitHub Pages blog about the UrbanEV EV-charging dataset, styled to match [ml-blog](https://github.com/SpyrosAnagnosto/ml-blog).

## Posts
- [Understanding UrbanEV](dataset-overview.md) — from `notebooks/01_dataset_overview.ipynb`
- [UrbanEV Data Quality Assessment](data-quality.md) — from `notebooks/02_data_quality.ipynb`

The Markdown posts include the notebook code, outputs, tables and exported figures. The original notebooks are also included for reproducibility. Running notebooks yourself requires the UrbanEV CSV data in a local `data/` folder; this repository does not redistribute the dataset.

## Publish
1. Create a **public** GitHub repository named `urbanev-blog` under `SpyrosAnagnosto`. Do not initialize it with a README.
2. Upload **the contents** of this folder to the root of the new repository (including `_layouts`, `assets`, and `notebooks`). You can use **Add file → Upload files** and drag the extracted contents.
3. Open **Settings → Pages**. Set **Build and deployment: Deploy from a branch**, then select **main** and **/(root)**, and save.
4. Open `https://spyrosanagnosto.github.io/urbanev-blog/` after GitHub Pages publishes.

If the repo name differs from `urbanev-blog`, update the `baseurl` in `_config.yml` and the header GitHub URL in `_layouts/default.html`.
