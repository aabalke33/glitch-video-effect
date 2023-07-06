# glitch-video-effect
Applies a Position Offset Glitch Effect to a still image or frames of a video.

This package applies a "glitch" effect to video frames or a still image.

REQUIRES [JAVA RGBOFFSET PACKAGE](https://github.com/aabalke33/rgb-offset).

The intensity of the effect can be controlled by means of an amplification parameter that is passed into the functions. See Usage for more information.
Multithreading is enabled by default.

Source                 |  Output (Amp: 4, maxLength: 48)        
:-------------------------:|:-------------------------:
![source](https://github.com/aabalke33/glitch-video-effect/assets/22086435/04a26cf3-3687-454c-bb41-ebda0fda8081)  |  ![glitch](https://github.com/aabalke33/glitch-video-effect/assets/22086435/5523eac7-bc1b-4fc3-8e06-345ca430e517)

## Usage
The glitch methods take image data expressed as BufferedImage(s). An example is provided below for a standard method of creating and inputting parameters.

Methods are overloaded. Allows methods for a single source image or an array of source images. Method overloading to provide an inputted max thread count is included as well (by default this package uses 4).

```java
// With maxThreads
  // For Single Image
  GlitchEffect.glitch(sourceImage, outputFolder, amplification, maxThreads, maxLength);

  // For Array of Images
  GlitchEffect.glitch(sourceImages, outputFolder, amplification, maxThreads, maxLength);

// Without maxThreads (Default 4)
  // For Single Image
  GlitchEffect.glitch(sourceImage, outputFolder, amplification, maxLength);

  // For Array of Images
  GlitchEffect.glitch(sourceImages, outputFolder, amplification, maxLength);

```

## Important Considerations
1. The current package requires an image file or array of image files to be inputted. A video file directly is not supported at this time.
2. Has to be 8 Bit per Channel Image(s).
3. Requires Java rgbOffset Package.

## Example

```java
import java.io.File;
import java.util.ArrayList;
import java.util.Arrays;

public class Example {
    public static void main(String[] args) {

        // Get all image files in each folder and add them to an ArrayList of files
        String sourceFolder = "./resources/source/";
        ArrayList<File> sourceFrames = new ArrayList<>(Arrays.asList(
            Objects.requireNonNull(new File(sourceFolder).listFiles())));

        // Create File object of output folder
        String outputPath = "./resources/output/";
        File outputFolder = new File(outputPath);


        int amplification = 4;
        int maxLength = 48;
        int maxThreads = 8;

        GlitchEffect.glitch(sourceFrames, outputFolder, amplification, maxThreads, maxLength);
    }
}
```

## FAQ
1. **Why do I get a Memory Heap Error?** You have too many threads. Lower maxThreads until stable. The multithreading gains significantly diminish if maxThreads is greater than the Physical CPU Core count. maxThreads defaults to 4 since this is the average amount of CPU cores at creation.
2. **Why are my composite frames out of order?** The source frames are ordered by name left to right, you will need to have padded zeros to keep serialized frame names in order. If you don't do this you will have problems (ex. if you have 200 frames the order will be "1.jpg, 10.jpg, 100.jpg, 2.jpg, 20.jpg, 200.jpg" instead of "1.jpg, 2.jpg, 3.jpg")

## Roadmap
- Add support for video files directly instead of relying on frame sequences
- Add support for different glitch "tapering off" functions, currently only e^x supported
- Support 16bit and 24bit images
