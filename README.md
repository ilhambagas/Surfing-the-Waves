# Solution to "Surfing the Waves" picoCTF Forensics Challenge
I have finished Surfing the Waves challenge (Hard level) from PicoCTF, and here is the solution if you want to try:
#
## Key Steps:
1. Visualize the Audio: Open `main.wav` in Audacity, apply the "Amplify" effect (e.g., +20 dB) to reveal patterns in the waveform.

2. Extract and Process Samples: Use Python with SciPy to read the audio data. The samples have values like `[2008, 2506, 2000, 1508, 2009, 8504]`. Chop off the last two digits (noise: always ends in 0 followed by 1-9), leaving rounded values like 20, 25, etc.

3. Decode Hex: The unique rounded values `[10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85]` map to hex digits 0-9, A-F (e.g., 10 → 0, 15 → 1, ..., 85 → F). Map each sample, concatenate into a hex string, and convert to ASCII to reveal a Python script (`decoded_program.py`) that generated the audio.

4. Run the Script: The decoded script prints the flag when executed.

**Sample Python Code for Decoding (from writeup):**
```python
from scipy.io import wavfile
import numpy as np

# Read WAV file
samplerate, data = wavfile.read('main.wav')

# Flatten if stereo
if len(data.shape) > 1:
    data = data[:, 0]

# Round by removing last two digits
rounded = [int(x / 100) * 100 for x in data]  # Adjust as needed for precision

# Unique values sorted
unique_vals = sorted(set(rounded))
hex_map = {val: hex(i)[2:].upper() for i, val in enumerate(unique_vals)}

# Map to hex string
hex_str = ''.join(hex_map[val] for val in rounded if val in hex_map)

# Convert hex to ASCII (in chunks of 2)
flag = ''.join(chr(int(hex_str[i:i+2], 16)) for i in range(0, len(hex_str), 2))
print(flag)
```
*(Note: Fine-tune rounding/mapping based on exact sample values; the script output is the flag.)*
#
# Final Flag
`picoCTF{mU21C_1s_1337_115155af}`






