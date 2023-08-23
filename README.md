# Utility DCTLS
These are DCTLs that I have developed.

## Contents
- [Utility DCTLS](#utility-dctls)
    - [Contents](#contents)
- [Installation](#installation)
- [The Fuses](#the-fuses)
    - [Fuses](#fuses)
        - [DCTL Interpreter](#dctl-interpreter)
        - [FrameAvg Fuse](#frameavg-fuse)
        - [HDR Blending Fuse](#hdr-blending-fuse)
        - [Linear Exposure Fuse](#linear-exposure-fuse)
        - [Merge Adjacent Fuse](#merge-adjacent-fuse)
        - [MTF Curve Fuse](#mtf-curve-fuse)
        - [Periodic Frame Sampler Fuse](#periodic-frame-sampler-fuse)
        - [Pixel Logger Fuse](#pixel-logger-fuse)
- [The DCTLs:](#the-dctls)
    - [Effects](#effects)
        - [Bleach Bypass DCTL](#bleach-bypass-dctl)
        - [Film Curve DCTL](#film-curve-dctl)
        - [Film Grain DCTL](#film-grain-dctl)
        - [Halation DCTL](#halation-dctl)
        - [Lens Distortion DCTL](#lens-distortion-dctl)
        - [Linear Contrast DCTL](#linear-contrast-dctl)
        - [MTF Curve DCTL](#mtf-curve-dctl)
        - [Photon Noise DCTL](#photon-noise-dctl)
        - [Random Channel Mixer](#random-channel-mixer)
        - [Random Contrast Curve](#random-contrast-curve)
        - [Random Linear Contrast](#random-linear-contrast)
        - [RGB Linear Contrast DCTL](#rgb-linear-contrast-dctl)
        - [Subtractive Saturation DCTL](#subtractive-saturation-dctl)
        - [Tone Mapping DCTL](#tone-mapping-dctl)
        - [Vignette DCTL](#vignette-dctl)
    - [Operations](#operations)
        - [Addition Function DCTL](#addition-function-dctl)
        - [Clamp DCTL](#clamp-dctl)
        - [Color Generator DCTL](#color-generator-dctl)
        - [Cross Product DCTL](#cross-product-dctl)
        - [Dot Product DCTL](#dot-product-dctl)
        - [Gamma Function DCTL](#gamma-function-dctl)
        - [Invert DCTL](#invert-dctl)
        - [Log Function](#log-function)
        - [Matrix](#matrix)
        - [Modulo Function DCTL](#modulo-function-dctl)
        - [Multiplication Function DCTL](#multiplication-function-dctl)
        - [Polynomial Kernel DCTL](#polynomial-kernel-dctl)
        - [Power Function DCTL](#power-function-dctl)
        - [Projective Transformation Matrix DCTL](#projective-transformation-matrix-dctl)
        - [Sigmoid Function DCTL](#sigmoid-function-dctl)
        - [Sigmoid Kernel DCTL](#sigmoid-kernel-dctl)
        - [Softmax DCTL](#softmax-dctl)
        - [Tanh Function DCTL](#tanh-function-dctl)
        - [Unit Length DCTL](#unit-length-dctl)
        - [Vector Norm DCTL](#vector-norm-dctl)
    - [Utilities](#utilities)
        - [ACES Exposure DCTL](#aces-exposure-dctl)
        - [Bit Depth Estimator DCTL](#bit-depth-estimator-dctl)
        - [Blanking Checker DCTL](#blanking-checker-dctl)
        - [Channel Viewer DCTL](#channel-viewer-dctl)
        - [Chroma Subsampling DCTL](#chroma-subsampling-dctl)
        - [CIELUV DCTL](#cieluv-dctl)
        - [ColorChecker DCTL](#colorchecker-dctl)
        - [Color Ramp DCTL](#color-ramp-dctl)
        - [Cube Rotate DCTL](#cube-rotate-dctl)
        - [DaVinci Tone Mapping DCTL](#davinci-tone-mapping-dctl)
        - [Exposure Chart DCTL](#exposure-chart-dctl)
        - [False Color Generator DCTL](#false-color-generator-dctl)
        - [Gradient Smoothness Chart DCTL](#gradient-smoothness-chart-dctl)
        - [Levels Converter](#levels-converter)
        - [Log Curve](#log-curve)
        - [Luminance](#luminance)
        - [Output Blanking DCTL](#output-blanking-dctl)
        - [Printer Lights](#printer-lights)
        - [Quantize](#quantize)
        - [RGB Chips DCTL](#rgb-chips-dctl)
        - [Safety Lines DCTL](#safety-lines-dctl)
        - [Saturation Adjustment DCTL](#saturation-adjustment-dctl)
        - [Spherical Color Space DCTL](#spherical-color-space-dctl)
        - [T-Log Curve](#t-log-curve)
        - [Waveform Guides](#waveform-guides)
        - [White Mask DCTL](#white-mask-dctl)


# Installation
DCTLs should be placed in the following folder:

**MacOS**: `/Library/Application Support/Blackmagic Design/DaVinci Resolve/LUT`

**Windows**: `C:\ProgramData\Blackmagic Design\DaVinci Resolve\Support\LUT`

Fuses should be placed in the following folder to install them into the Fusion page of DaVinci Resolve:

**MacOS**: `/Library/Application Support/Blackmagic Design/DaVinci Resolve/Fusion/Fuses`

**Windows**: `C:\ProgramData\Blackmagic Design\DaVinci Resolve\Fusion\Fuses`

Fuses should also be placed in the following folder to install them for Fusion Studio:

**MacOS**: `/Library/Application Support/Blackmagic Design/Fusion/Fuses`

**Windows**: `C:\ProgramData\Blackmagic Design\Fusion\Fuses`

If a DCTL is not working, you can usually find logs in these directories. If you find that there is a problem, make an Issue on Github with your OS and Resolve version so I can fix it.

**MacOS**: `[Your user directory]/Library/Application Support/Blackmagic Design/DaVinci Resolve/logs/davinci_resolve.log` or `/Library/Application Support/Blackmagic Design/DaVinci Resolve/logs/davinci_resolve.log`

**Windows**: `C:\ProgramData\Blackmagic Design\DaVinci Resolve\Support\logs\davinci_resolve.log` or `[Your user directory]\AppData\Blackmagic Design\DaVinci Resolve\Support\logs\davinci_resolve.log`

# The Fuses

## Fuses


---

### DCTL Interpreter
Adds support for DCTLs within Fusion Studio rather than just Resolve. This is done by reading in a DCTL file and any headers, then rewriting several parts of the source code so it can be run natively within the Fuse DCTL framework (which is somewhat different than the Resolve DCTL framework).

#### Parameters
**DCTL File**: Allows you to specify a .dctl file, anywhere on your system.

**Debug to Console**: Prints out the changes made to the source code and the quantity of captured params.

**Explicitly Typecast Builtin Funcs**: The Fuse DCTL framework has a bug where builtin functions like `_exp2f()`, when provided with an integer argument, will cause the DCTL to fail to build. As a workaround, if you check this box I parse all the code and inject the appropriate type cast to each parameter. This can have some performance impact if your DCTL is large enough as it's a lot of string manipulations that would take place every frame.

**Don't run the DCTL Code**: Self explanatory, If your DCTL is causing Fusion to crash, checking this box might help you help me debug it. Doesn't always stop Fusion from crashing though.


---

### FrameAvg Fuse
Blends together several frames, can be used to retime projects shot at high frame rates. Should certainly be used with a float input, and likely be used with a Linear input.

#### Parameters
**Number of Frames**: quantity of frames to look ahead, including this frame.

**Frame Hold**: How long to hold the current frame (units are quantity of frames), allowing you to control it so that your resulting frames average nonoverlapping input frames.

#### Examples:
Suppose you shot a video at 240fps, 360degree shutter. To simulate 24fps 360degree shutter, you would set Number of Frames to 10, Frame Hold to 10. To simulate 30fps 180degree shutter, you would set Number of Frames to 4, Frame Hold to 8.


---

### HDR Blending Fuse
For the purpose of stitching HDR (multiple exposure composites) images.

#### How to use
1. Take two images of the same scene at different exposures.
2. Load the images into post and correct the exposures to match.
3. Take the image with the lower clipping point and connect it to the background of this Fuse. Connect the other image with the highlight detail to the foreground of the fuse
4. Set the threshold in the Fuse to just below the clipping point in the background image, set the feathering to be around 10% lower than that. Consider bumping the Blur to 1.0 to account for slight misalignment between the images.

#### Parameters
**Foreground Threshold**: Threshold above which the foreground image is copied in.

**Feather Threshold**: For background code values between Feather Threshold and Foreground Threshold, the two layers are blended together.

**Blur Amount**: Indicates how much to blur the mask resulting from the above two thresholds.

**Show Mask**: Shows the mask used. Each of the three channels is masked individually.


---

### Linear Exposure Fuse
Simply multiplies the input values by `2^x`, where `x` is the specified Exposure (Stops) value. Expects a Linear input.

#### Parameters
**Exposure (Stops)**: Exposure compensation to make in stops.



---

### Merge Adjacent Fuse
Simply sticks the foreground image to be adjacent to the background image, really quickly so that it doesn't require any work or the necessary three or four nodes. You can choose what direction the two images are concatenated.



---

### MTF Curve Fuse
This is a higher quality version of the MTF Curve DCTL. Here, we provide 5 frequency bands in which the lower end number of line-pairs per mm can be specified, and the computation of the frequency bands is done in a higher quality way. Importantly, this Fuse requires that your input image is a Float16 or Float32 type image, and it works best on a Log state image. I do not recommend using it with a Linear state image, and I would also recommend clamping the input to be non-negative. This Fuse works using several different discrete frequency bands rather than via a fourier transform, but it largely gives good looking results regardless.

#### Parameters
**Gate Width**: The mm width of your frame.

**Band 1-5 LP/mm**: This indicates where the frequency band should start for bands 1-5. Technically the resizing operation is imperfect, so if you're trying to match a chart, lower each value slightly. IE if you have a chart with 20 LP/mm on it, set the band to start early at about 5 LP/mm below the value you're trying to target.

**Band 1-5 Contrast**: This allows you to control the contrast level of each band on a scale from 0 to 2. 1.0 is neutral, and higher is more contrast.

**Debug Mode**: This pull-down allows you to figure out what each band targets. You can choose from None (runs the plugin normally), Low Pass Mode (shows you the information that's too low frequency to be captured in this band), High Pass Mode (shows the information that's in this band), and High Pass Gray Mode (Same as High Pass mode, but normalized to 0.5 so that the frequency data in this band is more visible).

**Debug Band**: Allows you to choose which band will be shown in the Debug mode.

**Performance Mode**: Quality does the right thing, but if you find that it's too slow, you can change this to Performance. Performance uses a lower quality computation for the low frequency information, and importantly it disables all bands except for the last two.

**Method**: Choose between Quotient and Difference. In most real-world scenarios, the Quotient method provides better looking results, but the Difference method performs more accurately on zebra striped test charts. Here's the math:
```
quotient_out := (input_value / low_pass5)^band5_contrast * (low_pass5 / low_pass4)^band4_contrast * ... * low_pass1
difference_out := (input_value - low_pass5)*band5_contrast + (low_pass5 - low_pass4)*band4_contrast + ... + low_pass1
```


---

### Periodic Frame Sampler Fuse
Samples the input image at the specified frame interval. Suppose the current frame time is `Destination Time Start`, and you want to be looking at the input image at frame `Source Time Start`, and you want the next frame outputted from this Fuse to be at `Source Time Start + Period * 1` and so on, then this is the Fuse for you.

Essentially, we will compute:

`(current_time - destination_time_start) * period + source_time_start` and return the frame at this time.

#### Parameters
**Source Time Start**: Frame number we should consider to be the "First Frame" of the input image.

**Destination Time Start**: Frame number at which we should output the `Source Time Start` frame.

**Period**: As time progresses one frame, this indicates how many frames later we should sample from the source image.




---

### Pixel Logger Fuse
Samples the specified pixel in the image, then prints out the frame number and rgb values to console.

#### Instructions
1. Place it somewhere in the render path of your pipeline (it could simply be "rendered" by being sent to the viewer)
2. Go to the start of the video file
3. Clear the console logs with the CLS button
4. Hit the Reload button at the top of the Fuse
5. Hit play or scrub through frames, whenever the frame number changes, the Fuse will print to the log.
6. Copy and paste the contents of the log to a CSV file or spreadsheet or something, maybe filter by tags later.

**Note**: In order to make this thing print out on every frame change, I needed the Process function to be called on every frame change and had to use a hack to disable caching. This means that this Fuse and everything downstream of it should be uncachable and this will negatively impact playback and render performance. You probably want to put this at the end of the pipeline.

#### Parameters
**Pixel Location X/Y**: The x/y coordinates of the pixel you want to sample from.

**Tags**: Any tags you want to add to the end of the line. I figure if you have multiple of these nodes in your pipeline, then you'll probably want to be able to distinguish between them by setting different tags.

**Log Header**: If you're on Frame 0 and you hit Reload when this is checked, it'll print out a`frame,red,green,blue[,tags]`, which would make it useful when copying and pasting your console into spreadsheet software via csv. Uncheck this if you don't want that bit to be printed out.



# The DCTLs:

## Effects


---

### Bleach Bypass DCTL
Applies a beach bypass look to the image. Expects a Linear image. Uses a custom Overlay implementation designed for Linear images.

#### DCTL Parameters
**Saturation**: Indicates how saturated or desaturated the result should be.

**Gamma**: Controls the contrast of the image.

**Middle Gray**: Indicates the middle gray value that will be preserved.



---

### Film Curve DCTL
Assumes the scene is a linear image, then converts to log10 exposure values, applies a sigmoid characteristic curve to get density, then computes transmittance. Parametric over each of the three channels.

In practice, you should use the following pipeline: `1. Clamp 0+ ==> 2. Film Curve (to simulate the negative) ==> 3. Color Gain ==> 4. Film Curve (to simulate the print) ==> 5. Gain (if you want white to not be 100%) ==> 6. Display Encoding`. (3) represents your printer lights and should make it so that middle gray is preserved at 0.18 from (1) to (4).

#### DCTL Parameters
**Red/Green/Blue Gamma**: Film gamma for each channel.

**Red/Green/Blue D_MIN**: Minimum density for each channel.

**Red/Green/Blue D_MAX**: Maximum density for each channel.

**Red/Green/Blue Offset**: indicates how far to the left to move each characteristic curve in the Density vs Log10 Exposure chart.

**Mid Gray**: Indicates the mid gray value in the input image.

**Exposure Gain**: Increase or decrease the incoming exposure.

**Linear to Exposure**: Check to apply the Exposure Gain to convert from scene illuminance to Exposure value

**Exposure to Log10 Exposure**: Check to apply a Log10 Function to the Exposure value

**Characteristic Curve**: Check to apply the Sigmoid that converts from Log10 Exposure to Density

**Density to Transmittance**: Check to convert computed Density to Transmittance.

**Draw Characteristic Curve**: Draws the characteristic curve on-screen. The fat line in the middle is Lux-Seconds = 0.0, each vertical line is 1.0 Log10 exposure. The horizontal lines represent density in 1.0 increments.

**Curve Type**: allows you to use Sigmoid of "Quadratic Sigmoid", which follows a similar shape but has a more abrupt rolloff.



---

### Film Grain DCTL
Creates a random noise, inspired by statistical film models. You'll need to pass in a linear image and use two of these DCTLs in a pipeline, one for the Neg stock and one for the print stock, as each one returns the Transmittance of the film stock.

#### DCTL Parameters
**D Max**: Maximum density

**D Min**: Minimum density of the film.

**Grains Per Pixel**: Should control the variance of the noise. More grains results in a finer image.

**Num Layers of Grains**: Should contribute towards controlling the gamma of the film. Essentially controls how thick the emulsion layer is.

**Activation Threshold**: Controls the amount of light needed for a grain to be activated.

**Photon Gain**: Exposure increase applied to the incoming light.

**Seed Position X/Y**: Indicates where in the image to pull a pixel to start the random seed. Change this if the noise is fixed.

**Noise Mode**: Indicates a different noise mode. In RGB, noise is computed on each channel independently, In Monochrome Noise mode, I recycle the same random seed for all three channels to avoid introducing chroma noise.




---

### Halation DCTL
DCTL that physically emulates film halation, intended for ACES Linear AP0 images. This is intended to be used in a linear gamma timeline.

#### How Halation works
Light passes through three layers of film emulsion and various color filters, ultimately with the bottom channel being Red. Light then passes through the film base and reflects off the back of the film, and this red light then re-exposes the channels in reverse order (red, then green, then blue).

#### DCTL Parameters
**Focal Length** (mm): The focal length of the lens used

**Film Base Thickness** (mm): The thickness of the film (most films are between 0.12 and 0.20 mm thick). This is used in conjunction with the focal length to determine how much larger the reflected image is than the original image. Set the film thickness to zero to remove any scaling.

**Reflection exposure lost** (stops): As light passes through the film base and reflects off the anti-halation layer, it loses brightness. This parameter controls how many stops of light are lost by the time the reflection reaches the red channel again on the rebound.

**Green exposure lost** (stops): Controls how much light is lost when the reflected light passes through the red channel and then exposes the green channel. This is added to the Reflection exposure lost.

**Blue exposure lost** (stops): Controls how much light is lost when the reflected light passes through the green channel and then exposes the blue channel. This is added to the Green exposure lost and the Reflection exposure lost.

**Blur Amount** (Thousandths of image width): The light reflection is blurry by virtue of being out of focus and by being diffused by the film base and anti-halation layer. This control represents the width of the applied blur.

**Halation Gamma**: The halation color is first raised to this power before being added to the image. This allows for creative control over the effect of halation on the tonal range of the image, though it's not physically motivated.

**Show only halation**: Enabling this checkbox shows what is added (IE arithmetic addition) to the image.

**Correct for Red shift**: Because Halation re-exposes the red channel first, it will make the exposed negative more red than it originally was. Checking this box applies a correction that neutralizes this red shift so the halation effect will only have an effect at the edges. As a colorist would neutralize an image with a red balance, checking this box applies that correction in-line.

**Blur Type** [NONE, BOX BLUR, TRIANGLE BLUR, FAST BLUR]: Selects the blurring method. None will not apply any blur to the reflected image, Box Blur applies a square convolution of uniform weights, Triangle blur applies a center-weighted blurring with linear falloff, and Fast Blur is essentially a box blur that samples 9 points along the center and perimeter of a square, resulting in great runtime. Box and Triangle blur will run slower with larger blur amounts.




---

### Lens Distortion DCTL
Applies a basic lens distortion model, uses bilinear sampling to avoid aliasing problems for reasonable choices of $k_1$ and $k_2$.

#### How it works
Lens distortion here is modelled with:
$r_d = \sqrt{x_d^2 + y_d^2}$
$x_u = x_d (1 + k_{1x} r_d^2 + k_{2x} r_d^4)$
$y_u = y_d (1 + k_{1y} r_d^2 + k_{2y} r_d^4)$

#### DCTL Parameters
**X Distortion Amount K1**: Chooses the value of $k_{1x}$ in the above model

**Y Distortion Amount K1**: Chooses the value of $k_{1y}$ in the above model

**X Distortion Amount K2**: Chooses the value of $k_{2x}$ in the above model

**Y Distortion Amount K2**: Chooses the value of $k_{2y}$ in the above model

**Couple XY**: If checked, just use the same $k_{1x}, k_{2x}$ for both $x_u$ and $y_u$ calculations, keeping the lens distortion spherical.

---
### Linear Contrast DCTL
Applies a power function to the RGB channels, keeping 0.18 unchanged. This DCTL expects a scene linear image.

#### How it works
The DCTL works in three steps:
1. Apply gain to shift Middle gray to 1.0
2. Raise the code values to the power of `Neutral Gamma * Color Gamma` if Ungroup RGB is checked, otherwise `Neutral Gamma`
3. Revert the gain done in step 1 (divide by that scaling rather than multiply)

#### DCTL Parameters
**Neutral Gamma**: Gamma applied to all channels

**Red Gamma**: Gamma to be applied only to the Red Channel

**Green Gamma**: Gamma to be applied only to the Green Channel

**Blue Gamma**: Gamma to be applied only to the Blue Channel

**Mid Gray**: Specifies the middle gray code value that is preserved.


---

### MTF Curve DCTL
Gives you control over a MTF-like curve. Internally makes passes of different frequencies which can be increased or reduced in gain before combining them back together. Highly recommend using the Quotient method and feeding this DCTL a log image.

#### DCTL Parameters
**Band 16-1:1 Contrast**: Applies a gain to the information captured only by this band. Set to 0 to soften the image and raise up to 2 to increase sharpness. The bands are relative to the timeline resolution, with a 16:1 blur, 8:1 blur, 4:1 blur, and 2:1 blur.

**Debug Band**: Specifies which band is viewed when the Debug Mode isn't None.

**Debug Mode**: This pull-down allows you to figure out what each band targets. You can choose from None (runs the plugin normally), Low Pass Mode (shows you the information that's too low frequency to be captured in this band), High Pass Mode (shows the information that's in this band), and High Pass Gray Mode (Same as High Pass mode, but normalized to 0.5 so that the frequency data in this band is more visible).

**Method**: Allows you to choose between Quotient and Difference, which correspond to different ways to compute the frequency bands. In most real-world scenarios, the Quotient method provides better looking results, but the Difference method performs more accurately on zebra striped test charts. Here's the math:
```
quotient_out := (input_value / low_pass5)^band5_contrast * (low_pass5 / low_pass4)^band4_contrast * ... * low_pass1
difference_out := (input_value - low_pass5)*band5_contrast + (low_pass5 - low_pass4)*band4_contrast + ... + low_pass1
```




---

### Photon Noise DCTL
Helps simulate the effect of photon noise, a noise that's approximately poisson distributed, where the variance is proportional to the intensity of the signal. Apply this to a linear image.

#### DCTL Parameters
**Photon Exposure** (stops): The input signal is multiplied by `_exp2f(photon exposure)`to compute the variance

**Noise Mode**: Indicates a different noise mode. In RGB, noise is computed on each channel independently, In Monochrome Noise mode, I recycle the same random seed for all three channels to avoid introducing chroma noise.

**Seed Position X/Y**: Coordinate of the pixel used to generate a random seed.




---

### Random Channel Mixer
Constructs a random RGB matrix that is some distance away from the Identity matrix. Useful when trying out lots of different looks, expects image to be converted to Linear before using.

#### DCTL Parameters
**Eps**: Maximum acceptable entry-delta from the identity matrix. Essentially controls the intensity of the applied effect. In some cases, a value about 0.33 will result in division by zero errors and numerical instability when rows are rescaled.

**Seed**: Indicates the random seed used to construct the matrix. Handy if you want to remember the value for later and reproduce a certain look.

**Maintain White**: If checked, scales each row of the RGB matrix to each sum to 1. This helps keep grays neutral in the final result. If unchecked, the image can take on a tint, with luminance maintained by scaling the entire matrix to sum to 3 (but individual rows can sum to values other than 1).

**Show Matrix** [OFF, FLOAT VALUE, SCALED TEN BIT VALUE]: If set to "Float Value", the entries of the matrix are displayed, allowing you to copy the values down using the RGB picker in Fusion. If set to "Scaled Ten Bit Value", the entries of the matrix are displayed, and if you have a 10-bit color picker, the difference between the code value and 500 represents the hundreths of a point that you should enter in the corresponding entry in the RGB Mixer. IE if the top middle patch has a 10-bit code value of `496`, you would enter `-0.04` for the Green channel in the Red Output section of the RGB Mixer.




---

### Random Contrast Curve
Constructs a contrast curve, has the option to procedurally generate one with random parameters so you can try lots of different curves in a moment.

#### DCTL Parameters
**Pivot**: Specify the pivot point, which will remain unchanged in color and value.

**Toe**: Indicates what distance from the pivot to start rolling off the shadows. 0.0 is at the pivot point, 1.0 is near the black point.

**Shoulder**: Indicates what distance from the pivot to start rolling off the highlights. 0.0 is at the pivot point, 1.0 is near the white point.

**Black Point**: Specifies the minimum value in the image.

**White Point**: Specifies the maximum value in the image.

**Pivot Slope**: Specifies the slope of the curve when measured at the pivot point.

**Seed**: Random seed, only used if Randomize box is checked.

**Show Curve**: Check this box to get a graphical display of the curve. Within the gridded area, the x-axis goes from 0.0 to 1.0, negative values are shown to the left and right of the display.

**Randomize**: Check this box to randomly select parameters for the curve (excluding pivot).

**Ungroup RGB**: When checked, if Randomize is also checked, then this will randomize a contrast curve for each of the RGB channels.



---

### Random Linear Contrast
Expects a linear image, applies linear contrast to each channel via gamma, preserving mid gray, but you can use a random number generator so you can try a lot of different curves in only moments.

#### DCTL Parameters
**Global Contrast**: Initial gamma applied to all channels.

**Red/Green/Blue Contrast**: Gamma added to Global gamma for the corresponding channel.

**Random Interval**: Maximum amount that a randomly selected gamma can deviate from a per-channel gamma or the global gamma

**Random Seed**: User selected seed for the random number generator.

**Mid Gray**: Linear value of mid gray.

**Use Random**: Choose whether to not use the random number generator, to randomly generate a split tone (randomness is applied only to the per-channel contrasts), to randomly augment the Global Contrast, or both.




---

### RGB Linear Contrast DCTL

**(Deprecated - Use "Linear Contrast DCTL" instead.)**

You can find the old version of this DCTL [here](https://github.com/thatcherfreeman/utility-dctls/blob/167d849241ddd1e6dbd3963e2be3601694173a38/Effects/RGB%20Linear%20Contrast.dctl)

Applies a power function to the RGB channels, keeping 0.18 unchanged. This DCTL expects a scene linear image.

#### How it works
The DCTL works in three steps:
1. Apply gain to shift Middle gray to 1.0
2. Raise the code values to the power of `Neutral Gamma * Color Gamma` if Ungroup RGB is checked, otherwise `Neutral Gamma`
3. Revert the gain done in step 1 (divide by that scaling rather than multiply)

#### DCTL Parameters
**Red Gamma**: Gamma to be applied only to the Red Channel (if Ungroup RGB is checked)

**Green Gamma**: Gamma to be applied only to the Green Channel (if Ungroup RGB is checked)

**Blue Gamma**: Gamma to be applied only to the Blue Channel (if Ungroup RGB is checked)

**Neutral Gamma**: Gamma applied to all channels, regardless of Ungroup RGB

**Mid Gray**: Specifies the middle gray code value.

**Ungroup RGB**: If unchecked, only applies the Neutral Gamma, otherwise applies both Neutral gamma and the Color Gamma, multiplying together those two powers.




---

### Subtractive Saturation DCTL
Computes saturation in a way that adds "density" to more saturated colors, making them darker. Expects a Linear image.

#### How it works
Suppose a pixel is a color `input`. The DCTL will first compute a `Value` (IE luminance) of that pixel using one of many methods, and from there it can compute the input's `Color` by taking `input / Value`. The color is saturated or desaturated using the Gamma controls (we raise each channel of the `Color` to a power). From there, we multiply the `Color` by a different luminance called `Density` that's calculated using the method specified by the second dropdown menu. The result is scaled so that white is preserved.

#### DCTL Parameters
**Color Gamma**: Controls the saturation of the image.

**Cyan Gamma**: Saturation slider that's combined with the Color Gamma slider, moving this to the right will make the image more cyan.

**Magenta Gamma**: Similar to Cyan Gamma slider.

**Yellow Gamma**: Similar to Cyan Gamma slider.

**Density**: Allows you to control how much density is added, on a scale of 0 (`Density` is computed using the same method as `Value`), to 1 (`Density` is computed using the specified method). The floating scale effectively lets you choose the strength of the specified method.

**Value Calculation**: Allows you to select how the `Value` is computed. I don't recommend the Max or Min methods, and you should generally choose a method that runs large (Arithmetic Mean, Geometric Mean, and L2 Norm are recommended).

**Density Calculation**: Allows you to choose how the `Density` is computed. Again, I don't recommend the Max or Min methods, and you should choose a method that runs small (Harmonic Mean is recommended.)




---

### Tone Mapping DCTL
Applies a sigmoid function to compress highlights. Attempts to target a specific black point, white point, mid_gray, and slope at mid gray. Expects a linear state image and outputs a linear state image.

#### How it works
The DCTL computes the $h(x)$, where: $h(x) = g(m_i(x/m_i)^\gamma)$ and $g(x) = a\frac{x}{x+b} + c$. The parameters $a, b, c, \gamma$ are selected for you based on the specified white point, black point, and target slope, and input/output mid gray points.

#### DCTL Parameters
**Target Slope**: Linear slope at the output mid gray. $\gamma$ is selected so that $h'(m) = \text{target slope}$.

**White Point**: Maximum output, typically 1.0 corresponds to 100 nits, and $h(x)$ will asymptotically approach this value as $x \rightarrow \infty$.

**Black Point**: This indicates where $h(0)$ will map to. If your linear image has negative code values, they can map to be below this black point.

**Input Mid Gray**: Indicate the value corresponding to $m_i$.

**Output Mid Gray**: Specifies the value corresponding to $m_o$. We choose gamma such that $h(m_i) = m_o$.

**Scale Mid Gray with White Point**: If checked, the value of $m_o$ is overridden to `output_mid_gray * (white_point - black_point) + black_point`. If unchecked, $m_o$ is just set equal to `output_mid_gray`. If you want your mid gray to scale with the max output white point as you change white points, then check this box.



---

### Vignette DCTL
Corrects for a vignette in the image, only handles circular vignettes for now, expects a scene linear image.

#### DCTL Parameters
**Vignette Amount**: Uses a model of `1 + ar^2` to determine the amount of vignetting, then multiplies to vignette the image. Vignette amount controls the value of `a`.

**Show Vignette**: If checked, outputs the image that is multiplied by the source image.




## Operations



---

### Addition Function DCTL
Adds a value to each channel. The channels are computed by $\text{Red}_{\text{out}} = \text{Red}_{\text{in}} + \text{Global Offset} + \text{Red Offset}$ and likewise for the other two channels.




---

### Clamp DCTL
Clamps the code values of the current frame to the specified Min and Max values, such that for any `x`, we will then have `clamp_min <= x <= clamp_max`

#### DCTL Parameters
**Min Clamp**: Specifies the value at which we will set `x = max(x, Min Clamp)`

**Max Clamp**: Specifies the value at which we will set `x = min(x, Max Clamp)`

**Clamp Min (Checkbox)**: Uncheck to bypass the Min Clamping step.

**Clamp Max (Checkbox)**: Uncheck to bypass the Max Clamping step.




---

### Color Generator DCTL
Generates the specified RGB value across the whole frame. Also allows you to bypass certain channels via the "Pass-through" checkboxes.

#### DCTL Parameters
**Red/Green/Blue**: the Red/Green/Blue value that will be returned.

**Red/Green/Blue Pass-Through**: If checked, just return the red/green/blue value of the input image.



---

### Cross Product DCTL
Computes the cross product of the current color and the specified vector, or the cross product of the specified vector and the current color if you set the direction to $\overrightarrow{Vec} \times \overrightarrow{\text{Input RGB}}$.


---

### Dot Product DCTL
Takes the dot product of the current color and the specified `(r, g, b)` value.



---

### Gamma Function DCTL
Applies a power function with the reciprocal of the specified exponent.

#### DCTL Parameters
**Gamma**: Given some number $\gamma$, raise each of the RGB components to the power of $\gamma$

**Use Reciprocal**: If checked, instead raises each RGB component to the power of $1 / \gamma$.

**Negative Values**: For a negative input color component $x$, choose from:
* Clip 0 - returns $y = 0$
* y=x - Returns $y = x$
* y=x/gamma - Returns $y = x / \gamma$ so the slope somewhat scales according to the choice of exponent.
* Positive Reflection - Returns $y = \lvert x \rvert^\gamma$
* Sign Match Reflection - Returns $y = -\lvert x \rvert^\gamma$, returning a negative result if $x$ is negative.




---

### Invert DCTL
Inverts the values in an image.

#### DCTL Parameters
**Log Mode**: When checked, computes the inverse by taking `1 - x`. When unchecked, assumes the image is scene linear and therefore computes `1 / x`.





---

### Log Function
For each pixel and channel, takes the logarithm.

#### DCTL Parameters
**Log Base**: The base of the logarithm, 10.0 by default.




---

### Matrix
Multiplies the RGB values of the input by a 3x3 matrix with the specified entries. Supports negative values. Given your input $x = [r, g, b]^T$, this computes $f(x) = Ax$. If you choose to preserve neutrals, then we will ensure that the rows of $A$ sum to 1.0, so please make sure the matrix doesn't have rows of zeros if you intend to use that feature.

#### DCTL Parameters
**Red/Green/Blue => Red/Green/Blue**: Indicates the coefficient corresponding to how much of the left hand side (input) channel will be included in the right hand side channel (output). The entries ending with "=> Red" are the first row of the matrix, the entries ending in "=> Green" are the second row, etc.

**Preserve Neutral**: Sends $(1, 1, 1)$ through the matrix and applies RGB gain to the output to ensure that $(1, 1, 1)$ is ultimately returned.



---

### Modulo Function DCTL
Takes the input $x$ and computes $x \mod y$, the remainder when dividing $x$ by $y$.

#### DCTL Parameters
**Global Denominator**: If Use Per-Channel Denominator is unchecked, then the modulo of all three channels is computed with respect to this number.

**Red/Green/Blue Denominator**: If Use Per-Channel Denominator is checked, then each channel's modulo will be computed with respect to the corresponding number.

**Use Per-Channel Denominator**: Check in order to compute modulo with a different denominator for each channel.




---

### Multiplication Function DCTL
Multiplies each channel by a value. The channels are computed by $\text{Red}_{\text{out}} = \text{Red}_{\text{in}} * \text{Global Gain} + \text{Red Gain}$ and likewise for the other two channels.



---

### Polynomial Kernel DCTL
For each of $x_i \in \{r, g, b\}$, computes $(x_i \cdot x_j)^p$ and allows you to specify a linear combination of those into each of the r, g, b channels

#### How it works
One of the tricks with SVMs is the Polynomial kernel, where you extend your feature vector with $k(x_i, x_j) = (x_i * x_j)^p$ for some integer $p$, and for all $x_i$ or $x_j$ in your original input feature vector. This results in a higher dimensional input (in this case, 9 unique dimensions) where the dimensions are as follows: $r, g, b, k(r, r), k(g, g), k(b, b), k(r, g), k(r, b), k(g, b)$

Now, we can convert back to 3 dimensions by multiplying by a $3 \times 9$ matrix, which you specify with the parameters. I've intentionally actually skipped most of the first three columns as you can figure those out yourself with a normal 3x3 matrix prior to this DCTL, and that keeps it way cleaner.

There's an interesting special case when $p = 0.5$ and you're therefore taking the geometric mean of each pair of channels, and effectively get a 3x3 matrix in here too.

#### DCTL Parameters
**Red/Green/Blue => Red/Green/Blue**: The coefficient corresponding to the original color.

**Red/Green/Blue * Red/Green/Blue => Red/Green/Blue**: The coefficient corresponding to this $(x_i \cdot x_j)^p$ term.

**Power**: The value of $p$.

**Mid Gray**: Indicates the code value for Mid Gray, that will be restored via RGB gain if Preserve Gray is checked.

**Identity Point for Products**: By default, $x^p$ obviously is an identity function only when $x = 1$ (or $x = 0$). This allows you to choose a different stationary point, as we will scale the input and output of the power such that $x$ remains stationary for this specified value when Normalize Powers is checked.

**Preserve Gray**: If checked, runs mid-gray through the pipeline and applies gain at the end to restore it.

**Normalize Powers**: If checked, powers will be normalized at the value specified by Identity Point for Products.



---

### Power Function DCTL
Computes the function $\texttt{base}^x$.

#### DCTL Parameters
**Base** The base of the exponent, raised to the power of the input pixel.


---

### Projective Transformation Matrix DCTL
Rather than applying a 3x3 matrix, we can apply a Projective Transform.

#### How it works
Traditionally, you'd make a matrix $M_1 \in \mathbb{R}^{3 \times 3}$ and apply the matrix multiply:
```math
\begin{bmatrix} r' \\ g' \\ b'\end{bmatrix} = M_1 \begin{bmatrix} r \\ g \\ b \end{bmatrix}
```

Frequently used in computer graphics, there's a slightly more expressive alternative called Projective Transforms that make use of homogeneous coordinates. You would instead have $M_2 \in \mathbb{R}^{4 \times 4}$ and make the following modifications:

```math
\begin{bmatrix} r' \\ g' \\ b' \\ c' \end{bmatrix} = M_2 \begin{bmatrix} r \\ g \\ b \\ 1.0 \end{bmatrix}
```

You'd then output the color:

```math
\begin{bmatrix} r'/c' \\ g'/c' \\ b'/c' \end{bmatrix}
```

You can see that this is strictly more expressive than the $3 \times 3$ matrix approach because you could define $M_2$ in the following way:

```math
M_2 = \begin{bmatrix} M & 0 \\ 0 & 1 \end{bmatrix}
```

This DCTL allows you to construct $M_2$.

#### DCTL Parameters

**Red/Green/Blue/Bias => Red/Green/Blue/Bias**: These are listed in the same order as if you were reading $M_2$ row by row. Almost all the interesting behavior happens in the fourth row.

**Preserve Neutral**: If checked, applies an RGB gain operation at the end that restores white inputs to (1.0, 1.0, 1.0).

**Linearize White**: If checked, applies a curve to make the grayscale ramp a straight line.

---

### Sigmoid Function DCTL
Applies the sigmoid function to the inputs. Computes $b + (w-b)\frac{1}{1 + e^{-c(x-d)}}$.

#### DCTL Parameters
**X Midpoint**: Controls the value of $d$. Vanilla sigmoid has this set to 0.0.

**Contrast**: Controls the value of $c$. Vanilla sigmoid has this set to 1.0.

**Output White**: Controls the value of $w$. Vanilla sigmoid has this set to 1.0.

**Output Black**: Controls the value of $b$. Vanilla sigmoid has this set to 1.0.



---

### Sigmoid Kernel DCTL
Similar to Polynomial Kernele. For each of $x_i \in \{r, g, b\}$, computes $\sigma((x_i \cdot x_j)^p)$ where $\sigma(x) = \frac{x}{x + w}$, with $w$ being a user specified white point. Allows you to specify a linear combination of those into each of the r, g, b channels. You should use a linear input for this function.

#### How it works
One of the tricks with SVMs is the Polynomial kernel, where you extend your feature vector with $k(x_i, x_j) = \sigma((x_i * x_j)^p)$ for some integer $p$, and for all $x_i$ or $x_j$ in your original input feature vector. This results in a higher dimensional input (in this case, 9 unique dimensions) where the dimensions are as follows: $r, g, b, k(r, r), k(g, g), k(b, b), k(r, g), k(r, b), k(g, b)$

Now, we can convert back to 3 dimensions by multiplying by a $3 \times 9$ matrix, which you specify with the parameters. I've intentionally actually skipped most of the first three columns as you can figure those out yourself with a normal 3x3 matrix prior to this DCTL, and that keeps it way cleaner.

#### DCTL Parameters
**Red/Green/Blue => Red/Green/Blue**: The coefficient corresponding to the original color.

**Red/Green/Blue * Red/Green/Blue => Red/Green/Blue**: The coefficient corresponding to this $\sigma((x_i \cdot x_j)^p)$ term.

**Power**: The value of $p$.

**Mid Gray**: Indicates the code value for Mid Gray, that will be restored via RGB gain if Preserve Gray is checked.

**White Point**: The value of $w$ in the denominator of $\sigma(x)$.

**Preserve Gray**: If checked, runs mid-gray through the pipeline and applies gain at the end to restore it.




---

### Softmax DCTL
Applies a Softmax function, with Temperature. Outputs in the 0-1 range for all real inputs.

#### DCTL Parameters
**Temperature**: Scales the input values, so larger values will result in a more extreme output.



---

### Tanh Function DCTL
Computes a tanh of the input via $g \tanh(c (x - b))$.

#### DCTL Parameters
**Horizontal Offset**: Controls the value of $b$. Vanilla tanh has this set to 0.0.

**Contrast**: Controls the value of $c$. Vanilla tanh has this set to 1.0.

**Output White**: Controls the value of $g$. Vanilla tanh has this set to 1.0.

**Maintain Contrast**: If checked, then the Contrast control controls the derivative at x=0 even if the output white is changed (IE $c$ is scaled by $1/g$).



---

### Unit Length DCTL
Takes the current `(r, g, b)` color value, computes the L2 norm, and divides each component by the norm to convert the vector to unit length.


---

### Vector Norm DCTL
Computes various norms of the given vector.

#### DCTL Parameters
**Norm Type**: Choose between `L1 Norm, L2 Norm, Lp Norm, Maximum, Minimum, Arithmetic Mean, Geometric Mean, Harmonic Mean` to choose the norm type.

**P-Norm Power**: If `Lp Norm` is selected for the norm type, this selects the value of `p`. Lp norm is computed by $(\lvert r \rvert^p + \lvert g \rvert^p + \lvert b \rvert^p)^{1/p}$, and L1 and L2 norm are special cases of this.



## Utilities



---

### ACES Exposure DCTL
DCTL that allows for adjustment of exposure in ACES. Important: It's probably better to just set your timeline color space to the ACES color space you want to use, and then to use the Exposure slider in the HDR color wheels.

#### How it works
Internally, this DCTL converts ACEScc or ACEScct to Linear, and then applies a gain according to the specified exposure adjustment before converting the image back into the original color space.

#### DCTL Parameters
**ACES Gamma**: Pulldown menu in which you select from ACES (Linear), ACEScc, and ACEScct. This is where you specify the gamma of the image that is being fed into this DCTL.

**Exposure Adjustment**: Specifies the number of stops to increase or decrease (negative) exposure.



---

### Bit Depth Estimator DCTL
Tool to help estimate the true bit depth of a file. It works by comparing the code values in the current pixel to the adjacent pixels and measuring the smallest nonzero difference between the corresponding channels to estimate the effective bit depth for the current pixel.

#### DCTL Parameters
**Target Bit Depth**: When Highlight is enabled, Highlights all pixels whose effective bit depth is within 0.1 of this Target Bit Depth.

**Highlight**: When checked, highlights only pixels whose bit depth is near the Target Bit Depth, otherwise all pixels are replaced with their effective bit depth.




---

### Blanking Checker DCTL
Helps you spot pixels with NaN, infinity, negative, zero, or superwhite channels. Pixels with certain conditions are replaced by a specified highlight color. Optionally, the highlight can be a checkerboard shape.

#### DCTL Parameters
**Highlight Color Red**: Red component of the highlight color.

**Highlight Color Green**: Green component of the highlight color.

**Highlight Color Blue**: Blue component of the highlight color.

**Checkerboard Size**: Square size of the generated checkerboard, if set to zero, just uses the Highlight Color.

**Highlight NaNs**: Highlights pixels with a NaN channel.

**Highlight -Inf**: Highlights pixels with -infinity as at least one channel

**Highlight < 0.0**: Highlights pixels with negative real values.

**Highlight == 0.0**: Highlights pixels that have a channel equal to zero.

**Highlight == 1.0**: Highlights pixels that have a channel equal to 1.0.

**Highlight > 1.0**: Highlights pixels that have a channel that exceeds 1.0.

**Highlight +Inf**: Highlights pixels with +infinity as at least one channel.




---

### Channel Viewer DCTL
Emulates the Fusion channel viewer, for Red/Green/Blue channels.

#### DCTL Parameters
**Channel**: Allows you to choose whether the full color image will be returned, or only one of the red/green/blue channels (duplicated onto all three channels in the output for visibility).



---

### Chroma Subsampling DCTL
Applies chroma subsampling to an image by converting to YCbCr, downsampling the Cb and Cr channels via box averaging, then converting back to RGB.

#### DCTL Parameters
**X/Y Offset**: Allows you to offset the 2x4 filter box.

**Convert to YCbCr**: Check this box to have the DCTL bookend itself with a conversion from RGB Rec709 gamut to Y'CbCr. If unchecked, that means you're doing your own conversion before and after the DCTL and this will downsample channels 2 and 3.

**Chroma Subsampling Type**: Allows you to choose which kind of chroma subsampling to use.


---
### CIELUV DCTL
Converts between XYZ and CIELUV, CIELCH, or CIELSH.

#### DCTL Parameters
**White Point x,y**: Indicates the xy chromaticity coordinates of the white point. D65 by default.

**White Point Luminance**: Indicates nits of luminance for the white point.


---

### ColorChecker DCTL
Generates a synthetic image of the Macbeth ColorChecker based on one of two datasets: the X-Rite post-2014 measured LAB values, or the original paper (you can find in Documentation/ColorChecker.pdf). Outputs in XYZ/Linear color space.

#### DCTL Parameters
**Exposure Adjustment**: Stops of exposure adjustment, in case the rendered image isn't your preferred brightness.

**Outer Border Width**: How much black border to draw around the whole image.

**Inner Border Width**: How much black border to draw around individual chips.

**Dataset**: Choose Official to use the X-Rite values, and use McCamy to use the one from the old paper and wikipedia.

**Adapt to D65**: Check this box to use a bradford chromatic adaptation matrix to best approximate what the chart would look like under a D65 illuminant. Leave unchecked to use the native white point for each dataset (Illuminant A for official, Illuminant C for McCamy).


---

### Color Ramp DCTL
Creates a color ramp from 0 to 100% Hue, Saturation, or Luminance. This can be used to monitor the output of your tools and overall node pipeline.

#### DCTL Parameters
**Ramp Type**: Choose from Luminance, Saturation, or Hue ramp.

**Saturation Ramp Hue**: If Saturation ramp is selected, then this controls the hue of the ramp.

**Hue Ramp Saturation**: If Hue ramp is selected, then this controls the saturation of the hue ramp.



---

### Cube Rotate DCTL
Takes the specified vector and rotates the RGB cube (around 0,0,0) so that the given vector is now achromatic.

#### DCTL Parameters
**Color R, Color G, Color B**: The RGB components of the vector that will be rotated gray.

**Inverse**: Rotates the cube the opposite angle, so that the currently white vector rotates to the direction of the specified vector, therefore doing the opposite of the normal version.



---

### DaVinci Tone Mapping DCTL
Recreation of the DaVinci Tone Mapping setting in the Color Space Transform effect. Expects a linear image and outputs a linear image. Currently only replicates the tone mapping when Adaptation is set to 9.

#### How it works
DaVinci Tone Mapping is simply a function of $f(x) = a \frac{x}{x + b}$, with some strategically selected $a$ and $b$ depending on your selected parameters. I've solved both $a$ and $b$ when Adaptation is set to 9.

#### DCTL Parameters
**Max Input Nits**: Quantity of nits that will be mapped to the Max Output Nits. Resolve is scaled so that linear 1.0 means 100 nits.

**Max Output Nits**: Max Linear value that will be in the output image.

**Adaptation**: Makes the image brighter or darker.

**User Input B**: When "Use Custom Adaptation" is checked, we use this value of $b$ instead of the one computed by default when Adaptation is 9. You'll have to use this if you want to emulate DaVinci Tone Mapping in the scenario that Adaptation is not equal to 9.

**Use Custom Adaptation**: Allows you to override the selection of $b$ when checked. Otherwise, $b$ is computed from the specified value of Adaptation.

**Invert**: Inverts the tone mapping with the specified parameters.

**Clamp**: Allows you to disable white point clamping. Resolve would have this permanently enabled.



---

### Exposure Chart DCTL
Creates a middle gray exposure chart, an exponential ramp, a linear ramp, and several gray exposure chips that are an integer number of stops above and below middle gray. This is intended to be used in a linear gamma timeline.

#### DCTL Parameters
**Number of Steps**: Specifies the number of exposure chips to be displayed in the chart. One of the ones in the middle will share its value with middle gray, and each chip to the right will have a code value double of the previous chip.

**Middle Gray Value**: Specifies the desired value of middle gray, which is 18% by default. This controls the brightness of the large chip in the middle too.




---

### False Color Generator DCTL
Generates a false color conversion for linear images. Allows you to assign colors to specific regions of the image, in one-stop increments. You set a black point, a shadow point, mid gray, a highlight point, and a white point, and you can assign colors to all regions between and outside of those bounds.

#### How to use
##### Option 1:
1. Load in a clip from your camera
2. Apply a CST converting from your camera's color space to Linear
3. Apply this DCTL after the CST
4. Set the White Cutoff to capture the white point of your camera.
5. Set the shadow/highlight stops to the points you feel appropriate
6. Set the Black cutoff where the black point is in your clip.
7. Set the Brightness Mode to Value to use the max of the three channels as the input to the false color.
8. Convert this pipeline into a LUT. "Generate LUT" actually won't work, so you'll have to use an alternative method. Kaur Hendrikson has a great [video](https://www.youtube.com/watch?v=EAHzZH_tdHQ) on how to do this.

##### Option 2:
1. Load in a clip from your camera and go to the Fusion page.
2. Create a LUTCubeCreator node
3. Pipe that node into a CST converting from your camera's color space to Linear
4. Pipe the CST into this DCTL
5. Set the parameters using the same approach from Option 1.
6. Pipe the DCTL into a LutCubeAnalyzer node and choose CUBE as the type and save it somewhere.

#### DCTL Parameters
**Black Hue Angle**: Hue of colors below the Black Cutoff

**Near Black Hue Angle**: Hue of colors between Black Cutoff and Shadow Stop

**Shadow Hue Angle**: Hue of colors between shadow stop and mid gray

**Highlight Hue Angle**: Hue of colors between mid gray and Highlight Stop

**Near white Hue Angle**: Hue of colors between Highlight Stop and White Cutoff

**Clipped White Hue Angle**: Hue of colors brighter than the White Cutoff point

**Black Cutoff**: Number of stops below middle gray below which we'll consider Black.

**Shadow Stop**: Number of stops below middle gray we'll color with the Shadow Hue Angle

**Highlight Stop**: Number of stops above middle gray we'll color the Highlight Hue Angle

**White Cutoff**: Number of stops above middle gray after which we'll consider clipped.

**Middle Gray Value**: Indicates the Linear value we consider to be middle gray.

**Log Output**: Check this box if this DCTL isn't going into another CST that converts to a display or log color space.

**Brightness Mode**: If Luminance, then we simply take a weighted average of the RGB channels. If Value, then we take the channel with the maximum value before comparing it to any of the cutoffs or mid gray.




---

### Gradient Smoothness Chart DCTL
Generates a test chart with a series of linear gradients. Each gradient is a full linear interpolation between two colors. For each band drawn, the value of the start and end color is changed. The premise of this DCTL is that you would place it towards the head of your node graph and then look at the 3D Histogram towards the end of the graph. If these straight lines remain smooth, then I would assume your series of operations is going to look reasonably good on a lot of images. If the straight lines have discontinuities or sharp angles (particularly near the achromatic axis), you'll likely have breakage in certain images.

#### DCTL Parameters
**Base Hue**: Hue angle of the left edge of the gradients.

**Angle Between Patches**: Angle added to Base Hue to compute the hue of the right edge of the gradient.

**Saturation**: Saturation of left and right edges of the gradient. Computed in HSV.

**Number of Bands**: Number of gradients to draw on the screen.

**Mid Gray**: If Band Interval is set to Exponential, this represents the target value for the middle band.

**Min/Max Clamp**: If Clamp Output is checked, then colors will be replaced with white if any channel goes outside the range of [Min Clamp, Max Clamp]

**Continuous**: If checked, fills in the plane in the rgb cube between the bands.

**Clamp Output**: If checked, replaces values > 1 with pure white so that your image doesn't have any values that go outside the unit cube.

**Band Interval**: If set to Equal, then each band will be a constant code value apart from the previous band. If set to Exponential, each band will be 2x the previous band. For pipelines expecting a log or gamma encoded image, use Equal, and for pipelines expecting a linear image, use Exponential (and probably turn off output clamping).







---

### Levels Converter
Converts between full and (0-1023) legal levels (64-940)

#### DCTL Parameters
**Mode**: Indicates whether to convert to Legal or to Full levels (the direction of the transform)

**Clip**: Indicates whether to clip extreme values. If Mode is set to `Full to Legal`, then Clip will clip values outside of the range 64-940. Otherwise, clip will clip values outside of the range 0-1023.



---

### Log Curve
Applies the Linear To Log function or Log To Linear function generated by [this repo](https://github.com/thatcherfreeman/log2lin-finder)

#### DCTL Parameters
**Base, Offset, Scale, Slope, Intercept, Cut**: These are plugged into the functions:
```
def log2linear(x):
    if (t > cut):
        return scale * pow(base, x) + offset
    else:
        return slope * x + intercept

def linear2log(y):
    if (y > slope * cut + intercept):
        return log((y - offset) / scale) / log(base)
    else:
        return (y - intercept) / slope
```

**Linear Gain**: This applies an exposure correction to the resulting image. If you're running Log2Lin, then the linear image is multiplied by this value. If you're running Lin2Log, then the linear image is divided by this value.

**Direction**: This allows you to specify whether to convert Log to Linear, or Linear to Log.



---

### Luminance
Given a color gamut, compute the luminance channel associated with those primaries (IE the Y channel after converting to CIE XYZ). Expects the image to be in a Linear state.

#### DCTL Parameters
**Color Gamut**: Select a pre-configured color gamut, or choose Custom. If Custom is selected, then the _x,y_ parameters are used to specify the gamut.

**Red/Green/Blue/White x/y**: CIEXYZ _xy_ chromaticity coordinates of the four primary values.

**Show Luminance Vector**: If checked, just outputs the RGB weights of the generated luminance vector, you can take the dot product of this color and your input color to compute the luminance.



---

### Output Blanking DCTL
Draws black bars on the top/bottom or left/right sides of the frame to mask out all but a specified aspect ratio

#### DCTL Parameters
**Aspect Ratio** The aspect ratio you want to keep after pillar/letter boxing.


---
### Printer Lights
Photometric printer lights function. This DCTL expects a scene linear image and outputs a scene linear image. Applied gain for one or more channels is computed by $10^{\gamma \cdot 0.025(x - 25)}$, where $x$ is the aggregate printer light ($\text{exposure} + \text{rgbchannel} + \text{cmychannel}$) and $\gamma$ is the neg gamma.

#### DCTL Parameters
**Exposure Trim**: This adjusts the printer light setting for all channels.

**Red/Green/Blue Trim**: Adjusts the printer light setting for the corresponding single channel. Note that only this slider exists in a real film printer.

**Cyan/Magenta/Yellow Trim**: Adjusts the printer light setting for the corresponding two channels. IE increasing Cyan trim results in increasing Green and Blue trims.

**Neg Gamma**: Indicate the gamma of the negative film stock that's being printed. This is the term $\gamma$ that's multiplied in in the above expression. The print stock gamma (Slope of 3 or 4 in Log Exposure vs Density chart) will presumably be embodied downstream of this DCTL via some sort of contrast adjustment.

---

### Quantize
Simulates the effect of saving the current image at a specified bit depth.

#### DCTL Parameters
**Bit Depth**: The number of bits to be used to represent the current 0->1 value.

**Clip**: Specifies whether to clip values greater than 1.0 or less than 0.0.

**Quantization Method** [ROUND, TRUNCATE, STOCHASTIC]: If set to Round, round each value to the nearest code value, if set to truncate, simply round down to the nearest below code value. If set to Stochastic, round up or round down with a probability equal to how close the value is to the nearest integer.




---

### RGB Chips DCTL
Creates rows of colored chips at the specified increment of stops. Outputs a Linear image. This is useful in evaluating tone mapping and the "Notorious Six" problem where overexposed pure colors get mapped to pure RGBCMY primaries rather than preserving their hue.

#### DCTL Parameters
**Saturation**: Saturation of the chips. In Spherical, this is set so that 1.0 results in positive CMY colors.

**Number of Hues**: Number of rows with different hues. Set this to 3 if you want pure RGB chips, set to 6 if you want RGB CMY chips. Increase if you want more granularity.

**Number of Columns**: Number of columns to generate

**Mid Gray**: Indicates the Value that will be assigned to the middle chip

**Min/Max Clamp**: If Clamp Output is checked, then if any channel falls outside the range [Min Clamp, Max Clamp], it will be replaced with white.

**Clamp Output**: Check to remove values that fall outside the Min/Max Clamp range.

**Gray Ramp**: Adds a gray ramp to the top of the frame.

**Vertical**: Turns the chart 90 degrees so it's vertical instead.

**Band Interval**: Indicate whether bands should be placed equidistantly (Equal), or one-stop apart (Exponential). The latter is better suited for pipelines that expect a Linear image.

**Model**: Choose whether to use HSV or Spherical to generate the chips. The spherical model is scaled such that at 1.0 saturation, all tiles will fit within the cube, and at 1.0 value, white is (1.0, 1.0, 1.0).

**Continuous Mode**: Choose whether each chip should be discrete, or if hue, exposure, or both should be continuous.



---

### Safety Lines DCTL
DCTL that creates a white frame to indicate safety boundaries for the image.

#### DCTL Parameters
**Aspect Ratio**: Specify the Width to Height ratio for the drawn rectangle.

**Scale**: Size of the box, if the specified aspect ratio is wider than the frame, then 1.0 means that the width will be matched. If the specified aspect ratio is narrower than the frame, then 1.0 means that the height will be matched. Smaller values indicate a smaller drawn box.

**Alpha**: Opacity of the drawn lines.

**Line Thickness**: Thickness of the drawn box.

**Shade Darkness**: Indicates brightness of the region outside of the box.




---

### Saturation Adjustment DCTL
DCTL that applies Gain and Gamma controls to the Saturation of an image. Can be used in HSL saturation or HSV saturation, though note that in HSL, be sure that inputs are between 0 and 1, as saturation is poorly defined for RGB values outside of this range. This is similar to a combination of the Color Boost (which is approximately a combination of saturation gamma + gain) and Saturation (gain) controls.

#### How it works
This DCTL converts the input image to HSL or HSV, then applies a Gamma and Gain adjustment to the result. Then, it converts the image back into RGB. This formulation allows for precise control over the Sat v Sat curve, while only allowing for a nondecreasing response. IE, in Resolve's standard Sat v Sat curve, you can make the mistake of having highly saturated objects in the scene become less saturated than less saturated objects, which will typically look unnatural.

#### DCTL Parameters
**Saturation Type**: Specify one of [HSL, HSV]. This converts the RGB image to HSL or HSV space depending on what is specified, and uses the saturation channel from that image. HSV will typically perform better when your input image has luminances greater than 1, but HSL will sometimes make more appealing saturation adjustments.

**Gain**: The input saturation is linearly multiplied by the Gain, so this will adjust the maximum saturation allowed in the image.

**Gamma**: This mimics the Gamma control in Resolve's Primaries wheels, applying a power function to the saturation channel. This is applied before Gain.

**Show Saturation**: This checkbox allows you to view just the saturation channel, after the Gain and Gamma adjustments have been made.

**Show Curve**: When checked, this displays the corresponding curves adjustment that's being made to the saturation channel.




---

### Spherical Color Space DCTL
Converts between RGB and a spherical color space. Outputs a 3-channel image, $(\rho, \theta, \phi)$. $\rho$ represents the radius, $\theta$ is scaled 0-1 and represents the hue, and $\phi$ represents saturation and is scaled from 0 to $\pi / 2$.

#### DCTL Parameters
**Invert**: When unchecked, converts from RGB to spherical, and when checked, converts from spherical to RGB.




---

### T-Log Curve
Converts between linear and my super cool, fully-logarithmic curve. Spec for this curve is 18% gray maps to 50 IRE, then 100% IRE is middle gray plus `num_stops/2`, and 0% is middle gray minus `num_stops/2`. Every stop of dynamic range has an equal number of code values given by `100% / num_stops`. Also clamps the linear input to be >= 0.0 to avoid NaNs.

#### DCTL Parameters
**Stops of Dynamic Range**: Indicates the value of `num_stops`, number of stops between 100% and 0%.

**Exposure Compensation**: Applies this many stops of gain prior to a lin2log conversion, and removes this many stops of gain after a log2lin conversion. Convenient if you want to map a tone other than 18% gray to 50IRE, but typically you should leave this at 0.

**Direction**: Indicates whether to convert linear to tlog, or from tlog to linear.





---

### Waveform Guides
Adds a border to the image and draws on the border so that the luma waveform has a luminance scale drawn on it. Put this **after** your ODT.

#### DCTL Parameters
**Minor Line Width**: Indicates how wide the narrow dashes on the left side of the waveform are.

**Line Thickness**: How wide you want the lines on the waveform to appear.

**Text Size**: Height of characters written on the left margin.

**Margin for Text**: How much horizontal space is allocated to the characters on the left margin

**Graticule Brightness**: Allows you to increase the brightness of the drawn graticule by repeating the drawn lines and characters.

**Num Stops**: Indicates how many stops of lines to show when you have ST2084 Stops or Bt1886 Stops selected as the Waveform Scale.

**BT1886 White Point**: If BT.1886 Annex 1 is selected as the Waveform scale, this controls the white point parameter, specified by $L_W$ in the official recommendation.

**BT1886 Black Point**: If BT.1886 Annex 1 is selected as the Waveform scale, this controls the black point parameter, specified by $L_B$ in the official recommendation. If set to 0, then this is just a gamma function.

**BT1886 Gamma**: Specifies the gamma curve, the recommendation says to fix this at 2.4.

**Mid Gray Nits**: Specifies the target brightness mid gray on your monitor, as a neutral point for the Stops modes.

**Rescale Mode**: Choose from None, Fill, or Aspect. As the drawn stuff in the margins changes the remaining space in the viewer, this chooses whether to just crop in on the iamge (None), to scale the image to fill the remaining space (Fill), or to scale the image while preserving its aspect ratio.

**Waveform Scale**: Choose how the horizontal guides in the waveform are calculated. If you choose the ones that end in "Stops", then the vertical units are in stops above/below mid gray, and if you choose "Nits", then the units are in nits assuming that your display is calibrated for the specified function.


---
### White Mask DCTL
Draws a white matte around the border of the frame, in case you need to spice up your standard viewing environment.

#### DCTL Parameters
**Scaling**: The original image is resized down to this scale.

**White Point**: Code value to draw for the matte.
