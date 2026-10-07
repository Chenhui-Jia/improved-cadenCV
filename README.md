# Improved cadenCV

**Optical Music Recognition with MIDI Playback**
An academic project by **Chenhui Jia**, developed for CSC370 at Smith College.

Improved cadenCV builds on [Afika Nyati's cadenCV](https://github.com/afikanyati/cadenCV), an optical music recognition (OMR) system that converts images of printed sheet music into MIDI files. This project focuses on handling single-staff scores, extending symbol templates, and improving the mapping from note positions to pitches.

The generated MIDI file can be opened in a compatible player or music application to hear the recognized score. This is an experimental course project rather than a complete music transcription tool.

## My contributions

- **Single-staff handling:** added conditional logic for scores containing only one staff, avoiding an assumption that a second staff always exists.
- **Cropping adjustments:** bypassed the multi-staff cropping procedure for single-staff inputs to avoid invalid image crops.
- **Additional symbol templates:** expanded the reference images used to recognize treble clefs and time signatures.
- **Pitch mapping:** revised the calculation that relates a note's vertical position to its pitch.

These changes are implemented in `improved.py`. The repository also contains `main.py` and `chenhui.py`, which retain earlier development versions.

## How it works

1. Load a score image, reduce noise, and binarize it using Otsu's thresholding.
2. Estimate staff line thickness and spacing, then locate and remove staff lines.
3. Detect musical symbols using template matching and geometric information.
4. Interpret note positions and durations in the context of the detected staff.
5. Write the reconstructed notes to a MIDI file for playback.

The system uses traditional image processing and template matching. A neural network symbol recognizer was proposed as future work; it is not implemented here.

## Installation

The code uses Python and these libraries:

- NumPy
- OpenCV
- Matplotlib
- Pillow
- MIDIUtil

From a terminal, install the dependencies:

```sh
python -m pip install numpy opencv-python matplotlib Pillow MIDIUtil
```

The original project targeted Python 3.6. This repository contains legacy research code, and compatibility with current Python and library versions has not been verified.

## Running the improved version

Run commands from the repository root so that the relative template and output paths resolve correctly.

1. Open `improved.py` and find the input setting under `if __name__ == "__main__":`:

   ```python
   img_file = "example5.jpg"
   ```

2. To process another score, change this value to the path of your image. Keep the supplied example to try the current default.
3. Ensure the `output` directory exists, then run:

   ```sh
   python improved.py
   ```

**The current improved script uses this image setting and does not read a command-line image argument.**

The script writes diagnostic images and the MIDI result:

| File | Contents |
| --- | --- |
| `binarized.jpg` | Thresholded input image |
| `output/detected_staffs.jpg` | Detected staff regions |
| `output/staff_*_primitives.jpg` | Symbol detection visualizations for each staff |
| `output/output.mid` | Recognized music for MIDI playback |

Subsequent runs overwrite outputs with the same names. Open `output/output.mid` in a MIDI-compatible application to listen to the result.

## Results from the course project

The final report describes an 11-note test score on which the original system recognized **0 of 11 pitches correctly**, while the improved version recognized **9 of 11 pitches correctly**. The two remaining errors were B4 notes recognized as A4. Stem-direction handling was identified as a possible cause, but this was not confirmed.

Another experiment produced incorrect pitches for a two-staff score, while processing its first staff alone produced correct pitches for that excerpt. This motivated the focus on single-staff handling.

These are individual examples reported in the course project, not a benchmark across a large dataset. Recognition accuracy remains dependent on the score and image quality.

## Scope and limitations

The inherited system targets simple printed, monophonic scores, using treble or bass clefs and common time signatures such as 2/4, 3/4, 4/4, and 6/8. Its original scope covers note values of an eighth note or longer.

Multi-staff pitch recognition remains unreliable. The project does not provide comprehensive support for polyphonic music, multiple instruments, changes of key or meter, tuplets, dotted rhythms, repeats, ties, slurs, dynamics, or articulation. Tempo markings are not interpreted.

## Future work

- Extend the single-staff pitch corrections to multi-staff scores.
- Explore processing each staff separately before combining the results.
- Improve handling of note stems and ambiguous symbol matches.
- Evaluate a learned symbol recognizer and a larger, more varied test set.

## Repository guide

| Path | Purpose |
| --- | --- |
| `improved.py` | Improved OMR pipeline and current example entry point |
| `main.py`, `chenhui.py` | Earlier development versions |
| `staff.py`, `bar.py`, `primitive.py`, `box.py`, `best_match.py` | Supporting representations and matching logic |
| `resources/template/` | Reference images for musical symbol recognition |
| `example5.jpg`, `example6.jpg`, `example7.jpg` | Local example score images |
| `output/` | Generated MIDI and diagnostic images |

## Acknowledgments

The original cadenCV system was created by **Afika Nyati**. This fork preserves that project's foundation and documents **Chenhui Jia's** subsequent improvements.

- [Original cadenCV repository](https://github.com/afikanyati/cadenCV)
- [Original project demonstration](https://www.youtube.com/watch?v=amL6wHfAShw)

Project report: *Improved cadenCV: An Optical Music Recognition System with Audible Playback*, Chenhui Jia, CSC370 Final Report.
