---
published: true
---
switch on Camera2 API
  to more advanced controls of the camera, like manual exposure (ISO, shutter speed), focus and so on.
  
shutter is limited in how quickly it can open and close.
For example, shooting at 24 frames per second, the shutter would be open for 1/48 of a second, 24 times per second.

Slower frame rates produced motion blur.
if you leave your camera shooting in auto exposure mode, in bright conditions the camera will set shutter speed very high.

?To achieve a nice smooth look, a general rule is to keep the ISO low and the Shutter Speed slow.

ISO
when you move this slider too far, digital noise will begin to appear in your image.

Shutter Speed
For example, in countries which have a 50hz electricity supply, a 1/50 shutter speed is needed. Otherwise you can get a strobing effect in the video. But the difference between 1/48 and 1/50 is tiny in terms of motion blur created – you will not notice the difference.

Overexposure in Daylight
Pretty much the only way to get past this problem is to place an ND (neutral density) filter over your lens.

Underexposure in Low Light
When there’s not enough light, we have to push ISO up and this creates digital noise in the image.

Noise Reduction
To help reduce the digital noise created in low light situations, you can set your smartphone to apply noise reduction.

ref
How to Get the Film Look: Open Camera Tutorial
  https://momofilmfest.com/how-to-get-the-film-look-open-camera-tutorial/
  
DRO

Dynamic Range Optimisation (DRO) is a technique that optimises the dynamic range available in the image. In particular, dark regions will have their brightness boosted to bring out the detail.

DRO vs HDR
DRO requires only a single image from the sensor, so shots are fast to take, and fine for scenes with movement, unlike HDR.
HDR will in general be better at scenes with a high range of brightness values.

NR vs HDR
NR is better if you just want a "works best in most cases" option. HDR may be a better choice specifically in scenes with high dynamic range, that also don't have movement in the scene.

Noise Reduction
In dark scenes, NR will also apply pixel binning, merging 4 pixels into 1 to reduce noise. Therefore in such cases , the resultant photo resolution will be halved in width and height.
Taking photos with NR is significantly slower than regular photos.
  https://opencamera.sourceforge.io/help.html#dro
  
Night mode of the Open camera app adds extra clarity in the pictures when shot in dark mode. 
Portrait mode adds a background blur effect to the subject in the focus.

noise reduction
noise reduction feature in the Open camera app reduces any noise which causes the image to lose its sharpness. This mode is only recommended to use while you are taking pictures in artificial light like indoors.
  https://gadgetstouse.com/blog/2020/09/07/hidden-features-in-open-camera-app-android/
Open Camera will take a burst of up to 8 photos, align and merge them into a single image, reducing noise in the resultant photo.
In NR mode, Open Camera turns off certain post-processing options, including the device's usual filtering options to reduce noise - the problem is that whilst such filtering might smooth out noise (especially chroma noise), it also eliminates detail.
  https://sourceforge.net/p/opencamera/blog/2018/09/noise-reduction-photo-mode/