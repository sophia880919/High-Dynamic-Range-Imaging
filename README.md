# High-Dynamic-Range-Imaging
Compared to Low Dynamic Range (LDR) images, High Dynamic Range (HDR) images display a broader luminance range, revealing more details and subtle color variations. This Python-based project includes functions for image rescaling, cropping, alignment, histogram equalization (CLAHE), HDR reconstruction, and tone mapping.

## Description 
Compared to Low Dynamic Range (LDR) images, High Dynamic Range (HDR) images can display 
a broader range of luminance levels within the same pixels, thus revealing more details and subtle 
color variations. This project (Figure1), implemented in Python, offers users the capability to convert 
LDR images into HDR images. It encompasses a suite of image processing functions, including image 
rescaling, cropping, image alignment, histogram equalization (specifically Contrast Limited Adaptive 
Histogram Equalization - CLAHE), HDR reconstruction, and tone mapping, all designed to enhance 
the detail and dynamic range of digital images. 

![image](https://github.com/user-attachments/assets/2d997eae-8c07-4c7e-90aa-fa60a0386196)


## Implement 
### Step1. Data processing  
(The corresponding implementations are rescale and crop_image functions in hdr.py) 
We capture 13 photos (Figure 2) under different exposures, 3s, 2s, 1s, 1/2s, 1/4s, 1/5s, 1/8s, 1/13s, 
1/20s, 1/30s, 1/50s, 1/100s, 1/160s, 1/250s, respectively. The photos are show below. During capturing 
process, we use a tripod to ensure the stability of camera. We rescale the intensity range of an image 
from any range to a [0, 1] range. This is useful for standardizing data to a specific range. 

![image](https://github.com/user-attachments/assets/e4b017d6-4b1f-462f-9ffa-2a786d0b52fe)


### Step2. Alignment  
(The corresponding implementations are align_two_images and translate functions in hdr.py) 
We use the phase correlation method to finish our alignment between images. This method is relies 
on the Fourier shift theorem, which states that a spatial shift in the time-domain corresponds to a linear 
phase change in the frequency domain. By comparing the phase information of two images in the 
frequency domain, it's possible to accurately determine the relative shift between them.  


### Step 3. HDR Reconstruction 
(The corresponding implementations are gsolve and hdr_recon functions in hdr.py) 
When reconstructing the HDR image, we employ two distinct strategies for selecting points from 
the images: random sampling and structured grid-based sampling. These methods are crucial for 
accurately solving the camera response function, a key step in merging multiple images of different 
exposures into a single HDR image. The hdr_recon function uses the obtained camera response 
function and estimated logarithmic exposure times to reconstruct the HDR image. By combining these 
points through both random and structured approaches, we ensure a comprehensive coverage of the 
image's dynamic range, facilitating a more accurate and detailed HDR reconstruction. 
After trying both random sampling and grid-based sampling, we found that the convergence of 
the results was not significantly different (Figure 3). However, the outcomes from grid-based sampling 
were relatively more stable, unlike random sampling, which can yield different results depending on 
the points selected. Additionally, regarding the number of sampling points, we experimented with 
setting N to 625, 1444, 3136, and 4096 sampling points for comparison. It was observed that as N 
increases, the processing time becomes longer, but the effectiveness also improves. Therefore, we can 
consider resizing the image to reduce its size as a way to increase computational speed.

![image](https://github.com/user-attachments/assets/5cab9eaf-72e5-4215-b267-af163b53427b)


### Step 4. Histogram Equalization  
(The corresponding implementation are hist_equalize and other functions in hdr.py) 
This step implements the CLAHE algorithm. CLAHE enhances local contrast by dividing the image 
into multiple small blocks (tiles), applying histogram equalization to each block independently, and 
limiting the enhancement of contrast to avoid amplification of noise. im_hist calculates the histogram 
for a single block, clip_histogram clips the histogram to limit contrast enhancement, make_mapping 
creates brightness mappings based on the clipped histogram, and make_clahe_image applies these 
mappings and reassembles the processed blocks into a complete image. 
We try different norm_clip_limit value in hist_equalize. norm_clip_limit is a parameter that determines 
the normalized clip limit for the histogram equalization of each tile. It's essentially a threshold that 
controls the maximum allowed enhancement of contrast to prevent amplifying noise in nearly uniform 
regions of the image (Figure 4). We can find if we increase the clip limit, allowing more contrast 
enhancement before clipping occurs. This can lead to more pronounced adjustments in image areas 
with low contrast but might also amplify noise in nearly uniform regions. 

![image](https://github.com/user-attachments/assets/982d6553-bb62-4aea-9bd7-5178950408b5)


### Step 5. Tone Mapping 
(The corresponding implementation are tonemap and tone_operator in hdr.py) 
Finally, the function converts the HDR image into an LDR (Low Dynamic Range) image suitable 
for viewing on standard displays, by adjusting the brightness range and color saturation (Figure 5).

![image](https://github.com/user-attachments/assets/12306490-c56f-4975-9a9b-c11fc02a2094)


## Conclusion 
Based on the results illustrated in Figure 5, adjusting the N setting to 4096 resulted in superior HDR 
outcomes. When comparing various norm settings, both results achieved with norm set to 0.03 and 
0.003 exhibit commendable qualities. Despite a minor overexposure observed in the image using norm 
0.003 compared to norm 0.03, the former offers a noticeably more aesthetically pleasing visual 
outcome. Therefore, we have opted for the value 0.01, which lies between the aforementioned norm 
settings, as our final output result (see Figure 6).

![image](https://github.com/user-attachments/assets/e0bce42a-380f-49cd-a6e4-e750cc52b9b1)

