# SketchFuse

### Abstract
Inpainting is a process used to complete missing or corrupted parts of images. Many current inpainting methods do not allow additional user input except masks, causing a loss of generation control, while those using text prompts
risk ambiguity and the omission of key visual details. To address these issues, we introduce ”SketchFuse”, which **enables users to guide the inpainting process using sketches**, making the outcome more aligned with user’s intent. Additionally, SketchFuse provides a complete workflow, which not only generates images based on user sketches but also **converts the results into 3D point clouds**. Our tests on the Bonn Furniture Styles dataset demonstrate that our approach outperforms current methods in both objective and subjective assessments.

### 3D results
<p align="center">
  <img src="resultgif.gif" width="250"/>
</p>

### 2D results
<p align="center">
  <img src="result.png" />
  <img src="result2.png" />
</p>

### Method
<p align="center">
  <img src="final.png">
</p>
Our algorithm handles the inpainting task, letting users guide the process with a sketch. This way, the final outcome better matches what the user wants. It finally produces a 3D point cloud to give users more realistic experience.

In the first stage, called the preprocessing stage, we find the edges of the original picture using Canny Edge Detection. Users can then adjust the edges using various software tools.

In the second stage, we use ideas from a method called *Repain: Inpainting using Denoising Diffusion Probabilistic Models*. This stage is divided into two parts. One part deals with the area that isn't masked, and the other focuses on the masked area. We add noise to the non-masked area, and for the masked area, we use ControlNet and consider both user sketches and text prompts as input. However, just combining the results from these two areas isn’t enough, as it lacks coherence. To improve coherence, we add noise to the result and let the result go through the lower part again then repeating this process several times to better integrate the information from masked area and non-masked area.

After stage 2, the result still has noticeable boundaries. To fix this, we move to stage 3. Here, we focus on the mask edges, making them thicker to cover their surrounding areas. We then use a pretrained stable diffusion inpainting model to regenerate the results, which helps in removing the boundaries.

Lastly, in stage 4, we estimate the image's depth using MiDas. With the color and depth information, we can finally reconstruct the 3D point cloud.
