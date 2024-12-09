Pitch-shifting is a technique used in audio recording originating in the 1960s,
with a significant evolution with the emergence of digital pitch-shifting hardware
in the 1970s. It is now widely used in popular music for pitch correction and
unlocking boundless creative purposes. Over time, the audio industry has focused
on high-quality pitch-shifting tools, leading to creation of extraordinary algorithms.
This project delves into the realm of real time pitch shifting with utilization of input
signals through two modes: microphone and uploading an audio file. The versatility
of this pitch-shifting program is showcased by a graphical user interface (GUI)
that has been created for user-friendly application, presenting users with both high
and low pitch-shifting options. Furthermore, this project serves as a foundational
guide to the methodology of pitch-shifting laying a groundwork to perform more
sophisticated tasks by integrating pitch-shifting with other audio signal processing
methodologies as per one’s creativity and needs as a future work. A word of advice:
utilizing headphones while usage of this program in microphone mode to ensure a
better user experience and precise adjustments.

Methods
This code is a Python script that uses PySimpleGUI to create a GUI with options to
perform wave file pitch shifting and microphone (MIC) pitch shifting. It incorporates
libraries like PyAudio, NumPy, os, SoundDevice, Librosa, and Threading.
Here’s a brief summary of each library used in this code:

• **PySimpleGUI:** Simplifies GUI development in Python by providing an easy-to-use
interface for creating graphical user interfaces.

• **NumPy:** A powerful library for numerical computing in Python, offering support
for large, multi-dimensional arrays and matrices along with a collection of mathematical
functions.

• **os:** Provides a portable way to interact with the operating system, allowing Python
programs to perform various operating system-related tasks like file manipulation,
environment variables, etc.

• **Sounddevice:** A Python library facilitating real-time audio input-output tasks,
enabling input and output audio stream handling, ideal for audio processing applications.
• Librosa: Specifically designed for music and audio analysis, librosa aids in tasks
like feature extraction, manipulation, and analysis of audio signals.

• **Threading:** Offers a way to run multiple threads simultaneously in a Python program,
allowing for concurrent execution and efficient handling of tasks without
blocking the main program flow.

Here is a breakdown of the code:

• GUI Creation Functions: win(), win1(), and win2() functions create different
windows using PySimpleGUI library. These windows provide options for selecting
a file, controlling pitch, and displaying the status of playing audio. win(), creates
and returns a PySimpleGUI window for the main GUI interface with options for
WAV or MIC pitch shifting. win1() for handling WAV files, allowing selection,
playback, and pitch adjustment which can be input as a number. win2() for MIC
pitch shifting, enabling real-time pitch adjustment.

-Global Variables: It consist of following global variables: <br/>
-RATE: Sampling rate for audio processing. It is set at 44100 samples per second.<br/>
-PITCH SHIFT: Default pitch shift value. It is set at 2<br/>
-STREAM THREAD: Variable to store the stream thread for real-time microphone
pitch-shifting.

-STOP STREAM EVENT: Event to signal the stream thread to stop.<br/>
• Audio Processing Functions: change pitch(audio, semitones) function modifies
the pitch of the audio by adjusting the frame rate based on the specified semitones.
Audio: This parameter represents the input audio signal. It is assumed to be an
object that has attributes like raw data and frame rate.

Semitones: The amount by which the pitch should be shifted, expressed in semitones.
A positive value will raise the pitch, and a negative value will lower the pitch.<br/>
audio.raw data: This is the raw audio data of the input audio. The function is
using this raw data to create a new audio object.<br/>
audio spawn(): The audio spawn method is used to create a new instance of the
audio object with modified properties.<br/>
overrides: This is a dictionary that specifies the properties to be overridden in the
new audio segment.<br/>
audio.frame rate: The original frame rate (sampling rate) of the input audio.
(2 ** (semitones / 12.0)): This part calculates the factor by which the frame rate
should be modified based on the desired pitch shift. The formula (2 ** (semitones /
12.0)) represents the ratio of frequencies corresponding to the semitone difference.
It is derived from the fact that each semitone corresponds to a frequency ratio of
2**(1/12).<br/>
int(): The result is converted to an integer to ensure that the frame rate is a whole
number.<br/>
Return Statement: The function returns a new audio object with the modified
frame rate. The raw audio data remains unchanged, but the pitch will be affected
by the modified frame rate.<br/>
• mic callback: This function is a callback function intended to be used with the
sounddevice library for real-time audio streaming. It is called periodically by the
audio stream to process incoming audio samples.<br/>
indata: Input buffer containing incoming audio data.<br/>
outdata: Output buffer where processed audio data should be stored.<br/>
frames: Number of frames in the input buffer.<br/>
time: Timestamp associated with the current buffer.<br/>
status: Status information about the callback. It prints the status if available.
if status: This condition checks the status of the audio stream. If there is any
status information, it prints a message indicating the status. Status can include
information about overflows, underflows, etc.<br/>
audio data = np.frombuffer(indata, dtype=np.float32): It converts the incoming
raw audio data (contained in the indata buffer) to a NumPy array of 32-bit
floating-point numbers (np.float32).<br/>
shifted audio = librosa.effects.pitch shift(audio data, sr=RATE,
n steps=PITCH SHIFT, n fft=1024): It applies pitch shifting to the input
audio data using the librosa.effects.pitch shift function.<br/>
sr=RATE: The sampling rate of the audio data.<br/>
n steps=PITCH SHIFT: This is the pitch shift value in semitones, taken from
the global variable PITCH SHIFT.<br/>
n fft=1024: The size of the Fast Fourier Transform (FFT) window used in pitch
shifting. The shifted audio variable now contains the pitch-shifted audio data.
Padding: This code checks if the length of the pitch-shifted audio (shifted audio)
is less than the length of the incoming audio data (indata). This comparison is
checking if the pitch-shifted audio is shorter than the incoming chunk of audio.
np.pad: It is a NumPy function used to pad an array. In this case, it pads the
shifted audio array with zeros.<br/>
Truncation (else block): If the pitch-shifted audio is longer than or equal to the
length of the incoming audio chunk, it truncates the pitch-shifted audio to match
the length of the incoming audio. Finally, the pitch-shifted and possibly padded or
truncated audio data is copied to the output buffer (outdata), which will be used
for playback.<br/>

stream thread function: This code segment is using the SoundDevice library to
create an audio stream with a specified callback function (mic callback) that will
be called periodically to process incoming audio data.<br/>
with sd.Stream(...): This line establishes a context using the with statement
to create and manage a sound stream (sd.Stream). The context ensures that the
stream is properly opened and closed.<br/>
callback=mic callback: The callback parameter specifies the function (mic callback)
that will be called periodically to process incoming audio data. In this case, it is
the callback function responsible for pitch-shifting microphone input in real-time.
channels=1: The channels parameter specifies the number of audio channels. In
this case, it is set to 1, indicating mono audio.<br/>
samplerate=RATE: The sample rate parameter sets the sampling rate (number
of samples per second) for the audio stream. It uses the global variable RATE as
the sampling rate.<br/>
print(’Pitch shifting started. Press Ctrl+C to stop.’): This line prints a
message indicating that the pitch-shifting process has started. It also informs the
user that they can stop the program by pressing Ctrl+C in the terminal.
STOP STREAM EVENT.wait(): The STOP STREAM EVENT is an instance
of threading. This event is used to signal the stream thread to stop. The wait()
method blocks the execution of the thread until the event is set. In this context,
it is waiting for the STOP STREAM EVENT to be set, which is typically done by
another part of the program when the user decides to stop the pitch-shifting process.
play audio: This function plays a pitch-shifted audio file, continuously checking
the STOP PROGRAM flag to determine whether to stop the audio stream. It also
updates the GUI to clear the status message when the audio playback is complete.
def pitch(window pitch): This function continuously waits for user interactions
in the GUI, processes the selected audio file and pitch value, updates the GUI status,
and initiates parallel audio playback using threading. The function exits when
the user closes the window, clicks ”Exit,” or when the STOP PROGRAM flag is
set.<br/>
def pitch(): This function creates a GUI window for audio playback with pitchshifting
capabilities. It enters an event loop, continuously waiting for user interac-
tions. It handles exit conditions, missing audio files, and invalid pitch values. It
uses threading to play audio in a separate thread to avoid freezing the GUI. The
program terminates when the user closes the window or clicks ”Exit.”
set pitch shift function: This function allows for dynamically updating the value
of the global variable PITCH SHIFT by providing a new value as an argument to
the function. It provides a convenient way to change the pitch shift value from other
parts of the program.<br/>
stop mic function: The stop mic function is designed to stop the microphone
stream by setting the STOP STREAM EVENT. When this function is called, it sets
the event that is being checked by the microphone stream thread (stream thread).
This signal informs the stream thread that it should stop processing and exit.
def mic() function: This part of the program is responsible for handling the
microphone input with real-time pitch shifting. It sets up a GUI for controlling
real-time pitch-shifting of microphone input. It enters an event loop, continuously
waiting for user interactions. It handles exit conditions, pitch value changes, and
the stop button. It uses threading to stream microphone input with pitch shifting,
and the program terminates when the user closes the window, clicks ”Exit,” or stops
the microphone.<br/>
window main: This part of the code sets up the main PySimpleGUI window (window
main) and enters an event loop to continuously wait for user interactions, and
handles events such as quitting, starting the pitch-shifting process, and starting the
microphone process. It displays pop-up messages if there are exceptions during the
execution of the associated functions and continues to wait for user interactions.
The program terminates when the user closes the main window or clicks the ”Quit”
button.<br/>
This script integrates these libraries to provide an interactive interface for users to manipulate
audio files (WAV) or modify the real-time pitch of the microphone input (MIC).
Users can adjust pitch settings, play audio, and observe status updates within the GUI.
Overall, it is a comprehensive audio manipulation tool that offers both file-based and realtime
pitch-shifting capabilities through a user-friendly interface built with PySimpleGUI.
