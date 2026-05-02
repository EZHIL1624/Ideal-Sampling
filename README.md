# Ideal, Natural, & Flat-top -Sampling
# Aim
Write a simple Python program for the construction and reconstruction of ideal, natural, and flattop sampling.
# Tools required
GOOGLE COLLAB
# Program
## Impulse sampling
```
Impulse sampling
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import resample

fs, T, f = 100, 1, 5
t = np.arange(0, T, 1/fs)
signal = np.sin(2*np.pi*f*t)

plt.plot(t, signal); plt.title("Continuous"); plt.grid(); plt.show()

plt.stem(t, signal)
plt.title("Sampled"); plt.grid(); plt.show()

recon = resample(signal, len(t))
plt.plot(t, recon); plt.title("Reconstructed"); plt.grid(); plt.show()
```
## Natural sampling
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs, T, fm, pr = 1000, 1, 5, 50
t = np.arange(0, T, 1/fs)

msg = np.sin(2*np.pi*fm*t)

pulse = np.zeros_like(t)
w, step = int(fs/pr/2), int(fs/pr)
for i in range(0, len(t), step):
    pulse[i:i+w] = 1

sampled = msg * pulse

def lp(x, c): 
    b, a = butter(5, c/(0.5*fs), 'low')
    return lfilter(b, a, x)

recon = lp(sampled, 10)

titles = ["Original", "Pulse Train", "Natural Sampling", "Reconstructed"]
signals = [msg, pulse, sampled, recon]

for i, s in enumerate(signals, 1):
    plt.subplot(4,1,i)
    plt.plot(t, s)
    plt.title(titles[i-1])

plt.tight_layout()
plt.show()
```
## Flat top sampling
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs, T, fm, pr = 1000, 1, 5, 50
t = np.arange(0, T, 1/fs)
msg = np.sin(2*np.pi*fm*t)

idx = np.arange(0, len(t), int(fs/pr))
flat = np.zeros_like(t)
w = int(fs/(2*pr))

for i in idx:
    flat[i:i+w] = msg[i]

def lp(x, c):
    b, a = butter(5, c/(0.5*fs), 'low')
    return lfilter(b, a, x)

rec = lp(flat, 10)

titles = ["Original", "Sampling Points", "Flat-Top", "Reconstructed"]
signals = [msg, np.zeros_like(t), flat, rec]

for i, s in enumerate(signals, 1):
    plt.subplot(4,1,i)
    if i == 2:
        plt.stem(t[idx], np.ones_like(idx))
    else:
        plt.plot(t, s)
    plt.title(titles[i-1])

plt.tight_layout()
plt.show()
```
# Output Waveform

## Ideal sampling

<img width="568" height="435" alt="impulse sampling" src="https://github.com/user-attachments/assets/ed94e70a-dc64-496b-a5f1-d4c381f48135" />


## Natural sampling

<img width="989" height="790" alt="natural sampling" src="https://github.com/user-attachments/assets/478400df-9368-43cf-b399-dfb6162f33e0" />

## Flat top sampling

<img width="989" height="790" alt="flat top" src="https://github.com/user-attachments/assets/90897af2-4321-4666-b8b9-ef6699dad0b1" />

# Results

Thus, the construction and reconstruction of Ideal, Natural, and Flat-top sampling were successfully implemented using Python, and the corresponding waveforms were obtained.

