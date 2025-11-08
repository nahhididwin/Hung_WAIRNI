Project : Hung_WAIRNI method (Hưng_WAIRNI method)
(GitHub : https://github.com/nahhididwin/Hung_WAIRNI)

Creator : Đặng Đình Phú Hưng (Dang Dinh Phu Hung, https://github.com/nahhididwin)
Author's country: Vietnam
Date of birth (Dang Dinh Phu Hung) (DD/MM/YYYY): 20/06/2011
Languages ​​used: English (There may be mistakes in the Vietnamese to English translation, as I am Vietnamese.)
Publication date (DD/MM/YYYY): 06/11/2025

This project uses this license (MIT LICENSE) : https://github.com/nahhididwin/Hung_WAIRNI/blob/main/LICENSE
This version is based on many things from the previous version (This is not a request to reread the previous version, it is just a notice) : https://github.com/nahhididwin/Hung_WAIRNI/tree/main/WAIRNI.v2.0

--
Main parts:
0. What is this project for?
1. The "split" problem, asymmetry, and related data types
2. Important things
3. Find "peak" with Hung_WAIRNI
4. Calculate area (Numerical integration) with Hung_WAIRNI
5. Error analysis
6. Comparison with other methods/algorithms
7. Deploy with code
8. End
--


--
0. What is this project for?
A method called "Hung_WAIRNI", is a numerical integration method, a peak finding method, used to find "peaks", calculate "areas". Optimized to suit the specific task, currently according to the author, is suitable for AC power.
--

--
1. The "split" problem, asymmetry, and related data types

The process of converting continuous signals (waves from the environment) into digital signals on a computer is called digitization, includes two main steps:
Sampling and Quantization.
Equal spacing: In most common applications (such as audio, image, video, conventional radio signal processing, sensor measurement...),
analog-to-digital converters (ADC) will sample the signal at fixed intervals, called the sampling period (T_s).
Easy Calculation and Recovery: Equally spaced sampling makes digital signal processing (DSP) later becomes much simpler and more efficient.
More importantly, it allows to recover the original analog signal as accurately as possible (within the limits of Nyquist–Shannon Theorem)
if the sampling frequency (F_s = 1/T_s) is large enough.

Nyquist–Shannon Theorem :
on wikipedia (a theorem used in the field of information theory):
https://vi.wikipedia.org/wiki/%C4%90%E1%BB%8Bnh_l%C3%BD_l%E1%BA%A5y_m%E1%BA%ABu_Nyquist%E2%80%93Shannon

==> Yes, this is similar to an alternating current (AC) signal.

And the real data is mostly (almost certainly) "asymmetric", and has a lot of "sampling" locations.
Imagine: Has your heart ever had a beat exactly 1 second after an ECG, and then exactly 1 second later it beats again, and so on for the rest of your life?
Almost certainly not, the obvious truth. Well, that's why in real data the "measured/discrete/sampled/..." points will have coordinates on the x-axis (maybe time axis,...) that look like this: x = 1.259. x = 1.259 seconds.
And so that... the "sampling" points are evenly spaced?
Then there would have to be a lot of sampling points!
For example, when the x coordinate of point 1 is "1", point 2 is "2", point 3 is "3", we only need 3 "sampling" POINTS.
But if point 1 has x coordinate "1", point 2 is "1.5", point 3 is "3", that means we will need 5 POINTS (1;1.5;2;2.5;3) because they need "unit" of 0.5, yes, because the distance between the points must be EQUALLY DISTANT, mainly because "point 2" has "bad" coordinates (1.5). 
And this should be called "resolution", I guess. In real data this happens all the time.
And this is also quite related to Numerical Integration, when to calculate very accurately the area of ​​a shape with "curved" (nonlinear) sides (such as "1 half" of a circle), when using "Trapezoid Method, Riemann Sum,..." we need to have a lot of EQUALLY SPACED discrete points.

Suppose there is a linear line AB in a signal, created by 3 discrete points A, B, C connected together. And the coordinates of point A are (x:1;y:1); coordinates of point B are (x:2;y:2); C(x:3;y:2). And there will only need 3 measurement points spaced 1 unit apart (it can be seconds, minutes or something, but we will call it unit). Simply put, the word "unit" is a word I came up with to indicate the smallest distance between 2 points in the signal, for example, in the signal there are 3 points, point 1 is 1 second from point 2, point 2 is 0.5 seconds from point 3, so 1 unit will equal 0.5 seconds for the entire signal. Well, since the "sampling" points will be evenly spaced (I stated that above), the distance between two consecutive points in the signal will all be units.


In short :

The "dividing" issue is the 1.259 seconds issue I mentioned, which causes the number of "sampling" points to increase a lot.
And real-world data will often (almost certainly) be asymmetrical, including alternating current (AC).
The discrete "sampled" points are evenly spaced.

--

--
2. Important things


The IEEE standard (519) is an international standard, very popular, almost mandatory, and it is the standard for "harmonics" for AC electrical signals.

IEEE 519 : https://www.elspec-ltd.com/ieee-519-2014-standard-for-harmonics/

Okay, let's keep this project focused on an AC signal with Total Harmonic Distortion (THD) <= 8% (That is from 0% -> 8%. I have not looked into higher levels). And this THD level is EXTREMELY COMMON.

Okay, let's look at the image p98.png (https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v3.0/p98.png), source here: https://www.elspec-ltd.com/wp-content/uploads/2021/01/illustration2-1-800x387.png

The AC signal would theoretically be a pure sine wave. But with real data, the sine wave is "distorted" due to many problems.

You can see, although it is affected by "harmonics", but because THD <= 8%, it mostly affects the "peak" part of half the sine wave cycle, and slightly affects the part near "0 V".

Why?
Harmonic distortion is the presence of frequencies that are integer multiples of the fundamental frequency (usually 50 Hz or 60 Hz) in a signal.

Fundamental Frequency: The original frequency, which forms the main sine wave shape (e.g., 50 Hz).

Harmonics: Multiple frequencies (e.g. 3rd harmonic is 3 * 50 Hz = 150 Hz, 5th harmonic is 5 * 50 Hz = 250 Hz, etc.).

Low order harmonics (mainly 3rd, 5th, 7th) are the main cause of distortion:
In most power systems, the lower odd order harmonics such as 3rd, 5th, and 7th are the components with the largest amplitude and cause the most significant distortion.
Higher order harmonics (such as 11th, 13th, etc.) are usually very small in amplitude and have less effect.

Superposition of 3rd and 5th Harmonics:
3rd Harmonics: Have the greatest impact on the peak of the sine wave. When 3rd harmonics (which pass through zero at points other than the fundamental) are added, they tend to flatten or sharpen the peak of the 50 Hz sine wave.
5th Harmonics: The 5th harmonic often causes “notching” on the slope of the wave, but it still contributes to the overall distortion, especially around the peak.

Low harmonic amplitude:
Since the THD is only 8% (an acceptable limit in many industry standards), this means that the total energy of all the higher harmonics is only 8% of the energy of the fundamental.
Due to their small amplitudes, the higher harmonics are not strong enough to completely change the shape of the sine wave, but only enough to cause local distortion in the regions where the fundamental is at its maximum value (the peak region). At the peak region, the fundamental is changing the slowest (derivative is close to 0), so the superposition of small-amplitude harmonics is more likely to distort the shape there.
In-phase at 0 V: Both the fundamental sine wave and all odd harmonics (1st, 3rd, 5th, 7th...) tend to pass through 0 (or near 0) at the same point in the cycle.
When two waves are added together, if they are both near 0, their sum will also be very close to 0. This causes the zero-crossing of the distorted wave to still closely match the ideal sine wave, leaving the near 0 V portion of the wave less affected in amplitude.
The largest change near 0 V is usually the change in slope (rate of change), but because the harmonic amplitude is low (<= 8%), this change in slope is not too severe, resulting in the wave shape here still retaining the same "steepness" as the fundamental sine wave.

I guess, you should read more about these: Savitzky-Golay filter, Band-pass filter, Low-pass filter, High-pass filter, Notch filter, Adaptive Filter, Wavelet Transform, Fourier Transform,...

Practical Level (For Integral Accuracy): Power quality measurement instruments typically select F_s as a multiple of f and very high to ensure the accuracy of numerical integration and harmonic analysis. Common are 6.4 kS/s (kilo-samples/second), 10 kS/s or higher...

Number of Samples Per 1 Sine Wave Cycle:
That means if it is 10000S/s then 200 samples =)) (This level is quite common).
Actually there are many cases that don't stop at 200, it can go up to 1000 or more :).


--



--
3. Find "peak" with Hung_WAIRNI

The "peak" of half a sine wave cycle is the maximum amplitude value (Peak Amplitude) of the AC voltage or current signal.
In one complete sine wave cycle (one period), there are two peaks:
Maximum positive peak: Occurs in the positive half cycle (positive semi-cycle).
Maximum negative peak: Occurs in the negative half cycle (negative semi-cycle).
Determining the peak (maximum) value of an AC sine wave is necessary and has many important purposes:
For example: Designing a rectifier (converting AC to DC) requires knowing the peak voltage to calculate the output DC voltage. Measurement and Control,
Fault Analysis and Diagnosis,...

See image (p1.png) (https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v3.0/p1.png), I got that image here: https://www.researchgate.net/figure/Output-waveform-of-a-pure-sine-wave-inverter_fig2_341867445, I brought it here and edited it.
This is a pure sine wave, so it's theoretical, but I'm showing you, just like, just think of it as a general form.

WAIRNI_Peak_Derivative (WPD) method :

We choose point "a" to lie on the "body" of half a sine wave cycle.
(in this case a1,a2,a3,a4)
Calculate the approximate "derivative" of 2 points a1, a2. And approximate the derivative of 2 points a3, a4. To get 2 secant lines (a12; a34).
We give the distance between 2 points a (in this case a1, a2) to calculate the derivative as "far enough" and "close enough", that is, not too far so that it reaches the "peak" of the sine wave and goes further. And not too close (especially absolutely not like the derivative, because if the distance between 2 points "a" is delta_x -> 0 or delta_x is close, it's very easy to fail when it encounters "distortion" at near 0V or something. Yes, we will only apply Hung_WAIRNI to AC signals that have not enough noise at the "body" of the sine wave. This is also common. No need to worry about noise, using "secant line" instead of "tangent line" like this is essentially like a "low pass filter" with good noise filtering ability.). And the "a" points should also not lie on/near the "0 V" line, this can actually have a negative effect, though not too much.

Similar to a4,a3.
The intersection of the two secant lines is point x (in this case x1, note: I didn't draw it in image p1.png), and the coordinates of point x1 are the "predicted coordinates" for the "peak" of half a sine wave cycle. Then we check which "discrete point "sampled" from the actual data" is closest to that point, call it point "b", in this case "b1". 
But because of the "harmonics" and many factors, it is easy to "skew". So we have interval k, which is a small interval around point "b" (in this case b1). Do a linear search in interval k. And interval N is the interval that contains the entire half of the sine wave period.
And k is much smaller than N.
Do the same with the remaining sine wave cycles.


An example of how to calculate it (instead of discrete points, it's a function, for easier visualization):

(In reality, the data is in the form of discrete points, not functions!)

(Note: The "variables" in the calculation below are not the "variables" recorded in image p1.png, it is for illustration purposes only)

Oh, by the way, in the calculation below we are calculating the derivative, in reality we do something similar to the derivative but on a much longer secant line.

Coordinates of point A: (x_A, y_A)
Coordinates of point B: (x_B, y_B)

Blue nonlinear function: y = f(x)
Please do not confuse: f'(x) is the symbol for the derivative! Not f(x), the symbol for the function!

The slope of the tangent line AC at A(x_A, y_A) is: m_AC = f'(x_A)
The slope of the tangent line BC at B(x_B, y_B) is: m_BC = f'(x_B)
AC equation: y - y_A = m_AC (x - x_A) => y = f'(x_A)(x - x_A) + y_A
BC equation: y - y_B = m_BC (x - x_B) => y = f'(x_B)(x - x_B) + y_B

At C, the y coordinates of the two lines are equal, so:
f'(x_A)(x_C - x_A) + y_A = f'(x_B)(x_C - x_B) + y_B
=> f'(x_A)x_C - f'(x_A)x_A + y_A = f'(x_B)x_C - f'(x_B)x_B + y_B
=> f'(x_A)x_C - f'(x_B)x_C = f'(x_A)x_A - f'(x_B)x_B + y_B - y_A
=> x_C [f'(x_A) - f'(x_B)] = f'(x_A)x_A - f'(x_B)x_B + y_B - y_A

Formula for x_C :

x_C = [f'(x_A)x_A - f'(x_B)x_B + y_B - y_A]/[f'(x_A) - f'(x_B)]
Yes, this assumes the 2 secant lines are not parallel (if they are, we will call this a special case
and handle it separately, you guys are good enough to do it)

Formula for y_C :

y_C = f'(x_A)(x_C - x_A) + y_A

It takes about 16 to 18 calculations.

With linear search (the current, very popular method) of an interval N, i.e. O(N) complexity, for each discrete point the computational burden is usually equivalent to an addition or comparison. That is, with a discrete "sampling" frequency of 1000 (quite common) per cycle, we will need approximately 1000 comparisons/additions!
And Hung_WAIRNI only takes about O(1) + O(K), and K is much smaller than N.
In general, it still depends on the TYPE OF HARDWARE and the ALGORITHM too.


Don't you believe that the most popular and efficient algorithm today is "Linear Search"?
Read:
The peak value of the current (I_peak) is the maximum (maximum) value that the instantaneous current intensity reaches in one AC wave cycle.
In an AC power system with harmonics (as is very common case like PHD <= 8%), I_peak is really important because it determines many important things.
The most effective/popular I_peak Calculation Method today (Not considering Hung_WAIRNI):
High-Speed ​​Digital Sampling combined with Waveform Cycle Analysis.
This is the approach in harmonic environments (THD <= 8%) because it measures the actual value rather than inferring from the RMS value.
Analyze the Period and Find the Maximum Value:
Define Period: The computer will determine the start and end of a fundamental wave cycle (e.g. 20 ms for 50 Hz frequency).
In the sampled data set i[n] corresponding to one period, the computer only needs to perform one operation to find the absolute maximum value (Absolute Maximum Value).
Basically iterate through each "sample" in the data series. Then do the comparison. Get the absolute value...


...I am developing more...


--

--
4. Calculate area (Numerical integration) with Hung_WAIRNI
RMS (Root Mean Square) is actually a formula based on numerical integration. We will apply Hung_WAIRNI to calculate numerical integration faster, hence faster RMS.

...I am developing more...

--

--
5. Error analysis
--

--
6. Comparison with other methods/algorithms
--

--
7. Deploy with code
--

--
8. End
Thanks for reading, if you learned something new or applied something from it. I hope you can leave my project source there.
--
