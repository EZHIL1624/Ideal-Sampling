# Ideal, Natural, & Flat-top -Sampling
---
# Aim
Write a simple Python program for the construction and reconstruction of ideal, natural, and flattop sampling.

# Tools required
Personel Computer

Python IDLE

# Theory

### Ideal or Instantaneous or Impulse Sampling:
sampling signal is a periodic impulse train. The area of each impulse in the sampled signal is equal to the instantaneous value of the input signal.

### Natural Sampling:
Natural sampling is also called practical sampling. In this sampling technique, the sampling signal is a pulse train.
In natural sampling method, the top of each pulse in the sampled signal retains the shape of the input signal during pulse interval.

### Flat Top Sampling:
The flat top sampling is also the practical sampling technique. In the flat top sampling, the sampling signal is also a pulse train. The top of each pulse in the sampled signal remain constant and is equal to the instantaneous value of the input signal 𝑥(𝑛) at the start of the samples.

# Program

### Ideal Sampling
```
import numpy as np
import matplotlib.pyplot as plt

fs, f = 100, 5
t = np.arange(0, 1, 1/fs)
sig = np.sin(2*np.pi*f*t)

plt.figure(figsize=(10,4))
markerline, stemlines, baseline = plt.stem(t, sig, label='Sampled Signal (fs=100Hz)')
plt.setp(stemlines, color='red', linewidth=0.8)
plt.setp(markerline, color='red', markersize=3)

plt.title('Sampling of Continuous Signal (fs = 100 Hz)')
plt.xlabel('Time [s]')
plt.ylabel('Amplitude')
plt.legend(loc='upper right')
plt.grid()
plt.show()
```

### Natural Sampling
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs=1000
T=1
fm=5
pr = 50

t = np.arange(0, T, 1/fs)
msg = np.sin(2*np.pi*fm*t)

idx = np.arange(0, len(t), fs//pr)
pw = fs//(2*pr)

pulse_train = np.zeros_like(t)
for i in idx:
    pulse_train[i:i+pw] = 1.0         

ns = np.zeros_like(t)
for i in idx:
    ns[i:i+pw] = msg[i:i+pw]

def lpf(sig, cut):
    b, a = butter(5, cut/(0.5*fs), 'low')
    return lfilter(b, a, sig)

rec = lpf(ns, fm+2)

fig, ax = plt.subplots(4, 1, figsize=(10,8))
for a, y, lbl in zip(ax,
    [msg, pulse_train, ns, rec],
    ['Original Message Signal', 'Pulse Train', 'Natural Sampling', 'Reconstructed Message Signal']):
    a.plot(t, y, label=lbl)
    a.legend()
    a.grid()

plt.tight_layout()
plt.show()
```

### Flat Top Sampling
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs, T, fm, pr = 1000, 1, 5, 50
t = np.arange(0, T, 1/fs)
msg = np.sin(2*np.pi*fm*t)

idx = np.arange(0, len(t), fs//pr)
pw = fs//(2*pr)
ft = np.zeros_like(t)
for i in idx:
    ft[i:i+pw] = msg[i]

def lpf(sig, cut):
    b, a = butter(5, cut/(0.5*fs), 'low')
    return lfilter(b, a, sig)

rec = lpf(ft, fm+2)

fig, ax = plt.subplots(4, 1, figsize=(10,8))

signals = [msg, np.ones(len(idx)), ft, rec]
labels  = ['Original Message Signal', 'Ideal Sampling Instances', 'Flat-Top Sampled Signal', 'Reconstructed Signal']
colors  = ['blue', 'blue', 'blue', 'green']

for i, (a, y, lbl, col) in enumerate(zip(ax, signals, labels, colors)):
    if i == 1:
        markerline, stemlines, baseline = a.stem(t[idx], y, label=lbl)
    else:
        a.plot(t, y, label=lbl, color=col)
    a.legend(); a.grid()

plt.tight_layout()
plt.show()
```
# Output Waveform
### Ideal Sampling Waveform
<img width="948" height="451" alt="impulse_output" src="https://github.com/user-attachments/assets/7cb0e898-5bed-4097-ad94-a8450925931b" />


### Natural Sampling Waveform
<img width="948" height="451" alt="natural_output" src="https://github.com/user-attachments/assets/b0ec3a06-9b5b-48fa-8839-1f56b95f0fa3" />

### Flat Top Sampling Waveform
<img width="948" height="451" alt="flat_top_output" src="https://github.com/user-attachments/assets/224d4ce5-e7e8-40b0-9b73-4c88becaf4e6" />

# Result
Thus, the construction and reconstruction of Ideal, Natural, and Flat-top sampling were successfully implemented using Python, and the corresponding waveforms were obtained.
