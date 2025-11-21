project: "Camera Calibration & 3D Point Cloud Reconstruction (Lab 1)"

description: >
  This project implements a complete pipeline for reconstructing 3D point clouds
  using grayscale/RGB images and depth maps, computing camera intrinsics, and
  colorizing the final 3D points. All formulas and steps are included in raw text
  to avoid markdown rendering issues.

sections:

  - ## 1. Generate Grayscale 3D Point Cloud
    content: |
      The reconstruction uses the classic pinhole camera model.

      Projection formulas (back-projection from pixel → 3D):

          X = (u - cx) * Z / fx
          Y = (v - cy) * Z / fy
          Z = depth_value

      Steps performed:
        - Load grayscale image (0–255) //
        - Load depth map (.npy) //
        - Back-project each valid pixel to 3D
        - Construct Nx3 point array
        - Normalize grayscale intensities to [0,1]
        - Save output file: gray_pointcloud.ply

  - ## 2. Compute Intrinsic Matrix K2 
    content: |
      Intrinsic camera matrix structure:

          K = [
              [fx,  0, cx],
              [ 0, fy, cy],
              [ 0,  0,  1]
          ]

      Steps:
        - Load calibration parameters for camera 2
        - Construct 3×3 intrinsic matrix (K2)
        - Validate using image width/height

  - ## 3. Colorizing the 3D Point Cloud
    content: |
      The colorization process projects 3D points into the RGB image plane
      using intrinsic K2 and extrinsic parameters R, T.

      Projection model:

          [u]     [fx  0  cx] [R | T] [X]
          [v]  =  [ 0 fy  cy]        [Y]
          [1]     [0   0   1]        [Z]
                                       [1]

      Final pixel coordinates:

          u = (fx * X' / Z') + cx
          v = (fy * Y' / Z') + cy

      Steps:
        - Transform 3D points with extrinsics (R, T)
        - Project them on the RGB image
        - Fetch pixel colors: R,G,B = rgb_image[v, u]
        - Assign these colors to point cloud
        - Save output file: colored_pointcloud.ply

  - ## 4. Visualization
    content: |
      Visualization uses Matplotlib 3D scatter:

        - Downsample points
        - Colors from RGB values
        - Invert Y and Z axes to match camera convention
        - Display interactive 3D plot

  - ## 5. Output Files
    content: |
      gray_pointcloud.ply           - grayscale point cloud
      colored_pointcloud.ply        - RGB colored point cloud

  - ## 6. Technologies Used
    content: |
      Python
      NumPy
      Matplotlib
      Pillow
      Open3D
      Camera projection mathematics
      Depth-based 3D reconstruction

summary: >
  This project performs full 3D point cloud reconstruction from depth + image data,
  computes camera intrinsics, and generates both grayscale and RGB-colored point clouds.
  The entire pipeline is useful for multi-view geometry, calibration practice, and
  3D perception tasks.

