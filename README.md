# Eye Tracker (Gaze-Controlled Virtual Keyboard)

A real-time gaze-controlled virtual keyboard using OpenCV and dlib facial landmarks. Users navigate keys by looking left/right and select by blinking. Supports English and Arabic letters, symbols, numbers, CAPS LOCK for English letters, backspace, space, audio feedback, and a live text display GUI.

## Features

- Real-time eye and gaze tracking via webcam
- Blink-to-select with a visual loading bar (to tell person if he/she blinking or not and wait for how long time)
- Gaze left/right to cycle keys, center to hold position
- Multi-language layouts:
  - English (two pages)
  - Arabic (two pages)
  - Symbols (two pages)
  - Numbers
- Special keys: CAPS, ENG, ARB, SYB, NUM, << (to get the right part of keyboard), >> (to get the left part of keyboard), Backspace (<--), Space (__)
- Audio feedback for navigation and selection
- External text display GUI window

## Repository Structure


eye-tracker/
├── Keyboard_v2.py   # Keyboard rendering, key sets (EN/AR/Symbols/Numbers), drawing helpers
├── main_v2.py       # Main gaze-tracking loop (webcam, dlib, OpenCV, audio, GUI orchestration) --> has the mainn code of algorithm that determine if person look                         left or right or blinking
├── text_box.py      # Text display GUI (shows typed text in a separate window)
└── README.md


## Requirements

- Python 3.8+
- Webcam
- Good, even lighting

Python packages:
- opencv-python
- dlib
- numpy
- arabic-reshaper
- -PIL
- bidi
- pyglet
- threading
- math

Notes on dlib:
- Windows: May need Visual Studio Build Tools; consider prebuilt wheels for your Python version.
- macOS: xcode-select --install and brew install cmake can help.
- Linux: Ensure cmake, boost, and dev headers are installed.

## Assets

1) Dlib 68-face-landmarks model
- Download: http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2
- Decompress to get shape_predictor_68_face_landmarks.dat --> we will use it to get eyes regions

2) Audio files (optional but recommended)
- click.m4a, left.m4a, right.m4a

## Configuration

Update the hardcoded paths in main_v2.py to your local files (prefer relative paths):

- Dlib model (line 16):
python
predictor = dlib.shape_predictor("your_project_path/shape_predictor_68_face_landmarks.dat")


- Audio files (lines 22–24):
python

click_sound = pyglet.media.load("your_project_path/click.m4a", streaming=False)
left_sound  = pyglet.media.load("your_project_path/left.m4a",  streaming=False)
right_sound = pyglet.media.load("your_project_path/right.m4a", streaming=False)


## Run

main_v2.py to ru the project


Windows will appear:
- Frame: webcam feed with eye overlays and loading bar
- Virtual keyboard: on-screen keyboard with the highlighted key
- Text Display GUI: shows your typed text

Press Esc in the Frame window to exit and you will see only text box of what you typed.

## Interaction

- Navigate keys: Look left or right and hold briefly
  - Navigation threshold: 8 consecutive frames by default
- Select key: Blink and keep eyes closed briefly
  - Selection threshold: 9 consecutive frames by default
- Center gaze: Holds current key
- Special keys:
  - CAPS: Toggle uppercase for English
  - ENG / ARB / SYB / NUM: Switch layouts
  - >> / <<: Next/previous page within a layout
  - <--: Backspace
  - __: Space

## Tuning

In main_v2.py (bottom):
python
blinking_ratio = 4.9
right_ratio_down_from = 0.61
left_ratio_start_from  = 3.38
center_ratio = [right_ratio_down_from, left_ratio_start_from]


- Increase blinking_ratio if blinks trigger too easily
- Increase right_ratio_down_from if you want to increase the sensitivity of right eye gaze detection
- Decrease left_ratio_start_from if you want to increase the sensitivity of left eye gaze detection
- Keep lighting consistent and camera at eye level

## How It Works

- Detect face and 68 landmarks with dlib
- Extract and threshold eye regions to estimate white/pupil distribution
- Compute gaze ratio (left-eye white vs. right-eye white) and average both eyes
- Interpret direction from thresholds: left, center, right
- Count frames that person makes specific action on to confirm the action (navigation or blink selection)
- Render a virtual keyboard image and update active key
- Display typed text in a separate GUI window

## Troubleshooting

- dlib install fails:
  - Use a compatible Python version with prebuilt wheels or install required build tools
- No webcam feed:
  - Ensure camera access, try cv2.VideoCapture(1) if multiple cameras
- Blink too sensitive/insensitive:
  - Adjust blinking_ratio
- Gaze unreliable:
  - Improve/normalize lighting, tweak left/right thresholds
- Audio errors:
  - check file paths or the installation of pyglet


## project Running example
- here you will see the project with all features working
- https://drive.google.com/file/d/1uQkyUIwnTO9i4tSeV8DQkFgWxRKFC1pG/view?usp=drive_link


## future improvements
- add word suggestion as in google keyboard to increase wordd typing speed
- make the program smart enough to decide it's threshold depend on lighting and if person put glasses or not
- make the program can control everything on computer using person's eye like he/she has mouse to make him/her act normal like anyone

  
## Contact Team
- For questions or suggestions, go ahead and contact us.
  
- [Ahmed Saad](https://github.com/Ahmedsaad427) (UI/UX & Product Designer)
- [Ahmed Yasser](https://github.com/ahmed-yasser-taha) (Project Manager & Team Leader)
- [Ismail Elyan](https://github.com/ismaillalyaan) (Technical Lead)
- [Ismail Al-hetimi](https://github.com/IsmailMohamed010) (Market Analysis)
- [Kirillus Emad](https://github.com/Kirillus-Emad) (Software Engineer)
