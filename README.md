# SAM3 Privacy Filter

Video privacy pipeline built on **SAM 3** (Segment Anything Model) that detects and segments people or faces in video using natural language prompts, then applies visual effects over the segmented regions frame by frame.

## How it works

1. **Segmentation** — Each frame is passed through SAM 3 with a text prompt (`"face, glasses"`, `"person"`, etc.). The model returns pixel-level masks for every detected region matching the prompt.
2. **Mask composition** — All masks returned for a frame are merged into a single alpha mask using a logical OR, so overlapping detections (e.g. face + glasses) are handled correctly.
3. **Effect application** — The composite mask is used to apply a visual effect only over the segmented region. The rest of the frame is left untouched.
4. **Batch processing** — Frames are buffered in batches (default: 8) and processed together to take advantage of GPU throughput.

The effect pipeline is modular — each effect is an independent function that receives the original frame and the composite mask, making it straightforward to add new effects without touching the video processing logic.

---

## Results

### Blur

Gaussian blur applied over detected faces.

<video src="https://github.com/user-attachments/assets/505ce059-466f-4597-9ae7-e0933fef1070" controls width="640"></video>

---

### Green Screen

Original background replaced with a solid green background. The segmented subject is preserved intact.

<video src="https://github.com/user-attachments/assets/7ceebb62-18e4-43aa-a43e-f778524446ff" controls width="640"></video>

---

### Silhouette

A purple outline is drawn around the segmented subject, with a soft purple tint applied over the subject region while keeping them visible.

<video src="https://github.com/user-attachments/assets/c34406d2-c14e-4893-8d87-d09926efb268" controls width="640"></video>

---

### Pixelate

Pixelation applied over the segmented region as an alternative anonymization method.

<video src="https://github.com/user-attachments/assets/20dbb0a7-c52c-4740-985a-26db7fb1f378" controls width="640"></video>
