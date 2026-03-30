# Framework for the Quantification of Crops’ Consistency

## Row Detection
### (a) Row Segmentation
Semantic segmentation of the input image to produce a binary mask where crop rows are separated from the background. This step leverages a deep neural network trained on annotated field images, generating pixel‑wise predictions that isolate the crop canopy.
### (b) Row Thinning
The binary segmentation mask is skeletonized to a single‑pixel‑wide representation of each row. Morphological thinning algorithms (e.g., Guo‑Hall) reduce the row regions to their medial axes, preserving the geometric structure while removing redundant pixels.
### (c) Line Association to the Rows
The thinned row skeletons are grouped into individual row instances using clustering based on spatial proximity and orientation. Each cluster is fitted with a line (e.g., via Hough transform or linear regression) to represent the row’s central axis.
### (d) Line Extension to the Edges of the Image
Each detected row line is extrapolated to the image boundaries, ensuring full coverage from the top to the bottom of the field. This extension facilitates consistent row counting and spatial analysis across the entire image.

## Plant Stem Detection
Individual plant stems are identified within each segmented row region using [YOLOv8](https://github.com/ultralytics/ultralytics),. Stem positions are recorded as key points for subsequent counting and spacing analysis.

## Plants Per Row Counting
The number of plants per row is computed by counting the stem detections that fall within the boundaries of that row. Outputs include row‑wise plant counts, which can be aggregated to obtain total plant population per image or field plot.

## Inter-Row Plants Spacing
Distances between consecutive plants within the same row are calculated using the Euclidean distance between stem positions along the row line. Statistical measures (e.g., mean, variance, coefficient of variation) are provided to quantify spacing uniformity, which is a key indicator of crop consistency.
