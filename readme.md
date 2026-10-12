# Offer Explorer

Explorer Eye is a web app that uses AI and computer vision to help Grameenphone customers find GPStar loyalty offers. Point your camera at an everyday item, such as a coffee cup, laptop, cat or airplane, and the app shows offers that match it.

## Live Demo

**URL:** https://ferdous-gp.github.io/explore-eye/

> The app needs camera access. Open it over HTTPS (or `localhost`) and allow camera permission when prompted.

## How It Works

1. **Load the model:** When the page opens, it loads the [COCO-SSD](https://github.com/tensorflow/tfjs-models/tree/master/coco-ssd) object detection model with TensorFlow.js. The model runs in the browser.
2. **Start the camera:** The app opens the device camera, using the back camera when one is available.
3. **Capture and detect:** When you tap **Capture**, the video pauses and COCO-SSD looks for objects in the current frame. The app keeps the object it is most confident about.
4. **Match offers:** The app loads offers from `offers.json` and groups them by their `tags`. The detected object name (for example `cup`) is matched against these tags.
5. **Show results:** An overlay shows the matching offers in a [Splide](https://splidejs.com/) carousel. Each slide links to the offer page on grameenphone.com. If no offer matches, the overlay says so.

```mermaid
flowchart LR
    A[Camera frame] --> B[COCO-SSD detection]
    B --> C[Top object label]
    C --> D[Match tags in offers.json]
    D --> E[Offer carousel]
```

## Offer Data

- `scrapper/main.py` fetches offers from the Grameenphone Star offers API and saves the key fields (`nid`, `title`, `teaser`, `path`, `thumb_image`, `description`) to `offers_new.json`.
- An LLM generated the `tags` for each offer from its title and description, so offers can be matched to COCO-SSD object classes. The tagged data is stored in `offers.json`.

```bash
pip install requests
python scrapper/main.py
```

## Run Locally

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

## Tech Stack

- TensorFlow.js + COCO-SSD: object detection in the browser
- Splide: offer carousel
- Python (`requests`): offer scraper
- LLM: generating offer tags
