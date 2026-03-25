# AbserneyVision

A lightweight custom object detection model built for the [Abserny](https://github.com/abserny/abserny-mobile) assistive vision app. Designed to run on-device with no internet connection, helping visually impaired users identify objects in their home and office environment.

## What it detects

15 essential home and office objects:

| Class | Class | Class |
|-------|-------|-------|
| Chair | Table | Door |
| Person | Laptop | Phone |
| Cup | Bed | Sofa |
| Television | Stairs | Refrigerator |
| Sink | Backpack | Bottle |

## Model details

- **Architecture**: MobileNetV2 + custom classifier head (transfer learning)
- **Input**: 224×224 RGB image
- **Output**: 15-class softmax probabilities
- **Format**: TFLite (INT8 quantized)
- **Size**: ~5MB
- **Training**: 2-phase — classifier head first, then fine-tuning top 30 layers

## Project structure

```
AbserneyVision/
├── train.py              # Full training pipeline
├── download_dataset.py   # Download training images
├── test_model.py         # Test model on an image
├── requirements.txt      # Python dependencies
├── dataset/              # Training images (not committed)
│   ├── chair/
│   ├── table/
│   └── ...
└── export/               # Output files
    ├── AbserneyVision.tflite
    └── labels.json
```

## How to train

### 1. Install dependencies
```bash
pip install -r requirements.txt
```

### 2. Download dataset
```bash
python download_dataset.py
```
This downloads ~200 images per class from Open Images Dataset v7.

Or add your own images manually — just put them in `dataset/<classname>/` folders.

### 3. Train
```bash
python train.py
```
Training takes ~5 min with GPU, ~30 min with CPU.

### 4. Test
```bash
python test_model.py path/to/photo.jpg
```

## Output

After training you'll find in `export/`:
- `AbserneyVision.tflite` — model for the Abserny app
- `labels.json` — class index mapping
- `training_history.png` — accuracy/loss chart

## Using in Abserny app

Copy `AbserneyVision.tflite` and `labels.json` to your Expo project's `assets/model/` folder. The app loads the model locally with no internet needed.

## License

MIT
