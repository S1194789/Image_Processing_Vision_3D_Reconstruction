# Camera Calibration & 3D Point Cloud Reconstruction (Lab 1)

This project implements a complete pipeline for reconstructing 3D point clouds  
using grayscale/RGB images and depth maps, computing camera intrinsics, and  
colorizing the final 3D points. All formulas are placed inside code blocks  
to prevent rendering issues.

---

## 1. Generate Grayscale 3D Point Cloud

The reconstruction uses the classic pinhole camera model.

### Projection formulas (pixel → 3D)

          X = (u - cx) * Z / fx
          Y = (v - cy) * Z / fy
          Z = depth_value


### Steps performed
- Load grayscale image (0–255)
- Load depth map (.npy)
- Back-project each valid pixel to 3D
- Construct an N×3 point array
- Normalize grayscale intensity to [0,1]
- Save output file: `gray_pointcloud.ply`

---

## 2. Compute Intrinsic Matrix K2

### Intrinsic matrix structure


          K = [
              [fx,  0, cx],
              [ 0, fy, cy],
              [ 0,  0,  1]
          ]

      
### Steps
- Load calibration parameters for camera 2
- Construct 3×3 intrinsic matrix (K2)
- Validate using image dimensions

---

## 3. Colorizing the 3D Point Cloud

The colorization process re-projects 3D points into the RGB image plane using  
intrinsic K2 and extrinsic parameters R, T.

### Projection Model (3D → 2D)

          [u]     [fx  0  cx] [R | T] [X]
          [v]  =  [ 0 fy  cy]        [Y]
          [1]     [0   0   1]        [Z]
                                       [1]


### Pixel coordinate formulas


          u = (fx * X' / Z') + cx
          v = (fy * Y' / Z') + cy

 
### Steps
- Transform 3D points with extrinsics (R, T)
- Project onto RGB image
- Fetch pixel colors: `R,G,B = rgb_image[v, u]`
- Assign colors to each 3D point
- Save output file: `colored_pointcloud.ply`

---

## 4. Visualization

Point-cloud visualization is done using Matplotlib’s 3D scatter:

- Downsample points
- Apply RGB colors
- Invert Y/Z axes to match camera convention
- Display interactive 3D plot

---

## 5. Output Files

- `gray_pointcloud.ply` — grayscale point cloud  
- `colored_pointcloud.ply` — RGB-colored point cloud  

---

## 6. Technologies Used

- Python  
- NumPy  
- Matplotlib  
- Pillow  
- Open3D  
- Camera projection geometry  
- Depth-based 3D reconstruction  

---

## Summary

This project performs complete 3D reconstruction from depth + image data,  
computes camera intrinsics, and produces both grayscale and colorized 3D point clouds.  
The pipeline is useful for calibration, multi-view geometry, and 3D perception tasks.
