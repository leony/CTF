
# Signal.dat write-up

Below is write-up of Signal.dat which captured RF Signal.

After CTF ended, I solved this challenge.

For this write-up, I received a lot of help from **'Lee Kwon-hyung'**, my colleague.

```Challenge : Analyze the Signal and submit a flag.```

RF signal challenge usually demodulate signal to get the flag value in digital data.

There is a tool called URH that can do this,

URH(Universal Radio Hacker) really useful for beginners or newbie analyzing RF Signal. Because It has the ability to demodulate modulated signals automatically.

![1](https://user-images.githubusercontent.com/614361/97012727-245e7000-1583-11eb-9f3e-63ee625f680b.png)

Download link.
https://github.com/jopohl/urh

Install/Execute URH and then open signal.dat.

Click Autodetect parameters on the Interpretation TAB.

The result is there are digital data of eight signals and ASK(Amplitude Shift Keying) Modulation is used.

![2](https://user-images.githubusercontent.com/614361/97018268-eadd3300-1589-11eb-849c-7319dc686385.png)

Try to split the digital data by 8 bits and convert it into ASCII code. But If check on the Analysis tab, all eight signals are missing by one in multiples of 8. 
(First signal : 343 bits = 8bits x 43 – 1bits, Second signal : 247 bits = 8 bits * 31 – 1bits)

![3](https://user-images.githubusercontent.com/614361/97020552-a606cb80-158c-11eb-8382-fb233ff08012.png)

Click 'Mark diffs in protocol' on Analysis TAB and analyze differences for each signal.
Based on signal 1, Different parts for each signal are displayed in red. It make sense that signals 3, 5 and 7 are the same signal as signal 1. Also, 2,4,6,8 signals are the same.
Therefore, Only first and second signals need to be analyzed.

![4](https://user-images.githubusercontent.com/614361/97021423-caaf7300-158d-11eb-8b03-6857ad56c7eb.png)

Click "Analyze Protocol" on the Analyze TAB. As a result of execution, The front 24bit is the data of sync.

![5](https://user-images.githubusercontent.com/614361/97022298-e6ffdf80-158e-11eb-9c76-2d8c2366845f.png)

Converts the rest of the data to ASCII code except 24bit. Since 1 bit is less than 8 bit multiple, try adding 0 in front of it.

```
SIGNAL_1 = '01010011..........' # first signal data
SIGNAL_2 = '01010110..........' # second signal data
BIT =8

def decode_signal(SIGNAL):
	RESULT = ''
	for count in range(0, int(len(SIGNAL)/BIT)):
		TMP = SIGNAL[COUNT*BIT:COUNT*BIT+BIT]
		RESULT = '%s%s' % (RESULT, chr(int(TMP,2)))
	return RESULT

print(DECODE_SIGNAL(SIGNAL_1))
print(DECODE_SIGNAL(SIGNAL_2))
```
Decode base64




