# Photo to STL — User Instructions

Photo to STL turns a photograph into a downloadable STL mesh for 3D printing. It creates a **heightmap relief**: image brightness controls the height of the surface. It does not reconstruct hidden sides or create a complete 3D model of the photographed object.

## Open the website

Visit: **https://mentlone.github.io/photo-to-stl-/**

The site runs in your browser. The 3D preview library is loaded from a CDN, so an internet connection is needed for the preview to load.

## Create an STL

1. Open the website.
2. Tap or click **Choose photograph** and select a JPG, PNG, WebP, or another image format supported by your browser.
3. Adjust the settings below.
4. Tap or click **Generate 3D relief / STL**.
5. Wait for the status to say the STL is ready.
6. Select **Download STL** to save `photo-relief.stl`.
7. Open the STL in your preferred slicer (for example, Cura, PrusaSlicer, or Bambu Studio), check the model orientation and dimensions, slice it, and preview the toolpaths before printing.

## Settings explained

- **Model width (mm):** Sets the model's width. The height is calculated from the photograph's aspect ratio.
- **Relief height (mm):** Sets how much the image brightness changes the surface height.
- **Base thickness (mm):** Sets the minimum solid thickness underneath the relief.
- **Mesh resolution:** Controls the number of samples across the image. Higher values preserve more detail but create larger files and can take longer to process.
- **Contrast:** Increases or decreases the difference between light and dark areas before they are converted into height.
- **Invert height:** Reverses the height mapping so dark areas are raised instead of bright areas.

## Tips for better results

- Use a sharp, high-resolution photograph with a clearly separated subject.
- Portraits, logos, silhouettes, and images with strong light/dark contrast tend to work well.
- Crop away distracting background details before uploading.
- Start with a modest resolution, then increase it if you need more detail.
- If generation is slow or fails, lower the mesh resolution and try again.
- Remember that the model is a relief. It is useful for plaques, decorative panels, and image reliefs, but it won't create the back or sides of a real object.

## Troubleshooting

**The preview does not load**
- Check your internet connection and reload the page. The preview depends on a third-party JavaScript library loaded from a CDN.

**The model takes too long or generation fails**
- Lower the mesh resolution and try again. Very large images and high resolutions can use substantial browser memory.

**The STL opens at the wrong scale**
- The file uses millimeters. Check the units and dimensions in your slicer.

**The website shows a 404**
- Confirm GitHub Pages is enabled for the repository and wait briefly after a deployment. Check the repository's Actions tab for the latest deployment status.

## Privacy and limitations

- The image is processed in the browser; the application does not intentionally upload the photograph to a server.
- The preview uses an external CDN library, which requires internet access.
- This tool is provided for experimentation. Always inspect the generated mesh and slicer preview before printing; results depend on the input image and settings.

## Repository

Source code and deployment status: https://github.com/MENTLONE/photo-to-stl-


## New feature: photo standee

The **Photo standee** tool attempts to remove a plain background by comparing image pixels with the average color of the image corners. It then creates a thick silhouette STL with a small support foot.

1. Use a front-facing photo with a plain, evenly lit background.
2. Select the photo.
3. Adjust width, thickness, background-removal threshold, and edge detail.
4. Generate the standee and download `photo-standee.stl`.
5. Inspect it in your slicer before printing.

This is a simple browser-based background estimate, not AI segmentation. Busy backgrounds, hair, similar subject/background colors, and shadows can produce poor cutouts. Increase the background-removal threshold if background remains; decrease it if parts of the person disappear. The silhouette edge is pixel-based and may look stepped. The support foot is basic and may need adjustment for your printer or model.

## New feature: full 3D reconstruction from multiple views

The **Full 3D reconstruction (AI)** tool sends 1–4 photographs of the same person or object to the external Meshy multi-image-to-3D API.

1. Create a Meshy account and obtain your own API key from [Meshy](https://www.meshy.ai/).
2. Select 1–4 JPG, PNG, or WebP images (maximum 20 MB each). Use consistent lighting and show the same subject from different angles. The first image should be the front view.
3. Paste your Meshy API key into the password field. It is used in page memory only and is not stored by the app.
4. Choose an optional A-pose or T-pose, then start generation.
5. Wait for the AI task to finish and use the generated STL link.

**External service, cost, and privacy:** This feature sends selected photos to Meshy's servers. Meshy may require a paid plan or credits; check its current pricing and terms before generating. Never put an API key into source code or commit it to this public repository. Do not use an API key on a shared or untrusted device. The key is not written to local storage by this app.

**Browser integration limitation:** This GitHub Pages version calls the Meshy API directly from the browser. If the browser blocks requests due to CORS, or your account/API configuration prevents browser calls, the full 3D mode will not work from this static page. The reliable solution is a small server-side proxy (for example, a serverless function) that keeps the API key as a server secret; GitHub Pages alone cannot securely store server secrets. Do not expose your private API key to solve a CORS problem.

AI-generated models are estimates and can have missing details, distorted anatomy, or non-manifold geometry. Check the STL in a slicer or mesh-repair tool and verify scale, wall thickness, stability, and supports before printing. A single image cannot reveal the hidden sides; multiple well-spaced views usually help.
