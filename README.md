
# Oil Spillage Detection in the Gulf of Niger using Google Earth Engine

This repository contains Python code for detecting oil spillage in the Gulf of Niger using Google Earth Engine, Google Colab, and Google Cloud authentication. The project utilizes the `geemap` library to create a backend log from Earth Engine to visualize oil spillage areas. Additionally, preprocessing techniques are applied to visualize SAR Sentinel imagery of the oil spill locations, and machine learning models are employed in the detection process.

## Installation

To run this code, you need to install the following libraries:

```bash
!pip install geemap
!pip install earthengine-api
```

## Usage

To visualize oil spillage areas, run the following code:

```python
import os
import ee
import geemap

# Initialize Earth Engine
ee.Initialize()

# Define a region of interest (ROI)
roi = ee.Geometry.Point([5.9, 4.3])

# Filter Sentinel-1 GRD image collection
imgVV = (ee.ImageCollection('COPERNICUS/S1_GRD')
         .filter(ee.Filter.listContains('transmitterReceiverPolarisation', 'VV'))
         .filter(ee.Filter.eq('instrumentMode', 'IW'))
         .filter(ee.Filter.eq('orbitProperties_pass', 'ASCENDING'))
         .filterDate('2020-08-24', '2020-08-30')
         .select('VV'))

# Calculate the mean image
spImgAugL = imgVV.mean()

# Define visualization parameters
vis_params = {
    'min': -25,
    'max': 5,
    'palette': ['a50026', 'd73027', 'f46d43', 'fdae61', 'fee08b', 'ffffbf',
                'd9ef8b', 'a6d96a', '66bd63', '1a9850', '006837'],
}

# Create a map
Map = geemap.Map()
Map.setCenter(5.9, 4.3, 12)
Map.addLayer(spImgAugL, vis_params, 'Late August Spill', True)
Map

# Define the output image path
out_img = os.path.expanduser("~/path/sentinel_sar.jpg")

# Get the image thumbnail and save it
geemap.get_image_thumbnail(spImgAugL, out_img, vis_params, dimensions=300, format='jpg')

# Show the saved image
geemap.show_image(out_img)
```

## Contributors

- [Michael Seer](seermike411@gmail.com)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```
