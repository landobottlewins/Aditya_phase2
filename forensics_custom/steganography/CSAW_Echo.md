# CSAW 2024 Qualifiers: Is There An Echo
> "description": "Maybe next time you should record your music in an acoustically treated space."
>
> "files": "is_there_an_echo/256.wav"


## Solution

```
┌──(kali㉿kali)-[~/Desktop]
└─$ strings 256.wav
...

...

$ G O > K 
> Q P D 
!       F       k
        -       @
cepstral domain single echo
```

- Cepstral analysis is the standard method for detecting echoes in a signal.
- If a signal has an echo at delayN, the Cepstrum graph will have a spike at index N
- `ifft(log(abs(fft(signal)))))` in python
- I didnt understand exactly how to write the code for this so I will just walk through the provided solution from the archived writeup
```
secret_len = 256
```
- the audio file was divided into 256 small chunks. In each chunk, a fake "echo" was added at a specific delay
```
sol_cepstrum_helper(s, main_delta)
```
- It takes a small chunk of audio `s` and calculates the real cepstrum of that chunk.
- compares delta0 and delta1 to give either 1 or 0 as output

```
sol_cepstrum_analysis(main_delta)
```
- It splits the main audio file into 256 equal parts and build a list of 256 bits (0s and 1s)
```
solve(main_delta)
```
- 256 bits group them into 8-bit bytes and convert to ascii
- If the text starts with `csawctf{`, it prints the flag.


Then there is a brutefore loop
```python

for j in range(25, 75):
	print(j)
	for i in range(0, j):
		main_delta = (i, j)
		solve(main_delta)
```

- loops j from 25 to 74 and i from 0 to j.
- guesses if delay i is 0 and delay j is 1
- runs the solver for every combination until the output looks like a valid flag


```python
import scipy
import soundfile as sf
import numpy as np

# find the hint "cepstral domain single echo" by running the commandline tool "strings" on the file

stegsignal, rate = sf.read('is_there_an_echo/256.wav')

# length given in file name
secret_len = 256

def chrcatch(i):
    print(i)
    if i == 0:
        return
    else:
        return chr(i)

def find_hint(signal):
    c = [chrcatch(int(x)) for x in signal[-30:]]
    print(c)

def sol_cepstrum_analysis(main_delta):
    # divide array into secret_len segments, ignore trailing
    incr = len(stegsignal) // secret_len
    steg_arr = stegsignal[:secret_len*incr].reshape((secret_len, incr))

    #decode
    decode_arr = []
    for s in steg_arr:
        decode_arr.append(sol_cepstrum_helper(s, main_delta))
    return decode_arr

def sol_cepstrum_helper(s, main_delta):
    delta0, delta1 = main_delta

    # cepstrum analysis equation
    cep_s = scipy.fft.ifft(np.log(np.abs(scipy.fft.fft(s))))

    # determine 0 or 1
    if cep_s[delta0+1] > cep_s[delta1+1]:
        return 0
    else:
        return 1

def solve(main_delta):
	sol_bin = sol_cepstrum_analysis(main_delta)
	
	sol_binarr = ["".join([str(x) for x in sol_bin[i:i+8]]) for i in range(0, len(sol_bin) - 7, 8)]
	sol_string = ''.join([chr(int(i, 2)) for i in sol_binarr])
	if sol_string[:8] == 'csawctf{':
		print(sol_string)

find_hint(stegsignal)

for j in range(25, 75):
	print(j)
	for i in range(0, j):
		main_delta = (i, j)
		solve(main_delta)
```

## Flag
```
csawctf{1nv3st_1n_s0undpr00f1ng}
```
