1. Why Gaussian filtering before Canny?
Canny is very sensitive to noise. The Gaussian filter smooths small variations (skin texture, hair, camera noise) so they aren't detected as false edges, while the lesion border stays visible.

2. How did the three thresholds affect the result?

50-100 (low): detected too many edges, including noise, hair and skin texture.
100-200 (medium): gave a clear lesion border with little noise.
150-250 (high): removed noise, but also lost weak parts of the border, so it came out broken.

3. Which threshold gave the best boundary?
100-200 in most images. It kept the lesion edge strong and continuous while removing most background noise. (Check this against your own printed scores and plots, and change it if your results differ.)

4. Why are edges useful for detecting skin lesions?
A lesion is usually darker or differently coloured than the surrounding skin, so there is a sharp intensity change at its border. Edge detection finds that change, which gives the lesion's shape, area and perimeter. This helps with measuring size and checking border irregularity.

5. Problems observed

Hair and skin lines created extra edges.
Low-contrast or fuzzy borders gave broken edges.
Some lesions touching the image border were harder to extract.
Edges had small gaps, so morphological closing was needed to form a closed contour.
No single threshold worked perfectly for every image.

6. How could the method be improved?

Remove hair first (for example with the DullRazor method or morphological black-hat filtering).
Use adaptive or automatic thresholds (such as Otsu-based Canny) for each image.
Combine edges with colour or Otsu thresholding and morphological operations.
Use advanced methods such as GrabCut, active contours or a deep learning model like U-Net.
Compare results against ground-truth masks to measure accuracy.
